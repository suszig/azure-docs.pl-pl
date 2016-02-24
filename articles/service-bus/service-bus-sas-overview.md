<properties
   pageTitle="Shared Access Signature の概要 | Microsoft Azure"
   description="Shared Access Signature とは何か、その動作方法、およびノード、PHP、および C# での使用方法。"
   services="service-bus,event-hubs"
   documentationCenter="na"
   authors="djrosanova"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-bus"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="12/09/2015"
   ms.author="darosa"/>

# Shared Access Signature

*共有アクセス署名* (SAS) は、Event Hubs を含む、Service Bus の仲介型メッセージング (キューおよびトピック)、およびリレー型メッセージングの主要なセキュリティ メカニズムです。 この記事では、Shared Access Signature、そのしくみ、およびプラットフォームに依存しない方法で共有アクセス署名を使用する方法について説明します。

## SAS の概要

Shared Access Signature は、SHA-256 セキュア ハッシュまたは URI に基づいた認証メカニズムです。 SAS は、すべての Service Bus サービスによって使用される非常に強力なメカニズムです。 実際の使用で SAS が 2 つのコンポーネント: *共有アクセス ポリシー* と *Shared Access Signature* (と呼ばれ、 *トークン*)。

Service bus での共有アクセス署名に関する詳細な情報を検索する [Service Bus での共有アクセス署名認証](service-bus-shared-access-signature-authentication.md)します。

## 共有アクセス ポリシー

SAS を理解するうえで重要な点の 1 つは、SAS の基盤はポリシーであるということです。 3 つの情報を決定するすべてのポリシーについて: **名前**, 、**スコープ**, 、および **権限**します。  **名前** というだけです。 そのスコープ内で一意の名前。 スコープは簡単です。対象となるリソースの URI です。 Service Bus 名前空間のスコープは、完全修飾ドメイン名 (FQDN) など、 **`https://<yournamespace>.servicebus.windows.net/`**します。

ポリシーに使用可能な権限は、いずれも名前から意味がわかりやすくなっています。

  + 送信
  + リッスン
  + 管理

割り当てられているポリシーを作成した後、 *主キー* と *セカンダリ キー*します。 これらは、暗号化された強力なキーです。 常で使用できるします - 失われることまたは外部に漏らしたりしないで、 [Azure クラシック ポータル][]します。 生成されたキーのいずれかを使用できます。また、いつでも再生成できます。 ただし、再生成するか、またはポリシーのプライマリ キーを変更する場合は、ポリシーから作成された Shared Access Signature がすべて無効になります。

Service Bus 名前空間を作成するときに、ポリシーが自動的に作成という名前空間全体 **RootManageSharedAccessKey**, 、このポリシーは、すべてのアクセス許可を持つ。 としてログオンしない **ルート**, 、本当に正当な理由がない限り、このポリシーを使用しないようにします。 その他のポリシーを作成する、 **構成** ポータルで名前空間のタブをクリックします。 重要なのは、Service Bus の 1 つのツリーのレベル (名前空間、キュー、イベント ハブなど) には、最大 12 個のポリシーだけを保持できることです。

## Shared Access Signature (トークン)

ポリシー自体は、Service Bus のアクセス トークンではありません。 これは、アクセス トークンの生成元のオブジェクトで、プライマリ キーまたはセカンダリ キーを使用します。 次の形式の文字列を注意深く作成することで、トークンが生成されます。

```
SharedAccessSignature sig=<signature-string>&se=<expiry>&skn=<keyName>&sr=<URL-encoded-resourceURI>
```

ここで `signature-string` 、トークンのスコープの sha-256 ハッシュです (**スコープ** 前のセクションで説明した)、CRLF を追加し、有効期限の日時を (新しいエポック以降の秒単位で: `00:00:00 UTC` 1970 年 1 月)。

ハッシュは、次の擬似コードに似ていて、32 バイトを返します。

```
SHA-256('https://<yournamespace>.servicebus.windows.net/'+'\n'+ 1438205742)
```

