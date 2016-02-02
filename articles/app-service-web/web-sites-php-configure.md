<properties
    pageTitle="Azure App Service Web Apps での PHP の構成 | Microsoft Azure"
    description="Azure App Service の Web Apps 用に既定の PHP インストールを構成する方法、またはカスタム PHP インストールを追加する方法を説明します。"
    services="app-service"
    documentationCenter="php"
    authors="tfitzmac"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="12/16/2015"
    ms.author="tomfitz"/>


# Azure App Service Web Apps での PHP の構成方法

## はじめに

このガイドがビルトインの PHP ランタイムを Web Apps 用に構成する方法を説明 [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714), 拡張機能を有効にする、カスタムの PHP ランタイムを使用します。 App Service を使用するには、[無料評価版] にサインアップします。 このガイドを最大限に活用するには、まず App Service で PHP Web アプリを作成する必要があります。

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## 方法: ビルトインの PHP バージョンを変更する

既定では、App Service Web アプリを作成すると PHP 5.4 がインストールされ、直ちに使用できる状態になります。 使用可能なリリース リビジョン、既定の構成、および有効な拡張機能を確認する最善の方法では、[phpinfo()] 関数を呼び出すスクリプトを展開します。

PHP 5.5 および PHP 5.6 も使用できますが、既定では有効になっていません。 PHP バージョンを更新するには、次のいずれかの方法に従います。

### Azure ポータル

