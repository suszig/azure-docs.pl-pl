>[AZURE.NOTE] SharePoint 用 StorSimple アダプターの RBS 構成を変更する際は、Domain Admins グループに属しているユーザー アカウントでログオンする必要があります。 さらに、サーバーの全体管理と同じホストで実行されているブラウザーから構成ページにアクセスする必要があります。

#### RBS を構成するには

1. [SharePoint サーバーの全体管理] ページを開き、**[システム設定]** を参照します。

2. **[Azure StorSimple]** セクションで、**[StorSimple アダプターの構成]** をクリックします。

    ![Configure the StorSimple Adapter](./media/storsimple-sharepoint-adapter-configure-rbs/HCS_SSASP_ConfigRBS1-include.png)

3. **[StorSimple アダプターの構成]** ページで、次の手順を実行します。

 1. **[パスの編集を有効にする]** チェック ボックスがオンになっていることを確認します。

 2. ボックスに、BLOB ストアの汎用名前付け規則 (UNC) パスを入力します。

       >[AZURE.NOTE] The BLOB store volume must be hosted on an iSCSI volume configured on the StorSimple device.

 3. リモート記憶域用に構成する各コンテンツ データベースの下の **[有効]** をクリックします。

    >[AZURE.NOTE] The BLOB store must be shared and reachable by all web front-end (WFE) servers, and the user account that is configured for the SharePoint server farm must have access to the share.
    
    ![Enable the RBS provider](./media/storsimple-sharepoint-adapter-configure-rbs/HCS_SSASP_ConfigRBS2-include.png)
    
     When you enable or disable RBS, you will also see the following message.
    
    ![Configure StorSimple Adapter Enable Disable](./media/storsimple-sharepoint-adapter-configure-rbs/HCS_ConfigureStorSimpleAdapterEnableDisableMessage-include.png)

 4. **[更新]** をクリックして構成を適用します。 [**更新**] をクリックすると、すべての WFE サーバー上で RBS 構成状態が更新され、ファーム全体で RBS が有効になります。 次のメッセージが表示されます。

     ![Adapter configuration message](./media/storsimple-sharepoint-adapter-configure-rbs/HCS_SSASP_ConfigRBS3-include.png)
    
     >[AZURE.NOTE] If you are configuring RBS for a SharePoint farm with a very large number of databases (greater than 200), the SharePoint Central Administration web page might time out. If that occurs, refresh the page. This does not affect the configuration process.


4. 構成を確認します。

    1. SharePoint サーバーの全体管理 Web サイトにログオンし、**[StorSimple アダプターの構成]** ページに移動します。

    2. 構成の詳細を表示して、入力した設定に一致しているかどうかを確認します。

5. RBS が正しく動作することを確認します。

    1. ドキュメントを SharePoint にアップロードします。

    2. 構成した UNC パスを参照します。 RBS のディレクトリ構造が作成されていること、およびアップロードしたオブジェクトがそこに含まれていることを確認します。

6. (省略可能)Microsoft RBS を使用する `Migrate()` を StorSimple デバイスに既存の BLOB コンテンツを移行するように SharePoint に含まれている PowerShell コマンドレット。 詳細については、次を参照してください。 [[6] の SharePoint 2013 で RBS の内外にコンテンツを移行][6] または [RBS (SharePoint Foundation 2010) ][7]します。

7. (省略可能) テスト インストールで、次のように、BLOB がコンテンツ データベースから移動されたことを確認できます。

    1. SQL Management Studio を起動します。

    2. 次のように、ListBlobsInDB_2010.sql クエリまたは ListBlobsInDB_2013.sql クエリを実行します。

     **ListBlobsInDB_2013.sql**

         WSS_Content を使用します。
         GO

         DocStreams.DocId を選択します。
                LeafName AS 名
                コンテンツは、
                AllDocs.Size AS OrigSizeOfContent
                LEN (CAST (コンテンツ AS VARBINARY(MAX))) AS SizeOfContentInDB、
                DocStreams.RbsId、
                TimeLastModified

         DocStreams から
              内部結合 AllDocs ON DocStreams.DocId AllDocs.Id =
         TimeLastModified DESC で並べ替えます
         GO

     **ListBlobsInDB_2010.sql**

         WSS_Content を使用します。
         GO

         AllDocStreams.Id を選択します。
                LeafName AS 名
                コンテンツは、
                AllDocs.Size AS OrigSizeOfContent
                LEN (CAST (コンテンツ AS VARBINARY(MAX))) AS SizeOfContentInDB、
                RbsId、
                TimeLastModified
         AllDocStreams から
              内部結合 AllDocs ON AllDocStreams.Id AllDocs.Id =
         TimeLastModified DESC で並べ替えます
         GO

     RBS が正しく構成されている場合は、アップロードされて RBS によって正常に外部化されたすべてのオブジェクトについて、SizeOfContentInDB 列に NULL 値が表示されます。

8. (省略可能) RBS を構成し、すべての BLOB コンテンツを StorSimple デバイスに移動した後、コンテンツ データベースをデバイスに移動することができます。 コンテンツ データベースを移動する場合は、デバイスのコンテンツ データベース ストレージをプライマリ ボリュームとして構成することをお勧めします。 次に、SQL Server の移行に関する確立されたベスト プラクティスを使用して、コンテンツ データベースを StorSimple デバイスに移行します。
     >[AZURE.NOTE] デバイスへのコンテンツ データベースの移動は、StorSimple 8000 シリーズ デバイスでのみサポートされています (5000 シリーズまたは 7000 シリーズではサポートされていません)。

     BLOB とコンテンツ データベースを StorSimple デバイス上の別々のボリュームに保存する場合は、それらのボリュームを同じボリューム コンテナー内に構成することをお勧めします。 これにより、これらのボリュームは同時にバックアップされるようになります。

       >[AZURE.WARNING] If you have not enabled RBS, we do not recommend moving the content database to the StorSimple device. This is an untested configuration.

9. 次の手順に進みます。 [ガベージ コレクションを構成する](#configure-garbage-collection)します。


[6]: https://technet.microsoft.com/library/ff628254(v=office.15).aspx 
[7]: https://technet.microsoft.com/library/ff628255(v=office.14).aspx 

