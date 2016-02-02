### Composer 経由でインストールする

1. [Git をインストールする ][install-git]します。 Windows では、Git 実行可能ファイルも PATH 環境変数に追加する必要があります。

2. プロジェクトのルートに **composer.json** という名前のファイルを作成して、次のコードを追加します。

    ```
    {
        "repositories": [
            {
                "type": "pear",
                "url": "http://pear.php.net"
            }
        ],
        "require": {
            "pear-pear.php.net/mail_mime" : "*",
            "pear-pear.php.net/http_request2" : "*",
            "pear-pear.php.net/mail_mimedecode" : "*",
            "microsoft/windowsazure": "*"
        }
    }
    ```

3. ダウンロード * *[composer.phar ][composer-phar]* * プロジェクトのルートです。

4. コマンド プロンプトを開き、次のコマンドをプロジェクトのルートで実行します。

    ```
    php composer.phar install
    ```

### 手動でインストールする

Azure 向け PHP クライアント ライブラリを手動でダウンロードしてインストールするには、次のステップに従います。
> [AZURE.NOTE] Azure 向け PHP クライアント ライブラリ依存している、 [HTTP_Request2](http://pear.php.net/package/HTTP_Request2), 、[Mail_mime](http://pear.php.net/package/Mail_mime), 、および [Mail_mimeDecode](http://pear.php.net/package/Mail_mimeDecode) PEAR パッケージです。 これらの依存関係を解決するのには推奨される方法を使用してこれらのパッケージをインストールする、 [PEAR パッケージ マネージャー](http://pear.php.net/manual/en/installation.php)します。

1. ライブラリを含む .zip アーカイブをダウンロード [GitHub ][php-sdk-github]します。 または、リポジトリをフォークして、ローカル コンピューターにその複製を作成します この 2 つ目の方法では、GitHub アカウントが必要であるほか、Git がローカルにインストールされている必要があります。

2. コピー、 `WindowsAzure` アプリケーションのディレクトリ構造をダウンロードしたアーカイブのディレクトリ。

(PEAR パッケージとしてのインストールに関する情報を含めて) Azure 向け PHP クライアント ライブラリをインストールする方法の詳細については、次を参照してください。 [for PHP ][download-sdk-php]します。


[php-sdk-github]: http://go.microsoft.com/fwlink/?LinkId=252719 
[install-git]: http://git-scm.com/book/en/Getting-Started-Installing-Git 
[download-sdk-php]: ../articles/php-download-sdk.md 
[composer-phar]: http://getcomposer.org/composer.phar 

