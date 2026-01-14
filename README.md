# cko-permify

### Installation
#### Brew
```bash
brew install permify
permify serve --distributed-port=5001
```


#### Docker
```bash
docker run -p 3476:3476 -p 3478:3478  ghcr.io/permify/permify serve
```


### Health Check
```bash
curl --location 'http://localhost:3476/healthz'
```

### Writing Schema
```bash
curl --location 'http://localhost:3476/v1/tenants/t1/schemas/write' \
--header 'Content-Type: application/json' \
--data-raw '{
    "schema": "entity user {}\n\nentity account {\n    relation owner @user\n    relation following @user\n    relation follower @user\n    attribute public boolean\n\n    action view = (owner or follower) or public\n}\n\nentity post {\n    relation account @account\n    attribute restricted boolean\n\n    action view = account.view\n    action comment = account.following not restricted\n    action like = account.following not restricted\n}"
}'
```

### Check/Read Schema
```bash
curl --location 'http://localhost:3476/v1/tenants/t1/schemas/read' \
--header 'Content-Type: application/json' \
--data '{
    "metadata": {
        "schema_version": ""
    }
}'
```

