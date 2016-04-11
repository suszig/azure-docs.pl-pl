<properties 
    pageTitle="Visual Studio 2015 を使用した Azure App Service での ASP.NET API アプリの作成" 
    description="Visual Studio 2015 を使用して、Azure App Service で ASP.NET API アプリを作成する方法について説明します。 " 
    services="app-service\api" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service-api" 
    ms.workload="web" 
    ms.tgt_pltfrm="dotnet" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="11/10/2015" 
    ms.author="tdykstra"/>

# Visual Studio 2015 を使用した Azure App Service での ASP.NET API アプリの作成

[AZURE.INCLUDE [app-service-api-v2-note](../../includes/app-service-api-v2-note.md)]

## 概要

このチュートリアルで作成する ASP.NET Web API 2 プロジェクトを使用して、 [Visual Studio 2015 RC](https://www.visualstudio.com/en-us/downloads/visual-studio-2015-downloads-vs.aspx), 、としてクラウドへのデプロイ用に構成し、 [API アプリ](app-service-api-apps-why-best-platform.md) で [Azure App Service](../app-service/app-service-value-prop-what-is.md)します。 Azure へのプロジェクトのデプロイも行います。 このチュートリアルの最後には、API アプリが Azure クラウドで実行するようになります。

このチュートリアルでは、Visual Studio でファイルおよびフォルダーを使用する方法を理解して **ソリューション エクスプ ローラー**します。 

このチュートリアルでは、現在リリースされているバージョンの ASP.NET Web API を使用します。  ASP.NET MVC 6 API アプリを作成する方法については、このブログの投稿を参照してください: [https://alexanderzeitler.com/articles/Deploying-a-ASP-NET-MVC-6-API-as-Azure-API-App-in-Azure-App-Services/](https://alexanderzeitler.com/articles/Deploying-a-ASP-NET-MVC-6-API-as-Azure-API-App-in-Azure-App-Services/ "、ASP.NET MVC 6 API を Azure App Service での Azure API アプリとして展開する")です。   

[AZURE.INCLUDE [install-sdk-2015-only](../../includes/install-sdk-2015-only.md)]

このチュートリアルでは、Azure SDK for .NET 2.6 以降のバージョンが必要です。

## API アプリ プロジェクトの作成 

Visual Studio 2015 RC にはまだ API アプリ プロジェクト テンプレートがないので、API アプリ プロジェクトを作成するには、Web API プロジェクト テンプレートから始めます。

1. Visual Studio 2015 RC を開きます。

2. クリックして **ファイル > [新しいプロジェクト**します。 

3. [ **テンプレート**, 、] をクリックして **Web**, 、クリックして、 **ASP.NET Web アプリケーション** テンプレートです。

4. プロジェクトに名前を *ContactsList*

5. 確認して、 **Application Insights をプロジェクトに追加** ] チェック ボックスがオフになっています。

5. Click **OK**.

    ![](./media/app-service-dotnet-create-api-app-vs2015/newproj.png)

6.  **新しい ASP.NET プロジェクト** ] ダイアログ ボックス [ **ASP.NET 4.6 テンプレート**, を選択、 **空** プロジェクト テンプレートです。

7. 選択、 **Web API** チェック ボックスをオンします。

8. クリア、 **クラウドでホスト** チェック ボックスをオンします。

7. Click **OK**.

    ![](./media/app-service-dotnet-create-api-app-vs2015/newaspnet.png)

## NuGet パッケージの追加

API アプリ用の App Service ランタイムがによって提供される、 [Microsoft.Azure.AppService.ApiApps.Service](http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service/) NuGet パッケージ、および動的 [Swagger](http://swagger.io/ "公式の Swagger 情報") によって API メタデータの生成が提供される、 [Swashbuckle](http://www.nuget.org/packages/Swashbuckle/) NuGet パッケージ。 

> **注:** Swashbuckle パッケージをインストールするときに、API テスト ページが既定で有効にします。 API アプリを発行し、設定のアクセスのレベルをかどうか **パブリック (匿名)**, 、API の呼び出しに使用して、テスト ページの URL を検索したユーザーことができます。  このチュートリアルの後半では、テスト ページを使用します。

1. クリックして **ツール > NuGet パッケージ マネージャー > パッケージ マネージャー コンソール**します。

2.  **パッケージ マネージャー コンソール** (PMC) で、次のコマンドを入力します。

        install-package Microsoft.Azure.AppService.ApiApps.Service
        install-package Swashbuckle

    PMC が依存関係をチェックしていることを示すメッセージが表示された後、数分待たなければならないことがあります。

## API アプリ メタデータ ファイルの追加

メタデータに API アプリが含まれているとして展開される Web API プロジェクトが、 *apiapp.json* ファイルと *メタデータ* フォルダーとそのサブフォルダーおよびファイルです。 次の手順では、既定値でこれらのファイルを追加します。 

 [API アプリのメタデータ](#api-app-metadata) チュートリアルの後でこのメタデータをカスタマイズする方法について説明します。

1. プロジェクト フォルダー内に作成、 *.json* apiapp.json という名前のファイルし、新しいファイルの内容を次の JSON テキストに置き換えます。

        {
            "$schema": "http://json-schema.org/schemas/2014-11-01/apiapp.json#",
            "id": "ContactsList",
            "namespace": "microsoft.com",
            "gateway": "2015-01-14",
            "version": "1.0.0",
            "title": "ContactsListTitle",
            "summary": "Summary",
            "author": "Author",
            "endpoints": {
                "apiDefinition": "/swagger/docs/v1",
                "status": null
            }
        }

3. プロジェクト フォルダーでという名前のフォルダーを作成 *メタデータ*, 、および、 *メタデータ* フォルダー、という名前のフォルダーを作成 *デプロイ Templates*します。

5.  *デプロイ Templates* ] フォルダーを作成、 *.json* という名前のファイル *apiappconfig.azureresource.json*, 、新しいファイルの内容を以下の JSON テキストに置き換えます。

        {
          "$schema": "http://schemas.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "$system": {
              "type": "Object"
            }
          },
          "resources": []
        }

## Web API コードの追加

次の手順では、ハード コーディングされた連絡先リストを返す単純な HTTP Get メソッドのコードを追加します。 

1. 作成、 *モデル* 存在しない場合は、プロジェクト フォルダー内のフォルダーです。

2.  *モデル* フォルダー、という名前のクラス ファイルを追加 *Contact.cs*, 、ファイルの内容を次のコードに置き換えます。 
        namespace ContactsList.Models
        {
            public class Contact
            {
                public int Id { get; set; }
                public string Name { get; set; }
                public string EmailAddress { get; set; }
            {
        }

5. 右クリックし、 **コント ローラー** フォルダー、および選択 **追加 > コント ローラー**します。 

    ![](./media/app-service-dotnet-create-api-app-vs2015/05-new-controller-v3.png)

6.  **スキャフォールディングの追加** ダイアログ ボックスで、選択、 **Web API 2 コント ローラー - 空** ] をクリックし、 **追加**します。 

    ![](./media/app-service-dotnet-create-api-app-vs2015/06-new-controller-dialog-v3.png)

7. コント ローラーに名前 **ContactsController**, 、] をクリック **追加**します。 

    ![](./media/app-service-dotnet-create-api-app-vs2015/07-new-controller-name-v2.png)

8. 1 回、 *ContactsController.cs* ファイルが作成されたら、ファイルの内容を次のコードに置き換えます。 

        using ContactsList.Models;
        using System;
        using System.Collections.Generic;
        using System.Linq;
        using System.Net;
        using System.Net.Http;
        using System.Threading.Tasks;
        using System.Web.Http;
        
        namespace ContactsList.Controllers
        {
            public class ContactsController : ApiController
            {
                [HttpGet]
                public IEnumerable<Contact> Get()
                {
                    return new Contact[]{
                        new Contact { Id = 1, EmailAddress = "barney@contoso.com", Name = "Barney Poland"},
                        new Contact { Id = 2, EmailAddress = "lacy@contoso.com", Name = "Lacy Barrera"},
                        new Contact { Id = 3, EmailAddress = "lora@microsoft.com", Name = "Lora Riggs"}
                    };
                }
            }
        }

## Web API のテスト

API のテスト ページを表示するには、次の手順に従います。

1. アプリケーションをローカルで実行し (CTRL キーを押しながら f5 キーを押します)、ブラウザーのアドレス バーで URL の末尾に `/swagger` を追加します。 

    ![](./media/app-service-dotnet-create-api-app-vs2015/14-swagger-ui.png)

2. をクリックして **連絡先 > 取得 > 試してみる**, 、API が機能しているおり、期待される結果を返すことを確認します。 

    ![](./media/app-service-dotnet-create-api-app-vs2015/15-swagger-ui-post-test.png)

## Azure での API アプリの作成

1. API アプリを作成、 [Azure プレビュー ポータル](https://portal.azure.com)します。 

    * クリックして **新規作成 > Web + モバイル > API アプリ**します。

        ![](./media/app-service-dotnet-create-api-app-vs2015/createapiapp1.png)

    *  **名** ContactsList を入力します。

    *  **App Service プラン** ] をクリックして **新規作成** し、たとえば、名前を入力してください: **ContactsList**します。

        App Service プランに関する詳細については、次を参照してください。 [Azure App Service プランの詳細な概要](azure-web-sites-web-hosting-plans-in-depth-overview.md)します。 

    * をクリックして **料金レベル > のすべてを表示 > 空き > 選択** 無料の価格レベルを選択します。

        有料の料金レベルを使用できますが、このチュートリアルでは必要ありません。

    *  **リソース グループ**, 、] をクリックして **新規作成** し、たとえば、名前を入力してください: ContactsList です。

        リソース グループの詳細については、次を参照してください。 [リソース グループを使用した Azure リソースの管理に](resource-group-overview.md)します。

    * 複数のサブスクリプションがある場合は、使用するサブスクリプションを選択します。

    * 近くの場所を選択します。

    * クリックして **作成**します。

        ![](./media/app-service-dotnet-create-api-app-vs2015/createapiapp2.png)

