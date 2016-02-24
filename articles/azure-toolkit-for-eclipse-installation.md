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

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690946.aspx -->

# Azure Toolkit for Eclipse のインストール #

Azure Toolkit for Eclipse は、Eclipse 開発環境を使って Azure アプリケーションを簡単に作成、開発、テスト、またデプロイできるテンプレートと機能を提供します。 これはオープン ソース プロジェクトであり、そのソース コードは、GitHub のプロジェクト サイトから Apache License 2.0 で入手できます。URL は次のとおりです。

<https://github.com/MSOpenTech/WindowsAzureToolkitForEclipseWithJava>.

次の手順は、Azure Toolkit for Eclipse をインストールする方法を示しています。

## 前提条件 ##

* Eclipse IDE for Java EE Developers Indigo 以降。 これはからダウンロードできる <http://www.eclipse.org/downloads/>します。
* Java Developer Kit (JDK) v1.7 以降。 
* オペレーティング システム - Azure Toolkit for Eclipse は、次のオペレーティング システムでテストされています。
    * Windows 10
    * Windows 8 および Windows 8.1
    * Windows 7
    * Windows Server 2012
    * Windows Server 2008

> [AZURE.IMPORTANT] Azure Toolkit for Windows で Eclipse を使用している場合、ツールキットでは、Azure SDK 2.7 以降をインストールする必要があります。 Azure SDK をインストールするためのオプションは 2 つあります。
> 
> * ダウンロードして使用して、Azure SDK をインストール、 [Web Platform Installer (WebPI)][]します。
> * 最初の Azure デプロイ プロジェクトを作成するときに Azure SDK がインストールされていない場合は、必要なバージョンの Azure SDK が自動的にダウンロードされインストールされることを示すメッセージが表示されます。
> 
> Azure SDK は Windows でのみ必要です。

## Azure Toolkit for Eclipse をインストールするには ##

1. Eclipse を起動します。
2. Eclipse のメニューで [ <strong>ヘルプ</strong>, 、クリックして <strong>Install New Software</strong>, 次の図に示すように、します。
    ![][ic590123]
3. [ <strong>利用可能なソフトウェア</strong> ダイアログ内で、 <strong>使用します。</strong> テキスト ボックスに入力 <strong>http://dl.msopentech.com/eclipse</strong> 続いて、 <strong>次に、</strong> キー。
4. <strong>[Name]</strong> ウィンドウで、<strong>[Azure Toolkit for Eclipse]</strong> をオンにし、<strong>[Contact all update sites during install to find required software]</strong> をオフにします。 画面は、次のように表示されます。
    ![][ic719482]
5. 展開するかどうかは、 <strong>Azure Toolkit for Eclipse</strong>で、次の項目が表示されます。
    * **Azure の Access Control Services Filter**: このコンポーネントは、Azure ACS でのアプリケーションのユーザーを認証するためのサポートを提供します。
    * **Azure の一般的なプラグイン**: このコンポーネントが、共有が含まれる他のコンポーネントが依存して機能します。
    * **Azure Toolkit for Eclipse**: このコンポーネントには、プロジェクト構成ロジック、クラウドへの発行ウィザード、およびユーザー インターフェイスが含まれています。
    * **Microsoft JDBC Driver 4.0 for SQL Server**: このコンポーネントは、SQL データベースを使用したアプリケーション開発を簡略化します。
    * **Apache Qpid Client Libraries for JMS 用のパッケージ**: このコンポーネントは、Azure で高度な Messaging Queuing PROTOCOL ベースのメッセージングを使用するアプリケーションを有効にする、Apache Qpid プロジェクトからの JMS クライアント ライブラリを提供します。
    * **Azure libraries for Java のパッケージ**: このコンポーネントでは、Azure のスケーラブルなクラウド コンピューティング リソースを活用するための Java で Azure アプリケーションを構築することができます。
    * **Java 用 application Insights プラグイン**: このコンポーネントでは、アプリケーションとサーバー インスタンスの Azure の利用統計情報ログ記録および分析サービスを使用することができます。
6. クリックして **次**します。 (このツールキットをインストールする場合に異常な遅延が発生する場合は、確認 **必要なソフトウェアを検索するインストール中に更新プログラムのすべてのサイトと接続** がオフになっています)。
7.  **インストールの詳細** ダイアログ ボックスで、をクリックして **次**します。
8.  **Review Licenses** ] ダイアログ ボックスで、使用許諾契約の条項を確認します。 ライセンス契約の条項に同意する場合はクリックして **、使用許諾契約の条項に同意** 順にクリック **完了**です。 (残りの手順は、ライセンス契約の条項に同意したことを前提としています。 ライセンス契約の条項に同意しない場合はインストール プロセスを終了します。)
9. インストールの完了に Eclipse を再起動するメッセージが表示されたら、クリックして **今すぐ再起動**します。

## 関連項目 ##

[Azure Toolkit for Eclipse][]

[Azure 向け Hello World アプリケーションを Eclipse で作成する][]

[Azure Toolkit for Eclipse の新機能][]

Java で Azure を使用する方法の詳細については、次を参照してください。、 [Azure Java デベロッパー センター][]します。

<!-- URL List -->

[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[Web Platform Installer (WebPI)]: http://go.microsoft.com/fwlink/?LinkID=252838 
[What's New in the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699552

<!-- IMG List -->

[ic590123]: ./media/azure-toolkit-for-eclipse-installation/ic590123.png
[ic719482]: ./media/azure-toolkit-for-eclipse-installation/ic719482.png

