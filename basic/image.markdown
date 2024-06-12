# image 命令

```shell
docker system df
docker images
docker images -q
docker pull <Image Name>:<TAG>
docker rmi -f <Image Name/ID>
docker rmi -f $(docker ps -aq)
docker tag SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]
```

```shell
docker save my_image:latest > my_image_latest.tar
docker load my_image_latest.tar
```