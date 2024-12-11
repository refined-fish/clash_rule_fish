## 仓库说明
- 这是一份 `mihomo` 的Rule规则集合仓库，但**个人色彩**比较重，更推荐作为参考而不是直接引用。
  > 当然直接引用也可以😂

- 同时仓库提供了一份供参考的 `mihomo` 运行[配置文件](https://github.com/refined-fish/clash_rule_fish?tab=readme-ov-file#配置文件模板)，本配置文件无法直接运行，请保存后根据自己需求修改后使用。

- 在非必要时，更推荐各位朋友直接使用 **`geo数据库`** ，减少外部依赖，简化配置文件，提升使用体验❤️

- **值得注意的是**，无论你有什么问题，我都建议你先查看 `mihomo` [官方文档](https://wiki.metacubex.one/config/general/)，否则你既不能学会 `mihomo` 的使用，也可能浪费自己和大家的时间🥲

- 欢迎各位在 `issue` 友善讨论，我看到了都会抽时间回复。

## Ruleset 说明
**`♻️自动选择-FISH`** ：此规则收录的主要是 `geosite:gfw` 以外，必须代理和代理后体验更好的域名。

**`🌐Direct-FISH`** ：此规则收录的是被包括在 `一般代理规则集` 内，但是实际可以直连的域名。

## iKuai-Rule 说明
此文件夹内容为 `爱快+openWRT双wan双路由` 架构，搭配 [ikuai-bypass项目](https://github.com/joyanhui/ikuai-bypass) 使用的
  -  域名直连分流规则
  -  IP直连分流规则

## mihomo 配置文件模板
```
#mixed-port: 7890  #⭕混合端口，系统代理端口
#socks-port: 1080  #⭕SOCKS5 代理端口
#port: 8080  #⭕HTTP 代理端口
#redir-port: 7891  #⭕redir 透明代理端口，非linux系会报错
#tproxy-port: 7892  #⭕tproxy 透明代理端口，非linux系会报错

###########################################################################################################################
allow-lan: true  #⭕允许局域网
bind-address: "*"  #⭕监听地址，*为所有
lan-allowed-ips:  # 允许连接的客户端IP，默认允许全部
  - 0.0.0.0/0
  - ::/0
lan-disallowed-ips:  # 禁止连接的IP，优先级高于白名单
  - ~
authentication:  # http(s)/socks/mixed 代理的用户验证，格式："user1:pass1"，不写则禁止全部用户
  - ~
skip-auth-prefixes:  # 白名单，允许跳过用户验证的IP
  - ~
mode: rule  # 运行模式：rule（规则） / global（全局代理）/ direct（全局直连）
log-level: warning  #⭕日志级别
ipv6: true  #⭕是否接受ipv6流量
keep-alive-idle: 15  # 触发心跳包的无活动阈值
keep-alive-interval: 15  # 发送心跳包的间隔，最多9次
disable-keep-alive: false  # 禁用 keep alive，允许链接一直存在
find-process-mode: strict  # 进程匹配：strict由mihomo决定是否开启（off:关闭，alwas:开启）
external-controller: 0.0.0.0:9090  #⭕API面板监听地址
secret: ""  #⭕API面板访问密钥
external-ui: ./ui  #⭕API面板静态资源路径
external-ui-name: metacubexd  #⭕API面板名，配合路径拼接成路径，例如/ui/metacubexd
external-ui-url: "https://github.com/MetaCubeX/metacubexd/archive/refs/heads/gh-pages.zip"  #⭕API面板下载地址
profile:  # 缓存设置
  store-selected: true  # 缓存 API面板 对策略组的选择
  store-fake-ip: true  # 缓存 fakeip 映射表
unified-delay: false  # 统一延迟计算，没啥卵用
tcp-concurrent: true  # TCP并发
#interface-name: en0  #⭕clash流量出站接口，即接口绑定
#routing-mark: 6666  # Linux下的路由标记，为流量提供标记
global-client-fingerprint: random  # 全局客户端指纹，优先低于其他处的声明
geodata-mode: true  # GEOIP数据模式，false:mmdb，true:dat，mmdb更新但有设备不支持
geodata-loader: standard  # GEO文件加载模式，默认memconservative（小内存设备模式）
geo-auto-update: true  # GEO文件自动更新
geo-update-interval: 24  # 更新间隔，小时
geox-url:  # GEO数据库下载地址，内核会按需下载
  geoip: "https://github.com/MetaCubeX/meta-rules-dat/releases/download/latest/geoip.dat"
  geosite: "https://github.com/MetaCubeX/meta-rules-dat/releases/download/latest/geosite.dat"
  mmdb: "https://github.com/MetaCubeX/meta-rules-dat/releases/download/latest/geoip.metadb"
  asn: "https://github.com/xishang0128/geoip/releases/download/latest/GeoLite2-ASN.mmdb"
global-ua: clash.meta  # 全局UA，默认为clash.meta
etag-support: true  # 资源下载的 ETag 支持

###########################################################################################################################
public-dns: &public-dns
  - dhcp://wan   # 指定网卡获取的DNS，硬路由的wan口网卡名为wan
  - dhcp://eth1  # 指定网卡获取的DNS，软路由的wan口网卡名为最后一张网卡，双口就是eth1
  - 223.5.5.5    # 阿里主DNS
  - 223.6.6.6    # 阿里副DNS

secure-dns: &secure-dns
  - https://8.8.8.8/dns-query   # 谷歌主DNS
  - https://1.1.1.1/dns-query   # Cloudflare主DNS
  - https://1.0.0.1/dns-query   # Cloudflare次DNS

dns:
  enable: true  #⭕启用dns
  prefer-h3: true  # 优先使用HTTP/3，DoH能加速
  listen: 0.0.0.0:1053  #⭕DNS监听端口，仅支持udp
  ipv6: true  # 是否解析ipv6地址，否则返回空结果
  enhanced-mode: redir-host  #⭕DNS处理模式：redir-host/fake-ip，fake-ip更快但引入问题（缓存fakeip后无法故障切换）
  fake-ip-range: 198.18.0.1/16  # fakeip 下的 IP 段设置，tun接口的默认 IPV4 地址 也使用此值作为参考
  fake-ip-filter:  # fakeip过滤
    - geosite:category-ads-all,category-games,cn,private
  fake-ip-filter-mode: blacklist  # 黑/白名单模式，白名单下 filter 会返回 fake-ip
  use-hosts: true  # 查询配置文件 hosts
  use-system-hosts: true  # 查询系统 hosts
  respect-rules: false  # DNS请求遵循路由规则，需先配置好 proxy-server-nameserver，否则出现鸡蛋问题
  
  nameserver-policy:  # 优先级高于nameserver
    'geosite:category-ads-all': rcode://success  # 匹配广告域名
    'geosite:cn,apple,private': *public-dns  # 匹配中国，苹果域名，私有地址
    'geosite:geolocation-!cn': *secure-dns   # 匹配非中国域名
  proxy-server-nameserver: *secure-dns  # 解析节点域名的DNS，不指定则遵循namepolicy
  nameserver: *public-dns  # 默认DNS组，域名未命中 nameserver-policy 时使用
  direct-nameserver: *public-dns  # direct策略组的 DNS 服务器，不填则遵循nameserver-policy

###########################################################################################################################
hosts:
  'mihomo.mihomo': 127.0.0.1

###########################################################################################################################
sniffer:                      # 嗅探，获取SNI中的域名
  enable: true                # 启用sniffer
  force-dns-mapping: true     # 对redir-host类型识别的流量进行强制嗅探
  parse-pure-ip: true         # 对所有未获取到域名的流量进行强制嗅探
  override-destination: true  # 是否使用嗅探结果作为实际访问，默认为 true
  sniff:                      # 需要嗅探的协议类型，仅支持 HTTP/TLS/QUIC
    TLS:                      # TLS协议，不配置默认嗅探443端口
    HTTP:                     # HTTP协议
      ports:                  # 端口白名单，只针对下列端口嗅探，不配置则全部嗅探
        - 80
  force-domain:               # 强制嗅探的域名
    - "+.netflix.com"
    - "+.nflxvideo.net"
    - "+.amazonaws.com"
    - "+.media.dssott.com"
  skip-domain:                # 需要跳过嗅探的域名。主要解决部分站点sni字段非域名，导致嗅探结果异常的问题，如米家设备
    - "+.apple.com"
    - Mijia Cloud
    - dlg.io.mi.com
    - "+.oray.com"
    - "+.sunlogin.net"
    - geosite:cn

###########################################################################################################################
tun:
  enable: false                 #⭕启用Tun接口
  stack: mixed                  # TUN 堆栈模式,建议 mixed（TCP：system，UDP：Gvisor）
  device: utun0                 # TUN 网卡名，MacOS 只能 utun 开头
  auto-route: true              # 自动设置全局路由，自动将全局流量路由进 tun
  auto-redirect: true           # 仅Linux有效，自动配置 iptables/nftables 以重定向 TCP 连接，需要auto-route: true
  auto-detect-interface: true   # 自动选择流量的出接口（wan）
  dns-hijack:                   # dns 劫持，将匹配到的连接导入内部 dns 模块，不书写协议则默认 udp://
    - any:53
    - tcp://any:53
  strict-route: true            # 严格路由，添加防火墙规则禁止Windows使用多DNS，有效防止DNS泄露
  mtu: 9000                     # MTU
  gso: true                     # 通用分段卸载
  gso-max-size: 65536           # 数据块最大长度
  endpoint-independent-nat: false # 独立于端点的 NAT，性能可能下降，不建议不需要时开启
  exclude-package:              # 安卓排除的包名
    - com.tencent.mobileqq      # QQ
    - com.tencent.mm            # 微信
    - com.tencent.androidqqmail # QQ邮箱
    - com.xingin.xhs            # 小红书
    - idm.internet.download.manager  # 1DM下载器
    - com.sausage.download      # 浩克下载
    - com.eg.android.AlipayGphone  # 支付宝
    - com.sgcc.wsgw.cn          # 网上国网
    - com.taobao.taobao         # 淘宝
    - com.jingdong.app.mall     # 京东
    - com.MobileTicket          # 铁路12306
    - com.chinamworld.main      # 中国建设银行
    - com.jd.jrapp              # 京东金融

###########################################################################################################################
proxy-providers-general: &proxy-providers-general
  type: http      # 类型，可选http/file
  interval: 86400
  proxy: DIRECT
  size-limit: 0
  override:
    tfo: false  # 启用TCP Fast Open，一般都不支持
    mptcp: false  # 启用TCP Multi Path，一般都不支持
    udp: true
    udp-over-tcp: false  # 启用 UDP over TCP
    skip-cert-verify: false  # 跳过证书验证

proxy-providers:
  #❗provider占位1
  #❗provider占位2
  #❗provider占位3
  #❗provider占位4

###########################################################################################################################
use-all-proxy-providers: &use-all-proxy-providers
  use:
    #❗use-provider占位1
    #❗use-provider占位2
    #❗use-provider占位3
    #❗use-provider占位4

health-check-general: &health-check-general
  <<: *use-all-proxy-providers
  url: http://www.gstatic.com/generate_204
  interval: 30
  tolerance: 200
  lazy: true
  timeout: 5000

proxy-groups:
  - name: ♻️自动选择
    type: url-test
    <<: *health-check-general
  
  - name: ⚠️自动回退
    type: fallback
    <<: *health-check-general
  
  - name: 🐟漏网之鱼
    type: select
    proxies:
      - DIRECT
      - ♻️自动选择
      - 🇭🇰香港节点
      - 🇨🇳台湾节点
      - 🇸🇬狮城节点
      - 🇯🇵日本节点
      - 🇺🇲美国节点
      - 🖐️手动切换
  
  - name: 🖐️手动切换
    type: select
    <<: *use-all-proxy-providers
  
  - name: Ⓜ️微软Bing
    type: select
    proxies:
      - ⚠️自动回退
      - ♻️自动选择
      - 🇭🇰香港节点
      - 🇨🇳台湾节点
      - 🇸🇬狮城节点
      - 🇯🇵日本节点
      - 🇺🇲美国节点
      - 🖐️手动切换
      - DIRECT
  
  - name: 🤖OpenAi
    type: select
    proxies:
      - 🇸🇬狮城节点
      - ♻️自动选择
      - 🇭🇰香港节点
      - 🇨🇳台湾节点
      - 🇯🇵日本节点
      - 🇺🇲美国节点
      - 🖐️手动切换
  
  - name: 📞电报消息
    type: select
    proxies:
      - ♻️自动选择
      - 🇭🇰香港节点
      - 🇨🇳台湾节点
      - 🇸🇬狮城节点
      - 🇯🇵日本节点
      - 🇺🇲美国节点
      - 🖐️手动切换
  
########################### 
  - name: 🇭🇰香港节点
    type: url-test
    <<: *health-check-general
    filter: "港|HK|hk|Hong Kong|HongKong|hongkong"
  
  - name: 🇯🇵日本节点
    type: url-test
    <<: *health-check-general
    filter: "日本|川日|东京|大阪|泉日|埼玉|沪日|深日|JP|Japan"
  
  - name: 🇺🇲美国节点
    type: url-test
    <<: *health-check-general
    filter: "美|波特兰|达拉斯|俄勒冈|凤凰城|费利蒙|硅谷|拉斯维加斯|洛杉矶|圣何塞|圣克拉拉|西雅图|芝加哥|US|United States"
  
  - name: 🇨🇳台湾节点
    type: url-test
    <<: *health-check-general
    filter: "台|新北|彰化|TW|Taiwan"
  
  - name: 🇸🇬狮城节点
    type: url-test
    <<: *health-check-general
    filter: "新加坡|坡|狮城|SG|Singapore"

###########################################################################################################################
rule-providers-general: &rule-providers-general
  type: http
  behavior: classical
  interval: 86400

rule-providers:  
  ♻️自动选择-FISH:
    <<: *rule-providers-general
    url: https://raw.githubusercontent.com/refined-fish/clash_rule_fish/refs/heads/main/Ruleset/♻️自动选择-FISH.yaml
    path: ./providers/rule-provider_♻️自动选择-FISH.yaml
  
  🌐Direct-FISH:
    <<: *rule-providers-general
    url: https://raw.githubusercontent.com/refined-fish/clash_rule_fish/refs/heads/main/Ruleset/🌐Direct-FISH.yaml
    path: ./providers/rule-provider_🌐Direct-FISH.yaml
  
  UnBan:
    <<: *rule-providers-general
    url: https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/refs/heads/master/Clash/Providers/UnBan.yaml
    path: ./providers/rule-provider_UnBan.yaml
  
  Download:
    <<: *rule-providers-general
    url: https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/refs/heads/master/Clash/Providers/Download.yaml
    path: ./providers/rule-provider_Download.yaml
  
  GoogleCN:
    <<: *rule-providers-general
    url: https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/refs/heads/master/Clash/Providers/Ruleset/GoogleCN.yaml
    path: ./providers/rule-provider_GoogleCN.yaml
  
  Bing:
    <<: *rule-providers-general
    url: https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/refs/heads/master/Clash/Providers/Ruleset/Bing.yaml
    path: ./providers/rule-provider_Bing.yaml
  
  Microsoft:
    <<: *rule-providers-general
    url: https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/refs/heads/master/Clash/Providers/Ruleset/Microsoft.yaml
    path: ./providers/rule-provider_Microsoft.yaml

###########################################################################################################################
rules:
  - AND,((NETWORK,UDP),(DST-PORT,443)),REJECT  # 禁用QUIC
  - RULE-SET,♻️自动选择-FISH,♻️自动选择
  - RULE-SET,🌐Direct-FISH,DIRECT
  - GEOIP,private,DIRECT,no-resolve
  - GEOSITE,private,DIRECT
  - RULE-SET,UnBan,DIRECT
  - GEOSITE,category-ads-all,REJECT
  - RULE-SET,Download,DIRECT
  - RULE-SET,GoogleCN,DIRECT
  - GEOSITE,steam@cn,DIRECT
  - RULE-SET,Bing,Ⓜ️微软Bing
  - GEOSITE,onedrive,DIRECT
  - RULE-SET,Microsoft,DIRECT
  - GEOSITE,openai,🤖OpenAi
  - GEOSITE,bahamut,🇨🇳台湾节点
  - GEOSITE,telegram,📞电报消息
  - GEOIP,telegram,📞电报消息,no-resolve
  - GEOSITE,category-porn,♻️自动选择
  - GEOSITE,gfw,♻️自动选择
  - GEOSITE,cn,DIRECT
  - GEOIP,cn,DIRECT,no-resolve
  - MATCH,🐟漏网之鱼
```
