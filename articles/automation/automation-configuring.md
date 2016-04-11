<properties
   pageTitle="Configuring Azure Automation"
   description="Azure Automation を初めて使用するときに必要な構成手順について説明します。"
   services="automation"
   documentationCenter=""
   authors="SnehaGunda"
   manager="stevenka"
   editor="tysonn" />
<tags
   ms.service="automation"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="11/10/2015"
   ms.author="bwren;sngun" />

# Azure Automation の構成

この記事では、Azure Automation を使い始めるときに最初に行う必要があることについて説明します。

## Automation アカウント

Azure Automation を初めて開始するときに、少なくとも 1 つの Automation アカウントを作成する必要があります。 Automation アカウントでは、お使いの Automation リソース (Runbook、資産、構成) を、別の Automation アカウントに含まれる Automation リソースと分離することができます。 Automation アカウントを使って Automation リソースを異なる論理環境に分けられます。 たとえば、1 つのアカウントを開発環境用として、もう 1 つのアカウントを本番環境として使うこともできます。

各 Automation アカウントの Automation リソースは単一の Azure リージョンと関連付けられていますが、Automation アカウントではあらゆるリージョンの Azure サービスを管理することができます。 異なるリージョンで Automation アカウントを作成する主な理由としては、特定のリージョンに分離しなければならないデータやリソースを必要とするポリシーがある場合が挙げられます。

>[AZURE.NOTE] オートメーション アカウント、およびそこに含まれるリソースを Azure ポータルでの Azure プレビュー ポータルにアクセスできません。 これらのアカウントとリソースを Windows PowerShell で管理するには、[Azure リソース マネージャー] モジュールを使用する必要があります。 
>
>Azure ポータルで作成した Automation アカウントは、どちらのポータルでも、また、どちらのセットのコマンド レットでも管理できます。 アカウントを作成してしまえば、そのアカウントのリソースの作り方や管理方法に違いはありません。 引き続き Azure ポータルを使用する場合、Automation アカウントの作成には Azure プレビュー ポータルではなく、Azure ポータルを使用することをお勧めします。


お使いに Azure アカウントで支払の遅延などの問題が発生した場合、一時的に Automation アカウントを停止することがあります。 その場合、アカウントへのアクセスができなくなり、実行中のジョブはすべて中断されます。また、すべてのスケジュールが無効になります。 アカウントの閲覧はできますが、アカウントに含まれるリソースを見ることはできません。 問題を解決して Automation アカウントが有効になった後は、スケジュールを有効にして、中断された Runbook を再開する必要があります。


## Azure リソースへの認証の構成

