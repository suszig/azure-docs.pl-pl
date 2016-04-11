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

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn268600.aspx -->

# Azure サービス エンドポイント #

Azure サービス エンドポイントは、アプリケーションがどこにデプロイされて管理されるかを決定します (グローバル Azure プラットフォーム、中国の 21Vianet が運営する Azure、プライベート Azure プラットフォームのいずれか)。  **サービス エンドポイント** ダイアログ ボックスで使用するサービス エンドポイントを指定できます。 開くには、 **サービス エンドポイント** ダイアログで、Eclipse 内でクリックして **ウィンドウ**, 、] をクリックして **設定**, 、展開 **Azure**, 、] をクリックし、 **サービス エンドポイント**します。 設定、 **アクティブ セット** フィールドは、現在のワークスペース内の Azure プロジェクトの Azure サービス エンドポイントの使用を決定します。

次に示す、 **サービス エンドポイント** ダイアログ。

![][ic719493]

## サービス エンドポイントを設定するには ##

 **サービス エンドポイント** ダイアログで、次のいずれかを行います。

* グローバル Azure プラットフォームを使用するかどうか、 **アクティブ セット** ドロップダウン リストで、 **windowsazure.com** ] をクリック **OK**します。
* 中国の 21 vianet が運営する Azure を使用するかどうか、 **アクティブ セット** ドロップダウン リストで、 **windowsazure.cn (中国)** ] をクリック **OK**します。
* プライベート Azure プラットフォームを使用する場合:
    1. クリックして **編集**します。
    2. 通知するダイアログ ボックスが表示される、 **サービス エンドポイント** ダイアログが閉じ、基本設定セット ファイルが表示されます。 Click **OK**.
    3. Preferencesets.xml ファイルに、新しい `preferenceset` 要素を作成します。 この新しい要素の `name`、`blob`、`management`、`portalURL`、および `publishsettings` 属性を作成し、各属性にプライベート Azure プラットフォームに該当する値を追加します。 既存の `preferenceset` 要素に指定されている値をテンプレートとして使用できます。 **注**: 使用される値、 `blob` 属性は、URL には、"blob"というテキストを含める必要があります。
    4. preferencesets.xml を保存して閉じます。
    5. 再度開く、 **サービス エンドポイント** ダイアログ。
    6.  **アクティブ セット** ドロップダウン リストで、アクティブな設定を作成しをクリックして、[ **OK**します。
    7. プライベート Azure プラットフォームを作成した後 `preferenceset` 要素をクリックして、それに割り当てられている値を変更することができます、 **編集** ボタンをクリックして、 **サービス エンドポイント** ダイアログ。 必要であれば、複数のプライベート Azure プラットフォームの `preferenceset` 要素を作成することもできます。

## 関連項目 ##

[Azure Toolkit for Eclipse][]

[Azure Toolkit for Eclipse のインストール][] 

[Azure 向け Hello World アプリケーションを Eclipse で作成する][]

Java で Azure を使用する方法の詳細については、次を参照してください。、 [Azure Java デベロッパー センター][]します。

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic719493]: ./media/azure-toolkit-for-eclipse-azure-service-endpoints/ic719493.png

