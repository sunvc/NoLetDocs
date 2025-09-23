*[BARK](https://github.com/Finb/Bark)のオープンソースプロジェクトに感謝します*

## Docker-Compose 
* 設定

```yaml
system:
  user: ""
  password: ""
  addr: "0.0.0.0:8080"
  url_prefix: ""
  data: "./data"
  name: "NoLet"
  dsn: ""
  cert: ""
  key: ""
  reduce_memory_usage: false
  proxy_header: ""
  max_batch_push_count: -1
  max_apns_client_count: 1
  concurrency: 262144   # 256 * 1024
  read_timeout: 3s
  write_timeout: 3s
  idle_timeout: 10s
  debug: true
  version: ""
  build_date: ""
  commitID: ""
  expired: 0
  icp_info: ""
  time_zone: "UTC"
  voice: true
  auth_ids: []

apple:
  apnsPrivateKey: |-
    -----BEGIN PRIVATE KEY-----
    MIGTAgEAMBMGByqGSM49AgEGCCqGSM49AwEHBHkwdwIBAQQgvjopbchDpzJNojnc
    o7ErdZQFZM7Qxho6m61gqZuGVRigCgYIKoZIzj0DAQehRANCAAQ8ReU0fBNg+sA+
    ZdDf3w+8FRQxFBKSD/Opt7n3tmtnmnl9Vrtw/nUXX4ldasxA2gErXR4YbEL9Z+uJ
    REJP/5bp
    -----END PRIVATE KEY-----
  topic: "me.sunvc.Meoworld"
  keyID: "BNY5GUGV38"
  teamID: "FUWV6U942Q"
  develop: true


```

### コマンドラインパラメータ

設定ファイルの他に、コマンドラインパラメータや環境変数を使用してサービスを設定することもできます：


| パラメータ | 環境変数 | 説明 | デフォルト値 |
|------|----------|------|--------|
| `--addr` | `NoLet_SERVER_ADDRESS` | サーバーリスニングアドレス | `0.0.0.0:8080` |
| `--url-prefix` | `NoLet_SERVER_URL_PREFIX` | サービスURLプレフィックス | `/` |
| `--dir` | `NoLet_SERVER_DATA_DIR` | データストレージディレクトリ | `./data` |
| `--dsn` | `NoLet_SERVER_DSN` | MySQL DSN、フォーマット：`user:pass@tcp(host)/dbname` | 空 |
| `--cert` | `NoLet_SERVER_CERT` | TLS 証明書パス | 空 |
| `--key` | `NoLet_SERVER_KEY` | TLS 証明書の秘密鍵パス | 空 |
| `--reduce-memory-usage` | `NoLet_SERVER_REDUCE_MEMORY_USAGE` | メモリ使用量を削減（CPU消費量が増加） | `false` |
| `--user, -u` | `NoLet_SERVER_BASIC_AUTH_USER` | 基本認証ユーザー名 | 空 |
| `--password, -p` | `NoLet_SERVER_BASIC_AUTH_PASSWORD` | 基本認証パスワード | 空 |
| `--proxy-header` | `NoLet_SERVER_PROXY_HEADER` | HTTPヘッダーのリモートIPアドレスソース | 空 |
| `--max-batch-push-count` | `NoLet_SERVER_MAX_BATCH_PUSH_COUNT` | バッチプッシュの最大数、`-1`は無制限を意味する | `-1` |
| `--max-apns-client-count` | `NoLet_SERVER_MAX_APNS_CLIENT_COUNT` | 最大APNsクライアント接続数 | `1` |
| `--admins` | `NoLet_SERVER_ADMINS` | 管理者IDリスト | 空 |
| `--debug` | `NoLet_DEBUG` | デバッグモードを有効にする | `false` |
| `--apns-private-key` | `NoLet_APPLE_APNS_PRIVATE_KEY` | APNs 秘密鍵パス | 空 |
| `--topic` | `NoLet_APPLE_TOPIC` | APNs Topic | 空 |
| `--key-id` | `NoLet_APPLE_KEY_ID` | APNs Key ID | 空 |
| `--team-id` | `NoLet_APPLE_TEAM_ID` | APNs Team ID | 空 |
| `--develop, --dev` | `NoLet_APPLE_DEVELOP` | 启用 APNs 开发环境 | `false` |
| `--Expired, --ex` | `NoLet_EXPIRED_TIME` | 语音过期时间（秒） | `120` |
| `--help, -h` | - | 显示帮助信息 | - |
| `--config, -c` | - | 指定配置文件路径 | - |

命令行参数优先级高于配置文件，环境变量优先级高于命令行参数。

## Docker部署

```shell

docker run -d --name NoLets -p 8080:8080 -v ./data:/data  --restart=always  sanvc/NoLet:latest
```

## Docker-compose部署
* 复制项目中的/deploy文件夹到服务器上，然后执行以下命令即可。
* 可选 `config.yaml` 配置文件，文件中的配置项，可以根据自己的需求进行修改。

* 启动
```shell
docker-compose up -d
```

## 手动部署

1. 根据平台下载可执行文件:<br> <a href='https://github.com/sunvc/NoLets/releases'>https://github.com/sunvc/NoLets/releases</a><br>
或自己编译<br>
<a href="https://github.com/sunvc/NoLets">https://github.com/sunvc/NoLets</a>

2. 运行
---
```
./main
```

## 其他

1. APP端负责将<a href="https://developer.apple.com/documentation/uikit/uiapplicationdelegate/1622958-application">DeviceToken</a>发送到服务端。 <br>服务端收到一个推送请求后，将发送推送给Apple服务器。然后手机收到推送

2. 服务端代码: <a href='https://github.com/sunvc/NoLets'>https://github.com/sunvc/NoLets</a><br>

3. App代码: <a href="https://github.com/sunvc/NoLet">https://github.com/sunvc/NoLet</a>

