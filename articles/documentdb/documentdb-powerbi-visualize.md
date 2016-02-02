<properties 
    pageTitle="DocumentDB コネクタ用 Power BI チュートリアル | Microsoft Azure" 
    description="この Power BI のチュートリアルでは、JSON をインポートしたり、洞察に富むレポートを作成したり、DocumentDB および Power BI コネクタを使用してデータを視覚化する方法を説明します。" 
    keywords="power bi tutorial, visualize data, power bi connector"
    services="documentdb" 
    authors="h0n" 
    manager="jhubbard" 
    editor="mimig" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="11/18/2015" 
    ms.author="hawong"/>


# DocumentDB 用の Power BI チュートリアル: Power BI コネクタでデータを視覚化する

[PowerBI.com](https://powerbi.microsoft.com/) オンライン サービスを作成し、組織にとって重要なは、データをダッシュ ボードとレポートを共有することができます。 Power BI Desktop は、各種データ ソースのデータを取得し、データの結合と変換および強力なレポートと視覚エフェクトの作成を行い、レポートを Power BI に発行することができるレポート作成専用ツールです。 最新バージョンの Power BI Desktop では、Power BI 用 DocumentDB コネクタ経由で DocumentDB アカウントに接続できるようになりました。

この Power BI チュートリアルでは、Power BI Desktop で DocumentDB アカウントに接続し、ナビゲーターを使用してデータを抽出するコレクションに移動して、Power BI Desktop クエリ エディターで JSON データを表形式に変換し、レポートを作成して PowerBI.com に発行する手順について説明します。

この Power BI チュートリアルを完了すると、次の項目について説明できるようになります。

-   Power BI Desktop では、どのようにして DocumentDB のデータでレポートを作成できますか?
-   Power BI Desktop では、どのようにして DocumentDB アカウントに接続できますか?
-   Power BI Desktop では、どのようにしてコレクションからデータを取得できますか?
-   Power BI Desktop では、どのようにして入れ子になった JSON データを変換できますか?
-   PowerBI.com では、どのようにしてレポートを発行して共有できますか?

## 前提条件

この Power BI チュートリアルの手順を実行する前に、次のものを備えておく必要があります。

- [、最新バージョンの Power BI Desktop](https://powerbi.microsoft.com/desktop)します。
- Azure DocumentDB アカウント内のデモ アカウントまたはデータへのアクセス。
    - デモ アカウントには、このチュートリアルで示される火山データが設定されています。 このデモ アカウントは、どの SLA の制約も受けず、目的がデモンストレーションに限定されています。 Microsoft では、事前の通知や理由の提示なしでいつでもこのデモ アカウントに変更を加える権利を保有しています。これには、アカウントの終了、キーの変更、アクセスの制限、データの変更と削除などが含まれます。 
        -URL: https://analytics.documents.azure.com
        読み取り専用のキー: MSr6kt7Gn0YRQbjd6RbTnTt7VHc5ohaAFu7osF0HdyQmfR + YhwCH2D2jcczVIR1LNK3nMPNBD31losN7lQ/fkw = =
    - または、自分のアカウントを作成するを参照してください。 [Azure ポータルを使用して DocumentDB データベース アカウントの作成](https://azure.microsoft.com/documentation/articles/documentdb-create-account/)します。次に、サンプル火山を取得する内容を次のようなデータは、このチュートリアルで使用 (しますが、GeoJSON ブロックは含まれません) を参照してください、 [NOAA サイト](https://www.ngdc.noaa.gov/nndc/struts/form?t=102557&s=5&d=5) を使用してデータをインポートし、 [DocumentDB データ移行ツール](https://azure.microsoft.com/documentation/articles/documentdb-import-data/)します。


PowerBI.com でレポートを共有するには、PowerBI.com のアカウントが必要です。 無償版および Power BI Pro の Power BI の詳細については、次を参照してください [https://powerbi.microsoft.com/pricing](https://powerbi.microsoft.com/pricing)します。

## 作業を開始する

このチュートリアルでは、ユーザーが世界中の火山を研究している地質学者であると仮定します。 火山データは DocumentDB アカウントに格納されており、JSON ドキュメントは次のようになっています。

    {
        "Volcano Name": "Rainier",
        "Country": "United States",
        "Region": "US-Washington",
        "Location": {
            "type": "Point",
            "coordinates": [
            -121.758,
            46.87
            ]
        },
        "Elevation": 4392,
        "Type": "Stratovolcano",
        "Status": "Dendrochronology",
        "Last Known Eruption": "Last known eruption from 1800-1899, inclusive"
    }   

DocumentDB アカウントから火山データを取得し、次のような対話型の Power BI レポートでデータを視覚化する必要があります。

![Power BI Desktop のデータを視覚化する火山レポート](./media/documentdb-powerbi-visualize/power_bi_connector_pbireportfinal.png)

試してみる準備はできましたか。 それでは始めましょう。


1. ワークステーションで Power BI Desktop を実行します。
2. Power BI Desktop を起動すると、*ようこそ*画面が表示されます。

    ![Power BI Desktop 「ようこそ」 画面](./media/documentdb-powerbi-visualize/power_bi_connector_welcome.png)

3. *ようこそ*画面から直接、**データを取得し**、**最近使ったソース**を確認し、**他のレポートを開く**ことができます。 右上隅の [X] をクリックして画面を閉じます。 Power BI Desktop の **[レポート]** ビューが表示されます。

    ![Power BI Desktop レポート ビュー](./media/documentdb-powerbi-visualize/power_bi_connector_pbireportview.png)

4. **[ホーム]** リボンをクリックし、**[データの取得]** をクリックします。 **[データの取得]** ウィンドウが表示されます。

5. **[Azure]** をクリックし、**[Microsoft Azure DocumentDB (ベータ版)]** を選択して、**[接続]** をクリックします。 **[Microsoft Azure DocumentDB 接続]** ウィンドウが表示されます。

    ![Power BI Desktop データの取得](./media/documentdb-powerbi-visualize/power_bi_connector_pbigetdata.png)

6. 次のように、データを取得する DocumentDB アカウント エンドポイント URL を指定し、**[OK]** をクリックします。 URL は、Azure ポータルの **[キー]** ブレードで URI ボックスから取得するか、上記で指定されたデモ アカウント情報を使用することができます。 詳細については、次を参照してください。 [キー](documentdb-manage-account.md#keys)します。

    * に注意してください。 このチュートリアルでは、オプションのフィールドであるデータベース名、コレクション名、および SQL ステートメントは指定しません。 代わりに、私たちはナビゲーターを使用してデータの取得 from.* を識別するためにデータベースとコレクションの選択

    ![Power BI Desktop 接続ウィンドウ](./media/documentdb-powerbi-visualize/power_bi_connector_pbiconnectwindow.png)

7. このエンドポイントに初めて接続した場合は、アカウント キーの入力を求められます。 アカウント キーを入力し、**[接続]** をクリックします。

    * に注意してください。 レポートを作成する際は読み取り専用キーを使用することをお勧めします。 これにより、マスター キーが不用意に公開される潜在的なセキュリティ リスクを抑えることができます。 読み取り専用キーが Azure ポータルの [読み取り専用キー] ブレードから利用できるか above.* デモ アカウント情報を使用することができます。

    ![Power BI Desktop アカウント キーL](./media/documentdb-powerbi-visualize/power_bi_connector_pbidocumentdbkey.png)

8. アカウントが正常に接続されると、**ナビゲーター**が表示されます。 **ナビゲーター**には、アカウントで利用できるデータベースの一覧が表示されます。
9. レポートのデータを取得するデータベースをクリックして展開します。 データベースの下にコレクションの一覧が表示されます。

10. これで、データを取得するコレクション (volcano1 など) を選択できます。

    * に注意してください。 プレビュー ウィンドウに、**Record** アイテムの一覧が表示されます。 Power BI では、ドキュメントは **Record** タイプとして表されます。 同様に、ドキュメント内の入れ子になった JSON ブロックは、また、 **レコード**. *

    ![Power BI Desktop ナビゲーター](./media/documentdb-powerbi-visualize/power_bi_connector_pbinavigator.png)

11. データを変換するために、**[編集]** をクリックしてクエリ エディターを起動します。

## JSON ドキュメントをフラット化して変換する

1. Power BI のクエリ エディターで表示されます、 **ドキュメント** 中央のウィンドウ内の列です。
![Power BI Desktop クエリ エディター](./media/documentdb-powerbi-visualize/power_bi_connector_pbiqueryeditor.png)

2. **[ドキュメント]** 列ヘッダーの右側にある展開コントロールをクリックします。 フィールドの一覧を示すコンテキスト メニューが表示されます。 たとえば、レポートの必要なフィールド、火山名、国、リージョン、場所、昇格、型、状態と最後の把握の発生をクリックし選択 **OK**します。

    ![Power BI Desktop ドキュメント展開](./media/documentdb-powerbi-visualize/power_bi_connector_pbiqueryeditorexpander.png)

3. 中央のウィンドウに、選択したフィールドを含む結果のプレビューが表示されます。

    ![Power BI Desktop 一括結果](./media/documentdb-powerbi-visualize/power_bi_connector_pbiresultflatten.png)

4. この例では、Location プロパティは、ドキュメント内の GeoJSON ブロックです。 ご覧のように、Power BI Desktop では Location は **Record** タイプとして表されます。
5. [Location] 列ヘッダーの右側にある展開コントロールをクリックします。 type フィールドと coordinates フィールドを含むコンテキスト メニューが表示されます。 coordinates フィールドを選択して **[OK]** をクリックします。

    ![Power BI Desktop 場所のレコード](./media/documentdb-powerbi-visualize/power_bi_connector_pbilocationrecord.png)

6. 中央のウィンドウに、**List** タイプの [coordinates] 列が表示されます。 このチュートリアルの冒頭に示されているように、ここで使用する GeoJSON データは、Latitude 値と Longitude 値が coordinates 配列に記録された Point タイプです。

    * に注意してください。 座標 [0] の要素が経度を表し、座標 [1] が Latitude.* を表します
    ![Power BI Desktop 座標リスト](./media/documentdb-powerbi-visualize/power_bi_connector_pbiresultflattenlist.png)

7. coordinates 配列をフラット化するために、LatLong という **カスタム列**を作成します。 **[列の追加]** リボンを選択し、**[カスタム列の追加]** をクリックします。 **[カスタム列の追加]** ウィンドウが表示されます。

8. "LatLong" など、新しい列の名前を入力します。

9. 次に、新しい列のカスタム式を指定します。たとえば、次の式を使用して次のように、コンマで区切られた緯度と経度の値が連結: Text.From([coordinates]\{1\}) &「,」& Text.From([coordinates]{0}) します。**[OK]** をクリックします。

    * に注意してください。 DAX 関数を含むデータ分析 Expressions(DAX) の詳細については、次を参照してください [Power BI Desktop で基本的な DAX](https://support.powerbi.com/knowledgebase/articles/554619-dax-basics-in-power-bi-desktop). *

    ![Power BI Desktop カスタム列](./media/documentdb-powerbi-visualize/power_bi_connector_pbicustomlatlong.png)

10. これで、中央のウィンドウには Latitude と Longitude の値がコンマ区切りで設定された新しい [LatLong] 列が表示されます。

    ![Power BI Desktop カスタム LatLong](./media/documentdb-powerbi-visualize/power_bi_connector_pbicolumnlatlong.png)

11. このように、データをフラット化して表形式に変換することができました。 クエリ エディターのすべての機能を活用して、DocumentDB のデータを整形および変換することができます。 たとえば、Elevation のデータ型を**10 進数**に変更するには、**[ホーム]** リボンで **[データ型]** を変更します。

    ![Power BI Desktop 種類の変更](./media/documentdb-powerbi-visualize/power_bi_connector_pbichangetype.png)

12. **[終了して適用]** をクリックしてデータ モデルを保存します。

    ![Power BI Desktop 終了して適用](./media/documentdb-powerbi-visualize/power_bi_connector_pbicloseapply.png)

## レポートを作成する

Power BI Desktop レポート ビューは、データを視覚化するためにレポート作成を開始できる場所です。 **[レポート]** キャンバスにフィールドをドラッグ アンド ドロップすることにより、レポートを作成できます。

![Power BI Desktop レポート ビュー](./media/documentdb-powerbi-visualize/power_bi_connector_pbireportview2.png)

レポート ビューには以下が表示されます。

 1. **[フィールド]** ウィンドウ。ここには、レポートに使用できるフィールドと共にデータ モデルの一覧が表示されます。

 2. **[視覚エフェクト]** ウィンドウ。 レポートに 1 つまたは複数の視覚エフェクトを含めることができます。 **[視覚エフェクト]** ウィンドウで、ニーズに合った視覚エフェクトの種類を指定します。

 3. **[レポート]** キャンバス。ここでは、レポート用の視覚エフェクトを作成します。

 4. **[レポート]** ページ。 Power BI Desktop では、複数のレポート ページを追加できます。

単純な対話型マップ ビュー レポートを作成する基本的な手順を次に示します。

1. 例として、個々の火山の場所を示すマップ ビューを作成します。  **視覚エフェクト** ] ウィンドウで、上記のスクリーン ショットをハイライトとしてマップ ビジュアルの種類をクリックします。 **[レポート]** キャンバスに、ビジュアルの種類のマップが描かれます。 **[視覚化エフェクト]** ウィンドウには、ビジュアルの種類のマップに関連するプロパティのセットも表示されます。

2. ここで、**[フィールド]** ウィンドウの LatLong フィールドを **[視覚エフェクト]** ウィンドウの **Location** プロパティにドラッグ アンド ドロップします。
3. 次に、Volcano Name フィールドを **Legend** プロパティにドラッグ アンド ドロップします。

4. さらに、Elevation フィールドを **Values** プロパティにドラッグ アンド ドロップします。

5. ここで、マップ ビジュアルに複数のバブルが表示されます。バブルは個々の火山の場所を示し、バブルのサイズは火山の高度と相関性があります。

6. これで、基本的なレポートを作成できました。 別の視覚エフェクトを追加することで、レポートをさらにカスタマイズできます。 この例では、レポートを対話的にするために Volcano Type スライサーを追加しました。

    ![Power BI Desktop レポート最終](./media/documentdb-powerbi-visualize/power_bi_connector_pbireportfinal.png)

## レポートを発行して共有する

レポートを共有するには、PowerBI.com のアカウントが必要です。

1. Power BI Desktop で、をクリックして、 **ホーム** リボンです。
2. **[発行]** をクリックします。 PowerBI.com アカウントのユーザー名とパスワードの入力が求められます。
3. 資格情報が認証されると、PowerBI.com アカウントにレポートが発行されます。
4. これにより、PowerBI.com でレポートを共有できます。

## 次のステップ

- Power BI の詳細については、クリックして [ここ](https://support.powerbi.com/knowledgebase)
- DocumentDB の詳細については、次のようにクリックします。 [ここ](https://azure.microsoft.com/documentation/services/documentdb/)します。





