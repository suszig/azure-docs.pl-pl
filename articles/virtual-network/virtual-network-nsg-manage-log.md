<properties 
   pageTitle="NSG に関する操作、イベント、カウンターの監視 | Microsoft Azure"
   description="NGS に関するカウンター、イベント、操作のログ記録を有効にする方法について説明します。"
   services="virtual-network"
   documentationCenter="na"
   authors="telmosampaio"
   manager="carmonm"
   editor="tysonn"
   tags="azure-resource-manager"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="12/11/2015"
   ms.author="telmos" />


# ネットワーク セキュリティ グループ (NSG) のためのログ分析

Azure の各種ログを使用して NSG の管理やトラブルシューティングを行うことができます。 これらのログの一部はポータルからアクセスできます。また、すべてのログは、Azure BLOB ストレージから抽出して Excel や PowerBI などのさまざまなツールで表示することができます。 各種ログの詳細については、以下の一覧を参照してください。

- **監査ログ:** を使用することができます [Azure 監査ログ](insights-debugging-with-events.md) (旧称運用ログ)、Azure サブスクリプションとそれらの状態に送信されるすべての操作を表示します。 監査ログは既定で有効になっており、Azure プレビュー ポータルで表示できます。
- **イベント ログ:** このログを使用すると、MAC アドレスに基づいて VM とインスタンス ロールに適用される NSG ルールを表示できます。 これらのルールの状態は 60 秒ごとに収集されます。
- **カウンター ログ:** このログを使用すると、トラフィックを拒否または許可するために各 NSG ルールが適用された回数を表示できます。

>[AZURE.WARNING] ログは、リソース マネージャーのデプロイ モデルでデプロイされたリソースについてのみ使用できます。 クラシック デプロイ モデルのリソースには使用できません。 詳細については、2 つのモデルの参照、 [理解リソース マネージャーの展開と従来のデプロイ](resource-manager-deployment-model.md) 記事です。

## ログの有効化

監査ログは、リソース マネージャーのすべてのリソースで常に自動的に有効になります。 イベント ログとカウンター ログは、それらのログで利用できるデータの収集を開始するために有効にする必要があります。 ログを有効にするには、次の手順に従います。