使用して Azure のリソースにアクセスするとき、 [Azure コマンドレット](http://msdn.microsoft.com/library/azure/jj554330.aspx), 、Azure サブスクリプションに認証を提供する必要があります。 Azure Automation では、これは、サブスクリプションの管理者が構成する Azure Active Directory の組織アカウントで行います。 作成できます、 [資格情報](http://msdn.microsoft.com/library/dn940015.aspx) このユーザーのアカウントおよびそれを使用 [Add-azureaccount](http://msdn.microsoft.com/library/azure/dn722528.aspx) runbook でします。

>[AZURE.NOTE] 以前の LiveIDs、Microsoft アカウントは、Azure Automation では使用できません。

## Azure サブスクリプションを管理するための新しい Azure Active Directory ユーザーの作成

1. 管理する Azure サブスクリプションのサービス管理者として Azure ポータルにログインします。
2. Select **Active Directory**
3. Azure サブスクリプションと関連付けられているディレクトリ名を選択します。 かどうか、必要に応じて変更できますからこの関連付け **設定 > サブスクリプション > ディレクトリの編集**します。
4. [Active Directory ユーザーの新規作成](http://msdn.microsoft.com/library/azure/hh967632.aspx)します。  選択 **、組織内の新しいユーザー** の **ユーザーの種類** ものとない **多要素認証を有効にする**です。
5. Note the user’s full name and temporary password.
7. Select **Settings > Administrators > Add**.
8. 作成したユーザーの完全なユーザー名を入力します。
9. このユーザーで管理するサブスクリプションを選択してください。
10. ログアウトして、先ほど作成したアカウントで再度 Azure にログインする。 ユーザーのパスワードを変更するように促されます。
11. 新しい [Azure Automation の資格情報資産](http://msdn.microsoft.com/library/dn940015.aspx) 作成したユーザー アカウントのです。 The **Credential Type** should be **Windows PowerShell Credential**.

## Automation アカウントの作成

Automation アカウントは、Azure Automation リソースのコンテナーで、 環境の分離や、ワークフローの詳細な整理のための方法を提供します。 Automation アカウントを既に作成している場合は、次の手順を省略できます。

1. ログイン、 [Azure プレビュー ポータル](https://portal.azure.com/)します。

2. Azure プレビュー ポータルで、クリックして **新規** > **管理** > **オートメーション アカウント**

3.  **オートメーション アカウントの追加** ブレードで、オートメーション アカウントの詳細情報を構成します。 

>[AZURE.NOTE] Azure プレビュー ポータルを使用して、オートメーション アカウントが作成されると、アカウントと関連付けられているすべてのリソースに戻されない、従来の管理ポータル。 

構成するパラメーターを次に示します。

|パラメーター            |説明 |
|:---|:---|
| 名前 | Automation アカウントの名前。一意の値でなければなりません。 |
| リソース グループ | リソース グループを使用すると、関連する Azure リソースを簡単に表示および管理できます。 Azure プレビュー ポータルでは、既存のリソース グループを選択することも、Automation アカウント用に新しいリソース グループを作成することもできます。一方、Microsoft Azure 管理ポータルでは、すべての Automation アカウントが既定のリソース グループに入れられます。 |
| [サブスクリプション] | 使用可能なサブスクリプションの一覧から選択します。 |
| リージョン | リージョンでは、アカウントの Automation リソースが格納される場所を指定します。 一覧から任意のリージョンを選択できます。これは、アカウントの機能には影響しませんが、アカウントのリージョンがご使用のその他の Azure リソースが格納された場所に近い場合、Runbook をより素早く実行できます。 |
| アカウント オプション | このオプションでは、新しいオートメーション アカウント; に作成されるどのようなリソースを選択できます。選択すると **はい** チュートリアルの runbook を作成します。 |

![アカウントの作成](media/automation-configuration/automation-01-create-automation-account.png)

>[AZURE.NOTE] 従来の管理ポータルで作成したオートメーション アカウントの場合は [別のリソース グループに移動する](../resource-group-move-resources.md) Azure プレビュー ポータルを使用して、オートメーション アカウントしなくなる Azure クラシック ポータルで、使用できるように、従来の管理ポータルでは、Azure リソース マネージャーのアカウントはサポートされていません。



## Runbook での資格情報の使用

使用して runbook で資格情報を取得する、 [Get-automationpscredential](http://msdn.microsoft.com/library/dn940015.aspx) アクティビティと共に使用して [Add-azureaccount](http://msdn.microsoft.com/library/azure/dn722528.aspx) 、Azure サブスクリプションに接続します。 資格情報は、管理者は、複数の Azure サブスクリプションのかどうかは、使用することも必要があります [Select-azuresubscription](http://msdn.microsoft.com/library/dn495203.aspx) 適切なものを指定します。 これについては、ほとんどの Azure Automation Runbook の先頭に一般に出現する以下のサンプル Windows PowerShell で示されています。

    $cred = Get-AutomationPSCredential –Name "myuseraccount.onmicrosoft.com"
    Add-AzureAccount –Credential $cred
    Select-AzureSubscription –SubscriptionName "My Subscription"

いずれかの後、次の行を繰り返す必要があります [チェックポイント](http://technet.microsoft.com/library/dn469257.aspx#bk_Checkpoints) runbook でします。 Runbook が中断された後、別のワーカーで再開した場合は、もう一度認証を実行する必要があります。

## 関連記事:
- [Azure Automation: Authenticating to Azure using Azure Active Directory (Azure オートメーション: Azure Active Directory を使用して Azure を認証する)](http://azure.microsoft.com/blog/2014/08/27/azure-automation-authenticating-to-azure-using-azure-active-directory/)
 

