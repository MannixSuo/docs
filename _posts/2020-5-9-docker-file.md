---
title: Docker File
layout: posts
tags: program_language
---

用法：

`docker build` 命令会根据Dockerfile和**context**来生成镜像。

**context**就是在`PATH`或者`URL`位置指定的文件。其中`PATH`是本地文件夹，`URL`是一个远程仓库。

对**context**的处理时递归处理，所以所有的子文件夹都会包括在内。

```shell
# . 代表当前文件夹，下面这个命令吧当前文件夹下的东西都打到镜像中了

docker build .
Sending build context to Docker daemon  6.51 MB

```

Dockerfile这个文件一般在**context**的根目录下，也可以通过`-f`参数来指定Dockerfile的位置。

```shell

docker build -f /path/to/a/Dockerfile .

```

可以通过`-t`命令指定镜像的仓库以及标签

```shell

docker build -t shykes/myapp .

```

`-t`参数可以指定多个

```shell

docker build -t shykes/myapp:1.0.2 -t shykes/myapp:latest .

```

在Docker daemon执行Dockerfile前，会对Dockerfile进行语法检查，错误的语法会报错。

```shell

docker build -t test/myapp .

Sending build context to Docker daemon 2.048 kB
Error response from daemon: Unknown instruction: RUNCMD

```

Docker daemon 一个一个执行Dockerfile中的指令，在最终的镜像id产生之前，会将每个指令的执行结果提交到一个新的镜像中。 Docker daemon会自动清理接收到的**context**。
需要注意：每一条指令都是独立执行的，而且会会生成一个新的镜像，所以指令`RUN cd /tmp`不会影响到它的下个指令。

如果可能的话，docker 会重用中间镜像（cache）来加速构建过程。如果使用了cache的话，输出文本中会显示`Using cache`

```shell

$ docker build -t svendowideit/ambassador .

Sending build context to Docker daemon 15.36 kB
Step 1/4 : FROM alpine:3.2
 ---> 31f630c65071
Step 2/4 : MAINTAINER SvenDowideit@home.org.au
 ---> Using cache
 ---> 2a1c91448f5f
Step 3/4 : RUN apk update &&      apk add socat &&        rm -r /var/cache/
 ---> Using cache
 ---> 21ed6e7fbb73
Step 4/4 : CMD env | grep _TCP= | (sed 's/.*_PORT_\([0-9]*\)_TCP=tcp:\/\/\(.*\):\(.*\)/socat -t 100000000 TCP4-LISTEN:\1,fork,reuseaddr TCP4:\2:\3 \&/' && echo wait) | sh
 ---> Using cache
 ---> 7ea8aef582cc
Successfully built 7ea8aef582cc

```

Format

Dockerfile指令的格式：

```Dockerfile

# Comment
INSTRUCTION arguments

```

指令是不区分大小写的，但是一般都用大写，来区分指令和指令的参数

Docker顺序读取Dockerfile中的指令，注意：*A Dockerfile must begin with a `FROM` instruction.* 这里的开始位置不是说Dockerfile的第一行,FROM指令有可能会声明在解析器指令/注释/全局参数下。FROM指令指定了parent image，然后再这个镜像的基础上进行构建。

以*#*开始的行是注释行，除非这行是有效的解析器指令，在其他地方出现的话就被解析成一个参数：

```Dockerfile

# Comment
RUN echo 'we are running some # of cool things'

```

Parser directives

解析器参数会影响到它下面的指令的处理过程，不会增加构建镜像的层数，并且在build过程中不会显示。解析器参数是通过特殊的注解来指定的`# directive=value`,一旦Dockerfile中有注释，空行，构建指令被处理，解析器参数就不在其作用而是被当做注解处理，所以解析器参数都在Dockerfile的最上方。解析器参数是不区分大小写的，一般情况下都用小写。

当前支持两个解析器参数:syntax,escape

Syntax

语法格式:

`# syntax=[remote image reference]`

示例

```Dockerfile

# syntax=docker/dockerfile
# syntax=docker/dockerfile:1.0
# syntax=docker.io/docker/dockerfile:1
# syntax=docker/dockerfile:1.0.0-experimental
# syntax=example.com/user/repo:tag@sha256:abcdef...

```

此功能只有在启用buildKit时才有效。

Escape

