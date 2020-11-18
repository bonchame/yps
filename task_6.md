## yps1 task#6

#### 前回はこちらです
[#yps1 task#5](https://github.com/yotaro-ok/yps/blob/master/task_5.md)
<br>
<br>

***

#### 注意事項

~~Laravel7でのプロジェクト作成時に "7.25.0" を必ず指定してください
<br>
フォロワーさんから "7.*" 指定だとおかしな挙動をする場合があると報告がありました~~

Laravelでのプロジェクト作成時は、”7.*”を指定してください
👇
```
composer create-project "laravel/laravel=7.*" プロジェクト名
```

***

#### お手軽簡単WEBアプリケーション

Laravelでメール認証月のToDoアプリを作ります

初学者がある程度、学習してくると次に何をやればいいのか？と悩むことがあるそうです
<br>
そーゆーときはブログ記事などを参考にしてみましょう
<br>
でも、それだけだとコピペで済んでしまうので複数記事をドッキングしたりリミックスして
<br>
カスタマイズすると面白いですyo!

GitHubもちゃんと使ってくださいね

[git-flow 図解](https://qiita.com/ohnaka0410/items/7c7fa20710dfd72b7d7a)

***

#### 課題の題材

２つのブログ記事を利用します

1.[Laravel 6.x laravel/uiを利用してbootstrap 4を適用する](https://blog.hrendoh.com/laravel-6-setup-bootstrap4-with-laravel-ui/)
<br>
<br>
2.[【Laravel】ユーザ登録時にメール認証する(v5.7以上)](https://qiita.com/nekyo/items/03e50b4d0dd6f09287d6)

***

#### プロジェクトの作成

```
// まず新規プロジェクトを作ります

cd /var/www/html
composer create-project --prefer-dist "laravel/laravel=7.25.0" myapp2 // 👈ここ注意してね
php artisan -V // でバージョンを確認できます　※バージョン指定が効かなかった場合は、そのまま続行してください
```
```
// 途中で何か聞かれたら全部　Yes　でOKです
cd myapp2
composer require laravel/ui "2.*"
npm install
php artisan ui bootstrap --auth
npm install cross-env
npm install
npm run dev
```
```
// npm run dev　でエラーが出たらこれ👇やってください

rm ./package-lock.json
rm -rf ./node_modules/
npm install
```
```
// Nginxの設定をいじります
sudo vi /etc/nginx/conf.d/default.conf

/var/www/html/yps/public;

// 👇ypsをmyapp2に全部置換してください

/var/www/html/myapp2/public;

// で、再起動
sudo systemctl restart nginx && sudo systemctl restart php-fpm
```
```
// 前回入れた phpmyadmin も使えるようにしておきましょう

cd ../yps/public/
unlink pma
cd -
sudo ln -s /usr/share/pma/ /var/www/html/myapp2/public/pma

// pmaがphpmyadminとかの人はディレクトリ名変えてね
```
```
// キャッシュクリア
php artisan cache:clear &&php artisan config:clear && php artisan route:clear &&php artisan view:clear

composer clear-cache && composer dump-autoload --optimize

// オーナーとディレクトリ権限変更
sudo chown -R centos:nginx /var/www/
sudo chmod -R 777 storage/ bootstrap/cache/
```

右上にlogin、registerがあることを確認してください

![EhE0aGzU8AAOap0](https://user-images.githubusercontent.com/63440984/93662154-d2fd2580-fa98-11ea-9077-95cfe39b0018.jpeg)

***

#### タスクアプリを作成

```
php artisan make:model Task -m

// で表示されたファイルを開いてブログのとおりコピペしてください

ls -la database/migrations/

// で作成されたファイルが見つかるはずです
```
```
// データベースを作成します

mysql -u root -p

create database myapp2db;

exit
```
```
// .envを編集します
vi .env

DB_DATABASE=myapp2db                                                                                
DB_USERNAME=root
DB_PASSWORD="パスワード"
```
```
// マイグレートします
php artisan migrate

// テーブルが作成されているか確認します
mysql -u root -p -D myapp2db
show tables;

// で👇と同じならOKです

+--------------------+
| Tables_in_myapp2db |
+--------------------+
| failed_jobs        |
| migrations         |
| password_resets    |
| tasks              |
| users              |
+--------------------+
```

[サンプルソースのGitHubリポジトリ](https://github.com/hrendoh/laravel-ui-bootstrap-tasks)

***

#### Gmailでのメール認証

先のブログを読んで分からなかったらこっちのブログを参考にしてください

[【Laravel6】gmailを使ったメール送信の設定方法](https://programming.sincoston.com/laravel6-gmail-send-mail-config/)
<br>
<br>
[GmailでSMTPサーバーを使う](https://saba.omnioo.com/note/5755/gmail%E3%81%A7smtp%E3%82%B5%E3%83%BC%E3%83%90%E3%83%BC%E3%82%92%E4%BD%BF%E3%81%86/)

****

#### まとめ

あとは、余裕ですよね  
ブログやGitHubに書いてあるソースコードを読んで模写（実装）してください  
出来た人は、#yps1 #tsk6　のハッシュタグ付けてスクショ晒してください  

👇ここ多分、ハマると思うー　がんばえー

ちゃんとログインしてメール認証もしてね
TOP画面は何でもいいですyo!

***

<br>
<br>
#### TODO: 資料を纏める

[元ネタツイート](https://twitter.com/yotaro__ok/status/1301869326383812608)
<br>
<br>

#### 参考

[とりあえずこんな感じです](https://github.com/yotaro-ok/myapp2/tree/develop)
<br>

👇聖典となりつつある[miyupaca](https://twitter.com/miyupacaaa)さんのブログ
<br>
[2020-09-05 yps学習記録その6](https://paca-gatsby.netlify.app/2020-09-05/)
<br>
[Laravel-Todoアプリに一週間機能追加チャレンジ](https://paca-gatsby.netlify.app/laravel-todoapp-studylog/)
