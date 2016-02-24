<properties 
    pageTitle="Web サイトで ReportViewer を使用する | Microsoft Azure"
    description="このトピックでは、Microsoft Azure 仮想マシンに保存されたレポートを表示する Visual Studio ReportViewer コントロールを使用して、Microsoft Azure Web サイトを作成する方法を説明します。"
    services="virtual-machines"
    documentationCenter="na"
    authors="rothja"
    manager="jeffreyg"
    editor="monicar" 
    tags="azure-service-management" />
<tags 
    ms.service="virtual-machines"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="12/11/2015"
    ms.author="jroth" />

# Azure でホストされる Web サイトで ReportViewer を使用する

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] リソース マネージャーのモデルです。


Microsoft Azure 仮想マシンに保存されたレポートを表示する Visual Studio ReportViewer コントロールを使用して、Microsoft Azure Web サイトを作成できます。 ReportViewer コントロールは、ASP.NET Web アプリケーション テンプレートを使用して作成する Web アプリケーション内にあります。

>[AZURE.IMPORTANT] ASP.NET MVC Web アプリケーション テンプレートは、ReportViewer コントロールをサポートしていません。

ReportViewer を Microsoft Azure Web サイトに組み込むには、次のタスクを完了する必要があります。

- **追加** 展開パッケージにアセンブリ

- **構成** 認証と承認

- **発行** ASP.NET Web アプリケーションを Azure に

## 前提条件

「一般的な推奨事項とベスト プラクティス」のセクションを参照 [SQL Server Business Intelligence Azure Virtual Machines で](virtual-machines-sql-server-business-intelligence.md)します。

