<properties
    pageTitle="ソース管理への JavaScript バックエンド プロジェクト コードの保存 | Azure Mobile Services"
    description="コンピューターのローカル Git リポジトリにサーバー スクリプト ファイルとモジュールを格納する方法について説明します。"
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
- [.NET backend](mobile-services-dotnet-backend-store-code-source-control.md)
- [Javascript backend](mobile-services-store-scripts-source-control.md)


このトピックでは、Azure Mobile Services で提供されているソース管理を使用してサーバー スクリプトを保存する方法を示します。 スクリプトとその他の JavaScript バンクエンド コード ファイルは、ローカル Git リポジトリから運用モバイル サービスに昇格させることができます。 さらに、複数のスクリプトで必要になる場合がある共有コードを定義する方法と、package.json ファイルを使用して Node.js モジュールをモバイル サービスに追加する方法も説明します。

このチュートリアルを完了する必要がありますが既に作成したモバイル サービスを実行して、[モバイル サービスの開始を取得] チュートリアルです。

## <a name="enable-source-control"></a>モバイル サービスでソース管理を有効にします。

[AZURE.INCLUDE [mobile-services-enable-source-control](../../includes/mobile-services-enable-source-control.md)]

## <a name="clone-repo"></a>Git をインストールし、ローカル リポジトリを作成します。

1. ローカル コンピューターに Git をインストールします。

    Git をインストールするために必要な手順は、オペレーティング システムによって異なります。 オペレーティング システム固有の配布とインストールのガイダンスについては、[Git のインストール] を参照してください。
    > [AZURE.NOTE]
    > オペレーティング システムによっては、コマンド ラインと GUI の両方のバージョンの Git を使用できます。 この記事で説明する手順では、コマンド ライン バージョンを使用します。

2. **GitBash** (Windows) や **Bash** (Unix シェル) などのコマンド ラインを開きます。 OS X システムでは、**ターミナル** アプリケーションを使用してコマンド ラインにアクセスできます。

3. コマンド ラインで、スクリプトを格納するディレクトリに移動します。 たとえば、 `cd SourceControl`します。

4. 次のコマンドを使用して、新しい Git リポジトリのローカル コピーを作成する置換 `< your_git_URL >` をモバイル サービスの Git リポジトリの URL:

        git clone <your_git_URL>

5. ユーザー名とパスワードの指定を求めるメッセージが表示されたら、モバイル サービスのソース管理を有効にしたときに設定したユーザー名とパスワードを入力します。 認証が成功すると、次のような一連の応答が表示されます。

        remote: Counting objects: 8, done.
        remote: Compressing objects: 100% (4/4), done.
        remote: Total 8 (delta 1), reused 0 (delta 0)
        Unpacking objects: 100% (8/8), done.

6. 実行したディレクトリに移動、 `git クローン` コマンド、および次のディレクトリ構造を確認します。

    ![4][4]

    ここでは、新しいディレクトリがモバイル サービスの名前で作成されています。これが、データ サービスのローカル リポジトリになります。

7. .\service\table サブフォルダーを開くと、TodoItem.json ファイルが含まれていることがわかります。これは、TodoItem テーブルに対する操作のアクセス許可の JSON 表現です。

    サーバー スクリプトは、このテーブルに定義した後にという名前の 1 つまたは複数のファイルにはも <code>TodoItem_。< 操作 >_.js</code> 、特定のテーブル操作のスクリプトが含まれます。 Scheduler スクリプトとカスタム API スクリプトは、各スクリプトの名前が付けられたフォルダーに別々に格納されます。 詳細については、次を参照してください。 [ソース管理]。

これでローカル リポジトリが作成できたので、サーバー スクリプトを変更して、モバイル サービスに変更をプッシュ バックできます。

## <a name="deploy-scripts"></a>更新されたスクリプト ファイル、モバイル サービスをデプロイします。

1. .\service\table サブフォルダーに移動し、まだファイル todoitem.insert.js が存在しない場合は、ここで作成します。

2. テキスト エディターで新しいファイル todoitem.insert.js を開き、次のコードを貼り付けて変更を保存します。

        function insert(item, user, request) {
            request.execute();
            console.log(JSON.stringify(item, null, 4));
        }

    このコードは、挿入された項目を単にログに書き込みます。 このファイルが既にコードが含まれる場合への呼び出しなど、このファイルに有効な JavaScript コードを追加するだけ `console.log()`, 、変更を保存します。

