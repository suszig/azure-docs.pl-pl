<properties 
    pageTitle="Resources, roles and access control in Application Insights" 
    description="組織の insights の所有者、共同作成者、閲覧者。" 
    services="application-insights" 
    documentationCenter=""
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/01/2015" 
    ms.author="awills"/>
 
# Application Insights のリソース、ロール、アクセス制御

ユーザーに読み取りし、Visual Studio で、データに対する更新アクセスを制御できます [Application Insights][start], を使用して [Microsoft Azure のロールベースのアクセス制御](../role-based-access-control-configure.md)します。

> [AZURE.IMPORTANT] アクセスのユーザーを割り当てる、 **リソース グループまたはサブスクリプション** に、アプリケーション リソースが属するリソース自体ではありません。 割り当てる、 **Application Insights コンポーネント共同作成者** ロールです。 これにより、Web テストとアラート、アプリケーション リソースのアクセス制御が統一されます。 [詳細については](#access)です。


## リソース、グループ、サブスクリプション

最初に、いくつかの定義を紹介します。

* **Resource** - An instance of a Microsoft Azure service. Application Insights リソースはアプリケーションから送られてくる利用統計情報データを収集し、分析し、表示します。  その他の種類の Azure リソースには Web アプリ、データベース、VM があります。 

    すべてのリソースを表示するには、 [Azure ポータル][portal], 、サインインし、[参照] をクリックします。

    ![[参照] を選択し、[すべて] と [Application Insights でフィルター処理] のいずれかを選択します。](./media/app-insights-resources-roles-access-control/10-browse.png)

<a name="resource-group"></a>

* [**リソース グループ**][group] -すべてのリソースが 1 つのグループに属しています。 関連リソースを管理するとき、特にアクセス制御の場合、グループは便利な方法です。 たとえば、1 つのリソース グループに Web アプリ、そのアプリを監視する Application Insights リソース、エクスポートされたデータを保持する Storage リソースを配置できます。

    
    ![Choose Browse, Resource groups, then choose a group](./media/app-insights-resources-roles-access-control/11-group.png)

* [**サブスクリプション**](https://manage.windowsazure.com) - Application Insights またはその他の Azure リソースを使用して Azure サブスクリプションにサインインします。 すべてのリソース グループが 1 つの Azure サブスクリプションに属します。そのサブスクリプションで価格パッケージを選択します。会社のサブスクリプションの場合、メンバーとそのアクセス許可を選択します。 
* [**Microsoft アカウント**][account] -ユーザー名とパスワードを使用して、Microsoft Azure サブスクリプション、XBox Live、Outlook.com、およびその他の Microsoft サービスにサインインします。


## <a name="access"></a> リソース グループのアクセスの制御

アプリケーションに作成したリソースに加え、アラートと Web テストには非表示の個別リソースもあることを理解することが重要です。 同じアタッチ [リソース グループ](#resource-group) 、アプリケーションとします。 Web サイトやストレージなど、他の Azure サービスをそこに配置することもあります。

![Application Insights のリソース](./media/app-insights-resources-roles-access-control/00-resources.png)

これらのリソースのアクセスを制御するには、次の処置が推奨されます。

* アクセスを制御、 **リソース グループまたはサブスクリプション** レベルです。
* 割り当てる、 **Application Insights コンポーネント共同作成者** ロールをユーザーにします。 この処置により、グループのその他のサービスのアクセス権を与えなくても、ユーザーは Web テスト、アラート、Application Insights リソースを編集できます。 

## 別のユーザーにアクセスを与えるには

サブスクリプションまたはリソース グループの所有者権限が必要です。

ユーザーが必要な [Microsoft アカウント][account]します。 個人と Azure Active Directory に定義されているユーザー グループにアクセスを供与できます。

#### リソース グループに移動する

そこでユーザーを追加します。

![アプリケーションのリソース ブレードで、Essentials を開き、リソース グループを開き、設定/ユーザーを選択します。 [追加] をクリックします。](./media/app-insights-resources-roles-access-control/01-add-user.png)

あるいは、もう 1 つ上のレベルに進み、ユーザーをサブスクリプションに追加できます。

#### ロールを選択する

![新しいユーザーの役割を選択します。](./media/app-insights-resources-roles-access-control/03-role.png)

ロール | In the resource group
---|---
Owner | Can change anything, including user access
Contributor | Can edit anything, including all resources
Application Insights Component contributor | Can edit Application Insights resources, web tests and alerts
Reader | Can view but not change anything

'Editing' includes creating, deleting and updating:

* Resources
* Web tests
* Alerts
* 連続エクスポート

#### ユーザーを選択する


![新しいユーザーの電子メール アドレスを入力します。 ユーザーを選択する](./media/app-insights-resources-roles-access-control/04-user.png)

対象のユーザーがディレクトリにない場合、Microsoft アカウントを持つユーザーを招待できます。 
(Outlook.com、OneDrive、Windows Phone、XBox Live などのサービスを利用している場合、Microsoft アカウントを持っています。)



## ユーザーと役割

* [Azure のロール ベースのアクセス制御](../role-based-access-control-configure.md)



<!--Link references-->

[account]: https://account.microsoft.com
[group]: ../azure-preview-portal-using-resource-groups.md
[portal]: http://portal.azure.com/
[start]: app-insights-overview.md

 
