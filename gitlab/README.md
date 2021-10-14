# GitLab 及 GitLab-Runner

启动

> docker-compose up -d


## root 账户密码修改

## CLI

进入容器。方式有`手动`和通过 `docker descktop 应用 GUI`

手动方式：
> docker ps -all
> docker exec -it xxxx bash


GUI:

直接点击运行容器的 CLI 图标。

## 修改
进入命令行后执行命令进入 `gitlab 自定义命令行` ：

> gitlab-rails console -e production


查询用户并修改密码

> user=User.where(id:1).first
>
> user.password='1234'
>
> user.password_confirmation='1234'
>
> user.save!
>
> exit

# GitLab Runner 安装

## 概要介绍

`GitLab` 有三种 `Runner`，分别是：

- `Shared runners` are available to all groups and projects in a GitLab instance.
- `Group runners` are available to all projects and subgroups in a group.
- `Specific runners` are associated with specific projects. Typically, specific runners are used for one project at a time.


其中，`Shared runners` 是管理员才能操作的，通常只用在独立或小团队的 `GitLab` 中，默认是没有的。`Group runners` 比较常用，可以支持团队内多个项目共享。可复用的 `Runner`，可以同时支持一类项目的CI，提高资源复用率。

`Specific runners` 则是特定项目用的，不能共享。虽然注册上比较麻烦，但是能满足专用需求。 而且，对个人项目来说，没有Group这一层，只能使用另外两种 `Runner`。由于 `Shared runners` 通常不存在，所以只能使用 `Specific runners`。

`Runner` 的本体，是运行在某台机器上的守护进程。 一个守护进程，可以守护多个 `Runner`。 在Runner自己看来，没有类型的区别，它只是根据 `token` 和 `url`，注册到指定的 `GitLab` 而已。

## 启动

见 `docker-compose.yaml`。


## 注册
### CLI

安装上述同样的方式进入容器。

### 命令

> gitlab-runner register

`GitLab URL`

输入 `GitLab` 的地址。在 `docker-compose.yaml` 因为使用了同一个 `Network`，因此可以直接输入 `http://gitlab`。

`token`

从特定工程中的 `Setting` -> `CI/CD` -> `Runners` 中可以获取

接下来就是输入`描述`和`标签`。`标签`会在工程中的 `.gitlab-ci.yml` 中的 `tags` 中使用。

最后就是`执行器`。可以按照工程的 `.gitlab-ci.yml` 配置进行选择。

在 `Windows` 上，同时使用的是 `Runner` 的二进制文件。如果选择 `shell` 的话，需在其配置文件中（`config.toml`）将 `shell` 修改为 `powershell`。
