# PostgreSQL Storage for [OAuth 2.0](https://github.com/go-oauth2/oauth2)

[![GoDoc](https://godoc.org/github.com/vgarvardt/go-oauth2-pg?status.svg)](https://godoc.org/github.com/vgarvardt/go-oauth2-pg)
[![Coverage Status](https://codecov.io/gh/vgarvardt/go-oauth2-pg/branch/master/graph/badge.svg)](https://codecov.io/gh/vgarvardt/go-oauth2-pg)
[![ReportCard](https://goreportcard.com/badge/github.com/vgarvardt/go-oauth2-pg)](https://goreportcard.com/report/github.com/vgarvardt/go-oauth2-pg)
[![License](https://img.shields.io/npm/l/express.svg)](http://opensource.org/licenses/MIT)

## Install

Storage major version matches [OAuth 2.0](https://github.com/go-oauth2/oauth2) major version,
so use corresponding version (go modules compliant)

For `github.com/go-oauth2/oauth2/v4`:

```bash
go get -u github.com/brutalzinn/go-oauth2-pg/v4

```

For `gopkg.in/oauth2.v3` see [v3 branch](https://github.com/vgarvardt/go-oauth2-pg/tree/v3).

## PostgreSQL drivers

The store accepts an adapter interface that interacts with the DB.
Adapter and implementations extracted to separate
package [`github.com/vgarvardt/go-pg-adapter`](https://github.com/vgarvardt/go-pg-adapter) for easier maintenance.

## Usage example

```go
package main

import (
  "context"
  "os"
  "time"

  "github.com/go-oauth2/oauth2/v4/manage"
  "github.com/jackc/pgx/v4"
  pg "github.com/brutalzinn/go-oauth2-pg/v4
"
  "github.com/vgarvardt/go-pg-adapter/pgx4adapter"
)

func main() {
  pgxConn, _ := pgx.Connect(context.TODO(), os.Getenv("DB_URI"))

  manager := manage.NewDefaultManager()

  // use PostgreSQL token store with pgx.Connection adapter
  adapter := pgx4adapter.NewConn(pgxConn)
  tokenStore, _ := pg.NewTokenStore(adapter, pg.WithTokenStoreGCInterval(time.Minute))
  defer tokenStore.Close()

  clientStore, _ := pg.NewClientStore(adapter)

  manager.MapTokenStorage(tokenStore)
  manager.MapClientStorage(clientStore)
  // ...
}
```

## MIT License

> Copyright (c) 2020 Vladimir Garvardt
