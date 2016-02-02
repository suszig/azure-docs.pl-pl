

### Azure ポータルから接続文字列を取得する

使用して、 [Azure プレビュー ポータル](http://portal.azure.com/) クライアント プログラムは、Azure SQL データベースとやり取りするために必要な接続文字列を取得します。


1. **[参照]**、**[SQL データベース]** の順にクリックします。

    ![Select SQL][1-select-sql]

2. **[SQL データベース]** ブレードの左上近辺にあるフィルター テキスト ボックスにデータベースの名前を入力します。

    ![データベースを選択して][2-select-database]]

3. データベースの行をクリックします。

4. データベースのブレードが表示されたら、visual 利便性をクリックし参照やデータベースのフィルターを使用するブレードを折りたたむに最小化の標準的なコントロールです。

5. データベース用のブレードで、**[データベース接続文字列の表示]** をクリックします。

6. PHP 接続ライブラリを使用する場合は、**PHP** というラベルが付いた文字列をコピーします。

    ![データベース用の PHP 接続文字列をコピーする][3-get-connection-string]

7. 接続文字列情報を、クライアント プログラム コードに貼り付けます。 {your_password_here} を実際のパスワードに置き換える必要があります。












[1-select-sql]: ./media/sql-database-include-connection-string-20-portalshots/connection-string-select-sql.png 
[2-select-database]: ./media/sql-database-include-connection-string-20-portalshots/connection-string-select-database.PNG 
[3-get-connection-string]: ./media/sql-database-include-connection-string-20-portalshots/connection-string-php.PNG 

