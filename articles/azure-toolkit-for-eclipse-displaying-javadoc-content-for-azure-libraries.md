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




# Java 用 Azure ライブラリ パッケージの Javadoc コンテンツの Eclipse での表示

Java 用 Azure ライブラリの Javadoc コンテンツは、Javadoc コンテンツを Java 用 Azure ライブラリに関連付けることで、Eclipse 環境内で表示できます。 次の手順は、Eclipse 内でこの機能を使用する方法を示しています。

この手順は、Java 用 Azure ライブラリをビルド パスに既に追加していることを前提としています。

## Java 用 Azure ライブラリの Javadoc コンテンツを Eclipse で表示するには

* Eclipse の Project Explorer で、プロジェクトの **[Referenced Libraries]** セクションにある Java JAR 用 Azure ライブラリ (例: **microsoft Microsoft Azure api 0.1.0.jar** (インストールされているバージョンによって、バージョン番号が異なることがあります)) のコンテキスト メニューを開きます。
* **[プロパティ]** をクリックします。
* **[Properties]** ダイアログの左側のウィンドウで、**[Javadoc Location]** をクリックします。 **[Javadoc Location]** ダイアログが表示されます。
* **[Javadoc URL]** または **[Javadoc in archive]** を指定できます。
    * 指定する場合、 **Javadoc URL**, 、Url を使用して **http://dl.windowsazure.com/javadoc** または **http://dl.windowsazure.com/storage/javadoc**します。
    * **[Javadoc in archive]** を使用する場合は、外部ファイルまたはワークスペース ファイルを指定できます。
    選択したら、必要に応じて参照または検証します。 次の例は、対応する Javadoc JAR にという名前のフォルダーにローカルにダウンロードされたを Azure Libraries for Java を関連付けます **c:\MyAzureJARs**します。
    ![][ic553487]
* *省略可能*: **[Validate]** をクリックします。 Javadoc JAR に関する潜在的な問題があれば、ここに表示されます。
* **[OK]** をクリックします。

ライブラリに関連付けると、Javadoc コンテンツが Eclipse IDE 内で表示されます。 たとえば場合、 `blob` 型で定義される `CloudBlockBlob` 、コード内で入力するときに表示される Javadoc コンテンツの例を次に示します `blob.acquireLease` コードで。

![][ic553488]

## 関連項目

[Azure Toolkit for Eclipse の][]

[Eclipse ので、Azure の Hello World アプリケーションを作成します。][]

[Azure Toolkit for Eclipse のをインストールします。][]

Java で Azure を使用する方法の詳細については、次を参照してください。、 [Azure Java デベロッパー センターの []][]します。






[azure java developer center]: http://go.microsoft.com/fwlink/?LinkID=699547 
[azure toolkit for eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529 
[creating a hello world application for azure in eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533 
[installing the azure toolkit for eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546 
[ic553487]: ./media/azure-toolkit-for-eclipse-displaying-javadoc-content-for-azure-libraries/ic553487.png 
[ic553488]: ./media/azure-toolkit-for-eclipse-displaying-javadoc-content-for-azure-libraries/ic553488.png 

