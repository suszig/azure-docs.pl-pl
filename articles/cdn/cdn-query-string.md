<properties 
    pageTitle="CDN - クエリ文字列による要求のキャッシュ動作の制御" 
    description="CDN クエリ文字列のキャッシュにより、ファイルにクエリ文字列が含まれている場合のファイルのキャッシュ方法を制御します。" 
    services="cdn" 
    documentationCenter=".NET" 
    authors="camsoper" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="cdn" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="12/02/2015" 
    ms.author="casoper"/>

#クエリ文字列による CDN 要求のキャッシュ動作の制御

> [AZURE.SELECTOR]
- [Standard](cdn-query-string.md)
- [Premium](cdn-query-string-premium.md)

##概要

クエリ文字列のキャッシュにより、ファイルにクエリ文字列が含まれている場合のファイルのキャッシュ方法を制御します。 

> [AZURE.NOTE] Standard および Premium CDN 層は、キャッシュ機能、同一のクエリ文字列を提供しますが、ユーザー インターフェイスが異なります。  このドキュメントで説明、 **標準** 層ユーザー インターフェイスです。  Premium 階層を参照してください。 [CDN のキャッシュ動作を制御するが、クエリ文字列の Premium 要求](cdn-query-string-premium.md)します。

次の 3 つのモードを使用できます。

- **クエリ文字列を無視する**: これは、既定のモードです。  CDN エッジ ノードは、クエリ文字列を要求元から最初の要求の配信元に渡して、資産をキャッシュします。  エッジ ノードから提供される資産の後続の要求はすべて、キャッシュされた資産の有効期限が切れるまで、クエリ文字列を無視します。
- **クエリ文字列を含む URL のキャッシュをバイパス**: このモードでクエリ文字列を含む要求は CDN エッジ ノードにキャッシュされません。  エッジ ノードは配信元から直接資産を取得し、それを各要求により要求元に渡します。
- **すべての一意の URL をキャッシュ**: このモードでは、独自のキャッシュを使用して一意な資産としてクエリ文字列では、各要求を処理します。  要求のための原点からの応答など *foo.ashx?q=bar* エッジ ノードにキャッシュし、同じクエリ文字列での後続のキャッシュに返されるとします。  要求 *foo.ashx?q=somethingelse* はライブに固有の時間を使用して個別の資産としてキャッシュされます。
    
    >[AZURE.WARNING] セッション ID またはユーザー名には、非常に低いキャッシュではしまうので、ヒット率など、クエリ文字列にパラメーターを要求ごとに変更が含まれている場合、このモードを使用しない必要があります。

##クエリ文字列キャッシュ設定を変更する

1. CDN プロファイル ブレードで、管理する CDN エンドポイントをクリックします。
    
    ![CDN プロファイル ブレード エンドポイント](./media/cdn-query-string/cdn-endpoints.png)

    CDN エンドポイントのブレードが開きます。

2. クリックして、 **構成** ] ボタンをクリックします。

    ![[CDN プロファイル] ブレードの [管理] ボタン](./media/cdn-query-string/cdn-config-btn.png)
    
    CDN 構成ブレードが開きます。
    
3. 設定を選択して、 **キャッシュの動作のクエリ文字列** ドロップダウンです。
    
    ![CDN クエリ文字列のキャッシュ オプション](./media/cdn-query-string/cdn-query-string.png)
    
4. 選択した後、クリックして、 **保存** ] ボタンをクリックします。




    


