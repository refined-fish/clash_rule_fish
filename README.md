# Clash_Rule_FISH

- 这是一个**mihomo**的 **`配置文件`** + **`Ruleset规则集合`** 仓库，但**个人色彩**比较重，更推荐作为参考而不是直接引用。

  > 当然直接引用也可以😂

- 仓库提供的[mihomo配置文件模板](https://raw.githubusercontent.com/refined-fish/clash_rule_fish/refs/heads/main/FISH-Template.yaml)无法直接导入软件运行，缺少必要的机场订阅，请自行修改后使用。（[修改教程](https://github.com/refined-fish/clash_rule_fish#mihomo-%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%E6%A8%A1%E6%9D%BF%E4%BF%AE%E6%94%B9%E6%95%99%E7%A8%8B)）

- 由于最常用的订阅转换工具 [`subconverter`](https://github.com/tindy2013/subconverter) 不支持mihomo的高级功能，无法将本仓库直接变成 `subconverter` 的远程配置使用。如果你想用本仓库的模板实现远程订阅，自动同步仓库更新，此处提供一种较为[复杂的方案](https://github.com/refined-fish/clash_rule_fish#%E5%B0%86%E6%9C%AC%E4%BB%93%E5%BA%93%E4%BD%9C%E4%B8%BA%E8%BF%9C%E7%A8%8B%E8%AE%A2%E9%98%85%E4%BD%BF%E7%94%A8)。

- 关于 `路由规则Rule` 部分，在非必要时，更推荐各位朋友直接使用 **`geo数据库`** ，减少外部依赖，简化配置文件，提升使用体验❤️

- **值得注意的是**，无论你有什么问题，我都建议你先查看 **mihomo [官方文档](https://wiki.metacubex.one/config/general/)**，以及学会 **`yaml`** 的一般语法，否则你既不能学会 mihomo 的使用，也可能浪费自己和大家的时间🥲

- 欢迎各位在 `issue` 友善🙌讨论，我看到了都会抽时间回复。

## 更新日志

参见[ChangeLog.log](https://raw.githubusercontent.com/refined-fish/clash_rule_fish/refs/heads/main/ChangeLog.log)文件。

## Ruleset 说明

**`♻️自动选择-FISH`** ：此规则收录的主要是 `geosite:gfw` 以外，必须代理和代理后体验更好的域名。

**`🌐Direct-FISH`** ：此规则收录的是被包括在 `一般代理规则集` 内，但是实际可以直连的域名。

## Icon 说明

mihomo 支持为代理组设置icon字段来让显示更漂亮直观，此处收集了部分 `分辨率为px48` 的icon图标以供引用，效果如下
  
  ![image](https://github.com/user-attachments/assets/9fbfd5f6-fe80-4745-8ba0-e1716ccce26f)

## iKuai-Rule 说明

此文件夹内容为 `爱快+openWRT双wan双路由` 架构下，搭配 [ikuai-bypass项目](https://github.com/joyanhui/ikuai-bypass) 使用的

- 域名直连分流规则
- IP直连分流规则

## 如何修改 mihomo 配置文件模板

1. 找到模板中 `代理合集` 部分

    ```yaml
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

2. 将 `#❗provider占位` 修改为包含自己机场订阅地址的内容，有几个机场订阅修改几个占位符

    ```yaml
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
      provider1:
        <<: *proxy-providers-general  # 引用上方yaml锚点
        url: "你的机场订阅地址"
        override:  #可选，覆写设置，不需要可以删除
          additional-prefix: "provider1|"  # 可选，给订阅添加前缀，不需要可以删除
        path: ./providers/proxy-provider1.yaml  # 可选，指定下载路径，不需要可以删除
      #❗provider占位2
      #❗provider占位3
      #❗provider占位4
    ```

3. 找到策略组部分

    ```yaml
    use-all-proxy-providers: &use-all-proxy-providers
      use:
        #❗use-provider占位1
        #❗use-provider占位2
        #❗use-provider占位3
        #❗use-provider占位4
    ```

4. 将第2.步修改过的 `#❗provider占位` 引用进去，同上，你写了几个订阅就改几个引用

    ```yaml
    use-all-proxy-providers: &use-all-proxy-providers
      use:
        - provider1
        #❗use-provider占位2
        #❗use-provider占位3
        #❗use-provider占位4
    ```

5. 然后就可以导入配置文件使用了。

## 如何将本仓库作为远程订阅使用

  若想将本仓库模板变成远程订阅，需要：

  1. 有自己的 `nginx反向代理` 服务器
  2. 掌握 `nginx反向代理` 一般用法:

  正式开始：

  1. 用自己的 `NGINX反向代理服务器` 反向代理本仓库的模板文件。
  2. 在反向代理中使用 `nginx_http_sub` 子模块的 `sub_filter` 功能，自动替换模板中的 `❗占位符` 字段为自己的 `机场订阅` 字段。

      ```yaml
      sub_filter '#❗provider占位1' 'provider1:\n    <<: *proxy-providers-general\n    override:\n      additional-prefix: "一元|"\n    url: "你的机场订阅"\n    path: ./providers/proxy-provider1.yaml';
      sub_filter '#❗use-provider占位1' '- provider1';
      ```

  3. 然后就可以将自己的反向代理地址导入mihomo软件中快乐使用，可以实时同步我的更新（但你需要担心我的不正确更新往配置文件里掺💩）。

## 致谢

感谢以下人员的帮助
> [@morytyann](https://github.com/morytyann)
