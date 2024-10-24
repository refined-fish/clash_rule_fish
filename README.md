### Clash-Meta/Mihomo 内核自用订阅转换模板
改自ACL4SSR的online_full模板，根据个人使用习惯优化了分流，适合稳定长期启用clash且节省流量
```
* 由于个人架设在路由系统中7x24h运行，多人使用，为节省流量，兜底漏网之鱼代理组为直连策略
* 实测能解决绝大部分需求，若有“真·漏网之鱼”欢迎提交增补
```

### 关于fake-ip
个人实测fake-ip提升不明显，但来的隐患却很多，因此模板默认redir-host模式。
但也保留了fake-ip的全部设置（例如filter等），如有需要，转换后只需要在应用中切换fake-ip模式即可，其余设置无需关心


### 订阅转换配置链接
https://raw.githubusercontent.com/refined-fish/clash_rule_fish/main/Fish_Rule.ini
