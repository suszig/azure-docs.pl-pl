<properties 
    pageTitle="SendGrid 電子メール サービスの使用方法 (Node.js) | Microsoft Azure" 
    description="Azure で SendGrid 電子メール サービスを使用して電子メールを送信する方法について説明します。 コード サンプルは Node.js API を使用して記述されています。" 
    services="" 
    documentationCenter="nodejs" 
    authors="erikre" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="08/31/2015" 
    ms.author="erikre"/>
# SendGrid を使用して Node.js から電子メールを送信する方法

このガイドでは、Azure の SendGrid 電子メール サービスを使用して
一般的なプログラム タスクを実行する方法を紹介します。 サンプルは Node.js API を使用して
記述されています。 紹介するシナリオ **電子メールの作成**,、
**電子メールの送信**, 、**添付ファイルの追加**, 、**フィルターを使用して**, と
**プロパティを更新**します。 SendGrid と電子メールの送信の詳細については、
参照してください、 [次のステップ](#next-steps) セクションです。

## SendGrid 電子メール サービスとは

SendGrid は、[クラウド ベース電子メール サービス] を信頼性の高いを提供します。
[トランザクション電子メール配信]、スケーラビリティ、およびリアルタイム分析の柔軟な Api 機能
を備えているためカスタム統合も容易です。 SendGrid の一般的な使用シナリオを以下に
示します。

-   顧客に受信通知を自動送信する
-   顧客に広告メールを月 1 回送信するための
    配布リストを管理する
-   ブロックされた電子メールや顧客の応答性などを表すメトリックをリアルタイムで
    収集する
-   傾向を認識するために役立つレポートを生成する
-   顧客の問い合わせを転送する
-   アプリケーションからの電子メール通知

詳細については、次を参照してください。 [https://sendgrid.com](https://sendgrid.com)します。

## SendGrid アカウントの作成

[AZURE.INCLUDE [sendgrid-sign-up](../includes/sendgrid-sign-up.md)]

## SendGrid Node.js モジュールの参照

Node.js 用の SendGrid モジュールは、次のコマンドを使用することによって、
ノード パッケージ マネージャー (npm) でインストールできます。

    npm install sendgrid

インストールした後、次のコードを使用して、アプリケーションで
モジュールを要求できます。

    var sendgrid = require('sendgrid')(sendgrid_username, sendgrid_password);

SendGrid モジュールがエクスポート、 **SendGrid** と **電子メール** 関数です。
**SendGrid** の Web API による電子メールの送信 
中に **電子メール** 電子メール メッセージをカプセル化します。

## 方法: 電子メールを作成する

SendGrid モジュールを使って電子メール メッセージを作成するには、
最初に Email 機能で電子メール メッセージを作成し、
次に SendGrid 機能でメッセージを送信します。 以下は、Email 関数を使用した新しいメッセージの作成の
例です。

    var email = new sendgrid.Email({
        to: 'john@contoso.com',
        from: 'anna@contoso.com',
        subject: 'test mail',
        text: 'This is a sample email message.'
    });

また、html プロパティを設定することによって、HTML メッセージをサポートするクライアント向けに HTML メッセージ
を指定することもできます。 次に例を示します。

    html: This is a sample <b>HTML<b> email message.

text プロパティと html プロパティの両方を設定すると、HTML メッセージを
サポートしないクライアント向けに正常なフォールバックができます。

Email 関数でサポートされるプロパティの詳細については、
[sendgrid nodejs] を参照してください。

## 方法: 電子メールを送信する

Email 関数で電子メール メッセージを作成した後、SendGrid の
Web API を使用してメッセージを送信できます。 

### Web API

    sendgrid.send(email, function(err, json){
        if(err) { return console.error(err); }
        console.log(json);
    });

> [AZURE.NOTE] 上記の例については、電子メール オブジェクトの受け渡しを示して、
受け渡しを示していますが、電子メールのプロパティを直接指定
することによって、send 関数を直接呼び出すこともできます。 次に例を示します。  
>
>`````
sendgrid.send({
    : 'john@contoso.com'
    : 'anna@contoso.com'
    件名: [テスト メール]
    テキスト: 'これは、電子メール メッセージの例です' です。
});
`````

## How to: Add an Attachment

Attachments can be added to a message by specifying the file name(s) and
path(s) in the **files** property. The following example demonstrates
sending an attachment:

    sendgrid.send({
        to: 'john@contoso.com',
        from: 'anna@contoso.com',
        subject: 'test mail',
        text: 'This is a sample email message.',
        files: [
            {
                filename:     '',           // required only if file.content is used.
                contentType:  '',           // optional
                cid:          '',           // optional, used to specify cid for inline content
                path:         '',           //
                url:          '',           // == One of these three options is required
                content:      ('' | Buffer) //
            }
        ],
    });

> [AZURE.NOTE] When using the **files** property, the file must be accessible
through [fs.readFile](http://nodejs.org/docs/v0.6.7/api/fs.html#fs.readFile). If the file you wish to attach is hosted in Azure Storage, such as in a Blob container, you must first copy the file to local storage or to an Azure drive before it can be sent as an attachment using the **files** property.

## How to: Use Filters to Enable Footers and Tracking

SendGrid provides additional email functionality through the use of
filters. These are settings that can be added to an email message to
enable specific functionality such as enabling click tracking, Google
analytics, subscription tracking, and so on. For a full list of filters,
see [Filter Settings][].

Filters can be applied to a message by using the **filters** property.
Each filter is specified by a hash containing filter-specific settings.
The following examples demonstrate the footer and click tracking filters:

### Footer

    var email = new sendgrid.Email({
        to: 'john@contoso.com',
        from: 'anna@contoso.com',
        subject: 'test mail',
        text: 'This is a sample email message.'
    });
    
    email.setFilters({
        'footer': {
            'settings': {
                'enable': 1,
                'text/plain': 'This is a text footer.'
            }
        }
    });

    sendgrid.send(email);

### Click Tracking

    var email = new sendgrid.Email({
        to: 'john@contoso.com',
        from: 'anna@contoso.com',
        subject: 'test mail',
        text: 'This is a sample email message.'
    });
    
    email.setFilters({
        'clicktrack': {
            'settings': {
                'enable': 1
            }
        }
    });
    
    sendgrid.send(email);

## How to: Update Email Properties

Some email properties can be overwritten using **set*Property*** or
appended using **add*Property***. For example, you can add additional
recipients by using

    email.addTo('jeff@contoso.com');

or set a filter by using

    email.addFilter('footer', 'enable', 1);
    email.addFilter('footer', 'text/html', '<strong>boo</strong>');

For more information, see [sendgrid-nodejs][].

## How to: Use Additional SendGrid Services

SendGrid offers web-based APIs that you can use to leverage additional
SendGrid functionality from your Azure application. For full
details, see the [SendGrid API documentation][].

## Next Steps

Now that you've learned the basics of the SendGrid Email service, follow
these links to learn more.

-   SendGrid Node.js module repository: [sendgrid-nodejs][]
-   SendGrid API documentation:
    <https://sendgrid.com/docs>
-   SendGrid special offer for Azure customers:
    [http://sendgrid.com/azure.html](https://sendgrid.com/windowsazure.html)
  [special offer]: https://sendgrid.com/windowsazure.html
  [sendgrid-nodejs]: https://github.com/sendgrid/sendgrid-nodejs
  [Filter Settings]: https://sendgrid.com/docs/API_Reference/SMTP_API/apps.html
  [SendGrid API documentation]: https://sendgrid.com/docs
  [cloud-based email service]: https://sendgrid.com/email-solutions
  [transactional email delivery]: https://sendgrid.com/transactional-email

