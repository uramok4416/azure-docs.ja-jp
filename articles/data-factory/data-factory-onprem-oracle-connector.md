---
title: "Data Factory を使用して Oracle との間でデータを移動する | Microsoft Docs"
description: "Azure Data Factory を使用してオンプレミスの Oracle データベースとの間でデータを移動する方法を説明します。"
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 3c20aa95-a8a1-4aae-9180-a6a16d64a109
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/17/2017
ms.author: jingwang
translationtype: Human Translation
ms.sourcegitcommit: bb1ca3189e6c39b46eaa5151bf0c74dbf4a35228
ms.openlocfilehash: a27ec9e1ebfde3493e41c493b85c0dc7f0ada2a0
ms.lasthandoff: 03/18/2017


---
# <a name="move-data-tofrom-on-premises-oracle-using-azure-data-factory"></a>Azure Data Factory を使用してオンプレミスの Oracle との間でデータを移動する
この記事では、データ ファクトリのコピー アクティビティを使用して Oracle と他のデータ ストアとの間でデータを移動する方法について説明します。 この記事は、「 [データ移動アクティビティ](data-factory-data-movement-activities.md) 」という記事に基づき、コピー アクティビティによるデータ移動の一般概要とサポートされるデータ ストアの組み合わせについて紹介しています。

Data Factory は、Data Management Gateway を使用したオンプレミスの Oracle ソースへの接続をサポートします。 [Data Management Gateway](data-factory-data-management-gateway.md) の詳細については、「Data Management Gateway」をご覧ください。また、データを移動するデータ パイプラインにゲートウェイをセットアップするための詳しい手順については、[オンプレミスからクラウドへのデータの移動](data-factory-move-data-between-onprem-and-cloud.md)に関する記事をご覧ください。

> [!NOTE]
> Oracle が Azure IaaS VM でホストされている場合でも、ゲートウェイは必須です。 ゲートウェイはデータ ストアと同じ IaaS VM にインストールできるほか、ゲートウェイがデータベースに接続できれば別の VM にインストールしてもかまいません。
>

## <a name="supported-versions-and-installation"></a>サポートされているバージョンとインストール
Oracle コネクタでは、2 つのバージョンのドライバーがサポートされます。

- **Microsoft Driver for Oracle** は、バージョン 2.7 以降の Data Management Gateway にバンドルされています。 このドライバーを使用することを**お勧め**します。 ゲートウェイだけをインストールすれば Oracle に接続することができ、コピーのパフォーマンスが向上することも実感できます。 Oracle Database バージョン 10g リリース 2 以降がサポートされています。

    > [!NOTE]
    > 現在 Microsoft Driver for Oracle でサポートされているのは、Oracle からのデータ コピーだけです。Oracle への書き込みはサポートされていません。 また、Data Managemetn Gateway の [診断] タブのテスト接続機能では、このドライバーはサポートされていません。 代わりに、コピー ウィザードを使用して接続を確認できます。
    >

