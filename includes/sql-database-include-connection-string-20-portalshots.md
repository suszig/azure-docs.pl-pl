


### Azure ポータルから接続文字列を取得する

使用して、 [Azure プレビュー ポータル](http://portal.azure.com/) クライアント プログラムは、Azure SQL データベースとやり取りするために必要な接続文字列を取得します。


1. **[参照]**、**[SQL データベース]** の順にクリックします。

2. **[SQL データベース]** ブレードの左上近辺にあるフィルター テキスト ボックスにデータベースの名前を入力します。

3. データベースの行をクリックします。

4. データベースのブレードが表示されたら、visual 利便性をクリックし参照やデータベースのフィルターを使用するブレードを折りたたむに最小化の標準的なコントロールです。

    ![フィルター処理を行ってデータベースを分離する][10-filterdatabase]

5. データベース用のブレードで、**[データベース接続文字列の表示]** をクリックします。

6. ADO.NET 接続ライブラリを使用する場合は、**ADO** というラベルが付いた文字列をコピーします。

    ![データベース用の ADO 接続文字列をコピーする][20-copyadoconnectionstring]

7. 特定の形式の接続文字列情報を、クライアント プログラム コードに貼り付けます。



詳細についてを参照してください:<br/>[接続文字列と構成ファイル](http://msdn.microsoft.com/library/ms254494.aspx)します。










[10-filterdatabase]: ./media/sql-database-include-connection-string-20-portalshots/connqry-connstr-a.png 
[20-copyadoconnectionstring]: ./media/sql-database-include-connection-string-20-portalshots/connqry-connstr-b.png 