2. Azure が終了する際、API アプリ セットを作成する API アプリのアクセス レベルを **パブリック (匿名)**します。

    * クリックして **参照 > リソース グループ > [作成したリソース グループ] > [作成した API アプリ]**します。

    * クリックして **設定 > アプリケーション設定**します。

    * 変更 **アクセス レベル** に **パブリック (匿名)**します。
     
    * クリックして **保存**します。

        ![](./media/app-service-dotnet-create-api-app-vs2015/setpublicanon.png)
    
2. API アプリをホストしている基盤の Web アプリの名前を書き留めておきます。 Visual Studio プロジェクトをデプロイするときにこれを使用します。 

    *  **API アプリ ホスト**, 、クリックして **ContactsList**します。

        ![](./media/app-service-dotnet-create-api-app-vs2015/clickapiapphost.png)

    * 名前は、のタイトル、 **API アプリ ホスト** ブレードです。

        ![](./media/app-service-dotnet-create-api-app-vs2015/apiapphostname.png)

## Azure で新しい API アプリに Web API プロジェクトをデプロイします。
 
API アプリとは本質的に、Azure が Web サービス機能用の追加機能を提供する Web アプリです。 Visual Studio 2015 RC では、Web の発行ウィザードに API アプリ専用の選択肢がないため、API アプリの基になっている Web アプリに対して発行します。

