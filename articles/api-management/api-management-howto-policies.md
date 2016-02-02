<properties 
    pageTitle="Azure API Management のポリシー" 
    description="API Management のポリシーを作成、編集、構成する方法について説明します。" 
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
    ms.date="12/02/2015" 
    ms.author="sdanie"/>



# Azure API Management のポリシー

Azure API Management のポリシーは、発行者がその構成を通じて API の動作を変更できる、システムの強力な機能の 1 つです。 ポリシーは、API の要求または応答に対して順に実行される一連のステートメントのコレクションです。 代表的なステートメントとしては、XML 形式から JSON 形式への変換や、(開発者からの呼び出しの回数を制限する) 呼び出しレート制限が挙げられます。 他にも多数のポリシーが標準で提供されています。

参照してください、 [ポリシーのリファレンス][] ポリシー ステートメントとその設定の完全な一覧にします。

ポリシーは、API コンシューマーとマネージ API の間に配置されたゲートウェイ内で適用されます。 ゲートウェイは、すべての要求を受け取り、通常はそれらの要求をそのまま基底の API に転送します。 ただし、ポリシーを使用すると、受信要求と送信応答の両方に変更を適用できます。

ポリシーの式は、ポリシーで特に指定されていない限り、任意の API Management ポリシーで属性値またはテキスト値として使用できます。 などのいくつかのポリシー、 [制御フロー][] と [セット変数][] ポリシーはポリシー式に基づいています。 詳細については、次を参照してください。 [ポリシー [高度な][] と [ポリシー式 []][]します。

## <a name="scopes"> </a>ポリシーを構成する方法

グローバルまたはのスコープでは、ポリシーを構成することができます、 [製品][], 、[API][] または [操作][]します。 ポリシーを構成するには、パブリッシャー ポータルのポリシー エディターに移動します。

![[ポリシー] メニュー][policies-menu]

ポリシー エディターは、[ポリシー スコープ] (上部)、ポリシーを編集するための [ポリシー定義] (左側)、およびステートメントの一覧 (右側) の 3 つのメイン セクションから構成されます。

![Policies editor][policies-editor]

ポリシーの構成を開始するには、ポリシーを適用するスコープを最初に選択する必要があります。 次のスクリーンショットでは、**[スターター]** 成果物が選択されています。 ポリシー名の横の四角形は、既にポリシーがこのレベルで適用されていることを示します。

![スコープ][policies-scope]

ポリシーが既に適用されているため、定義ビューに構成が表示されます。

![構成][policies-configure]

ポリシーは、最初は読み取り専用として表示されます。 定義を編集するには、**[ポリシーの構成]** アクションをクリックします。

![編集][policies-edit]

ポリシー定義は、一連の受信ステートメントと送信ステートメントが記述された単純な XML ドキュメントです。 XML は、定義ウィンドウで直接編集できます。 ウィンドウの右側には、ステートメントの一覧が表示されます。上のスクリーンショットの**[呼び出しレート制限]** ステートメントを見るとわかるように、現在のスコープに適用できるステートメントが有効になり、強調表示されます。

有効なステートメントをクリックすると、定義ビュー内のカーソル位置に適切な XML が追加されます。

ポリシー ステートメントの完全な一覧とその設定で使用可能な [ポリシーのリファレンス][]します。

たとえば、着信要求を指定した IP アドレスに制限する新しいステートメントを追加するのコンテンツ内にカーソルを配置、 `受信` XML 要素をクリック、 **だけに制限する呼び出し元 Ip** ステートメントです。

![制限ポリシー][policies-restrict]

これに XML スニペットを追加するは、 `受信` 、ステートメントを構成する方法について説明する要素。

    <ip-filter action="allow | forbid">
        <address>address</address>
        <address-range from="address" to="address"/>
    </ip-filter>

受信要求を制限して、IP アドレス 1.2.3.4 からの要求のみを受け付けるには、次のように XML を変更します。

    <ip-filter action="allow">
        <address>1.2.3.4</address>
    </ip-filter>

![保存][policies-save]

ポリシーのステートメントを構成したら、**[保存]** をクリックします。変更は、API Management ゲートウェイに即座に反映されます。

## <a name="sections"> </a>ポリシー構成について

ポリシーは、要求および応答に関して順に実行される一連のステートメントから構成されます。 構成が適切に分けられています `受信`, 、`バックエンド`, 、`送信`, 、および `エラー` セクションの次の構成で示すようにします。

    <policies>
      <inbound>
        
      </inbound>
      <backend>
        
      </backend>
      <outbound>
        
      </outbound>
      <on-error>
        
      </on-error>
    </policies> 

要求の処理中にエラーがある場合、残りの手順を `受信`, 、`バックエンド`, 、または `送信` セクションをスキップし、実行が内のステートメントにジャンプ、 `エラー` セクションです。 ポリシー ステートメントを配置することで、 `エラー` セクション、を使用して、エラーを確認できます `コンテキスト。LastError` プロパティを検査し、エラー応答を使用して、カスタマイズ、 `本文の設定` ポリシー、およびエラーが発生した場合の動作を構成します。 組み込み手順用と、ポリシー ステートメントの処理中に発生する可能性があるエラー用のエラー コードがあります。 詳細については、次を参照してください。 [API Management ポリシーでのエラー処理](https://msdn.microsoft.com/library/azure/mt629506.aspx)します。

ポリシーは異なるレベル (グローバル、成果物、API、および操作) で指定できるため、構成では、親ポリシーに対するポリシー定義のステートメントを実行する順序を指定できるようになっています。

ポリシー スコープは、次の順序で評価されます。

1. グローバル スコープ
2. 成果物スコープ
3. API スコープ
4. 操作スコープ

そこに含まれるステートメントの配置評価する、 `基本` 要素が存在する場合。

たとえば、グローバル レベルのポリシーと API 向けに構成されたポリシーがある場合、API が使用されるたびに両方のポリシーが適用されます。 API Management では、基本要素を介してポリシー ステートメントの組み合わせの順序を指定できます。

    <policies>
        <inbound>
            <cross-domain />
            <base />
            <find-and-replace from="xyz" to="abc" />
        </inbound>
    </policies>

上記の例のポリシー定義で、 `ドメインを越えた` 、逆にするか、上位のポリシーの後に前にステートメントが実行、 `検索と置換` ポリシーです。

ポリシー ステートメントの中で同じポリシーが 2 回出現した場合は、最近評価されたポリシーが適用されます。 これを使用して、上位のスコープに定義されているポリシーを上書きできます。 現在のスコープに含まれるポリシーをポリシー エディターに表示するには、**[選択したスコープの有効なポリシーを再計算する]** をクリックします。

グローバル ポリシーには親ポリシーが含まれていないことに注意してくださいし、を使用して、 `< 基本 >` 要素を使用しても何も起こりません。

## 次のステップ

ポリシーの式に関する次のビデオをご覧ください。

> [AZURE.VIDEO policy-expressions-in-azure-api-management]


[policy reference]: api-management-policy-reference.md 
[product]: api-management-howto-add-products.md 
[api]: api-management-howto-add-products.md#add-apis 
[operation]: api-management-howto-add-operations.md 
[advanced policies]: https://msdn.microsoft.com/library/azure/dn894085.aspx 
[control flow]: https://msdn.microsoft.com/library/azure/dn894085.aspx#choose 
[set variable]: https://msdn.microsoft.com/library/azure/dn894085.aspx#set_variable 
[policy expressions]: https://msdn.microsoft.com/library/azure/dn910913.aspx 
[policies-menu]: ./media/api-management-howto-policies/api-management-policies-menu.png 
[policies-editor]: ./media/api-management-howto-policies/api-management-policies-editor.png 
[policies-scope]: ./media/api-management-howto-policies/api-management-policies-scope.png 
[policies-configure]: ./media/api-management-howto-policies/api-management-policies-configure.png 
[policies-edit]: ./media/api-management-howto-policies/api-management-policies-edit.png 
[policies-restrict]: ./media/api-management-howto-policies/api-management-policies-restrict.png 
[policies-save]: ./media/api-management-howto-policies/api-management-policies-save.png 

