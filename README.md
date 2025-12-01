在确保安装docker的情况下将dockerfile拷贝至任意文件夹（e.g./home/lyz/sisso）
运行:

```language
docker build /home/lyz/sisso
```
等待特别特别久之后（主要是文件大网速慢）显示

```language
Successfully built 675189be63ce
```
这样就构建成功了
运行

```language
docker image ls
```
可以看到类似如下输出：

```language
[lyz@kk-ejixarmkmzikcep3fi5s ~]$ docker image ls
REPOSITORY                             TAG                    IMAGE ID       CREATED          SIZE
<none>                                 <none>                 675189be63ce   2 minutes ago    5.55GB

```
我们可以将镜像重新命名

```language
docker tag 675189be63ce(填你自己的IMAGE ID) sisso:1.0
```
（不建议的操作除非你是老手）
创建容器

```language
docker run -it -v /home/lyz/sisso（你linux中存出SISSO.in的文件夹）:/sisso_workspace sisso:1.0 /bin/bash
```
然后你就可以在容器中执行

```language
mpirun -n 1 SISSO
```
你可以在/home/lyz/sisso中看到结果
建议的操作：
先切换到你的工作目录，例如：

```language
cd /home/lyz/sisso
```
确保里面有：SISSO.in以及train.dat
运行：

```language
docker run --rm -v $(pwd):/sisso_workspace sisso:1.0 mpirun -n 1 SISSO
```

于是你可以在工作目录中找到输出文件SISSO.out


其他一些注意点：
dockerfile中采用的docker镜像源为docker.1ms.run为了在国内能够正常连接
github源采用了bgithub.xyz
如果这两个镜像站失效，那么需要自行替换镜像源
