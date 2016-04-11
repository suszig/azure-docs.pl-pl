<properties 
    pageTitle="Azure App Service のロジック アプリでの BizTalk 変換の使用 | Microsoft Azure" 
    description="あるスキーマから別のスキーマへ XML ドキュメントを変換する方法について説明します" 
    authors="anuragdalmia" 
    manager="dwrede" 
    editor="" 
    services="app-service\logic" 
    documentationCenter=""/>

<tags
    ms.service="app-service-logic"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/07/2015"
    ms.author="anuragdalmia"/>

# BizTalk 変換


## 概要
簡単に言うと、BizTalk 変換 API アプリは、1 つの形式から別の形式へデータを変換します。 たとえば、注文書から出荷先住所と請求先住所を取得して、請求書ドキュメントに挿入する場合などです。 受信メッセージの現在の日付が含まれる場合がありますか、 *YearMonthDay* 形式です。 日付の書式を変更する、 *MonthDayYear* 形式です。 

Microsoft Azure App Service の変換 API プリを使用してこれを実行できます。 変換 (マップともいう) は、送信元 XML スキーマ (入力) と送信先 XML スキーマ (出力) で構成されます。 さまざまな組み込み関数を使用すると、文字列操作、条件付き割り当て、算術式、日時の書式設定や、ループ構造の操作などで、データを操作および制御できます。 

Visual Studio でのマップを作成を使用して、 [Microsoft Azure BizTalk Services SDK](http://www.microsoft.com/download/details.aspx?id=39087)します。 マップの作成とテストが終了したら、マップ (.trfm) を BizTalk 変換 API アプリへアップロードします。

他には次の機能があります。

- マップで作成される変換は、あるドキュメントから別のドキュメントに名前と住所をコピーするなど、単純な内容になります。 または、設定不要なマップ操作を使用して、より複雑な変換を作成することができます。
- 文字列、日時の関数などの複合的なマップ操作や関数をすぐに使用できます。
- スキーマ間でデータを直接コピーできます。 BizTalk マッパーを使用すれば、この操作は送信元スキーマ内の要素を送信先スキーマの対応する部分につなぐ線を描画するくらい簡単です。
- マップを作成しているとき、作成したすべてのリレーションシップやリンクなど、マップがグラフィカルに表示されます。
- 使用して、 **マップのテスト** サンプル XML メッセージを追加する機能です。 クリックするだけで、作成したマップをテストして、生成された出力を参照できます。
- 既存の Azure BizTalk Services マップ (.trfm) をアップロードし、変換 API のアプリを最大限まで活用してください。
- マップを作成するときに、スキーマを追加する必要はありません。 マップの準備ができたら、変換 API アプリに追加して、先に進むことができます。 
- XML 形式のサポートが含まれます。


## コネクタ API Apps のスキーマをダウンロードする
API Apps の概要ページから、SQL、SAP、SharePoint などのコネクタ用の XML スキーマをダウンロードできます。 たとえば、特定の SAP コネクタ API Apps 用の XML スキーマをダウンロードする場合は、API アプリを参照して概要ページを開きます。 選択 **スキーマのダウンロード** され、SAP アクションに対応するすべてのスキーマを含む zip ファイルがコンピューターにダウンロードします。 このスキーマを使用して、Visual Studio でマップ (.trfm) を作成することができます。


## マップを作成および追加する
Visual Studio での変換またはマップの作成を使用して、 [Microsoft Azure BizTalk Services SDK](http://www.microsoft.com/download/details.aspx?id=39087), 、これは無料でダウンロードします。 

マップの作成については、次を参照してください。 [Visual Studio でマップを作成](http://aka.ms/createamapinvs)します。 マップを作成し、実稼働の準備ができると、Azure ポータルで作成した BizTalk 変換 API アプリにマップ (.trfm ファイル) を追加できます。 

アップロード後にマップが変更された場合、更新したマップをアップロードして変換 API アプリの既存のマップと置き換えることができます。

1.  選択 **参照** 選択し、Azure ポータル (画面の左側) **API Apps**します。 場合 **API Apps** 表示され、選択されていない **すべて**, を選択して **API Apps** 使用可能な一覧から。

    ![][7]

2.  すべての一覧 **API Apps** 作成、azure サブスクリプションが示すようにします。

    ![][8]

3.  前のセクションで作成した BizTalk 変換 API アプリを選択します。

4.  API アプリの構成ブレードが開きます。 ご覧 **マップ** コンポーネント] セクションで。

    ![][9]

5.  選択 **マップ** マップの一覧と共に新しいブレードを開きます。

6.  選択、 **マップの追加** を開くには上にアイコン、 **マップの追加** ブレード。

    ![][10]

7.  ファイル アイコンを選択して、ローカル コンピューターのマップ ファイル (.trfm) を見つけます。

8.  選択 **OK** され、新しいマップを作成します。 新しいマップがマップの一覧に表示されます。


## ロジック アプリで BizTalk 変換 API アプリを使用する
マップの作成とテストが終わると、すぐに使用可能な状態になります。

1. BizTalk の変換は、Logic Apps 内のギャラリーの右側から使用できます。 選択  **BizTalk 変換サービス** ギャラリーからです。 [変換] はフローに追加されています。

    ![][11]

2. 選択、 **変換** アクション。 入力パラメーターが表示されます。

    ![][12]

3. 完了するには、次のパラメーターを入力して、 **変換** アクションの構成。
         
    - 入力 XML
        - 変換 API アプリのマップの送信元スキーマに準拠した有効な XML コンテンツを入力します。 ここには、"Call RFC – SAP" や "Insert Into Table – SQL" などのロジック アプリの前のアクションの出力を入力することができます。
        
    - マップ名 (省略可能)
        - 変換 API アプリに、既にアップロードされている有効なマップ名を入力します。 マップが入力されていない場合、マップは送信元スキーマに準拠している入力 XML に基づいて自動的に選択されます。

    ![][13]

4. アクション "Output XML" の出力は、Logic Apps のアプリの後続のアクションで使用できます。

<!--Image references-->
[1]: ./media/app-service-logic-transform-xml-documents/Create_Everything.png
[2]: ./media/app-service-logic-transform-xml-documents/Create_Marketplace.png
[4]: ./media/app-service-logic-transform-xml-documents/Search_TransformAPIApp.png
[5]: ./media/app-service-logic-transform-xml-documents/Transform_APIApp_Landing_Page.png
[6]: ./media/app-service-logic-transform-xml-documents/New_TransformAPIApp_Blade.png
[7]: ./media/app-service-logic-transform-xml-documents/Browse_APIApps.png
[8]: ./media/app-service-logic-transform-xml-documents/Select_APIApp_List.png
[9]: ./media/app-service-logic-transform-xml-documents/Configure_Transform_APIApp.png
[10]: ./media/app-service-logic-transform-xml-documents/Add_Map.png
[11]: ./media/app-service-logic-transform-xml-documents/Transform_action_flow.png
[12]: ./media/app-service-logic-transform-xml-documents/Transform_Inputs.png
[13]: ./media/app-service-logic-transform-xml-documents/Transform_configured.png
[14]: ./media/app-service-logic-transform-xml-documents/Download_Schemas.png



 
