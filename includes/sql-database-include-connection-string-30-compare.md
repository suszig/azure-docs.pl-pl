
<!--
includes/sql-database-include-connection-string-30-compare.md

Latest Freshness check:  2015-09-03 , GeneMi.

## Connection string
-->


### 接続文字列の比較


次の表では、C# プログラムでオンプレミスの SQL Server に接続するときに必要な接続文字列と、クラウドの Azure SQL Database に接続するときに必要な接続文字列を比較しています。 相違点を太字で示しています。


| 接続文字列<br/>Azure SQL データベース | 接続文字列<br/>Microsoft SQL Server |
| :-- | :-- |
| Server =**tcp:**{your_serverName_here}**. database.windows.net,1433**です。<br/>ユーザー ID = {your_loginName_here}**@{your_serverName_here}**です。<br/>Password={your_password_here}; (パスワード)<br/>**データベース {your_databaseName_here} を =;**<br/>**接続タイムアウト = 30**です。<br/>**暗号化 = True**です。<br/>**TrustServerCertificate = False**です。 | サーバー {your_serverName_here} を =;<br/>ユーザー ID {your_loginName_here} を =;<br/>Password={your_password_here}; (パスワード) |


 **データベース =** 、SQL Server ではオプションが SQL データベースでは必須です。


[.NET ADO SqlConnectionStringBuilder プロパティ](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder_properties.aspx) -すべてのパラメーターの詳細について説明します。


<!--
These three includes/ files are a sequenced set, but you can pick and choose:

includes/sql-database-include-connection-string-20-portalshots.md
includes/sql-database-include-connection-string-30-compare.md
includes/sql-database-include-connection-string-40-config.md
-->


