---
description: Developer notes regarding DataAPI app
---

# Development

There are no special CIs set up for DataAPI at the moment.&#x20;

### Release

To update the live version of the front end on the server, you need to:

1. log in into the sever via `ssh`
2. go to `dataapi` where the application and python virtualenv is hosted
3. pull newest version from github with `git pull`
4. restart nginx unit with `sudo systemctl restart unit`  &#x20;

### Nginx Unit config

The currently used config can be found in the repo.

### Nginx Config

The nginx serves the data requests separately,&#x20;

```nginx
server {
    listen       80;
    server_name api.metacity.cc;

    location / {
        proxy_pass http://127.0.0.1:8080;
        proxy_set_header Host $host;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}

server {
    listen 80;
    server_name data.metacity.cc;

    gzip on;
    gzip_types      *;
    gzip_proxied    no-cache no-store private expired auth;
    gzip_min_length 1000;

    location / {
        # Simple requests
        if ($request_method ~* "(GET|POST)") {
            add_header "Access-Control-Allow-Origin"  *;
        }

        # Preflighted requests
        if ($request_method = OPTIONS ) {
            add_header "Access-Control-Allow-Origin"  *;
            add_header "Access-Control-Allow-Methods" "GET, POST, OPTIONS, HEAD";
            add_header "Access-Control-Allow-Headers" "Authorization, Origin, X-Requested-With, Content-Type, Accept";
            return 200;
        }

        root /home/monika/data;
    }
}
```

