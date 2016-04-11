<properties 
    pageTitle=".NET を使用して Media Services 開発を行うためにコンピューターをセットアップする方法" 
    description="Media Services SDK for .NET を使用した Media Services の前提条件について説明します。 Visual Studio アプリケーションを作成する方法についても説明します。" 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="10/05/2015"
    ms.author="juliako"/>

#.NET を使用した Media Services 開発

[AZURE.INCLUDE [media-services-selector-setup](../../includes/media-services-selector-setup.md)]

このトピックでは、.NET を使用して Media Services アプリケーションを開発する方法について説明します。

 **Azure Media Services .NET SDK** ライブラリでは、.NET を使用して Media Services に対するプログラミングすることができます。 .NET では、使用して開発を非常に簡単に、 **Azure Media Services .NET SDK Extensions** ライブラリが提供されています。 このライブラリには、.NET コードの簡素化に役立つ一連の拡張メソッドとヘルパー関数が含まれています。 どちらのライブラリから入手 **NuGet** と **GitHub**します。


##前提条件

-   新規または既存の Azure サブスクリプションで作成した Media Services アカウント。 トピックを参照して [Media Services アカウントを作成する方法](media-services-create-account.md)します。
-   オペレーティング システム: Windows 10、Windows 7、Windows Server 2008 R2、または Windows 8。
-   .NET Framework 4.5。
-    Visual Studio 2015、Visual Studio 2013、Visual Studio 2012、または Visual Studio 2010 SP1 (Professional、Premium、Ultimate、または Express)。


##Visual Studio プロジェクトの作成と構成

このセクションでは、Media Services 開発用に Visual Studio でプロジェクトを作成し、セットアップする方法を説明します。  ここでは C# Windows コンソール アプリケーション プロジェクトを使用していますが、Media Services アプリケーション用に作成できる他の種類のプロジェクト (Windows フォーム アプリケーション、ASP.NET Web アプリケーションなど) についても、同じセットアップ手順を利用できます。

このセクションでは、使用する方法を示しています。 **NuGet** Media Services .NET SDK と依存するその他のライブラリを追加します。

または、GitHub から最新の Media Services .NET SDK バージョンを入手できます ([github.com/Azure/azure-sdk-for-media-services](https://github.com/Azure/azure-sdk-for-media-services) と [github.com/Azure/azure-sdk-for-media-services-extensions](https://github.com/Azure/azure-sdk-for-media-services-extensions))、ソリューションをビルドし、クライアント プロジェクトに参照を追加します。 必要なすべての依存関係が、自動的にダウンロードされ抽出されることにご注意ください。

1. Visual Studio 2013、Visual Studio 2012 か Visual Studio 2010 SP1 で、C# の新しいコンソール アプリケーションを作成します。 入力、 **名前**, 、**場所**, 、および **ソリューション名**, 、し、[OK] をクリックします。

2. ソリューションをビルドします。

2. 使用 **NuGet** をインストールして追加 **Azure Media Services .NET SDK Extensions**します。 このパッケージをインストールするとインストールも **Media Services .NET SDK** し、必要な他のすべての依存関係を追加します。
1. 最新バージョンの NuGet がインストールされていることをご確認ください。 詳細情報とインストール手順については、次を参照してください。 [NuGet](http://nuget.codeplex.com/)します。

2. ソリューション エクスプローラーでプロジェクトの名前を右クリックし、[NuGet パッケージの管理…] を選択します。

[NuGet パッケージの管理] ダイアログ ボックスが表示されます。

3. オンライン ギャラリーで、Azure Media Services Extensions を検索し、[Azure Media Services .NET SDK Extensions] を選択し、[インストール] をクリックします。

プロジェクトが変更され、Media Services .NET SDK Extensions、Media Services .NET SDK、およびその他の依存アセンブリへの参照が追加されます。

4. よりクリーンな開発環境を実現するために、NuGet パッケージの復元を有効にすることを検討してください。 詳細については、次を参照してください。 [NuGet パッケージの復元"](http://docs.nuget.org/consume/package-restore)します。

3. 参照を追加 **System.Configuratio**アセンブリ。 このアセンブリには、System.Configuration が含まれています**。ConfigurationManager** クラス構成ファイル (App.config など) にアクセスするために使用します。

[参照の管理] ダイアログを使用して参照を追加するには、次の操作を行います。

1. ソリューション エクスプローラーで、プロジェクト名を右クリックします。 次に、[追加と参照] を選択します。

[参照の管理] ダイアログが表示されます。

2. .NET framework のアセンブリで、System.Configuration アセンブリを探して選択します。
3. [OK] をクリックします。


4. App.config ファイルを開きます (追加、ファイルをプロジェクトに既定では追加されていない場合) を追加し、 *appSettings* セクションをファイルにします。 Azure Media Services のアカウント名とアカウント キーの値を設定します。次の例をご覧ください。

取得する、 **アカウント名** と **アカウント キー** については、開いている、 **Azure Classic Portal**, 、media services のアカウントを選択し、クリックして、 **キーの管理** ] ボタンをクリックします。


<configuration>
    ...
      <appSettings>
        <add key="MediaServicesAccountName" value="Media-Services-Account-Name" />
        <add key="MediaServicesAccountKey" value="Media-Services-Account-Key" />
      </appSettings>
      
    </configuration>

5. Program.cs ファイルの先頭にある既存のステートメントを次のコードで上書きします。

        using System;
        using System.Collections.Generic;
        using System.Linq;
        using System.Text;
        using System.Threading.Tasks;
        using System.Configuration;
        using System.Threading;
        using System.IO;
        using Microsoft.WindowsAzure.MediaServices.Client;

これで、Media Services アプリケーションの開発準備が整いました。    



##Media Services のラーニング パス

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##フィードバックの提供

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