2. Visual Studio で **ソリューション エクスプ ローラー**, をプロジェクトを右クリックし、コンテキスト メニューをクリックして **発行**します。

3.  **プロファイル** のステップ、 **Web の発行** ウィザード] をクリックして **Microsoft Azure Web Apps**します。

    ![](./media/app-service-dotnet-create-api-app-vs2015/pubwebselwebapp.png)

4.  **既存の Web Apps** ドロップダウン リストで、[API アプリを持つエントリ前にメモしたものと名前します。

    ![](./media/app-service-dotnet-create-api-app-vs2015/pubwebselapiapp.png)

5. クリックして **発行**します。

    ブラウザーで Web アプリの URL が開かれ、「API アプリが作成されました」ページが表示されます。

6. ブラウザーのアドレス バーで、URL の最後に "swagger/" を追加します。次に例を示します。

        https://microsoft-apiappb001b62a9033493a33748332233fca2.azurewebsites.net/swagger/

    前にローカルで実行したときと同じ Swagger UI が表示されますが、今度はクラウドで実行しています。

2. をクリックして **連絡先 > 取得 > 試してみる**, 、API が機能しているおり、期待される結果を返すことを確認します。 

    ![](./media/app-service-dotnet-create-api-app-vs2015/runninginazure.png)

## Azure プレビュー ポータルでの API 定義の表示

このセクションでは、ポータルに移動し、先ほど作成した API アプリの API の定義を表示します。

1.  [Azure プレビュー ポータル](https://portal.azure.com), に移動し、 **API アプリ** API アプリのブレード: をクリックして **参照 > リソース グループ > [作成したリソース グループ] > [作成した API アプリ]**します。

4. クリックして **API の定義**します。 

    アプリの **API の定義** ブレードは、アプリケーションを作成したときに定義した API 操作の一覧を表示します。 (このチュートリアルに従った場合には、get 操作のみが表示されます。) 

    ![API の定義](./media/app-service-dotnet-create-api-app-vs2015/29-api-definition-v3.png)

## Web API コードに操作を追加します。

5. Visual Studio でプロジェクトに戻り、次のコードを追加、 **ContactsController.cs** ファイルです。 このコードを追加、 **投稿** 新規投稿に使用できるメソッドを `Contact` インスタンスを API です。  

        [HttpPost]
        public HttpResponseMessage Post([FromBody] Contact contact)
        {
            // todo: save the contact somewhere
            return Request.CreateResponse(HttpStatusCode.Created);
        }

    ![コントローラーへの Post メソッドの追加](./media/app-service-dotnet-create-api-app-vs2015/30-post-method-added-v3.png)

6. 先ほどと同じように、プロジェクトを発行します (で **ソリューション エクスプ ローラー**, プロジェクトを右クリックし、クリックして、 **発行**, 、] をクリックし、 **発行** で、 **Web の発行** ウィザード)。

12. 発行プロセスが完了したら、ポータルに戻り、先ほどと同じようにゲートウェイを再起動します。

14. ポータルに戻り、 **API の定義** ブレードです。 

    先ほど作成して Azure サブスクリプションにデプロイした新しい API エンドポイントが表示されます。

    ![API の定義](./media/app-service-dotnet-create-api-app-vs2015/38-portal-with-post-method-v4.png)

[AZURE.INCLUDE [app-service-api-direct-deploy-metadata](../../includes/app-service-api-direct-deploy-metadata.md)]

## 次のステップ

Visual Studio 2015 RC を使用し、API アプリを作成してデプロイしました。  API アプリの詳細については、ページの左側 (ワイド ブラウザー ウィンドウの場合) またはページの上部 (幅の狭いブラウザー ウィンドウの場合) に表示される、ナビゲーション ウィンドウのエントリを参照してください。 API アプリ ドキュメントのほとんどでは Visual Studio 2013 が示されていますが、UI は似ていて、記述するコードおよびポータルの UI は同じなので、ほとんどは VS 2015 でも使用できます。
 

