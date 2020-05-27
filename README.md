# README

## sample

```
awslocal 使うとデフォルトでlocalstackのdynamodbにつながるらしい
brew install awscli-local
```

を入れてる前提

### dynamodbのテーブル一覧

```
# awslocal
awslocal dynamodb list-tables

# aws コマンドで試す場合は --endpoint-url を明示する(ポート4566に)
aws dynamodb delete-table --table-name SampleTable --endpoint-url http://localhost:4566
```

### dynamodbのテーブル操作

サンプル
```
# テーブル一覧
awslocal dynamodb list-tables

# テーブル作成
# ユニークキーとパーティションキー以外のカラムは実行時に定義すればいい
awslocal dynamodb create-table --table-name SampleTable --attribute-definitions \
  AttributeName=column1,AttributeType=S \
  AttributeName=column2,AttributeType=S \
  --key-schema \
    AttributeName=column1,KeyType=HASH \
    AttributeName=column2,KeyType=RANGE \
  --provisioned-throughput \
    ReadCapacityUnits=5,WriteCapacityUnits=5

# データ追加(数値(N) でも値はjsonとしては文字列で入れる)
awslocal dynamodb put-item --table-name SampleTable \
  --item '{ "column1": { "S": "column1-001"}, "column2": { "S": "00000001" }, "c3": { "S": "v3-001"}, "c4": { "N" : "1"}}'

awslocal dynamodb put-item --table-name SampleTable \
  --item '{ "column1": { "S": "column1-002"}, "column2": { "S": "00000002" }, "c3": { "S": "v3-002"}, "c4": { "N" : "2"}}'

awslocal dynamodb put-item --table-name SampleTable \
  --item '{ "column1": { "S": "column1-003"}, "column2": { "S": "00000003" }, "c3": { "S": "v3-003"}, "c4": { "N" : "3"}, "c5": { "S" : "extra_data3"}}'

# データ取得
awslocal dynamodb scan --table-name SampleTable

# テーブル削除
awslocal dynamodb delete-table --table-name SampleTable

```
