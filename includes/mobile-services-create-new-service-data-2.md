アプリケーション データを新しいモバイル サービスに格納できるようにするには、最初に新しいテーブルを関連付けられた SQL データベース インスタンスに作成する必要があります。

1.  [Azure クラシック ポータル](https://manage.windowsazure.com/), 、] をクリックして **Mobile Services**, 、作成したモバイル サービスをクリックします。

2. クリックして、 **データ** ] タブの [ **[作成**します。

    これを表示、 **新しいテーブルを作成する** ダイアログ。

3.  **テーブル名** 型 _TodoItem_, 、チェック ボタンをクリックします。 これが作成された新しいストレージ テーブル **TodoItem** 既定のアクセス許可を設定します。 これは、アプリケーションと共に配布されるアプリケーション キーを使用してだれもがテーブル内のデータにアクセスし、変更を加えることができることを表します。 

    >[AZURE.NOTE] モバイル サービスのクイック スタートでも同じテーブル名を使用するとします。 ただし、それぞれのテーブルは、特定のモバイル サービスに固有のスキーマ内で作成されています。 これには、複数のモバイル サービスで同じデータベースを使用した場合にデータの競合を回避する目的があります。

4. [新規] をクリックして **TodoItem** テーブルし、データ行がないことを確認します。

5. クリックして、 **列** ] タブをクリックします。 次の既定の列が自動的に作成されていることを確認します。 
    
    <table border="1" cellpadding="10">
    <tr>
    <th>列名</th>
    <th>種類</th>
    <th>インデックス</th>
    </tr>
    <tr>
    <td>id</td>
    <td>string</td>
    <td>インデックス設定済み</td>
    </tr>
    <tr>
    <td>__createdAt</td>
    <td>date</td>
    <td>インデックス設定済み</td>
    </tr>
    <tr>
    <td>__updatedAt</td>
    <td>date</td>
    <td><font color="transparent">-</font></td>
    </tr>
    <tr>
    <td>__version</td>
    <td>timestamp (MSSQL)</td>
    <td><font color="transparent">-</font></td>
    </tr>   
    </table>    
        

    This is the minimum requirement for a table in Mobile Services. 

    > [AZURE.NOTE] When dynamic schema is enabled on your mobile service, new columns are created automatically when JSON objects are sent to the mobile service by an insert or update operation.

これで、新しいモバイル サービスをアプリケーションのデータ ストレージとして使用する準備が整いました。

