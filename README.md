Name
---
lua-resty-maxminddb - A Lua library for reading [MaxMind's Geolocation database format](https://maxmind.github.io/MaxMind-DB/)  (aka mmdb or geoip2).


Prerequisites
---

**Bug fixed**

- [Error at lookup IP](https://github.com/anjia0532/lua-resty-maxminddb/issues/5)

- [bad argument #1 to 'concat' (table expected, got nil)](https://github.com/anjia0532/lua-resty-maxminddb/issues/4)

- [Memory leak](https://github.com/anjia0532/lua-resty-maxminddb/issues/6)

- [Multiple subdivisions](https://github.com/anjia0532/lua-resty-maxminddb/issues/7)

**Apology for infringement**
- https://github.com/anjia0532/lua-resty-maxminddb/issues/25

**Note**
- [maxmind/libmaxminddb][]

- [openresty][]

- [GeoLite2 Free Downloadable Databases][linkGeolite2FreeDownloadableDatabases]

- [maxmind/geoipupdate][]


Installation
---
```bash
opm get anjia0532/lua-resty-maxminddb
```

Synopsis
---
```
  local cjson = require 'cjson'
  local geo = require 'resty.maxminddb'
  if not geo.initted() then
    geo.init({
      city = "/usr/local/maxmind/GeoIP2-City.mmdb",
      isp  = "/usr/local/maxmind/GeoIP2-ISP.mmdb",
      asn  = "/usr/local/maxmind/GeoLite2-ASN.mmdb",
      conn = "/usr/local/maxmind/GeoIP2-Connection-Type.mmdb"
    })
  end
  local res,err = geo.lookup("city", ngx.var.arg_ip or ngx.var.remote_addr) --support ipv6 e.g. 2001:4860:0:1001::3004:ef68

  if not res then
      ngx.log(ngx.ERR,'failed to lookup by ip ,reason:',err)
  end

  ngx.say("full :",cjson.encode(res))
  if ngx.var.arg_node then
     ngx.say("node name:",ngx.var.arg_node," ,value:", cjson.encode(res[ngx.var.arg_node] or {}))
  end
```

```bash
  #ipv4
  $ curl localhost/ip=114.114.114.114&node=city
  #ipv6
  #$ curl localhost/ip=2001:4860:0:1001::3004:ef68&node=country
  full :{"city":{"geoname_id":1799962,"names":{"en":"Nanjing","ru":"Нанкин","fr":"Nankin","pt-BR":"Nanquim","zh-CN":"南京","es":"Nankín","de":"Nanjing","ja":"南京市"}},"subdivisions":[{"geoname_id":1806260,"names":{"en":"Jiangsu","fr":"Province de Jiangsu","zh-CN":"江苏省"},"iso_code":"32"}],"country":{"geoname_id":1814991,"names":{"en":"China","ru":"Китай","fr":"Chine","pt-BR":"China","zh-CN":"中国","es":"China","de":"China","ja":"中国"},"iso_code":"CN"},"registered_country":{"geoname_id":1814991,"names":{"en":"China","ru":"Китай","fr":"Chine","pt-BR":"China","zh-CN":"中国","es":"China","de":"China","ja":"中国"},"iso_code":"CN"},"location":{"time_zone":"Asia\/Shanghai","longitude":118.7778,"accuracy_radius":50,"latitude":32.0617},"continent":{"geoname_id":6255147,"names":{"en":"Asia","ru":"Азия","fr":"Asie","pt-BR":"Ásia","zh-CN":"亚洲","es":"Asia","de":"Asien","ja":"アジア"},"code":"AS"}}
  node name:city ,value:{"geoname_id":1799962,"names":{"en":"Nanjing","ru":"Нанкин","fr":"Nankin","pt-BR":"Nanquim","zh-CN":"南京","es":"Nankín","de":"Nanjing","ja":"南京市"}}
```

prettify
```json
full: {
    "city": {
        "geoname_id": 1799962,
        "names": {
            "en": "Nanjing",
            "ru": "Нанкин",
            "fr": "Nankin",
            "pt-BR": "Nanquim",
            "zh-CN": "南京",
            "es": "Nankín",
            "de": "Nanjing",
            "ja": "南京市"
        }
    },
    "subdivisions": [{
            "geoname_id": 1806260,
            "names": {
                "en": "Jiangsu",
                "fr": "Province de Jiangsu",
                "zh-CN": "江苏省"
            },
            "iso_code": "32"
        }
    ],
    "country": {
        "geoname_id": 1814991,
        "names": {
            "en": "China",
            "ru": "Китай",
            "fr": "Chine",
            "pt-BR": "China",
            "zh-CN": "中国",
            "es": "China",
            "de": "China",
            "ja": "中国"
        },
        "iso_code": "CN"
    },
    "registered_country": {
        "geoname_id": 1814991,
        "names": {
            "en": "China",
            "ru": "Китай",
            "fr": "Chine",
            "pt-BR": "China",
            "zh-CN": "中国",
            "es": "China",
            "de": "China",
            "ja": "中国"
        },
        "iso_code": "CN"
    },
    "location": {
        "time_zone": "Asia\/Shanghai",
        "longitude": 118.7778,
        "accuracy_radius": 50,
        "latitude": 32.0617
    },
    "continent": {
        "geoname_id": 6255147,
        "names": {
            "en": "Asia",
            "ru": "Азия",
            "fr": "Asie",
            "pt-BR": "Ásia",
            "zh-CN": "亚洲",
            "es": "Asia",
            "de": "Asien",
            "ja": "アジア"
        },
        "code": "AS"
    }
}
node name: city, value: {
    "geoname_id": 1799962,
    "names": {
        "en": "Nanjing",
        "ru": "Нанкин",
        "fr": "Nankin",
        "pt-BR": "Nanquim",
        "zh-CN": "南京",
        "es": "Nankín",
        "de": "Nanjing",
        "ja": "南京市"
    }
}

```

References
---

- [GeoIP2 City and Country CSV Databases][linkGeoip2CityAndCountryCsvDatabases]
- [lilien1010/lua-resty-maxminddb][]
- [maxmind/libmaxminddb#source#lookup_and_print][]
- [maxmind/libmaxminddb#source#dump_entry_data_list][]

Bug Reports
---
Please report bugs by filing an issue with our GitHub issue tracker at https://github.com/anjia0532/lua-resty-maxminddb/issues

If the bug is casued by libmaxminddb  tracker at https://github.com/maxmind/libmaxminddb/issues

Copyright and License
=====================

This module is licensed under the apache LICENSE-2.0 license.

Copyright 2017-now anjia (anjia0532@gmail.com)

Licensed under the Apache License, Version 2.0 (the "License");

you may not use this file except in compliance with the License.

You may obtain a copy of the License at

	http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software

distributed under the License is distributed on an "AS IS" BASIS,

WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.

See the License for the specific language governing permissions and

limitations under the License.

[maxmind/libmaxminddb]: https://github.com/maxmind/libmaxminddb
[openresty]: https://openresty.org/en/installation.html
[linkGeolite2FreeDownloadableDatabases]: http://dev.maxmind.com/geoip/geoip2/geolite2/
[maxmind/geoipupdate]: https://github.com/maxmind/geoipupdate
[linkGeoip2CityAndCountryCsvDatabases]: https://dev.maxmind.com/geoip/geoip2/geoip2-city-country-csv-databases/
[maxmind/libmaxminddb#source#lookup_and_print]: https://github.com/maxmind/libmaxminddb/blob/master/bin/mmdblookup.c#L262
[maxmind/libmaxminddb#source#dump_entry_data_list]: https://github.com/maxmind/libmaxminddb/blob/master/src/maxminddb.c#L1938
[lilien1010/lua-resty-maxminddb]: https://github.com/lilien1010/lua-resty-maxminddb
