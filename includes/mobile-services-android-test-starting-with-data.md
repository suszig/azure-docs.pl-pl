バック エンド ストレージの Mobile Services を使用するようにアプリケーションを更新した後は、Android エミュレーターまたは Android フォンを使用して、Mobile Services に対してアプリケーションをテストします。

1.  **実行** ] メニューのをクリックして **実行** プロジェクトを開始します。

    これにより、モバイル サービスから項目を取得するクエリを、クライアント ライブラリを使用して送信するアプリケーションが Android SDK でビルドされ、実行されます。

5. 前回と同様に、意味のあるテキストを入力し、クリックして **追加**します。

    これにより、新しい項目が挿入としてモバイル サービスに送信されます。

    アプリケーションを再起動して、変更内容が Azure のデータベースに保持されたことを確認できます。 Azure クラシック ポータルを使用してデータベースを確認することもできます。 次の 2 つのステップでは、データベースの変更を表示します。


4.  [Azure クラシック ポータル](https://manage.windowsazure.com/), 、モバイル サービスに関連付けられているデータベースの管理] をクリックします。

    ![](./media/mobile-services-dotnet-backend-windows-store-dotnet-get-started-data/manage-sql-azure-database.png)

5. Azure クラシック ポータルで、クエリを実行して Windows ストア アプリによって加えられた変更を表示します。 クエリは次のようになりますが、`todolist` の代わりにデータベースの名前を使用します。

        SELECT * FROM [todolist].[todoitems]

    ![](./media/mobile-services-dotnet-backend-windows-store-dotnet-get-started-data/sql-azure-query.png)

これは、時点で、 **データを使ってみる** Android 向けチュートリアルです。
