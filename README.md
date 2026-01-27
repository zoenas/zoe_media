# ZoeMedia

## 讨论方式
-----

## 介绍
暂时不考虑开源

*  默认账号密码 admin/admin123

## 其他容器
    Postgres16.x
    cd2

```yaml
services:
  zoe_media:
    image: zoenas/zoemedia:latest
    container_name: zoe_media
    restart: on-failure
    environment:
      - DB_HOST=local
      - DB_PORT=5433
      - DB_NAME=zoe_media
      - DB_USER=zoe_media
      - DB_PASSWORD=zoe_media
    ports:
      - "8080:8080"
#    volumes:
#      # - ./logs:/app/logs
#      # - /path/to/media:/media
  zoe_media_emby:
    image: zoenas/zoe_media_emby:latest
    container_name: zoe_media_emby
    network_mode: bridge
    # 如果需要使用bridge(桥接)网络,请取消ports(端口映射)注释,并注释network_mode
    # 端口映射规则为,宿主机端口:容器内部端口
    ports:
      - 18096:8098
    # 如果遇到海报全部裂开或日志中 permission denied,以下二选一
    # 还不行的话,自己给宿主机 embyCache 读写权限
    # privileged: true
    # entrypoint: |
    #   /bin/sh -c "chmod -R 777 /var/cache/nginx/emby"
    volumes:
#      - /vol1/1000/AppData/zoe_media_emby/nginx/nginx.conf:/etc/nginx/nginx.conf
#      - /vol1/1000/AppData/zoe_media_emby/nginx/conf.d:/etc/nginx/conf.d
      - /vol1/1000/AppData/zoe_media_emby/nginx/embyCache:/var/cache/nginx/emby
      - /vol1/1000/AppData/zoe_media_emby/nginx/log:/var/log/nginx
    environment:
      - EMBY_HOST=http://127.0.0.1:8096
      - EMBY_API_KEY=your_real_api_key_here
    restart: always
```
