\01_other\gitコマンド.txt" 開く


この通りにやってみましょう
Git Bash 起動

cd /c/mamp/htdocs/sample  ← あなたがプロジェクトを置いてるフォルダ

git init 	← git管理するための初期化コマンド
git config --global user.name ichiro 	←自分のアカウント名
git config --global user.email ichiro@efg.com ← 登録時のメールアドレス

https://github.com/ にログインして新規リポジトリを作成

作成したページに書かれてるこの一行↓のみコピーして実行
(sshの方)
git remote add origin git@github.com:アカウント名/リポジトリ名.git

git add		← ステージング
git commit -m '適当なコメント文'  ← 変更の確定
git push origin master

100% とか出てたら完了

別の作業をする場合
git checkout -b toppage   ← 別の作業用ブランチ作成
作業が終わったら add して commitして push です｡


__________________________________________________

[C:\Users\秋田花子\.ssh\config] の中身(なければ作る)
Host github github.com
HostName github.com
IdentityFile ~/.ssh/github_rsa
User git 


[C:\Users\秋田花子\.gitconfig] の中身(なければ作る)
[user]
	email = abcd@efg.com
	name = ichiro

[core]
	autoCRLF = false




鍵の作成
ssh-keygen -t rsa -b 4096 -C "とうろくした@メールアドレス" ↓Enter
Enter file in which to save the key (/c/Users/ginzo/.ssh/id_rsa): github_rsa ← ファイル名を入れる
Enter passphrase (empty for no passphrase): ↓そのままEnter
Enter same passphrase again: ↓このままEnter



以上


__________________________________________________
ローカルのサーバーに sshでログインする



鍵認証できるようにする
	鍵の置き場所へ移動する
	$cd ~ 

.sshディレクトリをつくる
	$ mkdir .ssh 

所有者(自分)以外見れないようにする
	$ sudo chmod 700 .ssh
	$ ll -a 
	   drwx------ ←こうなればOK   .ssh

鍵の置き場所へ移動
	$ cd .ssh

ローカルの開発サーバーで鍵をつくる
	$ ssh-keygen -f github_rsa  ←この名前の鍵ができる
    (empty for no passphrase): そのままEnter
    	※パスフレーズとは鍵を使うためのパスワードのこと(いらない)

 $ ll  ← 何が出来たかを見る
 -rw-------  17:26 github_rsa  秘密鍵
 -rw-r--r--  17:26 github_rsa.pub 公開鍵


config .ssh内に(設定)ファイルの作成
 $ vi config
 		これが中身 ↓ 4行コピーする
			Host github github.com
			HostName github.com
			IdentityFile ~/.ssh/github_rsa
			User git 
 
 (i で編集モードにして)貼りつけたら保存終了(Esc → :wq)

パーミッションを600にする
 $ sudo chmod 600 config
 $ sudo chmod 600 github_rsa


.gitconfig をつくって 600にする
  自分のhomeディレクトリにつくるのでひとつ上の階層に移動する
  $ cd ../

  vi .gitconfig 
  	これが中身 ↓
   [user]
        name = 自分のgitアカウント
        email = 登録したときのメアド

   [url "github:"]
      InsteadOf = https://github.com/
      InsteadOf = git@github.com:
     
貼ったら保存終了


github.com(webサイト)にログインし、
 マイページ(右上のユーザーアイコン) クリック 
 		→ settings → (左メニュー)SSH and GPG keysに入ります。

title: github_rsa.pub

 vi .ssh/github_rsa.pub	← 画面に開いてまるっとコピーする

作ってしまった場合は Deleteしてから
New SSH keyをクリックし、さきほどコピーした公開鍵をペーストします

接続確認のコマンド
	$ ssh -T git@github.com 
		(yes/no)
 		 Hi アカウント名 You'vesuccessfully 
  		↑ こうでたら もうつながるので設定は完了です


次は用語と 作業フローを覚えましょう

OneDriveのPDFファイル
	\01_other\Linux\Git_githubのはじめかた.pdf" 
		27P	ステージングだけ読んでください｡


プロジェクトをgit管理にする方法
初期化したいディレクトリに移動
	$ cd nuts-shop  ← 例

git管理にするためinit(初期化)する
 $ git init
 		(ディレクトリ内に .gitディレクトリができる)

git アカウントと紐付け
	$ git config --global user.name あなたのアカウント
	$ git config --global user.email あなたのメアド

githubにブラウザでログイン
で新規レポジトリを作成( リポジトリの名前は ディレクトリの名前にする)
	リポジトリのUR(ssh:)をコピーする

ローカルにリモートリポジトリの情報を追加
	$ git remote add origin ここペーストして実行する

# 除外ディレクトリ → gitへpushuしない ファイルが有ればやる
 .gitignore の書き方。ファイル/ディレクトリの除外

ステージする(コミット対象のファイルを選択)
	$ git add .  ← ピリオド(すべての意味)

# ファイルは消さずに、追跡だけ除外したいファイルが有ればやる
 $ git rm --cached ファイル名 

コミットする
	$ git commit -m 'inicial commit'

リモートへプッシュ
	$ git push origin master


ブラウザでgithubを見る
	作ったリポジトリにファイルが上がってるはず

==============とりあえずここまで==============================

Windowsの場合
	C:\Users\ginzo\
		.gitconfig
		.ssh\
				config
				github_rsa

	C:xampp
			\-htdocs (DocumntRootのこと)

			URL http://localhost ↑このフォルダが開く


本番サーバー
	3つのファイルをコピーするだけ



別のパソコンでリポジトリをクローンできます
	ここからは別のパソコンでの操作

	$ git clone git@github.com:コピペ

	Windowsだとしたら Users/自分/.ssh	
		に鍵とconfigファイルをコピー

	cloneで出来たフォルダへ移動して initから実行
	この次は 同じ
	 コミット→プッシュ


  教室の環境から続きをやるには
  	$ git pull origin master  ← 書き換わったコードを持ってくる

  	これの繰り返し




適当に作業用のfeatureブランチを作る	
	checkoutはブランチの切り替え, -b スイッチはビルド
	$ git checkout -b add-mail

vsCodeでjsとかなにか変更する

ステージする(コミットのリストに加える)
	編集していないファイルが意図せず書き換わった場合のリスク
	$ git add 編集したファイル名1 編集したファイル名2

コミットする(変更の確定)
	$ git commit -m 'てきとうに変更'

リモートへプッシュ(uploadのこと)
	$ git push origin add-mail

ブラウザでgithubを見る → ブランチが追加されてコードが変わってる
 
masterブランチに切り替える
	$ git checkout master

今いるブランチを確認
	$ git branch	

 ブラウザでファイルを見る(こっちは手を付けていない)
	
add-mail ブランチをmasterにマージ(結合)
	$ git merge add-mail

ファイルを見る → 変わってるはず

masterをリモート(web上のgithub)にプッシュ
	$ git push origin master

一連の作業が終わり

	本番サイトで masterをpullする


===================================================
	masterからブランチを作る
	ブランチからブランチを作るなら､しない
マスターブランチにする
git checkout master
git checkout -b develop 作成するブランチ名

git branch -a 		ブランチの一覧を見る



ブランチの削除
  git branch -d [ブランチ名]

 マージしてないブランチの削除
    git branch -D [ブランチ名]
    
リモートのブランチ削除  
  git push --delete origin [ブランチ名]


ブランチをリモートに登録
git push -u origin [作成したブランチ名]
リモートブランチを取得する方法 (良い例とされている)
$ git fetch
$ git checkout ブランチ名