- **Oracle Data Provider for .NET:** このコンポーネントは、Data Management Gateway バージョン 2.7 以降に含まれているため、別途インストールする必要はありません。 2.7 より前のバージョンのゲートウェイを使用する場合は、最新バージョンのゲートウェイを[こちら](https://www.microsoft.com/download/details.aspx?id=39717)からインストールすることをお勧めします。 Data Management Gateway Configuration Manager のヘルプ ページで、ゲートウェイのバージョンを確認できます ("Data Management Gateway" で検索)。

## <a name="copy-data-wizard"></a>データのコピー ウィザード
Oracle データベースから、サポートされているシンク データ ストアにデータをコピーするパイプラインを作成する最も簡単な方法は、データのコピー ウィザードを使用することです。 データのコピー ウィザードを使用してパイプラインを作成する簡単な手順については、「 [チュートリアル: コピー ウィザードを使用してパイプラインを作成する](data-factory-copy-data-wizard-tutorial.md) 」をご覧ください。

次の例は、[Azure Portal](data-factory-copy-activity-tutorial-using-azure-portal.md)、[Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)、または [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md) を使用してパイプラインを作成する際に使用できるサンプルの JSON 定義です。 これらの例は、Oracle データベースと Azure BLOB ストレージの間でデータをコピーする方法を示しています。 ただし、Azure Data Factory のコピー アクティビティを使用して、 [こちら](data-factory-data-movement-activities.md#supported-data-stores-and-formats) に記載されているシンクのいずれかにデータをコピーすることができます。   

## <a name="sample-copy-data-from-oracle-to-azure-blob"></a>サンプル: Oracle から Azure BLOB にデータをコピーする
このサンプルは、オンプレミスの Oracle データベースから Azure BLOB ストレージにデータをコピーする方法を示します。 Azure Data Factory のコピー アクティビティを使用して、 **こちら** に記載されているシンクのいずれかにデータを [直接](data-factory-data-movement-activities.md#supported-data-stores-and-formats) コピーすることもできます。  

このサンプルでは、次の Data Factory のエンティティがあります。

1. [OnPremisesOracle](data-factory-onprem-oracle-connector.md#oracle-linked-service-properties)型のリンクされたサービス。
2. [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service)型のリンクされたサービス。
3. [OracleTable](data-factory-onprem-oracle-connector.md#oracle-dataset-type-properties) 型の入力[データセット](data-factory-create-datasets.md)。
4. [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties) 型の出力[データセット](data-factory-create-datasets.md)。
5. source として [OracleSource](data-factory-onprem-oracle-connector.md#oracle-copy-activity-type-properties) を、sink として [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) を使用するコピー アクティビティを含む[パイプライン](data-factory-create-pipelines.md)。

このサンプルはオンプレミスの Oracle データベース内のテーブルから BLOB に 1 時間ごとにデータをコピーします。 サンプルで使用されるさまざまなプロパティの詳細については、サンプルに続くセクションのドキュメントを参照してください。

**Oracle のリンクされたサービス:**

```json
{
    "name": "OnPremisesOracleLinkedService",
    "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
            "driverType": "Microsoft",
            "connectionString":"Host=<host>;Port=<port>;Sid=<sid>;User Id=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```

**Azure BLOB ストレージのリンクされたサービス:**

```json
{
    "name": "StorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<Account key>"
        }
    }
}
```

**Oracle の入力データセット:**

このサンプルでは、Oracle で「MyTable」という名前のテーブルを作成し、時系列データ用に「timestampcolumn」という名前の列が含まれているものと想定しています。

”external” を ”true” に設定すると、データセットが Data Factory の外部にあり、Data Factory のアクティビティによって生成されたものではないことが Data Factory サービスに通知されます。

```json
{
    "name": "OracleInput",
    "properties": {
        "type": "OracleTable",
        "linkedServiceName": "OnPremisesOracleLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "external": true,
        "availability": {
            "offset": "01:00:00",
            "interval": "1",
            "anchorDateTime": "2014-02-27T12:00:00",
            "frequency": "Hour"
        },
        "policy": {     
            "externalData": {        
                "retryInterval": "00:01:00",    
                "retryTimeout": "00:10:00",       
                "maximumRetry": 3       
            }     
        }
    }
}
```

**Azure BLOB の出力データセット:**

データは新しい BLOB に 1 時間おきに書き込まれます (頻度: 時間、間隔: 1)。 BLOB のフォルダー パスとファイル名は、処理中のスライスの開始時間に基づき、動的に評価されます。 フォルダー パスは開始時間の年、月、日、時刻の部分を使用します。

```json
{
    "name": "AzureBlobOutput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
            "partitionedBy": [
                {
                    "name": "Year",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "yyyy"
                    }
                },
                {
                    "name": "Month",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "MM"
                    }
                },
                {
                    "name": "Day",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "dd"
                    }
                },
                {
                    "name": "Hour",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "HH"
                    }
                }
            ],
            "format": {
                "type": "TextFormat",
                "columnDelimiter": "\t",
                "rowDelimiter": "\n"
            }
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

**コピー アクティビティのあるパイプライン:**

パイプラインには、入力データセットと出力データセットを使用するように構成され、1 時間おきに実行するようにスケジュールされているコピー アクティビティが含まれています。 パイプライン JSON 定義で、**source** の型が **OracleSource** に設定され、**sink** の型が **BlobSink** に設定されています。  **oracleReaderQuery** プロパティに指定されている SQL クエリは過去のデータを選択してコピーします。

```json
{  
    "name":"SamplePipeline",
    "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline for copy activity",
        "activities":[  
            {
                "name": "OracletoBlob",
                "description": "copy activity",
                "type": "Copy",
                "inputs": [
                    {
                        "name": " OracleInput"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOutput"
                    }
                ],
                "typeProperties": {
                    "source": {
                        "type": "OracleSource",
                        "oracleReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                    },
                    "sink": {
                        "type": "BlobSink"
                    }
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "policy": {
                    "concurrency": 1,
                    "executionPriorityOrder": "OldestFirst",
                    "retry": 0,
                    "timeout": "01:00:00"
                }
            }
        ]
    }
}
```

## <a name="sample-copy-data-from-azure-blob-to-oracle"></a>サンプル: Azure BLOB から Oracle にデータをコピーする
このサンプルは、Azure Blob Storage からオンプレミスの Oracle データベースにデータをコピーする方法を示します。 ただし、Azure Data Factory のコピー アクティビティを使用して、**こちら**に記載されているいずれかのソースからデータを[直接](data-factory-data-movement-activities.md#supported-data-stores-and-formats)コピーできます。  

このサンプルでは、次の Data Factory のエンティティがあります。

1. [OnPremisesOracle](data-factory-onprem-oracle-connector.md#oracle-linked-service-properties)型のリンクされたサービス。
2. [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service)型のリンクされたサービス。
3. [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties) 型の入力[データセット](data-factory-create-datasets.md)。
4. [OracleTable](data-factory-onprem-oracle-connector.md#oracle-dataset-type-properties) 型の出力[データセット](data-factory-create-datasets.md)。
5. source として [BlobSource](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) を、sink として [OracleSink](data-factory-onprem-oracle-connector.md#oracle-copy-activity-type-properties) を使用するコピー アクティビティを含む[パイプライン](data-factory-create-pipelines.md)。

このサンプルはオンプレミスの Oracle データベース内のテーブルに BLOB から 1 時間ごとにデータをコピーします。 サンプルで使用されるさまざまなプロパティの詳細については、サンプルに続くセクションのドキュメントを参照してください。

**Oracle のリンクされたサービス:**
```json
{
    "name": "OnPremisesOracleLinkedService",
    "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
            "connectionString": "Data Source=(DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST=<hostname>)(PORT=<port number>))(CONNECT_DATA=(SERVICE_NAME=<SID>)));
            User Id=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```

**Azure BLOB ストレージのリンクされたサービス:**
```json
{
    "name": "StorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<Account key>"
        }
    }
}
```

**Azure BLOB の入力データセット**

データは新しい BLOB から 1 時間おきに取得されます (頻度: 時間、間隔: 1)。 BLOB のフォルダー パスとファイル名は、処理中のスライスの開始時間に基づき、動的に評価されます。 フォルダー パスは開始時間の年、月、日の部分を利用し、ファイル名は開始時間の時刻部分を使用します。 "external": "true" 設定は Data Factory サービスにこのテーブルが Data Factory の外部にあり、Data Factory のアクティビティでは生成されないことを通知します。

```json
{
    "name": "AzureBlobInput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}",
            "partitionedBy": [
                {
                    "name": "Year",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "yyyy"
                    }
                },
                {
                    "name": "Month",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "MM"
                    }
                },
                {
                    "name": "Day",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "dd"
                    }
                }
            ],
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ",",
                "rowDelimiter": "\n"
            }
        },
        "external": true,
        "availability": {
            "frequency": "Day",
            "interval": 1
        },
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

**Oracle 出力データセット:**

このサンプルは、「MyTable」という名前のテーブルが Oracle に作成されていることを前提としています。 BLOB CSV ファイルに含めることが予想される列の数と同じ列数で Oracle にテーブルを作成します。 新しい行は 1 時間ごとにテーブルに追加されます。

```json
{
    "name": "OracleOutput",
    "properties": {
        "type": "OracleTable",
        "linkedServiceName": "OnPremisesOracleLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "availability": {
            "frequency": "Day",
            "interval": "1"
        }
    }
}
```

**コピー アクティビティのあるパイプライン:**

パイプラインには、入力データセットと出力データセットを使用するように構成され、1 時間おきに実行するようにスケジュールされているコピー アクティビティが含まれています。 パイプライン JSON 定義で、**source** の型が **BlobSource** に設定され、**sink** の型が **OracleSink** に設定されています。  

```json
{  
    "name":"SamplePipeline",
    "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-05T19:00:00",
        "description":"pipeline with copy activity",
        "activities":[  
            {
                "name": "AzureBlobtoOracle",
                "description": "Copy Activity",
                "type": "Copy",
                "inputs": [
                    {
                        "name": "AzureBlobInput"
                    }
                ],
                "outputs": [
                    {
                        "name": "OracleOutput"
                    }
                ],
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "OracleSink"
                    }
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                },
                "policy": {
                    "concurrency": 1,
                    "executionPriorityOrder": "OldestFirst",
                    "retry": 0,
                    "timeout": "01:00:00"
                }
            }
        ]
    }
}
```

## <a name="oracle-linked-service-properties"></a>Oracle のリンクされたサービスのプロパティ
次の表は、Oracle のリンクされたサービスに固有の JSON 要素の説明をまとめたものです。

| プロパティ | 説明 | 必須 |
| --- | --- | --- |
| type |type プロパティを **OnPremisesOracle** |はい |
| driverType | Oracle Database との間でデータをコピーするときに使用するドライバーを指定します。 使用できる値は **Microsoft** または **ODP** (既定値) です。 ドライバーの詳細については、「[サポートされているバージョンとインストール](#supported-versions-and-installation)」を参照してください。 | いいえ |
| connectionString | connectionString プロパティの Oracle Database インスタンスに接続するために必要な情報を指定します。 | はい |
| gatewayName | オンプレミスの Oracle サーバーへの接続に使用されるゲートウェイの名前です |はい |

オンプレミスの Oracle データ ソースの資格情報の設定について詳しくは、「[Data Management Gateway を使用してオンプレミスのソースとクラウドの間でデータを移動する](data-factory-move-data-between-onprem-and-cloud.md)」を参照してください。

**例: Microsoft ドライバーの使用**
```JSON
{
    "name": "OnPremisesOracleLinkedService",
    "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
            "driverType": "Microsoft",
            "connectionString":"Host=<host>;Port=<port>;Sid=<sid>;User Id=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```

**例: ODP ドライバーの使用**

使用できる形式の詳細については、[こちらのサイト](https://www.connectionstrings.com/oracle-data-provider-for-net-odp-net/)を参照してください。
```JSON
{
    "name": "OnPremisesOracleLinkedService",
    "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
            "connectionString": "Data Source=(DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST=<hostname>)(PORT=<port number>))(CONNECT_DATA=(SERVICE_NAME=<SID>)));
User Id=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```

## <a name="oracle-dataset-type-properties"></a>Oracle データセットの type プロパティ
データセットの定義に利用できるセクションとプロパティの完全な一覧については、「[データセットの作成](data-factory-create-datasets.md)」という記事を参照してください。 データセット JSON の構造、可用性、ポリシーなどのセクションはすべてのデータセット型 (Oracle、Azure BLOB、Azure テーブルなど) で同じです。

typeProperties セクションはデータセット型ごとに異なり、データ ストアのデータの場所などに関する情報を提供します。 OracleTable 型のデータセットの typeProperties セクションには次のプロパティがあります。

| プロパティ | 説明 | 必須 |
| --- | --- | --- |
| tableName |リンクされたサービスが参照する Oracle データベース インスタンスのテーブルの名前です。 |いいえ (**OracleSource** の **oracleReaderQuery** が指定されている場合) |

## <a name="oracle-copy-activity-type-properties"></a>Oracle のコピー アクティビティの type プロパティ
アクティビティの定義に利用できるセクションとプロパティの完全な一覧については、[パイプラインの作成](data-factory-create-pipelines.md)に関する記事を参照してください。 名前、説明、入力テーブル、出力テーブル、ポリシーなどのプロパティは、あらゆる種類のアクティビティで使用できます。

> [!NOTE]
> コピー アクティビティは入力を 1 つだけ受け取り、出力を 1 つだけ生成します。
>
>

一方、アクティビティの typeProperties セクションで使用できるプロパティは、各アクティビティの種類によって異なります。 コピー アクティビティの場合、ソースとシンクの種類によって異なります。

### <a name="oraclesource"></a>oracleReaderQuery
コピー アクティビティで、source の種類が **OracleSource** である場合は、**typeProperties** セクションで次のプロパティを使用できます。

| プロパティ | 説明 | 使用できる値 | 必須 |
| --- | --- | --- | --- |
| oracleReaderQuery |カスタム クエリを使用してデータを読み取ります。 |SQL クエリ文字列。 例: select *from MyTable <br/><br/>指定されていない場合に実行される SQL ステートメント: select* from MyTable |いいえ (**データセット**の **tableName** が指定されている場合) |

### <a name="oraclesink"></a>パイプライン
**OracleSink** では次のプロパティがサポートされます。

| プロパティ | 説明 | 使用できる値 | 必須 |
| --- | --- | --- | --- |
| writeBatchTimeout |タイムアウトする前に一括挿入操作の完了を待つ時間です。 |timespan<br/><br/> 例: "00:30:00" (30 分)。 |いいえ |
| writeBatchSize |バッファー サイズが writeBatchSize に達したときに SQL テーブルにデータを挿入します。 |整数 (行数) |いいえ (既定値: 100) |
| sqlWriterCleanupScript |特定のスライスのデータを消去するコピー アクティビティのクエリを指定します。 |クエリ ステートメント。 |いいえ |
| sliceIdentifierColumnName |自動生成スライス ID を入力するためのコピー アクティビティの列名を指定します。再実行時、特定のスライスのデータを消去するときに使用されます。 |バイナリ (32) のデータ型の列の列名。 |なし |

## <a name="troubleshooting-tips"></a>トラブルシューティングのヒント
### <a name="problem-1-net-framework-data-provider"></a>問題 1: .NET Framework データ プロバイダー

次のような**エラー メッセージ**が表示されます。

    Copy activity met invalid parameters: 'UnknownParameterName', Detailed message: Unable to find the requested .Net Framework Data Provider. It may not be installed”.  

**考えられる原因**

1. Oracle 用の .NET Framework Data Provider がインストールされていません。
2. Oracle 用の .NET Framework Data Provider が .NET Framework 2.0 にインストールされ、.NET Framework 4.0 フォルダーで検出されません。

**解決/回避策**

1. Oracle 用の .NET Provider をインストールしていない場合は [インストール](http://www.oracle.com/technetwork/topics/dotnet/downloads/) した後、シナリオをやり直します。
2. プロバイダーをインストールしてもエラー メッセージが表示される場合は、次の手順を実行します。
   1. 次のフォルダーから .NET 2.0 のコンピューター構成を開きます。<system disk>:\Windows\Microsoft.NET\Framework64\v2.0.50727\CONFIG\machine.config
   2. **Oracle Data Provider for .NET** を検索すると、次のサンプルに示すように、**system.data** -> **DbProviderFactories** の下に 1 つのエントリが見つかります: "<add name="Oracle Data Provider for .NET" invariant="Oracle.DataAccess.Client" description="Oracle Data Provider for .NET" type="Oracle.DataAccess.Client.OracleClientFactory, Oracle.DataAccess, Version=2.112.3.0, Culture=neutral, PublicKeyToken=89b483f429c47342" />」というハウツー ガイドを参照してください。
3. このエントリを、v4.0 フォルダーの machine.config ファイル (<system disk>: \Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config) にコピーし、バージョンを 4.xxx.x.x に変更します。
4. `gacutil /i [provider path]` を実行して、"<ODP.NET インストール パス>\11.2.0\client_1\odp.net\bin\4\Oracle.DataAccess.dll" をグローバル アセンブリ キャッシュ (GAC) にインストールします。## トラブルシューティングのヒント

### <a name="problem-2-datetime-formatting"></a>問題 2: datetime 書式設定

次のような**エラー メッセージ**が表示されます。

    Message=Operation failed in Oracle Database with the following error: 'ORA-01861: literal does not match format string'.,Source=,''Type=Oracle.DataAccess.Client.OracleException,Message=ORA-01861: literal does not match format string,Source=Oracle Data Provider for .NET,'.

**解決/回避策**

場合によっては、次の例で示すように、Oracle データベースで日付がどのように構成されているかに基づいて、コピー アクティビティのクエリ文字列を調整する必要があります (to_date 関数を使用)。

    "oracleReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= to_date(\\'{0:MM-dd-yyyy HH:mm}\\',\\'MM/DD/YYYY HH24:MI\\')  AND timestampcolumn < to_date(\\'{1:MM-dd-yyyy HH:mm}\\',\\'MM/DD/YYYY HH24:MI\\') ', WindowStart, WindowEnd)"


[!INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### <a name="type-mapping-for-oracle"></a>Oracle の型マッピング
[データ移動アクティビティ](data-factory-data-movement-activities.md) に関する記事のとおり、コピー アクティビティは次の 2 段階のアプローチで型を source から sink に自動的に変換します。

1. ネイティブの source 型から .NET 型に変換する
2. .NET 型からネイティブの sink 型に変換する

Oracle からデータを移動すると、Oracle データ型から .NET 型へのマッピング (およびその逆) に、次のマッピングが使用されます。

| Oracle データ型 | .NET Framework データ型 |
| --- | --- |
| BFILE |Byte[] |
| BLOB |Byte[] |
| CHAR |String |
| CLOB |String |
| DATE |DateTime |
| FLOAT |Decimal |
| INTEGER |Decimal |
| INTERVAL YEAR TO MONTH |Int32 |
| INTERVAL DAY TO SECOND |timespan |
| LONG |String |
| LONG RAW |Byte[] |
| NCHAR |String |
| NCLOB |String |
| NUMBER |Decimal |
| NVARCHAR2 |String |
| RAW |Byte[] |
| ROWID |String |
| TIMESTAMP |DateTime |
| TIMESTAMP WITH LOCAL TIME ZONE |DateTime |
| TIMESTAMP WITH TIME ZONE |DateTime |
| 符号なし INTEGER |NUMBER |
| VARCHAR2 |String |
| XML |String |

> [!NOTE]
> Microsoft ドライバーを使用する場合、データ型 **INTERVAL YEAR TO MONTH** および **INTERVAL DAY TO SECOND** はサポートされません。
>

[!INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

## <a name="performance-and-tuning"></a>パフォーマンスとチューニング
Azure Data Factory でのデータ移動 (コピー アクティビティ) のパフォーマンスに影響する主な要因と、パフォーマンスを最適化するための各種方法については、「[コピー アクティビティのパフォーマンスとチューニングに関するガイド](data-factory-copy-activity-performance.md)」を参照してください。

