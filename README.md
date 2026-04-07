# 使用 Cloudflare Tunnel + Nginx 反代禁漫图片域名
解决防盗链图片在 freshrss 或手机客户端 Fluent Reader 无法正常显示的问题  
使用 `Cloudflare Worker` 部署会被 CF 盾拦截

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
`https://image-proxy.<your_domain>/<token>/?url=https://cdn-msp2.18comic.vip/templates/frontend/airav/img/title-png/title-circle.png`

## RSSHub 使用示例
```yaml
services:
  rsshub:
    image: diygod/rsshub
    environment:
      HOTLINK_TEMPLATE: https://image-proxy.<your_domain>/<token>/?url=$${href}
      HOTLINK_INCLUDE_PATHS: /18comic
```

## Ref
[解决少数派图片防盗链](https://flowus.cn/share/822c6b09-715d-47f6-8b49-d9fa3804e235)  
[解决IOS端少数派RSS图片加载失败问题](https://renyili.org/post/%E8%A7%A3%E5%86%B3ios%E7%AB%AF%E5%B0%91%E6%95%B0%E6%B4%BErss%E5%9B%BE%E7%89%87%E5%8A%A0%E8%BD%BD%E5%A4%B1%E8%B4%A5%E9%97%AE%E9%A2%98/)  
[目前可用的代理设置](https://github.com/FreshRSS/FreshRSS/issues/675#issuecomment-120707813)  
[无图片反代域名需要启用并修改 image-proxy 插件](https://syu.im/Docker/%E8%A7%A3%E5%86%B3%E5%B0%91%E6%95%B0%E6%B4%BE%E5%9B%BE%E7%89%87%E9%98%B2%E7%9B%97%E9%93%BE/)：`extensions/xExtension-ImageProxy/extension.php -> entry_before_display 改为 entry_before_insert`