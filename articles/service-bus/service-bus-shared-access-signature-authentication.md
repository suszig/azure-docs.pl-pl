<properties 
   pageTitle="Service Bus による Shared Access Signature 認証 | Microsoft Azure"
   description="Service Bus による SAS 認証について詳しく説明します。"
   services="service-bus"
   documentationCenter="na"
   authors="sethmanheim"
   manager="timlt"
   editor="" />
<tags 
   ms.service="service-bus"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="12/09/2015"
   ms.author="sethm" />

# Service Bus による Shared Access Signature 認証

[共有アクセス署名 (SAS)](service-bus-sas-overview.md) 認証により、アプリケーションはどの特定の権限が関連付けられていると、名前空間またはメッセージング エンティティ (キューまたはトピック) で構成されたアクセス キーを使用して Service Bus への認証にします。 次に、このキーを使用して、クライアントが後で Service Bus に対する認証に使用できる SAS トークンを生成できます。

Service Bus の SAS 認証サポートは、Azure SDK バージョン 2.0 以降に含まれています。 Service Bus の認証の詳細については、次を参照してください。 [Service Bus 認証と承認](service-bus-authentication-and-authorization.md)します。

## 概念

Service Bus の SAS 認証には、Service Bus リソースに対する関連した権限を使用した暗号化キーの構成が伴います。 クライアントは SAS トークンを渡して Service Bus のリソースへのアクセスを要求します。 このトークンは、アクセスされるリソース URI と、構成されたキーで署名された有効期限で構成されます。

