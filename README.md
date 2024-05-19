# port-range-proxy

ポートを範囲指定したプロキシ。

# QuickStart

```sh
git clone https://github.com/v1tam1nb2/port-range-proxy.git
cd port-range-proxy
docker-compose up -d
curl localhost:8000
curl localhost:8001
curl localhost:8002
```
800X番ポートなら"800X"というメッセージが表示されたらOK。

# Dockerのポート設定

`compose.yaml`の`port`の設定にて、ポートを範囲指定する。以下の場合、800Xポートが1800Xポートに対応する。
```
  proxy:
    container_name: proxy
    hostname: proxy
    image: nginx:latest
    ports:
      - '8000-8002:18000-18002'
```

# Nginxの設定

`listen`では以下のようにポートを範囲指定できる。リッスンしたポートは`server_port`という変数に格納される。

```conf
    server {
        listen 18000-18002;
        location / {
            #proxy_pass http://web_$server_port;
            proxy_pass http://<行先のホスト名 or IP>:$server_port;

            # proxy_http_version 1.1;
            # proxy_set_header Upgrade $http_upgrade;
            # proxy_set_header Connection "upgrade";
            # proxy_set_header X-Real-IP $remote_addr;
            # proxy_set_header X-Forwarded-Host $host;
            # proxy_set_header X-Forwarded-Server $host;
            # proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            # proxy_cache_bypass $http_upgrade;
            # proxy_set_header Host $host;
            # proxy_set_header Origin '';
        }
    }
```