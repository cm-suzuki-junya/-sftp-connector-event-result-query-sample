## 概要

SFTPコネクタによるSFTPサーバからS3に対するファイル転送を行い、すべてのファイルが転送された後にその結果の集計をSNSで通知するサンプルとなります。

以下ページにて紹介しておりますのでこちらもご参照ください。  
https://dev.classmethod.jp/articles/auto-athena-report-afer-sftp-connector-transfer/


## 含まれていないリソースについて

SFTPコネクタとSFTPサーバを構成するリソースは含まれないため  
別途作成しSFTPコネクタ側からSFTPサーバ側に接続できるようセットアップしてください。

## ビルド

```bash
sam build
```

## デプロイ

`ConnectorId`には事前に作成したSFTPコネクタのID、`ReportReceiveAddress`には通知先のメールアドレスを指定してください。

```bash
sam deploy --parameter-overrides ConnectorId=c-xxxxx ReportReceiveAddress=foo@example.com
```

リソース作成後Amazon SNSの認証確認のためのメールが送られてくるため確認してください。

## 実行

`TransferExecuter`のステートマシンに以下のように`FilePath`に対し対象のファイルパスを配列で指定し実行します。

```json
{
    "FilePath": ["/tmp/foo.txt", "/tmp/bar.txt"]
}
```

なお本サンプル作成時点で10個以上指定するとクォータの関係で実行に失敗する可能性があります(特にこの辺りの制御はしていません)。