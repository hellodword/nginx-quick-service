```sh
docker run \
    --name nginx-quick-service \
    --restart always --detach \
    --cpus 0.2 --memory 50m \
    -p 127.0.0.1:8080:8080 \
    -v `pwd`/nginx.conf:/etc/nginx/nginx.conf:ro \
    -v `pwd`/.cache:/data/nginx/cache:rw \
    nginx:latest

uuidgen
# aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa

# get the host
docker run \
    --rm \
    --network host \
    cloudflare/cloudflared:latest \
        tunnel \
        --no-autoupdate \
        --loglevel info \
        --transport-loglevel fatal \
        --quick-service http://127.0.0.1:8080/quick-service/aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa \
        --url http://127.0.0.1:1080

docker run \
    --name service-name \
    --restart always --detach \
    --network host \
    cloudflare/cloudflared:latest \
        tunnel \
        --no-autoupdate \
        --loglevel fatal \
        --transport-loglevel fatal \
        --quick-service http://127.0.0.1:8080/quick-service/aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa \
        --url http://127.0.0.1:1080

```