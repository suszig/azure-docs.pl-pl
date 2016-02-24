<properties
    pageTitle="Azure App Service での API Apps のサービス プリンシパルの認証 |Microsoft Azure"
    description="Azure App Service でサービス間シナリオで API アプリを保護する方法について説明します。"
    services="app-service\api"
    documentationCenter=".net"
    authors="tdykstra"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-api"
    ms.workload="na"
    ms.tgt_pltfrm="dotnet"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="11/28/2015"
    ms.author="tdykstra"/>

# Azure App Service での API Apps のサービス プリンシパル認証

[AZURE.INCLUDE [app-service-api-get-started-selector](../../includes/app-service-api-get-started-selector.md)]

## 概要

このチュートリアルでは、Azure App Service の認証および承認機能を使用し、API アプリを保護する方法と、保護されている API アプリをサービス アカウントで使用する方法について説明します。 このチュートリアルで示した認証プロバイダーは Azure Active Directory です。また、クライアントと API はどちらも、API アプリで実行される ASP.NET Web API です。

## App Service での認証と承認

このチュートリアルで使用する認証機能の概要については、このシリーズの前のチュートリアルを参照してください。 [Azure App Service で API Apps の認証と承認](app-service-api-authentication.md)します。

## このチュートリアルの使い方

このチュートリアルをダウンロードしてでの API アプリを作成するサンプル アプリケーションの構築、 [、API Apps と ASP.NET の取得の最初のチュートリアル シリーズの開始](app-service-api-dotnet-get-started.md)します。

## CompanyUsers.API サンプル プロジェクト

 [ContactsList サンプル アプリケーション](https://github.com/Azure-Samples/app-service-api-dotnet-contact-list), 、CompanyUsers.API プロジェクトは、単純な Web API プロジェクトをハード コーディングされた連絡先リストを返す 1 つの Get メソッドが含まれています。 サービス間のシナリオを示すためは、ContactsList.API の Get メソッドは、CompanyContacts.API で、Get メソッドを呼び出すし、連絡先を取得し任意に独自のデータ ストアでは結合リストを返しますを追加します。

これが CompanyUsers.API の Get メソッドです。

        public async Task<IEnumerable<Contact>> Get()
        {
            var contacts = new Contact[]{
                        new Contact { Id = 1, EmailAddress = "nancy@contoso.com", Name = "Nancy Davolio"},
                        new Contact { Id = 2, EmailAddress = "alexander@contoso.com", Name = "Alexander Carson"}
                    };
        
            return contacts;
        }


また、これが CompanyContacts.API の呼び出し方と、返されたものを結果に追加する ContactsList.API の Get メソッドです。 (ここではわかりやすくするために一部のコードは省いています)。

        private async Task<IEnumerable<Contact>> GetContacts()
        {
            var contacts = await _storage.Get(FILENAME);
        
            var contactsList = contacts.ToList<Contact>();
            using (var client = CompanyContactsAPIClientWithAuth())
            {
                var results = await client.Contacts.GetAsync();
                foreach (Contact c in results)
                {
                    contactsList.Add(c);
                }
            }
        
            return contactsList;
        }

上のコードで `CompanyContactsAPIClientWithAuth()` から返されるクライアント オブジェクトは、生成されたクライアント コードをベースとしていますが、承認トークンは HTTP 要求に追加します。

        private static CompanyContactsAPI CompanyContactsAPIClientWithAuth()
        {
            var client = new CompanyContactsAPI(new Uri(ConfigurationManager.AppSettings["CompanyContactsAPIUrl"]));
            client.HttpClient.DefaultRequestHeaders.Authorization =
                new AuthenticationHeaderValue("Bearer", ServicePrincipal.GetS2SAccessTokenForProdMSA().AccessToken);
            return client;
        }

## Azure で作成した API アプリへの CompanyContacts.API プロジェクトのデプロイ

1.  **ソリューション エクスプ ローラー**, を CompanyContacts.API プロジェクトを右クリックして、クリックして **発行**します。

3.   **プロファイル** のステップ、 **Web の発行** ウィザード] をクリックして **Microsoft Azure App Service**します。

    ![](./media/app-service-api-dotnet-service-principal-auth/selectappservice.png)

4. まだ行っていない場合は Azure アカウントにサインインし、有効期限が切れている場合は資格情報を更新します。

