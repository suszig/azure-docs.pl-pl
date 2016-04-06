<properties
    pageTitle="Mobile Services から App Service モバイル アプリへの移行"
    description="Mobile Services アプリケーションを App Service モバイル アプリに簡単に移行する方法について説明します。"
    services="app-service\mobile"
    documentationCenter=""
    authors="mattchenderson"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile"
    ms.devlang="na"
    ms.topic="article"
    ms.date="11/25/2015"
    ms.author="mahender"/>

# 既存の Azure モバイル サービスを App Service に移行する

このトピックでは、Azure Mobile Services の既存のアプリケーションを新しい App Service モバイル アプリに移行する方法について説明します。 既存のすべてのモバイル サービスは、新しい Mobile Apps バックエンドに簡単に移行できます。 移行中も、既存のモバイル サービスは機能し続けます。

モバイル サービスを App Service に移行すると、App Service のすべての機能にアクセスできるし、は」の手順に従って課金されます [App Service pricing], 、モバイル サービスではなく価格設定されます。

スケジュールされたジョブを、Microsoft 管理対象の Azure Scheduler プランから、ユーザー独自のサブスクリプションおよび制御下の Azure Scheduler プランに移動されることにも注意してください。 App Service への移行の利点と同様に、これにより、Azure Scheduler の全機能を活用できます。

### <a name="why-host"></a>App Service でホストする理由

App Service は、アプリケーションのホスト環境として、より充実した機能を備えています。 App Service でホストされたサービスから、ステージング スロット、カスタム ドメイン、Traffic Manager サポートなど、豊富な機能にアクセスできます。 App Service への移行後にモバイル サービスを Mobile App バックエンドにアップグレードできますが、一部の顧客は SDK の更新をすぐに実行せずに、これらの機能をすぐに利用することもできます。  

詳細については、App Service の利点は、次を参照してください。 [Mobile Services vs. App Service]します。

## 移行とアップグレード

[AZURE.INCLUDE [app-service-mobile-migrate-vs-upgrade](../../includes/app-service-mobile-migrate-vs-upgrade.md)]

- Node.js ベースのサーバー プロジェクトの場合、新しい [Mobile Apps Node.js SDK](https://github.com/Azure/azure-mobile-apps-node) 多くの新機能を提供します。 たとえば、ローカルで開発やデバッグを行い、0.10 より後のバージョンの Node.js を使用し、Express.js ミドルウェアでカスタマイズを行うことができます。

- します。NET ベースのサーバー プロジェクトと、新しい [モバイル アプリ SDK の NuGet パッケージ](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) NuGet 依存関係を高い柔軟性を持つ、新しい App Service の認証機能をサポートおよび MVC を含め、どのような ASP.NET プロジェクトを作成します。 アップグレードの詳細については、次を参照してください。 [App Service への既存の .NET Mobile Service のアップグレード](app-service-mobile-net-upgrading-from-mobile-services.md)します。


## <a name="understand"></a>App Service Mobile Apps について

App Service の Mobile Apps は、Azure を使用してアプリケーションを構築する新しい方法です。 モバイル アプリに関する詳細については、 [What are Mobile Apps?]します。

Mobile Apps に移行しても、既存のすべてのアプリ機能 (ビジネス ロジックを含む) を維持できます。 さらに、新しい機能をアプリケーションで利用できます。 Mobile Apps モデルでは、App Service の Web アプリ (新しいバージョンの Azure Web サイト) で、コードが実際に実行されます。 Web アプリとその動作を完全に制御できます。 さらに、Traffic Manager や開発スロットなど、以前は Mobile Services の顧客は使用できなかった Web アプリの機能が使用できるようになりました。

アップグレードの主な制限事項として、Mobile Services のスケジュールされたジョブが統合されないため、カスタム API を呼び出すように Azure Scheduler を設定するか、Web ジョブ機能を有効にする必要があります。

## [App Service に移行する] ウィザードを使用する Mobile Service の移行

モバイル サービスを App Service に移行する最も簡単で推奨される方法は、Azure クラシック ポータルで使用可能な [App Service に移行する] ウィザードを使用することです。 移動、 [Azure classic portal],  、モバイル サービスを参照し、移行するモバイル サービスの選択は、画面の下部にある実際は、 **App Service への移行** 、プロセス、モバイル サービスを移行するための手順を説明するボタンをクリックします。

### <a name="what-gets-migrated"></a>移行内容

移行ウィザードでは、モバイル サービスをホストするサーバー ファームを、Mobile Services による管理から App Service による管理に変更します。 また、Mobile Services が管理していた Scheduler プランを、Microsoft 管理対象サブスクリプションからユーザーのサブスクリプションに移動します。 サーバー ファームが App Service の管理制御に転送されると、サーバー ファームに関連付けられていたすべての Mobile Services が同時に移動されます。

Mobile Services は、リージョンおよび SKU (無料、基本、標準) に基づいて、サーバー ファームにモバイル サービスを編成します。 リージョン内のすべての無料サービスは同じサーバー ファーム上にあり、一緒に移行されます。リージョン内のいくつかの基本サービスは同じファームで一緒にホストされますが、基本サービスが多数ある場合は、別のプランに移行されます。標準サービスはそれぞれ独自のサーバー ファーム上にあります。 常に単一のサービスを移行するか、サービスが他のサイトと共に移動されないようにする場合は、基本にアップグレードできます。そうすることで、サービスは独自の独立したプランに移行されるようになります。

スケジュールされたジョブを使用していた場合、Azure Scheduler プランはサブスクリプションにも追加されます。 サブスクリプションごとに許可される無料のスケジュールされたジョブ数には制限があるため、投稿サブスクリプションを確認する必要がある、これに関連付けられている一部のコストが発生する場合があります。

###  移行ウィザードの使用

1. 移動し、 [Azure classic portal] モバイル サービス] をクリックします。 

