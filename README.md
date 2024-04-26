# KAKO's websiteを再現するための手順書
## リポジトリの内容物
   + 手順書（README.md）
   + index.htmlファイル
   + css > style.css
   + js > script.js
   + images > 使用した7枚の写真ファイル
   <br>

## 前提
   + Git/GitHubはインストール、登録済み
   + Dockerはインストール済み
   + dockerデーモンが立ち上がっている
   + （必要なら）いつでもリモート環境にアクセスできる

## 手順
   1. GitHubの[kako/ITkisokenshu](https://github.com/kakoyukiko/ITkisokenshu)リポジトリを開く。<br>

   2. リポジトリをフォークして、自分アカウントのリポジトリとしてコピーする。これを「A」と呼ぶ。<br>

   3. 「A」をcloneするためのURLをコピーする <br>
      `git@github.com:username/ITkisokenshu.git` <br>
      * 上記の場合、SSHkey認証のクローンである。自身の認証方法に応じてHTTPS, SSH, GitHub CLIのいずれかのURLを選ぶ。<br>
      必要なら[この記事](https://docs.github.com/ja/repositories/creating-and-managing-repositories/cloning-a-repository) を参考に 
      <br>

   4. 「A」を `git clone git@github.com:username/ITkisokenshu.git`でローカル上の自身の作業したいディレクトリにcloneする <br>
      
   5. docker imageにnginxがあることを `docker images` で確認する <br>
      なかったらpullする `docker pull nginx` <br>
   
   6. Dockerを立てている環境（ローカル・リモート）にcloneしたリポジトリがあるか確認する。<br>
      もしなかった場合、Dockerを立てている環境にコピーする <br>
      以下のコードは、ローカルからリモートにコピーするときのコマンド
      ```
      scp -i ~/path/to/ssh/key/~.pem -r /path/to/repository username@00.000.000.00:/path/to/where/you/wanna/put/
      ```
      <br>

      + `-r`でディレクトリごとコピーできる
      + `/path/to/repository` 今のリポジトリまでのパス
      + @の後ろはグローバルIPアドレス
      + `/path/to/where/you/wanna/put/`はコピー先のパス
      <br>

   7. dockerでnginxコンテナを作成&作成したHTMLがwebサーバ（nginx）で返せるようにする
       ```
       docker run --name containername -p 00000:80 -v /path/to/index.html:/usr/share/nginx/html:ro -d nginx
       ```

       ### オプションの意味を解説

       |コマンドラインでの表示|意味|
       |-|-|
       |-p|HTMLファイルを置いているポート番号(ローカルなら8080が多い?)：Docker上nginxサーバーのデフォルトポート番号(80)|
       |`/path/to/index.html`|HTMLファイルがあるディレクトリまでのパス|
       |`/usr/share/nginx/html`|nginxのデフォルトパス|
       |ro|read only|
       |-d|バックグラウンドでの起動|
       <br>
   
   8. webブラウザで`http://global_IP_address:port_number/`にアクセスし、作成したHTMLファイルが表示されるのかを確認

   + もしエラーが起こっているなら以下のことを確認する
       + ファイルの他ユーザーの権限は読み取りOKになっているか？
       + ファイルが入っているディレクトリの権限は実行OKになっているか？<br>
         →権限については[この記事](https://qiita.com/shisama/items/5f4c4fa768642aad9e06)を参考に。
       + Dockerコンテナをリモート環境に立てたならリモート環境に、ローカルに立てたならローカルにcloneしてきたリポジトリは置かれているか？<br>
         →手順6. を振り返る
       
      
