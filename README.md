# Phoenix Hello Docker

Create a phonix app with Docker Compose following this tutorial: [【Elixir 1.11】Phoenix Framework + DB開発をDockerでやる](https://qiita.com/im_miolab/items/84d9d71d109d689d267d) by [im](https://twitter.com/im_miolab).

Build the project

```sh
$ docker-compose build
```

Quick check for the build result

```sh
$ docker-compose run --rm app elixir --version
Elixir 1.11.2 (compiled with Erlang/OTP 23)

$ docker-compose run --rm app mix archive
* hex-0.20.6
* phx_new-1.5.7

$ docker-compose run --rm db psql --version
psql (PostgreSQL) 12.4

$ docker-compose run --rm app bash -c "node --version && npm --version"
v14.15.1
6.14.8
```

Create a phonix project

```sh
$ docker-compose run --rm app mix phx.new my_app
```

Modify the generated `app/my_app/config/dev.exs` file so that config values match those in `.env`.

```sh
config :my_app, MyApp.Repo,
  username: "postgres",   # must match .env
  password: "postgres",   # must match .env
  database: "my_app_dev", # must match .env
  hostname: "db",         # must match "db" as per defined in our `docker-compose.yml` file
```

Start the containers

```sh
$ docker-compose up -d

# Check the status of the containers
$ docker-compose ps
```

Create db

```sh
$ docker-compose run --rm app bash -c "cd my_app && mix ecto.create"
```

Restart the app container

```sh
$ docker-compose restart app
```

Check logs

```sh
$ docker-compose logs
```

Visit http://localhost:4000

Continue the [tutorial](https://qiita.com/im_miolab/items/84d9d71d109d689d267d#crud-webui%E5%AE%9F%E8%A3%85)
