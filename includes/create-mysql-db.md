このガイドが使用する方法を説明 [ClearDB] から MySQL データベースを作成する、 [Azure Store] を作成するときに、リンク済みリソースとして MySQL データベースを作成する方法と、 [Azure の Web サイト][waws] します。 [ClearDB] フォールト トレランスに優れたサービスとしてのデータベース プロバイダーを実行し、Azure データ センターで MySQL データベースを管理し、任意のアプリケーションからそれらに接続できるようにします。  

> [AZURE.NOTE] Web サイト作成プロセスの一部としての MySQL データベースを作成するときのみ、無料データベースを作成することができます。 Azure ストアから MySQL データベースを作成すると、無料データベースを作成するか、有料オプションから選択することができます。

## 方法: Azure ストアからの MySQL データベースの作成

MySQL データベースを作成する、 [Azure Store], 、次の操作を行います。

1. ログイン、 [Azure 管理ポータル][portal]します。
2. クリックして **+ 新規** 、ページの下部にあるを選択し、 **MARKETPLACE**します。

    ![ストアからアドオンを選択](./media/create-mysql-db/select-store.png)

3. 選択 **ClearDB MySQL Database**, 、フレームの下部にある矢印をクリックします。

    ![ClearDB MySQL データベースの作成](./media/create-mysql-db/select-cleardb-mysql.png)

4. プランを選択してデータベース名を入力し、リージョンを選択してフレームの下部にある矢印をクリックします。

    ![ストアから MySQL データベースを購入](./media/create-mysql-db/purchase-mysql.png)

5. チェックマークをクリックして購入を完了します。

    ![購入を確認して完了](./media/create-mysql-db/complete-mysql-purchase.png)

6. データベースが作成されたら後からそれを管理できます、 **アドオン** 管理ポータルでタブをクリックします。

    ![Azure ポータルで MySQL データベースを管理](./media/create-mysql-db/manage-mysql-add-on.png)

7. をクリックして、データベース接続情報を取得できます **接続情報]** (上記参照)、ページの下部にあります。

    ![MySql の接続情報](./media/create-mysql-db/mysql-conn-info.png) 


## 方法: Azure Web サイトのリンク済みリソースとしての MySQL データベースの作成

リンク済みリソースとして作成するときに MySQL データベースを作成するのには [Azure の Web サイト][waws], を次の操作します。

1. ログイン、 [Azure 管理ポータル][portal]します。
2. をクリックして **+ 新規** 、ページの下部にあるを選択し、 **コンピューティング**, 、**web サイト**, 、および **データベースとともに作成**します。

    ![データベースを使用する Web サイトの作成](./media/create-mysql-db/custom_create.png)

3. 提供、 **URL** 、web サイトを選択、 **地域** 、サイトの選択 **新しい MySQL データベースを作成** から、 **データベース** ドロップダウンです。 オプションで、接続文字列の既定の名前を置き換えることができます。 ページの下部にある矢印をクリックします。

    ![Web サイトの詳細の指定](./media/create-mysql-db/provide-website-details.png) 

4. データベースの指定 **名**, を選択、 **地域** (web サイトのリージョンと同じにこれでする必要があります)、データベースの ClearDB の法律条項に同意し、フレームの下部にあるチェック マークをクリックします。

    ![MySQL の詳細の指定](./media/create-mysql-db/provide-mysql-details.png)

5. Web サイトが作成されたら、サイトの名前をクリックしてサイトのダッシュボードに移動します。

    ![Web サイトのダッシュボードに移動](./media/create-mysql-db/go-to-website-dashboard.png)

6. をクリックして **構成**します。

    ![[構成] タブに移動](./media/create-mysql-db/go-to-configure-tab.png)

7. 下へスクロールして、 **接続文字列** セクションを **[接続文字列の**です。 

    ![接続文字列の表示](./media/create-mysql-db/show-conn-string.png)

8. アプリケーションで使用する接続文字列をコピーします。

    ![表示された接続文字列](./media/create-mysql-db/shown-conn-string.png)

> [AZURE.NOTE] 接続文字列には、web サイト アプリケーションから接続文字列名によりアクセスします。 .NET アプリケーションで接続文字列は、 **connectionStrings** オブジェクトです。 他のプログラミング言語では、接続文字列には環境変数としてアクセスできます。 詳細については、次を参照してください。 [Web サイトを構成する方法][configure]します。

[ClearDB]: http://www.cleardb.com/
[waws]: /documentation/services/web-sites/
[Azure Store]: ../articles/store.md
[portal]: http://manage.windowsazure.com
[configure]: ../article/app-service-web/web-sites-configure.md