4.  **App Service** ] ダイアログ ボックスで、Azure を選択します **サブスクリプション** を使用し、をクリックする **新規**します。

    ![](./media/app-service-api-dotnet-service-principal-auth/clicknew.png)

3.  **ホスティング** のタブ、 **App Service の作成** ダイアログ ボックスで、をクリックして **の種類の変更**, 、順にクリック **API アプリ**します。

4. 入力、 **API アプリの名前** 内で一意である、 *azurewebsites.net* ドメイン。 

6.  **リソース グループ** ドロップダウン リストで、これらのチュートリアルを使用しているリソース グループを選択します。

4.  **App Service プラン** ドロップダウン リストで、これらのチュートリアルを使用しているプランを選択します。 

7. クリックして **作成**します。

    ![](./media/app-service-api-dotnet-service-principal-auth/createappservice.png)

    Visual Studio を使うと、API アプリが作成され、新しい API アプリに必要なすべての設定が含まれる発行プロファイルが作成されます。 

8.  **接続** のタブ、 **Web の発行** ウィザード] をクリックして **発行**します。

    ![](./media/app-service-api-dotnet-service-principal-auth/conntab.png)

    Visual Studio によって、プロジェクトに新しい API アプリがデプロイされ、ブラウザーで API アプリの URL が開きます。 ブラウザーに "正常に作成されました" ページが表示されます。

9. ブラウザーを閉じます。

## ContactsList.API プロジェクトでの生成されたクライアント コードの更新

ContactsList.API プロジェクトには既に生成済みのクライアント コードがありますが、それを削除して、独自の API アプリから再生成します。

1. Visual Studio で **ソリューション エクスプ ローラー**, 、ContactsList.API プロジェクトで、削除、 *CompanyContactsAPI* フォルダーです。

2. ContactsList.API プロジェクトを右クリックし、 **追加 > REST API クライアント**です。

3.  **REST API のクライアントの追加** ダイアログ ボックスで、をクリックして **Microsoft Azure API アプリからダウンロード**, 、] をクリックし、 **参照**します。

8.  **App Service** ] ダイアログ ボックスでは、このチュートリアルで使用しているリソース グループを展開して、先ほど作成した API アプリを選択します。

    API アプリが一覧に表示されない場合、API アプリを作成するときに、アプリの種類を (Web アプリから API アプリに) 変更する手順を誤って省略した可能性があります。 その場合は、先行する手順をやり直して新しい API アプリを作成してください。 API アプリに別の名前を選択する必要があります。または、ポータルに移動して先に Web アプリを削除しておいてください。 

10. Click **OK**.

9.  **REST API のクライアントの追加** ダイアログ ボックスで、をクリックして **OK**します。

    Visual Studio が API アプリを元に命名したフォルダーを作成し、クライアント クラスを生成します。

## ContactsList.API のコードの更新とプロジェクトのデプロイ

前のチュートリアルで、CompanyContacts.API を呼び出す ContactsList.API 内のコードはコメント アウトされています。 このセクションでは、そのコードのコメントを解除し、アプリをデプロイします。

1. ContactsList.API プロジェクトで開きます *Controllers/ContactsController.cs*します。

2. `ContactsController` クラス上部近くの、生成されたクライアント クラスを使用する認証トークンを追加するコードで、クラス名 `CompanyContactsAPI` を API アプリで生成したクラスの名前に置き換えます。

    たとえば、API アプリの名前が CompanyContactsAPI3 の場合、コードは次のようになります。

         private static CompanyContactsAPI3 CompanyContactsAPIClientWithAuth()
         {
             var client = new CompanyContactsAPI3(new Uri(ConfigurationManager.AppSettings["CompanyContactsAPIUrl"]));
             client.HttpClient.DefaultRequestHeaders.Authorization =
                 new AuthenticationHeaderValue("Bearer", ServicePrincipal.GetS2SAccessTokenForProdMSA().AccessToken);
             return client;
         }
 
4. `Get` メソッドで、CompanyContacts.API を呼び出すコード ブロックのコメントを解除します。

        using (var client = CompanyContactsAPIClientWithAuth())
        {
            var results = await client.Contacts.GetAsync();
            foreach (Contact c in results)
            {
                contactsList.Add(c);
            }
        }

2. ContactsList.API プロジェクトを右クリックし、クリックして **発行**します。

     **Web の発行** ウィザードが以前に使用した発行プロファイルを開きます。

