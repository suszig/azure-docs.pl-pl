<properties 
    pageTitle="Azure リソース マネージャーでのリソースのリンク" 
    description="Azure リソース マネージャーで、異なるリソース グループ内のリソース間のリンクを作成します。" 
    services="azure-resource-manager" 
    documentationCenter="" 
    authors="tfitzmac" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="azure-resource-manager" 
    ms.workload="multiple" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="11/06/2015" 
    ms.author="tomfitz"/>


# Azure リソース マネージャーでのリソースのリンク

デプロイ後に、リソース間のリレーションシップまたはリンクを照会する場合があります。 依存関係の展開を通知します。 
ライフ サイクルは、デプロイ時に終了します。 デプロイが完了すると、依存リソースの間に特定のリレーションシップはありません。

Azure リソース マネージャーがリソースを設定し、リレーションシップをクエリ リンクという新機能を提供する代わりに、 
間のリソース。 リソースのリンク元となるリソースや、リソースのリンク先となるリソースを確認できます。

リソース リンクのスコープには、サブスクリプション、リソース グループまたは特定のリソースを使用できます。 つまり、そのリソースへのリンクを文書化 
リソース グループ全体にわたるリレーションシップです。 複数のテンプレートと複数のリソース グループに、ソリューションを分解を開始すると 
これらのリソースのリンクを識別するメカニズムは、役に立つ証明します。 たとえば、通常、独自のライフ サイクルを持つデータベースは 
1 つのリソース グループ内に存在し、別のライフ サイクルでアプリが異なるリソース グループ内に存在します。 そのため、アプリケーションがデータベースに接続します。 
別のリソース グループ内のリソース間のリンクがあります。

リンクされるリソースは、すべて同じサブスクリプションに属する必要があります。 各リソースは、そのリソース以外の 50 のリソースにリンクできます。 リンク済みリソースのいずれかの場合 
削除または移動して、リンクの所有者は、残りのリンクをクリーンアップする必要があります。

## テンプレートでのリンク

テンプレート内のリソース間のリンクを定義するを参照してください。 [リソースへのリンク - テンプレート スキーマ](resource-manager-template-links.md)します。

## REST API を使用したリンク

デプロイ済みのリソース間のリンクを定義するには、次のように実行します。

    PUT https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/{provider-namespace}/{resource-type}/{resource-name}/providers/Microsoft.Resources/links/{link-name}?api-version={api-version}

{subscription-id} を、サブスクリプション ID に置き換えます。 {リソース-グループ} を置き換える {プロバイダー名前空間、{リソースの種類} と {リソース名} 値を使用しています。 
リンクの最初のリソースを識別します。 {link-name} を、作成するリンクの名前に置き換えます。 api-version には 2015-01-01 を使用します。

この要求に、リンクの 2 番目のリソースを定義するオブジェクトを含めます。

    {
        "name": "{new-link-name}",
        "properties": {
            "targetId": "subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/{provider-namespace}/{resource-type}/{resource-name}/",
            "notes": "{link-description}",
        }
    }

properties 要素には、2 番目のリソースの識別子を含めます。

例については、リンク、に関する情報を取得する方法などを参照してください [リンク済みリソース](https://msdn.microsoft.com/library/azure/mt238499.aspx)します。

## 次のステップ

- タグで、リソースを整理することもできます。 リソースのタグを付ける方法については、次を参照してください。 [タグを使用して、リソースを整理する](resource-group-using-tags.md)します。
- テンプレートを作成してデプロイするリソースを定義する方法については、次を参照してください。 [のテンプレートの作成](resource-group-authoring-templates.md)します。





