# Clash_Rule_FISH 简介

这是一个 **`mihomo`** 的 **`配置文件`** + **`Ruleset`** 仓库，但 **个人色彩** 比较重，更推荐作为参考而不是直接引用。

  > 当然直接引用也可以😂

仓库提供的 [配置文件模板](https://raw.githubusercontent.com/refined-fish/clash_rule_fish/refs/heads/main/FISH-Template.yaml) 无法直接导入代理软件运行，因为缺少必要的机场订阅链接，请自行修改后使用。（修改 [教程](https://github.com/refined-fish/clash_rule_fish#mihomo-%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%E6%A8%A1%E6%9D%BF%E4%BF%AE%E6%94%B9%E6%95%99%E7%A8%8B) 见下文）

**关于路由规则**： 非必要时更推荐各位朋友直接使用 **GEO数据库** ，减少外部依赖，简化配置文件，提升使用体验❤️

**关于Fake-IP**： ~~我没有感受到明显的访问加速，但却感受到了许多问题，例如安全软件不支持fakeip，BT下载、游戏也受到影响。需要额外配置过滤来规避这些影响，增加维护复杂度并且不能一劳永逸。所以本仓库直接全面放弃，但在注释中保留了相关内容，有需要的人可以自行研究。~~ 最近额外花了一些时间配置完善Fakeip相关功能，现在已切换为Fakeip模式，喜欢redir-host模式的朋友仓库也保留了原来redir-host模式的备份可供参考。

**关于订阅转换**： 由于最常用的订阅转换工具 [**`subconverter`**](https://github.com/tindy2013/subconverter) 不能灵活支持配置 **`mihomo`** 的全部字段，因此本仓库没有提供订阅转换模板。本仓库的模板也不能直接变成 **`远程配置`** 使用。如果你想用本仓库的模板实现远程订阅，自动同步仓库更新，此处提供一种较为复杂的 [方案](https://github.com/refined-fish/clash_rule_fish#%E5%B0%86%E6%9C%AC%E4%BB%93%E5%BA%93%E4%BD%9C%E4%B8%BA%E8%BF%9C%E7%A8%8B%E8%AE%A2%E9%98%85%E4%BD%BF%E7%94%A8) 见下文。

最后欢迎各位在 `issue` 友善🙌讨论，我看到了都会抽时间回复。**值得注意的是**，无论你有什么问题，我都建议你先查看 **mihomo [官方文档](https://wiki.metacubex.one/config/general/)**，以及学会 **`yaml`** 的一般语法，否则你既不能学会 mihomo 的使用，也可能浪费自己和大家的时间🥲

## 一、Ruleset

**`♻️自动选择-FISH`** ：此规则收录的主要是 `geosite:gfw` 以外，必须代理或者是代理后体验更好的域名。

**`🌐Direct-FISH`** ：此规则收录的是被包括在 `一般代理规则集` 内，但是实际可以直连或者是直连后体验更好的域名。

## 二、Icon

mihomo 支持为代理组设置icon字段来让显示更漂亮直观，此处收集了部分icon图标以供引用，效果如下
  
  ![image](https://github.com/user-attachments/assets/9fbfd5f6-fe80-4745-8ba0-e1716ccce26f)

## 三、如何使用本仓库配置文件模板（机场订阅版）

1. 下载[配置文件模板](https://raw.githubusercontent.com/refined-fish/clash_rule_fish/refs/heads/main/FISH-Template.yaml)，找到模板中 `代理提供者` 部分

    ```yaml
    proxy-providers:
      #❗provider占位1
      #❗provider占位2
      #❗provider占位3
      #❗provider占位4
    ```

2. 将 `#❗provider占位` 修改为包含自己机场订阅地址的内容，有几个机场订阅修改几个占位符，以下是示例，注意缩进和引用不可遗漏！：

    ```yaml
    proxy-providers:
      provider1:
        <<: *proxy-providers-general  # 引用上文的yaml锚点
        url: "你的机场订阅地址"
        override:  #可选，覆写设置，不需要可以删除
          additional-prefix: "provider1|"  # 可选，给订阅添加前缀，不需要可以删除
        path: ./providers/proxy/proxy-provider1.yaml  # 可选，指定下载路径，不需要可以删除
      #❗provider占位2
      #❗provider占位3
      #❗provider占位4
    ```

3. 再找到 `代理组` 部分

    ```yaml
    use-all-proxy-providers: &use-all-proxy-providers
      use:
        #❗use-provider占位1
        #❗use-provider占位2
        #❗use-provider占位3
        #❗use-provider占位4
    ```

4. 将第 `2` 步修改过的 `#❗provider占位` 引用进去，同上，你写了几个订阅就改几个引用，注意不要遗漏 `-` 短横线

    ```yaml
    use-all-proxy-providers: &use-all-proxy-providers
      use:
        - provider1
        #❗use-provider占位2
        #❗use-provider占位3
        #❗use-provider占位4
    ```

5. 然后就可以将修改后的yaml导入代理软件中当做配置文件使用了。

## 四、如何将本仓库作为远程订阅

  若想将本仓库模板变成远程订阅，需要：

  1. 有自己的 `nginx反向代理` 服务器
  2. 掌握 `nginx反向代理` 一般用法:

  正式开始：

  1. 用自己的 `nginx反向代理服务器` 反向代理本仓库的模板文件。
  2. 在反向代理中使用 `nginx_http_sub` 子模块的 `sub_filter` 功能，自动替换模板中的 `❗占位符` 字段为自己的 `机场订阅` 字段，注意替换中的换行符 `\n` 不可遗漏！更要注意下方可能由于 **引号嵌套** 需要使用的 **转义引号** ！！！

      ```nginx
      server {
        listen 你的nginx监听端口;
        set $subscription "https://raw.githubusercontent.com/refined-fish/clash_rule_fish/refs/heads/main/FISH-Template.yaml";
        location / {
          # 反代我仓库模板
          proxy_pass $subscription;
          # 取消http传输压缩，sub模块只能处理未压缩的文本文件
          proxy_set_header Accept-Encoding "";
          # 替换的文件类型，*为处理所有文件
          sub_filter_types *;
          # 只匹配替换一次（否则全文匹配全部替换）
          sub_filter_once on;
          # 替换内容，有多个订阅可以自己复制多个替换
          sub_filter '#❗provider占位1' 'provider1:\n    <<: *proxy-providers-general\n    override:\n      additional-prefix: "当前订阅的自定义前缀|"\n    url: "你的机场订阅连接"\n    path: ./providers/proxy/proxy-provider1.yaml';
          sub_filter '#❗use-provider占位1' '- provider1';
        }
      }
      ```

  3. 然后就可以将自己的反向代理地址导入代理软件中更新订阅了，可以实时同步我的更新。

  > 但你需要担心我的不正确更新往配置文件里掺💩，以及我自己随性而起的更新，因此我更建议你fork本仓库自行维护，定期同步更改。

## 更新日志

日志我懒得写😫，不如直接看提交日志。

## 致谢

感谢以下人员的帮助

> [@morytyann](https://github.com/morytyann)
