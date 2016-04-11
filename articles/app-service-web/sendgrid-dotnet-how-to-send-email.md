<properties 
    pageTitle="SendGrid 電子メール サービスの使用方法 (.NET) | Microsoft Azure" 
    description="Azure で SendGrid 電子メール サービスを使用して電子メールを送信する方法について説明します。 コード サンプルは C# で記述され、.NET API を使用しています。" 
    services="app-service\web" 
    documentationCenter=".net" 
    authors="thinkingserious" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="10/12/2015" 
    ms.author="team-pi@sendgrid.com"/>





# SendGrid を使用した Azure での電子メールの送信方法


## 概要

このガイドでは、Azure の SendGrid 電子メール サービスを使用して
一般的なプログラム タスクを実行する方法を紹介します。 サンプルは C\#
で記述され、.NET API を利用しています。 紹介するシナリオ **を構築します。
電子メール**, 、**電子メールを送信する**, 、**添付ファイルの追加**, 、および **を使用します。
フィルター**します。 SendGrid と電子メールの送信の詳細については、
[次のステップ][] セクションです。

## SendGrid 電子メール サービスとは

SendGrid は、 [cloud-based email service] 信頼性を提供します。
[transactional email delivery], 、スケーラビリティ、およびリアルタイム分析の柔軟な Api 機能
を備えているためカスタム統合も容易です。 SendGrid の一般的な使用シナリオを以下に
示します。

-   顧客に受信通知を自動送信する。
-   顧客に月 1 回 e-flier や特別キャンペーンを送信するための
    配信リストを管理する。
-   電子メール、ブロックされているようなものを測定値をリアルタイムで収集して
    リアルタイムで収集する。
-   傾向を認識するために役立つレポートを生成する。
-   顧客の問い合わせを転送する。
-   受信電子メールを処理する。

