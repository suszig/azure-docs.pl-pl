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
>[AZURE.NOTE] Azure プレビュー ポータルで作成した Automation アカウントおよび各アカウントに含まれるリソースは、 Azure ポータルと関連付けることはできません。 これらのアカウントとリソースを Windows PowerShell で管理するには、[Azure リソース マネージャー] モジュールを使用する必要があります。 
>
>Azure ポータルで作成した Automation アカウントは、どちらのポータルでも、また、どちらのセットのコマンド レットでも管理できます。 アカウントを作成してしまえば、そのアカウントのリソースの作り方や管理方法に違いはありません。 引き続き Azure ポータルを使用する場合、Automation アカウントの作成には Azure プレビュー ポータルではなく、Azure ポータルを使用することをお勧めします。


お使いに Azure アカウントで支払の遅延などの問題が発生した場合、一時的に Automation アカウントを停止することがあります。 その場合、アカウントへのアクセスができなくなり、実行中のジョブはすべて中断されます。また、すべてのスケジュールが無効になります。 アカウントの閲覧はできますが、アカウントに含まれるリソースを見ることはできません。 問題を解決して Automation アカウントが有効になった後は、スケジュールを有効にして、中断された Runbook を再開する必要があります。


## Azure リソースへの認証の構成

使用して Azure のリソースにアクセスするとき、 [Azure コマンドレット](http://msdn.microsoft.com/library/azure/jj554330.aspx), 、Azure サブスクリプションに認証を提供する必要があります。 Azure Automation では、これは、サブスクリプションの管理者が構成する Azure Active Directory の組織アカウントで行います。 作成できます、 [資格情報](http://msdn.microsoft.com/library/dn940015.aspx) このユーザーのアカウントおよびそれを使用 [Add-azureaccount](http://msdn.microsoft.com/library/azure/dn722528.aspx) runbook でします。
>[AZURE.NOTE] Microsoft アカウント (旧称 LiveID) を Azure Automation で使用することはできません。

## Azure サブスクリプションを管理するための新しい Azure Active Directory ユーザーの作成

1. 管理する Azure サブスクリプションのサービス管理者として Azure ポータルにログインします。
2. **[Active Directory]** を選択します。
3. Azure サブスクリプションと関連付けられているディレクトリ名を選択します。 必要であれば、**[設定]、[サブスクリプション]、[ディレクトリの編集]** で関連付けを変更できます。
4. [Active Directory ユーザーの新規作成](http://msdn.microsoft.com/library/azure/hh967632.aspx)します。 **[ユーザーの種類]** では、**[多要素認証の有効化]** ではなく **[組織内の新しいユーザー]** を選択します。
5. ユーザーの完全な名前と一時的なパスワードを書き留めておきます。
7. **[設定]、[管理者]、[追加]** の順に選択します。
8. 作成したユーザーの完全なユーザー名を入力します。
9. このユーザーで管理するサブスクリプションを選択してください。
10. ログアウトして、先ほど作成したアカウントで再度 Azure にログインする。 ユーザーのパスワードを変更するように促されます。
11. 新しい [Azure Automation の資格情報資産](http://msdn.microsoft.com/library/dn940015.aspx) 作成したユーザー アカウントのです。 **[資格情報の種類]** は、**[Windows PowerShell 資格情報]** にする必要があります。

## Automation アカウントの作成

Automation アカウントは、Azure Automation リソースのコンテナーで、 環境の分離や、ワークフローの詳細な整理のための方法を提供します。 オートメーション アカウントを既に作成している場合は、次の手順を省略できます。

1. ログイン、 [Azure プレビュー ポータル](https://portal.azure.com/)します。

2. Azure プレビュー ポータルで、**[新規]**、**[管理]**、**[Automation アカウント]** の順にクリックします。

3. **[Automation アカウントの追加]** ブレードで、Automation アカウントの詳細を構成します。

>[AZURE.NOTE] Azure プレビュー ポータルを使用して Automation アカウントを作成すると、アカウントおよびそれに関連付けられているすべてのリソースは従来の管理ポータルには戻りません。 

構成するパラメーターを次に示します。

| パラメーター| 説明|
|:---|:---|
| 名前| Automation アカウントの名前。一意の値でなければなりません。|
| リソース グループ| リソース グループを使用すると、関連する Azure リソースを簡単に表示および管理できます。Azure プレビュー ポータルでは、既存のリソース グループを選択することも、Automation アカウント用に新しいリソース グループを作成することもできます。一方、Microsoft Azure 管理ポータルでは、すべての Automation アカウントが既定のリソース グループに入れられます。|
| [サブスクリプション]| 使用可能なサブスクリプションの一覧から選択します。|
| リージョン| リージョンでは、アカウントの Automation リソースが格納される場所を指定します。一覧から任意のリージョンを選択できます。これは、アカウントの機能には影響しませんが、アカウントのリージョンがご使用のその他の Azure リソースが格納された場所に近い場合、Runbook をより素早く実行できます。|
| アカウント オプション| このオプションを使用すると、新しい Automation アカウントで作成されるリソースを選択できます。**[はい]** を選択すると、チュートリアル Runbook が作成されます。|

![アカウントの作成](media/automation-configuration/automation-01-create-automation-account.png)
>[AZURE.NOTE] 従来の管理ポータルで作成したオートメーション アカウントの場合は [別のリソース グループに移動する](../resource-group-move-resources.md) Azure プレビュー ポータルを使用して、オートメーション アカウントしなくなる Azure クラシック ポータルで、使用できるように、従来の管理ポータルでは、Azure リソース マネージャーのアカウントはサポートされていません。



## Runbook での資格情報の使用

使用して runbook で資格情報を取得する、 [Get-automationpscredential](http://msdn.microsoft.com/library/dn940015.aspx) アクティビティと共に使用して [Add-azureaccount](http://msdn.microsoft.com/library/azure/dn722528.aspx) 、Azure サブスクリプションに接続します。 資格情報は、管理者は、複数の Azure サブスクリプションのかどうかは、使用することも必要があります [Select-azuresubscription](http://msdn.microsoft.com/library/dn495203.aspx) 適切なものを指定します。 これについては、ほとんどの Azure Automation Runbook の先頭に一般に出現する以下のサンプル Windows PowerShell で示されています。

    $cred = Get-AutomationPSCredential –Name "myuseraccount.onmicrosoft.com"
    Add-AzureAccount –Credential $cred
    Select-AzureSubscription –SubscriptionName "My Subscription"

いずれかの後、次の行を繰り返す必要があります [チェックポイント](http://technet.microsoft.com/library/dn469257.aspx#bk_Checkpoints) runbook でします。 Runbook が中断された後、別のワーカーで再開した場合は、もう一度認証を実行する必要があります。

## 関連記事:

- [Azure オートメーション: Azure Active Directory を使用して Azure への認証](http://azure.microsoft.com/blog/2014/08/27/azure-automation-authenticating-to-azure-using-azure-active-directory/)






