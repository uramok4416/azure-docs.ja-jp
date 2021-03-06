---
title: "Azure Network Watcher トポロジの確認 - Azure CLI | Microsoft Docs"
description: "この記事では Azure CLI を使用してネットワーク トポロジにクエリを実行する方法を説明します。"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 5cd279d7-3ab0-4813-aaa4-6a648bf74e7b
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: 1d0136b044f6049e59fa09d824cf244cac703c45
ms.openlocfilehash: 404e621bc1d16d15aeb705a1055ed4c2b9e251a2
ms.lasthandoff: 02/23/2017

---

# <a name="view-network-watcher-topology-with-azure-cli"></a>Azure CLI を使用した Network Watcher トポロジの確認

> [!div class="op_single_selector"]
> - [PowerShell](network-watcher-topology-powershell.md)
> - [CLI](network-watcher-topology-cli.md)
> - [REST API](network-watcher-topology-rest.md)

Network Watcher の Topology 機能では、サブスクリプションのネットワーク リソースを視覚的に確認できます。 ポータルでは、視認できる形で自動的に表示されます。 ポータルのトポロジ ビューに含まれる情報は、PowerShell 経由で取得することもできます。
この機能により、データを他のツールで視覚化して使用できるため、トポロジの情報の汎用性が高まります。

[!INCLUDE [network-watcher-preview](../../includes/network-watcher-public-preview-notice.md)]

相互接続は、2 つのリレーションシップでモデル化されます。

- **含有** - 例: VNet にサブネットが含まれ、サブネットに NIC が含まれる
- **関連付け** - 例: NIC が VM に関連付けられている

Topology REST API のクエリを実行して返されるプロパティの一覧は次のとおりです。

* **name** - リソースの名前。
* **id** - リソースの URI。
* **location** - リソースのある場所。
* **associations** - 参照されたオブジェクトへの関連付けの一覧。
    * **name** - 参照されたリソースの名前。
    * **resourceId** - 関連付けで参照されているリソースの URI。
    * **associationType** - 子オブジェクトと親のリレーションシップを参照する値。 有効な値は **Contains** または **Associated**。

## <a name="before-you-begin"></a>開始する前に

このシナリオでは、`network watcher topology` コマンドレットでトポロジ情報を取得します。 [REST API でネットワーク トポロジを取得する](network-watcher-topology-rest.md)方法を説明している記事もあります。

このシナリオは、[Network Watcher の作成](network-watcher-create.md)に関するページの手順を参照して、Network Watcher を作成済みであることを前提としています。

## <a name="scenario"></a>シナリオ

この記事で取り上げているシナリオでは、特定のリソース グループのトポロジの応答を取得します。

## <a name="retrieve-topology"></a>トポロジの取得

`network watcher topology` コマンドレットは、特定のリソース グループのトポロジを取得します。 JSON 形式で出力を確認するには、引数 "--json" を追加します。

```azurecli
azure network watcher topology -g resourceGroupName -n networkWatcherName -r topologyResourceGroupName --json
```

## <a name="results"></a>結果

返される結果には "Resources" というプロパティ名が付いています。これには、`network watcher topology` コマンドレットの JSON 応答の本文が含まれます。  この応答には、ネットワーク セキュリティ グループのリソースとその関連付け (つまり Contains と Associated) が含まれています。

```json
{
  "id": "00000000-0000-0000-0000-000000000000",
  "createdDateTime": "2017-02-17T22:20:59.461Z",
  "lastModified": "2016-12-19T22:23:02.546Z",
  "resources": [
    {
      "name": "testrg-vnet",
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/testrg-vnet",
      "location": "westcentralus",
      "associations": [
        {
          "name": "default",
          "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/testrg-vnet/subnets/default",
          "associationType": "Contains"
        }
      ]
    },
    {
      "name": "default",
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/testrg-vnet/subnets/default",
      "location": "westcentralus",
      "associations": []
    },
    {
      "name": "testclient",
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Compute/virtualMachines/testclient",
      "location": "westcentralus",
      "associations": [
        {
          "name": "testNic",
          "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkInterfaces/testNic",
          "associationType": "Contains"
        }
      ]
    },
    ...    
  ]
}
```

## <a name="next-steps"></a>次のステップ

[セキュリティ グループ ビューの概要](network-watcher-security-group-view-overview.md)に関するページで、ネットワーク リソースに適用されるセキュリティ ルールを確認する

