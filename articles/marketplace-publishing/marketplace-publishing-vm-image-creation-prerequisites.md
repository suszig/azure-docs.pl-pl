<properties
   pageTitle="Azure Marketplace 向け仮想マシン イメージを作成するための技術的な前提条件 | Microsoft Azure"
   description="他のユーザーが購入できる仮想マシン イメージを作成して Azure Marketplace にデプロイするための要件を理解します。"
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
  ms.date="10/09/2015"
  ms.author="hascipio; v-divte"/>


# Azure Marketplace 向け仮想マシン イメージを作成するための技術的な前提条件

始める前にプロセスをよく読み、各ステップをどこで、なぜ実行するのかを理解してください。 可能な限り、会社の情報と他のデータを準備し、必要なツールをダウンロードし、技術コンポーネントを作成してから、プラン作成プロセスを開始する必要があります。 この記事を読んでよく理解してください。

## 必要なツールとアプリケーションをダウンロードします。

プロセスを開始する前に、次の項目を準備してください。

- 対象とするオペレーティング システムに合わせて、Azure のダウンロード ページから、Azure PowerShell コマンドレットまたは Linux コマンド ライン インターフェイスのツールをインストールします。
- Azure Storage エクスプローラーを CodePlex からインストールします。
- 次のリンクから、Azure 認定用の認定テスト ツールをダウンロードおよびインストールします。
  - [http://go.microsoft.com/fwlink/?LinkID=526913](http://go.microsoft.com/fwlink/?LinkID=526913)します。 この認定ツールは、Windows ベースのコンピューターで実行する必要があります。 使用可能な Windows ベースのコンピューターがない場合は、Azure で Windows ベースの VM を使用するとツールを実行できます。

## サポートされているプラットフォーム

Azure ベース のVM は、Windows または Linux 上で開発することができます。 Azure と互換性のある仮想ハードディスク (VHD) の作成などの発行プロセスの一部の要素では、使用されているオペレーティング システムに合わせて異なるツールや手順を使用します。

- Linux を使用している場合の作成 Azure と互換性のある VHD (Linux ベース)] セクションを参照して、 [仮想マシン イメージの発行ガイド](marketplace-publishing-vm-image-creation.md)します。
- Windows を使用している場合の作成 Azure と互換性のある VHD (Windows ベース)] セクションを参照して、 [仮想マシン イメージの発行ガイド](marketplace-publishing-vm-image-creation.md)します。

> [AZURE.NOTE] Windows ベースのコンピューターにアクセスする必要があります。
- 証明書の検証ツールを実行します。
- VHD の認定の提出の VHD が共有アクセス署名 URL を作成します。

## VHD の開発

Azure VHD は、クラウドまたはオンプレミスでも開発できます。

- クラウド ベースで開発する場合、すべての開発手順は、Azure 上の VHD でリモート実行することになります。
- オンプレミスで開発する場合、VHD をダウンロードして、オンプレミス インフラストラクチャを使用して VHD を開発する必要があります。 これを実行することは可能ですが、お勧めしません。 オンプレミスで Windows 用または SQL 用に開発する場合は、関連するオンプレミスのライセンス キーが必要になることに注意してください。 VM の作成後に SQL Server をインストールすることはできません。 また、プランは Azure ポータルからの承認済みの SQL イメージに基づく必要があります。 オンプレミスで開発する場合は、クラウドで開発する場合とは異なる手順をいくつか行わなければなりません。 関連する情報を検索する [内部設置型 VM イメージの作成](marketplace-publishing-vm-image-creation-on-premise.md)します。

## 次のステップ

これで、前提条件を確認し、必要なタスクを完了すると、進むで詳述するように仮想マシン イメージのサービスを作成すると、 [仮想マシン イメージの発行ガイド](marketplace-publishing-vm-image-creation.md)します。

## 関連項目

- [作業の開始: オファーを Azure Marketplace に発行する方法](marketplace-publishing-getting-started.md)
- [Azure プレビュー ポータルで Windows を実行する仮想マシンの作成します。](../virtual-machines/virtual-machines-windows-tutorial/)



[link-acct-creation]: marketplace-publishing-accounts-creation-registration.md 

