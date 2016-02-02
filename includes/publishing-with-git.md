[Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) BitBucket、CodePlex、Dropbox、Git、GitHub、Mercurial、TFS などのソース コード管理やリポジトリ ツールから Web アプリの継続的なデプロイをサポートしています。 これらのツールを使用すると、アプリケーションのコンテンツとコードをメンテナンスすることができ、必要なときに変更を Azure Web アプリにすばやく簡単にプッシュできます。

この記事では、Git を使用してローカル コンピューターから Web Apps に直接発行する方法について説明します (Azure では、この発行方法は**ローカル Git** と呼ばれます)。 また、BitBucket、CodePlex、Dropbox、GitHub、Mercurial などのリポジトリ サイトからの継続的なデプロイを有効にする方法についても説明します。 継続的なデプロイに TFS を使用する方法の詳細については、[Visual Studio チームのサービスを使用して Azure に継続的な配信] を参照してください。
> [AZURE.NOTE] この記事で説明されている Git コマンドの多くは自動的に実行を使用して web アプリを作成するときに、 [Mac および Linux 用 Azure コマンド ライン ツール](/develop/nodejs/how-to-guides/command-line-tools/)します。

## <a id="Step1"></a>手順 1: Git のインストール

Git をインストールするために必要な手順は、オペレーティング システムによって異なります。 オペレーティング システム固有の配布とインストールのガイダンスについては、[Git のインストール] を参照してください。
> [AZURE.NOTE] オペレーティング システムによっては、コマンド ラインと GUI の両方のバージョンの Git を使用できます。 この記事で説明する手順では、コマンド ライン バージョンを使用します。

## <a id="Step2"></a>手順 2: ローカル リポジトリを作成します。

次のタスクを実行して、新しい Git リポジトリを作成します。

1. Git リポジトリと Web アプリのファイルを格納するディレクトリを MyGitRepository という名前で作成します。

2. **GitBash** (Windows) や **Bash** (Unix シェル) などのコマンド ライン ツールを開きます。 OS X システムでは、**ターミナル** アプリケーションを使用してコマンド ラインにアクセスできます。

3. コマンド ラインで、MyGitRepository ディレクトリに移動します。

        cd MyGitRepository

4. 次のコマンドを使用して、新しい Git リポジトリを初期化します。

        git init

    このコマンドでは、"**Initialized empty Git repository in [path]**" などのメッセージが返されます。

## <a id="Step3"></a>手順 3: web ページを追加します。

Web Apps では、さまざまなプログラミング言語で作成されたアプリケーションをサポートしています。 この例では、静的 .html ファイルを使用します。

1. テキスト エディターを使用して、Git リポジトリ (先ほど作成した MyGitRepository ディレクトリ) のルートに **index.html** という名前の新しいファイルを作成します。

2. index.html ファイルの内容として次のテキストを追加し、ファイルを保存します。

        Hello Git!

3. コマンド ラインで、現在の位置が Git リポジトリのルートであることを確認します。 その後、次のコマンドを使用して **index.html** ファイルをリポジトリに追加します。

        git add index.html 

    > [AZURE.NOTE] git コマンドのヘルプを見つけるには、コマンドの後に「-help」または「--help」と入力します。 たとえば、add コマンドのパラメーターのオプションを見つけるには、「git add -help」と入力します。より詳細なヘルプを見つけるには、「git add --help」と入力します。

4. 次に、次のコマンドを使用して、リポジトリへの変更をコミットします。

        git commit -m "Adding index.html to the repository"

    次のような出力が表示されます。

        [master (root-commit) 369a79c] Adding index.html to the repository
         1 file changed, 1 insertion(+)
         create mode 100644 index.html


## <a id="Step4"></a>Web アプリのリポジトリを有効にします。

次に示している手順を実行して、Web アプリに対して Git リポジトリを有効にします。

1. [Azure ポータル] にログインします。

