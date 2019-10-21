# [pa-api] -- APIs for Amazon Product Advertising API v5 by Golang

[![Build Status](https://travis-ci.org/spiegel-im-spiegel/pa-api.svg?branch=master)](https://travis-ci.org/spiegel-im-spiegel/pa-api)
[![GitHub license](https://img.shields.io/badge/license-Apache%202-blue.svg)](https://raw.githubusercontent.com/spiegel-im-spiegel/pa-api/master/LICENSE)
[![GitHub release](https://img.shields.io/github/release/spiegel-im-spiegel/pa-api.svg)](https://github.com/spiegel-im-spiegel/pa-api/releases/latest)

## Usage

### Create PA-API Information

Default PA-API information.

```go
sv := paapi5.New() //Create default server
fmt.Println("Marketplace:", sv.Marketplace())
fmt.Println("Region:", sv.Region())
fmt.Println("AcceptLanguage:", sv.AcceptLanguage())
fmt.Println("URL:", sv.URL(paapi5.GetItems.Path()))
// Output:
// Marketplace: www.amazon.com
// Region: us-east-1
// AcceptLanguage: en_US
// URL: https://webservices.amazon.com/paapi5/getitems
```

PA-API information for Japan region.

```go
sv := paapi5.New(paapi5.WithMarketplace(paapi5.LocaleJapan)) //Create server in Japan region
fmt.Println("Marketplace:", sv.Marketplace())
fmt.Println("Region:", sv.Region())
fmt.Println("AcceptLanguage:", sv.AcceptLanguage())
fmt.Println("URL:", sv.URL(paapi5.GetItems.Path()))
// Output:
// Marketplace: www.amazon.co.jp
// Region: us-west-2
// AcceptLanguage: ja_JP
// URL: https://webservices.amazon.co.jp/paapi5/getitems
```

### Create Client Instance

Create default client instance.

```go
client := paapi5.DefaultClient("mytag-20", "AKIAIOSFODNN7EXAMPLE", "1234567890") //Create default client
fmt.Println("Marketplace:", client.Marketplace())
// Output:
// Marketplace: www.amazon.com
```

Create client instance for Japan region.

```go
//Create client for Janan region
client := paapi5.New(
    paapi5.WithMarketplace(paapi5.LocaleJapan),
).CreateClient(
    "mytag-20",
    "AKIAIOSFODNN7EXAMPLE",
    "1234567890",
    paapi5.WithContext(context.Background()),
    paapi5.WithHttpClient(http.DefaultClient),
)
fmt.Println("Marketplace:", client.Marketplace())
// Output:
// Marketplace: www.amazon.co.jp
```

## Sample code

### Operation GetItems

```go
package main

import (
    "fmt"

    paapi5 "github.com/spiegel-im-spiegel/pa-api"
    "github.com/spiegel-im-spiegel/pa-api/entity"
    "github.com/spiegel-im-spiegel/pa-api/query"
)

func main() {
    //Create client
    client := paapi5.New(
        paapi5.WithMarketplace(paapi5.LocaleJapan),
    ).CreateClient(
        "mytag-20",
        "AKIAIOSFODNN7EXAMPLE",
        "1234567890",
        paapi5.WithContext(context.Background()),
        paapi5.WithHttpClient(http.DefaultClient),
    )

    //Make query
    q := query.NewGetItems(
        client.Marketplace(),
        client.PartnerTag(),
        client.PartnerType(),
    ).ASINs([]string{"B07YCM5K55"}).EnableImages().EnableItemInfo().EnableParentASIN()

    //Requet and response
    body, err := client.Request(q)
    if err != nil {
        fmt.Printf("%+v\n", err)
        return
    }
    //io.Copy(os.Stdout, bytes.NewReader(body))

    //Decode JSON
    res, err := entity.DecodeResponse(body)
    if err != nil {
        fmt.Printf("%+v\n", err)
        return
    }
    fmt.Println(res.String())
}
```

### Operation SearchItems

```go
package main

import (
    "fmt"

    paapi5 "github.com/spiegel-im-spiegel/pa-api"
    "github.com/spiegel-im-spiegel/pa-api/entity"
    "github.com/spiegel-im-spiegel/pa-api/query"
)

func main() {
    //Create client
    client := paapi5.New(
        paapi5.WithMarketplace(paapi5.LocaleJapan),
    ).CreateClient(
        "mytag-20",
        "AKIAIOSFODNN7EXAMPLE",
        "1234567890",
        paapi5.WithContext(context.Background()),
        paapi5.WithHttpClient(http.DefaultClient),
    )

    //Make query
    q := query.NewSearchItems(
        client.Marketplace(),
        client.PartnerTag(),
        client.PartnerType(),
    ).Search(query.Keywords, "数学ガール").EnableImages().EnableItemInfo().EnableParentASIN()

    //Requet and response
    body, err := client.Request(q)
    if err != nil {
        fmt.Printf("%+v\n", err)
        return
    }
    //io.Copy(os.Stdout, bytes.NewReader(body))

    //Decode JSON
    res, err := entity.DecodeResponse(body)
    if err != nil {
        fmt.Printf("%+v\n", err)
        return
    }
    fmt.Println(res.String())
}
```

## Contributors

Many thanks for [contributors](https://github.com/spiegel-im-spiegel/pa-api/graphs/contributors "Contributors to spiegel-im-spiegel/pa-api")

## Links

- [Go 言語用 PA-API v5 クライアント・パッケージ — リリース情報 | text.Baldanders.info](https://text.baldanders.info/release/pa-api-v5/)

[pa-api]: https://github.com/spiegel-im-spiegel/pa-api "spiegel-im-spiegel/pa-api: APIs for Amazon Product Advertising API v5 by Golang"
