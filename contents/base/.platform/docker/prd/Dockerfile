# prd/Dockerfile — the artifact CI publishes and the platform deploys
#
# Layout-agnostic on purpose: this overlay retrofits an EXISTING application, so it copies the whole
# repo and runs the application's OWN build command rather than assuming a module name, a crate, a
# src/ layout or an output path. `build_command` and `runtime_artifact` are answers; their defaults
# match what this language's p6m service archetype produces, so a greenfield-shaped repo needs
# neither.

FROM golang:1.23 AS builder
WORKDIR /app
COPY . .
RUN go mod download
RUN {{ build_command }}

FROM gcr.io/distroless/static:nonroot
COPY --from=builder /app/{{ runtime_artifact }} /server
EXPOSE {{ service_port }}
EXPOSE {{ management_port }}
ENTRYPOINT ["/server"]
