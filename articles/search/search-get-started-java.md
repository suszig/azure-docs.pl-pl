<properties
    pageTitle="Java での Azure Search の使用 | Microsoft Azure | ホスト型クラウド検索サービス"
    description="プログラミング言語として Java を使用して Azure でホスト型クラウド検索アプリケーションを作成する方法を説明します。"
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

# Java での Azure Search の使用

検索エクスペリエンスとして Azure Search を使用するカスタム Java 検索アプリケーションを作成する方法を説明します。 このチュートリアルでは、 [Azure Search サービス REST API](https://msdn.microsoft.com/library/dn798935.aspx) オブジェクトおよびこの演習で使用する操作を作成します。

このサンプルをビルドしてテストするには次のソフトウェアを使用しました。

- [Eclipse IDE for Java EE Developers](https://eclipse.org/downloads/packages/eclipse-ide-java-ee-developers/lunar)します。 EE バージョンを必ずダウンロードしてください。 検証手順の 1 つで、このエディションにしかない機能が必要です。

- [JDK 8u40](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)

- [Apache Tomcat 8.0](http://tomcat.apache.org/download-80.cgi)

このサンプルを実行する必要がありますでにサインアップできる Azure Search サービス、 [Azure Classic Portal](https://portal.azure.com)します。

> [AZURE.TIP] このチュートリアルのソース コードをダウンロード [Azure Search Java デモ](http://go.microsoft.com/fwlink/p/?LinkId=530197) Github にします。

## データについて

このサンプル アプリケーションからデータを使用して、 [United States Geological Services (USGS)](http://geonames.usgs.gov/domestic/download_data.htm), 、ロードアイランドでフィルター処理、状態のデータセットのサイズを小さきます。 このデータを使用して、病院や学校などの目立つ建物および河川、湖沼、山などの地理的特徴を返す検索アプリケーションを作成します。

このアプリケーションで、 **SearchServlet.java** プログラムがビルドされ、インデックスを使用して、ロード、 [インデクサー](https://msdn.microsoft.com/library/azure/dn798918.aspx) コンストラクト、パブリック Azure SQL Database からフィルター処理された USGS データセットを取得します。 プログラム コードでは、定義済みの資格情報とオンライン データ ソースへの接続情報が提供されます。 データ アクセスに関しては、これ以上の構成は必要ありません。

> [AZURE.NOTE] 件未満に無料の価格レベルの 10,000 ドキュメント制限には、このデータセットにフィルターを適用しました。 標準レベルを使用する場合は、この制限は適用されず、より大きなデータセットを使用するようにこのコードを変更できます。 各価格レベルの容量の詳細については、次を参照してください。 [制限および制約](search-limits-quotas-capacity.md)します。

## プログラム ファイルについて

このサンプルに関連するファイルの一覧を次に示します。

- Search.jsp: ユーザー インターフェイスを提供します
- SearchServlet.java: メソッドを提供します (MVC のコントローラーに似ています)
- SearchServiceClient.java: HTTP 要求を処理します
- SearchServiceHelper.java: 静的メソッドを提供するヘルパー クラスです
- Document.java: データ モデルを提供します
- config.properties: Search サービスの URL と API キーを設定します
- Pom.xml: Maven が依存します


## サービスの作成

1. サインイン、 [Azure クラシック ポータル](https://portal.azure.com)します。

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

サービスを作成した後は、ポータルに戻って URL および `api-key` を取得できます。 Search サービスに接続するには、URL に加えて、呼び出しを認証するための `api-key` が必要になります。

1. ジャンプバーで、次のようにクリックします。 **ホーム** し、[Search サービスを開くには、サービス ダッシュ ボード] をクリックします。

2. サービスのダッシュボードには、基本情報のタイルのほか、管理者キーにアクセスするためのキー アイコンが表示されます。

    ![][3]

3. サービスの URL と管理者キーをコピーします。 必要になります、後で追加するとき、 **config.properties** ファイルです。

## サンプル ファイルのダウンロード

1. 移動して [AzureSearchJavaDemo](http://go.microsoft.com/fwlink/p/?LinkId=530197) Github にします。

2. クリックして **ZIP のダウンロード**, をディスクに .zip ファイルを保存、およびが含まれているすべてのファイルを抽出します。 後でプロジェクトを探しやすいように、Java ワークスペースにファイルを抽出してください。

3. サンプル ファイルは読み取り専用です。 フォルダーのプロパティを右クリックし、読み取り専用の属性をオフにします。

以降のすべてのファイル変更および実行ステートメントは、このフォルダー内のファイルに対して行われます。  

## プロジェクトのインポート

1. Eclipse で、次のように選択します。 **ファイル** > **インポート** > **全般** > **Existing Projects into workspace]**します。

    ![][4]

2.  **Select root directory**, 、サンプル ファイルを含むフォルダーを参照します。 .project フォルダーが含まれるフォルダーを選択します。 プロジェクトに表示されます、 **プロジェクト** 選択項目としてリストされます。

    ![][12]

3. クリックして **完了**します。

4. 使用 **プロジェクト エクスプ ローラー** を表示して、ファイルを編集します。 まだ開いていない場合はクリックして **ウィンドウ** > **ビューを表示** > **プロジェクト エクスプ ローラー** か、ショートカットを使用して開きます。

## サービスの URL と API キーの構成

1.  **プロジェクト エクスプ ローラー**, をダブルクリックして **config.properties** サーバー名と api キーを含む構成設定を編集します。

2. サービスの URL と api キーを確認したところ、この記事で前述の手順を参照してください、 [Azure Classic Portal](https://portal.azure.com), に入力する値を取得するには、 **config.properties**します。

3.  **Config.properties**, 、Api キー] をサービスの api キーに置き換えます。 次に、サービス名 (URL http://servicename.search.windows.net の最初のコンポーネント) には、同じファイルで「サービス名」が置き換えられます。

    ![][5]

## プロジェクト、ビルド、ランタイムの環境の構成

1. Eclipse の Project Explorer でプロジェクトを右クリックして > **プロパティ** > **Project Facets**します。

2. 選択 **Dynamic Web Module**, 、**Java**, 、および **JavaScript**します。

    ![][6]

3. クリックして **適用**します。

4. 選択 **ウィンドウ** > **設定** > **Server** > **ランタイム環境** > **追加します。**します。

5. Apache を展開し、前にインストールした Apache Tomcat サーバーのバージョンを選択します。 この例では、バージョン 8 をインストールしました。

    ![][7]

6. 次のページでは、Tomcat のインストール ディレクトリを指定します。 Windows コンピューターでこの最も可能性がある C:\Program \apache Software foundation \tomcat *バージョン*です。

6. クリックして **完了**します。

7. 選択 **ウィンドウ** > **設定** > **Java** > **Installed jres]** > **追加**します。

8.  **Add JRE**, [ **標準 VM**します。

10. クリックして **次**します。

11. JRE home、] で、JRE definition] をクリックして **ディレクトリ**します。

12. 移動 **Program Files** > **Java** し、以前にインストールした JDK を選択します。 JRE として JDK を選択することが重要です。

13. [Installed jres] を選択、 **JDK**します。 設定は次のスクリーン ショットのようになります。

    ![][9]

14. 必要に応じて、選択 **ウィンドウ** > **Web ブラウザー** > **Internet Explorer** を外部のブラウザー ウィンドウで、アプリケーションを開きます。 外部のブラウザーを使用すると、Web アプリケーションのエクスペリエンスがよくなります。

    ![][8]

構成タスクが完了しました。 次に、プロジェクトをビルドして実行します。

## プロジェクトのビルド

1. プロジェクト エクスプ ローラーでプロジェクト名を右クリックし、選択 **実行** > **Maven build...]** プロジェクトを構成します。

    ![][10]

8. [Goals の編集の構成では、「clean install」を入力し、クリック **実行**します。

ステータス メッセージがコンソール ウィンドウに出力されます。 [BUILD SUCCESS] と表示されれば、プロジェクトはエラーなしでビルドされています。

## アプリの実行

この最後の手順では、ローカル サーバーのランタイム環境でアプリケーションを実行します。

まだ Eclipse でサーバーのランタイム環境を指定していない場合は、最初に行う必要があります。

1. プロジェクト エクスプ ローラーで、 **WebContent**します。

5. 右クリック **Search.jsp** > **として実行** > **サーバー上で実行**します。 Apache Tomcat サーバーを選択し、クリックして **実行**します。

> [AZURE.TIP] 既定以外のワークスペースを使用して、プロジェクトを格納した場合は、変更する必要があります **実行構成** 、サーバー起動エラーを避けるためにプロジェクトの場所を指すようです。 プロジェクト エクスプ ローラー [ **Search.jsp** > **として実行** > **Run Configurations**します。 Apache Tomcat サーバーを選択します。 クリックして **引数**します。 クリックして **ワークスペース** または **ファイル システム** プロジェクトを含むフォルダーを設定します。

アプリケーションを実行すると、ブラウザー ウィンドウが開き、語句を入力するための検索ボックスが表示されます。

クリックする前に約 1 分間待機 **検索** サービスが、インデックスを作成して読み込めるようにします。 HTTP 404 エラーが発生する場合は、もう少し長く待ってから再び試すだけで十分です。

## USGS データの検索

USGS データ セットには、ロードアイランド州に関連するレコードが含まれています。 クリックすると **検索** [空の検索ボックスでは、表示されます、上位 50 のエントリが既定です。

検索語句を入力すると、検索エンジンに処理するものが提供されます。 地域の名前を入力してみてください。 "Roger Williams" はロードアイランド州の最初の州知事でした。 多数の公園、建造物、および学校に彼の名前が付けられています。

![][11]

次のような語句も試すことができます。

- Pawtucket
- Pembroke
- goose +cape

## 次のステップ

これは、Java と USGS データセットに基づく最初の Azure Search チュートリアルです。 カスタム ソリューションで使用できる他の検索機能を紹介できるように、時間をかけてこのチュートリアルを拡張する予定です。

Azure Search の知識を既にがあることを使用する場合このサンプルの出発点としてさらに実験を行うには、おそらくを補足する、 [検索ページ](search-pagination.md), 、または実装する [ファセット ナビゲーション](search-faceted-navigation.md)します。 また、件数を追加してドキュメントを一括処理することで検索結果の表示を改善し、ユーザーが結果をページ移動できるようにすることもできます。

Azure Search を初めて使用する場合は、 他のチュートリアルも試して、作成できるものについての理解を深めることをお勧めします。 参照してください、 [ドキュメント ページ](http://azure.microsoft.com/documentation/services/search/) を他のリソースを検索します。 内のリンクを表示することも、 [ビデオとチュートリアルの一覧](search-video-demo-tutorial-list.md) の詳細にアクセスします。

<!--Image references-->
[1]: ./media/search-get-started-java/create-search-portal-1.PNG
[2]: ./media/search-get-started-java/create-search-portal-21.PNG
[3]: ./media/search-get-started-java/create-search-portal-31.PNG
[4]: ./media/search-get-started-java/AzSearch-Java-Import1.PNG
[5]: ./media/search-get-started-java/AzSearch-Java-config1.PNG
[6]: ./media/search-get-started-java/AzSearch-Java-ProjectFacets1.PNG
[7]: ./media/search-get-started-java/AzSearch-Java-runtime1.PNG
[8]: ./media/search-get-started-java/AzSearch-Java-Browser1.PNG
[9]: ./media/search-get-started-java/AzSearch-Java-JREPref1.PNG
[10]: ./media/search-get-started-java/AzSearch-Java-BuildProject1.PNG
[11]: ./media/search-get-started-java/rogerwilliamsschool1.PNG
[12]: ./media/search-get-started-java/AzSearch-Java-SelectProject.png

