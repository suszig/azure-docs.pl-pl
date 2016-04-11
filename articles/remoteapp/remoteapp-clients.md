
<properties
    pageTitle="任意のデバイスからのアプリへのアクセス | Microsoft Azure"
    description="Azure RemoteApp にサポートされているクライアントとアプリへのアクセス方法について説明します。"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2015"
    ms.author="elizapo" />



# Azure RemoteApp でのアプリへのアクセス

Azure RemoteApp の優れた点の 1 つは、どのデバイスからでもアプリにアクセスできることです。 さらに、1 つのデバイスで作業を開始し、2 つ目のデバイスにシームレスに移行し、中断した箇所から作業を再開できます。 開始するには、デバイスの適切なクライアントをダウンロードして、サービスにサインインする必要があります。

このトピックでは、現在サポートされているクライアントとダウンロードする方法を確認してから、各クライアントから RemoteApp へサインインする方法を説明します。

## サポートされているクライアント

デバイスがこれらのいずれかのオペレーティング システムを実行している場合は、次の手順を使用して RemoteApp にアクセスできます。

 - Windows 10 
 - Windows 8.1
 - Windows 8
 - Windows 7 Service Pack 1
 - Windows Phone 8.1
 - iOS
 - Mac OS X
 - Android


 シン クライアントはどうでしょうか。 次の Windows Embedded のシン クライアントはサポートされています。

- Windows Embedded Standard 7
- Windows Embedded 8 Standard
- Windows Embedded 8.1 Industry Pro
- Windows 10 IoT Enterprise


## クライアントのダウンロード

使用しているプラットフォームや、RemoteApp にアクセスする必要があるクライアントにある [ [リモート デスクトップ クライアントのダウンロード](https://www.remoteapp.windowsazure.com/ClientDownload/AllClients.aspx) ページです。

別のリンクをクリックすると、クライアントのダウンロードを直接開始するか、またはプラットフォームのアプリ ストアのクライアント ダウンロード ページ移動します。 画面の手順に従って、クライアントをインストールします。

デバイスにクライアントをインストールし、起動したら、次の対応するセクションにジャンプし、そのクライアントから RemoteApp へサインインする方法を学習します。

## Android

Google Play ストアから、Microsoft リモート デスクトップ アプリをインストールした後は、アプリの一覧で見つけることができます **リモート デスクトップ**します。

1. アプリを起動すると、まだアプリを使用していない場合は、空の接続センターに移動します。 Azure RemoteApp を開始するには、追加ボタンをタップ **「"+」"** タップ **Azure RemoteApp**します。 

     ![空の接続センター](./media/remoteapp-clients/Android1.png)

2. サービスにアクセスするには、メール アドレスでサインインする必要があります。 タップ **開始**します。

    ![サインインのプロンプト](./media/remoteapp-clients/Android2.png)

3. 次のページで入力、 **電子メール アドレス** タップ **続行**します。 これで、Azure Active Directory を使用したサインイン プロセスが開始されます。

    ![Azure Active Directory の最初のページ](./media/remoteapp-clients/Android3.png)

4. 画面の指示に従って、Microsoft アカウント (以前の "LiveID") や組織 ID でサインインします。 サインインすると、受信したすべての招待を一覧表示したページが表示されます。 信頼し、タップへの招待を選択する場合は、 **実行**します。 

    ![招待のページ](./media/remoteapp-clients/Android4.png)

5. 招待を受け付けると、アクセスできるアプリの一覧がデバイスにダウンロードされ、接続センターで使用可能になります。 アプリの 1 つをタップし、使用を開始します。

    ![フィードによる接続センター](./media/remoteapp-clients/Android5.png)

6. まだ招待がない場合でも、サービスを試すことができます。 これを行うには、タップ **には、無料評価版** 入力を求められたらします。

    ![デモ フィードのプロンプト](./media/remoteapp-clients/Android6.png)

7. これで基本的なアプリのセットにアクセスし、RemoteApp を開始できます。

    ![Azure RemoteApp のデモ フィード](./media/remoteapp-clients/Android7.png)

## iOS

App store から Microsoft リモート デスクトップ アプリをインストールした後は、アプリの一覧で検索できます **RD Client**します。

1. アプリを起動すると、まだアプリを使用していない場合は、空の接続センターに移動します。 Azure RemoteApp を開始するには、追加ボタンをタップ **「"+」"** タップ **[Azure RemoteApp を**します。

    ![空の接続センター](./media/remoteapp-clients/IOS1.png)

2. 必要なプロセスを開始するサービスにアクセスする、電子メール アドレスでサインインに入力する、 **電子メール アドレス** タップ **続行**します。

    ![サインインのプロンプト](./media/remoteapp-clients/picture1.png)

3. 画面の指示に従って、Microsoft アカウント (LiveID) や組織 ID でサインインします。 サインインすると、受信したすべての招待を一覧表示したページが表示されます。 信頼し、タップへの招待を選択する場合は、 **実行**します。

    ![招待のページ](./media/remoteapp-clients/IOS3.png)

4. 招待を受け付けると、アクセスできるアプリの一覧がデバイスにダウンロードされ、接続センターで使用可能になります。 アプリのいずれかをタップして起動し、使用を開始します。

    ![フィードによる接続センター](./media/remoteapp-clients/IOS4.png)

5. まだ招待がない場合でも、サービスを試すことができます。 これを行うには、タップ **には、無料評価版** 入力を求められたらします。

    ![デモ フィードのプロンプト](./media/remoteapp-clients/IOS5.png)

6. これで基本的なアプリのセットにアクセスし、RemoteApp を開始できます。

    ![Azure RemoteApp のデモ フィード](./media/remoteapp-clients/IOS6.png)

## Mac OS X

App store から Microsoft リモート デスクトップ アプリをインストールした後は、アプリの一覧で検索できます **Microsoft リモート デスクトップ**します。

1. アプリを起動すると、まだアプリを使用していない場合は、空の接続センターに移動します。 Azure RemoteApp を開始する] をクリックして、 **Azure RemoteApp** ] ボタンをクリックします。

    ![空の接続センター](./media/remoteapp-clients/Mac1.png)

2. そのプロセスを開始する、サービスにアクセスし、メール アドレスでサインイン タップ **開始**します。

    ![サインインのプロンプト](./media/remoteapp-clients/Mac2.png)

3. 次のページで入力、 **電子メール アドレス** タップ **続行**します。 これで、Azure Active Directory を使用したサインイン プロセスが開始されます。

    ![Azure Active Directory の最初のページ](./media/remoteapp-clients/picture2.png)

4. 画面の指示に従って、Microsoft アカウント (LiveID) や組織 ID でサインインします。 サインインすると、受信したすべての招待を一覧表示したページが表示されます。 信頼する招待を選択し、 ダイアログを閉じます。

    ![招待のページ](./media/remoteapp-clients/Mac4.png)

5. 招待を受け付けると、アクセスできるアプリの一覧がデバイスにダウンロードされ、接続センターで使用可能になります。 アプリのいずれかをダブルクリックして起動し、使用を開始します。

    ![フィードによる接続センター](./media/remoteapp-clients/Mac5.png)

6. まだ招待がない場合でも、サービスを試すことができます。 これを行うには、次のようにクリックします。 **には、無料評価版** 入力を求められたらします。

    ![デモ フィードのプロンプト](./media/remoteapp-clients/Mac6.png)

7. これで基本的なアプリのセットにアクセスし、RemoteApp を開始できます。

    ![Azure RemoteApp のデモ フィード](./media/remoteapp-clients/Mac7.png)

## Windows (Windows Phone 以外のサポートされているすべてのバージョン)

ただしそれにアクセスする必要があるときに後でもう一度があるアプリの一覧、名の下をインストールが終了した後、クライアントは自動的に起動 **Azure RemoteApp**します。

1. クライアントを起動すると、最初に、Azure RemoteApp の開始ページが表示されます。 続行するにはクリックして **開始**します。

    ![Azure RemoteApp のクライアントの開始ページ](./media/remoteapp-clients/Windows1.png)

2. 次のページでは、Azure Active Directory を使用した Azure RemoteApp のサインイン プロセスが開始されます。 過去に Microsoft のサービスを使用したことのあるユーザーには、このプロセスは見慣れたものです。 開始」と入力して、 **電子メール アドレス** ] をクリック **続行**します。

    ![Azure Active Directory の最初のプロンプト](./media/remoteapp-clients/Windows2.png)