1. Web アプリを参照し、 [Azure ポータル](https://portal.azure.com) ] をクリックし、 **設定** ] ボタンをクリックします。

    ![Web アプリ設定][settings-button]

2. **[設定]** ブレードで、**[アプリケーションの設定]** を選択し、新しい PHP バージョンを指定します。

    ![アプリケーションの設定][application-settings]

3. **[Web app settings]** ブレードの上部にある **[保存]** ボタンをクリックします。

    ![構成設定を保存する][save-button]

### Azure PowerShell (Windows)。

1. Azure PowerShell を起動し、アカウントにログインします。

        PS C:\> Login-AzureRmAccount

2. Web アプリの PHP バージョンを設定します。

        PS C:\> Set-AzureWebsite -PhpVersion [5.4 | 5.5 | 5.6] -Name {site-name}

3. PHP バージョンが設定されました。 これらの設定を確認できます。

        PS C:\> Get-AzureWebsite -Name {site-name} | findstr PhpVersion


### Azure コマンド ライン インターフェイス (Mac、Linux、Windows)

Azure コマンド ライン インターフェイスを使用するには、**Node.js** をコンピューターにインストールする必要があります。

1. ターミナルを開いてアカウントにログインします。

        azure login

2. Web アプリの PHP バージョンを設定します。

        azure site set --php-version [5.4 | 5.5] {site-name}

3. PHP バージョンが設定されました。 これらの設定を確認できます。

        azure site show {site-name}



## 方法: ビルトインの PHP 構成を変更する

次の手順に従うと、いずれのビルトイン PHP ランタイムについても、任意の構成オプションを変更できます (Php.ini ディレクティブについては、[php.ini ディレクティブの一覧] を参照してください)。

### PHP\_INI\_USER PHP\_INI\_PERDIR を変更する PHP\_INI\_ALL 構成設定

1. 追加、[. '.user.ini'] ファイルをルート ディレクトリにします。
2. 構成設定を追加、 `. '.user.ini'` で使用するものと同じ構文を使用して、 `php.ini` ファイルです。 有効にする場合など、 `display_errors` 設定にして `含めます` を 10 M に設定、 `. '.user.ini'` ファイルには、このテキストにが含まれます。

        ; Example Settings
        display_errors=On
        upload_max_filesize=10M

3. Web アプリをデプロイします。
4. Web アプリを再起動します。 (によって読み取られる頻度 php ために再起動が必要 `. '.user.ini'` ファイルといいます、 `user_ini.cache_ttl` システム レベルの設定は、既定では 300 秒 (5 分) を設定します。 新しい設定を読み取るには、PHP web アプリを再起動するとより、 `. '.user.ini'` ファイルです)。

使用する代わりに、 `. '.user.ini'` ファイルを [ini_set()] 関数スクリプトで使用できるシステム レベルのディレクティブではない構成オプションを設定します。

### PHP\_INI\_SYSTEM 構成設定の変更

1. Web アプリにキーを使用して、アプリケーション設定を追加 `PHP_INI_SCAN_DIR` および値 `d:\home\site\ini`
2. 作成、 `settings.ini` Kudu コンソールを使用したファイル (http://< サイト名 >. scm.azurewebsite.net) で、 `d:\home\site\ini` ディレクトリ。
3. 構成設定を追加、 `settings.ini` 'php.ini' ファイルで使用するものと同じ構文を使用したファイルです。 たとえば、ポイントする必要がある場合、 `curl.cainfo` に設定、 `*.crt` ファイルし、'wincache.maxfilesize' 設定を 512 K に設定、 `settings.ini` ファイルには、このテキストにが含まれます。

        ; Example Settings
        curl.cainfo="%ProgramFiles(x86)%\Git\bin\curl-ca-bundle.crt"
        wincache.maxfilesize=512

4. 変更内容を読み込むには、Web アプリを再起動します。

## 方法: 既定の PHP ランタイムで拡張機能を有効にする

前のセクションに示すとおり、既定の PHP バージョン、既定の構成、および有効な拡張機能を確認する最善の方法が、[phpinfo()] を呼び出すスクリプトを展開します。 追加の拡張機能を有効にするには、次の手順に従います。

### Ini 設定を使用して構成します。

1. 追加、 `ext` ディレクトリを `d:\home\site` ディレクトリ。
2. Put `.dll` 拡張ファイル、 `ext` ディレクトリ (たとえば、 `php_mongo.dll` と `php_xdebug.dll`)。 拡張機能は、PHP の既定バージョン (この文書の作成時点では PHP 5.4) との互換性があり、VC9 および非スレッドセーフ (nts) 互換であることを確認してください。
3. Web アプリにキーを使用して、アプリケーション設定を追加 `PHP_INI_SCAN_DIR` および値 `d:\home\site\ini`
4. 作成、 `ini` ファイル `d:\home\site\ini` と呼ばれる `extensions.ini`します。
5. 構成設定を追加、 `extensions.ini` 'php.ini' ファイルで使用するものと同じ構文を使用したファイルです。 たとえば、MongoDB や XDebug 拡張機能を有効にする場合、 `extensions.ini` ファイルには、このテキストにが含まれます。

        ; Enable Extensions
        extension=d:\home\site\ext\php_mongo.dll
        zend_extension=d:\home\site\ext\php_xdebug.dll

6. 変更内容を読み込むには、Web アプリを再起動します。

### アプリ設定の構成

1. 追加、 `bin` ディレクトリをルート ディレクトリにします。
2. Put `.dll` 拡張ファイル、 `bin` ディレクトリ (たとえば、 `php_mongo.dll`)。 拡張機能は、PHP の既定バージョン (この文書の作成時点では PHP 5.4) との互換性があり、VC9 および非スレッドセーフ (nts) 互換であることを確認してください。
3. Web アプリをデプロイします。
4. Azure ポータルで Web アプリに移動し、**[設定]** ボタンをクリックします。

    ![Web アプリ設定][settings-button]

5. **[設定]** ブレードで、**[アプリケーションの設定]** を選択し、**[アプリの設定]** セクションまでスクロールします。
6. **[アプリの設定]** セクションで、**PHP_EXTENSIONS** キーを作成します。 このキーの値は、Web サイト ルート (**bin\your-ext-file**) への相対パスになります。

    ![[アプリケーション設定] で拡張機能を有効にする][php-extensions]

7. **[Web app settings]** ブレードの上部にある **[保存]** ボタンをクリックします。

    ![構成設定を保存する][save-button]

Zend 拡張機能も、**PHP_ZENDEXTENSIONS** キーを使用することによってサポートされます。 複数の拡張機能を有効にするには、コンマ区切りの一覧を含める `.dll` アプリケーション設定の値のファイルです。


## 方法: カスタムの PHP ランタイムを使用する

App Service Web Apps では、既定の PHP ランタイムを使用する代わりに、指定された PHP ランタイムを使用して PHP スクリプトを実行することができます。 提供するランタイムを構成できます、 `php.ini` ファイルを指定することもできます。 カスタムの PHP ランタイムを Web Apps で使用するには、次の操作を行います。

1. 非スレッドセーフおよび VC9 または VC11 互換バージョンの Windows 用 PHP を入手します。 Windows 用 PHP の最近のリリースをこちら: [http://windows.php.net/download/] です。 以前のリリースは、アーカイブ、: [http://windows.php.net/downloads/releases/archives/] です。
2. 変更、 `php.ini` ランタイム用にファイルです。 システム レベルのみのディレクティブである構成設定は、Web Apps では無視されます (システム レベルのみのディレクティブについては、[php.ini ディレクティブの一覧] を参照してください。)
3. 必要に応じて、PHP ランタイムに拡張を追加しでそれらを有効にする、 `php.ini` ファイルです。
4. 追加、 `bin` ディレクトリをルート ディレクトリ、および put をその中に、PHP ランタイムを含むディレクトリに (たとえば、 `bin\php`)。
5. Web アプリをデプロイします。
4. Azure ポータルで Web アプリに移動し、**[設定]** ボタンをクリックします。

    ![Web アプリ設定][settings-button]

7. **[設定]** ブレードで、**[アプリケーションの設定]** を選択し、**[ハンドラー マッピング]** セクションまでスクロールします。 追加 `*.php` 拡張機能にフィールドし、パスを追加、 `cgi.exe` 実行可能ファイルです。 PHP ランタイムを配置した場合、 `bin` でアプリケーションのルート ディレクトリ パスになります `D:\home\site\wwwroot\bin\php\php-cgi.exe`します。

    ![[ハンドラー マッピング] でハンドラーを指定する][handler-mappings]

8. **[Web app settings]** ブレードの上部にある **[保存]** ボタンをクリックします。

    ![構成設定を保存する][save-button]

## 次のステップ

詳細については、次を参照してください。、 [PHP デベロッパー センター](/develop/php/)します。
>[AZURE.NOTE] 場合は、Azure アカウントがサインアップする前に Azure App Service の使用を開始するには、 [App Service の試用](http://go.microsoft.com/fwlink/?LinkId=523751), 、App Service で有効期間の短いスターター web アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

## 変更内容

* Web サイトから App Service への変更のガイドを参照してください: [Azure App Service と既存の Azure サービスへの影響](http://go.microsoft.com/fwlink/?LinkId=529714)


[free trial]: https://www.windowsazure.com/pricing/free-trial/ 
[phpinfo()]: http://php.net/manual/en/function.phpinfo.php 
[select-php-version]: ./media/web-sites-php-configure/select-php-version.png 
[list of php.ini directives]: http://www.php.net/manual/en/ini.list.php 
[.user.ini]: http://www.php.net/manual/en/configuration.file.per-user.php 
[ini_set()]: http://www.php.net/manual/en/function.ini-set.php 
[application-settings]: ./media/web-sites-php-configure/application-settings.png 
[settings-button]: ./media/web-sites-php-configure/settings-button.png 
[save-button]: ./media/web-sites-php-configure/save-button.png 
[php-extensions]: ./media/web-sites-php-configure/php-extensions.png 
[handler-mappings]: ./media/web-sites-php-configure/handler-mappings.png 
[http://windows.php.net/download/]: http://windows.php.net/download/ 
[http://windows.php.net/downloads/releases/archives/]: http://windows.php.net/downloads/releases/archives/ 
[setphpvercli]: ./media/web-sites-php-configure/ChangePHPVersion-XPlatCLI.png 
[getphpvercli]: ./media/web-sites-php-configure/ShowPHPVersion-XplatCLI.png 
[setphpverps]: ./media/web-sites-php-configure/ChangePHPVersion-PS.png 
[getphpverps]: ./media/web-sites-php-configure/ShowPHPVersion-PS.png 

