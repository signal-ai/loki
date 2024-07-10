FROM golang:1.22.5-alpine AS build-image

COPY lambda-promtail /src/lambda-promtail
WORKDIR /src/lambda-promtail

RUN go version

RUN apk update && apk upgrade && \
  apk add --no-cache bash git
RUN go version

COPY go.mod /src/lambda-promtail/go.mod
COPY go.sum /src/lambda-promtail/go.sum

RUN ls -al
RUN go mod download
RUN GOOS=linux CGO_ENABLED=0 go build -o /main -tags lambda.norpc -ldflags="-s -w" *.go
# copy artifacts to a clean image
FROM public.ecr.aws/lambda/provided:al2
COPY --from=build-image /main /main
ENTRYPOINT [ "/main" ]
