<properties
    pageTitle="ソース管理への .NET バックエンド プロジェクト コードの保存 | Azure Mobile Services"
    description=".NET バックエンド プロジェクトをコンピューターのローカル Git リポジトリに格納し、リポジトリから発行する方法について説明します。"
    services="mobile-services"
    documentationCenter=""
    authors="ggailey777"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="12/07/2015"
    ms.author="glenga"/>

# ソース管理へのモバイル サービス プロジェクト コードの保存

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

&nbsp;


> [AZURE.SELECTOR]
- [.NET バックエンド](mobile-services-dotnet-backend-store-code-source-control.md)
- [Javascript バックエンド](mobile-services-store-scripts-source-control.md)

このトピックでは、.NET バックエンド サービス プロジェクトの格納に Azure のモバイル サービスによって提供される、ソース管理を使用する方法を説明します。 実稼働環境のモバイル サービスに、ローカルの Git リポジトリからアップロードすることで、プロジェクトをパブリッシュできます。

このチュートリアルを完了する必要がありますが既に作成したモバイル サービスいずれかの操作を完了することで、[モバイル サービスの開始を取得] チュートリアルです。

##<a name="enable-source-control"></a>モバイル サービスでソース管理を有効にする

[AZURE.INCLUDE [mobile-services-enable-source-control](../../includes/mobile-services-enable-source-control.md)]

##<a name="clone-repo"></a>Git をインストールし、ローカル リポジトリを作成する

1. ローカル コンピューターに Git をインストールします。

    Git をインストールするために必要な手順は、オペレーティング システムによって異なります。 オペレーティング システム固有の配布とインストールのガイダンスについては、[Git のインストール] を参照してください。

    > [AZURE.NOTE]
    > オペレーティング システムによっては、コマンド ラインと GUI の両方のバージョンの Git を使用できます。 この記事で説明する手順では、コマンド ライン バージョンを使用します。

2. などのコマンド ラインを開く **GitBash** (Windows) または **Bash** (Unix シェル)。 OS X システムで使用してコマンドラインにすることができますアクセス、 **ターミナル** アプリケーションです。

3. コマンド ラインで、スクリプトを格納するディレクトリに移動します。 たとえば、「`cd SourceControl`」のように入力します。

4. 次のコマンドを使用して、新しい Git リポジトリのローカル コピーを作成します。`<your_git_URL>` の部分は、モバイル サービスの Git リポジトリの URL に置き換えます。

        git clone <your_git_URL>

5. ユーザー名とパスワードの指定を求めるメッセージが表示されたら、モバイル サービスのソース管理を有効にしたときに設定したユーザー名とパスワードを入力します。 認証が成功すると、次のような一連の応答が表示されます。

        remote: Counting objects: 8, done.
        remote: Compressing objects: 100% (4/4), done.
        remote: Total 8 (delta 1), reused 0 (delta 0)
        Unpacking objects: 100% (8/8), done.

6. 実行する `git clone` コマンド、およびモバイル サービスの名前で作成された新しいディレクトリの通知を参照します。 .NET バックエンド モバイル サービスでは、git リポジトリははじめは空です。

ローカル リポジトリを作成すると、このリポジトリから .NET バックエンド サービス プロジェクトを発行することができます。

##<a name="deploy-scripts"></a>Git を使用して、プロジェクトを発行する

1. Visual Studio 2013 で新しい .NET バックエンド モバイル サービス プロジェクトを作成するか既存のプロジェクトを新しいローカル リポジトリに移動します。

    クイック テストでは、モバイル サービスのクイック スタート プロジェクトをダウンロードして、このフォルダーに保存します。

2. Packages.config ファイルを残して、すべての NuGet パッケージ フォルダーを削除します。

    モバイル サービスは、packages.confign ファイルに基づいて、NuGet パッケージを自動的に復元します。 また、.gitignore ファイルを定義してパッケージのディレクトリが追加されないようにすることもできます。

3. Git コマンド プロンプトで次のコマンドを入力し、新しいスクリプト ファイルの追跡を開始します。

        $ git add .

4. 次のコマンドを入力し、変更をコミットします。

        $ git commit -m "adding the .NET backend service project"

5. 変更をリモート リポジトリにアップロードするには、次のコマンドを入力し、資格情報を提供します。

        $ git push origin master

    モバイル サービスにプロジェクトがデプロイされ、パッケージが追加され、サービスの再起動することを示す一連のコマンドが表示されます。

6. .NET バックエンドのモバイル サービスでの URL を参照すると、次が表示されます。

    ![Mobile Services のスタートアップ ページ](./media/mobile-services-dotnet-backend-store-code-source-control/mobile-service-startup.png)

モバイル サービス プロジェクトは、ソース管理で管理され、ローカル リポジトリから更新をプッシュすることで、サービスの更新を発行することができます。 SQL データベースを使用する .NET バックエンド モバイル サービスでデータ モデルの変更を行う方法の詳細については、[を .NET バックエンド モバイル サービス データ モデルを変更する方法] を参照してください。

<!-- Anchors. -->

<!-- Images. -->

<!-- URLs. -->
[Git website]: http://git-scm.com
[Source control]: http://msdn.microsoft.com/library/windowsazure/c25aaede-c1f0-4004-8b78-113708761643
[Installing Git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[Get started with Mobile Services]: mobile-services-dotnet-backend-ios-get-started.md
[How to make data model changes to a .NET backend mobile service]: mobile-services-dotnet-backend-how-to-use-code-first-migrations.md

