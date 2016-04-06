<properties
    pageTitle="C# クエリによる SQL Database への接続 | Microsoft Azure"
    description="SQL データベースへのクエリおよび接続を実行するプログラムを C# で作成します。 IP アドレス、接続文字列、セキュリティで保護されたログイン、および無料版の Visual Studio に関する情報。"
    services="sql-database"
    keywords="c# database query, c# query, connect to database"
    documentationCenter=""
    authors="MightyPen"
    manager="jeffreyg"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="get-started-article"
    ms.date="10/09/2015"
    ms.author="genemi"/>


# SQL Database へのクエリおよび接続を実行するプログラムを C&#x23; で作成する

> [AZURE.SELECTOR]
- [C#](sql-database-connect-query.md)
- [SSMS](sql-database-connect-query-ssms.md)
- [Excel](sql-database-connect-excel.md)

クラウド内の Azure SQL データベースへのクエリおよび接続を実行するプログラムを C# で作成する方法を説明します。

この記事では、Azure SQL Database、C#、および ADO.NET を初めて使用するユーザー向けに、すべての手順を説明します。 Microsoft SQL Server と C# の使用経験が豊富なユーザーなら、一部の手順をスキップして SQL Database に固有の手順のみに集中することができます。


## 前提条件


C# クエリ コード サンプルを実行するには、以下のものが必要です。


- Azure アカウントとサブスクリプション。 サインアップできる、 [無料評価版](http://azure.microsoft.com/pricing/free-trial/)します。


-  **AdventureWorksLT** Azure SQL Database サービスでデータベースをデモンストレーションします。
 - [デモのデータベースを作成する](sql-database-get-started.md) (分) です。


- Visual Studio 2013 Update 4 以降。 Microsoft は、Visual Studio コミュニティで *無料*します。
 - [Visual Studio Community のダウンロード](http://www.visualstudio.com/products/visual-studio-community-vs)
 - [その他の無料版 Visual Studio の選択肢](http://www.visualstudio.com/products/free-developer-offers-vs.aspx)
 - させたり、 [ステップ](#InstallVSForFree) このトピックで後で説明する方法、 [Azure ポータル](http://portal.azure.com/) を Visual Studio のインストールについて説明します。


<a name="InstallVSForFree" id="InstallVSForFree"></a>

&nbsp;

## 手順 1. 無料版の Visual Studio Community をインストールする


Visual Studio のインストールが必要な場合、以下の方法でインストールできます。

- 無料ダウンロードやその他の選択肢が提供されている Visual Studio 製品ページにブラウザーでアクセスして、無料版の Visual Studio Community をインストールする。または
-  [Azure ポータル](http://portal.azure.com/) ダウンロードの web ページは、次に説明するガイドです。


### Azure ポータルから Visual Studio へ


1. ログイン、 [Azure ポータル](http://portal.azure.com/), 、http://portal.azure.com/ です。

2. クリックして **参照* すべて** > **SQL データベース**します。 データベースを検索するためのブレードが開きます。

3. 上部にある [フィルター] ボックスで、開始の名前を入力、 **AdventureWorksLT** データベースです。

4. お使いのサーバー上のデータベースに関する行が表示されたら、その行をクリックします。 データベースに関するブレードが開きます。

5. 操作がしやすいように、1 つ前までの各ブレードで最小化のコントロールをクリックします。

6. クリックして、 **Visual Studio で開く** データベースのブレードの上部にあるボタンをクリックします。 Visual Studio のインストール場所へのリンクを備えた、Visual Studio に関する新しいブレードが開きます。

    ![[Visual Studio で開く] ボタン][20-OpenInVisualStudioButton]

7. クリックして、 **コミュニティ (無料)** リンク、またはのようなリンク。 新しい Web ページが追加されます。

8. 新しい Web ページ上のリンクを使用して Visual Studio をインストールします。

9. [Visual Studio がインストールされた後、 **Visual Studio で開いている** ブレードをクリックして、 **Visual Studio で開いている** ] ボタンをクリックします。 Visual Studio が開きます。

10. 特典の **SQL Server オブジェクト エクスプ ローラー** ] ウィンドウで、Visual Studio のダイアログ ボックスで接続文字列のフィールドに入力するように要求します。
 - 選択 **SQL Server 認証**, ではなく、 **Windows 認証**します。
 - 必ず指定してください、 **AdventureWorksLT** データベース (**オプション** > **接続プロパティ** ] ダイアログ ボックス)。

11.  **SQL Server オブジェクト エクスプ ローラー**, 、データベースのノードを展開します。


## 手順 2. Visual Studio で新しいプロジェクトを作成する


Visual Studio で、c# のスタート テンプレートに基づく新しいプロジェクトを作成 > Windows > **コンソール アプリケーション**します。


1. クリックして **ファイル** > **新しい** > **プロジェクト**します。 **** ダイアログが表示されます。

2.  **インストールされている**, 、C# の場合と、Windows を展開できるように、 **コンソール アプリケーション** 中央のペインでオプションが表示されます。

    ![[新しいプロジェクト] ダイアログ][30-VSNewProject]

2.  **名前** 入力 **ConnectAndQuery_Example**します。 Click **OK**.


## 手順 3. 構成処理のアセンブリ参照を追加する


この c# サンプルは .NET Framework アセンブリを使用して **System.Configuration.dll**, のでへの参照を追加してみましょう。


1.  **ソリューション エクスプ ローラー** ] ウィンドウで、右クリックし **参照** > **参照の追加**します。  **参照マネージャー** ウィンドウが表示されます。

2. 展開 **アセンブリ** > **Framework**します。

3. スクロールし、クリックして強調表示 **System.Configuration**します。 チェック ボックスがオンになっていることを確認します。

4. Click **OK**.

5. メニューでプログラムをコンパイル **ビルド** > **ソリューションのビルド**します。


## 手順 4. 接続文字列を取得する


使用して、 [Azure ポータル](http://portal.azure.com/) SQL データベースに接続するために必要な接続文字列をコピーします。

最初の使用は、Visual Studio を Azure SQL Database に接続する **AdventureWorksLT** データベースです。


[AZURE.INCLUDE [sql-database-include-connection-string-20-portalshots](../../includes/sql-database-include-connection-string-20-portalshots.md)]


## 手順 5. 接続文字列を App.config ファイルに追加する


1. Visual Studio で、[ソリューション エクスプローラー] ペインから App.config ファイルを開きます。

2. 追加、 **& #x3c; の構成 #x3e; & #x3c;/構成と #x3e;** 例 App.config の次のコード サンプルで示すようにします。
 - 置き換える、 *{your_placeholders}* を実際の値にします。

```
    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
        <startup>
            <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5" />
        </startup>

        <connectionStrings>
            <clear />
            <add name="ConnectionString4NoUserIDNoPassword"
            connectionString="Server=tcp:{your_serverName_here}.database.windows.net,1433; Database={your_databaseName_here}; Connection Timeout=30; Encrypt=True; TrustServerCertificate=False;"
            />
        </connectionStrings>
    </configuration>
```

3. App.config への変更を保存します。

4. ソリューション エクスプ ローラー ペインで右クリックし、 **App.config** ノードをクリックして **プロパティ**します。

5. 設定、 **出力ディレクトリにコピー** に **常にコピー**します。
 - これにより、&#x2a;.exe ファイルのビルド先ディレクトリにある &#x2a;.exe.config ファイルの内容が、App.config ファイルの内容に置き換わります。 この置き換えは、&#x2a;.exe を再コンパイルするたびに発生します。
 - &#x2a;.exe.config ファイルは、C# サンプル プログラムの実行時に読み取られます。

    ![[出力ディレクトリにコピー] = [常にコピーする]][50-VSCopyToOutputDirectoryProperty]


## 手順 6. C# サンプル コード内に貼り付ける


1. Visual Studio で使用して、 **ソリューション エクスプ ローラー** ウィンドウを開き、 **Program.cs** ファイルです。

    ![C# サンプル コード内に貼り付けます。][40-VSProgramCsOverlay]

2. 次の C# サンプル コードを貼り付けて、Program.cs 内のすべてのスタート コードを上書きします。
 - 短いコード サンプルを実行する場合に、変数、リテラルとして接続文字列全体を割り当てることができます **SQLConnectionString**します。 2 つのメソッドを消去することができますし、 **GetConnectionStringFromExeConfig** と **GatherPasswordFromConsole**します。


```
using System;  // C#
using G = System.Configuration;   // System.Configuration.dll
using D = System.Data;            // System.Data.dll
using C = System.Data.SqlClient;  // System.Data.dll
using T = System.Text;

namespace ConnectAndQuery_Example
{
    class Program
    {
        static void Main()
        {
            string connectionString4NoUserIDNoPassword,
                password, userName, SQLConnectionString;

            // Get most of the connection string from ConnectAndQuery_Example.exe.config
            // file, in the same directory where ConnectAndQuery_Example.exe resides.
            connectionString4NoUserIDNoPassword = Program.GetConnectionStringFromExeConfig
                ("ConnectionString4NoUserIDNoPassword");
            // Get the user name from keyboard input.
            Console.WriteLine("Enter your User ID, without the trailing @ and server name: ");
            userName = Console.ReadLine();
            // Get the password from keyboard input.
            password = Program.GatherPasswordFromConsole();

            SQLConnectionString = "Password=" + password + ';' +
                "User ID=" + userName + ";" + connectionString4NoUserIDNoPassword;

            // Create an SqlConnection from the provided connection string.
            using (C.SqlConnection connection = new C.SqlConnection(SQLConnectionString))
            {
                // Formulate the command.
                C.SqlCommand command = new C.SqlCommand();
                command.Connection = connection;

                // Specify the query to be executed.
                command.CommandType = D.CommandType.Text;
                command.CommandText = @"
                    SELECT TOP 9 CustomerID, NameStyle, Title, FirstName, LastName
                    FROM SalesLT.Customer;  -- In AdventureWorksLT database.
                    ";
                // Open a connection to database.
                connection.Open();

                // Read data returned for the query.
                C.SqlDataReader reader = command.ExecuteReader();
                while (reader.Read())
                {
                    Console.WriteLine("Values:  {0}, {1}, {2}, {3}, {4}",
                        reader[0], reader[1], reader[2], reader[3], reader[4]);
                }
            }
            Console.WriteLine("View the results here, then press any key to finish...");
            Console.ReadKey(true);
        }
        //----------------------------------------------------------------------------------

        static string GetConnectionStringFromExeConfig(string connectionStringNameInConfig)
        {
            G.ConnectionStringSettings connectionStringSettings =
                G.ConfigurationManager.ConnectionStrings[connectionStringNameInConfig];

            if (connectionStringSettings == null)
            {
                throw new ApplicationException(String.Format
                    ("Error. Connection string not found for name '{0}'.",
                    connectionStringNameInConfig));
            }
                return connectionStringSettings.ConnectionString;
        }

        static string GatherPasswordFromConsole()
        {
            T.StringBuilder passwordBuilder = new T.StringBuilder(32);
            ConsoleKeyInfo key;
            Console.WriteLine("Enter your password: ");
            do
            {
                key = Console.ReadKey(true);
                if (key.Key != ConsoleKey.Backspace)
                {
                    passwordBuilder.Append(key.KeyChar);
                    Console.Write("*");
                }
                else  // Backspace char was entered.
                {
                    // Retreat the cursor, overlay '*' with ' ', retreat again.
                    Console.Write("\b \b");
                    passwordBuilder.Length = passwordBuilder.Length - 1;
                }
            }
            while (key.Key != ConsoleKey.Enter); // Enter key will end the looping.
            Console.WriteLine(Environment.NewLine);
            return passwordBuilder.ToString();
        }
    }
}
```


### プログラムをコンパイルする


1. Visual Studio でメニューをクリックして、プログラムをコンパイル **ビルド** > **ソリューションのビルド**します。


### サンプル プログラム内の操作の概要


1. SQL 接続文字列の大部分を構成ファイルから読み取ります。

2. キーボードから入力されるユーザー名とパスワードを収集し、それを追加して接続文字列を完成させます。

3. 接続する接続文字列、および ADO.NET クラスを使用して、 **AdventureWorksLT** SQL データベースでデータベースをデモンストレーションします。

4. SQL の問題 **選択** を読み取ったり、 **SalesLT** テーブルです。

5. 返された行をコンソールに出力します。


C# サンプルのコードは常に短く記述するようにしています。 しかし、ユーザーの皆様からのご要望を尊重して、このサンプルには構成ファイルを読み取るためのコードを追加しました。 ご要望のとおり、実稼働品質のプログラムでは、.exe にハードコーディングしたリテラルではなく、構成ファイルを使用することが必要です。


> [AZURE.WARNING] コードの簡潔さを優先するためには、例外処理のためのコードを含む、このなしの教育用のサンプルの再試行ロジックを選択しなかった。 ただし、クラウド データベースとやり取りをする実稼働プログラムには、両方のコードを加える必要があります。
>
> [ここで](sql-database-develop-csharp-retry-windows.md) 再試行ロジックを持つコード サンプルへのリンクです。


## 手順 7. 使用できる IP アドレスの範囲をサーバーのファイアウォールに追加する


クライアント コンピューターの IP アドレスを SQL Database のファイアウォールに追加しない限り、クライアントの C# プログラムが SQL Database に接続することはできません。 プログラムを実行しても失敗します。実行に失敗すると、必要な IP アドレスが記載された簡単なエラー メッセージが表示されます。


使用することができます、 [Azure ポータル](http://portal.azure.com/) IP アドレスを追加します。



[AZURE.INCLUDE [sql-database-include-ip-address-22-v12portal](../../includes/sql-database-include-ip-address-22-v12portal.md)]



詳細については、次を参照してください。<br/>
[方法: ファイアウォール設定を構成する (SQL Database)](sql-database-configure-firewall-settings.md)



## 手順 8. プログラムを実行する


1. Visual Studio でプログラムを実行 c# クエリ] メニューの [ **デバッグ** > **[デバッグ開始]**します。 コンソール ウィンドウが表示されます。

2. 指示に従って、ユーザー名とパスワードを入力します。
 - 一部の接続ツールでは、ユーザー名に "@{サーバー名}" を追加する必要がありますが、ADO.NET の場合、このサフィックスは省略可能です。 このサフィックスを入力する必要はありません。

3. 数行のデータが表示されます。


## 関連リンク


- [SQL Database のクライアント クイック スタート コード サンプル](sql-database-develop-quick-start-client-code-samples.md)

- クライアント プログラムは、Azure VM で稼働する場合は、ある 1433年以外の TCP ポートについて説明します。<br/>[ポート 1433 for ADO.NET 4.5 および SQL 以外のデータベースの V12](sql-database-develop-direct-route-ports-adonet-v12.md)します。



<!-- Image references. -->

[20-OpenInVisualStudioButton]: ./media/sql-database-connect-query/connqry-free-vs-e.png

[30-VSNewProject]: ./media/sql-database-connect-query/connqry-vs-new-project-f.png

[40-VSProgramCsOverlay]: ./media/sql-database-connect-query/connqry-vs-program-cs-overlay-g.png

[50-VSCopyToOutputDirectoryProperty]: ./media/sql-database-connect-query/connqry-vs-appconfig-copytoputputdir-h.png