>[AZURE.NOTE] ReportViewer コントロールは Visual studio と Standard Edition 以上に含まれています。 インストールする必要があります Web Developer Express Edition を使用している場合、 [MICROSOFT REPORT VIEWER 2012 RUNTIME](https://www.microsoft.com/download/details.aspx?id=35747) 、ReportViewer ランタイム機能を使用します。
>
>Microsoft Azure では、ローカル処理モードで構成された ReportViewer はサポートされません。

ホワイト ペーパーを参照して [Reporting Services レポート ビューアー コントロールと Microsoft Azure 仮想マシン ベースのレポート サーバー](http://download.microsoft.com/download/2/2/0/220DE2F1-8AB3-474D-8F8B-C998F7C56B5D/Reporting%20Services%20report%20viewer%20control%20and%20Azure%20VM%20based%20report%20servers.docx)します。

## デプロイ パッケージにアセンブリを追加する

ASP.NET アプリケーションをオンプレミスでホストする場合、ReportViewer アセンブリは通常、Visual Studio のインストール時に IIS サーバーのグローバル アセンブリ キャッシュ (GAC) 内にインストールされるため、アプリケーションから直接アクセスが可能です。 ただし、ASP.NET アプリケーションをクラウドでホストする場合、Microsoft Azure では GAC へのインストールが許可されないため、ReportViewer アセンブリがアプリケーションでローカルで使用できるようにしておく必要があります。 このためには、プロジェクトにアセンブリへの参照を追加し、これらがローカルでコピーされるように構成します。

リモート処理モードでは、次のアセンブリが ReportViewer コントロールで使用されます。

- **Microsoft.ReportViewer.WebForms.dll**: ページで ReportViewer を使用する必要がある ReportViewer コードが含まれています。 プロジェクト内の ASP.NET ページに ReportViewer コントロールをドロップすると、このアセンブリへの参照がプロジェクトに追加されます。

- **Microsoft.ReportViewer.Common.dll**: 実行時に ReportViewer コントロールで使用されるクラスが含まれています。 自動的にはプロジェクトに追加されません。

### Microsoft.ReportViewer.Common に参照を追加する

- プロジェクトを右クリックして **参照** ノード **参照の追加**, [.NET] タブでアセンブリを選択し、クリックして、 **[ok]**します。

### ASP.NET アプリケーションからアセンブリにローカル アクセスできるようにする

1.  **参照** フォルダー内の Microsoft.ReportViewer.Common アセンブリをクリックして、プロパティ ペインでそのプロパティが表示されます。

1. プロパティ ペインで次のように設定します。 **ローカル コピー** True に設定します。

1. Microsoft.ReportViewer.WebForms についても、手順 1. と 2. を繰り返します。

### ReportViewer Language Pack を入手する

1. 適切な Microsoft Report Viewer 2012 Runtime 再頒布可能パッケージをインストール [Microsoft ダウンロード センター](http://go.microsoft.com/fwlink/?LinkId=317386)します。

1. ドロップダウン リストから言語を選択すると、ダウンロード センターの対応するページにリダイレクトされます。

1. クリックして **ダウンロード** ReportViewerLP.exe のダウンロードを開始します。

1. ReportViewerLP.exe をダウンロードした後] をクリックして **実行** 即座に、インストールするか、クリックして **保存** をコンピューターに保存します。 クリックすると **保存**, 、ファイルを保存するフォルダーの名前に注意してください。

1. ファイルを保存したフォルダーを見つけます。 ReportViewerLP.exe を右クリックし、をクリックして **管理者として実行**, 、クリックして **はい**します。

1. ReportViewerLP.exe を実行した後、c:\windows\assembly にリソース ファイルを表示、 **Microsoft.ReportViewer.Webforms.Resources** と **Microsoft.ReportViewer.Common.Resources**します。

### ローカライズされた ReportViewer コントロール用に構成する

1. 前の手順に従って、Microsoft Report Viewer 2012 Runtime 再頒布可能パッケージをダウンロードしてインストールします。

1. プロジェクトに <language> フォルダーを作成し、関連付けられたリソース アセンブリ ファイルをそこにコピーします。 コピーするリソース アセンブリ ファイル: **Microsoft.ReportViewer.Webforms.Resources.dll** と **Microsoft.ReportViewer.Common.Resources.dll**します。リソース アセンブリ ファイルを選択し、プロパティ ペインで次のように設定します。 **出力ディレクトリにコピー** に"**常にコピー**"です。

1. Web プロジェクトのカルチャと UI カルチャを設定します。 ASP.NET Web ページのカルチャおよび UI カルチャを設定する方法の詳細については、次を参照してください。 [方法: ASP.NET Web ページのグローバリゼーション用のカルチャおよび UI カルチャの設定](http://go.microsoft.com/fwlink/?LinkId=237461)します。

## 認証と承認を構成する

ReportViewer は、適切な資格情報を使用してレポート サーバーに対する認証を行う必要があります。必要なレポートにアクセスするには、その資格情報がレポート サーバーによって承認される必要があります。 認証については、ホワイト ペーパーを参照してください。 [Reporting Services レポート ビューアー コントロールと Microsoft Azure 仮想マシン ベースのレポート サーバー](https://msdn.microsoft.com/library/azure/dn753698.aspx)します。

## ASP.NET Web アプリケーションを Azure に発行する

ASP.NET Web アプリケーションを Azure に発行する方法については、次を参照してください。 [方法: 移行し、Visual Studio から Azure に Web アプリケーションを発行](../vs-azure-tools-migrate-publish-web-app-to-cloud-service.md) と [Web Apps と ASP.NET を使ってみる](../app-service-web/web-sites-dotnet-get-started.md)します。

>[AZURE.IMPORTANT] ソリューション エクスプ ローラーで、ショートカット メニューで、[Azure デプロイ プロジェクトまたは Azure クラウド サービス プロジェクトの追加] コマンドが表示されない場合は、プロジェクトのターゲット フレームワークを .NET Framework 4 に変更する必要があります。
>
>これら 2 つのコマンド機能は、実質的には同じです。 インストールされている Microsoft Azure SDK のバージョンに合わせて応じて、いずれか一方のコマンドがショートカット メニューに表示されます。

## リソース

[Microsoft レポート](http://go.microsoft.com/fwlink/?LinkId=205399)

[Azure Virtual Machines での SQL Server Business Intelligence](virtual-machines-sql-server-business-intelligence.md)

[ネイティブ モードのレポート サーバーを実行する Azure VM を PowerShell を使用して作成する](virtual-machines-sql-server-create-native-mode-report-server-powershell.md)

[Reporting Services のレポート ビューアー制御と Microsoft Azure 仮想マシン ベースのレポート サーバー](http://download.microsoft.com/download/2/2/0/220DE2F1-8AB3-474D-8F8B-C998F7C56B5D/Reporting%20Services%20report%20viewer%20control%20and%20Azure%20VM%20based%20report%20servers.docx)