非ハッシュ値は、 **SharedAccessSignature** 文字列を受信者は、同じ結果が返されることを保証する、同じパラメーターを使用してハッシュを計算できます。 URI はスコープを指定し、キー名はハッシュを計算するために使用するポリシーを識別します。 これは、セキュリティの観点からは重要です。 署名が、受信者 (Service Bus) が計算した署名と一致しない場合は、アクセスは拒否されます。 この時点で、送信者にはキーへのアクセス権があり、この送信者にポリシーで指定した権限を付与する必要があることが確認できます。

## ポリシーからの署名の生成

実際に、どのようにコードでこの操作を実現するのでしょうか。 これらのサンプルを見てみましょう。

### NodeJS

```
function createSharedAccessToken(uri, saName, saKey) { 
    if (!uri || !saName || !saKey) { 
            throw "Missing required parameter"; 
        } 
    var encoded = encodeURIComponent(uri); 
    var now = new Date(); 
    var week = 60*60*24*7;
    var ttl = Math.round(now.getTime() / 1000) + week;
    var signature = encoded + '\n' + ttl; 
    var signatureUTF8 = utf8.encode(signature); 
    var hash = crypto.createHmac('sha256', saKey).update(signatureUTF8).digest('base64'); 
    return 'SharedAccessSignature sr=' + encoded + '&sig=' +  
        encodeURIComponent(hash) + '&se=' + ttl + '&skn=' + saName; 
}
``` 

### Java

```
private static String GetSASToken(String resourceUri, String keyName, String key)
  {
      long epoch = System.currentTimeMillis()/1000L;
      int week = 60*60*24*7;
      String expiry = Long.toString(epoch + week);

      String sasToken = null;
      try {
          String stringToSign = URLEncoder.encode(resourceUri, "UTF-8") + "\n" + expiry;
          String signature = getHMAC256(key, stringToSign);
          sasToken = "SharedAccessSignature sr=" + URLEncoder.encode(resourceUri, "UTF-8") +"&sig=" +
                  URLEncoder.encode(signature, "UTF-8") + "&se=" + expiry + "&skn=" + keyName;
      } catch (UnsupportedEncodingException e) {

          e.printStackTrace();
      }

      return sasToken;
  }


public static String getHMAC256(String key, String input) {
    Mac sha256_HMAC = null;
    String hash = null;
    try {
        sha256_HMAC = Mac.getInstance("HmacSHA256");
        SecretKeySpec secret_key = new SecretKeySpec(key.getBytes(), "HmacSHA256");
        sha256_HMAC.init(secret_key);
        Encoder encoder = Base64.getEncoder();

        hash = new String(encoder.encode(sha256_HMAC.doFinal(input.getBytes("UTF-8"))));

    } catch (InvalidKeyException e) {
        e.printStackTrace();
    } catch (NoSuchAlgorithmException e) {
        e.printStackTrace();
   } catch (IllegalStateException e) {
        e.printStackTrace();
    } catch (UnsupportedEncodingException e) {
        e.printStackTrace();
    }

    return hash;
}
```

### PHP

```
function generateSasToken($uri, $sasKeyName, $sasKeyValue) 
{ 
$targetUri = strtolower(rawurlencode(strtolower($uri))); 
$expires = time();  
$expiresInMins = 60; 
$week = 60*60*24*7;
$expires = $expires + $week; 
$toSign = $targetUri . "\n" . $expires; 
$signature = rawurlencode(base64_encode(hash_hmac('sha256',             
 $toSign, $sasKeyValue, TRUE))); 

$token = "SharedAccessSignature sr=" . $targetUri . "&sig=" . $signature . "&se=" . $expires .      "&skn=" . $sasKeyName; 
return $token; 
}
```
 
### C (&) #35；

