# 加古のwebページを再現するための手順書
## 前提
   + Git/GitHubはインストール、登録済み
   + Dockerはインストール済み

## 手順
   1. GitHubの[kako/ITkisokenshu](https://github.com/kakoyukiko/ITkisokenshu)リポジトリを開く。<br>

   2. リポジトリをフォークして、自分アカウントのリポジトリにコピーする。自分アカウントのリポジトリにコピーしたプログラムを「A」と呼ぶ。<br>

   3. 「A」を `git clone git@github.com:username/ITkisokenshu.git` で自身の作業したい場所にcloneする <br>
      * 上記の場合、SSHkey認証のクローンなので自身の認証方法に応じてHTTPS, SSH, GitHub HTTPSのいずれかのURLを選ぶ。<br>

   4. dockerを起動する <br>

   5. docker imageにnginxがあることを `docker images` 確認する <br>
      なかったらpullする `docker pull nginx` <br>

   6. dockerでnginxコンテナを作成
       ```
       docker run --name containername -p 00000:80 -v /path/to/index.html:/usr/share/nginx/html:ro -d nginx
       ```
       <br>

       |コマンド|意味|
       |-|-|
       |-p|HTMLファイルを置いているポート番号(ローカルなら8080が多い?)：Docker上nginxサーバーのデフォルトポート番号(80)|
       |`/path/to/index.html`|HTMLファイルがあるディレクトリまでのパス|
       |`/usr/share/nginx/html`|nginxのデフォルトパス|
       |ro|read only|
       |-d|バックグラウンドでの起動|
       <br>

明日
+ HTMLファイル
+ CSSファイル
+ images
をGitHubに加える