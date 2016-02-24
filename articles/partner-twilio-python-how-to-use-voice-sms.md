<properties
    pageTitle="音声および SMS に Twilio を使用する方法 (PHP) | Microsoft Azure"
    description="Azure で Twilio API サービスを使用して通話や SMS メッセージの送信を行う方法について学習します。 コード サンプルは PHP で記述されています。"
    services=""
    documentationCenter="python"
    authors="devinrader"
    manager="twilio"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="python" 
    ms.topic="article"
    ms.date="02/19/2015"
    ms.author="MicrosoftHelp@twilio.com"/>





# PHP で音声および SMS 機能に Twilio を使用する方法
このガイドでは、Azure の Twilio API サービスを使用して一般的なプログラミング タスクを実行する方法を紹介します。 電話の発信と Short Message Service (SMS) メッセージの送信の各シナリオについて説明します。 アプリケーションで音声と SMS を使用して Twilio の詳細については、次を参照してください。、 [次のステップ](#NextSteps) セクションです。

## <a id="WhatIs"></a>Twilio とは
Twilio は、開発者がアプリケーションに音声、VoIP、およびメッセージングを埋め込むことを可能にし、ビジネス コミュニケーションを強化していきます。 必要なすべてのインフラストラクチャをクラウド ベースのグローバル環境で仮想化し、Twilio 通信 API プラットフォームを通じてそれを公開します。 アプリケーションは構築しやすく、スケーラビリティも優れています。 従量課金制の柔軟性と、クラウドの信頼性の利点を活用できます。

**Twilio Voice** にアプリケーションを電話の通話を送受信します。 **Twilio SMS** アプリケーションでテキスト メッセージを送受信できます。 **Twilio Client** 、電話、タブレット、またはブラウザーから VoIP 通話を行うことができますし、WebRTC がサポートされています。

## <a id="Pricing"></a>Twilio の料金および特別プラン

Azure ユーザーには、Twilio アカウントをアップグレードする際に、[特別プラン]$10 の Twilio クレジットが提供されます。 この Twilio クレジットは、任意の Twilio 使用に対して利用できます。$10 のクレジットは、約 1,000 件の SMS メッセージの送信、または最大で 1,000 分の受信音声に相当します (ご利用の電話番号の場所と、メッセージまたは通話の相手の場所に応じて異なります)。 この Twilio クレジットを利用するには、[ahoy.twilio.com/azure] にアクセスします。

Twilio は、従量課金制サービスです。 セットアップ料金は不要で、いつでもアカウントを閉じることができます。 詳細についてを検索する [Twilio の料金] [twilio_pricing]します。

## <a id="Concepts"></a>概念
Twilio API は、アプリケーションに音声および SMS 機能を提供する REST ベースの API です。 クライアント ライブラリは、複数の言語で利用可能です一覧については、次を参照してください。 [Twilio API ライブラリに関する] [twilio_libraries]します。

Twilio API の主要な側面として、Twilio 動詞と Twilio Markup Language (TwiML) が挙げられます。

### <a id="Verbs"></a>Twilio 動詞
API を利用して Twilio の動詞です。たとえば、 **& lt;Say & gt;** 動詞は、呼び出しにメッセージを音声で返すことを Twilio に指示します。

Twilio 動詞の一覧を次に示します。 他の動詞と機能については、[Twilio Markup Language のドキュメント][http://www.twilio.com/docs/api/twiml] を参照してください。

* **& Lt; ダイヤル & gt;**: 呼び出し元を別の電話に接続します。
* **& lt;ギャザー & gt;**: 電話キーパッドに入力された数字を収集します。
* **& lt;Hangup & gt;**: 通話を終了します。
* **& lt;プレイ & gt;**: 音声ファイルを再生します。
* **& lt;一時停止 & gt;**: 何も行わずに指定した秒数待機します。
* **& lt;レコード & gt;**: 呼び出し元の声を録音し、記録を含むファイルの URL を返します。
* **& lt;リダイレクト & gt;**: 通話または SMS の制御を別の URL の TwiML に転送します。
* **& lt;却下 & gt;**: Twilio 番号への着信通話を拒否します課金はされません。
* **& lt;Say & gt;**: で呼び出しが行われるテキストの音声を変換して返します。
* **& lt;Sms & gt;**: SMS メッセージを送信します。

### <a id="TwiML"></a>TwiML
TwiML は、Twilio 動詞に基づいた XML ベースの命令のセットで、通話または SMS をどのように処理するかを Twilio に通知します。

たとえば、次の TwiML は、テキストを変換 **Hello World** を音声にします。

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
       <Say>Hello World</Say>
    </Response>

アプリケーションで Twilio API を呼び出す場合は、API パラメーターの 1 つである URL によって TwiML 応答が返されます。 開発用には、Twilio から提供される URL を使用して、アプリケーションで使用する TwiML 応答を提供することができます。 TwiML 応答を生成する、独自に Url をホストすることもでき、別のオプションは、使用する、 **TwiMLResponse** オブジェクトです。

Twilio の動詞、属性、および TwiML の詳細については、次を参照してください。 [TwiML] [twiml]します。 Twilio API に関する詳細については、次を参照してください。 [Twilio API] [twilio_api]します。

## <a id="CreateAccount"></a>Twilio アカウントを作成する
Twilio アカウントを取得する準備ができたら、サインアップ ページ [twilio のサインアップ ページ] [try_twilio]します。 無料アカウントで始め、後でアカウントをアップグレードすることができます。

Twilio アカウントにサインアップすると、アカウント ID と認証トークンが発行されます。 Twilio API を呼び出すには、この両方が必要になります。 自分のアカウントが不正にアクセスされないように、認証トークンを安全に保管してください。 アカウント ID と認証トークンは確認できます、 [Twilio アカウント ページ] [twilio_account], 、フィールドで **アカウント SID** と **AUTH TOKEN**, 、それぞれします。

## <a id="create_app"></a>PHP アプリケーションの作成
Twilio サービスを使用する Azure 上の PHP アプリケーションと、Twilio サービスを使用するその他の PHP アプリケーションに違いはありません。 この記事は Twilio サービスを使用する方法について重点的 Twilio サービスは REST ベースであり、いくつかの方法で、PHP から呼び出すことができる、 [GitHub から PHP 用 Twilio ライブラリ][twilio_php]します。 PHP 用 Twilio ライブラリの使用に関する詳細については、次を参照してください。 [http://readthedocs.org/docs/twilio-php/en/latest/index.html][twilio_lib_docs]します。

ビルドと Twilio と PHP アプリケーションを Azure に展開する詳細な手順については、「 [Azure 上の PHP アプリケーションで使用する Twilio 通話を行う方法][howto_phonecall_php]します。

## <a id="configure_app"></a>Twilio ライブラリを使用するようにアプリケーションを構成する
PHP 用 Twilio ライブラリを使用するようにアプリケーションを構成するには、次の 2 つの方法があります。

1. GitHub から PHP 用 Twilio ライブラリをダウンロード ([https://github.com/twilio/twilio-php][twilio_php]) を追加し、 **サービス** ディレクトリをアプリケーションにします。

    - または -

2. PHP 用 Twilio ライブラリを PEAR パッケージとしてインストールします。 インストールには、次のコマンドを使用できます。

        $ pear channel-discover twilio.github.com/pear
        $ pear install twilio/Services_Twilio

追加できますし、PHP 用 Twilio ライブラリをインストールした後、 **require_once** ライブラリを参照する PHP ファイルの上部にあるステートメント。

        require_once 'Services/Twilio.php';

詳細については、次を参照してください。 [https://github.com/twilio/twilio-php/blob/master/README.md][twilio_github_readme]します。

## <a id="howto_make_call"></a>方法: 発信通話する
発信を使用して通話を行う方法を次に示します、 **Services_Twilio** クラスです。 このコードは、Twilio から提供されるサイトも使用して、Twilio Markup Language (TwiML) 応答を返します。 値を置き換えて、 **から** と **に** 電話番号を確認することを確認して、 **から** コードを実行する前に、Twilio アカウント用の電話番号。

    // Include the Twilio PHP library.
    require_once 'Services/Twilio.php';

    // Library version.
    $version = "2010-04-01";

    // Set your account ID and authentication token.
    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";

    // The number of the phone initiating the the call.
    // (Must be previously validated with Twilio.)
    $from_number = "NNNNNNNNNNN";

    // The number of the phone receiving call.
    $to_number = "NNNNNNNNNNN";

    // Use the Twilio-provided site for the TwiML response.
    $url = "http://twimlets.com/message";

    // The phone message text.
    $message = "Hello world.";

    // Create the call client.
    $client = new Services_Twilio($sid, $token, $version);

    //Make the call.
    try
    {
        $call = $client->account->calls->create(
            $from_number,
            $to_number,
            $url.'?Message='.urlencode($message)
        );
    }
    catch (Exception $e)
    {
        echo 'Error: ' . $e->getMessage();
    }

既に説明したように、このコードは Twilio から提供されるサイトを使用して、TwiML 応答を返します。 代わりに TwiML 応答を提供するのに独自のサイトを使用する可能性があります。詳細については、次を参照してください。 [独自の Web サイトから TwiML 応答を返す方法](#howto_provide_twiml_responses)します。


- **注**: SSL 証明書の検証エラーをトラブルシューティングするには、次を参照してください [http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html。][ssl_validation]


## <a id="howto_send_sms"></a>方法: SMS メッセージを送信する
使用して SMS メッセージを送信する方法を次に示します、 **Services_Twilio** クラスです。  **から** 試用アカウントを Twilio に SMS メッセージを送信して番号を提供します。  **に** コードを実行する前に、Twilio アカウントの数を確認する必要があります。

    // Include the Twilio PHP library.
    require_once 'Services/Twilio.php';

    // Library version.
    $version = "2010-04-01";

    // Set your account ID and authentication token.
    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";


    $from_number = "NNNNNNNNNNN"; // With trial account, texts can only be sent from your Twilio number.
    $to_number = "NNNNNNNNNNN";
    $message = "Hello world.";

    // Create the call client.
    $client = new Services_Twilio($sid, $token, $version);

    // Send the SMS message.
    try
    {
        $client->account->sms_messages->create($from_number, $to_number, $message);
    }
    catch (Exception $e)
    {
        echo 'Error: ' . $e->getMessage();
    }

## <a id="howto_provide_twiml_responses"></a>方法: 独自の web サイトから TwiML 応答
アプリケーションで Twilio API の呼び出しを開始すると、Twilio は TwiML 応答を返すことが想定されている URL にユーザーの要求を送信します。 上記の例は、Twilio から提供される URL を使用して [http://twimlets.com/message][twimlet_message_url]します。 (TwiML は Twilio で使用するように設計されており、ブラウザーで表示できます。 たとえば、クリックして [http://twimlets.com/message][twimlet_message_url] すると、空 `<Response>` 要素である別の例として次のようにクリックします [http://twimlets.com/message?Message%5B0%5D=Hello%20World][twimlet_message_url_hello_world] を表示する、 `<Response>` を含む要素、 `<Say>` 要素です。)。

Twilio から提供される URL を使用する代わりに、HTTP 応答を返す独自のサイトを作成できます。 XML 応答を返すサイトは任意の言語で作成できます。このトピックでは、PHP を使用して TwiML を作成するとします。

次の PHP ページでは、TwiML 応答で読み上げ **Hello World** の呼び出しで。

    <?php
        header("content-type: text/xml");
        echo "<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n";
    ?>
    <Response>
        <Say>Hello world.</Say>
    </Response>

この例からわかるように、TwiML 応答は XML ドキュメントにすぎません。 PHP 用 Twilio ライブラリには、TwiML を生成するクラスが用意されています。 次の例は、上記のように同等の応答が生成されますが、使用、 **\_twilio\_twiml** PHP 用 Twilio ライブラリ内のクラス。

    require_once('Services/Twilio.php');

    $response = new Services_Twilio_Twiml();
    $response->say("Hello world.");
    print $response;

TwiML の詳細については、次を参照してください。 [https://www.twilio.com/docs/api/twiml][twiml_reference]します。

渡される URL としてに PHP ページの URL を使用して、TwiML 応答を提供するように設定 PHP ページを作成したら、  `Services_Twilio->account->calls->create`  メソッドです。 という名前の Web アプリケーションがある場合など **された MyTwiML** に配置された Azure ホステッド サービス、および、その PHP ページの名前は **mytwiml.php**, に URL を渡すことが  **Services_Twilio アカウント]-> [呼び出し]->-> [作成**  次の例で示すように。

    require_once 'Services/Twilio.php';

    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";
    $from_number = "NNNNNNNNNNN";
    $to_number = "NNNNNNNNNNN";
    $url = "http://<your_hosted_service>.cloudapp.net/MyTwiML/mytwiml.php";

    // The phone message text.
    $message = "Hello world.";

    $client = new Services_Twilio($sid, $token, "2010-04-01");

    try
    {
        $call = $client->account->calls->create(
            $from_number,
            $to_number,
            $url.'?Message='.urlencode($message)
        );
    }
    catch (Exception $e)
    {
        echo 'Error: ' . $e->getMessage();
    }

詳細については、azure で PHP に基づいて Twilio を使用して、次を参照してください。 [Azure 上の PHP アプリケーションで使用する Twilio の通話を行う方法][howto_phonecall_php]します。

## <a id="AdditionalServices"></a>方法: その他の Twilio サービスを使用する
ここに示す例以外にも、Twilio が提供する Web ベースの API を使用して、Azure アプリケーションからその他の Twilio 機能を利用することができます。 詳細については、次を参照してください。、 [Twilio API に関するドキュメント] [twilio_api_documentation]します。

## <a id="NextSteps"></a>次のステップ
これで、Twilio サービスの基本を学習できました。さらに詳細な情報が必要な場合は、次のリンク先を参照してください。

* [Twilio に関するセキュリティ ガイドラインのページ] [twilio_security_guidelines]
* [Twilio のハウツー ガイドとコード例のページ] [twilio_howtos]
* [Twilio に関するクイック スタート チュートリアルのページ][twilio_quickstarts]
* [GitHub 上の Twilio に関するページ] [twilio_on_github]
* [Twilio に関するサポートへの連絡のページ] [twilio_support]

[twilio_php]: https://github.com/twilio/twilio-php
[twilio_lib_docs]: http://readthedocs.org/docs/twilio-php/en/latest/index.html
[twilio_github_readme]: https://github.com/twilio/twilio-php/blob/master/README.md
[ssl_validation]: http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html

[howto_phonecall_php]: http://windowsazure.com/documentation/articles/partner-twilio-php-make-phone-call
[twimlet_message_url]: http://twimlets.com/message
[twimlet_message_url_hello_world]: http://twimlets.com/message?Message%5B0%5D=Hello%20World
[twiml_reference]: https://www.twilio.com/docs/api/twiml
[twilio_pricing]: http://www.twilio.com/pricing

[twilio_libraries]: https://www.twilio.com/docs/libraries
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api]: http://www.twilio.com/api
[try_twilio]: https://www.twilio.com/try-twilio
[twilio_account]:  https://www.twilio.com/user/account
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#
[twilio_api_documentation]: http://www.twilio.com/api
[twilio_security_guidelines]: http://www.twilio.com/docs/security
[twilio_howtos]: http://www.twilio.com/docs/howto
[twilio_on_github]: https://github.com/twilio
[twilio_support]: http://www.twilio.com/help/contact
[twilio_quickstarts]: http://www.twilio.com/docs/quickstart

