---
layout: post
title: "Configuration"
date: 2020-10-10 00:00:00 +0000
categories: release
---

# Example

## Client

```toml
[client]
port = 1080
http_port = 1088
redir_port = 1090
server = "example.com"
username = "username"
password = "password"
proxy_all = true

[dns]
type = "doh"
server = "cloudflare-dns.com"
addr = "1.1.1.1"

[log]
file = "./log.xml"
level = "trace"

```

## Server

```toml
[server]
port = 80
admin_password = "password"

[db]
type = "sqlite3"
username = "root"
password = "password"
host = "localhost"
port = 1433
database = "relaybaton.db"

[dns]
type = "doh"
server = "cloudflare-dns.com"
addr = "1.1.1.1"

[log]
file = "./log.xml"
level = "trace"

```

# Description of the fields

|         Field         | TOML Type |                      Go Type                      |             Description             |
| :-------------------: | :-------: | :-----------------------------------------------: | :---------------------------------: |
|      client.port      |  Integer  |                      uint16                       |  SOCKS5 port that client listen to  |
|   client.http_port    |  Integer  |                      uint16                       |   HTTP port that client listen to   |
|   client.redir_port   |  Integer  |                      uint16                       | Redirect port that client listen to |
|     client.server     |  String   |                      string                       |      domain name of the server      |
|    client.username    |  String   |                      string                       |       username of the client        |
|    client.password    |  String   |                      string                       |       password of the client        |
|   client.proxy_all    |  Boolean  |                       bool                        |        if proxy all traffic         |
|      server.port      |  Integer  |                      uint16                       |     port that server listen to      |
| server.admin_password |  String   |                      string                       |     password of account "admin"     |
|        db.type        |  String   | github.com/iyouport-org/relaybaton config.dbType  |        type of the database         |
|      db.username      |  String   |                      string                       |  username for database connection   |
|      db.password      |  String   |                      string                       |  password for database connection   |
|        db.host        |  String   |                      string                       |  hostname for database connection   |
|        db.port        |  Integer  |                      uint16                       |    port for database connection     |
|      db.database      |  String   |                      string                       |          name of database           |
|       dns.type        |  String   | github.com/iyouport-org/relaybaton config.DNSType |        type of DNS resolver         |
|      dns.server       |  String   |                      string                       |    server name of the DNS server    |
|       dns.addr        |  String   |                     net.Addr                      |    IP address of the DNS server     |
|       log.file        |  String   |                      os.File                      |        filename of log file         |
|       log.level       |  String   |      github.com/sirupsen/logrus logrus.Level      |     minimum log level to write      |

## Client

The `client` section is required only on the client side, it defines which port will the client listen at, which server it will connect to with what username and password.

```toml
[client]
port = 1080
http_port = 1088
redir_port = 1090
server = "example.com"
username = "username"
password = "password"
proxy_all = true
```

### Port

- Type: Integer
- Range: 0-65535
- Cannot be same with `redir_port` or `http_port`
- always required in `client`

```toml
port = 1080
```

`port` is the TCP port the client listen to, accepting SOCKS5 proxy protocol.

### HTTP Port

- Type: Integer
- Range: 0-65535
- Cannot be same with `redir_port` or `port`
- always required in `client`

```toml
http_port = 1088
```

`http_port` is the TCP port the client listen to, accepting HTTP request with CONNECT method to build a local HTTP tunnel.

### Redir Port

- Type: Integer
- Range: 0-65535
- Cannot be same with `port` or `http_port`
- always required in `client`

```toml
redir_port = 1090
```

`redir_port` is the TCP port the client listen to, for redirected TCP connection on routers.

### Server

- Type: String
- Have to be a valid domain name
- always required in `client`

```toml
server = "example.com"
```

`server` is the domain name of the proxy server you want to connect to. Get this information from your service provider.

### Username

- Type: String
- always required in `client`

```toml
username = "username"
```

`username` is your username to connect to the proxy server. Get this information from your service provider.

### Password

- Type: String
- always required in `client`

```toml
password = "password"
```

`password` is your password to connect to the proxy server.

### Proxy All

- Type: Boolean
- always required in `client`

```toml
proxy_all = true
# proxy_all = false
```

If `proxy_all` is set to true, all connections received by the client whose destination IP address is not a reserved IP address will be proxied.

