<properties
   pageTitle="Microsoft Azure リソースの消費を把握する | Microsoft Azure"
   description="Azure Billing Usage API と RateCard API の概念の概要について説明します。これらの API を使用すると、Azure のリソース消費と動向を把握できます。"
   services="billing"
   documentationCenter=""
   authors="BryanLa"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="billing"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="billing"
   ms.date="11/02/2015"
   ms.author="mobandyo;bryanla"/>

# Microsoft Azure リソースの消費を把握する 

ユーザーとパートナーには、Azure のコストを正確に予測し、管理する機能が必要です。  資本支出モデルから運営コスト モデルに移行する場合、特に大規模なクラウド デプロイの場合には、ショーバックとチャージバックを分析する機能だけでなく、見積と課金に関するモードの忠実度も提供する必要があります。 

このようなニーズに対して、この記事で説明する Azure Resource Usage API と Rate Card API を利用すると、Azure リソースの消費を新しい方法で把握できるようになります。  

## Azure Resource Usage API と RateCard API 

Azure Resource Usage API と RateCard API は、Azure リソース マネージャーが公開している API ファミリに含まれ、リソース プロバイダーとして実装されています。  

### Azure Resource Usage API (プレビュー)
ユーザーとパートナーは、Azure Resource Usage API を使用して推定 Azure 消費データを取得できます。 機能は、次のとおりです。
    
