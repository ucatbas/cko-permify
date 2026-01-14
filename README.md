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

### Write Data
```bash
curl --location 'http://localhost:3476/v1/tenants/t1/data/write' \
--header 'Content-Type: application/json' \
--data '{
    "metadata": {
        "schema_version": ""
    },
    "tuples": [
        {
            "entity": {
                "type": "account",
                "id": "1"
            },
            "relation": "owner",
            "subject": {
                "type": "user",
                "id": "kevin"
            }
        },
        {
            "entity": {
                "type": "account",
                "id": "2"
            },
            "relation": "owner",
            "subject": {
                "type": "user",
                "id": "george"
            }
        },
        {
            "entity": {
                "type": "account",
                "id": "1"
            },
            "relation": "following",
            "subject": {
                "type": "user",
                "id": "george"
            }
        },
        {
            "entity": {
                "type": "account",
                "id": "2"
            },
            "relation": "follower",
            "subject": {
                "type": "user",
                "id": "kevin"
            }
        },
        {
            "entity": {
                "type": "post",
                "id": "1"
            },
            "relation": "account",
            "subject": {
                "type": "account",
                "id": "1"
            }
        },
        {
            "entity": {
                "type": "post",
                "id": "2"
            },
            "relation": "account",
            "subject": {
                "type": "account",
                "id": "2"
            }
        }
    ],
    "attributes": []
}'
```
### Check Permission
```bash
curl --location 'http://localhost:3476/v1/tenants/t1/permissions/check' \
--header 'Content-Type: application/json' \
--data '{
    "metadata": {
        "snap_token": "",
        "schema_version": "",
        "depth": 20
    },
    "entity": {
        "type": "post",
        "id": "1"
    },
    "permission": "view",
    "subject": {
        "type": "user",
        "id": "george"
    }
}'
```
### Playground Link
https://play.permify.co/?s=1768411325712-up0mniv
