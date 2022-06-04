# syntax=docker/dockerfile:1.3
FROM rust:1.61-alpine AS builder

RUN apk --no-cache add libc-dev

WORKDIR /build
COPY Cargo.* ./
COPY src ./src

RUN --mount=type=cache,target=/root/.cargo cargo build --verbose --target=x86_64-unknown-linux-musl --release

FROM alpine

RUN apk --no-cache add tini

COPY --from=builder /build/target/x86_64-unknown-linux-musl/release/agate /usr/bin/agate
COPY content /content

EXPOSE 1965
VOLUME ["/content", "/.certificates"]

ENTRYPOINT ["tini", "--", "agate"]
CMD ["--hostname", "localhost"]