1.  サインインする、 [Azure プレビュー ポータル](http://portal.azure.com)します。 既存のネットワーク セキュリティ グループをまだ持っていない場合 [NSG を作成する](virtual-networks-create-nsg-arm-ps.md) 続行する前にします。

2.  プレビュー ポータルで、**[参照]**、**[ネットワーク セキュリティ グループ]** の順にクリックします。

    ![Preview portal - Network security groups](./media/virtual-network-nsg-manage-log/portal-enable1.png)

3. 既存のネットワーク セキュリティ グループを選択します。

    ![Preview portal - Network security group settings](./media/virtual-network-nsg-manage-log/portal-enable2.png)

4. **[設定]** ブレードで **[診断]** をクリックし、**[診断]** ウィンドウで、**[状態]** の下の **[オン]** をクリックします。
5. **[設定]** ブレードで、**[ストレージ アカウント]** をクリックし、既存のストレージ アカウントを選ぶか、新しいストレージ アカウントを作成します。

>[AZURE.INFORMATION] 監査ログには、別々 のストレージ アカウントは不要です。 イベントとルールのログ記録にストレージを使用すると、サービス料金が発生します。

6. **[ストレージ アカウント]** の下にあるドロップダウン リストで、イベント、カウンター、またはその両方から記録するログを選択し、**[保存]** をクリックします。

    ![Preview portal - Diagnostics logs](./media/virtual-network-nsg-manage-log/portal-enable3.png)

## 監査ログ

監査ログ (以前の "操作ログ") は、既定では Azure によって生成されます。 ログは、Azure のイベント ログ ストアに 90 日間保存されます。 読み取ることによってこれらのログの詳細について、 [イベントの表示し、監査ログ](insights-debugging-with-events.md) 記事です。

## カウンター ログ

このログは、既に詳しく説明したように、NSG ごとにログを有効にした場合のみ生成されます。 データは、ログ記録を有効にしたときに指定したストレージ アカウントに格納されます。 次に示すように、リソースに適用された各ルールは、JSON 形式でログに記録されます。

    {
        "time": "2015-09-11T23:14:22.6940000Z",
        "systemId": "e22a0996-e5a7-4952-8e28-4357a6e8f0c5",
        "category": "NetworkSecurityGroupRuleCounter",
        "resourceId": "/SUBSCRIPTIONS/D763EE4A-9131-455F-8C5E-876035455EC4/RESOURCEGROUPS/INSIGHTOBONRP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/NSGINSIGHTOBONRP",
        "operationName": "NetworkSecurityGroupCounters",
        "properties": {
            "vnetResourceGuid":"{DD0074B1-4CB3-49FA-BF10-8719DFBA3568}",
            "subnetPrefix":"10.0.0.0/24",
            "macAddress":"001517D9C43C",
            "ruleName":"DenyAllOutBound",
            "direction":"Out",
            "type":"block",
            "matchedConnections":0
            }
    }

## イベント ログ

このログは、既に詳しく説明したように、NSG ごとにログを有効にした場合のみ生成されます。 データは、ログ記録を有効にしたときに指定したストレージ アカウントに格納されます。 次のデータがログに記録されます。

    {
        "time": "2015-09-11T23:05:22.6860000Z",
        "systemId": "e22a0996-e5a7-4952-8e28-4357a6e8f0c5",
        "category": "NetworkSecurityGroupEvent",
        "resourceId": "/SUBSCRIPTIONS/D763EE4A-9131-455F-8C5E-876035455EC4/RESOURCEGROUPS/INSIGHTOBONRP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/NSGINSIGHTOBONRP",
        "operationName": "NetworkSecurityGroupEvents",
        "properties": {
            "vnetResourceGuid":"{DD0074B1-4CB3-49FA-BF10-8719DFBA3568}",
            "subnetPrefix":"10.0.0.0/24",
            "macAddress":"001517D9C43C",
            "ruleName":"AllowVnetOutBound",
            "direction":"Out",
            "priority":65000,
            "type":"allow",
            "conditions":{
                "destinationPortRange":"0-65535",
                "sourcePortRange":"0-65535",
                "destinationIP":"10.0.0.0/8,172.16.0.0/12,169.254.0.0/16,192.168.0.0/16,168.63.129.16/32",
                "sourceIP":"10.0.0.0/8,172.16.0.0/12,169.254.0.0/16,192.168.0.0/16,168.63.129.16/32"
            }
        }
    }

## 監査ログの表示と分析

次のいずれかの方法を使用して、監査ログのデータを表示および分析できます。

- **Azure Tools:** Azure PowerShell、Azure コマンド ライン インターフェイス (CLI)、Azure REST API、または Azure プレビュー ポータルを使用して、監査ログから情報を取得します。 それぞれの方法の詳細な手順の詳細については、 [リソース マネージャーでの操作を監査](resource-group-audit.md) 記事です。
- **Power BI:** まだしていない場合、 [Power BI](https://powerbi.microsoft.com/pricing) アカウント、ことができます、いざ挑戦は無料です。 使用して、 [Azure 監査ログ用コンテンツ パック Power BI](https://support.powerbi.com/knowledgebase/articles/742695) として使用できる構成済みのダッシュ ボードを使用してデータを分析することができます-、またはカスタマイズします。

## カウンター ログとイベント ログの表示と分析

イベント ログとカウンター ログの場合は、自身のストレージ アカウントに接続して JSON ログ エントリを取得する必要があります。 JSON ファイルをダウンロードした後、そのファイルを CSV に変換し、Excel、Power BI などのデータ視覚化ツールで表示できます。
>[AZURE.TIP] Visual Studio と c# の定数および変数の値を変更する基本的な概念に慣れている場合を使用できます、 [コンバーター ツールをログイン](https://github.com/Azure-Samples/networking-dotnet-log-converter) Github から入手できます。

## その他のリソース

- [Power BI を使用して、Azure 監査ログを視覚化](http://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx) ブログの投稿です。
- [表示し、Power BI での Azure 監査ログの分析と](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/) ブログの投稿です。





