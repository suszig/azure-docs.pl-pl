このガイドでは説明を作成するときに、リンク済みリソースとして [Azure Store]、および MySQL データベースを作成する方法から MySQL データベースを作成する [ClearDB] を使用する方法、 [Azure の Web サイト ][waws] します。 [ClearDB] を実行し、Azure データ センターで MySQL データベースを管理し、任意のアプリケーションからそれらに接続できるようにするフォールト トレランスに優れたサービスとしてのデータベース プロバイダーです。
> [AZURE.NOTE] MySQL データベースを Web サイト作成プロセスの一部として作成するときは、無料データベースを作成するだけでかまいません。 Azure ストアから MySQL データベースを作成すると、無料データベースを作成するか、有料オプションから選択することができます。

## 方法: Azure ストアからの MySQL データベースの作成

[Azure Store] から MySQL データベースを作成するには、次の操作を行います。

1. ログイン、 [Azure 管理ポータルの ][portal]します。
2. ページの下部にある **[+新規]** をクリックし、**[MARKETPLACE]** を選択します。

    ![ストアからアドオンを選択](./media/create-mysql-db/select-store.png)

3. **[ClearDB MySQL データベース]** を選択し、フレームの下部にある矢印をクリックします。

    ![ClearDB MySQL データベースの作成](./media/create-mysql-db/select-cleardb-mysql.png)

4. プランを選択してデータベース名を入力し、リージョンを選択してフレームの下部にある矢印をクリックします。

    ![ストアから MySQL データベースを購入](./media/create-mysql-db/purchase-mysql.png)

5. チェックマークをクリックして購入を完了します。

    ![購入を確認して完了](./media/create-mysql-db/complete-mysql-purchase.png)

6. データベースが作成されたら、管理ポータルの **[アドオン]** タブから管理できます。

    ![Azure ポータルで MySQL データベースを管理](./media/create-mysql-db/manage-mysql-add-on.png)

7. ページの下部にある **[接続文字列]** をクリックして、データベース接続情報を取得できます (上図を参照)。

    ![MySql の接続情報](./media/create-mysql-db/mysql-conn-info.png)


## 方法: Azure Web サイトのリンク済みリソースとしての MySQL データベースの作成

リンク済みリソースとして作成するときに MySQL データベースを作成するのには [Azure の Web サイト ][waws], 、次の操作します。

1. ログイン、 [Azure 管理ポータルの ][portal]します。
2. ページの下部にある **[新規]** をクリックし、**[コンピューティング]**、**[Web サイト]**、**[データベースと共に作成]** の順に選択します。

    ![データベースを使用する Web サイトの作成](./media/create-mysql-db/custom_create.png)

3. Web サイトの **URL** を指定して、サイトの**リージョン**を選択し、**[データベース]** ボックスから **[新しい MySQL データベースを作成する]** を選択します。 オプションで、接続文字列の既定の名前を置き換えることができます。 ページの下部にある矢印をクリックします。

    ![Web サイトの詳細の指定](./media/create-mysql-db/provide-website-details.png)

4. データベースの**名前**を指定してデータベースの**リージョン**を選択し (これは Web サイトのリージョンと同じにしてください)、ClearDB の法律条項に同意して、フレームの下部にあるチェックマークをクリックします。

    ![MySQL の詳細の指定](./media/create-mysql-db/provide-mysql-details.png)

5. Web サイトが作成されたら、サイトの名前をクリックしてサイトのダッシュボードに移動します。

    ![Web サイトのダッシュボードに移動](./media/create-mysql-db/go-to-website-dashboard.png)

6. **[構成]** をクリックします。

    ![[構成] タブに移動](./media/create-mysql-db/go-to-configure-tab.png)

7. **[接続文字列]** セクションまで下方向へスクロールし、**[接続文字列の表示]** をクリックします。

    ![接続文字列の表示](./media/create-mysql-db/show-conn-string.png)

8. アプリケーションで使用する接続文字列をコピーします。

    ![表示された接続文字列](./media/create-mysql-db/shown-conn-string.png)

> [AZURE.NOTE] 接続文字列には、Web サイト アプリケーションから接続文字列名によりアクセスできます。 .NET アプリケーションでは、接続文字列は **connectionStrings** オブジェクトにあります。 他のプログラミング言語では、接続文字列には環境変数としてアクセスできます。 詳細については、次を参照してください。 [Web サイトを構成する方法 ][configure]します。


[cleardb]: http://www.cleardb.com/ 
[waws]: /documentation/services/web-sites/ 
[azure store]: ../articles/store.md 
[portal]: http://manage.windowsazure.com 
[configure]: ../article/app-service-web/web-sites-configure.md 

