<properties 
    pageTitle="Twilio から通話する方法 (PHP) | Microsoft Azure" 
    description="Azure で Twilio API サービスを使用して通話や SMS メッセージの送信を行う方法について学習します。 サンプルは PHP アプリケーション用です。" 
    documentationCenter="php" 
    services="" 
    authors="devinrader" 
    manager="twilio" 
    editor="mollybos"/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="PHP" 
    ms.topic="article" 
    ms.date="11/25/2014" 
    ms.author="microsofthelp@twilio.com"/>

# Azure 上の PHP アプリケーションで Twilio を使用して通話する方法 

次の例では、Azure でホストされる PHP Web ページから Twilio を使用して通話する方法を示しています。 次のスクリーン ショットに示すように、作成されたアプリケーションは通話に関する値の入力をユーザーに求めます。

![Twilio と PHP を使用する Azure 通話フォーム][twilio_php]

このトピックでコードを使用するためには次の操作を行う必要があります。

1. Twilio アカウントと認証トークンを取得します。 Twilio を使用する評価で価格を [http://www.twilio.com/pricing][twilio_pricing]します。 試用アカウントにサインアップすることができます [https://www.twilio.com/try-twilio][try_twilio]します。 Twilio から提供される API については、次を参照してください。 [http://www.twilio.com/api][twilio_api]します。
2. PHP 用 Twilio ライブラリを入手します。 GitHub からダウンロードできます ([https://github.com/twilio/twilio-php][twilio_php_github]) か、PEAR パッケージとしてインストールします。 詳細については、次を参照してください。 [https://github.com/twilio/twilio-php/blob/master/README.md][twilio_github_readme]します。
3. Azure SDK for PHP をインストールします。 SDK およびそのインストール手順についての概要については、次を参照してください。 [Azure SDK for PHP セットアップ][setup_php_sdk]します。

## 通話用の Web フォームの作成

次の HTML コードは、web ページを作成する方法を示します (**callform.html**) の呼び出しを行うためのユーザー データを取得します。

    <html>
    <head>
        <title>Automated call form</title>
    </head>
    <body>
    <h1>Automated Call Form</h1>
    <p>Fill in all fields and click <b>Make this call</b>.</p>
    <form action="makecall.php" method="post">
    <table>
        <tr>
            <td>To:</td>
            <td><input type="text" size=50 name="callTo" value=""></td>
        </tr>
        <tr>
            <td>From:</td>
            <td><input type="text" size=50 name="callFrom" value=""></td>
        </tr>
        <tr>
            <td>Call message:</td>
            <td><input type="text" size=100 name="callText" value="Hello. This is the call text. Good bye." /></td>
        </tr>
        <tr>
            <td colspan=2><input type="submit" value="Make this call"></td>
        </tr>
    </table>
    </form>
    <br/>
    </body>
    </html>

## 通話用のコードの作成
次のコードは、web ページを作成する方法を示します (**makecall.php**)、ユーザーによって表示されるフォームを送信するときに呼び出されるか **callform.html**します。 次のコードによって通話メッセージが作成され、通話が生成されます (に割り当てられたプレース ホルダー値の代わりには、Twilio アカウントと認証トークンを使用して **$sid** と **$token** 次のコードにします)。

    <html>
    <head><title>Making call...</title></head>
    <body>
    <p>Your call is being made.</p>

    <?php
    require_once 'Services/Twilio.php';

    $sid = "your_account_sid";
    $token = "your_authentication_token";

    $from_number = $_POST['callFrom']; // Calls must be made from a registered Twilio number.
    $to_number = $_POST['callTo'];
    $message = $_POST['callText'];

    $client = new Services_Twilio($sid, $token, "2010-04-01");

    $call = $client->account->calls->create(
        $from_number, 
        $to_number,
        'http://twimlets.com/message?Message='.urlencode($message)
    );

    echo "Call status: ".$call->status."<br />";
    echo "URI resource: ".$call->uri."<br />";
    ?>
    </body>
    </html>

呼び出しを行うだけでなく **makecall.php** いくつかの通話メタデータ (例を次のスクリーン ショットに示すように) が表示されます。 通話メタデータの詳細については、次を参照してください。 [https://www.twilio.com/docs/api/rest/call#instance-properties][twilio_call_properties]します。

![Twilio と PHP を使用する Azure 通話応答][twilio_php_response]

## アプリケーションの実行
次のステップでは、アプリケーションを Azure Websites にデプロイします。 この後に示している記事は、Web サイトの作成と、Git、FTP、または WebMatrix によるコードのデプロイに関連しています (ただし各記事内のすべての情報が関連しているわけではありません)。

* [PHP-MySQL Azure の Web サイトを作成して Git で展開する][website-git]
* [PHP-MySQL Azure の Web サイトを作成して FTP で展開する][website-ftp]
* [WebMatrix を使用して PHP-MySQL Azure の Web サイトを作成および展開する][website-webmatrix]

## 次のステップ
Azure 上の PHP で Twilio を使用した基本機能を示すために、このコードが用意されました。 運用環境で Azure にデプロイする前に、エラー処理やその他の機能をさらに追加することができます。 次に例を示します。

* Web フォームを使用する代わりに、Azure ストレージ BLOB または SQL Database を使用して、電話番号と通話テキストを保存できます。 PHP で Azure ストレージ blob の使用方法の詳細については、次を参照してください。 [PHP アプリケーションでの Azure ストレージの使用][howto_blob_storage_php]します。 PHP で SQL データベースの使用方法の詳細については、次を参照してください。 [PHP アプリケーションでの SQL Database][howto_sql_azure_php]します。
*  **Makecall.php** コードは Twilio から提供される URL を使用して ([http://twimlets.com/message][twimlet_message_url]) 呼び出しをする方法を Twilio に通知する Twilio Markup Language (TwiML) 応答を提供します。 たとえば、返される TwiML 応答に `<Say>` 動詞を含めて、通話受信者に対してテキストが読み上げられるようにできます。 Twilio から提供される URL を使用する代わりに Twilio の要求に応答するサービスを作成することができます。詳細については、次を参照してください。 [音声および SMS 機能を PHP で Twilio を使用する方法][howto_twilio_voice_sms_php]します。 TwiML の詳細については掲載されて [http://www.twilio.com/docs/api/twiml][twiml], 、および詳細情報 `<Say>` し、その他の Twilio 動詞は掲載されて [http://www.twilio.com/docs/api/twiml/say][twilio_say]します。
* Twilio に関するセキュリティ ガイドライン [https://www.twilio.com/docs/security][twilio_docs_security]します。

Twilio の詳細については、次を参照してください。 [https://www.twilio.com/docs][twilio_docs]します。

## 関連項目
* [PHP で音声および SMS 機能に Twilio を使用する方法](partner-twilio-php-how-to-use-voice-sms.md)

[twilio_pricing]: http://www.twilio.com/pricing
[try_twilio]: http://www.twilio.com/try-twilio
[twilio_api]: http://www.twilio.com/api
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#
[twilio_php]: https://github.com/twilio/twilio-php
[twilio_github_readme]: https://github.com/twilio/twilio-php/blob/master/README.md
[setup_php_sdk]: http://azurephp.interoperabilitybridges.com/articles/setup-the-windows-azure-sdk-for-php
[twimlet_message_url]: http://twimlets.com/message
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api_service]: http://api.twilio.com
[build_php_azure_app]: http://azurephp.interoperabilitybridges.com/articles/build-and-deploy-a-windows-azure-php-application
[howto_twilio_voice_sms_php]: partner-twilio-php-how-to-use-voice-sms.md
[howto_blob_storage_php]: http://azure.microsoft.com/documentation/articles/storage-php-how-to-use-blobs/
[howto_sql_azure_php]: http://azure.microsoft.com/documentation/articles/sql-database-php-how-to-use/
[twilio_call_properties]: https://www.twilio.com/docs/api/rest/call#instance-properties
[twilio_docs_security]: http://www.twilio.com/docs/security
[twilio_docs]: http://www.twilio.com/docs
[twilio_say]: http://www.twilio.com/docs/api/twiml/say
[ssl_validation]: http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html
[twilio_php]: ./media/partner-twilio-php-make-phone-call/WA_TwilioPHPCallForm.jpg
[twilio_php_response]: ./media/partner-twilio-php-make-phone-call/WA_TwilioPHPMakeCall.jpg
[website-git]: https://www.windowsazure.com/develop/php/tutorials/website-w-mysql-and-git/
[website-ftp]: https://www.windowsazure.com/develop/php/tutorials/website-w-mysql-and-ftp/
[website-webmatrix]: https://www.windowsazure.com/develop/php/tutorials/website-w-mysql-and-webmatrix/
[twilio_php_github]: https://github.com/twilio/twilio-php

