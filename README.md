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
## 其他描述
    不收费，但是认证方式还没考虑好，当前添加默认的一年的有效认证
    CD2目前主要作用监听入库 移动 重命名等操作，转存时更快的响应，目前在考虑需不需要移除
    PG库目前暂时先不考虑移除    
    功能很多缺失目前只能满足常规的 转存-整理-生成STRM-刮削-302播放细节待补充
    其他功能，都在考虑开发中
    当前版本并不是完善的一个版本，使用需谨慎~
    
   
-----