通过escape定义dockerfile中的转义字符

```dockerfile

# escape=\ (backslash)

```

```dockerfile

# escape=` (backtick)

```

Environment replacement

通过ENV语句定义的环境变量，可以在其他的dockerfile命令中引用，

```dockerfile

$variable_name


${variable_name}

```

`${variable_name}`这种还支持bash修饰符语法

`${variable:-word}` 如果`variable`不存在这个表达式值就是`word`.
`${variable:+word}`  如果`variable`存在,那么这个表达式值就是`word`,否则是空字符串.

example：

```dockerfile

FROM busybox
ENV foo /bar
WORKDIR ${foo}   # WORKDIR /bar
ADD . $foo       # ADD . /bar
COPY \$foo /quux # COPY $foo /quux

```

From

```dockerfile

FROM [--platform=<platform>] <image> [AS <name>]

FROM [--platform=<platform>] <image>[:<tag>] [AS <name>]

FROM [--platform=<platform>] <image>[@<digest>] [AS <name>]

```

form命令初始化一个新的构建层，并且下面的命令都是在这个基本镜像上面进行操作。

RUN

* `RUN <command>` (shell格式,相当于执行一个shell命令，默认是`/bin/sh -c`)
* `RUN ["executable", "param1", "param2"]` (exec 格式)

Run命令会在当前镜像上新建一层然后将执行结果保存在新建的那层上，然后这个命令下面的命令都在新的镜像上操作。

如果一行命令太长可以用反斜杠`\`来开启新的一行

```dockerfile

//shell 形式
RUN /bin/bash -c 'source $HOME/.bashrc; \
echo $HOME'

//exec形式
RUN ["/bin/bash", "-c", "echo hello"]

```

CMD

用于指定默认的容器主进程的启动命令
每个dockerfile只能有一个CMD命令，存在多个的话只有最后一个会生效。

`CMD ["executable","param1","param2"]`

`CMD ["param1","param2"]`

`CMD command param1 param2`

LABEL

```dockerfile

LABEL <key>=<value> <key>=<value> <key>=<value> ...

```

label为镜像添加元数据

EXPOSE

```dockerfile

EXPOSE <port> [<port>/<protocol>...]

```

expose 声明了容器需要暴露的端口，声明expose不会主动暴露端口，需要在运行`docker run` 命令的时候增加`-p` 参数来指定暴露端口。

```dockerfile

EXPOSE 80/tcp
EXPOSE 80/udp

```

```shell

docker run -p 80:80/tcp -p 80:80/udp ...

```

ENV

env命令是用来设置环境变量的，设置好以后其他命令就能获取到环境变量的值。

```dockerfile
ENV <key> <value>
ENV <key>=<value> ...
```

env的声明有两种形式

```dockerfile
ENV myName="John Doe" myDog=Rex\ The\ Dog \
    myCat=fluffy

ENV myName John Doe
ENV myDog Rex The Dog
ENV myCat fluffy
```

在执行docker命令时可对env进行覆盖`docker run --env <key>=<value>`

ADD

add有两种形式

```dockerfile
ADD [--chown=<user>:<group>] <src>... <dest>
# 如果有空格用下面这种
ADD [--chown=<user>:<group>] ["<src>",... "<dest>"]
#--chown=<user>:<group>这个命令只有在linux镜像仲裁有用，windows镜像中无作用
```

add的作用是将在`<src>`声明的文件目录或者远程文件的url添加到镜像的`<dest>`路径下。

可以指定多个`<src>`，如果它们是文件或目录，则将其路径解释为相对于构建上下文的路径。

add还支持go的文件路径匹配规则如：

```dockerfile
# 添加一hom开头的文件
ADD hom* /mydir/
# ？代表任意一个字符 如home.txt
ADD hom?.txt /mydir/
```

`<dst>`代表源文件要拷贝到容器中的目标地址，一般是一个绝对路径，或者是相对于WORKDIR的路径。

```dockerfile
# 将test.txt添加到<WORKDIR>/relativeDir/ 路径下
ADD test.txt relativeDir/
# 将test.txt添加到/absoluteDir/ 这个绝对路径下
ADD test.txt /absoluteDir/
```

Note:

1. `<src>`必须在`context`中
2. 如果`<src>`是url文件，并且`<dest>`不以