3. 画面の指示に従って、Microsoft アカウント (LiveID) や組織 ID でサインインします。 サインインすると、受信したすべての招待を一覧表示したページが表示されます。 招待状を信頼し、をクリックするを選択する場合は、 **実行**します。

    ![Azure RemoteApp のクライアントの招待のページ](./media/remoteapp-clients/Windows3.png)

4. 招待を受け付けると、アクセスできるアプリの一覧がデバイスにダウンロードされ、接続センターで使用可能になります。 アプリのいずれかをダブルクリックして起動し、使用を開始します。

    ![Azure RemoteApp のクライアントの接続センター](./media/remoteapp-clients/Windows4.png)

5. まだ、だれも招待を送信していなくても、デモ コレクションでサービスをテストすることができます。 デモ コレクションへはアクセスできますので、サービスをテストすることができます。

    ![Azure RemoteApp のデモ フィード](./media/remoteapp-clients/Windows5.png)

## Windows Phone 8.1

Windows Phone 8.1 ストアから、Microsoft リモート デスクトップ アプリをインストールした後は、アプリの一覧で見つけることができます **リモート デスクトップ**します。

1. アプリを起動すると、まだアプリを使用していない場合は、空の接続センターに移動します。 Azure RemoteApp を開始するには、追加ボタンをタップ **「"+」"** 、画面の下部にあります。

    ![空の接続センター](./media/remoteapp-clients/WinPhone1.png)

2. 次に、[タップ **Azure RemoteApp**します。

    ![項目ページの追加](./media/remoteapp-clients/WinPhone2.png)

3. そのプロセスを開始する、サービスにアクセスし、メール アドレスでサインイン タップ **接続**します。

    ![サインインのプロンプト](./media/remoteapp-clients/WinPhone3.png)

4. 次のページで入力、 **電子メール アドレス** タップ **続行**します。 これで、Azure Active Directory を使用したサインイン プロセスが開始されます。

    ![Azure Active Directory の最初のページ](./media/remoteapp-clients/WinPhone4.png)

5. 画面の指示に従って、Microsoft アカウント (LiveID) や組織 ID でサインインします。 サインインすると、受信したすべての招待を一覧表示したページが表示されます。 信頼し、タップへの招待を選択する場合は、 **保存**します。

    ![招待のページ](./media/remoteapp-clients/WinPhone5.png)

6. 招待を受け付けると、アクセスできるアプリの一覧がデバイスにダウンロードされ、接続センターで使用可能になります。 アプリのいずれかをタップして起動し、使用を開始します。

    ![フィードによる接続センター](./media/remoteapp-clients/WinPhone6.png)

7. まだ招待がない場合でも、サービスを試すことができます。 これを行うには、タップ **はい** 入力を求められたらします。

    ![デモ フィードのプロンプト](./media/remoteapp-clients/WinPhone7.png)

8. これで基本的なアプリのセットにアクセスし、RemoteApp を開始できます。

    ![Azure RemoteApp のデモ フィード](./media/remoteapp-clients/WinPhone8.png)
 
