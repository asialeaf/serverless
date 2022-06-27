### 部署一个Golang HTTP Server

#### 准备

- 已安装好Knative Serving
- Docker
- A Docker Hub，用来更新container image

#### 构建代码到镜像

##### 代码示例 

helloworld.go

```go
package main

import (
  "fmt"
  "log"
  "net/http"
  "os"
)

func handler(w http.ResponseWriter, r *http.Request) {
  log.Print("helloworld: received a request")
  target := os.Getenv("TARGET")
  if target == "" {
    target = "World"
  }
  fmt.Fprintf(w, "Hello %s!\n", target)
}

func main() {
  log.Print("helloworld: starting server...")

  http.HandleFunc("/", handler)

  port := os.Getenv("PORT")
  if port == "" {
    port = "8080"
  }

  log.Printf("helloworld: listening on port %s", port)
  log.Fatal(http.ListenAndServe(fmt.Sprintf(":%s", port), nil))
}
```

或者通过**git clone**下载代码

```bash
   git clone https://github.com/xxx/helloworld.git helloworld
   cd helloworld
```

##### 编写构建文件

Dockerfile

   ```docker
   # Use the official Golang image to create a build artifact.
   # This is based on Debian and sets the GOPATH to /go.
   FROM golang:1.13 as builder
   # Create and change to the app directory.
   WORKDIR /app
   # Retrieve application dependencies using go modules.
   # Allows container builds to reuse downloaded dependencies.
   COPY go.* ./
   RUN go mod download
   # Copy local code to the container image.
   COPY . ./
   # Build the binary.
   # -mod=readonly ensures immutable go.mod and go.sum in container builds.
   RUN CGO_ENABLED=0 GOOS=linux go build -mod=readonly -v -o server
   # Use the official Alpine image for a lean production container.
   # https://hub.docker.com/_/alpine
   # https://docs.docker.com/develop/develop-images/multistage-build/#use-multi-stage-builds
   FROM alpine:3
   RUN apk add --no-cache ca-certificates
   # Copy the binary to the production image from the builder stage.
   COPY --from=builder /app/server /server
   # Run the web service on container startup.
   CMD ["/server"]
   ```

 使用Go tool 创建 go.mod manifest

```bash
go mod init github.com/xxx/helloworld
```

##### 构建镜像&推送至镜像仓库

```bash
# Build the container on your local machine
docker build -t {username}/helloworld-go .

# Push the container to docker registry
docker push {username}/helloworld-go
```

#### 部署Knative Serving

service.yaml

```yaml
apiVersion: serving.knative.dev/v1
kind: Service
metadata:
  name: helloworld-go
  namespace: default
spec:
  template:
    spec:
      containers:
        - image: docker.io/{username}/helloworld-go       #应用镜像
          env:
            - name: TARGET
              value: "Go Sample v1"
```

使用kubectl查看Service

```bash
kubectl get ksvc helloworld-go  --output=custom-columns=NAME:.metadata.name,URL:.status.url
```

结果

```bash
NAME                URL
helloworld-go       http://helloworld-go.default.example.com
```

#### 验证

```bash
curl -H "Host: helloworld-go.default.example.com" $ISTIO_GATEWAY_URL
```

#### 移除APP

```bash
kubectl delete --filename service.yaml
```

或者

```bash
kubectl delete ksvc helloworld-go
```

