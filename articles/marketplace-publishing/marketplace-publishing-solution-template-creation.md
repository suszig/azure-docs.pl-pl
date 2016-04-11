<properties
   pageTitle="Marketplace 用ソリューション テンプレートを作成するためのガイド |Microsoft Azure"
   description="Azure Marketplace で購入できる複数 VM イメージのソリューション テンプレートを作成、認定、およびデプロイする方法を詳しく説明します。"
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager=""
   editor=""/>

   <tags
      ms.service="marketplace"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="na"
      ms.date="10/28/2015"
      ms.author="hascipio; v-divte" />

# Azure Marketplace 向けソリューション テンプレートの作成ガイド
終了すると、手順 1、 [アカウントの作成と登録][link-acct-creation], にある Azure と互換性のあるソリューション テンプレートの作成時に、ガイド付きお [ソリューション テンプレートを作成するための技術的な前提条件](marketplace-publishing-solution-template-creation-prerequisites.md)します。 これで、手順を説明する複数の Vm 用のソリューション テンプレートを作成するための手順に従って、 [公開ポータル][link-pubportal] Azure Marketplace のです。

## 発行ポータルでソリューション テンプレート プランを作成する
移動して  [https://publish.windowsazure.com](http://publish.windowsazure.com)します。 初めてサインインするときに、 [公開ポータル](https://publish.windowsazure.com/), 、会社の販売者のプロファイルが登録されていると、同じアカウントを使用します。 発行ポータルでは、他の従業員を共同管理者として後から追加できます。

### 1.[ソリューション テンプレート] を選択する

  ![図][img-pubportal-menu-sol-templ]

### 2.新しいソリューション テンプレートを作成する

  ![図][img-pubportal-sol-templ-new]

### 3.最初にトポロジを作成する
ソリューション テンプレートは、作成したすべてのトポロジの 「親」 となります。 1 つのプランまたはソリューション テンプレートでは、複数のトポロジを定義できます。 プランをステージングにプッシュすると、すべてのトポロジも一緒にプッシュされます。 次の手順に従ってプランを定義します。     
- トポロジの作成: 通常、"トポロジ識別子" はソリューション テンプレートのトポロジ名です。 トポロジ識別子は、次に示すように URL で使用されます。

  Azure Marketplace。
http://azure.microsoft.com/marketplace/partners/{PublisherNamespace}/{OfferIdentifier} {TopologyIdentifier}

  Azure プレビュー ポータル:
https://ms.portal.azure.com/#gallery/{PublisherNamespace} します。{OfferIdentifier}{TopologyIdentifier}

- 新しいバージョンを追加します。

### 4.トポロジのバージョンの認定を受ける
トポロジの特定バージョンをプロビジョニングするために必要な、すべてのファイルを含む zip ファイルをアップロードします。 この zip ファイルには、次のファイルを含める必要があります。
- *mainTemplate.json* と *createUiDefinition.json* のルート ディレクトリにあるファイルです。
- リンクされたテンプレートと必要なすべてのスクリプト。

Zip ファイルをアップロードすると、次のようにクリックします。 **証明書の要求**します。 Microsoft の認定チームがファイルを確認し、トポロジを認定します。

以下の手順を実行すると、顧客向けに実際にデプロイメントを行わずに、作成エクスペリエンスを検証することもできます。

1. 保存、 *createUiDefinition.json* と絶対 URL を生成します。 この URL は、パブリックにアクセスできる必要があります。
2. 位置にあるツールを使用して、URL をエンコード [http://www.url-encode-decode.com/](http://www.url-encode-decode.com/)します。
3. 太字のテキストを置き換えるの場所 (エンコードされた URL)、 *createUiDefinition.json* 検証する必要があります。

  > https://portal.azure.com/?clientOptimizations=false#blade/Microsoft_Azure_Compute/CreateMultiVmWizardBlade/internal_bladeCallId/anything/internal_bladeCallerParams/ **{"initialData": {}、"providerConfig": {"createUiDefinition":"http://yoururltocreateuidefinition.jsonURLencoded"}}**

4. 任意のブラウザーで URL をコピーして貼り付けると、createUiDefinition.json ファイルのカスタマー エクスペリエンスを参照できます。

  > [AZURE.TIP] ソリューション テンプレートのトポロジを作成して、両者を認定を受けて、ビジネスに機能しますが、開発者は、会社の部門をマーケティング、または法的ながマーケティングおよび法的コンテンツで作業できます。

## 次のステップ
これでソリューション テンプレートを作成して、証明書の必要なファイルの zip ファイルを送信して、ことができますできます引き続きを指示に従って、 [Marketplace マーケティング コンテンツ ガイド](marketplace-publishing-push-to-staging.md) ステージング環境でテストするため、サービスを準備する前にします。 または、marketplace の記事の発行の完全なセットを表示するを参照して [作業の開始: オファーを Azure Marketplace に発行する方法](marketplace-publishing-getting-started.md)します。

必要に応じて次の関連する記事も参照してください。

- VM イメージ: [Azure の仮想マシン イメージについて](https://msdn.microsoft.com/library/azure/dn790290.aspx)

- VM 拡張機能: [VM エージェントと VM 拡張機能の概要](https://msdn.microsoft.com/library/azure/dn832621.aspx) と [Azure VM 拡張機能と機能](https://msdn.microsoft.com/library/azure/dn606311.aspx)

- Azure リソース マネージャー: [Azure ARM テンプレートの作成](../resource-group-authoring-templates/) と [単純な ARM テンプレートの例](https://github.com/rjmax/ArmExamples)

- ストレージ アカウントの調整: [のストレージ アカウントの制限の監視方法](http://blogs.msdn.com/b/mast/archive/2014/08/02/how-to-monitor-for-storage-account-throttling.aspx) と [Premium storage](../storage/storage-premium-storage-preview-portal/#scalability-and-performance-targets-when-using-premium-storage)

[img-pubportal-menu-sol-templ]:media/marketplace-publishing-solution-template-creation/pubportal-menu-solution-templates.png
[img-pubportal-sol-templ-new]:media/marketplace-publishing-solution-template-creation/pubportal-solution-template-new.png
[link-acct-creation]:marketplace-publishing-microsoft-accounts-creation-registration.md
[link-pubportal]:https://publish.windowsazure.com


