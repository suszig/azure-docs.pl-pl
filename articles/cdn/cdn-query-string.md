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


# クエリ文字列による CDN 要求のキャッシュ動作の制御

> [AZURE.SELECTOR]
- [Standard](cdn-query-string.md)
- [Premium](cdn-query-string-premium.md)


## 概要

クエリ文字列のキャッシュにより、ファイルにクエリ文字列が含まれている場合のファイルのキャッシュ方法を制御します。
> [AZURE.NOTE] Standard および Premium CDN レベルでは同じクエリ文字列キャッシュ機能が提供されますが、ユーザー インターフェイスは異なります。 このドキュメントでは、**Standard** レベルのユーザー インターフェイスについて説明します。 Premium 階層を参照してください。 [CDN のキャッシュ動作を制御するが、クエリ文字列の Premium 要求](cdn-query-string-premium.md)します。

次の 3 つのモードを使用できます。

- **クエリ文字列を無視する**: これは、既定のモードです。 CDN エッジ ノードは、クエリ文字列を要求元から最初の要求の配信元に渡して、資産をキャッシュします。 エッジ ノードから提供される資産の後続の要求はすべて、キャッシュされた資産の有効期限が切れるまで、クエリ文字列を無視します。
- **クエリ文字列を含む URL のキャッシュをバイパス**: このモードでクエリ文字列を含む要求は CDN エッジ ノードにキャッシュされません。 エッジ ノードは配信元から直接資産を取得し、それを各要求により要求元に渡します。
- **すべての一意の URL をキャッシュ**: このモードでは、独自のキャッシュを使用して一意な資産としてクエリ文字列では、各要求を処理します。 たとえば、*foo.ashx?q=bar* の要求の配信元からの応答はエッジ ノードでキャッシュされ、後続のキャッシュではその同じクエリ文字列により返されます。 *foo.ashx?q=somethingelse* の要求は、独自の有効期限を持つ個別の資産としてキャッシュされます。
    >[AZURE.WARNING] このモードは、クエリ文字列に各要求で異なるパラメーター (セッション ID、ユーザー名など) が含まれる場合には使用できません。これは、非常に低いキャッシュ ヒット率を招くことがあるためです。

## クエリ文字列キャッシュ設定を変更する

1. CDN プロファイル ブレードで、管理する CDN エンドポイントをクリックします。

    ![CDN プロファイル ブレード エンドポイント](./media/cdn-query-string/cdn-endpoints.png)

    CDN エンドポイントのブレードが開きます。

2. **[構成]** をクリックします。

    ![[CDN プロファイル] ブレードの ](./media/cdn-query-string/cdn-config-btn.png)

    CDN 構成ブレードが開きます。

3. **[クエリ文字列のキャッシュ動作]** ドロップダウンから設定を選択します。

    ![CDN クエリ文字列のキャッシュ オプション](./media/cdn-query-string/cdn-query-string.png)

4. 選択したら、**[保存]** をクリックします。











