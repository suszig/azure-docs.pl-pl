<properties
    pageTitle="Azure Batch アカウントの作成 | Microsoft Azure"
    description="Azure ポータルで、クラウド内で大規模な並列ワークロードを実行する Azure Batch アカウントを作成する方法について説明します"
    services="batch"
    documentationCenter=""
    authors="dlepow"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.workload="big-compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="11/10/2015"
    ms.author="danlep"/>



# Azure ポータルで Azure Batch アカウントを作成して管理する

> [AZURE.SELECTOR]
- [Azure ポータル](batch-account-create-portal.md)
- [.NET のバッチ管理](batch-management-dotnet.md)

この記事は、使用する方法を示します、 [Azure ポータル](https://portal.azure.com) を作成し、Azure Batch アカウントとアカウント キーなどの設定を管理します。 すべての Batch API 要求を認証するには、Batch アカウント URL と関連するアクセス キーが必要です。 また、Batch ワークロードのすべての Batch リソース (プール、ジョブ、タスクなど) を特定の Batch アカウントと関連付けます。  

>[AZURE.NOTE] 現在、プレビュー ポータルでは、Batch アカウントの管理およびいくつかのアカウントのリソースを表示するための機能をサポートしています。 開発時は、Batch API を介して Batch の全機能を使用できます。

## Batch アカウントを作成する

1. [Azure ポータル](https://portal.azure.com)にサインインします。

2. クリックして **新しい** > **コンピューティング** > **サービスのバッチ**します。

    ![Marketplace での Batch][marketplace_portal]

3. 情報を確認し、クリックして **作成**します。

4.  **新しい Batch アカウント** ブレードで、次の情報を入力します。

    a.  **アカウント名**, 、Batch アカウントの URL で使用する一意の名前を入力します。

    >[AZURE.NOTE] Batch アカウント名では、Azure に一意で、3 ~ 24 文字で、および小文字と数字のみを使用する必要があります。

    b. 複数のサブスクリプションの場合はクリックして **サブスクリプション** アカウントを作成する使用可能なサブスクリプションを選択します。

    c. クリックして **リソース グループ** 、アカウントの既存のリソース グループを選択するか、新しく作成します。

    d.  **場所**, 、Batch が使用可能な Azure リージョンを選択します。

    ![Batch アカウントを作成する][account_portal]

5. クリックして **作成** アカウントの作成を完了します。

## アクセス キーとアカウント設定の管理
アカウントの作成後は、ポータルでアカウントを見つけて、アクセス キー、正規ユーザーなどの設定を管理できます。

URL は、Batch アカウント **Essentials**します。 `https://<account_name>.<region>.batch.azure.com` という形式の URL です。

アクセス キーの表示と管理には、キー アイコンをクリックします。

![Batch アカウント キー][account_keys]

## Batch アカウントに関するその他の注意事項

* バッチ アカウント作成および管理する手段として、 [Batch PowerShell コマンドレット](batch-powershell-cmdlets-get-started.md) と [バッチ管理 .NET ライブラリ](http://www.nuget.org/packages/Microsoft.Azure.Management.Batch/)します。


* Azure Batch アカウントの取得は無料です。 のみの Azure コンピューティング リソース、およびその他のサービスの使用した場合でも課金のワークロードの実行 (を参照してください [バッチ料金](https://azure.microsoft.com/pricing/details/batch/))。

* 1 つの Batch アカウントで複数の Batch ワークロードを実行したり、異なる Azure リージョンの複数の Batch アカウントにワークロードを分散したりすることができます。

* いくつかの大規模なバッチ ワークロードを実行している場合は、特定の注意して [バッチ サービスのクォータと制限](batch-quota-limit.md) 、Azure サブスクリプションと各 Batch アカウントに適用されています。 Batch アカウントの現在のクォータは、アカウント プロパティのプレビュー ポータルに表示されます。

## 次のステップ

* 参照してください [Azure Batch の機能概要](batch-api-basics.md) を Batch の概念の詳細を参照してください。

* 使用して最初のアプリケーションの開発を開始する、 [バッチ .NET クライアント ライブラリ](batch-dotnet-get-started.md)します。

[marketplace_portal]: ./media/batch-account-create-portal/marketplace_batch.PNG
[account_portal]: ./media/batch-account-create-portal/batch_acct_portal.png
[account_keys]: ./media/batch-account-create-portal/account_keys.PNG

