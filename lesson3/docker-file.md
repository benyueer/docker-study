# 书写原则
1. 单一职责
   不同功能尽量拆分为不同容器，每一容器只负责单一业务进程
2. 提供注释信息
3. 保障容器最小化
   避免安装无用的软件包
4. 合理的选择基础镜像
   容器的核心是应用，基础镜像只要满足应用的运行环境即可
5. 使用 .dockerignore 文件
   在构建时忽略掉不需要参与构建的文件
6. 尽量使用构建缓存
   每一条dockerfile指令都会提交给一个镜像层，下一条指令都是基于上一条指令构建的，构建时发现父镜像层已经存在且下一条命相同，即命中构建缓存
   对于ADD和COPY指令，不仅要校验命令是否相同，还要为拷贝的文件计算校验和，命令和校验和完全一致才命中缓存
   因此，最好将变动的部分放到文件尾
7. 正确设置时区
8. 使用国内软件源加快镜像构建速度
9. 最小化镜像层数
   能一条写完的命令不要拆成多条

# 指令书写建议
## RUN
RUN 指令在构建时将会生成一个新的镜像层并执行RUN后的内容
当RUN指令后面的内容比较复杂时，建议使用反斜杠换行

## CMD 和 ENTRYPOINT
### 相同点
```
# exec 模式，基于exec命令实现
CMD/ENTRYPOINT ["command", "param"]

# shell 模式 , 使用 sh -c command
CMD/ENTRYPOINT command param
```

### 不同点
如果使用了ENTRYPOINT指令，启动Docker容器时需要使用 --entrypoint 参数才能覆盖Dockerfile中的ENTRYPOINT指令
而CMD指令则可以背 docker run 后的参数直接覆盖

ENTRYPOINT指令可以结合CMD指令使用，也可以单独使用
而CMD只能单独使用


希望镜像足够灵活，推荐使用CMD
如果镜像只执行单一具体程序，且不希望参数被docker run覆盖，建议使用ENTRYPOINT
无论使用CMD/ENTRYPOINT，都尽量使用exec模式

## ADD和COPY
COPY 只支持基本的文件和文件夹拷贝
ADD 支持更多文件来源类型，比如自动提取tar包，并支持源文件为URL格式

推荐使用 COPY  
COPY更加透明，仅支持本地文件拷贝，而且可以更好的使用构建缓存


## WORKDIR
推荐使用 WORKDIR 指定容器的工作路径
尽量避免使用 RUN cd /work/path && do some work 这样的指令
