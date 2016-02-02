<properties
   pageTitle="Azure Marketplace 向けプランを作成するための技術以外の前提条件 | Microsoft Azure"
   description="他のユーザーが購入できるプランを作成して、Azure Marketplace にデプロイするための要件を理解します。"
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager=""
   editor=""/>

<tags
  ms.service="marketplace"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="Azure"
  ms.workload="na"
  ms.date="12/06/2015"
  ms.author="hascipio; v-divte"/>


# Azure Marketplace 向けプラン作成の一般的な前提条件

プランの作成プロセスに必要な、ビジネス処理中心の一般的な前提条件を理解します。

## Microsoft 販売者として登録されていることを確認します。

Microsoft 販売者のアカウントを登録する方法の詳細な手順についてを参照してください [アカウントの作成と登録](marketplace-publishing-accounts-creation-registration.md)します。

- **既に登録されている**場合は、アカウントの所有者、または登録に使用された資格情報を確認します。
- 場合 **の公開アカウントの所有者ではない**, 、アカウントの所有者に共同管理者として、Microsoft アカウントを追加することが、 [公開ポータル](https://publish.windowsazure.com)します。 **[発行元]** タブで、**[管理者]** リンクを使用します。
- Azure 発行プロセスの関係者が、このアドレスへのリンクを含む電子メールを受信するようにします。 電子メールは、発行プロセスを完了するために、監視および応答する必要があります。
- 1 人のユーザーのみに関連付けられたアカウントを使用するのは避けてください。 そのユーザーが会社を退職した場合、SKU に関する情報へのアクセスや発行機能に影響することがあります。

> [AZURE.IMPORTANT] 無料プランのみを発行する予定 (またはライセンス持ち込み) であれば、会社の税金や銀行情報を入力する必要はありません。

> プラン作成を開始するには、会社の登録を完了する必要があります。 ただし、Microsoft 開発者アカウントの税金および銀行の情報に動作しますが、会社、開発者は、操作を開始できますで仮想マシン イメージを作成する、 [公開ポータル](https://publish.windowsazure.com), 、認定を受けて、取得して、Azure のステージング環境でテストします。 販売者アカウントの承認が必要になるのは、プランを Azure Marketplace に発行する最後の手順のみです。
>
> 販売者の登録が完了すると問題がある場合は、サポート チケットを次に示すようログします。
> 1. Contact [Support](https://support.microsoft.com/getsupport?wf=0&tenant=ClassicCommercial&oaspworkflow=start_1.0.0.0&supportregion=en-us&pesid=15635&ccsid=635847950577064286).
> 2. **[デベロッパー センター]** を選択する。
> 3. **[Publisher Profile]** を選択します。
> 4. 連絡方法を選択する。


## Azure の「従量課金制」サブスクリプションを取得する

これは、VM イメージを作成しを画像上に使用するサブスクリプション、 [Azure Marketplace](http://azure.microsoft.com/marketplace)します。 既存のサブスクリプションがないし、サインアップして https://account.windowsazure.com/signup?offer=ms-azr-0003p にしましょう。

## 「販売元」の国

> [AZURE.WARNING]
Azure Marketplace でサービスを販売するには、登録済みエンティティが、承認されたいずれかの「販売元」の国からのものである必要があります。 この制限は、支払いおよび課税上の理由から生じます。 「販売元」の国の範囲は今後拡大される予定です。 完全な一覧については、セクション 1 b を参照してください、 [Azure Marketplace への参加ポリシー](http://go.microsoft.com/fwlink/?LinkID=526833)します。

## 次のステップ

次は、プランの種類ごとの技術的な前提条件を紹介します。 Azure Marketplace 向けに作成するプランの種類に合わせて、記事へのリンクをクリックしてください。

| 仮想マシン イメージ| 開発者サービス| データ サービス| ソリューション テンプレート|
|-----|-----|-----|-----|
| [VM の技術的な前提条件](marketplace-publishing-vm-image-creation-prerequisites.md)| 開発者サービスの技術的な前提条件| [データ サービスの技術的な前提条件](marketplace-publishing-data-service-creation-prerequisites.md)| [ソリューション テンプレートの技術的な前提条件](marketplace-publishing-solution-template-creation-prerequisites.md)|

## 関連項目

- [作業の開始: オファーを Azure Marketplace に発行する方法](marketplace-publishing-getting-started.md)





