# Rivulet

<p align="center">
  <img src="public/logo.png" width="120" alt="Rivulet Logo" />
</p>

[![codecov](https://codecov.io/gh/songtianlun/rivulet/graph/badge.svg?token=78qRn2zmPD)](https://codecov.io/gh/songtianlun/rivulet)

溪流记账（Rivulet）是一款极简但强大的个人财务管理软件：快速记录收支，支持多账户、多账本与共享账本；提供预算管理、投资记录与财务分析，让你的钱流向清晰可控。

Rivulet is a minimalist yet powerful personal finance app. Track income and expenses fast, organize with multiple accounts and ledgers, and collaborate via shared ledgers. Stay on top of budgets, log investments, and unlock clear analytics to understand where your money goes.

![Rivulet Demo Gif](public/screenshots/Rivulet_demo.gif)

Rivulet 使用 GO 和 Svelte 开发，并使用 Docker 封装并公开发布，支持 SQLite 和 PostgreSQL 数据库。

Rivulet is built with Go and Svelte, and is packaged and published using Docker. It supports both SQLite and PostgreSQL databases.

> 目前处于早期开发阶段，欢迎试用并提出反馈！如果有问题，可以在[这里](https://github.com/songtianlun/rivulet-docs/discussions)讨论。如果你有兴趣参与开发，请[与我联系](https://github.com/songtianlun#contact-me)。 
> 
> Rivulet is in early development. Feedback and contributions are very welcome! If you have questions, please discuss [here](https://github.com/songtianlun/rivulet-docs/discussions). If you're interested in contributing, please [contact me](https://github.com/songtianlun#contact-me).

## 快速开始 | Quick Start

最简单的方式是直接构建镜像并运行容器，映射宿主机 `2212` 到容器 `2212`。

The quickest way is to build the image and run the container with port `2212` mapped to the host.

```bash
docker run --rm -p 2212:2212 rivulet:latest
```

在浏览器打开并访问 `http://localhost:2212`。
Open your browser and navigate to `http://localhost:2212`.

### 持久化数据和配置 | Persisting Data and Config

如果需要保留 SQLite 数据，只需挂载数据目录。

To persist SQLite data, mount the data directory. 

```bash
mkdir -p ./data

docker run -d \
	--name rivulet \
	-p 2212:2212 \
	-v $(pwd)/data:/app/data \
	--env-file .env \
	rivulet:latest
```

数据会写入宿主机的 `./data` 目录。若需要使用配置文件，可放在宿主机的 `./data/rivulet.yaml` 并随目录一同挂载，或通过启动参数 `--config` / 环境变量 `RIVULET_CONFIG` 指定其他位置。

Data is stored in the host `./data` directory. If you want to use a config file, place it at `./data/rivulet.yaml` and mount that directory, or point to another path with `--config` or `RIVULET_CONFIG`.

### 使用 PostgresQL 数据库 | Using PostgreSQL Database

默认情况下，Rivulet 使用内置的 SQLite 数据库。要使用 PostgreSQL，可以设置相关环境变量。

By default, Rivulet uses an embedded SQLite database. To use PostgreSQL, set the relevant environment variables.

```bash
docker run --rm \
	-p 2212:2212 \
    -e RIVULET_DB_DRIVER=postgres \
    -e RIVULET_DB_DSN="host=your-postgres-host user=your-user password=your-password dbname=your-db sslmode=disable" \
    rivulet:latest
```

## 配置方法 | Configuration Methods

推荐通过环境变量配置，尤其是在 Docker 部署中。变量名统一使用 `RIVULET_` 前缀，可参考 `example.env`。

Environment variables are the recommended configuration method, especially for Docker deployments. All variables use the `RIVULET_` prefix. See `example.env`.

```bash
docker run --rm \
	-p 2212:2212 \
	--env-file .env \
	rivulet:latest
```

环境变量会覆盖配置文件中的同名项。

Environment variables override values loaded from the config file.

如需使用配置文件，默认读取 `data/rivulet.yaml`；也可以通过启动参数 `--config` 或环境变量 `RIVULET_CONFIG` 指向其他 YAML 文件。

If you want to use a config file, the default location is `data/rivulet.yaml`. You can also point to another YAML file with `--config` or `RIVULET_CONFIG`.

两种方式都支持，但文档默认推荐环境变量；配置文件更适合需要完整静态配置的场景。

Both methods are supported, but the documentation now recommends environment variables by default. Config files remain useful when you want one complete static config.

## 测试覆盖率图示 | Coverage Visualization

> 我们非常重视代码测试，并使用 Codecov 来跟踪测试覆盖率，并提供了一个 icicle 图来展示代码覆盖率的层次结构。
> 
> We take code testing seriously and use Codecov to track test coverage, providing an icicle graph to visualize the hierarchical structure of our code coverage.

[![Coverage icicle](https://codecov.io/gh/songtianlun/rivulet/graphs/icicle.svg?token=78qRn2zmPD)](https://codecov.io/gh/songtianlun/rivulet)
