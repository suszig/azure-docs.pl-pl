<properties 
    pageTitle="音声および SMS に Twilio を使用する方法 (PHP) | Microsoft Azure" 
    description="Azure で Twilio API サービスを使用して通話や SMS メッセージの送信を行う方法について学習します。コード サンプルは PHP で記述されています。" 
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


# PHP で音声および SMS 機能に Twilio を使用する方法

このガイドでは、Azure の Twilio API サービスを使用して一般的なプログラミング タスクを実行する方法を紹介します。 電話の発信と Short Message Service (SMS) メッセージの送信の各シナリオについて説明します。 アプリケーションで音声と SMS を使用して Twilio の詳細については、次を参照してください。、 [次のステップ](#NextSteps) セクションです。

## <a id="WhatIs"></a>Twilio とは何ですか。

Twilio は、開発者がアプリケーションに音声、VoIP、およびメッセージングを埋め込むことを可能にし、ビジネス コミュニケーションを強化していきます。 必要なすべてのインフラストラクチャをクラウド ベースのグローバル環境で仮想化し、Twilio 通信 API プラットフォームを通じてそれを公開します。 アプリケーションは構築しやすく、スケーラビリティも優れています。 従量課金制の柔軟性と、クラウドの信頼性の利点を活用できます。

**Twilio Voice** を使用すると、アプリケーションで音声通話の発着信処理を行うことができます。 **Twilio SMS** を使用すると、アプリケーションでテキスト メッセージの送受信を行うことができます。 **Twilio Client** では、任意の電話、タブレット、またはブラウザーから VoIP 通話を行うことができ、WebRTC がサポートされています。

## <a id="Pricing"></a>Twilio の料金および特別プラン

Azure ユーザーは、 [特別プラン](http://www.twilio.com/azure): $10 の Twilio クレジット、Twilio アカウントをアップグレードするとします。 この Twilio クレジットは、任意の Twilio 使用に対して利用できます。$10 のクレジットは、約 1,000 件の SMS メッセージの送信、または最大で 1,000 分の受信音声に相当します (ご利用の電話番号の場所と、メッセージまたは通話の相手の場所に応じて異なります)。 この Twilio クレジットを利用するに: [http://ahoy.twilio.com/azure](http://ahoy.twilio.com/azure)します。

Twilio は、従量課金制サービスです。 セットアップ料金は不要で、いつでもアカウントを閉じることができます。 詳細についてを検索する [Twilio の料金 ][twilio_pricing]します。

## <a id="Concepts"></a>概念

Twilio API は、アプリケーションに音声および SMS 機能を提供する REST ベースの API です。 クライアント ライブラリは、複数の言語で利用可能です一覧については、次を参照してください。 [Twilio API ライブラリに関する ][twilio_libraries]します。

Twilio API の主要な側面として、Twilio 動詞と Twilio Markup Language (TwiML) が挙げられます。

### <a id="Verbs"></a>Twilio 動詞

API では、Twilio 動詞を使用します。たとえば、**&lt;Say&gt;** 動詞は、メッセージを音声で返すことを Twilio に指示します。

Twilio 動詞の一覧を次に示します。 その他の動詞と機能について学習 [Twilio Markup Language のドキュメント](http://www.twilio.com/docs/api/twiml)します。

* **&lt;Dial&gt;**: 呼び出し元を別の電話に接続します。
* **&lt;Gather&gt;**: 電話キーパッドに入力された数字を収集します。
* **&lt;Hangup&gt;**: 通話を終了します。
* **&lt;Play&gt;**: 音声ファイルを再生します。
* **&lt;Pause&gt;**: 何も行わずに指定された秒数待機します。
* **&lt;Record&gt;**: 呼び出し元の声を録音し、声が録音されたファイルの URL を返します。
* **&lt;Redirect&gt;**: 通話または SMS の制御を別の URL の TwiML に転送します。
* **&lt;Reject&gt;**: Twilio 番号への着信通話を拒否します。課金はされません。
* **&lt;Say&gt;**: テキストを音声に変換して返します。
* **&lt;Sms&gt;**: SMS メッセージを送信します。

### <a id="TwiML"></a>TwiML

TwiML は、Twilio 動詞に基づいた XML ベースの命令のセットで、通話または SMS をどのように処理するかを Twilio に通知します。

たとえば、次の TwiML は、テキスト **Hello World** を音声に変換します。

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
       <Say>Hello World</Say>
    </Response>

アプリケーションで Twilio API を呼び出す場合は、API パラメーターの 1 つである URL によって TwiML 応答が返されます。 開発用には、Twilio から提供される URL を使用して、アプリケーションで使用する TwiML 応答を提供することができます。 また、独自に URL をホストして、TwiML 応答を生成することもできます。別のオプションとして、**TwiMLResponse** オブジェクトを使用することもできます。

Twilio の動詞、属性、および TwiML の詳細については、次を参照してください。 [TwiML ][twiml]します。 Twilio API に関する詳細については、次を参照してください。 [Twilio API ][twilio_api]します。

## <a id="CreateAccount"></a>Twilio アカウントを作成します。

Twilio アカウントを取得する準備ができたらでサインアップ [Twilio ][try_twilio]します。 無料アカウントで始め、後でアカウントをアップグレードすることができます。

Twilio アカウントにサインアップすると、アカウント ID と認証トークンが発行されます。 Twilio API を呼び出すには、この両方が必要になります。 自分のアカウントが不正にアクセスされないように、認証トークンを安全に保管してください。 アカウント ID と認証トークンは確認できます、 [Twilio アカウント ページの ][twilio_account], 、フィールドで **アカウント SID** と **AUTH TOKEN**, 、それぞれします。


## <a id="create_app"></a>PHP アプリケーションを作成します。

Twilio サービスを使用する Azure 上の PHP アプリケーションと、Twilio サービスを使用するその他の PHP アプリケーションに違いはありません。 この記事は Twilio サービスを使用する方法について重点的 Twilio サービスは REST ベースであり、いくつかの方法で、PHP から呼び出すことができる、 [GitHub ][twilio_php]します。 PHP 用 Twilio ライブラリの使用に関する詳細については、次を参照してください。 [http://readthedocs.org/docs/twilio-php/en/latest/index.html ][twilio_lib_docs]します。

ビルドと Twilio と PHP アプリケーションを Azure に展開する詳細な手順については、「 [Azure ][howto_phonecall_php]します。

## <a id="configure_app"></a>Twilio ライブラリを使用するアプリケーションを構成します。

PHP 用 Twilio ライブラリを使用するようにアプリケーションを構成するには、次の 2 つの方法があります。

1. GitHub から PHP 用 Twilio ライブラリをダウンロード ([https://github.com/twilio/twilio-php ][twilio_php]) を追加し、 **サービス** ディレクトリをアプリケーションにします。

    - または -

2. PHP 用 Twilio ライブラリを PEAR パッケージとしてインストールします。 インストールには、次のコマンドを使用できます。

        $ pear channel-discover twilio.github.com/pear
        $ pear install twilio/Services_Twilio


PHP 用 Twilio ライブラリをインストールしたら、ライブラリを参照する **require_once** ステートメントを PHP ファイルの先頭に追加できます。

        require_once 'Services/Twilio.php';

詳細については、次を参照してください。 [https://github.com/twilio/twilio-php/blob/master/README.md ][twilio_github_readme]します。

## <a id="howto_make_call"></a>方法: 発信通話を行う

次のコードでは、**Services_Twilio** クラスを使用して発信通話を行う方法を示しています。 このコードは、Twilio から提供されるサイトも使用して、Twilio Markup Language (TwiML) 応答を返します。 コードを実行する前に、**From** および **To** の電話番号の値を置き換えて、Twilio アカウントの **From** の電話番号を確認します。

    // Include the Twilio PHP library.
    require_once 'Services/Twilio.php';
    
    // Library version.
    $version = "2010-04-01";
    
    // Set your account ID and authentication token.
    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";
    
    // The number of the phone initiating the the call.
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


- **注**: SSL 証明書の検証エラーのトラブルシューティングを行う、を参照してください [http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html ][ssl_validation]。


## <a id="howto_send_sms"></a>方法: SMS メッセージを送信

次のコードでは、**Services_Twilio** クラスを使用して SMS メッセージを送信する方法を示しています。 試用アカウントで SMS メッセージを送信できるように、**From** の番号が Twilio から提供されます。 コードを実行する前に、Twilio アカウントの **To** の番号を確認する必要があります。

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
        $client->$client->account->messages->sendMessage($from_number, $to_number, $message);
    }
    catch (Exception $e) 
    {
        echo 'Error: ' . $e->getMessage();
    }

## <a id="howto_provide_twiml_responses"></a>方法: 独自の web サイトから TwiML 応答

アプリケーションで Twilio API の呼び出しを開始すると、Twilio は TwiML 応答を返すことが想定されている URL にユーザーの要求を送信します。上記の例は、Twilio から提供される URL を使用して [http://twimlets.com/message ][twimlet_message_url]します。(TwiML は Twilio で使用するように設計されており、ブラウザーで表示できます。たとえば、クリックして [http://twimlets.com/message ][twimlet_message_url] すると、空 `< 応答 >` 要素である別の例として次のようにクリックします [http://twimlets.com/message?Message%5B0%5D=Hello%20World ][twimlet_message_url_hello_world] を表示する、 `< 応答 >` を含む要素、 `< Say >` 要素です。)。

Twilio から提供される URL を使用する代わりに、HTTP 応答を返す独自のサイトを作成できます。 XML 応答を返すサイトは任意の言語で作成できます。このトピックでは、PHP を使用して TwiML を作成します。

次の PHP ページでは、通話時の TwiML 応答で "**Hello World**" というテキストが読み上げられます。

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

TwiML の詳細については、次を参照してください。 [https://www.twilio.com/docs/api/twiml ][twiml_reference]します。

渡される URL としてに PHP ページの URL を使用して、TwiML 応答を提供するように設定 PHP ページを作成したら、  `Services_Twilio アカウント]-> [呼び出し]->-> [作成`  メソッドです。 という名前の Web アプリケーションがある場合など **された MyTwiML** に配置された Azure ホステッド サービス、および、その PHP ページの名前は **mytwiml.php**, に URL を渡すことが  **Services_Twilio アカウント]-> [呼び出し]->-> [作成**  次の例で示すように。

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

詳細については、azure で PHP に基づいて Twilio を使用して、次を参照してください。 [Azure ][howto_phonecall_php]します。

## <a id="AdditionalServices"></a>方法: その他の Twilio サービスを使用します。

ここに示す例以外にも、Twilio が提供する Web ベースの API を使用して、Azure アプリケーションからその他の Twilio 機能を利用することができます。 詳細については、次を参照してください。、 [Twilio API に関するドキュメント ][twilio_api_documentation]します。

## <a id="NextSteps"></a>次のステップ

これで、Twilio サービスの基本を学習できました。さらに詳細な情報が必要な場合は、次のリンク先を参照してください。

* [Twilio のセキュリティに関するガイドライン ][twilio_security_guidelines]
* [Twilio に関する方法とコードの例 ][twilio_howtos]
* [Twilio のクイック スタート チュートリアル ][twilio_quickstarts]
* [[に関するページ twilio_on_github] GitHub 上の Twilio][twilio_on_github]
* [Twilio に関するサポート ][twilio_support]


[twilio_php]: https://github.com/twilio/twilio-php 
[twilio_lib_docs]: http://readthedocs.org/docs/twilio-php/en/latest/index.html 
[twilio_github_readme]: https://github.com/twilio/twilio-php/blob/master/README.md 
[ssl_validation]: http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html 
[twilio_api_service]: https://api.twilio.com 
[howto_phonecall_php]: partner-twilio-php-make-phone-call.md 
[twilio_voice_request]: https://www.twilio.com/docs/api/twiml/twilio_request 
[twilio_sms_request]: https://www.twilio.com/docs/api/twiml/sms/twilio_request 
[misc_role_config_settings]: http://msdn.microsoft.com/library/windowsazure/hh690945.aspx 
[twimlet_message_url]: http://twimlets.com/message 
[twimlet_message_url_hello_world]: http://twimlets.com/message?Message%5B0%5D=Hello%20World 
[twiml_reference]: https://www.twilio.com/docs/api/twiml 
[twilio_pricing]: http://www.twilio.com/pricing 
[special_offer]: http://ahoy.twilio.com/azure 
[twilio_libraries]: https://www.twilio.com/docs/libraries 
[twiml]: http://www.twilio.com/docs/api/twiml 
[twilio_api]: http://www.twilio.com/api 
[try_twilio]: https://www.twilio.com/try-twilio 
[twilio_account]: https://www.twilio.com/user/account 
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified# 
[twilio_api_documentation]: http://www.twilio.com/api 
[twilio_security_guidelines]: http://www.twilio.com/docs/security 
[twilio_howtos]: http://www.twilio.com/docs/howto 
[twilio_on_github]: https://github.com/twilio 
[twilio_support]: http://www.twilio.com/help/contact 
[twilio_quickstarts]: http://www.twilio.com/docs/quickstart 