```
private static string createToken(string resourceUri, string keyName, string key)
{
    TimeSpan sinceEpoch = DateTime.UtcNow - new DateTime(1970, 1, 1);
    var week = 60 * 60 * 24 * 7;
    var expiry = Convert.ToString((int)sinceEpoch.TotalSeconds + week);
    string stringToSign = HttpUtility.UrlEncode(resourceUri) + "\n" + expiry;
    HMACSHA256 hmac = new HMACSHA256(Encoding.UTF8.GetBytes(key));
    var signature = Convert.ToBase64String(hmac.ComputeHash(Encoding.UTF8.GetBytes(stringToSign)));
    var sasToken = String.Format(CultureInfo.InvariantCulture, "SharedAccessSignature sr={0}&sig={1}&se={2}&skn={3}", HttpUtility.UrlEncode(resourceUri), HttpUtility.UrlEncode(signature), expiry, keyName);
    return sasToken;
}
```

## Shared Access Signature の使用 (HTTP レベル)
 
Service Bus ですべてのエンティティの Shared Access Signature を作成する方法を理解したので、HTTP POST を実行する準備ができました。

```
POST https://<yournamespace>.servicebus.windows.net/<yourentity>/messages
Content-Type: application/json
Authorization: SharedAccessSignature sr=https%3A%2F%2F<yournamespace>.servicebus.windows.net%2F<yourentity>&sig=<yoursignature from code above>&se=1438205742&skn=KeyName
ContentType: application/atom+xml;type=entry;charset=utf-8
``` 
    
この方法は、すべての機能に対して利用できます。 キュー、トピック、サブスクリプション、イベント ハブまたはリレーに対して SAS を作成できます。 Event Hubs の発行元ごとの ID を使用する場合は、`/publishers/< publisherid>` を追加するだけです。

送信者またはクライアントに SAS トークンを付与する場合は、送信者またはクライアントはキーを直接保持しないため、キーを取得するハッシュを反転できません。 そのため、管理者は、送信者またはクライアントがアクセスできる対象とアクセスする期間を制御できます。 重要なのは、ポリシーのプライマリ キーを変更する場合は、ポリシーから作成された Shared Access Signature がすべて無効になるということです。

## Shared Access Signature の使用 (AMQP レベル)

