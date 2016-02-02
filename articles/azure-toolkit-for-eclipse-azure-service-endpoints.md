<properties
    pageTitle="Azure サービス エンドポイント"
    description="Azure Toolkit for Eclipse での Azure サービス エンドポイントの設定について説明します。"
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="11/19/2015" 
    ms.author="robmcm"/>




# Azure サービス エンドポイント

Azure サービス エンドポイントは、アプリケーションがどこにデプロイされて管理されるかを決定します (グローバル Azure プラットフォーム、中国の 21Vianet が運営する Azure、プライベート Azure プラットフォームのいずれか)。  **[サービス エンドポイント]** ダイアログで、使用するサービス エンドポイントを指定できます。 **[サービスエンド ポイント]** ダイアログを開くには、Eclipse で **[Window]**、**[Preferences]** の順にクリックし、**[Azure]** を展開し、**[Service Endpoints]** をクリックします。 **[Active Set]** フィールドを設定することで、現在のワークスペース内の Azure プロジェクトで使用される Azure サービス エンドポイントが決まります。

**[Service Endpoints]** ダイアログを次に示します。

![][ic719493]

## サービス エンドポイントを設定するには

**[Service Endpoints]** ダイアログで、次のいずれかを行います。

* グローバル Azure プラットフォームを使用する場合は、**[Active Set]** ドロップダウン リストから **[windowsazure.com]** を選択し、**[OK]** をクリックします。
* 中国の 21Vianet が運営する Azure を使用する場合は、**[Active Set]** ドロップダウン リストから **[windowsazure.cn (China)]** を選択し、**[OK]** をクリックします。
* プライベート Azure プラットフォームを使用する場合:
    1. **[編集]** をクリックします。
    2. **[Service Endpoints]** ダイアログを閉じて基本設定ファイルを開くことを通知するダイアログ ボックスが表示されます。 **[OK]** をクリックします。
    3. Preferencesets.xml ファイルでは、新しい作成 `preferenceset` 要素。 この新しい要素に対して作成 `名`, 、`blob`, 、`管理`, 、`portalURL` と `publishsettings` 属性、および、プライベート Azure プラットフォームに対応するそれらの値を追加します。 既存の指定した値を使用する `preferenceset` テンプレートとしての要素。 **注**: 使用される値、 `blob` 属性は、URL には、"blob"というテキストを含める必要があります。
    4. preferencesets.xml を保存して閉じます。
    5. **[Service Endpoints]** ダイアログを再度開きます。
    6. **[Active Set]** ドロップダウン リストから、作成したアクティブ セットを選択し、**[OK]** をクリックします。
    7. プライベート Azure プラットフォームを作成した後 `preferenceset` 要素をクリックして、それに割り当てられている値を変更することができます、 **編集** ボタンをクリックして、 **サービス エンドポイント** ダイアログ。 また、複数のプライベート Azure プラットフォームを作成することもできます。 `preferenceset` 要素、必要な場合です。

## 関連項目

[Azure Toolkit for Eclipse の][]

[Azure Toolkit for Eclipse のをインストールします。][]

[Eclipse ので、Azure の Hello World アプリケーションを作成します。][]

Java で Azure を使用する方法の詳細については、次を参照してください。、 [Azure Java デベロッパー センターの []][]します。






[azure java developer center]: http://go.microsoft.com/fwlink/?LinkID=699547 
[azure toolkit for eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529 
[creating a hello world application for azure in eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533 
[installing the azure toolkit for eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546 
[ic719493]: ./media/azure-toolkit-for-eclipse-azure-service-endpoints/ic719493.png 

