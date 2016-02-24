<properties 
   pageTitle="Visual Studio でのリソース マネージャー テンプレートの編集 | Microsoft Azure"
   description="Visual Studio を使用して Azure リソース マネージャー テンプレートにリソースを追加する方法について説明します。"
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
   ms.date="11/13/2015"
   ms.author="tarcher" />

# Visual Studio でのリソース マネージャー テンプレートの編集

Visual Studio では、リソース グループのリソース マネージャー テンプレートを編集できます。 Visual Studio の [JSON アウトライン] ウィンドウでテンプレートを変更するか、テンプレートの構文を直接変更します。

## [JSON アウトライン] ウィンドウでのリソース グループへのリソースの追加

### リソース グループにリソースを追加するには

1. ソリューション エクスプローラーで、Azure リソース グループの JSON ファイルを選択します。 エディターに JSON ファイルが表示され、 **JSON のアウトライン** ウィンドウは、エディターの横にも表示されます。 [JSON アウトライン] ウィンドウを閉じると、JSON テンプレート ファイル (パラメーター ファイルではありません) のコンテキスト メニューで [アウトラインの表示] を選択するまでは、再度自動で開くことはありません。

    ![Azure リソース グループの JSON ファイル](./media/vs-azure-tools-resource-group-adding-resources/arm-json-file.png)

1.  **JSON のアウトライン** ウィンドウで、選択、 **リソース** ノード、いずれかをクリックして、 **新しいリソースの追加** コンテキスト メニューで、コマンドまたは選択、 **リソースの追加** JSON アウトライン] ウィンドウの上部にあるボタンです。

    ![リソース グループへの新しいリソースの追加](./media/vs-azure-tools-resource-group-adding-resources/arm-add-resource.png)

1. リソースの一覧で、 **リソースの追加** ] ダイアログ ボックスでは、追加、リソースが必要な情報を提供するリソースを選択し、選択、 **追加** ] ボタンをクリックします。 たとえば、ストレージ アカウントを追加する場合は、作成されるパラメーターのベース名を指定する必要があります。 
 
    ![[リソースの追加] ダイアログ ボックス](./media/vs-azure-tools-resource-group-adding-resources/arm-add-resource-dialog.png)

    リストにリソースがリソースに追加、 **JSON のアウトライン** ウィンドウ、および、新しい JSON を表すファイルで、リソースが表示されます。 関連するパラメーターや変数も追加される場合があります。


    ![Resource added to JSON file](./media/vs-azure-tools-resource-group-adding-resources/arm-add-resource-json.png)

1. Azure リソース グループに新しいリソースをデプロイします。 ソリューション エクスプ ローラーで、リソース グループ プロジェクトのコンテキスト メニューで、次のように選択します。 **展開** し、リソース グループの名前を選択します。 

    ![デプロイされた Azure リソース グループ](./media/vs-azure-tools-resource-group-adding-resources/deploy-arm-resource-group.png)

     **リソース グループにデプロイ** ] ダイアログ ボックスが表示されます。


1. 選択、 **展開** ] ボタンをクリックします。

1. すべてのパラメーターは、ユーザーによって指定する必要がある場合、 **パラメーターの編集** ] ダイアログ ボックスが表示されます。 必要な値を入力し、[、 **保存** ] ボタンをクリックします。 新しいリソースが Azure リソース グループにデプロイされます。

## テンプレートの構文の編集

Visual Studio では、テンプレートを直接編集することもできます。 テンプレートの値の編集を開始すると、指定できる値に関するヘルプがエディター内に表示されます。 

![テンプレートの編集](./media/vs-azure-tools-resource-group-adding-resources/arm-edit-template.png)

テンプレートの構造に関する詳細については、次を参照してください [Azure リソース マネージャーの作成テンプレート。](resource-group-authoring-templates.md)

## 関連項目

[Visual Studio での Azure リソース グループの作成とデプロイ](vs-azure-tools-resource-groups-deployment-project-create-deploy.md)

[Azure リソース マネージャー コマンドレット](https://msdn.microsoft.com/library/azure/dn757692.aspx)

[Azure リソース マネージャーでの Windows PowerShell の使用](../powershell-azure-resource-manager/)

[Channel9 ビデオ: Azure リソース マネージャー](http://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/DEV-B224#fbid=)


