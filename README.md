
■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■


（５）

　　アプリからDB接続後に、CRUDsの構築（所要時間: 　分）


FastAPI入門
FastAPI開発入門

https://zenn.dev/sh0nk/books/537bb028709ab9/viewer/bdf8a5


https://zenn.dev/kou_kawa/articles/08-first-mysql-fastapi



■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■


■■■ Section 1 DockerImageの作成■■■



■STEP 1 定義ファイルの内容を実行する→Docker Image, containerを作成


$docker compose -f C:\Users\maete\Documents\fastAPI_folder7\docker-compose.yml up -d 

OR 

$cd C:\Users\maete\Documents\fastAPI_folder7

$docker compose -f docker-compose.yml up -d 

#このとき、.dockervenvと.venvの２つのフォルダが作成される。


■STEP 2 コンテナが作成されたことを確認する

docker ps -a
#この時点では、statusは、exitedとなっている。


■■■ Section 2 PoetryによるPython環境のセットアップ■■■


コンテナの場所に移動する

$cd C:\Users\maete\Documents\fastAPI_folder7


次に、以下のコマンドを打つ。

$docker compose run --entrypoint "poetry init --name demo-app --dependency fastapi --dependency uvicorn[standard]" demo-app

#このときに、tomlファイルが作成される

$ docker-compose run \
  --entrypoint "poetry init \
    --name demo-app \
    --dependency fastapi \
    --dependency uvicorn[standard]" \
  demo-app


■■■ Section 3 FastAPIのインストール ■■■


$docker compose run --entrypoint "poetry install --no-root" demo-app

#この後、poetry.lockファイルが作成される
#この時点でも、Statusは、Exitedとなっている。
#この時点で、.dockervenvと.venvの２つのフォルダの中に、pythonパッケージが保存される。


■■■ Section 8 docker composeを実行する ■■■

#これまでに、材料フォルダの中に変更が加えられているため、それらを含めて、コンテナを更新する
#変更内容：pythonパッケージのインストール
#これを実行すると、Statusが、ExitedからUpに変更される。

$docker compose -f docker-compose.yml up -d


$docker compose -f C:\Users\maete\Documents\fastAPI_folder7\docker-compose.yml up -d




■■■ Section 10　mysqlクライアントのインストール ■■■


$cd C:\Users\maete\Documents\fastAPI_folder7
$docker compose exec demo-app poetry add sqlalchemy aiomysql

以下のようになっていることを確認する

これで、aiomysql, sqlalchemyがインストールされる。


#####################################################################

<pyproject.toml>　→ VScodeから確認する。fastAPI_folder5の直下にある

[tool.poetry.dependencies]
python = "^3.9"
fastapi = "^0.109.0"
uvicorn = {extras = ["standard"], version = "^0.26.0"}
sqlalchemy = "^2.0.25"
aiomysql = "^0.2.0"

######################################################################

#インストールにより、 pyproject.toml や poetry.lock も中身が変更されているのが確認できます


■■■ Section 8 docker composeを実行する ■■■

#これまでに、材料フォルダの中に変更が加えられているため、それらを含めて、コンテナを更新する
#変更内容：pythonパッケージのインストール
#これを実行すると、Statusが、ExitedからUpに変更される。


$docker compose -f docker-compose.yml up -d

$docker compose -f C:\Users\maete\Documents\fastAPI_folder7\docker-compose.yml up -d


■■■ Section 8 migrate_dbを実行する ■■■


次に、以下のpyプログラムを実行する

# api モジュールの migrate_db スクリプトを実行する
# 

$cd C:\Users\maete\Documents\fastAPI_folder7
$docker compose exec demo-app poetry run python -m api.migrate_db


以下が表示されれば、OKです。

##############################################################################
2024-01-29 02:10:57,040 INFO sqlalchemy.engine.Engine [no key 0.00011s] {}
2024-01-29 02:10:57,080 INFO sqlalchemy.engine.Engine
CREATE TABLE dones (
        id INTEGER NOT NULL,
        PRIMARY KEY (id),
        FOREIGN KEY(id) REFERENCES tasks (id)
)


2024-01-29 02:10:57,080 INFO sqlalchemy.engine.Engine [no key 0.00014s] {}
2024-01-29 02:10:57,101 INFO sqlalchemy.engine.Engine COMMIT
###############################################################################


■■■ Section 14　動作確認 ■■■


$cd C:\Users\maete\Documents\fastAPI_folder7
$docker compose exec db mysql demo

#dbコンテナの中のmysqlの中のdemoテーブルを参照する

###############################################################################
Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql>
###############################################################################

ログアウトするには、「Ctrl + d」のショートカットキーを打つ




■■■ Section 9　動作確認 ■■■

この状態で、ブラウザで以下のURLにアクセスする


http://localhost:8007/docs

次に、以下にアクセスする

http://localhost:8007/hello　

サーバーの立ち上がりを確認


次に、MySQLに接続する


$cd C:\Users\maete\Documents\fastAPI_folder6

プロジェクトディレクトリの中で以下を実行

$docker compose exec db mysql demo

以下の表示がでれば、OK

MySQLクライアントが実行され、DBに接続できているのが確認された。

####################################################################
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 8
Server version: 8.0.36 MySQL Community Server - GPL

Copyright (c) 2000, 2024, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.
#####################################################################

#以下も同様にできる。2-stepで、MySQLに入る

$docker compose exec db /bin/bash

これで、dbコンテナに入る

$mysql -u root

これで、MySQLに入る




（終わり）

