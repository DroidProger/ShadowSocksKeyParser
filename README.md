# SsKeyParser
Parser for shadowsocks keys on web pages

#### Key must look like  
```
ss://...........#
```

### Always backup your worked shadowsocks config.json 

This app is extract shadowsocks keys from web pages and telegramm channels
To run it type in terminal "path_to_app" "path_to_config_file"
Example for linux
```
/bin/sskeyparser-nix64 /etc/sskeyparser/config.json
```
### Parameters explanation
```
"ssconfigfile":"/etc/shadowsocks/config.json",
```
Path to shadowsocks config file
```
"sspath":"/bin/systemctl",
"ssrestartcommand":[
  "restart",
  "sslocal.service"
],
```
Block of parameters for restarting shadowsocks. If shadowsocks is not running as a service, this section may look like this.
```
"sspath":"/bin/shadowsocks/sslocal",
    "ssrestartcommand":[
        "-c",
        "/etc/shadowsocks/config.json"
    ],
```

If shadowsocks is running as a service, this section may look like this.
```
"sspath":"/bin/systemctl",
    "ssrestartcommand":[
        "restart",
        "sslocal.service"
    ],
```

```
"ssconfigsectionpath":[
        "servers"
    ],
```
Section name in shadowsocks config file, where servers will be added. Your shadowsocks config file must contain this section even it has no servers configurations.

Example of shadowsocks config.json
```
{
  "local_adress":"0.0.0.0"
  ...
  "servers":[
    {
      "server":"permanent_server"
    }
    ...
    {
      "server":"temporary_server"
    }
  ],
  "local_port":1234
  ...
}
```

```
"ssserverseditpos":1,
```
Position from which servers will be edited. In example above first server config will be save, other configs will be rewrite.
```
"sstimeoutdefault":0,
```
Default timeout. Ignored when zero
```
"outputfile":"/etc/sskeyparser/parsingresult.json",
```
Results of parsing is save to this file.
```
"links":[
        {
            "url":"https://t.me/some_channel_with_keys",
            "mask":[
                "ss://"
            ],
            "configcount":3,
            "parsetoptobot":false 
        },
        {
            "url":"https://www.some_site_with_keys.com/",
            "mask":[
                "ss://"
            ],
            "configcount":1,
            "parsetoptobot":true 
        }
    ]
```
Links for parsing. In this section 
```
  "configcount" 
```
how many configs do you want to extract from this page
```
  "parsetoptobot"
```
if true parsing will done from top to bottom. 
- Use "true" for pages where new information placing at the top, like sites
- Use "false" for pages where new information placing at the bottom, like telegram channels and some forums

The following block of parameters is used to filter ip addresses by country. Because some genius makes shadowsocks servers in country with powerfull censorship like Russia. You can exclude this ip from use if it belongs to a specific country.

```
"ipcheckserver"
```
Url to check if parsed IP address belongs to a country you don't want to connect to.

```
"ipcheckkey"
```
Key in json response
```
"ipcheckvalue"
```
Value in json response

It may look like
```
 "ipcheckserver":"https://ipinfo.io/",
 "ipcheckkey":"country",
 "ipcheckvalue":"RU"
```
or
```
 "ipcheckserver":"api.2ip.io/",
 "ipcheckkey":"country",
 "ipcheckvalue":"Russian Federation"
```
I found two servers wich can get simple request and send json response. To check it i used curl
```
curl https://ipinfo.io/1.2.3.4
```
or
```
curl api.2ip.io/1.2.3.4
```
In output you can see how it returns country. For example
```
curl api.2ip.io/1.2.3.4
{
    "ip": "1.2.3.4",
    "city": "Moscow",
    "region": "Moscow",
    "country": "Russian Federation",
    "code": "RU",
    "emoji": ""
    "lat": "",
    "lon": "",
    "timezone": "Europe/Moscow",
    "asn": {
        "id": "",
        "name": "",
        "hosting": false
    },
    "signup": "Get more at 2ip.io/free"
}
```
If you know another servers like those two let me know