前のセクションでは、データを Service Bus に送信するために、HTTP POST 要求を使用して SAS トークンを使用する方法について説明しました。 ご存じのように、Service Bus へのアクセスには、AMQP (Advanced Message Queue Protocol) プロトコルを使用できます。AMQP は、多くのシナリオでパフォーマンスの理由から主に使用され、好まれているプロトコルです。 AMQP で SAS トークンの使用率が次のドキュメントで説明されている [AMQP Claim-Based Security Version 1.0](https://www.oasis-open.org/committees/download.php/50506/amqp-cbs-v1%200-wd02%202013-08-12.doc) 2013年以降のワーキング ドラフト内にあるがもサポートされている Azure 今日です。

サービス バスにデータを送信する前に、パブリッシャーは AMQP メッセージ内で SAS トークンを明確に定義されたという AMQP ノードに送信する必要がある **"$cbs"** (表示を取得およびすべての SAS トークンを検証する、サービスで使用される「特別な」キューと同様に)。 パブリッシャーを指定する必要がある、 **"ReplyTo"** AMQP メッセージ内のフィールド。 これは、ノードは、サービスの応答 (パブリッシャーとサービス間で簡単な要求/応答パターン) トークンの検証の結果を使用してパブリッシャーにします。 この応答ノードは、AMQP 1.0 仕様に記載されているように、"リモート ノードの動的作成" について話すことで "その場で" 作成されます。 発行元は SAS トークンが有効であることを確認した後に、次の処理に進み、サービスに対してデータを送信できるようになります。

次の手順では、AMQP プロトコルを使用すると、SAS トークンを送信する方法を説明します。、 [AMQP.Net Lite](http://amqpnetlite.codeplex.com) ライブラリの公式の Service Bus SDK を使用できない場合に便利です (winrt、.Net Compact Framework、.Net Micro Framework、mono など) (&) #35; での開発。 当然ながら、HTTP レベル ("Authorization" ヘッダー内で送信される HTTP POST 要求と SAS トークン) の場合と同様に、要求ベースのセキュリティが AMQP レベルでどのように機能するかを理解するためにもこのライブラリは役立ちます。 ただし、心配は無用です。 AMQP に関するこのような豊富な知識を必要としない場合は、.Net を使用して公式の Service Bus SDK を使用することができますがそれを実行する Framework アプリケーションまたは [Azure SB Lite](http://azuresblite.codeplex.com) ライブラリはすべて、他のプラットフォーム (上記参照)。

### C (&) #35；

```
/// <summary>
/// Send Claim Based Security (CBS) token
/// </summary>
/// <param name="shareAccessSignature">Shared access signature (token) to send</param>
private bool PutCbsToken(Connection connection, string sasToken)
{
    bool result = true;
    Session session = new Session(connection);

    string cbsClientAddress = "cbs-client-reply-to";
    var cbsSender = new SenderLink(session, "cbs-sender", "$cbs");
    var cbsReceiver = new ReceiverLink(session, cbsClientAddress, "$cbs");

    // construct the put-token message
    var request = new Message(sasToken);
    request.Properties = new Properties();
    request.Properties.MessageId = Guid.NewGuid().ToString();
    request.Properties.ReplyTo = cbsClientAddress;
    request.ApplicationProperties = new ApplicationProperties();
    request.ApplicationProperties["operation"] = "put-token";
    request.ApplicationProperties["type"] = "servicebus.windows.net:sastoken";
    request.ApplicationProperties["name"] = Fx.Format("amqp://{0}/{1}", sbNamespace, entity);
    cbsSender.Send(request);

    // receive the response
    var response = cbsReceiver.Receive();
    if (response == null || response.Properties == null || response.ApplicationProperties == null)
    {
        result = false;
    }
    else
    {
        int statusCode = (int)response.ApplicationProperties["status-code"];
        if (statusCode != (int)HttpStatusCode.Accepted && statusCode != (int)HttpStatusCode.OK)
        {
            result = false;
        }
    }

    // the sender/receiver may be kept open for refreshing tokens
    cbsSender.Close();
    cbsReceiver.Close();
    session.Close();

    return result;
}
```

上記 *PutCbsToken()* メソッドは受信、 *接続* (AMQP Connection クラス インスタンス AMQP .Net Lite ライブラリによって提供される) サービスへの TCP 接続を表す、 *sasToken* 、sas パラメーターのトークンを送信します。 
注: がで接続を作成することが重要 **外部に設定された SASL 認証メカニズム** (およびしない既定の PLAIN でユーザー名とパスワードの SAS トークンを送信する必要がある場合に使用)。

次に、発行元は、SAS トークンの送信とサービスからの応答 (トークンの検証結果) の受信に使用される 2 つの AMQP リンクを作成します。

AMQP メッセージは、プロパティが多数あり、簡単なメッセージよりも情報が多いので、やや複雑です SAS トークンは、(コンストラクターを使用して) メッセージの本文として設定されます  **"ReplyTo"** ノード名 (できる名前を変更するようにして、サービスによって動的に作成されます)、受信側リンクで検証結果を受信するためにプロパティを設定します。 最後の 3 つの application/custom プロパティは、実行する必要がある操作の種類をサービスが認識するために使用されます。 必要がある CBS ドラフト仕様に従って、 **操作名** (したがって「put-トークン」)、 **トークンの種類** 配置する (ように"servicebus.windows.net:sastoken") し、最後に、 **対象ユーザーの「名前」** 、トークンが (全体のエンティティ) を適用します。

発行元は、送信側リンクで SAS トークンを送信した後に、受信側リンクの応答を読み取る必要があります。 応答はという名前のアプリケーション プロパティの単純な AMQP メッセージ **「状態コード」** HTTP ステータス コードと同じ値を持つことができます。 

## 次のステップ

参照してください、 [Service Bus REST API リファレンス](https://msdn.microsoft.com/library/azure/hh780717.aspx) 詳細については、これらの SAS トークンを行うことができます。

Service Bus の認証の詳細については、次を参照してください。 [Service Bus 認証と承認](service-bus-authentication-and-authorization.md)します。 

C# と Java スクリプトでの SAS の例については、 [このブログの投稿](http://developers.de/blogs/damir_dobric/archive/2013/10/17/how-to-create-shared-access-signature-for-service-bus.aspx)します。

[Azure classic portal]: http://manage.windowsazure.com
