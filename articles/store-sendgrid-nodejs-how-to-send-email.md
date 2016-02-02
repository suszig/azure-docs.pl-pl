<properties 
    pageTitle="SendGrid 電子メール サービスの使用方法 (Node.js) | Microsoft Azure" 
    description="Azure で SendGrid 電子メール サービスを使用して電子メールを送信する方法について説明します。コード サンプルは Node.js API を使用して記述されています。" 
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
**電子メールを送信する**, 、**添付ファイルの追加**, 、**フィルターを使用して**, と
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

SendGrid モジュールは、**SendGrid** 機能および **Email** 機能をエクスポートします。
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
参照してください [、sendgrid-nodejs:operator[]][]します。

## 方法: 電子メールを送信する

Email 関数で電子メール メッセージを作成した後、SendGrid の
Web API を使用してメッセージを送信できます。

### Web API

    sendgrid.send(email, function(err, json){
        if(err) { return console.error(err); }
        console.log(json);
    });

> [AZURE.NOTE] 前の例では、電子メール オブジェクトとコールバック関数の
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

## 方法: 添付ファイルを追加する

添付ファイルをメッセージに追加するには、
パスを **ファイル** プロパティです。 次の例に、
添付ファイルを送信する方法を示します。

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

> [AZURE.NOTE] **files** プロパティを使用する場合、
を通じて [fs.readFile](http://nodejs.org/docs/v0.6.7/api/fs.html#fs.readFile)します。 添付するファイルが、BLOB コンテナーなどの Azure ストレージでホストされている場合、**files** プロパティを使用して添付ファイルとして送信するには、最初にファイルをローカル ストレージまたは Azure ドライブにコピーする必要があります。

## 方法: フィルターを使用してフッターと追跡を有効にする

SendGrid では、"フィルター" を使用することで追加の電子メール機能を
利用することが できます。
その設定を電子メール メッセージに追加することで、クリック追跡、Google 分析、
サブスクリプション追跡などの独自の機能を有効にすることができます。 すべてのフィルターの一覧については、
参照してください [フィルターの設定][]します。

フィルターは、**フィルター**のプロパティを使用してメッセージに適用できます。
各フィルターは、フィルター固有の設定を格納したハッシュで指定します。
次の例に、フッター フィルターとクリック追跡フィルターの使用方法を示します。

### フッター

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

### クリック追跡

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

## 方法: 電子メールのプロパティを更新する

使用して一部の電子メールのプロパティを上書きできる **設定 * プロパティ *** または
使用して追加 **追加 * プロパティ ***します。 たとえば、次のようにして、
新しい受信者を追加できます。

    email.addTo('jeff@contoso.com');

また、次のように、フィルターを設定することもできます。

    email.addFilter('footer', 'enable', 1);
    email.addFilter('footer', 'text/html', '<strong>boo</strong>');

詳細については、次を参照してください。 [、sendgrid-nodejs:operator[]][]します。

## 方法: その他の SendGrid サービスを使用する

SendGrid の Web ベース API を使用して、Azure アプリケーションから
その他の SendGrid 機能を利用できます。 詳細については、
詳細についてを参照してください、 [SendGrid API に関するドキュメントの][]します。

## 次のステップ

これで、SendGrid 電子メール サービスの基本を学習できました。
さらに詳細な情報が必要な場合は、次のリンク先を参照してください。

-   SendGrid Node.js モジュールのリポジトリ: [、sendgrid-nodejs:operator[]][]
-   SendGrid API に関するドキュメント:
    <https://sendgrid.com/docs>
-   Azure ユーザー向けの SendGrid 特別プラン:
    [http://sendgrid.com/azure.html](https://sendgrid.com/windowsazure.html)

[special offer]: https://sendgrid.com/windowsazure.html 
[sendgrid-nodejs]: https://github.com/sendgrid/sendgrid-nodejs 
[filter settings]: https://sendgrid.com/docs/API_Reference/SMTP_API/apps.html 
[sendgrid api documentation]: https://sendgrid.com/docs 
[cloud-based email service]: https://sendgrid.com/email-solutions 
[transactional email delivery]: https://sendgrid.com/transactional-email 

