# Gitに関して

## Gitとは

バージョン管理システムの1つ  
バージョン管理システムを使うことで

1. 変更履歴を辿れる

2. 差分を確認できる

3. 最新のファイルがわかる

4. 前のバージョンに簡単に戻せる

5. 他人の作業場所がわかる

6. 他人の作業を簡単に結合できる

などといったメリットがある.

## Gitの用語解説

- リポジトリ : プロジェクトの全てのファイル(変更履歴や設定ファイルも全て)管理している

- リモートリポ : リモートサーバーに置かれたリポジトリ

- ローカルリポ : 手元のパソコンのリポジトリ

- clone : リモートリポをローカルリポに複製すること

- fork : 他人のリポジトリを自分のアカウントのリモートリポジトリにコピーすること

- コミット : ファイルの状態をセーブすること

- コミットポイント : セーブするポイント

- Working directory : 自分が作業しているディレクトリ

- Staging area : コミットする前段階, Staging area で変更をひとまとめにしコミットする

- ブランチ : メインの開発の流れとは別に流れを作れこれをブランチという

- マージ : 分岐したブランチをメインのブランチに統合させること

## Gitの準備

1. gitのインストール

バージョンが確認できればインストール済み. なければ[こちら](https://git-scm.com/downloads)からダウンロードする.

```bash
> git --version
git version 2.28.0
```

2. githubへのssh接続の設定

```bash
> ssh-keygen -t ed25519 -b 4096 -C "schnelldickheiter@gmail.com"
# -t 暗号化方式を指定
# -b 暗号化強度を指定
# -C コメントを設定
Generating public/private ed25519 key pair.
Enter file in which to save the key (/Users/schnell/.ssh/id_ed25519):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /Users/schnell/.ssh/id_ed25519.
Your public key has been saved in /Users/schnell/.ssh/id_ed25519.pub.

# ssh-agentが起動しているか確認
> eval "$(ssh-agent -s)"
Agent pid 54751

> vi ~/.ssh/config
```

次の内容を追記

```
Host GitHub
        AddKeysToAgent yes
        UseKeychain yes
        IdentityFile ~/.ssh/id_ed25519
```

秘密鍵をssh-agentに追加

```bash
> ssh-add -K ~/.ssh/id_ed25519
Identity added: /Users/schnell/.ssh/id_ed25519 (schnelldickheiter@gmail.com)
```

公開鍵をGitHubに登録し接続できるか確認する

```bash
> ssh -T git@github.com
Hi schnell3526! You've successfully authenticated, but GitHub does not provide shell access.
```

## Gitの基本ワークフロー

- gitコマンド

```bash
> git <command>
```

の形で記述する. 長いのでalias登録しとくといいかも

- gitの設定ファイル

```bash
~/.gitconfig
```

にある.　リポジトリ毎の設定ファイルは各リポジトリに存在している.

- gitの設定ファイル更新

```bash
> git config --global <attribute> <value>
```

--globalをつけない場合、実行したリポジトリ独自の仕様となる。


- Gitのユーザー設定

```bash
> git config --global user.name "<username>"
> git config --global user.email "<email>"
```

- リモートリポからローカルリポにclone

```bash
> git clone <remote_repo_url>
```

- ローカルリポに紐付いているリモートリポのURLを確認

```bash
> git remote -v
```

- ブランチ

  - ブランチ一覧を表示
  
  ```bash
  > git branch
  ```
  
  - 新しいブランチを切る

  ```bash
  > git branch <branch_name>
  ```
  
  作業する内容がわかりやすい名前にする
  長すぎないようにする
  スネークケース

  - 作業するブランチを切り替える

  ```bash
  > git checkout <branch-name>
  ```

  -b オプションをつければ作成して切り替えることも可能

- 作業内容をステージする

  Gitは作業履歴をコミット単位で管理し変更をコミットするには一度Staging areaにあげる必要がある.
  gitはファイルの変更を3つのステージに分けて管理する.

  ![a](https://github.com/schnell3526/memo/blob/picture/git_github/git01.png?raw=true)

  - 作業内容をstaging area に追加する

    ```bash
    # 特定のファイルを追加する場合
    > git add <filename>

    # カレントディレクトリ以下全てのファイルを追加する場合
    > git add .
    ```

- Staging areaの作業内容をコミットする

  ```bash
  > git commit -m "<commit message>"
  ```

  コミット時には必ずコミットメッセージをつける
  基本的には一文で変更内容がわかるようにする
  書き方はチームでルールが決められていることも少なくない
  コミット後にはコミットポイントが作られる

  - コミット履歴をみる

    ```bash
    > git log
    ```

    