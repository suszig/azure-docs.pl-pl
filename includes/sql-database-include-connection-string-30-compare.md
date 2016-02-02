


### 接続文字列の比較

次の表では、C# プログラムでオンプレミスの SQL Server に接続するときに必要な接続文字列と、クラウドの Azure SQL Database に接続するときに必要な接続文字列を比較しています。 相違点を太字で示しています。


| 接続文字列を<br/>Azure SQL Database| 接続文字列を<br/>Microsoft SQL Server|
| :-- | :-- |
| Server =**tcp:**{your_serverName_here}**. database.windows.net,1433**;<br/>ユーザー ID = {your_loginName_here}**@{your_serverName_here}**;<br/>パスワード {your_password_here} を =;<br/>**データベース {your_databaseName_here} を =;**<br/>**接続タイムアウト = 30**;<br/>**Encrypt=True**;<br/>**TrustServerCertificate = False**です。| サーバー {your_serverName_here} を =;<br/>ユーザー ID {your_loginName_here} を =;<br/>パスワード {your_password_here} を =;|


**Database=** は、SQL Server の場合は省略可能ですが、SQL Database では必須です。


[.NET ADO SqlConnectionStringBuilder プロパティ](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder_properties.aspx) -すべてのパラメーターの詳細について説明します。








