# go module テスト

## go modの中身
github.comから始まるリポジトリ名にすること。この場合github.com/go-mod-testね。  
go.modで設定したパッケージ名以下のディレクトリならちゃんとgo mod edit -replaceしなくても読んでくれる。   
このリポジトリならmainもsub1もsub2もトップのgo.modで認識される。  

## これは失敗
go build(go install)はmain関数がある場所じゃなくてトップで実行すること。  
goファイルは無いからと言ってmainディレクトリ配下で実行しちゃ駄目。  
そうするとgithub.com/ogaty/go-mod-test/main/sub1とかを探しに行ってしまう。  
main配下でgo mod initしちゃ駄目。  
そうするとgithub.com/ogaty/go-mod-test/mainを探しに行ってしまう。
```
can't load package: package github.com/ogaty/go-mod-test/main: unknown import path "github.com/ogaty/go-mod-test/main": cannot find module providing package github.com/ogaty/go-mod-test/main
```

もちろんパブリックリポジトリならダウンロードできるから動いてしまうのだが。  
プライベートリポジトリはこのやり方じゃだめ。

## これが正解
リポジトリのトップで
```
GO111MODULE=on go build -o test ./main/
```
