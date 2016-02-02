<properties 
    pageTitle="Azure App Service で PHP-MySQL Web アプリを作成して Git でデプロイする" 
    description="Azure SQL Database にデータを保存する PHP Web アプリを作成し、Azure App Service への Git デプロイメントを使用する方法を示すチュートリアル。" 
    services="app-service\web, sql-database" 
    documentationCenter="php" 
    authors="tfitzmac" 
    manager="wpickett" 
    editor="mollybos"/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="PHP" 
    ms.topic="article" 
    ms.date="11/19/2015" 
    ms.author="tomfitz"/>


# Azure App Service で PHP-MySQL Web アプリを作成して Git でデプロイする

このチュートリアルは、PHP web アプリを作成する方法を示します [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) Git を使用してそれをデプロイする方法と Azure SQL Database に接続します。 このチュートリアルには、必要 [PHP ][install-php], 、[SQL Server Express の ][install-sqlexpress], 、 [Microsoft Drivers for SQL Server for PHP](http://www.microsoft.com/download/en/details.aspx?id=20098), 、および [Git ][install-git] お使いのコンピューターにインストールされています。 このチュートリアルを完了すると、Azure で動作する PHP-SQL Web アプリが完成します。
> [AZURE.NOTE]
> インストールして、PHP を使用するため、SQL Server 用 PHP、SQL Server Express、およびマイクロソフトのドライバーを構成、 [Microsoft Web Platform Installer](http://www.microsoft.com/web/downloads/platform.aspx)します。

学習内容:

* Azure の web アプリと SQL データベースを使用して、作成する方法、 [Azure ポータル](http://go.microsoft.com/fwlink/?LinkId=529715)します。 App Service Web Apps では PHP が既定で有効になっているため、特に何もしなくても PHP コードを実行できます
* Git を使用して Azure にアプリケーションを発行および再発行する方法。

このチュートリアルでは、登録用の単純な Web アプリケーションを PHP で作成します。 このアプリケーションは Azure の Web サイトでホストします。 完成したアプリケーションのスクリーンショットは次のようになります。

![Azure PHP Web Site](./media/web-sites-php-sql-database-deploy-use-git/running_app_3.png)

[AZURE.INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]
>[AZURE.NOTE] 場合は、Azure アカウントがサインアップする前に Azure App Service の使用を開始するには、 [App Service の試用](http://go.microsoft.com/fwlink/?LinkId=523751), 、App Service で有効期間の短いスターター web アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

## Azure Web アプリの作成と Git 発行の設定

Azure の Web アプリと SQL Database を作成するには、次の手順に従います。

1. ログイン、 [Azure ポータル](https://portal.azure.com/)します。

2. ダッシュ ボードの左上の **[新規]** アイコンをクリックして Azure Marketplace を開き、Marketplace の横の **[すべて選択]** をクリックし、**[Web + モバイル]** を選択します。

3. Marketplace で **[Web + モバイル]** を選択します。

4. **[Web アプリ + SQL]** アイコンをクリックします。

5. Web アプリ + SQL アプリの説明を読み、**[作成]** を選択します。

6. 各部分 (**[リソース グループ]**、**[Web アプリ]**、**[データベース]**、**[サブスクリプション]**) をクリックし、必要なフィールドに値を入力するか、値を選択します。

    - 好みの URL 名を入力します
    - データベース サーバーの資格情報を構成します。
    - 最も近いリージョンを選択します

    ![configure your app](./media/web-sites-php-sql-database-deploy-use-git/configure-db-settings.png)

7. Web アプリの定義が完了したら、**[作成]** をクリックします。

    Web アプリが作成されると、**[通知]** ボタンが緑色の **[成功]** と点滅し、リソース グループ ブレードが開いてグループの Web アプリと SQL Database 双方が表示されます。

4. リソース グループ ブレードの Web アプリのアイコンをクリックして Web アプリのブレードを開きます。

    ![Web アプリ リソース グループ](./media/web-sites-php-sql-database-deploy-use-git/resource-group-blade.png)

5. **[設定]** で **[継続的なデプロイメント]**、**[必要な設定の構成]** を順にクリックします。 **Local[ローカル Git リポジトリ]** を選択し、**[OK]** をクリックします。

    ![ソース コードの位置](./media/web-sites-php-sql-database-deploy-use-git/setup-local-git.png)

    Git リポジトリが未選択である場合は、ユーザー名とパスワードを指定する必要があります。 そのためには、Web アプリのブレードで **[設定]**、**[デプロイメント資格情報]** をクリックします。

    ![](./media/web-sites-php-sql-database-deploy-use-git/deployment-credentials.png)

6. **[設定]** で **[プロパティ]** をクリックして、後ほど PHP アプリのデプロイで使用する Git リモート URL を確認します。

## SQL Database 接続情報を取得する

Web アプリにリンクされている SQL Database インスタンスに接続するには、データベースの作成時に指定した接続情報が必要になります。 SQL Database 接続情報を取得するには、次の手順に従います。

1. リソース グループのブレードに戻り、SQL データベース アイコンをクリックします。

2. SQL Database ブレードで **[設定]**、**[プロパティ]**、**[データベース接続文字列を表示]** を順にクリックします。

    ![データベースのプロパティの表示](./media/web-sites-php-sql-database-deploy-use-git/view-database-properties.png)

3. **PHP** セクションで表示されたダイアログの値を書き留めて `サーバー`, 、`SQL Database`, 、および `ユーザー名`します。 この値は後ほど PHP Web アプリを Azure App Service に発行する際に使用します。

## アプリケーションの作成とローカル テスト

Registration アプリケーションは、名前と電子メール アドレスを入力してイベントに登録するための、単純な PHP アプリケーションです。 それまでの登録者情報がテーブルに表示されます。 登録情報は SQL Database インスタンスに保存されます。 アプリケーションは、次の 2 つのファイルで構成されます (下にあるコードをコピーし、貼り付けて使用できます)。

* **index.php**: 登録用のフォームと登録者情報が含まれたテーブルを表示します。
* **createtable.php**: アプリケーション用の SQL Database テーブルを作成します。 このファイルは 1 度しか使用されません。

アプリケーションをローカルで実行するには、次の手順に従います。 次の手順は、PHP と SQL Server Express、ローカル コンピューターで設定があることを前提としてに有効にした、 [[pdo sqlsrv] の SQL Server 用 PDO 拡張機能][pdo-sqlsrv]します。

1. いう SQL Server データベースを作成する `登録`します。 これを行うことができます、 `sqlcmd` コマンド プロンプトで次のコマンド。

        >sqlcmd -S localhost\sqlexpress -U <local user name> -P <local password>
        1> create database registration
        2> GO   

2. アプリケーションのルート ディレクトリでもという 2 つのファイルを作成 `createtable.php` と呼ばれる 1 つ `index.php`します。

3. 開いている、 `createtable.php` ファイルをテキスト エディターまたは IDE で、次のコードを追加します。 このコードは作成に使用される、 `registration_tbl` テーブルに、 `登録` データベースです。

        <?php
        // DB connection info
        $host = "localhost\sqlexpress";
        $user = "user name";
        $pwd = "password";
        $db = "registration";
        try{
            $conn = new PDO( "sqlsrv:Server= $host ; Database = $db ", $user, $pwd);
            $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );
            $sql = "CREATE TABLE registration_tbl(
            id INT NOT NULL IDENTITY(1,1) 
            PRIMARY KEY(id),
            name VARCHAR(30),
            email VARCHAR(30),
            date DATE)";
            $conn->query($sql);
        }
        catch(Exception $e){
            die(print_r($e));
        }
        echo "<h3>Table created.</h3>";
        ?>

    値を更新する必要があります <code>$user</code> と <code>$pwd</code> ローカル SQL Server ユーザー名とパスワードを使用します。

4. アプリケーションのルート ディレクトリのターミナルで、次のコマンドを入力します。

        php -S localhost:8000

4. Web ブラウザーを開きを参照 **http://localhost:8000/createtable.php**します。 これは、作成、 `registration_tbl` データベース内のテーブルです。

5. **index.php** ファイルをテキスト エディターまたは IDE で開いて、ページの基本的な HTML コードおよび CSS コードを追加します (PHP コードは後で追加します)。

     <html>
     <head>
     <Title>Registration Form</Title>
     <style type="text/css">
         body { background-color: #fff; border-top: solid 10px #000;
             color: #333; font-size: .85em; margin: 20; padding: 20;
             font-family: "Segoe UI", Verdana, Helvetica, Sans-Serif;
         }
         h1, h2, h3,{ color: #000; margin-bottom: 0; padding-bottom: 0; }
         h1 { font-size: 2em; }
         h2 { font-size: 1.75em; }
         h3 { font-size: 1.2em; }
         table { margin-top: 0.75em; }
         th { font-size: 1.2em; text-align: left; border: none; padding-left: 0; }
         td { padding: 0.25em 2em 0.25em 0em; border: 0 none; }
     </style>
     </head>
     <body>
     <h1>Register here!</h1>
     <p>Fill in your name and email address, then click <strong>Submit</strong> to register.</p>
     <form method="post" action="index.php" enctype="multipart/form-data" >
           Name  <input type="text" name="name" id="name"/></br>
           Email <input type="text" name="email" id="email"/></br>
           <input type="submit" name="submit" value="Submit" />
     </form>
     <?php
    
     ?>
     </body>
     </html>

6. PHP タグ内に、データベースに接続するための PHP コードを追加します。

        // DB connection info
        $host = "localhost\sqlexpress";
        $user = "user name";
        $pwd = "password";
        $db = "registration";
        // Connect to database.
        try {
            $conn = new PDO( "sqlsrv:Server= $host ; Database = $db ", $user, $pwd);
            $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );
        }
        catch(Exception $e){
            die(var_dump($e));
        }

    値を更新する必要がありますが、もう一度 <code>$user</code> と <code>$pwd</code> ローカルの MySQL ユーザー名とパスワードを使用します。

7. データベース接続コードの次に、登録情報をデータベースに挿入するためのコードを追加します。

        if(!empty($_POST)) {
        try {
            $name = $_POST['name'];
            $email = $_POST['email'];
            $date = date("Y-m-d");
            // Insert data
            $sql_insert = "INSERT INTO registration_tbl (name, email, date) 
                           VALUES (?,?,?)";
            $stmt = $conn->prepare($sql_insert);
            $stmt->bindValue(1, $name);
            $stmt->bindValue(2, $email);
            $stmt->bindValue(3, $date);
            $stmt->execute();
        }
        catch(Exception $e) {
            die(var_dump($e));
        }
        echo "<h3>Your're registered!</h3>";
        }

8. 上のコードの次に、データベースからデータを取得するためのコードを追加します。

        $sql_select = "SELECT * FROM registration_tbl";
        $stmt = $conn->query($sql_select);
        $registrants = $stmt->fetchAll(); 
        if(count($registrants) > 0) {
            echo "<h2>People who are registered:</h2>";
            echo "<table>";
            echo "<tr><th>Name</th>";
            echo "<th>Email</th>";
            echo "<th>Date</th></tr>";
            foreach($registrants as $registrant) {
                echo "<tr><td>".$registrant['name']."</td>";
                echo "<td>".$registrant['email']."</td>";
                echo "<td>".$registrant['date']."</td></tr>";
            }
            echo "</table>";
        } else {
            echo "<h3>No one is currently registered.</h3>";
        }


移動できるようになりました **http://localhost:8000/index.php** アプリケーションをテストします。

## アプリケーションの発行

アプリケーションをローカルでテストした後、Git を使用してそのアプリケーションを App Service Web Apps に発行できます。 ただし、まずアプリケーション内のデータベース接続情報を更新する必要があります。 先に取得したデータベース接続情報を使用して (で、 **SQL Database の接続情報の取得** セクション) で次の情報を更新 **両方** 、 `createdatabase.php` と `index.php` ファイルの適切な値。

    // DB connection info
    $host = "tcp:<value of Server>";
    $user = "<value of User Name>";
    $pwd = "<your password>";
    $db = "<value of SQL Database>";

> [AZURE.NOTE]
> <code>$Host</code>, 、サーバーの値の先頭にする必要があります <code>tcp:</code>します。


これで、Git 発行を設定してアプリケーションを発行する準備ができました。
> [AZURE.NOTE]
> これらは、上の「**Azure の Web アプリの作成と Git 発行の設定**」セクションの最後でメモした手順と同じです。


1. Gitbash (Git にある場合は、ターミナル、 `パス`)、アプリケーションのルート ディレクトリに移動 (、 **登録** ディレクトリ)、次のコマンドを実行します。

        git init
        git add .
        git commit -m "initial commit"
        git remote add azure [URL for remote repository]
        git push azure master

    先ほど作成したパスワードを入力するように求められます。

2. 参照 **http://[web アプリ name].azurewebsites.net/createtable.php** アプリケーション用の SQL データベース テーブルを作成します。
3. 参照 **http://[web アプリ name].azurewebsites.net/index.php** アプリケーションの使用を開始します。

アプリケーションを発行した後、アプリケーションへの変更を開始し、Git を使用してその変更を発行することもできます。

## アプリケーションへの変更の発行

アプリケーションへの変更を発行するには、次のステップに従います。

1. ローカルでアプリケーションへの変更を行います。
2. Gitbash (ターミナル Git が、 `パス`)、ディレクトリ、アプリケーションのルート ディレクトリに変更し、次のコマンドを実行します。

        git add .
        git commit -m "comment describing changes"
        git push azure master

    先ほど作成したパスワードを入力するように求められます。

3. 参照 **http://[web アプリ name].azurewebsites.net/index.php** 変更内容を確認します。

## 変更内容

* Web サイトから App Service への変更のガイドを参照してください: [Azure App Service と既存の Azure サービスへの影響](http://go.microsoft.com/fwlink/?LinkId=529714)





[install-php]: http://www.php.net/manual/en/install.php 
[install-sqlexpress]: http://www.microsoft.com/download/details.aspx?id=29062 
[install-drivers]: http://www.microsoft.com/download/details.aspx?id=20098 
[install-git]: http://git-scm.com/ 
[pdo-sqlsrv]: http://php.net/pdo_sqlsrv 