共有アクセス署名認証規則を構成するにはサービス バスの [中継](service-bus-fundamentals-hybrid-solutions.md/#relays), 、[キュー](service-bus-fundamentals-hybrid-solutions.md/#queues), 、[トピック](service-bus-fundamentals-hybrid-solutions.md/#topics), 、および [Event Hubs](https://azure.microsoft.com/documentation/services/event-hubs/)します。

SAS 認証では、次の要素が使用されます。

- [共有アクセス認証ルール](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sharedaccessauthorizationrule.aspx): A 256 ビット base 64 形式でプライマリ暗号化キー、オプションのセカンダリ キーとキー名および関連付けられている権限 (一連の *リッスン*, 、*送信*, 、または *管理* 権限)。

- [Shared Access Signature](https://msdn.microsoft.com/library/azure/microsoft.servicebus.sharedaccesssignaturetokenprovider.sharedaccesssignature.aspx) トークン: hmac-sha256 リソース文字列の暗号化キーを使用してアクセスされるリソースの URI と expiry でから成るを使用して生成します。 署名と、次のセクションで説明されているその他の要素は、フォームに文字列にフォーマットされて、 [SharedAccessSignature](https://msdn.microsoft.com/library/azure/microsoft.servicebus.sharedaccesssignaturetokenprovider.sharedaccesssignature.aspx) トークンです。

## Shared Access Signature 認証の構成

構成することができます、 [SharedAccessAuthorizationRule](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sharedaccessauthorizationrule.aspx) Service Bus 名前空間、キューまたはトピックのルール。 構成、 [SharedAccessAuthorizationRule](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sharedaccessauthorizationrule.aspx) サービス バスのサブスクリプションは現在はサポートされませんが、サブスクリプションへのアクセスをセキュリティで保護する名前空間またはトピックに構成されているルールを使用することができます。 この手順を示しています。 実際のサンプルを参照してください、 [Service Bus サブスクリプションで使用する共有アクセス署名 (SAS) 認証](http://code.msdn.microsoft.com/windowsazure/Using-Shared-Access-e605b37c) サンプルです。

Service Bus の名前空間、キュー、トピックでは、このような規則を最大 12 個構成できます。 Service Bus 名前空間に構成されている規則は、その名前空間内のすべてのエンティティに適用されます。

![SAS](./media/service-bus-shared-access-signature-authentication/IC676272.gif)

この図では、 *manageRuleNS*, 、*sendRuleNS*, 、および *listenRuleNS* キュー Q1 とトピック T1 の両方に承認規則を適用中に *listenRuleQ* と *sendRuleQ* キュー Q1 にのみ適用し、 *sendRuleT* トピック T1 のみに適用されます。

キー パラメーター、 [SharedAccessAuthorizationRule](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sharedaccessauthorizationrule.aspx) は次のようになります。

|パラメーター|説明|
|---|---|
|*KeyName*|承認規則を説明する文字列。|
|*PrimaryKey*|SAS トークンの署名と検証用の Base64 でエンコードされた 256 ビットのプライマリ キー。|
|*SecondaryKey*|SAS トークンの署名と検証用の Base64 でエンコードされた 256 ビットのセカンダリ キー。|
|*AccessRights*|承認規則によって付与されたアクセス権限の一覧。 これらの権限には、Listen、Send、Manage 権限の任意のコレクションを指定できます。|

Service Bus 名前空間をプロビジョニングするときは、 [SharedAccessAuthorizationRule](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sharedaccessauthorizationrule.aspx), と [KeyName](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sharedaccessauthorizationrule.keyname.aspx) に設定 **RootManageSharedAccessKey**, 、既定で作成されます。 2 つの既定 [SharedAccessAuthorizationRule](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sharedaccessauthorizationrule.aspx) オブジェクト通知ハブ用に構成されて: Listen 権限のみで、Listen、Send、および管理の権限を持つ 1 つとします。

## 共有アクセス承認規則のキーの再生成と取り消し

使用されるキーを定期的に再生成することをお勧めしますが、 [SharedAccessAuthorizationRule](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sharedaccessauthorizationrule.aspx) オブジェクトです。 一般にアプリケーションを使用する必要があります、 [PrimaryKey](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sharedaccessauthorizationrule.primarykey.aspx) SAS トークンを生成します。 置き換える必要がありますが、キーを再生成する際、 [SecondaryKey](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sharedaccessauthorizationrule.secondarykey.aspx) と古いプライマリ キーし、新しい主キーとして新しいキーを生成します。 これにより、古いプライマリ キーで発行され、まだ期限が切れていないトークンを引き続き承認に使用できます。

キーが侵害されたキーを失効する必要がある場合は、再生成両方とも、 [PrimaryKey](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sharedaccessauthorizationrule.primarykey.aspx) と [SecondaryKey](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sharedaccessauthorizationrule.secondarykey.aspx) の [SharedAccessAuthorizationRule](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sharedaccessauthorizationrule.aspx), 、新しいキーで置き換えします。 この手順により、古いキーで署名されたすべてのトークンが無効になります。

## Shared Access Signature トークンの生成

共有アクセス承認規則で指定された署名キーにアクセスできるクライアントは、SAS トークンを生成できます。 これは次のような形式になります。

```
SharedAccessSignature sig=<signature-string>&se=<expiry>&skn=<keyName>&sr=<URL-encoded-resourceURI>
```

 **署名** 、SAS トークンが計算への署名文字列を hmac-sha256 のハッシュを使用する、 [PrimaryKey](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sharedaccessauthorizationrule.primarykey.aspx) 承認規則のプロパティです。 署名文字列は、リソース URI と有効期限で構成され、次のような形式になります。

```
StringToSign = <resourceURI> + "\n" + expiry;
```

この操作には、エンコード済みのリソース URI を使用する必要があることに注意してください。 リソース URI とは、アクセスが要求される Service Bus リソースの完全な URI です。 たとえば、`http://<namespace>.servicebus.windows.net/<entityPath>` または `sb://<namespace>.servicebus.windows.net/<entityPath>` (つまり `http://contoso.servicebus.windows.net/contosoTopics/T1/Subscriptions/S3`) です。

有効期限は 1970 年 1 月 1 日の 00 時 00 分 00 秒 UTC からのエポック秒で表されます。

署名に使用される共有アクセス承認規則は、この URI、またはその階層の親のいずれかで指定したエンティティに構成する必要があります。 たとえば、前の例では、`http://contoso.servicebus.windows.net/contosoTopics/T1` または `http://contoso.servicebus.windows.net` となります。

SAS トークンは、署名文字列で使用される `<resourceURI>` の下のすべてのリソースで有効です。

 [KeyName](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sharedaccessauthorizationrule.keyname.aspx) 、sas トークンが参照する、 **keyName** 、トークンを生成するために使用する共有アクセス認証ルールのです。

 *URL でエンコードされた resourceURI* 、署名の計算中に署名する文字列で使用する URI と同じである必要があります。  [パーセント エンコード](https://msdn.microsoft.com/library/4fkewx0t.aspx)します。

## Service Bus による Shared Access Signature 認証の使用方法

以下のシナリオには、承認規則の構成、SAS トークンの生成、クライアントの承認などが含まれます。

完全な構成と使用法の SAS 認証を示すサービス バス アプリケーションの実際のサンプルを参照してください [Service Bus での共有アクセス署名認証](http://code.msdn.microsoft.com/Shared-Access-Signature-0a88adf8)します。 SAS 承認規則の名前空間および Service Bus のサブスクリプションをセキュリティで保護するトピックでは構成の使用方法について説明する関連サンプルは、ここで使用可能な: [Service Bus サブスクリプションで使用する共有アクセス署名 (SAS) 認証](http://code.msdn.microsoft.com/Using-Shared-Access-e605b37c)します。

## 名前空間の共有アクセス承認規則へのアクセス

Service Bus 名前空間のルートに対する操作では、証明書の認証が必要です。 Azure サブスクリプションの管理証明書をアップロードする必要があります。 管理証明書をアップロードするにはクリックして **設定** の左側のウィンドウで、 [Azure クラシック ポータル][]します。 Azure 管理証明書の詳細については、次を参照してください。 [Azure の管理証明書を作成する](https://msdn.microsoft.com/library/azure/gg551722.aspx)です。

Service Bus 名前空間の共有アクセス承認規則にアクセスするためのエンドポイントは次のようになります。

```
https://management.core.windows.net/{subscriptionId}/services/ServiceBus/namespaces/{namespace}/AuthorizationRules/
```

作成する、 [SharedAccessAuthorizationRule](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sharedaccessauthorizationrule.aspx) Service Bus 名前空間のオブジェクトは、JSON または XML としてシリアル化された規則情報でこのエンドポイントに対して、POST 操作を実行します。 次に例を示します。

```
// Base address for accessing authorization rules on a namespace
string baseAddress = @"https://management.core.windows.net/<subscriptionId>/services/ServiceBus/namespaces/<namespace>/AuthorizationRules/";

// Configure authorization rule with base64-encoded 256-bit key and Send rights
var sendRule = new SharedAccessAuthorizationRule("contosoSendAll",
    SharedAccessAuthorizationRule.GenerateRandomKey(),
    new[] { AccessRights.Send });

// Operations on the Service Bus namespace root require certificate authentication.
WebRequestHandler handler = new WebRequestHandler
{
    ClientCertificateOptions = ClientCertificateOption.Manual
};
// Access the management certificate by subject name
handler.ClientCertificates.Add(GetCertificate(<certificateSN>));

HttpClient httpClient = new HttpClient(handler)
{
    BaseAddress = new Uri(baseAddress)
};
httpClient.DefaultRequestHeaders.Accept.Add(
    new MediaTypeWithQualityHeaderValue("application/json"));
httpClient.DefaultRequestHeaders.Add("x-ms-version", "2015-01-01");

// Execute a POST operation on the baseAddress above to create an auth rule
var postResult = httpClient.PostAsJsonAsync("", sendRule).Result;
```

同様に、名前空間に構成された承認規則を読み取るには、エンドポイントに対して GET 操作を使用します。

特定の承認規則を更新または削除するには、次のエンドポイントを使用します。

```
https://management.core.windows.net/{subscriptionId}/services/ServiceBus/namespaces/{namespace}/AuthorizationRules/{KeyName}
```

## エンティティの共有アクセス承認規則へのアクセス

アクセスできる、 [Microsoft.ServiceBus.Messaging.SharedAccessAuthorizationRule](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sharedaccessauthorizationrule.aspx) Service Bus キューまたはトピックを通じてに構成されているオブジェクト、 [AuthorizationRules](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.authorizationrules.aspx) に対応するコレクション [QueueDescription](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.aspx), 、[TopicDescription](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicdescription.aspx), 、または [NotificationHubDescription](https://msdn.microsoft.com/library/azure/microsoft.servicebus.notifications.notificationhubdescription.aspx) オブジェクトです。

次のコードでは、キューの承認規則を追加する方法を示します。

```
// Create an instance of NamespaceManager for the operation
NamespaceManager nsm = NamespaceManager.CreateFromConnectionString( 
    <connectionString> );
QueueDescription qd = new QueueDescription( <qPath> );

// Create a rule with send rights with keyName as "contosoQSendKey"
// and add it to the queue description.
qd.Authorization.Add(new SharedAccessAuthorizationRule("contosoSendKey", 
    SharedAccessAuthorizationRule.GenerateRandomKey(), 
    new[] { AccessRights.Send }));

// Create a rule with listen rights with keyName as "contosoQListenKey"
// and add it to the queue description.
qd.Authorization.Add(new SharedAccessAuthorizationRule("contosoQListenKey",
    SharedAccessAuthorizationRule.GenerateRandomKey(),
    new[] { AccessRights.Listen }));

// Create a rule with manage rights with keyName as "contosoQManageKey"
// and add it to the queue description.
// A rule with manage rights must also have send and receive rights.
qd.Authorization.Add(new SharedAccessAuthorizationRule("contosoQManageKey",
    SharedAccessAuthorizationRule.GenerateRandomKey(),
    new[] {AccessRights.Manage, AccessRights.Listen, AccessRights.Send }));

// Create the queue.
nsm.CreateQueue(qd);
```

## Shared Access Signature 承認の使用

Service Bus .NET ライブラリと Azure .NET SDK を使用してアプリケーションがにより SAS 認証を使用して、 [SharedAccessSignatureTokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.sharedaccesssignaturetokenprovider.aspx) クラスです。 次のコードでは、Service Bus キューにメッセージを送信するトークン プロバイダーの使用を示しています。

```
Uri runtimeUri = ServiceBusEnvironment.CreateServiceUri("sb", 
    <yourServiceNamespace>, string.Empty);
MessagingFactory mf = MessagingFactory.Create(runtimeUri, 
    TokenProvider.CreateSharedAccessSignatureTokenProvider(keyName, key));
QueueClient sendClient = mf.CreateQueueClient(qPath);

//Sending hello message to queue.
BrokeredMessage helloMessage = new BrokeredMessage("Hello, Service Bus!");
helloMessage.MessageId = "SAS-Sample-Message";
sendClient.Send(helloMessage);
```

アプリケーションは、接続文字列を受け入れるメソッドで SAS 接続文字列を使用することで、認証に SAS を使用することもできます。

Service Bus リレーで SAS 承認を使用するには、Service Bus 名前空間に構成されている SAS キーを使用できます。 名前空間に対してリレーを明示的に作成する場合 ([NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) で、 [RelayDescription](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.relaydescription.aspx)) オブジェクト、そのリレーについてのみ SAS 規則を設定することができます。 Service Bus サブスクリプションで SAS 承認を使用するには、Service Bus 名前空間またはトピックに構成されている SAS キーを使用できます。

## Service Bus の操作に必要な権限

次の表に、Service Bus のリソースでのさまざまな操作に必要となるアクセス権を示します。

|操作|必要な要求|要求のスコープ|
|---|---|---|
|**名前空間**|||
|名前空間での承認規則を構成する|管理|任意の名前空間アドレス|
|**サービス レジストリ**|||
|プライベート ポリシーを列挙する|管理|任意の名前空間アドレス|
|リレー|||
|名前空間でリッスンを開始する|リッスン|任意の名前空間アドレス|
|名前空間のリスナーにメッセージを送信する|送信|任意の名前空間アドレス|
|**キュー**|||
|キューを作成する|管理|任意の名前空間アドレス|
|キューを削除する|管理|任意の有効なキュー アドレス|
|キューを列挙する|管理|/$Resources/Queues|
|キューの説明を取得する|管理または送信|任意の有効なキュー アドレス|
|キューの承認規則を構成する|管理|任意の有効なキュー アドレス|
|キューに送信する|送信|任意の有効なキュー アドレス|
|キューからメッセージを受信する|リッスン|任意の有効なキュー アドレス|
|ピーク ロック モードでメッセージを受信した後にそのメッセージを破棄または終了する|リッスン|任意の有効なキュー アドレス|
|後で取得するためにメッセージを保留する|リッスン|任意の有効なキュー アドレス|
|メッセージを配信不能にする|リッスン|任意の有効なキュー アドレス|
|メッセージのキュー セッションに関連付けられた状態を取得する|リッスン|任意の有効なキュー アドレス|
|メッセージのキュー セッションに関連付けられた状態を設定する|リッスン|任意の有効なキュー アドレス|
|**トピック**|||
|トピックを作成する|管理|任意の名前空間アドレス|
|トピックを削除する|管理|任意の有効なトピック アドレス|
|トピックを列挙する|管理|/$Resources/Topics|
|トピックの説明を取得する|管理または送信|任意の有効なトピック アドレス|
|トピックの承認規則を構成する|管理|任意の有効なトピック アドレス|
|トピックに送信する|送信|任意の有効なトピック アドレス|
|**[サブスクリプション]**|||
|サブスクリプションを作成する|管理|任意の名前空間アドレス|
|サブスクリプションを削除する|管理|../myTopic/Subscriptions/mySubscription|
|サブスクリプションを列挙する|管理|../myTopic/Subscriptions|
|サブスクリプションの説明を取得する|管理またはリッスン|../myTopic/Subscriptions/mySubscription|
|ピーク ロック モードでメッセージを受信した後にそのメッセージを破棄または終了する|リッスン|../myTopic/Subscriptions/mySubscription|
|後で取得するためにメッセージを保留する|リッスン|../myTopic/Subscriptions/mySubscription|
|メッセージを配信不能にする|リッスン|../myTopic/Subscriptions/mySubscription|
|トピック セッションに関連付けられた状態を取得する|リッスン|../myTopic/Subscriptions/mySubscription|
|トピック セッションに関連付けられた状態を設定する|リッスン|../myTopic/Subscriptions/mySubscription|
|**ルール**|||
|規則を作成する|管理|../myTopic/Subscriptions/mySubscription|
|規則を削除する|管理|../myTopic/Subscriptions/mySubscription|
|規則を列挙する|管理またはリッスン|../myTopic/Subscriptions/mySubscription/Rules|
|**Notification Hubs**|||
|通知ハブを作成する|管理|任意の名前空間アドレス|
|アクティブなデバイスの登録を作成または更新する|リッスンまたは管理|../notificationHub/tags/{tag}/registrations|
|PNS 情報を更新する|リッスンまたは管理|../notificationHub/tags/{tag}/registrations/updatepnshandle|
|通知ハブに送信する|送信|../notificationHub/messages|

## 次のステップ

Service Bus の SAS の大まかな概要については、次を参照してください。 [共有アクセス署名](service-bus-sas-overview.md)します。

参照してください [Service Bus 認証と承認](service-bus-authentication-and-authorization.md) Service Bus 認証背景情報についてです。

[Azure classic portal]: http://manage.windowsazure.com
