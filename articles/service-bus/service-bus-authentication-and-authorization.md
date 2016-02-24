<properties 
   pageTitle="Service Bus の認証と承認 | Microsoft Azure"
   description="Shared Access Signature (SAS) 認証の概要です。"
   services="service-bus"
   documentationCenter="na"
   authors="sethmanheim"
   manager="timlt"
   editor="tysonn" />
<tags 
   ms.service="service-bus"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="12/09/2015"
   ms.author="sethm" />

# Service Bus の認証と承認

アプリケーションは、Shared Access Signature (SAS) 認証または Azure Active Directory Access Control (Access Control Service または ACS とも呼ばれます) を使用して、Azure Service Bus に対して認証できます。 Shared Access Signature 認証により、アプリケーションは、名前空間、または特定の権限が関連付けられているエンティティで構成されたアクセス キーを使用して Service Bus に対して認証できます。 次に、このキーを使用して、クライアントが Service Bus に対する認証に使用できる Shared Access Signature トークンを生成できます。

SAS は、簡単で柔軟性が高く、使いやすい認証スキームを Service Bus に提供しているため、ACS よりも推奨されています。 アプリケーションは、承認された "ユーザー" の概念を管理する必要がないシナリオで SAS を使用できます。

## Shared Access Signature 認証

[SAS 認証](service-bus-sas-overview.md) 特定の権限を持つサービス バス リソースへのアクセスを許可することができます。 Service Bus の SAS 認証には、Service Bus リソースに対する関連した権限を使用した暗号化キーの構成が伴います。 これにより、クライアントは SAS トークンを提示してリソースへのアクセス権を取得できます。このトークンは、アクセスされるリソース URI と、構成されたキーで署名された有効期限から成ります。

SAS のキーは Service Bus 名前空間で構成できます。 このキーは、その名前空間内のすべてのメッセージング エンティティに適用されます。 Service Bus のキューとトピックでキーを構成することもできます。 SAS は、Service Bus Relay でもサポートされています。

SAS を使用することができます、 [SharedAccessAuthorizationRule](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sharedaccessauthorizationrule.aspx) 名前空間、キュー、または以下のトピック上のオブジェクト。

- *KeyName* ルールを識別します。

- *PrimaryKey* は SAS トークンの署名/検証に使用される暗号化キー。

- *SecondaryKey* は SAS トークンの署名/検証に使用される暗号化キー。

- *権限* リッスンのコレクションを表す、送信、または管理権限を付与します。

名前空間レベルで構成された承認規則では、対応するキーを使用して署名されたトークンによって、クライアントの名前空間内のすべてのエンティティへのアクセス権を付与できます。 Service Bus の名前空間、キュー、トピックでは、このような承認規則を最大 12 個構成できます。 既定では、 [SharedAccessAuthorizationRule](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sharedaccessauthorizationrule.aspx) すべての権限を持つが構成されているすべての名前空間は、最初にプロビジョニングされたとき。

エンティティにアクセスするクライアントには、固有の仕様を使用して生成された SAS トークンが必要な [SharedAccessAuthorizationRule](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sharedaccessauthorizationrule.aspx)します。 SAS トークンの生成には、承認規則に関連付けられた暗号化キーによって、アクセスが要求されるリソース URI と、有効期限で構成されるリソース文字列の HMAC-SHA256 を使用します。

Service Bus の SAS 認証サポートは、Azure .NET SDK バージョン 2.0 以降に含まれています。 SAS にはサポートが含まれています、 [SharedAccessAuthorizationRule](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sharedaccessauthorizationrule.aspx)します。 接続文字列をパラメーターとして受け取るすべての API では、SAS 接続文字列がサポートされています。

## ACS 認証

ACS を使用した Service Bus の認証は、付属の "-sb" ACS 名前空間を通じて管理されます。 コンパニオンの ACS 名前空間の Service Bus 名前空間を作成する場合は、Azure の旧ポータルを使用して、Service Bus 名前空間を作成することはできません。使用して、名前空間を作成する必要があります、 [New-azuresbnamespace](https://msdn.microsoft.com/library/azure/dn495165.aspx) PowerShell コマンドレット。 次に例を示します。

```
New-AzureSBNamespace <namespaceName> "<Region>” -CreateACSNamespace $true
```

ACS 名前空間を作成しないようにするには、次のコマンドを発行します。

```
New-AzureSBNamespace <namespaceName> "<Region>” -CreateACSNamespace $false
```

呼ばれる Service Bus 名前空間を作成する場合など **contoso.servicebus.windows.net**, 、コンパニオン ACS 名前空間と呼ばれる **contoso-sb.accesscontrol.windows.net です** 自動的にプロビジョニングされます。 2014 年 8 月以前に作成されたすべての名前空間では、付属の ACS 名前空間も作成されていました。

この付属の ACS 名前空間では、すべての権限を持つ既定のサービス ID "所有者" が既定でプロビジョニングされます。 適切な信頼関係を構成することで、ACS を通じて任意の Service Bus エンティティに対する細かい制御が可能になります。 Service Bus エンティティへのアクセスを管理するために追加のサービス ID を構成できます。

エンティティにアクセスするために、クライアントは資格情報を提示して適切な要求を伴った ACS からの SWT トークンを要求します。 その後、SWT トークンは要求の一部として Service Bus に送信され、エンティティへのアクセスのためにクライアントの承認が有効になります。

Service Bus の ACS 認証サポートは、Azure .NET SDK バージョン 2.0 以降に含まれています。 この認証をサポートしています、 [SharedSecretTokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.sharedsecrettokenprovider.aspx)します。 接続文字列をパラメーターとして受け取るすべての API では、ACS 接続文字列がサポートされています。

## 次のステップ

読み続ける [Service Bus での共有アクセス署名認証](service-bus-shared-access-signature-authentication.md) SAS の詳細についてです。

Service Bus の SAS の大まかな概要については、次を参照してください。 [共有アクセス署名](service-bus-sas-overview.md)します。

ACS トークンの詳細についてを検索する [方法: OAuth WRAP プロトコル経由で ACS からトークンを要求](https://msdn.microsoft.com/library/hh674475.aspx)します。




