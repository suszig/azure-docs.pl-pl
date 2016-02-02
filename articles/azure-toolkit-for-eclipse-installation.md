<properties
    pageTitle="Azure Toolkit for Eclipse のインストール"
    description="Azure Toolkit for Eclipse をインストールする方法について説明します。"
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




# Azure Toolkit for Eclipse のインストール

Azure Toolkit for Eclipse は、Eclipse 開発環境を使って Azure アプリケーションを簡単に作成、開発、テスト、またデプロイできるテンプレートと機能を提供します。 これはオープン ソース プロジェクトであり、そのソース コードは、GitHub のプロジェクト サイトから Apache License 2.0 で入手できます。URL は次のとおりです。

<https://github.com/MSOpenTech/WindowsAzureToolkitForEclipseWithJava>.

次の手順は、Azure Toolkit for Eclipse をインストールする方法を示しています。

## 前提条件

* Eclipse IDE for Java EE Developers Indigo 以降。これはからダウンロードできる <http://www.eclipse.org/downloads/>します。
* Java Developer Kit (JDK) v1.7 以降。
* オペレーティング システム - Azure Toolkit for Eclipse は、次のオペレーティング システムでテストされています。
    * Windows 10
    * Windows 8 および Windows 8.1
    * Windows 7
    * Windows Server 2012
    * Windows Server 2008

> [AZURE.IMPORTANT] Windows で Azure Toolkit for Eclipse を使用する場合は、Azure SDK 2.7 以降をインストールする必要があります。 Azure SDK をインストールするためのオプションは 2 つあります。
> 
> * ダウンロードしてインストール、Azure SDK を使用して、 [Web Platform Installer (WebPI)][]します。
> * Azure SDK を作成するときに、最初の Azure デプロイ プロジェクトを求められますを自動的にダウンロードをインストールする必要はない場合は、Azure SDK の必要なバージョンをインストールします。
> 
> Azure SDK は Windows でのみ必要です。

## Azure Toolkit for Eclipse をインストールするには

1. Eclipse を起動します。
2. Eclipse のメニューで [ <strong>ヘルプ</strong>, 、クリックして <strong>Install New Software</strong>, 次の図に示すように、します。
    ![][ic590123]
3. <strong>利用可能なソフトウェア</strong> ダイアログ内で、 <strong>使用</strong> テキスト ボックスに「 <strong>http://dl.msopentech.com/eclipse</strong> 続けて、 <strong>Enter</strong> キー。
4. <strong>名</strong> ] ウィンドウで、チェック <strong>Azure Toolkit for Eclipse</strong>, 、ボックスをオフに <strong>を見つける必要なソフトウェアのインストール中に更新プログラムのすべてのサイトと接続</strong>します。 画面は、次のように表示されます。
    ![][ic719482]
5. 展開する場合、 <strong>Azure Toolkit for Eclipse</strong>, 、次の項目が表示されます。
    * **Azure Access Control Services Filter**: このコンポーネントは、Azure ACS でのアプリケーション ユーザーの認証をサポートします。
    * **Azure Common Plugin**: このコンポーネントには、他のコンポーネントが依存する共有機能が含まれています。
    * **Azure Toolkit for Eclipse**: このコンポーネントには、プロジェクト構成ロジック、クラウドへの発行ウィザード、およびユーザー インターフェイスが含まれています。
    * **Microsoft JDBC Driver 4.0 for SQL Server**: このコンポーネントは、SQL Database を使用したアプリケーション開発を簡略化します。
    * **Package for Apache Qpid Client Libraries for JMS**: このコンポーネントは、アプリケーションが Azure 内で Advanced Messaging Queuing Protocol (AMQP) ベースのメッセージングを使用できるようにする、Apache Qpid プロジェクトの JMS クライアント ライブラリを提供します。
    * **Package for Azure Libraries for Java**: このコンポーネントによって、Azure のスケーラブルなクラウド コンピューティング リソースを活用できる Azure アプリケーションを Java でビルドできます。
    * **Application Insights Plugin for Java**: このコンポーネントによって、アプリケーションとサーバー インスタンスに対する Azure のテレメトリ ログと分析サービスを使用できます。
6. **[次へ]** をクリックします。 (ツールキットのインストール時に異常な遅延が発生する場合は、**[Contact all update sites during install to find required software]** がオフになっていることを確認してください。)
7. **[Install Details]** ダイアログで、**[Next]** をクリックします。
8. **[Review Licenses]** ダイアログで、ライセンス契約の条項を確認します。 ライセンス契約の条項に同意する場合は、**[I accept the terms of the license agreements]** をクリックし、**[Finish]** をクリックします。 (残りの手順は、ライセンス契約の条項に同意したことを前提としています。 ライセンス契約の条項に同意しない場合はインストール プロセスを終了します。)
9. インストールを完了するために Eclipse を再起動することを求めるメッセージが表示されたら、**[Restart Now]** をクリックします。

## 関連項目

[Azure Toolkit for Eclipse の][]

[Eclipse ので、Azure の Hello World アプリケーションを作成します。][]

[新機能 Azure のツールキットに Eclipse][]

Java で Azure を使用する方法の詳細については、次を参照してください。、 [Azure Java デベロッパー センターの []][]します。






[azure toolkit for eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529 
[azure java developer center]: http://go.microsoft.com/fwlink/?LinkID=699547 
[creating a hello world application for azure in eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533 
[installing the azure toolkit for eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546 
[web platform installer (webpi)]: http://go.microsoft.com/fwlink/?LinkID=252838 
[what's new in the azure toolkit for eclipse]: http://go.microsoft.com/fwlink/?LinkID=699552 
[ic590123]: ./media/azure-toolkit-for-eclipse-installation/ic590123.png 
[ic719482]: ./media/azure-toolkit-for-eclipse-installation/ic719482.png 

