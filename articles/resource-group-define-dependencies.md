<properties
   pageTitle="Azure リソース マネージャーのテンプレートでの依存関係の定義"
   description="デプロイ時にあるリソースを別のリソースに依存するように設定する方法について説明します。"
   services="azure-resource-manager"
   documentationCenter="na"
   authors="mmercuri"
   manager="wpickett"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="12/07/2015"
   ms.author="mmercuri"/>


# Azure リソース マネージャーのテンプレートでの依存関係の定義

特定のリソースでは、トポロジの成功に不可欠なアップストリームと子の依存関係が複数存在する場合があります。 使用して他のリソースの依存関係を定義する **dependsOn** と 
**リソース** リソースのプロパティです。 依存関係は、**reference** 関数を使用して指定することもできます。

    {
        "name": "<name-of-the-resource>",
        "type": "<resource-provider-namespace/resource-type-name>",
        "apiVersion": "<supported-api-version-of-resource>",
        "location": "<location-of-resource>",
        "tags": { <name-value-pairs-for-resource-tagging> },
        "dependsOn": [ <array-of-related-resource-names> ],
        "properties": { <settings-for-the-resource> },
        "resources": { <dependent-resources> }
    }

 また、リソース間のリレーションシップを定義したり、リソース グループ間でこのようなリレーションシップの定義をサポートできるリソース リンクもあります。

## dependsOn

特定の仮想マシンでは、データベース リソースが正常にプロビジョニングされていることを前提としている場合があります。 別の場合、複数ノードに対して依存であります、クラスターをインストールします。 
前に、クラスター管理ツールを使用してバーチャル マシンを展開します。

テンプレート内では、dependsOn プロパティを使用すると、リソースのこの依存関係を定義できます。 その値には、リソース名のコンマ区切りリストを指定できます。 間の依存関係 
リソースが評価され、リソースが依存する順にデプロイされます。 相互依存していないリソースは、平行してデプロイされます。

dependsOn を使用してリソース間の依存関係をマップする傾向があるものの、重要なのは、その操作を行う理由を理解することです。これは、デプロイのパフォーマンスに影響を与える可能性があるためです。 
たとえば、リソースが相互にどのように接続されているかをドキュメント化することが理由である場合、dependsOn は適切な方法ではありません。 DependsOn のライフ サイクルはデプロイのみを対象とは 
配置後の利用できません。 デプロイ後にこれらの依存関係を照会する方法はありません。 DependsOn を使用して実行するパフォーマンスに影響を及ぼすリスクをすることがあります誤って注意をそらす、 
並列処理を使用している必要があるそれ以外の場合配置エンジンです。 文書化し、リソース間のリレーションシップに対するクエリ機能を行うため、代わりに実行してください [リソースをリンク](resource-group-link-resources.md)します。

この要素は、reference 関数を使用してリソースの表現を取得する場合には不要です。これは、参照オブジェクトがリソースに対する依存関係を意味するためです。 実のところがある場合、 
参照と dependsOn、ガイダンスを使用する場合に、reference 関数を使用して、暗黙的な参照です。 繰り返しますが、その根拠はパフォーマンスです。 参照は暗黙的な依存関係を定義します。 
テンプレート内で参照されている、必要になることがわかっています。 その存在によって、該当、再度パフォーマンスさせ潜在的なリスクを回避するための最適化を回避します。 
並列処理を避けることが不必要に配置エンジンを煩雑です。

リソースとコピー ループで作成されたリソースの間に依存関係を定義する必要がある場合、dependsOn 要素をループの名前に設定できます。 例については、次を参照してください。 [Azure リソース マネージャーでリソースの複数のインスタンスを作成する](resource-group-create-multiple.md)します。

## resources

resources プロパティを使用すると、定義されているリソースに関連する子リソースを指定できます。 子リソースの定義の深さは 5 レベルまでです。 重要なのは、子リソースと親リソースの間に暗黙的な依存関係を作成しないことです。 親リソースの後に子リソースをデプロイする必要がある場合は、dependsOn プロパティを使用してその依存関係を明示的に指定する必要があります。

各親リソースは、子リソースとして特定のリソースの種類のみを受け取ります。 許容されるリソースの種類が指定された、 [テンプレート スキーマ](https://github.com/Azure/azure-resource-manager-schemas) の親リソースのです。 子リソースの種類の名前には、親リソースの種類の名前 (**Microsoft.Web/sites/config**、**Microsoft.Web/sites/extensions** など。これは両方とも **Microsoft.Web/sites** の子リソース)。

## reference 関数

reference 関数を使用すると、式では、他の JSON の名前と値のペアまたはランタイム リソースからその値を導出することができます。 参照式では、あるリソースが別のリソースに依存することを暗黙的に宣言します。 
下記の **propertyPath** で表されるプロパティは省略可能です。指定しない場合は、参照先はリソースになります。

    reference('resourceName').propertyPath

この要素または dependsOn 要素のいずれかを使用して依存関係を指定できますが、同じ依存リソースに両方の要素を使用する必要はありません。 このガイドを回避する暗黙の参照を使用する、 
誤って不要な dependsOn 要素に含まれる危険性は、並列配置の特性を行ってから配置エンジンを停止します。

詳細については、次を参照してください。 [関数を参照](../resource-group-template-functions/#reference)します。

## 次のステップ

- Azure リソース マネージャー テンプレートの作成方法については、次を参照してください。 [のテンプレートの作成](resource-group-authoring-templates.md)します。
- テンプレートで使用可能な関数の一覧は、次を参照してください。 [テンプレート関数](resource-group-template-functions.md)します。






