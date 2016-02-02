<properties
    pageTitle="コマンド ラインでの Mobile Services の管理 | Microsoft Azure"
    description="コマンド ライン ツールを使用して Azure Mobile Services を作成、デプロイ、管理する方法について説明します。"
    services="mobile-services"
    documentationCenter="Mobile"
    authors="ggailey777"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="NA"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="11/02/2015"
    ms.author="glenga"/>


# コマンド ライン ツールを使用したモバイル サービスの自動化

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

&nbsp;


## 概要

このトピックでは、Azure コマンド ライン ツールを使用して、Azure Mobile Services の作成と管理を自動化する方法を紹介します。 このトピックでは、コマンド ライン ツールのインストールとその使用を始めるための方法、およびコマンド ライン ツールを使用して主な Mobile Services を実行する方法について説明します。

個々のコマンドを単一のスクリプトまたはバッチ ファイルと組み合わせることにより、モバイル サービスの作成、確認、および削除のプロセスを自動化することができます。

このトピックでは、Azure コマンド ライン ツールでサポートされている一般的な管理タスクを取り上げます。 詳細については、次を参照してください。 [Azure コマンド ライン ツールのドキュメント ][reference-docs]します。

## Azure コマンド ライン ツールのインストール

次の説明に従って、お使いのオペレーティング システムに対応するコマンド ライン ツールをインストールしてください。

* **Windows**: ダウンロード、 [Azure コマンド ライン ツール インストーラー ][windows-installer]します。 ダウンロードした .msi ファイルを開き、指示に従ってインストール手順を実行します。

* **Mac**: ダウンロード、 [Azure SDK インストーラー ][mac-installer]します。 ダウンロードした .pkg ファイルを開き、指示に従ってインストール手順を実行します。

* **Linux**: 最新バージョンのインストール [Node.js ][nodejs-org] (を参照してください [Node.js パッケージ マネージャーを使用して ][install-node-linux])、次のコマンドを実行します。

    npm install azure-cli -g

インストールをテストするには、入力 `azure` コマンド プロンプト。 インストールが成功したときに、使用可能なすべての一覧が表示されます `azure` コマンドです。

## 発行の設定をダウンロードおよびインポートする方法

最初に、発行の設定をダウンロードしてインポートする必要があります。 これにより、Azure サービスを作成および管理するためのツールを使用できるようになります。 ダウンロードする、発行の設定を使用して、 `アカウント ダウンロード` コマンド。

    azure account download

これにより、既定のブラウザーが開き、Azure クラシック ポータルにサインインするよう求められます。 サインインした後、 `.publishsettings` ファイルをダウンロードします。 このファイルを保存した場所をメモしておきます。

次に、インポート、 `.publishsettings` ファイルは、次のコマンドを実行して置き換える `< パスの設定-ファイル >` へのパスで、 `.publishsettings` ファイル。

    azure account import <path-to-settings-file>

保存された情報のすべてを削除することができます、 <code>インポート</code> コマンドを使用する、 <code>クリア アカウント</code> コマンド。

    azure account clear

オプションの一覧を表示する `アカウント` コマンドを使用して、 `-ヘルプ` オプション。

    azure account -help

インポートした後、発行の設定を削除する必要があります、 `.publishsettings` セキュリティ強化のためのファイルです。 詳細については、[Mac および Linux 用 Azure コマンド ライン ツールをインストールする方法] を参照してください。 これで、コマンド ラインまたはバッチ ファイルを使用して Azure Mobile Services の作成および管理を開始する準備が整いました。

## 新しい Mobile Services を作成する方法

コマンド ライン ツールを使用して、新しいモバイル サービス インスタンスを作成することができます。 モバイル サービスを作成する間に、SQL Database インスタンスを新しいサーバーに作成することもできます。

次のコマンドは、サブスクリプション内で新しいモバイル サービス インスタンスを作成、 `< サービス名 >` 、新しいモバイル サービスの名前を指定 `< サーバー管理者 >` 新しいサーバーのログイン名を指定し、 `< サーバー パスワード >` 新しいログインのパスワードを指定します。

    azure mobile create <service-name> <server-admin> <server-password>

`Mobile 作成` コマンドが指定されたモバイル サービスが存在する場合に失敗します。 自動化スクリプト内では、モバイル サービスの再作成を試行する前に、そのモバイル サービスを削除する必要があります。

## サブスクリプション内の既存の Mobile Services をリストする方法

> [AZURE.NOTE] "リスト" と "スクリプト" に関連する CLI でのコマンドは、JavaScript バックエンドでのみ機能します。

次のコマンドでは、Azure サブスクリプション内のすべてのモバイル サービスのリストが返されます。

    azure mobile list

このコマンドでは、各モバイル サービスの現在の状態と URL も表示されます。

## 既存の Mobile Services を削除する方法

コマンド ライン ツールを使用して、関連する SQL Database およびサーバーと共に既存のモバイル サービスを削除することができます。次のコマンドは、モバイル サービスを削除、 `< サービス名 >` 削除するモバイル サービスの名前を指定します。

    azure mobile delete <service-name> -a -q

