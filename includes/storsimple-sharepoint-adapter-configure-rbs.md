<!--author=SharS last changed: 9/17/15-->

>[AZURE.NOTE] SharePoint の RBS 構成用 StorSimple アダプターを変更するときは、Domain Admins グループに属しているユーザー アカウントでログオンする必要があります。 さらに、サーバーの全体管理と同じホストで実行されているブラウザーから構成ページにアクセスする必要があります。

#### RBS を構成するには

1. SharePoint サーバーの全体管理ページを開きを参照 **システム設定**します。 

2.  **Azure StorSimple** ] をクリックして **StorSimple アダプターの構成**します。

    ![Configure the StorSimple Adapter](./media/storsimple-sharepoint-adapter-configure-rbs/HCS_SSASP_ConfigRBS1-include.png) 

3.  **StorSimple アダプターの構成** ページ。

    1. 確認して、 **パスの編集を有効にする** ] チェック ボックスをオンします。

    2. ボックスに、BLOB ストアの汎用名前付け規則 (UNC) パスを入力します。

          >[AZURE.NOTE] BLOB ストア ボリュームは、StorSimple デバイスで構成されている iSCSI ボリュームでホストされている必要があります。

    3. をクリックして、 **を有効にする** 各リモート記憶域を構成するコンテンツ データベースの下のボタンをクリックします。

          >[AZURE.NOTE] BLOB ストアがすべての web フロント エンド (WFE) サーバーで共有しており、到達をする必要があり、SharePoint サーバー ファーム用に構成されているユーザー アカウントが共有へのアクセスを持つ必要があります。

          ![Enable the RBS provider](./media/storsimple-sharepoint-adapter-configure-rbs/HCS_SSASP_ConfigRBS2-include.png)

           When you enable or disable RBS, you will also see the following message.

          ![Configure StorSimple Adapter Enable Disable](./media/storsimple-sharepoint-adapter-configure-rbs/HCS_ConfigureStorSimpleAdapterEnableDisableMessage-include.png)

    4. クリックして、 **更新** 構成を適用する] ボタンをクリックします。 クリックすると、 **更新** ] ボタンはすべての WFE サーバーで RBS 構成状態に更新し、ファーム全体に RBS が有効になります。 次のメッセージが表示されます。

           ![Adapter configuration message](./media/storsimple-sharepoint-adapter-configure-rbs/HCS_SSASP_ConfigRBS3-include.png)

           >[AZURE.NOTE] If you are configuring RBS for a SharePoint farm with a very large number of databases (greater than 200), the SharePoint Central Administration web page might time out. If that occurs, refresh the page. This does not affect the configuration process.
 
4. 構成を確認します。

    1. SharePoint サーバーの全体管理 web サイトにログオンを見つけて、 **StorSimple アダプターの構成** ページです。

    2. 構成の詳細を表示して、入力した設定に一致しているかどうかを確認します。 

5. RBS が正しく動作することを確認します。

    1. ドキュメントを SharePoint にアップロードします。 

    2. 構成した UNC パスを参照します。 RBS のディレクトリ構造が作成されていること、およびアップロードしたオブジェクトがそこに含まれていることを確認します。

6. (省略可能) SharePoint に付属する Microsoft RBS `Migrate()` PowerShell コマンドレットを使用して、既存の BLOB コンテンツを StorSimple デバイスに移行できます。 詳細については、次を参照してください。 [SharePoint 2013 で RBS の内外にコンテンツを移行][6] または [RBS (SharePoint Foundation 2010) の内外にコンテンツを移行][7]します。

7. (省略可能) テスト インストールで、次のように、BLOB がコンテンツ データベースから移動されたことを確認できます。 

    1. SQL Management Studio を起動します。

    2. 次のように、ListBlobsInDB_2010.sql クエリまたは ListBlobsInDB_2013.sql クエリを実行します。

     **ListBlobsInDB_2013.sql**

         USE WSS_Content
         GO
    
         SELECT DocStreams.DocId,
                LeafName AS Name,
                Content,
                AllDocs.Size AS OrigSizeOfContent,
                LEN(CAST(Content AS VARBINARY(MAX))) AS SizeOfContentInDB,
                DocStreams.RbsId,
                TimeLastModified
    
         FROM DocStreams
              INNER JOIN AllDocs ON DocStreams.DocId = AllDocs.Id
         ORDER BY TimeLastModified DESC
         GO

     **ListBlobsInDB_2010.sql**

         USE WSS_Content
         GO

         SELECT AllDocStreams.Id,
                LeafName AS Name,
                Content,
                AllDocs.Size AS OrigSizeOfContent,
                LEN(CAST(Content AS VARBINARY(MAX))) AS SizeOfContentInDB,
                RbsId,
                TimeLastModified
         FROM AllDocStreams
              INNER JOIN AllDocs ON AllDocStreams.Id = AllDocs.Id
         ORDER BY TimeLastModified DESC
         GO

     RBS が正しく構成されている場合は、アップロードされて RBS によって正常に外部化されたすべてのオブジェクトについて、SizeOfContentInDB 列に NULL 値が表示されます。

8. (省略可能) RBS を構成し、すべての BLOB コンテンツを StorSimple デバイスに移動した後、コンテンツ データベースをデバイスに移動することができます。 コンテンツ データベースを移動する場合は、デバイスのコンテンツ データベース ストレージをプライマリ ボリュームとして構成することをお勧めします。 次に、SQL Server の移行に関する確立されたベスト プラクティスを使用して、コンテンツ データベースを StorSimple デバイスに移行します。 

     >[AZURE.NOTE] デバイスへのコンテンツ データベースの移動は、(5000 や 7000 シリーズについてはサポートされません)、StorSimple 8000 シリーズでのみサポートされます。
 
     BLOB とコンテンツ データベースを StorSimple デバイス上の別々のボリュームに保存する場合は、それらのボリュームを同じボリューム コンテナー内に構成することをお勧めします。 これにより、これらのボリュームは同時にバックアップされるようになります。

       >[AZURE.WARNING] If you have not enabled RBS, we do not recommend moving the content database to the StorSimple device. This is an untested configuration.
 
9. 次の手順に進みます。 [ガベージ コレクションを構成する](#configure-garbage-collection)です。

[6]: https://technet.microsoft.com/library/ff628254(v=office.15).aspx
[7]: https://technet.microsoft.com/library/ff628255(v=office.14).aspx

