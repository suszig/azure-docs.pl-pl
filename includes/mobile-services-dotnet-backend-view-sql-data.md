
このチュートリアルの最後の手順 (省略可能) は、モバイル サービスと関連付けられた SQL Database で格納されているデータを確認することです。 

1.  [Azure クラシック ポータル](https://manage.windowsazure.com/), 、モバイル サービスに関連付けられているデータベースの管理] をクリックします。
 
    ![サインインして SQL Database を管理する](./media/mobile-services-dotnet-backend-view-sql-data/manage-sql-azure-database.png)

2. ポータルで、クエリを実行して Windows ストア アプリによって加えられた変更を表示します。 クエリは次のクエリのようになりますの代わりに、データベース名を使用 <code>todolist</code>.</p>

        SELECT * FROM [todolist].[todoitems]

    ![SQL Database に格納された項目をクエリする](./media/mobile-services-dotnet-backend-view-sql-data/sql-azure-query.png)

    テーブルに Id、__createdAt、__updatedAt、__version の各列があることに注意してください。 これらの列をサポートしてオフラインのデータを同期させで実装された、 [EntityData](http://msdn.microsoft.com/library/microsoft.windowsazure.mobile.service.entitydata.aspx) 基本クラスです。 詳細については「オフライン データ同期の使用」を参照してください。