3. Git コマンド プロンプトで次のコマンドを入力し、新しいスクリプト ファイルの追跡を開始します。

        $ git add .

4. 次のコマンドを入力し、変更をコミットします。

        $ git commit -m "updated the insert script"

5. 次のコマンドを入力し、変更をリモート リポジトリにアップロードします。

        $ git push origin master

    コミットがモバイル サービスにデプロイされることを示す一連のコマンドが表示されるはずです。

6. ポータルに戻り、[Azure クラシック] をクリックして、 **データ** ] タブのをクリックして、 **TodoItem** テーブルで、をクリックして  **スクリプト**, 選択してから、 **挿入** 操作します。 表示された挿入操作のスクリプトが、直前にリポジトリにアップロードした JavaScript コードと同じであることを確認します。

## <a name="use-npm"></a>共有コードとサーバー スクリプトでの Node.js モジュールを活用します。

Mobile Services では、すべての Node.js コア モジュールにアクセスでき、コード内で **require** 関数を使用することでそのモジュールを利用することができます。 さらに、Node.js コア パッケージに含まれない Node.js モジュールも使用できるだけでなく、自分で作成した共有コードを Node.js モジュールとして定義することもできます。 モジュールの作成についての詳細については、[モジュール] Node.js API リファレンス ドキュメントを参照してください。

Node.js モジュールをモバイル サービスに追加するための推奨される方法では、リファレンスをサービスの package.json ファイルに追加します。 次に、package.json ファイルを更新することで、[node-uuid] Node.js モジュールをモバイル サービスに追加されます。 更新が Azure にプッシュされると、モジュール サービスが再起動され、モジュールがインストールされます。 次に、このモジュールを使用して、挿入された項目の **uuid** プロパティに対応する新しい GUID 値を生成します。

2. 移動し、 `. \service` フォルダー、ローカルの Git リポジトリのおよびオープンし package.json ファイルをテキスト エディターで、次のフィールドを追加、 **の依存関係** オブジェクト。

        "node-uuid": "~1.4.3"

    >[AZURE.NOTE]このように package.json ファイルが更新されると、コミットをプッシュしたときにモバイル サービスが再起動します。

4. .\service\table サブフォルダーに移動し、todoitem.insert.js ファイルを開いて、次のように変更します。

        function insert(item, user, request) {
            var uuid = require('node-uuid');
            item.uuid = uuid.v1();
            request.execute();
            console.log(item);
        }

    このコードはテーブルに uuid 列を追加し、一意な GUID 識別子を設定します。

5. 前のセクションと同様に、Git コマンド プロンプトで次のコマンドを入力します。

        $ git add .
        $ git commit -m "added node-uuid module"
        $ git push origin master

    これで、新しいファイルが追加され、変更がコミットされ、新しい node-uuid モジュールと todoitem.insert.js スクリプトの変更がモバイル サービスにプッシュされます。

## <a name="next-steps"> </a>次のステップ

このチュートリアルでは、スクリプトをソース管理に保存する方法について説明しました。 サーバー スクリプトとカスタム API の操作の詳細については、以下を参照してください。

+ [モバイル サービスでサーバー スクリプトを使用]
    <br/>サーバー スクリプト、ジョブ スケジューラ、およびカスタム Api を使用する方法を示します。





[enable source control in your mobile service]: #enable-source-control 
[install git and create the local repository]: #clone-repo 
[deploy updated script files to your mobile service]: #deploy-scripts 
[leverage shared code and node.js modules in your server scripts]: #use-npm 
[4]: ./media/mobile-services-store-scripts-source-control/mobile-source-local-repo.png 
[5]: ./media/mobile-services-store-scripts-source-control/mobile-portal-data-tables.png 
[6]: ./media/mobile-services-store-scripts-source-control/mobile-insert-script-source-control.png 
[git website]: http://git-scm.com 
[source control]: http://msdn.microsoft.com/library/windowsazure/c25aaede-c1f0-4004-8b78-113708761643 
[installing git]: http://git-scm.com/book/en/Getting-Started-Installing-Git 
[get started with mobile services]: mobile-services-ios-get-started.md 
[work with server scripts in mobile services]: mobile-services-how-to-use-server-scripts.md 
[azure classic portal]: https://manage.windowsazure.com/ 
[modules]: http://nodejs.org/api/modules.html 
[node-uuid]: https://npmjs.org/package/node-uuid 

