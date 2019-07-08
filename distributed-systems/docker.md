# docker

```bash
# 重新创建镜像，一定要加--no-cache
docker-compose -f docker-compose.yml build --nocache
```

```text
# build do not pull images
docker-compose -f docker-compose.yml build

# up will automative pull images
docker-compose -f docker-compose.yml up
```

```text
# 下面方法停止一个container的时候，container是会保存运行状态的，通过 docker ps -a -q
# 还是能找到得到刚停止的container
docker stop containername

# 下面这种方式删除container，是会停止container并且删除运行状态，通过 docker ps -a -q
# 将不会列出来
docker rm containername
```