If set to false, then only connections that go outside of China will be proxied.

## Server

The `server` section is required only on the server side.

```toml
[server]
port = 80
admin_password = "password"
```

### Port

- Type: Integer
- Range: 0-65535
- always required in `server`

```toml
port = 80
```

`port` is the TCP port the client listen to, accepting HTTP. The recommended port is `80`

### Admin Password

- Type: String
- always required in `server`

```toml
admin_password = "password"
```

`admin_password` is the password for the account `admin`, which is used as the admin account and the first account of Web UI, but cannot be used as a client.

## DB

The `db` section is required only on the server side. It defines how the server connect to the database.

```toml
[db]
type = "sqlite3"
username = "root"
password = "password"
host = "localhost"
port = 1433
database = "relaybaton.db"
```

### Type

- Type: String
- Have to be one of the `sqlite3`/`mysql`/`postgresql`/`sqlserver`
- always required in `db`

```toml
type = "sqlite3"
# type = "mysql"
# type = "postgresql"
# type = "sqlserver"
```

`type` is the type of database.

Three database types are supported:

- `sqlite3`
- `mysql`
- `postgresql`
- `sqlserver`

### Username

- Type: String
- always required in `db`

```toml
username = "root"
```

`username` is the username for database connection

if the value of `type` is `sqlite3`, this field do not have any meaning

### Password

- Type: String
- always required in `db`

```toml
password = "password"
```

`password` is the password for database connection

if the value of `type` is `sqlite3`, this field do not have any meaning

### Host

- Type: String
- Have to be a valid hostname
- always required in `db`

```toml
host = "localhost"
```

`host` is the host of the database

if the value of `type` is `sqlite3`, this field do not have any meaning

### Port

- Type: Integer
- Range: 0-65535
- always required in `db`

```toml
port = 1433
```

`port` is the port of the database

if the value of `type` is `sqlite3`, this field do not have any meaning

### Database

- Type: String
- always required in `db`

```toml
database = "relaybaton.db"
```

`database` is the name of the database

if the value of `type` is `sqlite3`, this field is the path of the database file

## DNS

The `dns` section is required in both client and server side. It defines which DNS should be used.

```toml
[dns]
type = "doh"
server = "cloudflare-dns.com"
addr = "1.1.1.1"
```

### Type

- Type: String
- Have to be one of the `default`/`dot`/`doh`
- always required in `dns`

```toml
type = "doh"
# type = "dot"
# type = "default"
```

`type` is the type of DNS.

Three types are supported:

- `default`: use default DNS, not recommended for the client side
- `dot`: use DNS over TLS
- `doh`: use DNS over HTTPS

### Server

- Type: String
- Have to be a valid domain name
- always required in `dns`

```toml
server = "cloudflare-dns.com"
```

`server` is the domain of the DNS server

if the value of `type` is `default`, this field do not have any meaning

### Addr

- Type: String
- if the value of `type` is `dot`, this field have to be an TCP address (IP:Port)
- if the value of `type` is doh, this field have to be an IP address
- always required in `dns`

```toml
addr = "1.1.1.1" #DoH
# addr = "1.1.1.1:53" #DoT
```

`addr` it the IP address of the DNS server

if the value of `type` is `default`, this field do not have any meaning

## Log

The `log` section is required in both client and server side. It is related to logging.

```toml
[log]
file = "./log.xml"
level = "trace"
```

### File

- Type: String
- Have to be a valid file path
- always required in `log`

```toml
file = "./log.xml"
```

`file` is the path to the log file

### Level

- Type: String
- Have to be one of the `panic`/`fatal`/`error`/`warn`/`info`/`debug`/`trace`
- always required in `log`

```toml
level = "trace"
# level = "panic"
# level = "fatal"
# level = "error"
# level = "warn"
# level = "info"
# level = "debug"
```

`level` is the lowest level of the log.

Seven levels are supported:

- `panic`: Highest level of severity.
- `fatal`: Fatal errors.
- `error`: Errors that should definitely be noted.
- `warn`: Non-critical entries that deserve eyes.
- `info`: General operational entries about what's going on inside the application.
- `debug`: Very verbose logging.
- `trace`: Designates finer-grained informational events than the Debug.