2. 移行するモバイル サービスを選択してから、下部にあるメニュー バーで [App Service に移行する] をクリックします。 これで、移行される Mobile Services のリストがポップアップに表示されます。 参照してください、 [前のセクション](#what-gets-migrated) どのようなサービスの詳細については、最大とその理由を表示されます。 

3. モバイル サービスの名前を入力し、移行を確認してから、チェックマークをクリックして続行します。 移行対象として複数のモバイル サービスが含まれている場合も、選択された元のサービスの名前を入力する必要があります。 

4. モバイル サービスの一覧から現在の状態を表示するには、移行が開始した後、 [Azure classic portal]します。 現在移行中のサービスはすべて "移行中" と示されます。 すべての移行を完了済みとして表示した後でそれらを表示できる、 [Azure portal] Mobile Apps の下にあります。 移行中および移行後も、モバイル サービスは引き続き機能し、URL は変わりません。

## Mobile Services .NET バックエンドの手動移行

>[AZURE.NOTE] モバイル サービスを移行するための推奨される方法は前のセクションで説明する、App Service ウィザードへの移行を使用して、注意してください。 手動による移行は、ウィザードを使用できない場合にのみ使用する必要があります。

このセクションでは、Azure App Service 内での .NET Mobile Services プロジェクトのホスト方法を示します。 この方法でホストされているアプリは、すべてを使用して、 *Mobile Services* .NET ランタイムのチュートリアルが、App Service のインスタンスのドメインととしてドメインを使用して任意の URL を置き換える必要があります。

モバイル サービス プロジェクトをホストすることも、 [App Service Environment]します。 その場合も、このトピックの内容はすべて当てはまりますが、サイトの形式は `contoso.azurewebsites.net` ではなく `contoso.contosoase.p.azurewebsites.net` になります。

>[AZURE.NOTE] 手動移行を実行する場合、 *ことはできません* 、既存の保持 **service.azure として** URL です。 この URL に接続しているモバイル クライアントがある場合は、自動移行オプションを使用するか、すべてのモバイル クライアントが新しい URL にアップグレードされるまでモバイル サービスを実行しておく必要があります。


### <a name="app-settings"></a>アプリケーションの設定
Mobile Services では、いくつかのアプリケーション設定を環境内で使用できる必要があります。このセクションではそれについて説明します。

モバイル アプリ バックエンドにアプリケーションの設定を設定するために最初にサインイン、 [Azure portal]します。 App Service アプリをモバイル アプリのバックエンドとして使用して、をクリックする **設定** > **アプリケーション設定**, 、下方向にスクロールし、 **アプリ設定**, 、これは、次の必要なキーと値のペアを設定することができます。

+ **MS_MobileServiceName** 、アプリの名前に設定する必要があります。 たとえば、ときに、バックエンドの URL は `contoso.azurewebsites.net`, 、し *contoso* 、適切な値は、です。

+ **MS_MobileServiceDomainSuffix** web アプリの名前に設定する必要があります。 たとえば、ときに、バックエンドの URL は `contoso.azurewebsites.net`, 、し *azurewebsites.net* 、適切な値は、です。

+ **MS_ApplicationKey** 、任意の値に設定することができますが、クライアント SDK で使用される同じ値である必要があります。 GUID を使用することをお勧めします。

+ **MS_MasterKey** 、任意の値に設定することができますが、管理 Api/クライアントで使用される同じ値である必要があります。 GUID を使用することをお勧めします。

マスター_キーとアプリケーション キーの両方をから取得できます、モバイル サービスを移行するときに、 **構成** の Mobile Services] セクションのタブ、 [Azure classic portal]します。 をクリックして **キーの管理** 下部にあるキーをコピーします。


### <a name="client-sdk"></a>方法: クライアント SDK を変更する

クライアント アプリ プロジェクトで、新しいアプリの URL (例: `https://contoso.azurewebsites.net`) と、前に構成したアプリケーション キーを受け入れるように、Mobile Services クライアント オブジェクトのコンストラクターを変更します。 クライアント SDK のバージョンにする必要があります、 **Mobile Services** バージョンする必要があります **いない** アップグレードします。 iOS クライアントと Android クライアントの場合は 2.x バージョンを使用し、Windows/Xamarin の場合は 1.3.2 を使用します。 Javascript クライアントは 1.2.7 を使用する必要があります。