3.  **Web の発行** ウィザード] をクリックして **発行**します。

    Visual Studio によってプロジェクトがデプロイされ、API アプリのベース URL がブラウザー ウィンドウで開きます。 このブラウザー ウィンドウを閉じます。

## Azure での新しい API アプリの認証と承認の設定

1.  [Azure ポータル](https://portal.azure.com/), CompanyContacts.API プロジェクトは、このチュートリアルで作成した API アプリの API アプリ] ブレードに移動し、クリックして、 **設定**します。

2. 検索、 **機能** セクションは、クリックして **認証/承認**します。

3.  **認証/承認** ブレードで、をクリックして **に**します。

4.  **要求が認証されていない場合に実行するアクション** ドロップダウン リストで、 **Azure Active Directory を使用してログイン**します。

5.  **認証プロバイダー**, 、クリックして **Azure Active Directory**します。

6.  **Azure Active Directory 設定** ブレードで、をクリックして **Express**します。

    Azure が AAD テナントに AAD アプリケーションを自動的に作成します。 クライアント ID を取得するために Azure クラシック ポータルに後で移動して選択するために、新しい AAD アプリケーションの名前をメモします。

7. Click **OK**.

10.  **認証/承認** ブレードで、をクリックして **保存**します。
 
11.  [Azure クラシック ポータル](https://manage.windowsazure.com/), に進み、 **Azure Active Directory**します。

12.  **ディレクトリ** ] タブで、AAD テナントをクリックします。

14. クリックして **アプリケーション > 自分の会社が所有するアプリケーション**, 、チェック マークをクリックします。

15. アプリケーションの一覧から、API アプリの認証を有効にしたときに Azure によって作成されたアプリケーション名をクリックします。

16. クリックして **構成**します。

17. チュートリアルの後の手順で、値をコピーして貼り付けることができるよう、このページは開いたままにします。

## ContactsList.API プロジェクト コードを実行する API アプリの設定の更新

3. 別のブラウザー ウィンドウに移動、 [従来の Azure ポータル](https://manage.windowsazure.com/), 、順に選択し、 **構成** ContactsList.API API アプリ用に作成した AAD アプリケーションのタブをクリックします。

5.  **キー**, [ **1 年間** から、 **の期間を選択** ドロップダウン リストです。

6. クリックして **保存**します。

    ![](./media/app-service-api-dotnet-service-principal-auth/genkey.png)

7. キー値をコピーします。

    ![](./media/app-service-api-dotnet-service-principal-auth/genkeycopy.png)

1. 別のブラウザー ウィンドウで、 [Azure ポータル](https://portal.azure.com/), 、ContactsList.API プロジェクトをデプロイした API アプリの API アプリのブレードに移動します。  (これは呼び出し先ではなく呼び出し元の API アプリです。つまり、ContactsList.API であって、CompanyContacts.API ではありません。)

2. クリックして **設定 > アプリケーション設定**します。

3.  **アプリ設定** セクションで"ida: ClientSecret"という名前のキーを追加し、[値] フィールドで作成したキーを貼り付けます。

3. "Ida: ClientId"という名前のキーを追加し、[値] フィールドには、同じ AAD からのクライアント ID を貼り付けます **構成** ページです。

4. "ida:Authority" という名前のキーを追加し、その値フィールドに「https://login.windows.net/{該当するテナント}」と入力します (例: https://login.windows.net/contoso.onmicrosoft.com)。

3. 従来の Azure ポータルに移動、 **構成** CompanyContacts.API API アプリ用に作成した AAD アプリケーションのタブをクリックします。

4. クライアント ID をコピーします。

3. Azure ポータルで **アプリケーション設定** ブレードで、 **アプリ設定** セクションで ida: リソースをという名前のキーを追加し、[値] フィールドにコピーしたクライアント ID を貼り付けます。

4. "CompanyContactsAPIUrl"という名前のキーを追加し、[値] フィールドに次のように入力します"https://{your api アプリ名} *your_company*.azurewebsites.net .net"例:"https://companycontactsapi.azurewebsites.net"。

6. [保存] をクリックします。

    ![](./media/app-service-api-dotnet-service-principal-auth/appsettings.png)

## Azure でのテスト

1. ContactsList.Angular.AAD プロジェクトをデプロイした Web アプリの URL に移動します。

2. クリックして、 **連絡先** タブをクリックし、ログインしています。

    CompanyContacts.API API アプリから取得した追加の連絡先が含まれる [連絡先] ページが開きます。

    ![](./media/app-service-api-dotnet-service-principal-auth/contactspagewithdavolio.png)

前のチュートリアルと同様、この Visual Studio プロジェクトは localhost の SSL URL でセットアップし、アプリケーションをローカルで実行することもできます。 その場合、Azure での実行を想定して Azure に保存した設定 (クライアント ID、クライアント シークレットなど) は、Web.config ファイルに保存できます。  ただし、クライアント シークレットなどの機密情報を含んだ Web.config ファイルをソース管理にチェックインしないよう注意してください。 詳細については、次を参照してください。 [ASP.NET と Azure App Service にパスワードやその他の機密情報を展開するためのベスト プラクティス](http://www.asp.net/identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure)します。

## ブラウザー アクセスから API アプリを保護する

このチュートリアルでは、サービス プリンシパル認証を使ってアクセスする API アプリに対し、Azure ポータルの [Express] オプションを使用して AAD 認証を設定しました。 既定では、ブラウザーから API アプリの URL にアクセスしてログオンすることをユーザーに許可するような構成が、App Service によって新しい AAD アプリケーションに適用されます。 つまり、サービス プリンシパルだけでなくエンド ユーザーも API にアクセスすることができます。 変更することによってブラウザー アクセスを禁止するには、API にアクセスするサービス プリンシパル、のみを作成する場合、 **応答 URL** AAD アプリケーションでは、API アプリの異なるようにベース URL です。 

### ブラウザー アクセスが有効であることを確認する

1. 新しいブラウザー ウィンドウから、CompanyContacts.API プロジェクト用に作成した API アプリの HTTPS URL にアクセスします。

    ブラウザーにログイン画面が表示されます。
    
2. AAD テナント内のユーザーの資格情報を使用してログインします。

3. 正常に作成されたことを示す API アプリの画面がブラウザーに表示されます。 

    Swagger UI が有効になっていれば、`/swagger` URL にアクセスして、API を呼び出すことができます。 この URL に `/api/contacts/get` を追加することによって、ブラウザーから API を呼び出すことができます。

### ブラウザー アクセスを無効にする

1. 従来のポータルで **構成** CompanyContacts.API プロジェクト用に作成した AAD アプリケーション] タブの値を変更、 **応答 URL** フィールドは有効な URL が API アプリの URL ではありません。
 
2. クリックして **保存**します。

### ブラウザー アクセスが無効になったことを確認する

1. 新しいブラウザー ウィンドウで、API アプリの URL にもう一度アクセスします。

2. ログインを求められたらログインします。

3. ログインは成功しますが、エラー ページにリダイレクトされます。

    サービス プリンシパルのトークンを使って API アプリにアクセスすることはできますが、AAD テナントのユーザーがブラウザーからログインして API を利用することはできません。

## 次のステップ

これは、API Apps の概要シリーズの最終のチュートリアルです。 このセクションでは、API アプリを使用する方法に関するその他の推奨事項を示します。

* App Service アプリをデプロイするその他の方法

    Visual Studio を使用するか、web アプリに web プロジェクトを展開するには、その他の方法については [展開を自動化する](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery) から、 [ソース管理システム](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control), を参照してください [Azure web アプリを展開する方法](web-sites-deploy.md)します。

    Visual Studio により、デプロイを自動化するために使用可能な Windows PowerShell スクリプトも生成できます。 詳細については、次を参照してください。 [(Azure で構築実際のクラウド アプリケーション) を自動化してすべて](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything)です。

* App Service アプリのトラブルシューティングの方法

    Visual Studio には、リアルタイムで生成される Azure ログの表示を容易にする機能があります。 Azure では、リモートでデバッグ モードを使用して実行することもできます。 詳細については、次を参照してください。 [Visual Studio で web アプリを Azure のトラブルシューティング](web-sites-dotnet-troubleshoot-visual-studio.md)します。

* カスタム ドメイン名および SSL の追加方法

    SSL および独自のドメイン (contoso.azurewebsites.net の代わりに www.contoso.com など) の使用方法については、次のリソースを参照してください。

    * [Azure App Service のカスタム ドメイン名の構成](web-sites-custom-domain-name.md)
    * [Azure の Web サイトでの HTTPS の有効化](web-sites-configure-ssl-certificate.md)

