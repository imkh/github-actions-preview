#################################################
# Build image - copy code & build application
#################################################
FROM golang:1.12-alpine as build

# Install git (required for fetching the dependencies).
RUN apk --no-cache --quiet update && apk add --no-cache --quiet git

# Copy the local code to the container image.
WORKDIR /app
ADD . /app

# Fetch dependencies.
RUN go get -d -v ./...

# Build application.
RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -a -tags netgo -ldflags '-s -w -extldflags "-static"' -o /go/bin/app

#################################################
# Runtime image - smallest image
#################################################
FROM gcr.io/distroless/static

COPY --from=build /go/bin/app /app

CMD [ "/app" ]
