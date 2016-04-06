<properties
    pageTitle="Microsoft 認証用の登録 | Microsoft Azure"
    description="Azure Mobile Services アプリケーションで Microsoft 認証用に登録する方法について説明します。"
    authors="ggailey777"
    services="mobile-services"
    documentationCenter="Mobile"
    manager="dwrede"
    editor=""/>

<tags 
    ms.service="mobile-services" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="NA" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="11/30/2015" 
    ms.author="glenga"/>

# 認証で Microsoft アカウントを使用するためのアプリケーションの登録

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

&nbsp;


[AZURE.INCLUDE [mobile-services-selector-register-identity-provider](../../includes/mobile-services-selector-register-identity-provider.md)]

## 概要

このトピックでは、Azure Mobile Services の ID プロバイダーとして Microsoft アカウントを使用できるようにモバイル アプリを登録する方法について説明します。 Live SDK を使用するサービス主導型認証とクライアント主導型認証の両方に同じ手順が適用されます。

##Windows デベロッパー センターで Windows ストア アプリを登録する

Windows ストア アプリを最初に Windows デベロッパー センターで登録する必要があります。

>[AZURE.NOTE]Windows Phone 8、Windows Phone 8.1 Silverlight、および Windows 以外のアプリは、このセクションを省略できます。

1. アプリを登録済みでない場合に移動、 [Windows デベロッパー センター](https://dev.windows.com/dashboard/Application/New), を Microsoft アカウントでログオン] をクリックし、アプリの名前を [ **アプリ名の予約**します。

3. オープンでソリューション エクスプ ローラーを Windows ストア アプリ プロジェクトを右クリックし、Visual Studio での Windows アプリケーション プロジェクトをクリックして **ストア** > **アプリケーションを... ストアと関連付ける**します。

    ![](./media/mobile-services-how-to-register-microsoft-authentication/mobile-services-store-association.png)

5. ウィザードで、次のようにクリックします。 **サインイン** 選択に、Microsoft アカウントでサインイン予約したアプリケーション名の順にクリック **次** > **関連付ける**します。

6. (省略可能) ユニバーサル Windows 8.1 アプリケーションの場合、Windows Phone ストア プロジェクトの手順 4 および 5 を繰り返します。

6. 新しいアプリケーションの Windows デベロッパー センター ページに戻って **サービス** > **プッシュ通知**します。

7.  **プッシュ通知** ] ページで [ **Live サービス サイト** [ **Windows プッシュ通知サービス (WNS) と Microsoft Azure Mobile Services**します。

これにより、アプリの Microsoft アカウント ページが表示されます。 次に、アプリで Microsoft 認証を使用するために Azure が必要とする認証資格情報を取得します。

## Microsoft アカウントの登録の構成と Mobile Services への接続

このセクションの最初の手順は、Windows Phone 8、Windows Phone 8.1 Silverlight、および Windows 以外のストア アプリにのみ適用されます。 これらのアプリの場合、Windows ストア アプリでだけ使用可能なパッケージ セキュリティ ID (SID) も無視できます。

1. 移動し、Windows ストア アプリ、 [マイ アプリケーション](http://go.microsoft.com/fwlink/p/?LinkId=262039) (必要な場合)、Microsoft アカウント デベロッパー センター、Microsoft アカウントでログオン] ページで [ **アプリケーションを作成**, 、型、 **アプリケーション名**, 、順にクリックして **同意**します。

    これにより Microsoft アカウントでアプリ名が確保され、アプリの Microsoft アカウント ページが表示されます。

2. アプリの Microsoft アカウント] ページでクリックして **API 設定**, を有効にする **モバイルやデスクトップ クライアント アプリ**, 、としてモバイル サービスの URL を設定、 **ターゲット ドメイン**, 、装置で、次の URL のいずれかの書式設定し、 **リダイレクト URL** ] をクリック **保存**:

    + **.NET バックエンド**: `https://<mobile_service>.azure-mobile.net/signin-microsoft`
    + **JavaScript バックエンド**: `https://<mobile_service>.azure-mobile.net/login/microsoftaccount`

     >[AZURE.NOTE]モバイル サービス バックエンドの種類には、正しいリダイレクト URL パスの形式を使用することを確認します。 これが正しくない場合、認証は失敗します。  **ルート ドメイン** 自動的に入力する必要があります。
&nbsp;

    ![Microsoft アカウント API の設定](./media/mobile-services-how-to-register-microsoft-authentication/mobile-services-win8-app-push-auth-2.png)


4. クリックして **アプリ設定** の値をメモしておきます、 **クライアント ID**, 、**クライアント シークレット** と **パッケージ SID**します。

    ![Microsoft アカウント アプリの設定](./media/mobile-services-how-to-register-microsoft-authentication/mobile-services-win8-app-push-auth.png)


    > [AZURE.NOTE] クライアント シークレットは、重要なセキュリティ資格情報です。 クライアント シークレットは、他のユーザーと共有したり、アプリケーションで配信したりしないでください。 Windows ストア アプリの登録でのみ、パッケージ SID フィールドが表示されます。

4.  [Azure classic portal], 、] をクリックして、 **Identity** 、パッケージ SID のタブに移動して、モバイル サービスをクライアント ID、クライアント シークレットを入力、id プロバイダーから取得したをクリックし、 **保存**します。

    >[AZURE.NOTE]Windows Phone 8、Windows Phone ストア 8.1 Silverlight、または Windows 以外のアプリのパッケージ SID 値を指定する必要はありません。

これで、Microsoft アカウントと連携するようにモバイル サービスとアプリケーションが構成されました。

<!-- Anchors. -->

<!-- Images. -->

<!-- URLs. -->

[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039

[Azure classic portal]: https://manage.windowsazure.com/