- **Azure ロールベースの Access Control** -顧客やパートナーが、アクセス ポリシーを構成して、 [Azure プレビュー ポータル](https://portal.azure.com) または [Azure PowerShell コマンドレット](powershell-install-configure.md) を指定するユーザーまたはアプリケーションには、サブスクリプションの使用状況データへのアクセスを取得できます。 呼び出し元は、認証に Azure Active Directory トークンを使用する必要があります。 また、呼び出し元が特定の Azure サブスクリプションの使用状況データにアクセスするには、リーダー、所有者、または共同作成者のロールに呼び出し元を追加する必要があります。

- **時間単位または日単位の集計** - 呼び出し元は、バケットを対象とするか、Azure 使用状況データ時間単位を指定できますか、日単位のバケットします。 既定値は日単位です。

- **提供されるインスタンス メタデータ (リソース タグが含まれています)** – インスタンス レベルの詳細など、完全修飾リソース uri (/subscriptions/{subscription-id}-/..)、加え、リソース グループの情報とリソースのタグを応答で提供されます。 ユーザーはこのデータのタグを使用して、クロス課金などの使用事例について、確定的にプログラムで使用状況を割り当てることができます。

- **提供されるリソース メタデータ** -測定名、測定カテゴリ、測定サブカテゴリ、単位および地域も渡されるので、呼び出し元は消費内容を理解することをより詳しくへの応答でなどのリソースの詳細。 マイクロソフトは、Azure ポータルでリソース メタデータの用語を揃えるに取り組んでも Azure 使用状況 CSV、EA 課金 CSV など他の公開されたエクスペリエンスのエクスペリエンス全体でデータを関連付けるために顧客を有効にします。

- **プランの種類のすべての使用状況** – について、従量課金制、MSDN、料金コミットメント、料金クレジット、EA をなどの他の種類、すべてのプランに、使用状況データがアクセスできます。

### Azure Resource RateCard API (プレビュー)
ユーザーとパートナーは、Azure Resource RateCard API を使用して、使用できる Azure リソースの一覧と、それぞれの推定料金情報を取得できます。 機能は、次のとおりです。

- **Azure ロールベースの Access Control** -顧客やパートナーが、アクセス ポリシーを構成して、 [Azure プレビュー ポータル](https://portal.azure.com) または [Azure PowerShell コマンドレット](powershell-install-configure.md) を指定するユーザーまたはアプリケーションには、RateCard データにアクセスを取得できます。 呼び出し元は、認証に Azure Active Directory トークンを使用する必要があります。 また、呼び出し元が特定の Azure サブスクリプションの使用状況データにアクセスするには、リーダー、所有者、または共同作成者のロールに呼び出し元を追加する必要があります。
    
- **プランの従量課金制、MSDN、料金コミットメントや料金クレジットのサポート (EA はサポートされていません)** -この API は、プラン レベルの Azure の料金については、サブスクリプション レベルとの比較を提供します。  この API の呼び出し元は、プラン情報を渡してリソースの詳細と料金を取得する必要があります。  EA プランには登録ごとにカスタマイズされた料金があるので、現在は EA 料金を提供できません。

## シナリオ

Usage API と RateCard API を組み合わせて実現できるシナリオ例を次に示します。

- **Azure 月間の** - 使用法を利用して、RateCard Api を組み合わせてより詳しく把握、クラウドには、時間単位または日単位のバケットの使用状況と料金の推定を分析することでので、月間です。 

- **アラートを設定する** – お客様やパートナーを設定できますリソース ベースまたは料金ベースのアラートの推定の消費と料金を取得することによって、クラウドの消費 Usage api と RateCard API を使用しています。

- **部品表を予測** – お客様やパートナーは、その推定の消費量を取得でき、クラウドが請求することが、請求サイクルの終わりを予測する機械学習アルゴリズムを適用して金額です。

- **消費前のコスト分析** – 顧客を予測するどの程度の部品表は、RateCard API を使用できますも予定の使用数を提供することにより、ワークロードを Azure に移行した場合。 他のクラウドまたはプライベート クラウドに既存のワークロードがある場合、その使用状況を Azure の料金に対応付けることで、より正確な Azure の推定使用量を取得できます。 これにより、経由で取得した内容の表示機能の改善、 [Azure 料金計算ツール](http://azure.microsoft.com/pricing/calculator/), 、(たとえば) 課金パートナーは、ピボット テーブルを提供し、料金コミットメントや料金クレジットなど、従量課金制以外のプラン タイプ間の比較、対照することです。 また、世界各国のさまざまなデータセンターにリソースをデプロイすると、合計コストに直接的な影響があるので、API には、デプロイの決定に必要な what-if 分析の種類を有効にして、コスト見積をリージョン別に変更する機能も用意されています。

- **What-if 分析** -

    - ユーザーとパートナーは、Azure リソースのリージョンや構成を変えた場合に、コスト効率が高くなるかどうかを判断できます。 Azure リソースのコストは、稼動している Azure リージョンに基づいて異なる場合があります。ユーザーとパートナーは、what-if 分析を利用してコストを最適化することができます。

    - また、別の Azure プラン タイプの方が Azure リソースの料金が安くなるかどうかについても判断できます。

## パートナー ソリューション

[Microsoft Azure の使用状況と RateCard Api を有効にする Cloudyn ユーザー向けに ITFM を提供する](billing-usage-rate-card-partner-solution-cloudyn.md) Azure Billing API パートナーが提供した統合エクスペリエンスについて説明 [Cloudyn](https://www.cloudyn.com/microsoft-azure/)します。  この記事では、Azure ユーザーが Cloudyn と Azure Billing API を使用して Azure 消費データを把握する方法に関する短時間のビデオなどで、統合エクスペリエンスについて詳しく説明しています。 

[Cloud Cruiser と Microsoft Azure Billing API の統合](billing-usage-rate-card-partner-solution-cloudcruiser.md) 説明方法 [Cloud Cruiser の Express for Azure Pack](http://www.cloudcruiser.com/partners/microsoft/) 顧客 1 人のユーザー インターフェイスから、Microsoft Azure プライベート クラウドまたはホスト型パブリック クラウドの運用面と料金面の両方をシームレスに管理できるようにする、WAP ポータルから直接利用します。   

## 次のステップ
+ チェック アウト、 [Azure Billing REST API リファレンス](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c) 両方の Api の詳細については、これは Azure リソース マネージャーによって提供される Api のセットの一部です。
+ すぐサンプル コードにある場合をチェック アウト、Microsoft Azure Billing API コード サンプルに [Azure Code Samples](https://azure.microsoft.com/documentation/samples/?term=billing)します。

## 詳細情報
+ 参照してください、 [Azure リソース マネージャーの概要](resource-group-overview.md) Azure リソース マネージャーについての詳細については、記事です。
+ その他のクラウドの把握に役立てるために必要なツール群については、Gartner の記事を参照してください [IT 財務管理 (ITFM) ツールの市場ガイド](http://www.gartner.com/technology/reprints.do?id=1-212F7AL&ct=140909&st=sb)します。