詳細については、次を参照してください [https://sendgrid.com](https://sendgrid.com) または [c# ライブラリ。][sendgrid-csharp]

## SendGrid アカウントを作成する

[AZURE.INCLUDE [sendgrid-sign-up](../../includes/sendgrid-sign-up.md)]

## SendGrid .NET クラス ライブラリを参照する

 [SendGrid NuGet パッケージ](https://www.nuget.org/packages/Sendgrid) SendGrid API を取得する最も簡単な方法は、
依存関係をすべて備えたアプリケーションを構成して。 NuGet は、
Microsoft Visual Studio 2015 に含まれている visual Studio の拡張機能をしやすいのインストールし、更新
ライブラリとツールです。 

> [AZURE.NOTE] Visual Studio 2015 より前のバージョンの Visual Studio を実行している場合は、NuGet をインストールするを参照してください。 [http://www.nuget.org](http://www.nuget.org), 、] をクリックし、 **Install NuGet** ] ボタンをクリックします。

アプリケーションに SendGrid NuGet パッケージをインストールするには、次のステップを行います。

1.  新しいプロジェクトを作成します。

    ![新しいプロジェクトを作成する][create-new-project]

2.  テンプレートを選択します。

    ![テンプレートの選択][select-a-template]

3.   **ソリューション エクスプ ローラー**, を右クリックして **参照**, 、] をクリックし、
    **NuGet パッケージの管理**します。

4.  検索 **SendGrid** を選択し、 **SendGrid** 内の項目、
    を選択します。

    ![SendGrid NuGet パッケージ][SendGrid-NuGet-package]

5.  クリックして **インストール** をインストールを完了し、これを閉じます
    閉じます。

SendGrid の .NET クラス ライブラリと呼ばれる **SendGridMail**します。 次の名前空間が
含まれます。

-   **SendGridMail** の電子メール アイテムの作成と操作します。
-   **SendGridMail.Transport** 使用して電子メールを送信するため、
    **SMTP** プロトコル、または HTTP 1.1 プロトコルを **WEB/REST**します。

プログラムを使用してオートスケーリング アプリケーション ブロックにアクセスする
SendGrid 電子メール サービスをプログラムでアクセスするのには、です。
**System.Net** と **System.Net.Mail** は .NET Framework 名前空間
一般的に使用する型が含まれるために含まれています。
SendGrid Api。

    using System;
    using System.Net;
    using System.Net.Mail;
    using SendGrid;

## 方法: 電子メールを作成する

使用して、 **SendGridMessage** 電子メールを作成するオブジェクト
メッセージです。 メッセージ オブジェクトを作成します。
プロパティとメソッドのなどを設定することができます、
電子メール送信者、電子メール受信者、件名と本文の
電子メールをします。

次の例に、すべての値が設定された電子メール オブジェクトの作成方法を示します。
オブジェクト:

    // Create the email object first, then add the properties.
    var myMessage = new SendGridMessage();

    // Add the message properties.
    myMessage.From = new MailAddress("john@example.com");

    // Add multiple addresses to the To field.
    List<String> recipients = new List<String>
    {
        @"Jeff Smith <jeff@example.com>",
        @"Anna Lidman <anna@example.com>",
        @"Peter Saddow <peter@example.com>"
    };

    myMessage.AddTo(recipients);

    myMessage.Subject = "Testing the SendGrid Library";

    //Add the HTML and Text bodies
    myMessage.Html = "<p>Hello World!</p>";
    myMessage.Text = "Hello World plain text!";

型でサポートされるすべてのプロパティとメソッドの詳細については、
**SendGrid** 入力は、「 [sendgrid-csharp に関するページ][] GitHub にします。

## 方法: 電子メールを送信する

電子メール メッセージを作成した後を使用して送信できます。
SendGrid API を参照してください。 または、行為が禁止 [を使用します。NET の組み込みライブラリ](https://sendgrid.com/docs/Code_Examples/csharp.html)します。

電子メールを送信するには、
SendGrid アカウントの資格情報 (ユーザー名とパスワード) か、SendGrid API キー。 可能であれば API キーの使用をお勧めします。 API キーを構成する方法の詳細を実行する場合を参照してください、 [ドキュメント](https://sendgrid.com/docs/Classroom/Send/api_keys.html)

Azure ポータルを使用してこれらの資格情報を格納することがあります。
構成] をクリックし、[アプリ設定] の下のキー/値ペアを追加します。

 ![Azure app settings][azure_app_settings]

 これらには、次のようにしてアクセスすることができます。 
    
    var username = System.Environment.GetEnvironmentVariable("SENDGRID_USER"); 
    var pswd = System.Environment.GetEnvironmentVariable("SENDGRID_PASS");
    var apiKey = System.Environment.GetEnvironmentVariable("SENDGRID_APIKEY");

資格情報の使用:
    
    // Create network credentials to access your SendGrid account
    var username = "your_sendgrid_username";
    var pswd = "your_sendgrid_password";

    var credentials = new NetworkCredential(username, pswd);
    // Create an Web transport for sending email.
    var transportWeb = new Web(credentials);

API キーの使用:

    var apiKey = "your_sendgrid_api_key";  
    // create a Web transport, using API Key
    var transportWeb = new Web(apiKey);


次の例に、Web API でメッセージを送信する方法を示します。

    // Create the email object first, then add the properties.
    SendGridMessage myMessage = new SendGridMessage();
    myMessage.AddTo("anna@example.com");
    myMessage.From = new MailAddress("john@example.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Text = "Hello World!";

    // Create credentials, specifying your user name and password.
    var credentials = new NetworkCredential("username", "password");

    // Create an Web transport for sending email.
    var transportWeb = new Web(credentials);

    // Send the email, which returns an awaitable task.
    transportWeb.DeliverAsync(myMessage);

    // If developing a Console Application, use the following
    // transportWeb.DeliverAsync(mail).Wait();

## 方法: 添付ファイルを追加する

呼び出して、メッセージに添付ファイルを追加することができます、 **AddAttachment**
メソッドは、添付するファイルのパスと名前を指定します。
1 回このメソッドを呼び出すことによって、複数の添付ファイルを含めることができます。
各ファイルを添付するとします。 次の例では、追加を示します
メッセージの添付ファイル:

    SendGridMessage myMessage = new SendGridMessage();
    myMessage.AddTo("anna@example.com");
    myMessage.From = new MailAddress("john@example.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Text = "Hello World!";

    myMessage.AddAttachment(@"C:\file1.txt");
    
データから添付ファイルを追加することもできます。 **ストリーム**します。 上記と同じメソッドを呼び出すことによって行うことができます **AddAttachment**, 、メッセージに表示させるファイル名、データのストリームに渡すことでもです。 この場合、System.IO ライブラリを追加する必要があります。

    SendGridMessage myMessage = new SendGridMessage();
    myMessage.AddTo("anna@example.com");
    myMessage.From = new MailAddress("john@example.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Text = "Hello World!";

    using (var attachmentFileStream = new FileStream(@"C:\file.txt", FileMode.Open))
    {
        myMessage.AddAttachment(attachmentFileStream, "My Cool File.txt");
    }


## 方法: アプリを使用してフッター、追跡、および分析を有効にする

SendGrid では、"フィルター" を使用することで追加の電子メール機能を
アプリです。 できます。
クリックなどの特定の機能を有効にする追跡、Google アナリティクス
サブスクリプション追跡などとします。 すべてのアプリの一覧については、
[アプリケーション設定][]します。

アプリに適用できる **SendGrid** メソッドを使用してメッセージを電子メールで送信
一部として実装された、 **SendGrid** クラスです。

次の例では、フッターを示す、クリックして追跡
フィルター:

### フッター

    // Create the email object first, then add the properties.
    SendGridMessage myMessage = new SendGridMessage();
    myMessage.AddTo("anna@example.com");
    myMessage.From = new MailAddress("john@example.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Text = "Hello World!";

    // Add a footer to the message.
    myMessage.EnableFooter("PLAIN TEXT FOOTER", "<p><em>HTML FOOTER</em></p>");

### クリック追跡

    // Create the email object first, then add the properties.
    SendGridMessage myMessage = new SendGridMessage();
    myMessage.AddTo("anna@example.com");
    myMessage.From = new MailAddress("john@example.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Html = "<p><a href=\"http://www.example.com\">Hello World Link!</a></p>";
    myMessage.Text = "Hello World!";
    
    // true indicates that links in plain text portions of the email 
    // should also be overwritten for link tracking purposes. 
    myMessage.EnableClickTracking(true);

## 方法: その他の SendGrid サービスを使用する

Sendgrid の web ベース Api とその他を使用して webhook
その他の SendGrid 機能を利用できます。 詳細については、
詳細についてを参照してください、 [SendGrid API に関するドキュメント][]します。

## 次のステップ

これで、SendGrid 電子メール サービスの基本を学習できました。
さらに詳細な情報が必要な場合は、次のリンク先を参照してください。

*   SendGrid c# ライブラリ レポート: [sendgrid-csharp に関するページ][]
*   SendGrid API に関するドキュメント: <https://sendgrid.com/docs>
*   Azure ユーザー向けの SendGrid 特別プラン: [https://sendgrid.com](https://sendgrid.com)

  [Next steps]: #next-steps
  [What is the SendGrid Email Service?]: #whatis
  [Create a SendGrid Account]: #createaccount
  [Reference the SendGrid .NET Class Library]: #reference
  [How to: Create an Email]: #createemail
  [How to: Send an Email]: #sendemail
  [How to: Add an Attachment]: #addattachment
  [How to: Use Filters to Enable Footers, Tracking, and Analytics]: #usefilters
  [How to: Use Additional SendGrid Services]: #useservices
  
  [special offer]: https://www.sendgrid.com/windowsazure.html
  
  [create-new-project]: ./media/sendgrid-dotnet-how-to-send-email/create_new_project.png
  [select-a-template]: ./media/sendgrid-dotnet-how-to-send-email/select_a_template.png
  [SendGrid-NuGet-package]: ./media/sendgrid-dotnet-how-to-send-email/sendgrid_nuget.png
  [azure_app_settings]: ./media/sendgrid-dotnet-how-to-send-email/app_settings.png
  [sendgrid-csharp]: https://github.com/sendgrid/sendgrid-csharp
  [SMTP vs. Web API]: https://sendgrid.com/docs/Integrate/index.html
  [App Settings]: https://sendgrid.com/docs/API_Reference/SMTP_API/apps.html
  [SendGrid API documentation]: https://sendgrid.com/docs
  
  [cloud-based email service]: https://sendgrid.com/email-solutions
  [transactional email delivery]: https://sendgrid.com/transactional-email
 

