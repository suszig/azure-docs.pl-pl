## 仮想ネットワーク
仮想ネットワーク (VNET) とサブネット リソースは、Azure で実行するワークロードのセキュリティ境界を定義する際に役立ちます。 VNET は、(CIDR ブロックとして定義される) アドレス空間のコレクションを特徴としています。 

>[AZURE.NOTE] ネットワーク管理者は、CIDR 表記法に慣れています。 CIDR に慣れていない場合 [詳細情報を入手](http://whatismyipaddress.com/cidr)します。

![VNet with multiple subnets](./media/resource-groups-networking/Figure4.png)

VNet には、次のプロパティが含まれています。

|プロパティ|説明|値の例|
|---|---|---|
|**addressSpace**|VNet を構成するアドレス プレフィックスのコレクション (CIDR 表記)|192.168.0.0/16|
|**サブネット**|VNet を構成するサブネットのコレクション|参照してください [サブネット](#Subnets) 以下です。|
|**ipAddress**|オブジェクトに割り当てられる IP アドレスです。 これは、読み取り専用のプロパティです。|104.42.233.77|

### サブネット
サブネットは、VNet の子リソースで、IP アドレスのプレフィックスを使用して、CIDR ブロック内のアドレス空間のセグメントの定義に役立ちます。 NIC をサブネットに追加し、VM に接続して、さまざまなワークロードへの接続を提供できます。

サブネットには、次のプロパティが含まれています。 

|プロパティ|説明|値の例|
|---|---|---|
|**addressPrefix**|サブネットを構成する単一のアドレス プレフィックス (CIDR 表記)|192.168.1.0/24|
|**networkSecurityGroup**|サブネットに適用される NSG|参照してください [Nsg](#Network-Security-Group)|
|**routeTable**|サブネットに適用されるルート テーブル|参照してください [UDR](#Route-table)|
|**ipConfigurations**|サブネットに接続されている NIC で使用される IP 構成オブジェクトのコレクション|参照してください [UDR](#Route-table)|


JSON 形式の VNet のサンプルは次のとおりです。

    {
        "name": "TestVNet",
        "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet",
        "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
        "type": "Microsoft.Network/virtualNetworks",
        "location": "westus",
        "tags": {
            "displayName": "VNet"
        },
        "properties": {
            "provisioningState": "Succeeded",
            "resourceGuid": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
            "addressSpace": {
                "addressPrefixes": [
                    "192.168.0.0/16"
                ]
            },
            "subnets": [
                {
                    "name": "FrontEnd",
                    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                    "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                    "properties": {
                        "provisioningState": "Succeeded",
                        "addressPrefix": "192.168.1.0/24",
                        "networkSecurityGroup": {
                            "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-BackEnd"
                        },
                        "routeTable": {
                            "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/routeTables/UDR-FrontEnd"
                        },
                        "ipConfigurations": [
                            {
                                "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB1/ipConfigurations/ipconfig1"
                            },
                            ...]
                    }
                },
                ...]
        }
    }

### その他のリソース

- 詳細 [VNet](virtual-networks-overview.md)します。
- 読み取り、 [REST API リファレンス ドキュメント](https://msdn.microsoft.com/library/azure/mt163650.aspx) Vnet のです。
- 読み取り、 [REST API リファレンス ドキュメント](https://msdn.microsoft.com/library/azure/mt163618.aspx) サブネットです。
