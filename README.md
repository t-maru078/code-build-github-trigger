# code-build-github-trigger

CodeBuild と GitHub を Webhook で連携し、GitHub 側で発生する Event をトリガーにして CodeBuild を起動するサンプルです。
CloudFormation を使って AWS リソースをデプロイできます。

`ci/ci-resources.yml` ファイルにのテンプレート中の `UnitTestProject -> Triggers -> FilterGroups` の内容を書き換えることで下記いずれかの event trigger を設定することができます。必要に応じてコメントアウトするなどして内容を書き換えてください。

- Pull Request イベントトリガー
- main, develop ブランチ以外への Push イベントトリガー


## AWS へのデプロイ

事前に下記の作業 3 つが必要です。

1. AWS CLI のインストール。詳細は下記の公式ドキュメント参照。

    https://docs.aws.amazon.com/ja_jp/cli/latest/userguide/getting-started-install.html

1. GitHub にて Personal access token を取得する。詳細は下記の公式ドキュメント参照。

    https://docs.aws.amazon.com/ja_jp/codebuild/latest/userguide/access-tokens.html

1. 上記で取得した token をデプロイコマンド実行前に AWS の Management Console で設定する

    1. CodeBuild の console を開き `Create build project` ボタンを押す
    1. Source provider のドロップダウンから `GitHub` を選択する
    1. `Connect with a GitHub personal access token` を選択し、`GitHub personal access token` の欄に GitHub から取得した Personal access token を入力し `Save token` ボタンを押す
    1. console 最下部の `cancel` ボタンを押して `Create build project` のウィザードを閉じる


上記の手順が完了後、この README と同じディレクトリ階層で下記コマンドを実行することで CodeBuild 等の必要な環境が AWS 環境にデプロイされます。

下記コマンドにおいて `<　>` で囲まれた値に関しては利用する際に適切な値を設定してください。

```
aws cloudformation deploy --template-file ./ci/ci-resources.yml \
--stack-name <CloudFormation の stack name> \
--capabilities CAPABILITY_IAM \
--parameter-overrides SourceCodeRepositoryURL=<GitHub リポジトリの URL>
```
