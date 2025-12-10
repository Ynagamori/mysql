# dumptest MySQL 環境

Docker Compose を使ってローカルに MySQL（8.0）を立ち上げるための最小構成です。

## 構成

- `docker-compose.yml`  
  - `mysql` サービスを定義
  - ビルド元: `mysql/Dockerfile`
  - ポート: ホスト `3306` → コンテナ `3306`
  - 永続化: `./mysql/storage` → `/var/lib/mysql`
- `mysql/Dockerfile`  
  - ベースイメージ: `mysql:8.0`
  - タイムゾーン: `Asia/Tokyo`

## 前提条件

- Docker がインストールされていること
- Docker Compose（`docker compose` コマンド）が利用できること

## 起動方法

リポジトリのルート（このファイルがあるディレクトリ）で実行します。

```bash
docker compose up -d
```

起動後、ホストの `localhost:3306` に接続できます。

## 接続情報

`mysql/Dockerfile` で以下の環境変数が設定されています。

- デフォルト DB 名: `default_db`（`MYSQL_DATABASE`）
- 一般ユーザー: `admin`（`MYSQL_USER`）
- 一般ユーザーのパスワード: `admin`（`MYSQL_PASSWORD`）
- root ユーザーのパスワード: `root`（`MYSQL_ROOT_PASSWORD`）

例: `mysql` CLI から接続する場合

```bash
mysql -h 127.0.0.1 -P 3306 -u admin -padmin default_db
```

root で接続する場合:

```bash
mysql -h 127.0.0.1 -P 3306 -u root -proot
```

## データの永続化について

- MySQL のデータディレクトリは `./mysql/storage` にマウントされています。
- 既存の DB データやバイナリログが含まれています。
- 不要に削除すると DB が初期化されるので注意してください。

## 停止 / 再起動

停止:

```bash
docker compose down
```

ログを見ながら起動したい場合:

```bash
docker compose up
```

すでに起動済みのコンテナのログを確認:

```bash
docker compose logs -f
```

