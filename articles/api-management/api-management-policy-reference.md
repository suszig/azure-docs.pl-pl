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

このセクションでは、ポリシーのインデックスを提供、 [API Management のポリシーを参照][]します。 詳細を追加して、ポリシーの構成については、次を参照してください。 [[API Management] ポリシー][]します。

ポリシーの式は、ポリシーで特に指定されていない限り、任意の API Management ポリシーで属性値またはテキスト値として使用できます。 などのいくつかのポリシー、 [制御フロー][] と [セット変数][] ポリシーはポリシー式に基づいています。 詳細については、次を参照してください [ポリシー [高度な][] と [ポリシー式 []][]。

## ポリシー リファレンスのインデックス

-   [アクセス制限ポリシーの][]
    -   [チェックの HTTP ヘッダーの][] の存在または HTTP ヘッダーの値を適用します。
    -   [サブスクリプションで呼び出しレートを制限][] -サブスクリプションごとに、呼び出しレートを制限することによって、API の使用量が急増します。
    -   [キーによって呼び出しレートを制限](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) -あたりキーごとに、呼び出しレートを制限することによって、API の使用量が急増します。
    -   [呼び出し元の ip アドレスを制限する][] -特定の IP アドレスまたはアドレス範囲からの呼び出しをフィルター処理 (許可/拒否) します。
    -   [サブスクリプションでの使用量クォータを設定][] のクォータを適用、更新可能な有効期間の呼び出しボリューム、または帯域幅、サブスクリプションごとにすることができます。
    -   [キーによって使用量クォータを設定する](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) のクォータを適用、更新可能な有効期間の呼び出しボリューム、または帯域幅、あたりキーごとにすることができます。
    -   [JWT を検証する][] の存在と指定した HTTP ヘッダーまたは指定されたクエリ パラメーターから抽出した JWT の有効性を適用します。
-   [高度なポリシーの][]
    -   [制御フロー][] - 条件付きでブール値の評価の結果に基づいてポリシー ステートメントが適用される [式][]します。
    -   [の転送要求][] のバックエンド サービスに要求を転送します。
    -   [ログのイベント ハブの][] -によって定義されたメッセージのターゲットに指定された形式でメッセージを送信、 [Logger](https://msdn.microsoft.com/library/azure/mt592020.aspx#Logger) エンティティです。
    -   [応答を返す](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) のパイプラインの実行を中止し、呼び出し元に直接指定された応答を返します。
    -   [一方向の要求を送信](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest) -指定された URL への応答を待つことがなく、要求を送信します。
    -   [送信要求](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) -指定された URL に要求を送信します。
    -   [Set 要求メソッド](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetRequestMethod) -要求の HTTP メソッドを変更することができます。
    -   [状態設定](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetStatus) -指定した値に HTTP ステータス コードを変更します。
    -   [セット変数][] -名前付きの値を保持 [コンテキスト][] 後から使用できる変数。
-   [認証ポリシーの][]
    -   [基本的なで認証][] -基本認証を使用してバックエンド サービスで認証します。
    -   [クライアント証明書ので認証][] -クライアント証明書を使用してバックエンド サービスで認証します。
-   [キャッシュ ポリシーの][]
    -   [キャッシュから取得][] -キャッシュを検索し、使用可能な場合は有効なキャッシュされた応答を返します。
    -   [をキャッシュするストア][] -指定したキャッシュ制御構成に従って応答をキャッシュします。
    -   [キャッシュから値を取得](https://msdn.microsoft.com/library/azure/dn894086.aspx#GetFromCacheByKey) -キーによってキャッシュされた項目を取得します。
    -   [値をキャッシュに格納](https://msdn.microsoft.com/library/azure/dn894086.aspx#StoreToCacheByKey) -キーによって、キャッシュに項目を格納します。
-   [クロス ドメイン ポリシー][]
    -   [許可クロス ドメイン呼び出し][] -Adobe Flash や Microsoft Silverlight ブラウザー ベースのクライアントからアクセス可能な API を作成します。
    -   [CORS:operator[]][] -クロス オリジン リソース共有 (CORS) サポートを操作またはブラウザー ベースのクライアントからのクロス ドメイン呼び出しを許可するように API を追加します。
    -   [JSONP:operator[]][] -JSON with padding (JSONP) のサポートを操作または API JavaScript ブラウザー ベースのクライアントからのクロス ドメイン呼び出しを許可するように追加します。
-   [変換ポリシー][]
    -   [JSON XML から変換][] - 変換の要求本文または応答本文を JSON からです。
    -   [JSON に XML を変換][] - 変換の要求本文または応答本文の XML から JSON にします。
    -   [検索し、置換 ][] - 要求または応答の部分文字列を検索し、別の部分に置き換えられます。
    -   [[コンテンツ] の Url のマスク][] -(マスクして) リンクを書き換え、応答本文および場所ヘッダー、ゲートウェイ経由で同等のリンクを指し示すようにします。
    -   [セット バックエンド サービスの][] -受信要求のバックエンド サービスに変更します。
    -   [セット本文][] -着信要求と発信要求のメッセージ本文を設定します。
    -   [設定の HTTP ヘッダーの][] - 既存の応答または要求ヘッダーに値を代入するか、新しい応答または要求ヘッダーを追加します。
    -   [セットのクエリ パラメーターの string[]][] - を追加、値の置換、または要求クエリ文字列パラメーターを削除します。
    -   [書き換え URL][] -要求 URL をパブリックな形式から web サービスで求められる形式に変換します。

## 次のステップ

ポリシー式に関する詳細については、次のビデオをご覧ください。

> [AZURE.VIDEO policy-expressions-in-azure-api-management]


[access restriction policies]: https://msdn.microsoft.com/library/azure/dn894078.aspx 
[check http header]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#CheckHTTPHeader 
[limit call rate by subscription]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#LimitCallRate 
[restrict caller ips]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#RestrictCallerIPs 
[set usage quota by subscription]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#SetUsageQuota 
[validate jwt]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#ValidateJWT 
[advanced policies]: https://msdn.microsoft.com/library/azure/dn894085.aspx 
[control flow]: https://msdn.microsoft.com/library/azure/dn894085.aspx#choose 
[set variable]: https://msdn.microsoft.com/library/azure/dn894085.aspx#set_variable 
[expressions]: https://msdn.microsoft.com/library/azure/dn910913.aspx 
[context]: https://msdn.microsoft.com/library/azure/ea160028-fc04-4782-aa26-4b8329df3448#ContextVariables 
[forward request]: https://msdn.microsoft.com/library/azure/dn894085.aspx#ForwardRequest 
[log to event hub]: https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub 
[authentication policies]: https://msdn.microsoft.com/library/azure/dn894079.aspx 
[authenticate with basic]: https://msdn.microsoft.com/library/azure/061702a7-3a78-472b-a54a-f3b1e332490d#Basic 
[authenticate with client certificate]: https://msdn.microsoft.com/library/azure/061702a7-3a78-472b-a54a-f3b1e332490d#ClientCertificate 
[caching policies]: https://msdn.microsoft.com/library/azure/dn894086.aspx 
[get from cache]: https://msdn.microsoft.com/library/azure/8147199c-24d8-439f-b2a9-da28a70a890c#GetFromCache 
[store to cache]: https://msdn.microsoft.com/library/azure/8147199c-24d8-439f-b2a9-da28a70a890c#StoreToCache 
[cross domain policies]: https://msdn.microsoft.com/library/azure/dn894084.aspx 
[allow cross-domain calls]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#AllowCrossDomainCalls 
[cors]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#CORS 
[jsonp]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#JSONP 
[transformation policies]: https://msdn.microsoft.com/library/azure/dn894083.aspx 
[convert json to xml]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#ConvertJSONtoXML 
[convert xml to json]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#ConvertXMLtoJSON 
[find and replace string in body]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#Findandreplacestringinbody 
[mask urls in content]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#MaskURLSContent 
[set backend service]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#SetBackendService 
[set body]: https://msdn.microsoft.com/library/azure/dn894083.aspx#SetBody 
[set http header]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#SetHTTPheader 
[set query string parameter]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#SetQueryStringParameter 
[rewrite url]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#RewriteURL 
[policies in api management]: api-management-howto-policies.md 
[api management policy reference]: https://msdn.microsoft.com/library/azure/dn894081.aspx 
[policy expressions]: https://msdn.microsoft.com/library/azure/dn910913.aspx 