2. Web アプリのブレードで、**[設定]、[継続的なデプロイ]** の順にクリックします。 **[ソースの選択]** をクリックし、**[ローカル Git リポジトリ]** をクリックして **[OK]** をクリックします。

    ![ローカルの Git リポジトリ](./media/publishing-with-git/azure1-local-git.png)

4. Azure で初めてリポジトリを設定した場合、そのログイン資格情報を作成する必要があります。 それらを使用して、Azure リポジトリにログインし、ローカル Git リポジトリから変更をプッシュします。 Web アプリのブレードで **[設定]、[デプロイ資格情報]** の順にクリックし、デプロイ用のユーザー名とパスワードを構成します。 完了したら **[OK]** をクリックします。

    ![](./media/publishing-with-git/azure2-credentials.png)

## <a id="Step5"></a>プロジェクトをデプロイします。

* [ローカル ファイル (ローカル Git) を Azure へのプッシュ](#Step6)
* [BitBucket、CodePlex、Dropbox、GitHub、または Mercurial などのリポジトリ web サイトからのファイルをデプロイします。](#Step7)
* [BitBucket、CodePlex、Dropbox、GitHub、または Mercurial からの Visual Studio ソリューションをデプロイします。](#Step75)

次に示している手順に従って、ローカル Git を使用して Web アプリを Azure に発行します。

1. Web アプリのブレードで、**[設定]、[プロパティ]** の順にクリックし、**[Git の URL]** を確認します。

    ![](./media/publishing-with-git/azure3-repo-details.png)

    **Git URL** は、ローカル リポジトリからデプロイする際のリモート参照です。 この URL は次の手順で使用します。

1. コマンド ラインで現在の位置が、先ほど index.html ファイルを作成したローカル Git リポジトリのルートであることを確認します。

2. 使用 `git リモート` にリストされたリモート参照を追加する **Git URL** 手順 1. のです。 コマンドは次のようになります。

        git remote add azure https://username@needsmoregit.scm.azurewebsites.net:443/NeedsMoreGit.git

    > [AZURE.NOTE] この **remote** コマンドは、名前付きの参照をリモート リポジトリに追加します。 この例では、Web アプリのリポジトリに「azure」という名前の参照を作成しています。

1. コマンド ラインで次のコマンドを使用して、ローカル リポジトリから "azure" リモートに現在のリポジトリのコンテンツをプッシュします。

        git push azure master

    ポータルでデプロイメント資格情報をリセットしたときに作成したパスワードの入力を求められます。 パスワードを入力します (パスワードを入力しても Gitbash によってコンソールにアスタリスクはエコーされません)。 次のような出力が表示されます。

        Counting objects: 6, done.
        Compressing objects: 100% (2/2), done.
        Writing objects: 100% (6/6), 486 bytes, done.
        Total 6 (delta 0), reused 0 (delta 0)
        remote: New deployment received.
        remote: Updating branch 'master'.
        remote: Preparing deployment for commit id '369a79c929'.
        remote: Preparing files for deployment.
        remote: Deployment successful.
        To https://username@needsmoregit.scm.azurewebsites.net:443/NeedsMoreGit.git
        * [new branch]      master -> master

    > [AZURE.NOTE] Web アプリにプッシュ要求のターゲットが必要ですがに対して作成したリポジトリ、 <strong>マスター</strong> web アプリのコンテンツとしてを使用して、そのリポジトリの分岐します。

2. Azure ポータルの Web アプリのブレードに戻ります。 **[デプロイが見つかりませんでした]** が **[アクティブなデプロイ]** に変わり、直近のプッシュのログ エントリが表示されます。

    ![](./media/publishing-with-git/azure4-deployed.png)

2. Web アプリ ブレードの上部にある **URL** をクリックして、**index.html** がデプロイされていることを確認します。 "こんにちは Git!"を含むページ 表示されます。

    !["Hello Git!" を含む Web ページ][hello-git]

3. テキスト エディターを使用し、**index.html** ファイルに変更を加えて "Yay!" を追加します。次に、ファイルを保存します。

4. コマンド ラインから次のコマンドを使用し、変更を**追加**して**コミット**します。その後で、変更をリモート リポジトリに**プッシュ**します。

        git add index.html
        git commit -m "Celebration"
        git push azure master

    **push** コマンドが完了したら、ブラウザーを更新して (場合によっては、ブラウザーを正しく更新するには Ctrl + F5 キーを押す必要があります)、コミットした最新の変更がページのコンテンツに反映されていることを確認します。

### <a id="Step7"></a>BitBucket、CodePlex、Dropbox、GitHub、または Mercurial などのリポジトリ サイトからのファイルをデプロイします。

手動で更新をプッシュ、ローカル プロジェクトから、Azure の web アプリに BitBucket、CodePlex、Dropbox、GitHub、または継続的なデプロイ プロセスの結果を Mercurial からのデプロイ時に、Azure をプロジェクトから最新の更新にプルする場所は、ローカルの Git を使用してローカル ファイルを Azure にプッシュします。

どちらの方法でも、プロジェクトが Web Apps にデプロイされます。ただし、複数の人がプロジェクトに携わっており、だれが最新の更新を行ったかに関係なく、常に最新バージョンが発行されるようにする場合は、継続的なデプロイが便利です。 また、継続的なデプロイメントは、上記のいずれかのツールをアプリケーションの中央リポジトリとして使用する場合にも便利です。

GitHub、CodePlex、BitBucket からファイルを展開するには、これらのサービスのいずれかにローカル プロジェクトを発行している必要があります。 これらのサービスに、プロジェクトの発行の詳細については、[リポジトリ (GitHub) を作成] を参照してください。 [を使用して付属する Git CodePlex]、[create a Repo 作成 (BitBucket)]、[を使用して Dropbox に Git リポジトリの共有]、または [クイック スタート - Mercurial] です。

1. まず、継続的なデプロイ用に選択したリポジトリに Web アプリのファイルを配置します。

2. ポータルの Web アプリのブレードで、**[設定]、[継続的な配信]** の順にクリックします。 **[ソースの選択]** をクリックし、(たとえば) **[GitHub]** をクリックします。

    ![](./media/publishing-with-git/azure6-setup-github.png)

2. **[継続的なデプロイ]** ブレードで、**[承認]** をクリックした後**、[承認する]** をクリックします。 承認プロセスを完了するため、Azure ポータルからリポジトリ サイトにリダイレクトされます。

4. 完了して Azure ポータルに戻ったら、**[承認]** ブレードで **[OK]** をクリックします。

5. **[継続的なデプロイ]** ブレードで、デプロイ元の組織、プロジェクト、および分岐を選択します。 完了したら **[OK]** をクリックします。

    ![](./media/publishing-with-git/azure7-setup-github-configure.png)
    > [AZURE.NOTE] GitHub または Bitbucket を使用して継続的なデプロイメントを有効にする場合、パブリック プロジェクトとプライベート プロジェクトの両方が表示されます。

Azure によって、選択したリポジトリとの関連付けが作成され、指定された分岐からファイルがプルされます。 このプロセスが完了したら、Web アプリのブレードの **[デプロイ]** セクションに、デプロイが成功したことを示す**[アクティブなデプロイ]** メッセージが表示されます。

7. この時点で、選択したリポジトリから Web アプリにプロジェクトがデプロイされました。 Web アプリがアクティブであることを確認するために、ポータルの上部にある **[URL]** をクリックします。 ブラウザーに Web アプリが表示されます。

8. 継続的なデプロイが選択したリポジトリから実行されていることを確認するために、変更をリポジトリにプッシュします。 リポジトリへのプッシュが完了すると、すぐに Web アプリは更新され、変更が反映されます。 更新がプルされていることは、Web アプリの **[デプロイ]** ブレードで確認できます。

### <a id="Step75"></a>BitBucket、CodePlex、Dropbox、GitHub、または Mercurial からの Visual Studio ソリューションをデプロイします。

Visual Studio ソリューションを Azure App Service の Web Apps にプッシュすることは、単純な index.html ファイルをプッシュすることと同じくらい簡単です。 Web Apps のデプロイ プロセスでは、NuGet 依存関係の復元やアプリケーション バイナリの構築などのすべての詳細が合理化されます。 Git リポジトリでコードのみを維持し、Web Apps デプロイメントで残りを処理する、ソース管理のベスト プラクティスに従うことができます。

Visual Studio ソリューションを Web アプリにプッシュする手順と同様、 [前のセクション](#Step7), ソリューションとリポジトリを次のように構成する。

-   リポジトリのルートを追加、 `.gitignore` ファイルをすべてのファイルとなど、リポジトリから除外するフォルダーを指定し、 `Obj`, 、`Bin`, と `パッケージ` フォルダー (を参照してください [gitignore ドキュメント](http://git-scm.com/docs/gitignore) 情報を書式設定するため)。 次に例を示します。

        [Oo]bj/
        [Bb]in/
        *.user
        /TestResults
        *.vspscc
        *.vssscc
        *.suo
        *.cache
        *.csproj.user
        packages/*
        App_Data/
        /apps
        msbuild.log
        _app/
        nuget.exe

    >[AZURE.NOTE] GitHub を使用する場合、リポジトリの作成時に、必要に応じて Visual Studio 固有の .gitignore ファイルを生成できます。これには、すべての一般的な一時ファイル、ビルド結果などが含まれます。ファイルは、特定のニーズに合わせてカスタマイズできます。

-   .sln ファイルをリポジトリ ルートに入れて、ソリューションのディレクトリ ツリー全体をリポジトリに追加します。

-   Visual Studio ソリューションで [NuGet パッケージの復元を有効にする](http://docs.nuget.org/Consume/Package-Restore) Visual Studio が自動的に失われたパッケージを復元できるようにします。

説明のとおりにリポジトリを設定し、いずれかのオンライン Git リポジトリからの継続的な発行のために Azure の Web アプリを構成したら、Visual Studio で ASP.NET アプリケーションをローカルで開発し、オンライン Git リポジトリに変更をプッシュするだけで、コードを継続的にデプロイできます。

## 継続的なデプロイの無効化

継続的なデプロイメントは、**[デプロイ]** ブレードから無効にできます。 Web アプリのブレードで、**[設定]、[継続的なデプロイ]** の順にクリックします。 次に、**[切断]** をクリックします。

![git-DisconnectFromGitHub](./media/publishing-with-git/azure5-disconnect.png)

別のソースからの発行を設定する場合、確認メッセージで **[はい]** を選択した後、Web アプリのブレードに戻り、**[設定]、[継続的なデプロイ]** の順にクリックします。

## <a id="Step8"></a>トラブルシューティング

Git を使用して Azure の Web アプリに発行する場合に発生する一般的なエラーまたは問題を以下に示します。

****


**症状**: '[siteURL]' にアクセスできません: [scmAddress] に接続できませんでした

**原因**: このエラーは Web アプリが実行されていない場合に発生する可能性があります。

**解決策**: Azure ポータルで Web アプリを起動します。 Git デプロイは Web アプリが実行されていない限り機能しません。


****


**症状**: Couldn't resolve host 'hostname'

**原因**: このエラーは、"azure" リモートを作成するときに入力したアドレス情報が間違っている場合に発生します。

**解像度**: を使用して、 `git リモート-v` すべてのリモートおよび関連付けられている URL を一覧表示するコマンドです。 "azure" リモートの URL が正しいことを確認します。 必要に応じて、このリモートを削除し、正しい URL を使用して再作成します。

****


**症状**: No refs in common and none specified; doing nothing. Perhaps you should specify a branch such as 'master'.

**原因**: このエラーは、git push 操作を実行するときに分岐を指定せず、Git で使用される push.default 値を設定していない場合に発生します。

**解決策**: master 分岐を指定して、もう一度 push 操作を実行します。 次に例を示します。

    git push azure master

****


**症状**: src refspec [branchname] does not match any.

**原因**: このエラーは、"azure" リモートの master 以外の分岐にプッシュしようとした場合に発生します。

**解決策**: master 分岐を指定して、もう一度 push 操作を実行します。 次に例を示します。

    git push azure master

****


**症状**: エラー - 変更がリモート リポジトリにコミットされましたが、Web アプリは更新されていません。

**原因**: このエラーは、必要な追加モジュールを指定する package.json ファイルを含む Node.js アプリケーションをデプロイする場合に発生します。

**解像度**:"npm ERR!"を含むその他のメッセージ このエラーをログに記録された前にする必要があり、エラーに関する追加のコンテキストを提供することができます。 次は既知の原因のこのエラー メッセージと、対応する"npm ERR!" メッセージ:

* **形式が正しくない package.json ファイル**: npm ERR! Couldn't read dependencies.

* **Windows 用のバイナリ配布がないネイティブ モジュール**:

    * npm ERR! \`cmd "/c" "node-gyp rebuild"\` failed with 1

        OR

    * npm ERR! [modulename@version] プレインストール: \'make | |gmake\'


## その他のリソース

* [Azure の PowerShell を使用する方法]
* [Mac および Linux 用 Azure コマンド ライン ツールを使用する方法]
* [Git のドキュメント]
* [プロジェクト Kudu](https://github.com/projectkudu/kudu/wiki)

>[AZURE.NOTE] 場合は、Azure アカウントがサインアップする前に Azure App Service の使用を開始するには、 [App Service の試用](http://go.microsoft.com/fwlink/?LinkId=523751), 、App Service で有効期間の短いスターター web アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

## 変更内容

* Web サイトから App Service への変更のガイドを参照してください: [Azure App Service と既存の Azure サービスへの影響](http://go.microsoft.com/fwlink/?LinkId=529714)


[azure developer center]: http://www.windowsazure.com/en-us/develop/overview/ 
[azure portal]: https://portal.azure.com 
[git website]: http://git-scm.com 
[installing git]: http://git-scm.com/book/en/Getting-Started-Installing-Git 
[how to use powershell for azure]: ../articles/install-configure-powershell.md 
[how to use the azure command-line tools for mac and linux]: ../articles/xplat-cli-install.md 
[git documentation]: http://git-scm.com/documentation 
[portal-select-website]: ./media/publishing-with-git/git-select-website.png 
[git-whereisyoursourcecode]: ./media/publishing-with-git/git-WhereIsYourSourceCode.png 
[git-instructions]: ./media/publishing-with-git/git-instructions.png 
[portal-deployment-credentials]: ./media/publishing-with-git/git-deployment-credentials.png 
[git-choosearepositorytodeploy]: ./media/publishing-with-git/git-ChooseARepositoryToDeploy.png 
[hello-git]: ./media/publishing-with-git/git-hello-git.png 
[yay]: ./media/publishing-with-git/git-yay.png 
[git-githubdeployed]: ./media/publishing-with-git/git-GitHubDeployed.png 
[git-githubdeployed-updated]: ./media/publishing-with-git/git-GitHubDeployed-Updated.png 
[git-disconnectfromgithub]: ./media/publishing-with-git/git-DisconnectFromGitHub.png 
[git-deploymenttrigger]: ./media/publishing-with-git/git-DeploymentTrigger.png 
[create a repo (github)]: https://help.github.com/articles/create-a-repo 
[using git with codeplex]: http://codeplex.codeplex.com/wikipage?title=Using%20Git%20with%20CodePlex&referringTitle=Source%20control%20clients&ProjectName=codeplex 
[create a repo (bitbucket)]: https://confluence.atlassian.com/display/BITBUCKET/Create+an+Account+and+a+Git+Repo 
[quick start - mercurial]: http://mercurial.selenic.com/wiki/QuickStart 
[using dropbox to share git repositories]: https://gist.github.com/trey/2722927 
[continuous delivery to azure using visual studio team services]: ../articles/cloud-services/cloud-services-continuous-delivery-use-vso.md 

