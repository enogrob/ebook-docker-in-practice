FROM golang:1.4.2
RUN CGO_ENABLED=0 go get \
-a -ldflags '-s' -installsuffix cgo \
github.com/docker-in-practice/go-web-server
CMD ["cat","/go/bin/go-web-server"]