### <a name="data"></a>方法: データ機能を有効にする

Mobile Services で既定の Entity Framework クラスを使用するには、さらに 2 つのアプリケーション設定が必要です。

接続文字列に格納されます、 **接続文字列** アプリケーション設定] ブレードのすぐ下、 **アプリ設定** セクションです。 データベースの接続文字列を設定する必要があります、 **MS_TableConnectionString** キー。 既存のモバイル サービスを App Service に移行する場合に移動、 **接続文字列** 、モバイル サービスのセクション **構成** ] タブで、 [Azure クラシック ポータル](https://manage.windowsazure.com/)します。 クリックして **[接続文字列の** 値をコピーします。

使用するスキーマは、既定では、 **MS_MobileServiceName**, でこれを上書きすることが、 **MS_TableSchema** 設定します。 下に **アプリ設定** 設定 **MS_TableSchema** を使用するスキーマの名前を指定します。 既存の Mobile Services アプリケーションを移動する場合は、スキーマが既に Entity Framework を使用して作成されています。この名前は、現在コードをホストする App Service インスタンスではなく、モバイル サービスになっています。

### <a name="push"></a>方法: プッシュ機能を有効にする

プッシュを機能させるには、Web アプリが通知ハブに関する情報を把握する必要もあります。

 **接続文字列**, 、 **MS_NotificationHubConnectionString** 通知ハブの DefaultFullSharedAccessSignature の接続文字列を使用します。 移動する既存のモバイル サービスを移行する、 **接続文字列** のモバイル サービスのセクション **構成** ] タブで、 [Azure クラシック ポータル](https://manage.windowsazure.com/)します。 クリックして **[接続文字列の** 値をコピーします。

 **MS_NotificationHubName** アプリ設定は、ハブの名前に設定する必要があります。 既存のモバイル サービスを移動するときに、モバイル サービスからこの値を取得できます **プッシュ** ] タブで、 [Azure クラシック ポータル](https://manage.windowsazure.com/)します。 このタブの他のフィールドは、ハブそのものにリンクされているため、他の場所にコピーする必要はありません。

### <a name="auth"></a>方法: 認証機能を有効にする

ID 機能には、プロバイダーごとにアプリケーション設定の要件があります。 各フィールドで既存のモバイル サービスから移動している場合、 **Identity** Azure クラシック ポータルでのタブには、対応するアプリケーションの設定です。

Microsoft アカウント

* **MS_MicrosoftClientID**

* **MS_MicrosoftClientSecret**

* **MS_MicrosoftPackageSID**

Facebook

* **MS_FacebookAppID**

* **MS_FacebookAppSecret**

Twitter

* **MS_TwitterConsumerKey**

* **MS_TwitterConsumerSecret**

Google

* **MS_GoogleClientID**

* **MS_GoogleClientSecret**

AAD

* **MS_AadClientID**

* **MS_AadTenants** -メモ: **MS_AadTenants** テナントのドメインのコンマ区切りリストとして格納されます (、 **許可されたテナント** Azure クラシック ポータル内のフィールド)。

### <a name="publish"></a>方法: モバイル サービス プロジェクトを発行する

1.  [Azure portal], アプリへの移動をクリックして、 **発行プロファイルを取得** コマンド バーで、ローカル コンピューターにダウンロードしたプロファイルを保存します。
2. Visual Studio では、Mobile Services のサーバー プロジェクトを右クリックし、をクリックして **発行**します。 
3. [プロファイル] タブで、次のように選択します。 **インポート** し、ダウンロードしたプロファイルを参照します。
3. クリックして **発行** App Service へのコードをデプロイします。

標準の App Service ログ機能によってログが処理されます。 ログの詳細については、次を参照してください。 [Enable diagnostics logging in Azure App Service]します。



<!-- URLs. -->

[Azure portal]: https://portal.azure.com/
[Azure classic portal]: https://manage.windowsazure.com/
[What are Mobile Apps?]: app-service-mobile-value-prop.md
[I already use web sites and mobile services – how does App Service help me?]: /en-us/documentation/articles/app-service-mobile-value-prop-migration-from-mobile-services
[Mobile App Server SDK]: http://www.nuget.org/packages/microsoft.azure.mobile.server
[Create a Mobile App]: app-service-mobile-xamarin-ios-get-started.md
[Add push notifications to your mobile app]: app-service-mobile-xamarin-ios-get-started-push.md
[Add authentication to your mobile app]: app-service-mobile-xamarin-ios-get-started-users.md
[Azure Scheduler]: /en-us/documentation/services/scheduler/
[Web Job]: ../app-service-web/websites-webjobs-resources.md
[How to use the .NET server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md


[Enable diagnostics logging in Azure App Service]: web-sites-enable-diagnostic-log.md
[App Service pricing]: https://azure.microsoft.com/en-us/pricing/details/app-service/
[App Service Environment]: app-service-app-service-environment-intro.md
[Mobile Services vs. App Service]: app-service-mobile-value-prop-migration-from-mobile-services-preview.md
[migrate a mobile service to a mobile app on App Service]: app-service-mobile-migrating-from-mobile-services.md


