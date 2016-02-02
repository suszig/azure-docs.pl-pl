<properties 
    pageTitle="音声および SMS に Twilio を使用する方法 (Ruby) | Microsoft Azure" 
    description="Azure で Twilio API サービスを使用して通話や SMS メッセージの送信を行う方法について学習します。コード サンプルは Ruby で記述されています。" 
    services="" 
    documentationCenter="ruby" 
    authors="devinrader" 
    manager="twilio" 
    editor=""/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ruby" 
    ms.topic="article" 
    ms.date="11/25/2014" 
    ms.author="MicrosoftHelp@twilio.com"/>






# Ruby で音声および SMS 機能に Twilio を使用する方法

このガイドでは、Azure の Twilio API サービスを使用して一般的なプログラミング タスクを実行する方法を紹介します。 電話の発信と Short Message Service (SMS) メッセージの送信の各シナリオについて説明します。 アプリケーションで音声と SMS を使用して Twilio の詳細については、次を参照してください。、 [次のステップ](#NextSteps) セクションです。

## <a id="WhatIs"></a>Twilio とは何ですか。

Twilio は、既存の Web 言語およびスキルを使用して音声および SMS アプリケーションの作成を可能にするテレフォニー Web サービス API です。 Twilio は、サードパーティ製のサービスです (Azure の機能および Microsoft 製品ではありません)。

**Twilio Voice** を使用すると、アプリケーションで音声通話の発着信処理を行うことができます。 **Twilio SMS** を使用すると、アプリケーションで SMS メッセージの送受信を行うことができます。 **Twilio Client** を使用すると、アプリケーションに (モバイル接続を含む) 既存のインターネット接続を使用した音声通信を組み込むことができます。

## <a id="Pricing"></a>Twilio の料金および特別プラン

Twilio の料金に関する情報については、「 [Twilio の料金 ][twilio_pricing]します。 Azure ユーザーは、 [特別プラン ][special_offer]: 無料クレジットが 1000年テキストまたは 1,000 分の着信します。 このプランにサインアップまたは取得の詳細については、次を参照してください [http://ahoy.twilio.com/azure ][special_offer]します。

## <a id="Concepts"></a>概念

Twilio API は、アプリケーションに音声および SMS 機能を提供する REST ベースの API です。 クライアント ライブラリは、複数の言語で利用可能です一覧については、次を参照してください。 [Twilio API ライブラリに関する ][twilio_libraries]します。

### <a id="TwiML"></a>TwiML

TwiML は、通話または SMS をどのように処理するかを Twilio に通知する XML ベースの命令のセットです。

たとえば、次の TwiML は、テキスト **Hello World** を音声に変換します。

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
       <Say>Hello World</Say>
    </Response>

すべての TwiML ドキュメントが `< 応答 >` がルート要素として。ここから、Twilio 動詞を使用してアプリケーションの動作を定義します。

### <a id="Verbs"></a>TwiML 動詞

Twilio 動詞は、何を**実行する**かを Twilio に通知する XML タグです。 たとえば、**&lt;Say&gt;** 動詞は、メッセージを音声で返すことを Twilio に指示します。

Twilio 動詞の一覧を次に示します。

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

Twilio の動詞、属性、および TwiML の詳細については、次を参照してください。 [TwiML ][twiml]します。 Twilio API に関する詳細については、次を参照してください。 [Twilio API ][twilio_api]します。

## <a id="CreateAccount"></a>Twilio アカウントを作成します。

Twilio アカウントを取得する準備ができたらでサインアップ [Twilio ][try_twilio]します。 無料アカウントで始め、後でアカウントをアップグレードすることができます。

Twilio アカウントにサインアップすると、アプリケーションで使用できる無料の電話番号が与えられます。 さらに、アカウント SID と認証トークンも受け取ります。 Twilio API を呼び出すには、この両方が必要になります。 自分のアカウントが不正にアクセスされないように、認証トークンを安全に保管してください。 アカウント SID と認証トークンを確認できますが、 [Twilio アカウント ページの ][twilio_account], 、フィールドで **アカウント SID** と **AUTH TOKEN**, 、それぞれします。

### <a id="VerifyPhoneNumbers"></a>電話番号を確認します。

Twilio から与えられた番号の他に、自分で管理している番号 (携帯電話や自宅の電話番号) を確認してアプリケーションで使用することもできます。

電話番号を確認する方法については、次を参照してください。 [番号の管理 ][verify_phone]します。

## <a id="create_app"></a>Ruby アプリケーションを作成します。

Twilio サービスを使用し、Azure で動作している Ruby アプリケーションと、Twilio サービスを使用するその他の Ruby アプリケーションに違いはありません。 この記事は Twilio サービスを使用する方法について重点的 Twilio サービスは rest ベースであり、いくつかの方法で、Ruby から呼び出すことができる、 [[twilio_ruby] Ruby 用 Twilio ヘルパー ライブラリ][twilio_ruby]します。

まず、 [新しい ][azure_vm_setup] 、新しい Ruby web アプリケーションのホストとして機能します。 Rails アプリケーションの作成に関する手順は無視し、VM のセットアップだけを行ってください。 作成するエンドポイントは、外部ポートを 80、内部ポートを 5000 としてください。

次の例で使用するので [Sinatra ][sinatra], 、Ruby 用の非常にシンプルな web フレームワークです。 もちろん、Ruby on Rails などの他の Web フレームワークで Twilio ヘルパー ライブラリを使用することもできます。

新しい VM に SSH で接続し、新しいアプリケーション用のディレクトリを作成します。 そのディレクトリの中に Gemfile というファイルを作成し、次のコードをファイルにコピーします。

    source 'https://rubygems.org'
    gem 'sinatra'
    gem 'thin'

コマンドラインの実行で `バンドル インストール`します。 これにより、上に示した依存関係がインストールされます。 次にという名前のファイルを作成 `web.rb`します。 これが Web アプリケーションのコードを記述する場所になります。 このファイルに次のコードを貼り付けます。

    require 'sinatra'
    
    get '/' do
        "Hello Monkey!"
    end

この時点でことができます、コマンドを実行 `ruby web.rb-p 5000`します。 これにより、単純な Web サーバーがポート 5000 で動き始めます。 Azure VM 用に設定した URL にブラウザーでアクセスすると、このアプリケーションを参照できます。 ブラウザーで Web アプリケーションにアクセスできたら、Twilio アプリケーションの作成を始める準備ができています。

## <a id="configure_app"></a>Twilio を使用するアプリケーションを構成します。

更新することで、Twilio ライブラリを使用する web アプリを構成することができます、 `Gemfile` にこの行を含めます。

    gem 'twilio-ruby'

コマンド ライン上で実行 `バンドル インストール`します。 今すぐ開きます `web.rb` 上部にあるこの行を追加します。

    require 'twilio-ruby'

これで、Ruby 用の Twilio ヘルパー ライブラリを Web アプリケーションで使用するための設定が完了しました。

## <a id="howto_make_call"></a>方法: 発信通話を行う

次のコードでは、発信通話を行う方法を示します。 ここで重要となるのは、Ruby 用の Twilio ヘルパー ライブラリを使用した REST API の呼び出しと、TwiML のレンダリングです。 コードを実行する前に、**From** および **To** の電話番号の値を置き換えて、Twilio アカウントの **From** の電話番号を確認します。

この関数を追加 `web.md`:

    # Set your account ID and authentication token.
    sid = "your_twilio_account_sid";
    token = "your_twilio_authentication_token";
    
    # The number of the phone initiating the the call.
    # This should either be a Twilio number or a number that you've verified
    from = "NNNNNNNNNNN";
    
    # The number of the phone receiving call.
    to = "NNNNNNNNNNN";
    
    # Use the Twilio-provided site for the TwiML response.
    url = "http://yourdomain.cloudapp.net/voice_url";
    
    get '/make_call' do
      # Create the call client.
      client = Twilio::REST::Client.new(sid, token);
    
      # Make the call
      client.account.calls.create(to: to, from: from, url: url)
    end
    
    post '/voice_url' do
      "<Response>
         <Say>Hello Monkey!</Say>
       </Response>"
    end

開く場合 `http://yourdomain.cloudapp.net/make_call` 、ブラウザーで通話を発信する Twilio api の呼び出しがトリガーされます。 最初の 2 つのパラメーター `client.account.calls.create` 文字どおり: 番号は、呼び出しは `から` 番号は、呼び出しは、 `に`します。

3 番目のパラメーター (`url`) に電話がつながったときの動作に関する指示を取得する Twilio が要求される URL です。この例では url のセットアップ (`http://yourdomain.cloudapp.net`) 単純な TwiML ドキュメントを返しを使用している、 `< Say >` をいくつかの音声合成を行い、通話の受信者に"こんにちは Monkey"という動詞です。

## <a id="howto_recieve_sms"></a>方法: SMS メッセージを受信

前の例では、**発信**通話を開始しました。 今度は、Twilio へのサインアップ時に受け取った電話番号を使用して、**受信** SMS メッセージを処理します。

最初に、ログイン、 [Twilio ダッシュ ボード ][twilio_account]します。 上部のナビゲーションで [Numbers] をクリックし、自分の Twilio 番号をクリックします。 構成できる URL が 2 つ表示されます。 Voice Request URL と、SMS Request URL です。 これらは、該当する番号に電話がかかってきたとき、または SMS メッセージが送信されたときに Twilio から呼び出される URL です。 これらの URL は "Web フック" とも呼ばれます。

今回は受信 SMS メッセージを処理するので、更新の URL をする `http://yourdomain.cloudapp.net/sms_url`します。 ページの下部にある [Save Changes] をクリックします。 戻り、 `web.rb` 、アプリケーションをプログラミングして次の処理します。

    post '/sms_url' do
      "<Response>
         <Message>Hey, thanks for the ping! Twilio and Azure rock!</Message>
       </Response>"
    end

変更を加えた後は、必ず Web アプリケーションを再起動してください。 次に、電話を使用して Twilio 番号に SMS を送信します。 "Hey, thanks for the ping! Twilio and Azure rock!" という SMS 応答がすぐに返されます。

## <a id="additional_services"></a>方法: その他の Twilio サービスを使用します。

ここに示す例以外にも、Twilio が提供する Web ベースの API を使用して、Azure アプリケーションからその他の Twilio 機能を利用することができます。 詳細については、次を参照してください。、 [Twilio API に関するドキュメント ][twilio_api_documentation]します。

### <a id="NextSteps"></a>次のステップ

これで、Twilio サービスの基本を学習できました。さらに詳細な情報が必要な場合は、次のリンク先を参照してください。

* [Twilio のセキュリティに関するガイドライン ][twilio_security_guidelines]
* [Twilio に関する方法とコードの例 ][twilio_howtos]
* [Twilio のクイック スタート チュートリアル ][twilio_quickstarts]
* [[に関するページ twilio_on_github] GitHub 上の Twilio][twilio_on_github]
* [Twilio に関するサポート ][twilio_support]


[twilio_ruby]: https://www.twilio.com/docs/ruby/install 
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
[sinatra]: http://www.sinatrarb.com/ 
[azure_vm_setup]: http://www.windowsazure.com/develop/ruby/tutorials/web-app-with-linux-vm/ 

