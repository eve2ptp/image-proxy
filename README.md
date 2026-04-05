# 使用 Cloudflare Tunnel + Nginx 反代禁漫图片域名
解决防盗链图片在 freshrss 或手机客户端 Fluent Reader 无法正常显示的问题

## 环境
VPS + Docker

## 配置
修改 `docker-compose.yml` 映射端口  
修改 `config/nginx.conf` 禁漫画域名，token 防止被滥用  
在 Cloudflare 新建 Tunnel `image-proxy.<your_domain>`，指向 http://localhost:<映射端口>

## 启动
```sh
mkdir {cache,logs}
docker compose up -d
```

## 测试
测试：`https://image-proxy.<your_domain>/<token>/?url=https://cdn-msp2.18comic.vip/templates/frontend/airav/img/title-png/title-circle.png`

## RSSHub 使用示例
```yaml
services:
  rsshub:
    image: diygod/rsshub
    environment:
      HOTLINK_TEMPLATE: https://image-proxy.evimo.cc/dc78d870-b493-4afa-9415-34f1221beb16/?url=$${href}
      HOTLINK_INCLUDE_PATHS: /18comic
```

## FreshRSS 相关
[image-proxy 插件修改](https://syu.im/Docker/%E8%A7%A3%E5%86%B3%E5%B0%91%E6%95%B0%E6%B4%BE%E5%9B%BE%E7%89%87%E9%98%B2%E7%9B%97%E9%93%BE/)：`extensions/xExtension-ImageProxy/extension.php -> entry_before_display 改为 entry_before_insert`  
[freshrss 目前可用的代理设置](https://github.com/FreshRSS/FreshRSS/issues/675#issuecomment-120707813)
