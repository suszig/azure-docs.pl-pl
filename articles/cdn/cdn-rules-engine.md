<properties 
    pageTitle="CDN - 規則エンジンを使用した既定の HTTP 動作のオーバーライド" 
    description="規則エンジンでは、HTTP 要求の処理方法 (特定種類のコンテンツの配信のブロックなど) のカスタマイズのほか、キャッシュ ポリシーの定義、HTTP ヘッダーの変更などを行えます。" 
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

# 規則エンジンを使用して既定の HTTP 動作をオーバーライドする

## 概要

規則エンジンでは、特定の種類のコンテンツの配信のブロック、キャッシュ ポリシーの定義、HTTP ヘッダーの変更など、HTTP 要求の処理方法をカスタマイズできます。  このチュートリアルでは、CDN 資産のキャッシュ動作を変更するルールの作成について説明します。

> [AZURE.NOTE] ルール エンジンは、Premium CDN 層の機能です。  Standard および Premium CDN 機能の比較では、次を参照してください。 [Azure CDN の概要](cdn-overview.md)します。

## チュートリアル

1. CDN プロファイル ブレードで、クリックして、 **管理** ] ボタンをクリックします。

    ![[CDN プロファイル] ブレードの [管理] ボタン](./media/cdn-rules-engine/cdn-rules-manage-btn.png)
    
    CDN 管理ポータルが開きます。
    
2. をクリックして、 **HTTP 大きな** ] タブで後に **ルール エンジン**します。
    
    新しい規則のオプションが表示されます。
    
    ![CDN の新しい規則オプション](./media/cdn-rules-engine/cdn-new-rule.png)

3. 名前を入力、 **名前/説明** ] ボックスに貼り付けます。

4. 規則を適用する要求の種類を指定します。  既定では、 **常に** 一致条件を選択します。  使う **常に** このチュートリアルでのままに選択します。

    ![CDN の一致条件](./media/cdn-rules-engine/cdn-request-type.png)
    
    >[AZURE.TIP] あるさまざまな種類の一致のドロップダウン リストで利用可能な状態です。  一致条件の左側の青い情報アイコンをクリックすると、現在選択している状態の詳細が表示されます。
    >
    >すべての一致条件の詳細の一覧では、次を参照してください。 [ルール エンジンの一致条件と機能の詳細](cdn-rules-engine-details.md#match-conditions)します。
    
5.  をクリックして、 **+** ボックスの横に **機能** 新しい機能を追加します。  左側のドロップダウン リストで選択 **Force 内部 Max-age**します。  表示されるテキスト ボックスに、次のように入力します。 **300**します。  他の既定値はそのまま使用します。

    ![CDN の機能](./media/cdn-rules-engine/cdn-new-feature.png)

    >[AZURE.NOTE] 一致の条件に、新しい機能の左側にある青色の情報] アイコンをクリックすると詳細が表示されますこの機能についてです。  場合に **Force 内部 Max-age**, 、アセットをオーバーライドお **Cache-control** と **Expires** CDN エッジ ノードが元の資産を更新する場合を制御するヘッダー。  今回の 300 秒のサンプルでは、その配信元から資産を更新する前に、CDN エッジ ノードは 5 分間、資産をキャッシュします。
    >
    >すべての機能の詳細の一覧では、次を参照してください。 [ルール エンジンの一致条件と機能の詳細](cdn-rules-engine-details.md#features)します。
    
6.  クリックして、 **追加** 新しいルールを保存するボタンをクリックします。  次に、新しい規則は承認を待機します。 ステータスはから変更が承認すると、 **保留中の XML** に **Active XML**します。

## 考慮事項

- 複数の規則が表示される順序は、これらの規則の処理方法に影響します。 前の規則で指定したアクションは、後続の規則でオーバーライドされます。

## 関連項目
* [ルール エンジンの一致条件と機能詳細](cdn-rules-engine-details.md)
* [Azure CDN の概要](cdn-overview.md)
* [Microsoft Azure CDN でのリアルタイム統計情報](cdn-real-time-stats.md)
* [詳細な HTTP レポート](cdn-advanced-http-reports.md)
* [Edge のパフォーマンスの分析](cdn-edge-performance.md)


    


