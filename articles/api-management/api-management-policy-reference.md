<properties 
    pageTitle="Azure API Management ポリシー リファレンス" 
    description="API Management の構成に使用できるポリシーについて説明します。" 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="12/16/2015" 
    ms.author="sdanie"/>

# Azure API Management ポリシー リファレンス

このセクションでは、ポリシーのインデックスを提供、 [API Management ポリシー リファレンス][]します。 詳細を追加して、ポリシーの構成については、次を参照してください。 [API Management のポリシー][]します。

ポリシーの式は、ポリシーで特に指定されていない限り、任意の API Management ポリシーで属性値またはテキスト値として使用できます。 などのいくつかのポリシー、 [制御フロー][] と [変数の設定][] ポリシーはポリシー式に基づいています。 詳細については、次を参照してください [ポリシーを高度な][] と [ポリシー式。][]

## ポリシー リファレンスのインデックス

-   [アクセス制限ポリシー][]
    -   [HTTP ヘッダーを確認][] の存在または HTTP ヘッダーの値を適用します。
    -   [サブスクリプションによって呼び出しレートを制限][] -サブスクリプションごとに、呼び出しレートを制限することによって、API の使用量が急増します。
    -   [キーによって呼び出しレートを制限](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) -あたりキーごとに、呼び出しレートを制限することによって、API の使用量が急増します。
    -   [呼び出し元 Ip を制限する][] -特定の IP アドレスまたはアドレス範囲からの呼び出しをフィルター処理 (許可/拒否) します。
    -   [サブスクリプションによって使用量クォータを設定する][] のクォータを適用、更新可能な有効期間の呼び出しボリューム、または帯域幅、サブスクリプションごとにすることができます。
    -   [キーに使用状況クォータを設定する](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) のクォータを適用、更新可能な有効期間の呼び出しボリューム、または帯域幅、あたりキーごとにすることができます。
    -   [JWT を検証する][] の存在と指定した HTTP ヘッダーまたは指定されたクエリ パラメーターから抽出した JWT の有効性を適用します。
