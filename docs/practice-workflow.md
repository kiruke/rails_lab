# rails_lab 練習ワークフロー

## 目的
Railsの機能を繰り返し実装し、理解を深めるための実験環境として利用する。
練習用ブランチを作成し、実装・検証・初期化を繰り返す。

## 1. 練習を開始する
mainブランチを最新にする。

```bash
git switch main
git pull origin main
```

練習用ブランチを作成する。

```bash
git switch -c practice/associations
```

## 2. 練習中の変更を保存する
必要に応じてコミットする。

```bash
git add .
git commit -m "practice: implement associations"
```

学習成果をmainに残したい場合はPRを作成する。
繰り返し練習するだけの場合は、PRを作成せずブランチを削除してよい。

## 3. 練習をやり直す
未コミットの変更がないことを確認する。

```bash
git status
```

mainに戻り、練習ブランチを削除する。

```bash
git switch main
git branch -D practice/associations
```

同じ名前で練習ブランチを再作成する。

```bash
git switch -c practice/associations
```

注意：ブランチを削除すると、そのブランチだけに存在するコミットを通常の履歴から参照できなくなる。保存したい変更は事前に退避する。

## 4. データベースを初期化する
Gitの操作ではPostgreSQLのデータは元に戻らない。

### レコードだけ削除する
テーブル構造を維持してデータを削除する。

```bash
docker compose exec web bin/rails db:truncate_all
```

### データベースを再作成する
現在のRailsプロジェクトのスキーマに合わせてDBを作り直す。

```bash
docker compose exec web bin/rails db:drop db:prepare
```

### マイグレーションを最初から実行する
マイグレーションを練習したい場合は、開発DBを削除してから作成・マイグレーションを実行する。

```bash
docker compose exec web bin/rails db:drop
docker compose exec web bin/rails db:create db:migrate
```

注意：これらの操作ではデータが失われる。rails_lab専用の開発DBであることを確認してから実行する。

## 5. 過去の状態から練習する
User/Postモデル導入前の環境から練習する例。

```bash
git switch main
git switch -c practice/models-from-scratch 544e477
```

これにより、指定したコミット時点のコードから練習を開始できる。

ただし、Gitのブランチを切り替えてもDBは自動的に戻らない。モデルやマイグレーションを最初から練習する場合は、DBの状態も合わせて初期化する。

## 6. 運用ルール
- mainは基本環境と保存したい学習成果を管理する。
- practice/*は繰り返し練習するための一時ブランチとして使用する。
- 完成した機能を残す場合はPRを作成する。
- 同じ課題を繰り返す場合は、開始地点のコミットを揃える。
- DBの初期化とGitの操作は別々に考える。
- 本番環境や他のプロジェクトのデータベースには初期化コマンドを実行しない。