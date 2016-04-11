<properties 
    pageTitle="Azure App Service での WordPress から Multisite への変換" 
    description="Azure のギャラリーを使って作成された既存の WordPress Web アプリを取得し、WordPress Multisite に変換する方法を説明します。" 
    services="app-service\web" 
    documentationCenter="php" 
    authors="tfitzmac" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="PHP" 
    ms.topic="article" 
    ms.date="10/01/2015" 
    ms.author="tomfitz"/>



# Azure App Service での WordPress から Multisite への変換

## 概要

*によって [Ben Lobaugh][ben-lobaugh], 、[Microsoft Open Technologies inc.][ms-open-tech]*

このチュートリアルでは、Azure のギャラリーを使って作成された既存の WordPress Web アプリを取得し WordPress Multisite インストールに変換する方法を説明します。 また、インストール内の各サブサイトにカスタム ドメインを割り当てる方法も説明します。

ここでは、WordPress のインストールが既にあるものとします。 そうしない場合は」の説明に従ってください [Azure でギャラリーから WordPress web サイトの作成][website-from-gallery]します。

変換する既存の WordPress から Multisite へのシングル サイト インストールには一般に非常に単純なもので、ここでは、最初の手順を実行直線が、 [ネットワークの作成][wordpress-codex-create-a-network] ] ページで、 [WordPress Codex](http://codex.wordpress.org)します。

それでは始めましょう。

## Multisite の許可

まず、Multisite を有効にする必要があります、 `wp-config.php` ファイルと、 **\_allow\_multisite** 定数です。 Web アプリ ファイルを編集するには、2 つの方法があります。FTP を使用する方法と、Git を使用する方法です。 どちらのセットアップ方法にも慣れていない場合は、以下のチュートリアルを参照してください。

* [MySQL および FTP による PHP Web サイト][website-w-mysql-and-ftp-ftp-setup]

* [MySQL および Git による PHP Web サイト][website-w-mysql-and-git-git-setup]

任意のエディターで `wp-config.php` ファイルを開き、以下の行を `/* That's all, stop editing! Happy blogging. */` 行の上に追加します。

    /* Multisite */

    define( 'WP_ALLOW_MULTISITE', true );

必ずファイルを保存し、サーバーにアップロードし直してください。

## ネットワークのセットアップ

ログイン、 *wp admin* の web アプリがあり、領域が [新しい項目を表示、 **ツール** というメニュー **ネットワーク セットアップ**します。 クリックして **ネットワーク セットアップ** し、ネットワークの詳細を入力します。

![[Network Setup] 画面][wordpress-network-setup]

このチュートリアルでは、 *sub-directories* サイト スキーマを常に機能する必要があり、チュートリアルの後半で各サブサイトのカスタム ドメインを設定することができます。 ただし、ことができます、サブドメイン インストールを経由でドメインをマップする場合は、セットアップに、 [Azure ポータル](https://portal.azure.com) し、ワイルドカード DNS を適切にセットアップします。

サブディレクトリのセットアップを参照するくださいとサブディレクトリの詳細については、 [マルチサイト ネットワークの種類][wordpress-codex-types-of-networks] 、WordPress Codex の記事です。

## ネットワークの有効化

これで、ネットワークがデータベースで構成されましたが、ネットワーク機能を有効にするにはもう 1 つ手順が残っています。 `wp-config.php` 設定の最終処理を行い、`web.config` が各サイトを適切にルーティングしていることを確認します。


クリックすると、 **インストール** のボタンでは、 *ネットワーク セットアップ* ] ページで、WordPress に更新されます、 `wp-config.php` と `web.config` ファイルです。 ただし、これらのファイルを必ずチェックし、正常に更新されていることを確認してください。 正常に更新されていない場合は、この画面に必要な更新が表示されます。 ファイルを編集して保存します。


これらの更新を行った後で、いったんログアウトしてから、この wp-admin ダッシュボードに再ログインする必要があります。

今すぐありますメニューが追加というラベルが付いた、管理バーに **個人用サイト**します。 このメニューでは、ボードを通じて新しいネットワークを制御することができます、 **Network Admin** ダッシュ ボード。

## カスタム ドメインの追加

 [WordPress MU Domain Mapping][wordpress-plugin-wordpress-mu-domain-mapping] プラグインを利用する簡単に、ネットワーク内のサイトにカスタム ドメインを追加します。 このプラグインを正常に機能させるには、ポータルとドメイン レジストラーで追加のセットアップを行う必要があります。

## Web アプリへのドメイン マッピングの有効化

 **Free** [App Service](http://go.microsoft.com/fwlink/?LinkId=529714) プラン モードでは、Web アプリへのカスタム ドメインの追加はサポートされません。 切り替える必要があります **Shared** または **標準** モードです。 これを行うには、次の手順を実行します。

* Azure ポータルにログインして、自分の Web アプリに移動します。 
* をクリックして、 **スケール** メイン コンテンツ領域のタブをクリックします。
*  **全般**, 、いずれかを選択 *SHARED* または *標準*
* クリックして **保存**

設定してある使用方法やその他の構成によっては、変更の確認と、Web アプリへの今後の課金の承認を求めるメッセージが表示される場合があります。

新しい設定の処理には少し時間がかかるので、ドメインのセットアップを開始します。

## ドメインの確認

Azure Web Apps でドメインをサイトにマップするには、まず、ドメインをマップする権限が与えられていることを確認する必要があります。 そのためには、新しい CNAME レコードを DNS エントリに追加します。

* 自分のドメインの DNS マネージャーにログインします
* 新しい CNAME を作成する *awverify*
* ポイント *awverify* に *awverify します。YOUR_DOMAIN.azurewebsites.net*

場合によっては、DNS の変更が完全に有効になるまで少し時間がかかります。以下の手順がすぐにはうまく動作しない場合は、しばらくしてからやり直してください。

## Web アプリへのドメインの追加

Azure ポータルを通じて web アプリに戻りをクリックして **設定**, 、クリックして **カスタム ドメインと SSL**します。

ときに、 *SSL 設定* が表示されたら、web アプリに割り当てるすべてのドメインを入力するためのフィールドが表示されます。 ドメインがこの一覧に表示されない場合、ドメイン DNS がどのようにセットアップされているかにかかわらず、そのドメインは WordPress 内でのマッピングに使用できません。

![[カスタム ドメインの管理] ダイアログ][wordpress-manage-domains]

ドメインをテキスト ボックスに入力すると、以前に作成した CNAME レコードが確認されます。 DNS が完全に反映されていない場合は、赤いインジケーターが表示されます。 成功した場合は、緑のチェック マークが表示されます。 

ダイアログの下部に表示される IP アドレスをメモしておきます。 ドメインのための A レコードをセットアップするときに必要になります。

## ドメイン A レコードのセットアップ

ここまでの手順が成功すると、DNS A レコードを通じてドメインを自分の Azure Web アプリに割り当てられるようになります。 

Azure の web アプリで許容される CNAME と A レコードのどちらもただしここで注意することが重要する *必要があります* A レコードを使用して、適切なドメイン マッピングを有効にします。 Azure では、YOUR_DOMAIN.azurewebsites.net で CNAME が作成されますが、CNAME は他の CNAME に転送することができません。

DNS マネージャーに戻り、前の手順でメモした IP アドレスを指すように A レコードをセットアップします。


## プラグインのインストールとセットアップ

現在、WordPress Multisite には、カスタム ドメインをマップするための方法が組み込まれていません。 ただし、というプラグインは [WordPress MU Domain Mapping][wordpress-plugin-wordpress-mu-domain-mapping] 機能を追加します。 サイトの Network Admin 部分にログインし、インストール、 **WordPress MU Domain Mapping** プラグインします。

インストールすると、プラグインをアクティブ化する、次を参照してください。 **設定** > **Domain Mapping** プラグインを構成します。 最初のテキスト ボックスに *サーバーの IP アドレス*, 、ドメインの A レコードをセットアップするために使用する IP アドレスを入力します。 いずれかの設定 *ドメイン オプション* する要望 (既定値は問題ありません多くの場合) をクリックして **保存**します。

## ドメインのマップ

参照してください、 **ダッシュ ボード** ドメインのマップ対象のサイトにします。 をクリックして **ツール** > **Domain Mapping** ] をクリックし、テキスト ボックスに新しいドメインを入力して **追加**します。

既定では、新しいドメインは自動生成されたサイト ドメインに書き換えられます。 新しいドメインのチェックに送信されるすべてのトラフィックがある場合、 *このブログのプライマリ ドメイン* 保存する前にボックス。 サイトにドメインの無制限の数を追加することができますが、1 つのみをプライマリにすることができます。

## 繰り返し

Azure Web Apps では、Web アプリに追加できるドメインの数に制限はありません。 実行する必要があります別のドメインを追加する、 **ドメインを確認** と **ドメイン A レコードをセットアップ** セクションでは、ドメインごとにします。  

>[AZURE.NOTE] 場合は、Azure アカウントがサインアップする前に Azure App Service の使用を開始するには、 [App Service の試用](http://go.microsoft.com/fwlink/?LinkId=523751), 、App Service で有効期間の短いスターター web アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

## 変更内容
* Web サイトから App Service への変更のガイドを参照してください: [Azure App Service と既存の Azure サービスへの影響](http://go.microsoft.com/fwlink/?LinkId=529714)

[ben-lobaugh]: http://ben.lobaugh.net
[ms-open-tech]: http://msopentech.com
[website-from-gallery]: https://www.windowsazure.com/develop/php/tutorials/website-from-gallery/
[wordpress-codex-create-a-network]: http://codex.wordpress.org/Create_A_Network
[website-w-mysql-and-ftp-ftp-setup]: https://www.windowsazure.com/develop/php/tutorials/website-w-mysql-and-ftp/#header-0
[website-w-mysql-and-git-git-setup]: https://www.windowsazure.com/develop/php/tutorials/website-w-mysql-and-git/#header-1
[wordpress-network-setup]: ./media/web-sites-php-convert-wordpress-multisite/wordpress-network-setup.png
[wordpress-codex-types-of-networks]: http://codex.wordpress.org/Before_You_Create_A_Network#Types_of_multisite_network
[wordpress-plugin-wordpress-mu-domain-mapping]: http://wordpress.org/extend/plugins/wordpress-mu-domain-mapping/

[wordpress-manage-domains]: ./media/web-sites-php-convert-wordpress-multisite/wordpress-manage-domains.png

 

