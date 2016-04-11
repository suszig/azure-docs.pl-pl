<properties 
    pageTitle="Twilio から通話する方法 (Java) | Microsoft Azure" 
    description="Azure 上の Java アプリケーションで Twilio を使用して Web ページから通話する方法について説明します。" 
    services="" 
    documentationCenter="java" 
    authors="devinrader" 
    manager="twilio" 
    editor="mollybos"/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="11/25/2014" 
    ms.author="microsofthelp@twilio.com"/>

# Azure 上の Java アプリケーションで Twilio を使用して通話する方法 

次の例では、Azure でホストされる Java Web ページから Twilio を使用して通話する方法を示しています。 次のスクリーン ショットに示すように、作成されたアプリケーションは通話に関する値の入力をユーザーに求めます。

![Twilio および Java を使用した Azure 通話フォーム][twilio_java]

このトピックでコードを使用するためには次の操作を行う必要があります。

1. Twilio アカウントと認証トークンを取得します。 Twilio を使用する評価で価格を [http://www.twilio.com/pricing][twilio_pricing]します。 サインアップできます [https://www.twilio.com/try-twilio][try_twilio]します。 Twilio から提供される API については、次を参照してください。 [http://www.twilio.com/api][twilio_api]します。
2. Twilio JAR を入手します。  [Https://github.com/twilio/twilio-java][twilio_java_github], 、GitHub のソースをダウンロードし、独自の JAR を作成するまたはビルド済み JAR (依存関係のないにかかわらず) をダウンロードします。
このトピックでのコードは、ビルド済み TwilioJava-3.3.8-with-dependencies JAR を使用して記述されています。
3. JAR を Java ビルド パスに追加します。
4. この Java アプリケーションの作成に Eclipse を使用している場合は、Eclipse のデプロイ アセンブリ機能を使用して、アプリケーション デプロイ ファイル (WAR) に Twilio JAR をインクルードできます。 この Java アプリケーションの作成に Eclipse を使用していない場合、Twilio JAR が Java アプリケーションと同じ Azure ロールにインクルードされており、アプリケーションのクラス パスに追加されていることを確認してください。
5. cacerts キーストアに Equifax Secure Certificate Authority 証明書と MD5 フィンガープリント 67:CB:9D:C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 が格納されていることを確認します (シリアル番号は 35:DE:F4:CF、SHA1 フィンガープリントは D2:32:09:AD:23:D3:14:23:21:74:E4:0D:7F:9D:62:13:97:86:63:3A)。 これは、証明機関 (CA) 証明書、 [https://api.twilio.com][twilio_api_service] サービスで、Twilio Api を使用するときに呼び出されます。 この CA 証明書を JDK の cacerts ストアに追加する方法については、次を参照してください。 [を Java CA 証明書ストアに証明書を追加する][add_ca_cert]です。

情報がさらに、使いやすさ [を作成する Hello World アプリケーションを使用して、Azure Toolkit for Eclipse][azure_java_eclipse_hello_world], 、または、Eclipse を使用していない場合は、Azure で Java アプリケーションをホストするための他の手法では強くお勧めします。

## 通話用の Web フォームの作成

次のコードは、通話するためのユーザー データを取得する Web フォームの作成方法を示しています。 という名前の新しい動的 web プロジェクトをこの例の目的で **TwilioCloud**, 、作成されたと **callform.jsp** JSP ファイルとして追加しました。

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
        pageEncoding="ISO-8859-1" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Automated call form</title>
    </head>
    <body>
     <p>Fill in all fields and click <b>Make this call</b>.</p>
     <br/>
      <form action="makecall.jsp" method="post">
       <table>
         <tr>
           <td>To:</td>
           <td><input type="text" size=50 name="callTo" value="" />
           </td>
         </tr>
         <tr>
           <td>From:</td>
           <td><input type="text" size=50 name="callFrom" value="" />
           </td>
         </tr>
         <tr>
           <td>Call message:</td>
           <td><input type="text" size=400 name="callText" value="Hello. This is the call text. Good bye." />
           </td>
         </tr>
         <tr>
           <td colspan=2><input type="submit" value="Make this call" />
           </td>
         </tr>
       </table>
     </form>
     <br/>
    </body>
    </html>

## 通話用のコードの作成
次のコードは、callform.jsp によって表示されるフォームへの入力が完了すると呼び出され、通話メッセージを作成して通話を生成します。 この例の目的では、JSP ファイルの名前は **makecall.jsp** が追加されたおよび、 **TwilioCloud** プロジェクトです。 (に割り当てられたプレース ホルダー値の代わりには、Twilio アカウントと認証トークンを使用して **accountSID** と **authToken** 次のコードにします)。

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
    import="java.util.*"
    import="com.twilio.*"
    import="com.twilio.sdk.*"
    import="com.twilio.sdk.resource.factory.*"
    import="com.twilio.sdk.resource.instance.*"
    pageEncoding="ISO-8859-1" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Call processing happens here</title>
    </head>
    <body>
        <b>This is my make call page.</b><p/>
     <%
    try 
    {
         // Use your account SID and authentication token instead
         // of the placeholders shown here.
         String accountSID = "your_twilio_account";
         String authToken = "your_twilio_authentication_token";
     
         // Instantiate an instance of the Twilio client.     
         TwilioRestClient client;
         client = new TwilioRestClient(accountSID, authToken);

         // Retrieve the account, used later to retrieve the CallFactory.
         Account account = client.getAccount();

         // Display the client endpoint. 
         out.println("<p>Using Twilio endpoint " + client.getEndpoint() + ".</p>");
     
         // Display the API version.
         String APIVERSION = TwilioRestClient.DEFAULT_VERSION;
         out.println("<p>Twilio client API version is " + APIVERSION + ".</p>");
    
         // Retrieve the values entered by the user.
         String callTo = request.getParameter("callTo");  
         // The Outgoing Caller ID, used for the From parameter,
         // must have previously been verified with Twilio.
         String callFrom = request.getParameter("callFrom");
         String userText = request.getParameter("callText");
     
         // Replace spaces in the user's text with '%20', 
         // to make the text suitable for a URL.
         userText = userText.replace(" ", "%20");
     
         // Create a URL using the Twilio message and the user-entered text.
         String Url="http://twimlets.com/message";
         Url = Url + "?Message%5B0%5D=" + userText;
     
         // Display the message URL.
         out.println("<p>");
         out.println("The URL is " + Url);
         out.println("</p>");
    
         // Place the call From, To and URL values into a hash map. 
         HashMap<String, String> params = new HashMap<String, String>();
         params.put("From", callFrom);
         params.put("To", callTo);
         params.put("Url", Url);
     
         CallFactory callFactory = account.getCallFactory();
         Call call = callFactory.create(params);
         out.println("<p>Call status: " + call.getStatus()  + "</p>"); 
    } 
    catch (TwilioRestException e) 
    {
        out.println("<p>TwilioRestException encountered: " + e.getMessage() + "</p>");
        out.println("<p>StackTrace: " + e.getStackTrace().toString() + "</p>");
    }
    catch (Exception e) 
    {
        out.println("<p>Exception encountered: " + e.getMessage() + "");
        out.println("<p>StackTrace: " + e.getStackTrace().toString() + "</p>");
    }
    %>
    </body>
    </html>

通話を行うだけでなく、makecall.jsp は Twilio エンドポイント、API バージョン、通話状態を表示します。 たとえば、次のスクリーン ショットのようになります。

![Twilio および Java を使用した Azure 通話応答][twilio_java_response]

## アプリケーションの実行
次に、アプリケーションを実行する手順の概要詳細に、次の手順が掲載されて [を作成する Hello World アプリケーションを使用して、Azure Toolkit for Eclipse][azure_java_eclipse_hello_world]します。

1. TwilioCloud WAR を Azure にエクスポート **approot** フォルダーです。 
2. 変更 **startup.cmd** TwilioCloud WAR を解凍するようにします。
3. アプリケーションをコンピューティング エミュレーター用にコンパイルします。
4. コンピューティング エミュレーターでデプロイを開始します。
5. ブラウザーを開き、実行 **http://localhost:8080/TwilioCloud/callform.jsp**します。
6. 値の形式で入力] をクリックして **電話をかける**, 、makecall.jsp で結果を確認します。

Azure、クラウドへのデプロイ用に再コンパイルにデプロイする準備ができたら、Azure にデプロイし、http://を実行します。*your_hosted_name*、ブラウザーで.cloudapp.net/TwilioCloud/callform.jsp (の値に置き換えてください *your_hosted_name*)。

## 次のステップ
Azure 上の Java で Twilio を使用した基本機能を示すために、このコードが用意されました。 運用環境で Azure にデプロイする前に、エラー処理やその他の機能をさらに追加することができます。 次に例を示します。

* Web フォームを使用する代わりに、Azure ストレージ BLOB または SQL Database を使用して、電話番号と通話テキストを保存できます。 Java で Azure ストレージ blob の使用については、次を参照してください。 [Java から Blob ストレージ サービスを使用する方法][howto_blob_storage_java]します。 Java で SQL データベースの使用については、次を参照してください。 [Java で SQL データベースを使用して][howto_sql_azure_java]します。
* 使って **RoleEnvironment.getConfigurationSettings** Twilio を取得するアカウント ID と認証トークンは、デプロイの構成設定から値を makecall.jsp 内にハードコーディングする代わりにします。 については、 **RoleEnvironment** を参照してください [JSP で Azure サービス ランタイム ライブラリを使用して][azure_runtime_jsp] と、Azure サービス ランタイム パッケージのドキュメント [http://dl.windowsazure.com/javadoc][azure_javadoc]します。
* Makecall.jsp コードは Twilio から提供される URL を割り当てます [http://twimlets.com/message][twimlet_message_url], を **Url** 変数です。 この URL で、通話の次の動作を Twilio に指示する TwiML (Twilio マークアップ言語) 応答が返されるようにします。 たとえば、返される twiml 応答を含む、 **& lt;Say & gt;** ナレーション中に、通話受信者に対してテキスト動詞です。 Twilio から提供される URL を使用する代わりに Twilio の要求に応答するサービスを作成することができます。詳細については、次を参照してください。 [Twilio 音声および SMS 機能で Java を使用する方法][howto_twilio_voice_sms_java]します。 TwiML の詳細については掲載されて [http://www.twilio.com/docs/api/twiml][twiml], 、および詳細情報 **& lt;Say & gt;** その他の Twilio 動詞は掲載されて [http://www.twilio.com/docs/api/twiml/say][twilio_say]します。
* Twilio に関するセキュリティ ガイドライン [https://www.twilio.com/docs/security][twilio_docs_security]します。

Twilio の詳細については、次を参照してください。 [https://www.twilio.com/docs][twilio_docs]します。

## 関連項目
* [Java で音声および SMS 機能に Twilio を使用する方法][howto_twilio_voice_sms_java]
* [証明書を Java CA 証明書ストアに追加する方法][add_ca_cert]

[twilio_pricing]: http://www.twilio.com/pricing
[try_twilio]: http://www.twilio.com/try-twilio
[twilio_api]: http://www.twilio.com/api
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#
[twilio_java_github]: http://github.com/twilio/twilio-java
[twimlet_message_url]: http://twimlets.com/message
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api_service]: http://api.twilio.com
[add_ca_cert]: java-add-certificate-ca-store.md
[azure_java_eclipse_hello_world]: http://msdn.microsoft.com/library/windowsazure/hh690944.aspx
[howto_twilio_voice_sms_java]: partner-twilio-java-how-to-use-voice-sms.md
[howto_blob_storage_java]: http://www.windowsazure.com/develop/java/how-to-guides/blob-storage/
[howto_sql_azure_java]: http://msdn.microsoft.com/library/windowsazure/hh749029.aspx
[azure_runtime_jsp]: http://msdn.microsoft.com/library/windowsazure/hh690948.aspx
[azure_javadoc]: http://dl.windowsazure.com/javadoc
[twilio_docs_security]: http://www.twilio.com/docs/security
[twilio_docs]: http://www.twilio.com/docs
[twilio_say]: http://www.twilio.com/docs/api/twiml/say
[twilio_java]: ./media/partner-twilio-java-phone-call-example/WA_TwilioJavaCallForm.jpg
[twilio_java_response]: ./media/partner-twilio-java-phone-call-example/WA_TwilioJavaMakeCall.jpg

