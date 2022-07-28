# idol prince

## 環境構築方法

### (config/master_keyがない場合)
config/credentials.yml.encは使わないので、 config/master.keyは固定値を使う。

プロジェクトルートで以下を実行
```shell
cat <<EOS > config/master.key
fbf90172f07f81df19da7a919e44d28d
EOS
```

### dockerの起動

```shell
docker volume create --name=idol_prince_rails_cache && docker volume create --name=idol_prince_bundle
docker network create idol_prince_link
docker compose build

docker compose run --rm app bundle install
docker compose run --rm app bin/rails db:create
docker compose run --rm app bin/rails ridgepole:apply
docker compose up
```

## コンテナの操作方法

### イメージを再生成したい場合

```shell
docker compose build --no-cache
```

### 環境をリセットしたい場合

```shell
docker compose down --remove-orphans
```

### ssh container

```shell
docker compose run --rm app sh
```

### bundle install

```shell
docker compose run --rm app bundle install --binstubs
```

### migration

```shell
# 一度適用後の構成を確認する
docker compose run --rm app bin/rails ridgepole:dry_run

docker compose run --rm app bin/rails ridgepole:apply
```

### seeds

```shell
docker compose run --rm app bin/rails db:seed
```

### テーブル構造情報をモデルに付与する


```shell
docker compose run --rm app bundle exec annotate
```


### rails console

```shell
docker compose run --rm app bin/rails console
```

## APIドキュメント

### Swagger UI
http://localhost:3000/api-docs

### ドキュメントの生成、更新

```shell
docker compose run --rm app bin/rails rswag:specs:swaggeriz
```
