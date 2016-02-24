<properties
    pageTitle="NodeJS での Azure Search の使用 | Microsoft Azure | ホスト型クラウド検索サービス"
    description="プログラミング言語として NodeJS を使用して Azure でホスト型クラウド検索アプリケーションを作成する手順を示します。"
    services="search"
    documentationCenter=""
    authors="HeidiSteen"
    manager="mblythe"
    editor="v-lincan"/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.date="11/04/2015"
    ms.author="heidist"/>

# NodeJS での Azure Search の使用

検索エクスペリエンスとして Azure Search を使用するカスタム NodeJS 検索アプリケーションを作成する方法を説明します。 このチュートリアルでは、 [Azure Search サービス REST API](https://msdn.microsoft.com/library/dn798935.aspx) オブジェクトおよびこの演習で使用する操作を作成します。

使用して [NodeJS](https://nodejs.org) と NPM、 [Sublime Text 3](http://www.sublimetext.com/3), 、および Windows PowerShell を Windows 8.1 このコードを開発し、テストします。

このサンプルを実行する必要がありますでにサインアップできる Azure Search サービス、 [Azure Classic Portal](https://portal.azure.com)します。

> [AZURE.TIP] このチュートリアルのソース コードをダウンロード [AzureSearchNodeJSIndexerDemo](http://go.microsoft.com/fwlink/p/?LinkId=530198)します。

## データについて

このサンプル アプリケーションからデータを使用して、 [United States Geological Services (USGS)](http://geonames.usgs.gov/domestic/download_data.htm), 、ロードアイランドでフィルター処理、状態のデータセットのサイズを小さきます。 このデータを使用して、病院や学校などの目立つ建物および河川、湖沼、山などの地理的特徴を返す検索アプリケーションを作成します。

このアプリケーションで、 **DataIndexer** プログラムがビルドされ、インデックスを使用して、ロード、 [インデクサー](https://msdn.microsoft.com/library/azure/dn798918.aspx) コンストラクト、パブリック Azure SQL Database からフィルター処理された USGS データセットを取得します。 資格情報と接続プログラム コードでオンラインでのデータ ソースへの情報を提供します。 それ以上の構成は必要ありません。

> [AZURE.NOTE] 件未満に無料の価格レベルの 10,000 ドキュメント制限には、このデータセットにフィルターを適用しました。 標準レベルを使用する場合は、この制限は適用されません。 各価格レベルの容量の詳細については、次を参照してください。 [制限および制約](search-limits-quotas-capacity.md)します。

## サービスの作成

1. サインイン [Azure クラシック ポータル](https://portal.azure.com)します。

2. ジャンプバーで、次のようにクリックします。 **新規** > **データ + ストレージ** > **検索**します。

     ![][1]

3. サービス名、価格レベル、リソース グループ、サブスクリプション、および場所を構成します。 これらの設定は必須であり、サービスがプロビジョニングされた後は変更できません。

     ![][2]

    - **サービス名** 一意で、以下の小文字、スペースなし、15 文字にする必要があります。 この名前は、Azure Search サービスのエンドポイントの一部になります。 参照してください [名前付け規則](https://msdn.microsoft.com/library/azure/dn857353.aspx) 名前付け規則の詳細についてです。

    - **価格レベル** 容量と課金を決定します。 どちらのレベルも同じ機能を提供しますが、リソース レベルが異なります。

        - **無料**  は他のサブスクライバーと共有しているクラスターで実行します。 Free 版はチュートリアルを試用して概念実証コードを書くには十分な機能を提供しますが、運用アプリケーションには対応していません。 Free サービスは、通常は数分でデプロイできます。
        - **標準的な** 専用リソースで実行され、拡張性に優れています。 最初、Standard サービスは 1 つのレプリカと 1 つのパーティションを使用してプロビジョニングされますが、サービスを作成した後で容量を調整することができます。 標準サービスをデプロイするには、通常は約 15 分かかります。

    - **リソース グループ** はサービスと、一般的な目的に使用されるリソースのコンテナーです。 たとえば、Azure Search、Azure Websites、Azure BLOB ストレージを使用してカスタム検索アプリケーションを構築する場合は、リソース グループを作成することで、これらのサービスをポータル管理ページにまとめておくことができます。

    - **サブスクリプション** 1 つ以上のサブスクリプションがある場合、複数のサブスクリプションの中から選択することができます。

    - **場所** はデータ センターのリージョンです。 現時点では、すべてのリソースは同じデータ センターで実行する必要があります。 複数のデータ センターにリソースを分散させることはできません。

4. クリックして **作成** サービスをプロビジョニングします。

ジャンプバーで、通知を確認します。 サービスが使用できるようになると、通知が表示されます。

<a id="sub-2"></a>
## Azure Search サービスのサービス名と API キーの取得

サービスを作成したら、ポータルに戻って URL または `api-key` を取得します。 Search サービスに接続するには、URL に加えて、呼び出しを認証するための `api-key` が必要になります。

1. ジャンプバーで、次のようにクリックします。 **ホーム** し、[Search サービスを開くには、サービス ダッシュ ボード] をクリックします。

2. サービスのダッシュボードには、基本情報のタイルのほか、管理者キーにアクセスするためのキー アイコンが表示されます。

    ![][3]

3. サービスの URL、管理キー、クエリ キーをコピーします。 後で config.js ファイルに追加するときにこれら 3 つがすべて必要になります。

## サンプル ファイルのダウンロード

次のいずれかの方法を使用してサンプルをダウンロードします。

1. 移動して [AzureSearchNodeJSIndexerDemo](http://go.microsoft.com/fwlink/p/?LinkId=530198)します。
2. クリックして **ZIP のダウンロード**, .zip ファイルを保存、およびが含まれているすべてのファイルを抽出します。

以降のすべてのファイル変更および実行ステートメントは、このフォルダー内のファイルに対して行われます。

または、パス ステートメントに GIT がある場合は、PowerShell ウィンドウを開いて「`git clone https://github.com/EvanBoyle/AzureSearchNodeJSIndexerDemo.git`」と入力してもかまいません。

## Config.js を更新します。 Search サービスの URL と api キー

先にコピーした URL と API キーを使用し、構成ファイルで URL、管理キー、クエリ キーを指定します。

管理キーは、インデックスの作成または削除やドキュメントの読み込みなど、サービス操作に対する完全な制御を付与します。 これに対し、クエリ キーは読み取り専用の操作用であり、通常は、Azure Search に接続するクライアント アプリケーションによって使用されます。

このサンプルでは、クライアント アプリケーションではクエリ キーを使用するというベスト プラクティスを補強するためにクエリ キーを含めます。

次のスクリーン ショット **config.js** 区分は、search サービスに対して有効な値を持つファイルを更新する場所を表示できるように、関連するエントリのテキスト エディターで開きます。

![][5]


## サンプルのランタイム環境のホスト

このサンプルには HTTP サーバーが必要です。これは、npm を使用してグローバルにインストールできます。

PowerShell ウィンドウを使用して次のコマンドを実行します。

1. 含まれているフォルダーに移動し、 **package.json** ファイルです。
2. 「`npm install`」と入力します。
2. 「`npm install -g http-server`」と入力します。

## インデックスの作成とアプリケーションの実行

1. 「`npm run indexDocuments`」と入力します。
2. 「`npm run build`」と入力します。
3. 「`npm run start_server`」と入力します。
4. ブラウザーで `http://localhost:8080/index.html` にアクセスします。

## USGS データの検索

USGS データ セットには、ロードアイランド州に関連するレコードが含まれています。 クリックすると **検索** [空の検索ボックスでは、表示されます、上位 50 のエントリが既定です。

検索語句を入力すると、検索エンジンに処理するものが提供されます。 地域の名前を入力してみてください。 "Roger Williams" はロードアイランド州の最初の州知事でした。 多数の公園、建造物、および学校に彼の名前が付けられています。

![][9]

次のような語句も試すことができます。

- Pawtucket
- Pembroke
- goose +cape


## 次のステップ

これは、NodeJS と USGS データセットに基づく最初の Azure Search チュートリアルです。 カスタム ソリューションで使用できる他の検索機能を紹介できるように、時間をかけてこのチュートリアルを拡張する予定です。

Azure Search についての知識が既にある場合は、このサンプルを基にして、サジェスター (先行入力またはオートコンプリート クエリ)、フィルター、ファセット ナビゲーションなどを試すことができます。 また、件数を追加してドキュメントを一括処理することで検索結果の表示を改善し、ユーザーが結果をページ移動できるようにすることもできます。

Azure Search を初めて使用する場合は、 他のチュートリアルも試して、作成できるものについての理解を深めることをお勧めします。 参照してください、 [ドキュメント ページ](http://azure.microsoft.com/documentation/services/search/) を他のリソースを検索します。 内のリンクを表示することも、 [ビデオとチュートリアルの一覧](search-video-demo-tutorial-list.md) の詳細にアクセスします。

<!--Image references-->
[1]: ./media/search-get-started-nodejs/create-search-portal-1.PNG
[2]: ./media/search-get-started-nodejs/create-search-portal-2.PNG
[3]: ./media/search-get-started-nodejs/create-search-portal-3.PNG
[5]: ./media/search-get-started-nodejs/AzSearch-NodeJS-configjs.png
[9]: ./media/search-get-started-nodejs/rogerwilliamsschool.png

