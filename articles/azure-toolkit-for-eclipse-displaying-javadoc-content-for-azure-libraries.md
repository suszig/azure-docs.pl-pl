<properties
    pageTitle="Java 用 Azure ライブラリ パッケージの Javadoc コンテンツの Eclipse での表示"
    description="Azure ライブラリの Javadoc コンテンツを Eclipse で表示する方法"
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
    ms.date="11/30/2015" 
    ms.author="robmcm"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh698319.aspx -->

# Java 用 Azure ライブラリ パッケージの Javadoc コンテンツの Eclipse での表示 #

Java 用 Azure ライブラリの Javadoc コンテンツは、Javadoc コンテンツを Java 用 Azure ライブラリに関連付けることで、Eclipse 環境内で表示できます。 次の手順は、Eclipse 内でこの機能を使用する方法を示しています。

この手順は、Java 用 Azure ライブラリをビルド パスに既に追加していることを前提としています。

## Java 用 Azure ライブラリの Javadoc コンテンツを Eclipse で表示するには ##

* Eclipse のプロジェクト エクスプ ローラーで、 **Referenced Libraries** セクションで、プロジェクトのコンテキスト メニューを開き、Azure ライブラリの Java の JAR をします。 たとえば、 **microsoft windows azure api 0.1.0.jar** (バージョン番号はインストールされているバージョンによっては、さまざまな可能性があります)。
* クリックして **プロパティ**します。
* 内で、 **プロパティ** ダイアログで左側のウィンドウでクリックして **Javadoc Location**します。  **Javadoc Location** ダイアログが表示されます。
* 指定できます、 **Javadoc URL**, 、または **Javadoc in archive**します。
    * 指定する場合、 **Javadoc URL**, 、Url を使用して **http://dl.windowsazure.com/javadoc** または **http://dl.windowsazure.com/storage/javadoc**します。
    * 使用する場合は、 **Javadoc in archive**, 、外部のファイルまたはワークスペース ファイルを指定することができます。
    選択したら、必要に応じて参照または検証します。 次の例は、対応する Javadoc JAR にという名前のフォルダーにローカルにダウンロードされたを Azure Libraries for Java を関連付けます **c:\MyAzureJARs**します。
    ![][ic553487]
* *省略可能な*: クリックして **検証**します。 Javadoc JAR に関する潜在的な問題があれば、ここに表示されます。
* Click **OK**.

ライブラリに関連付けると、Javadoc コンテンツが Eclipse IDE 内で表示されます。 たとえば、コードで `CloudBlockBlob` 型の `blob` が定義されている場合、コードに「`blob.acquireLease`」と入力すると、次のような Javadoc コンテンツが表示されます。

![][ic553488]

## 関連項目 ##

[Azure Toolkit for Eclipse][]

[Azure 向け Hello World アプリケーションを Eclipse で作成する][]

[Azure Toolkit for Eclipse のインストール][] 

Java で Azure を使用する方法の詳細については、次を参照してください。、 [Azure Java デベロッパー センター][]します。

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic553487]: ./media/azure-toolkit-for-eclipse-displaying-javadoc-content-for-azure-libraries/ic553487.png
[ic553488]: ./media/azure-toolkit-for-eclipse-displaying-javadoc-content-for-azure-libraries/ic553488.png
