## 仓库说明
- 这是一个 **`mihomo`** 的 **`配置文件`** + **`Ruleset规则集合`** 仓库，但**个人色彩**比较重，更推荐作为参考而不是直接引用。
  > 当然直接引用也可以😂

- 仓库提供的 `mihomo` [配置文件](https://github.com/refined-fish/clash_rule_fish?tab=readme-ov-file#配置文件模板)无法直接下载导入运行，里面缺少必要的机场订阅，请保存后根据自己需求修改后使用，[修改教程](https://github.com/refined-fish/clash_rule_fish#mihomo-%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%E6%A8%A1%E6%9D%BF%E4%BF%AE%E6%94%B9%E6%95%99%E7%A8%8B)见下。
  > 若想制作成远程订阅，需要掌握 `nginx反向代理` 一般用法:
  >> 1. 反向代理本仓库的模板文件。
  >> 2. 使用 `nginx_http_sub` 子模块，反向代理时自动替换模板中的 `占位符` 为自己的 `机场订阅` 。
  >> 3. 然后就可以将自己的反向代理地址导入mihomo软件中快乐使用，可以实时同步我得更新（但你需要担心我的不正确更新往配置文件里掺💩）。

- 关于 `Rule` 部分，在非必要时，更推荐各位朋友直接使用 **`geo数据库`** ，减少外部依赖，简化配置文件，提升使用体验❤️

- **值得注意的是**，无论你有什么问题，我都建议你先查看 **`mihomo` [官方文档](https://wiki.metacubex.one/config/general/)**，以及学会 **`yaml`** 的一般语法，否则你既不能学会 `mihomo` 的使用，也可能浪费自己和大家的时间🥲

- 欢迎各位在 `issue` 友善讨论，我看到了都会抽时间回复。

## Ruleset 说明
**`♻️自动选择-FISH`** ：此规则收录的主要是 `geosite:gfw` 以外，必须代理和代理后体验更好的域名。

**`🌐Direct-FISH`** ：此规则收录的是被包括在 `一般代理规则集` 内，但是实际可以直连的域名。

## Icon 说明
`mihomo` 支持为代理组设置icon字段来让显示更漂亮直观，此处收集了部分 `分辨率为px48` 的icon图标以供引用，效果如下
  
  ![image](https://github.com/user-attachments/assets/9fbfd5f6-fe80-4745-8ba0-e1716ccce26f)


## iKuai-Rule 说明
此文件夹内容为 `爱快+openWRT双wan双路由` 架构下，搭配 [ikuai-bypass项目](https://github.com/joyanhui/ikuai-bypass) 使用的
  -  域名直连分流规则
  -  IP直连分流规则

## mihomo 配置文件模板
参见仓库文件 [FISH-Template.yaml](https://raw.githubusercontent.com/refined-fish/clash_rule_fish/refs/heads/main/FISH-Template.yaml)

## mihomo 配置文件模板修改教程
1. 找到代理合集部分
```
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
```
2. 将 `#❗provider占位` 修改为包含自己机场订阅地址的内容，有几个订阅修改几个占位符
```
proxy-providers:
  provider1:
    <<: *proxy-providers-general  # 引用yaml锚点
    override:
      additional-prefix: "provider1|"  # 可选，给订阅添加前缀
    url: "你的机场订阅地址"
    path: ./providers/proxy-provider_main.yaml  # 可选，指定下载路径
  #❗provider占位2
  #❗provider占位3
  #❗provider占位4
```
3. 找到策略组部分
```
use-all-proxy-providers: &use-all-proxy-providers
  use:
    #❗use-provider占位1
    #❗use-provider占位2
    #❗use-provider占位3
    #❗use-provider占位4
```
4. 将修改过的 `#❗provider占位` 引用进去，同上，你写了几个订阅就改几个引用
```
use-all-proxy-providers: &use-all-proxy-providers
  use:
    provider1
    #❗use-provider占位2
    #❗use-provider占位3
    #❗use-provider占位4
```