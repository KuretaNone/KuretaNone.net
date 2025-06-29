---
id: GitHub Actionsでデプロイを自動化する
aliases:
  - GitHub Actionsでデプロイを自動化する
tags: []
created: "2025-06-25"
updated: "2025-06-25"
---
# GitHub Actionsでデプロイを自動化する

[[GitHub Actions]]を使って、サイトのデプロイを自動化する方法について説明します。  
この辺のネタって死ぬほどこすられてるから、あまり需要はないかもしれませんが、いいんです。
自分で調べて、やってみたいから、やる。そういうものです。

## GitHub Actionsって何？

[[CI]]/[[CD]]のプラットフォームで、[[リポジトリ]]に対して、ちょっとコードを管理、提供していくうえで便利な機能が集まっているという印象です。

## 自分のサイトの更新について

自分のサイトは[[MkDocs]]を使って作成しています。  
概要自体は[[MkDocsを使ってサイトを作る]]に書いています。  
ここでは、最終成果物である[[Markdown]]を`mkdocd build` コマンドを叩いて生成した`site`ディレクトリを
[[WinScp]]を使って[[サーバー]]と接続して、`public_html`ディレクトリにアップロードしています。

さすがに毎回手動でビルドして、アップロードするのが大変です。  
なにより、[[Material for MkDocs]]には[[GitHub Actions]]のテンプレートが用意されているので、これを使わない手はありません。

早速設定をしていきましょう。

## GitHub Actionsの設定

`.github/workflows/deploy.yml`というファイルを作成します。 
以下の内容を記述します。
```yaml
name : Deploy to Server
on:
  push:
    branches:
      - main
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: actions/setup-python@v2
        with:
          python-version: '3.10.8'
      - name: Install requirements
        run: 
          python -m pip install --upgrade pip
          pip install mkdocs-material
      - name: Build MkDocs
        run: mkdocs build --verbose --clean
      - name: Deploy to GitHub Pages
        run: mkdocs gh-deploy --force
```


以上の設定をすると、`main`ブランチにプッシュしたときに自動的にMkDocsのビルドとKuretaNone.netにデプロイが行われます。



## 参考リンク

- [GitHub Actions を理解する](https://docs.github.com/ja/actions/about-github-actions/understanding-github-actions)
- [Publushing your site](https://squidfunk.github.io/mkdocs-material/publishing-your-site/)