含めることによって `-a` と `-q` パラメーターを次のコマンド、SQL データベースやサーバーも削除するプロンプトを表示することも、モバイル サービスによって使用されます。
> [AZURE.NOTE] 指定しない場合、 <code>-q</code> またはと共にパラメーター <code>-a</code> または <code>-d</code>, 、実行が一時停止され、SQL Database の削除オプションを選択するように求められます。 のみを使用して、 <code>-a</code> 他のサービスを使用するデータベースまたはサーバーのパラメーター。 それ以外の場合の使用、 <code>-d</code> パラメーターを削除するモバイル サービスに属するデータのみを削除します。

## Mobile Services にテーブルを作成する方法

次のコマンドは、指定されたモバイル サービスでテーブルを作成、 `< サービス名 >` モバイル サービスの名前を指定し、 `< テーブル名 >` を作成するテーブルの名前を指定します。

    azure mobile table create <service-name> <table-name>

既定のアクセス許可で新しいテーブルを作成 `アプリケーション`, 、テーブル操作: `挿入`, 、`読み取り`, 、`更新`, と `削除`します。

次のコマンドは、パブリックで、新しいテーブルを作成 `読み取り` アクセス許可が、 `削除` 管理者のみに付与されたアクセス許可。

    azure mobile table create <service-name> <table-name> -p read=public,delete=admin

次の表は、[Azure クラシック ポータル] で、アクセス許可値と比較して、スクリプトのアクセス許可値を示します。

|スクリプト値 |ポータルの値 |
|========|========|
|`パブリック`|Everyone|
|`アプリケーション`(既定値) |アプリケーション キーを持つユーザーを持つユーザー
|`user`|認証されたユーザーのみ | のみ
|`admin`|スクリプトと管理者のみ | のみ

`モバイル テーブル作成` コマンドは、指定されたテーブルが既に存在する場合に失敗します。 自動化スクリプト内では、テーブルの再作成を試行する前に、そのテーブルを削除する必要があります。

## Mobile Services 内の既存のテーブルをリストする方法

次のコマンドは、モバイル サービスでのすべてのテーブルの一覧を返します場所 `< サービス名 >` モバイル サービスの名前を指定します。

    azure mobile table list <service-name>

このコマンドでは、各テーブルのインデックスの数と、テーブル内に現在存在するデータ行の数も表示されます。

## Mobile Services から既存のテーブルを削除する方法

次のコマンドは、モバイル サービスからテーブルを削除、 `< サービス名 >` モバイル サービスの名前を指定し、 `< テーブル名 >` を削除するテーブルの名前を指定します。

    azure mobile table delete <service-name> <table-name> -q

自動化スクリプト内で使用して、 `-q` 実行をブロックする確認プロンプトを表示せずにテーブルを削除するパラメーターです。

## テーブル操作にスクリプトを登録する方法

次のコマンドをアップロードして、テーブルに対する操作に関数を登録場所 `< サービス名 >` モバイル サービスの名前を指定 `< テーブル名 >` 、テーブルの名前を指定と `< 操作 >` になる可能性がテーブル操作は、 `読み取り`, 、`挿入`, 、`更新`, 、または `削除`:

    azure mobile script upload <service-name> table/<table-name>.<operation>.js

この操作では、JavaScript (.js) ファイルがローカル コンピューターからアップロードされます。 テーブルと操作の名前でファイルの名前を構成する必要があり、これに配置する必要があります、 `テーブル` コマンドを実行する場所のサブフォルダーです。 たとえば、次の操作アップロードして登録する新しい `挿入` に対するスクリプトが、 `TodoItems` テーブル。

    azure mobile script upload todolist table/todoitems.insert.js

スクリプト ファイル内の関数宣言は、登録されているテーブル操作にも一致する必要があります。 つまり、 `挿入` スクリプト、アップロードされたスクリプトには、次のシグネチャを持つ関数が含まれています。

    function insert(item, user, request) {
        ...
    }

スクリプトの登録の詳細については、[モバイル サービスのサーバー スクリプト リファレンス] を参照してください。
















[download and install the command-line tools]: #install 
[download and import publish settings]: #import 
[create a new mobile service]: #create-service 
[get the master key]: #get-master-key 
[create a new table]: #create-table 
[register a new table script]: #register-script 
[delete an existing table]: #delete-table 
[delete an existing mobile service]: #delete-service 
[test the mobile service]: #test-service 
[list mobile services]: #list-services 
[list tables]: #list-tables 
[next steps]: #next-steps 
[mobile services server script reference]: http://go.microsoft.com/fwlink/p?LinkId=262293 
[azure classic portal]: https://manage.windowsazure.com/ 
[nodejs-org]: http://nodejs.org/ 
[install-node-linux]: https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager 
[mac-installer]: http://go.microsoft.com/fwlink/p?LinkId=252249 
[windows-installer]: http://go.microsoft.com/fwlink/p?LinkID=275464 
[reference-docs]: http://azure.microsoft.com/documentation/articles/virtual-machines-command-line-tools/#Commands_to_manage_mobile_services 
[how to install the azure command-line tools for mac and linux]: http://go.microsoft.com/fwlink/p/?LinkId=275795 

