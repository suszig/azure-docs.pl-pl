<properties 
   pageTitle="クラウド エクスプローラーを使用した Azure リソースの管理"
   description="クラウド エクスプローラーを使用し、Visual Studio 内で Azure リソースを参照および管理する方法について説明します。"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/13/2015"
   ms.author="tarcher" />

# クラウド エクスプローラーを使用した Azure リソースの管理

##概要

クラウド エクスプローラーは、Visual Studio IDE 内で Azure リソースをより簡単かつ迅速に参照および管理できるように設計されています。 たとえば、これを使用すると、Azure ポータルまたはブラウザー内で Web アプリを開いたり、デバッガーをアタッチしたりできます。また、BLOB コンテナーのプロパティを表示して BLOB コンテナー エディターで開くことができます。

クラウド エクスプローラーは、Microsoft Azure プレビュー ポータルと同様に、Azure リソース マネージャー スタックに基づいて構築されています。 Azure リソース グループなどのリソースとロジック アプリと API アプリなどの Azure サービスを認識していると、サポート [ロールベースのアクセス制御](../role-based-access-control-configure/) (RBAC)。 Azure リソースの追加または変更されたことを確認する、 **更新** クラウド エクスプ ローラー ツールバーのボタンをクリックします。

クラウド エクスプローラーは、Visual Studio Tools for Azure SDK 2.7 の一部としてインストールされます。 

## 前提条件

- Visual Studio 2015 RTM。

- Visual Studio Tools for Azure SDK。 
- クラウド エクスプローラーに Azure リソースを表示するには、Azure アカウントを所有していて、そのアカウントにログインしている必要もあります。 お持ちでない場合でも、数分でアカウントを作成できます。 MSDN サブスクリプションがある場合は、次を参照してください。 [MSDN サブスクライバー向けの Azure 特典](http://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)します。 そうしないと、 [無料の試用アカウントを作成する](http://azure.microsoft.com/pricing/free-trial/)です。

- クラウド エクスプ ローラーが表示されない場合を選択して表示できる **ビュー**, 、**その他のウィンドウ** **クラウド エクスプ ローラー** メニュー バーでします。

## Azure アカウントと Azure サブスクリプションの管理

クラウド エクスプローラーで Azure リソースを表示するには、1 つ以上の有効なサブスクリプションを持つ Azure アカウントにログインする必要があります。 複数の Azure アカウントがある場合は、クラウド エクスプローラーでそれらを追加し、クラウド エクスプローラーのリソース ビューに追加するサブスクリプションを選択します。

これまでに Azure を使用したことがない場合または必要なアカウントを Visual Studio に追加していない場合は、この操作を行うように求められます。

## Azure アカウントをクラウド エクスプローラーに追加するには

1. クラウド エクスプローラーのツール バーの [設定] アイコンを選択します。

1. 選択、 **アカウントを追加する** リンクします。 参照するリソースを所有する Azure アカウントにログインします。 ここで追加したアカウントは、アカウントを選択するドロップダウン リストで選択する必要があります。 選択したアカウントのサブスクリプションが、アカウント項目の下に表示されます。

    ![Adding Azure subscriptions](./media/vs-azure-tools-resources-managing-with-cloud-explorer/IC819514.png)

    ![Choosing Azure subscriptions](./media/vs-azure-tools-resources-managing-with-cloud-explorer/IC819515.png)

1. 参照し、選択するアカウントのサブスクリプションのチェック ボックスをオン、 **適用** ] ボタンをクリックします。

    選択したサブスクリプションの Azure リソースがクラウド エクスプローラーに表示されます。

## Azure アカウントを削除するには

1. 選択 **ファイル**, 、**アカウント設定** メニュー バーでします。

1.  **のすべてのアカウント** のセクションで、 **アカウント設定** ] ダイアログ ボックスで、選択、 **削除** を削除するアカウントの横にあるコマンドです。 このコマンドは、Visual Studio からアカウントを削除するだけです。Azure アカウント自体には影響しません。

## リソースの種類またはグループの表示

Azure リソースを表示する、いずれかを選択できる **リソースの種類** または **リソース グループ** 表示します。

![Resource view dropdown](./media/vs-azure-tools-resources-managing-with-cloud-explorer/IC819516.png)

- **リソースの種類** ビューも Azure ポータルで使用される一般的なビューには、web アプリ、ストレージ アカウント、仮想マシンなどのそれらの種類によって分類された Azure リソースを示しています。 これは、サーバー エクスプローラーで Azure リソースが表示されるしくみに似ています。

- [リソース グループ] ビューでは、Azure リソースが、関連付けられている Azure リソース グループで分類されます。

 
    A resource group is a bundle of Azure resources, typically used by a specific application. To learn more about Azure resource groups, see [Azure Resource Manager Overview](https://azure.microsoft.com/documentation/articles/resource-group-overview/).

## リソースの表示および移動

クラウド エクスプローラーで Azure リソースに移動してその情報を表示するには、項目の種類または関連付けられているリソース グループを展開して、リソースを選択します。 リソースを選択すると、クラウド エクスプローラーの下部にある 2 つのタブに情報が表示されます。

![Choose a resource view](./media/vs-azure-tools-resources-managing-with-cloud-explorer/IC819517.png)

-  **アクション** ] タブは、選択したリソースに対してクラウド エクスプ ローラーで実行できるアクションを示します。 リソースのショートカット メニューには、使用可能なアクションも表示されます。

-  **プロパティ** ] タブに関連付けられた、タイプ、ロケール、およびリソース グループなどのリソースのプロパティを示しています。

すべてのリソースがアクション **ポータルで開く**します。 このアクションを選択すると、クラウド エクスプローラーでは、Azure ポータルで選択したリソースが表示されます。 この機能は、深く入れ子になったリソースまで移動する場合に特に便利です。

その他のアクションやプロパティ値も、Azure リソースに基づいて表示される場合があります。 たとえば、web apps logic apps ともアクションを持っている、 **ブラウザーで開く** と **デバッガーをアタッチ** に加えて **ポータルで開く**します。 エディターを開くアクションは、ストレージ アカウントの BLOB、キュー、またはテーブルを選択すると表示されます。 Azure のアプリがある **URL** と **ステータス** 記憶域リソースが鍵と接続文字列プロパティのプロパティです。

## リソースの検索

Azure アカウントのサブスクリプションで特定の名前のリソースを検索するには、クラウド エクスプローラーの検索ボックスに名前を入力します。

![Finding resources in Cloud Explorer](./media/vs-azure-tools-resources-managing-with-cloud-explorer/IC820394.png)

検索ボックスに文字を入力すると、その文字に一致するリソースのみがリソース ツリーに表示されます。