-   [高度なポリシー][]
    -   [制御フロー][] - 条件付きでブール値の評価の結果に基づいてポリシー ステートメントが適用される [式][]します。
    -   [要求の転送][] のバックエンド サービスに要求を転送します。
    -   [Event Hub にログ][] -によって定義されたメッセージのターゲットに指定された形式でメッセージを送信、 [Logger](https://msdn.microsoft.com/library/azure/mt592020.aspx#Logger) エンティティです。
    -   [応答を返す](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) のパイプラインの実行を中止し、呼び出し元に直接指定された応答を返します。
    -   [一方向の要求を送信](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest) -指定された URL への応答を待つことがなく、要求を送信します。
    -   [要求を送信](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) -指定された URL に要求を送信します。
    -   [要求メソッドを設定](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetRequestMethod) -要求の HTTP メソッドを変更することができます。
    -   [状態を設定](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetStatus) -指定した値に HTTP ステータス コードを変更します。
    -   [変数の設定][] -名前付きの値を保持 [コンテキスト][] 後から使用できる変数。
-   [認証ポリシー][]
    -   [基本認証][] -基本認証を使用してバックエンド サービスで認証します。
    -   [クライアント証明書による認証][] -クライアント証明書を使用してバックエンド サービスで認証します。
-   [キャッシュ ポリシー][] 
    -   [キャッシュから取得][] -キャッシュを検索し、使用可能な場合は有効なキャッシュされた応答を返します。
    -   [キャッシュ ストア][] -指定したキャッシュ制御構成に従って応答をキャッシュします。
    -   [キャッシュから値を取得](https://msdn.microsoft.com/library/azure/dn894086.aspx#GetFromCacheByKey) -キーによってキャッシュされた項目を取得します。
    -   [値をキャッシュに格納](https://msdn.microsoft.com/library/azure/dn894086.aspx#StoreToCacheByKey) -キーによって、キャッシュに項目を格納します。
-   [クロス ドメイン ポリシー][] 
    -   [クロス ドメイン呼び出しを許可する][] -Adobe Flash や Microsoft Silverlight ブラウザー ベースのクライアントからアクセス可能な API を作成します。
    -   [CORS][] -クロス オリジン リソース共有 (CORS) サポートを操作またはブラウザー ベースのクライアントからのクロス ドメイン呼び出しを許可するように API を追加します。
    -   [JSONP][] -JSON with padding (JSONP) のサポートを操作または API JavaScript ブラウザー ベースのクライアントからのクロス ドメイン呼び出しを許可するように追加します。
-   [変換ポリシー][] 
    -   [JSON を XML に変換][] - 変換の要求本文または応答本文を JSON からです。
    -   [XML を JSON に変換][] - 変換の要求本文または応答本文の XML から JSON にします。
    -   [検索し、本文内の文字列を置換][] - 要求または応答の部分文字列を検索し、別の部分に置き換えられます。
    -   [コンテンツの URLs のマスク][] -(マスクして) リンクを書き換え、応答本文および場所ヘッダー、ゲートウェイ経由で同等のリンクを指し示すようにします。
    -   [バックエンド サービスの設定][] -受信要求のバックエンド サービスに変更します。
    -   [本文を設定][] -着信要求と発信要求のメッセージ本文を設定します。
    -   [HTTP ヘッダーを設定する][] - 既存の応答または要求ヘッダーに値を代入するか、新しい応答または要求ヘッダーを追加します。
    -   [クエリ文字列パラメーターの設定][] - を追加、値の置換、または要求クエリ文字列パラメーターを削除します。
    -   [URL の書き換え][] -要求 URL をパブリックな形式から web サービスで求められる形式に変換します。

## 次のステップ

ポリシー式に関する詳細については、次のビデオをご覧ください。

> [AZURE.VIDEO policy-expressions-in-azure-api-management]

[Access restriction policies]: https://msdn.microsoft.com/library/azure/dn894078.aspx
[Check HTTP header]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#CheckHTTPHeader
[Limit call rate by subscription]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#LimitCallRate
[Restrict caller IPs]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#RestrictCallerIPs
[Set usage quota by subscription]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#SetUsageQuota
[Validate JWT]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#ValidateJWT

[Advanced policies]: https://msdn.microsoft.com/library/azure/dn894085.aspx
[Control flow]: https://msdn.microsoft.com/library/azure/dn894085.aspx#choose
[Set variable]: https://msdn.microsoft.com/library/azure/dn894085.aspx#set_variable
[expressions]: https://msdn.microsoft.com/library/azure/dn910913.aspx
[context]: https://msdn.microsoft.com/library/azure/ea160028-fc04-4782-aa26-4b8329df3448#ContextVariables
[Forward request]: https://msdn.microsoft.com/library/azure/dn894085.aspx#ForwardRequest
[Log to Event Hub]: https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub

[Authentication policies]: https://msdn.microsoft.com/library/azure/dn894079.aspx
[Authenticate with Basic]: https://msdn.microsoft.com/library/azure/061702a7-3a78-472b-a54a-f3b1e332490d#Basic
[Authenticate with client certificate]: https://msdn.microsoft.com/library/azure/061702a7-3a78-472b-a54a-f3b1e332490d#ClientCertificate
[Caching policies]: https://msdn.microsoft.com/library/azure/dn894086.aspx
[Get from cache]: https://msdn.microsoft.com/library/azure/8147199c-24d8-439f-b2a9-da28a70a890c#GetFromCache
[Store to cache]: https://msdn.microsoft.com/library/azure/8147199c-24d8-439f-b2a9-da28a70a890c#StoreToCache

[Cross domain policies]: https://msdn.microsoft.com/library/azure/dn894084.aspx
[Allow cross-domain calls]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#AllowCrossDomainCalls
[CORS]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#CORS
[JSONP]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#JSONP

[Transformation policies]: https://msdn.microsoft.com/library/azure/dn894083.aspx
[Convert JSON to XML]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#ConvertJSONtoXML
[Convert XML to JSON]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#ConvertXMLtoJSON
[Find and replace string in body]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#Findandreplacestringinbody
[Mask URLs in content]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#MaskURLSContent
[Set backend service]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#SetBackendService
[Set body]: https://msdn.microsoft.com/library/azure/dn894083.aspx#SetBody
[Set HTTP header]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#SetHTTPheader
[Set query string parameter]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#SetQueryStringParameter
[Rewrite URL]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#RewriteURL



[Policies in API Management]: api-management-howto-policies.md
[API Management policy reference]: https://msdn.microsoft.com/library/azure/dn894081.aspx

[Policy expressions]: https://msdn.microsoft.com/library/azure/dn910913.aspx

 
