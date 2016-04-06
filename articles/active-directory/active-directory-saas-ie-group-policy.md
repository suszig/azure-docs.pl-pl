<properties
   pageTitle="グループ ポリシーを使用して Internet Explorer 用アクセス パネル拡張機能をデプロイする方法 | Microsoft Azure"
   description="グループ ポリシーを使用してマイ アプリ ポータル用の Internet Explorer アドオンをデプロイする方法。"
   services="active-directory"
   documentationCenter=""
   authors="liviodlc"
   manager="stevenpo"
   editor=""/>
<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="11/18/2015"
   ms.author="liviodlc"/>

#グループ ポリシーを使用して Internet Explorer 用アクセス パネル拡張機能をデプロイする方法

このチュートリアルでは、グループ ポリシーを使用して Internet Explorer 用のアクセス パネル拡張機能をユーザーのコンピューターにリモートでインストールする方法を示します。 この拡張機能を使用して構成されているアプリにサインインする必要がある Internet Explorer のユーザーに必要な [シングル サインオンをパスワードに基づく](active-directory-appssoaccess-whatis.md#password-based-single-sign-on)します。

管理者にはこの拡張機能のデプロイメントを自動化することをお勧めします。 自動化しないと、ユーザーは自分で拡張機能をダウンロードしてインストールする必要があるので、ユーザー エラーが発生しやすく、管理者のアクセス許可が必要になります。 このチュートリアルでは、グループ ポリシーを使用してソフトウェアのデプロイメントを自動化する方法について説明します。 [グループ ポリシーの詳細についてはこちらを参照してください。](https://technet.microsoft.com/windowsserver/bb310732.aspx)

アクセス パネルの拡張機能が可能な [Chrome](https://go.microsoft.com/fwLink/?LinkID=311859) と [Firefox](https://go.microsoft.com/fwLink/?LinkID=626998), 、インストールする管理者のアクセス許可が必要なのです。

##前提条件

- 設定している [Active Directory ドメイン サービス](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx), 、ユーザーのコンピューターをドメインに参加しているとします。
- グループ ポリシー オブジェクト (GPO) を編集するには、"設定の編集" アクセス許可が必要です。 既定では、Domain Administrators、Enterprise Administrators、Group Policy Creator Owners の各セキュリティ グループ メンバーにはこのアクセス許可があります。 [詳細情報。](https://technet.microsoft.com/library/cc781991%28v=ws.10%29.aspx)

##手順 1: 配布ポイントを作成する

最初に、拡張機能をリモートでインストールするすべてのコンピューターからアクセスできるネットワークの場所に、インストーラー パッケージを置く必要があります。 そのためには、次の手順に従います。

1. サーバーに管理者としてログオンします。

2.  **サーバー マネージャー** ウィンドウ **ファイルおよび記憶域サービス**します。

    ![[ファイル サービスおよびストレージ サービス] を開く](./media/active-directory-saas-ie-group-policy/files-services.png)

3. 移動して、 **共有** ] タブをクリックします。 [ **タスク** > **新しい共有しています.**

    ![[ファイル サービスおよびストレージ サービス] を開く](./media/active-directory-saas-ie-group-policy/shares.png)

4. 完了、 **新しい共有ウィザード** し、ユーザーのマシンからアクセスできることを確認するアクセス許可を設定します。 [共有の詳細についてはこちらを参照してください。](https://technet.microsoft.com/library/cc753175.aspx)

5. 次の Microsoft Windows インストーラー パッケージ (.msi ファイル) をダウンロードします [アクセス パネル Extension.msi] (https://account.activedirectory.windowsazure.com/Applications/Installers/x64/Access パネル Extension.msi)。

6. インストーラー パッケージを共有上の目的の場所にコピーします。

    ![.msi ファイルを共有にコピーする](./media/active-directory-saas-ie-group-policy/copy-package.png)

8. クライアント コンピューターが共有のインストーラー パッケージにアクセスできることを確認します。 

##手順 2: グループ ポリシー オブジェクトを作成する

1. Active Directory ドメイン サービス (AD DS) のインストールをホストしているサーバーにログオンします。

2. サーバー マネージャーで、 **ツール** > **Group Policy Management**します。

    ![[ツール] > [グループ ポリシーの管理] に移動する](./media/active-directory-saas-ie-group-policy/tools-gpm.png)

3. 左側のウィンドウで、 **Group Policy Management** ウィンドウでは、組織単位 (OU) 階層を表示して、スコープにするには、グループ ポリシーを適用するかを決定します。 たとえば、テストのために数ユーザーにデプロイする場合は小さい OU を選択し、組織全体にデプロイする場合は最上位レベルの OU を選択します。

    > [AZURE.NOTE] 作成または編集組織単位 (Ou)、サーバー マネージャーに戻る切り替えるおよびに移動したい場合 **ツール** > **Active Directory ユーザーとコンピューター**します。

4. OU を選択するを右クリックして選択 **このドメインに GPO を作成し、ここにリンクしています.**

    ![新しい GPO を作成する](./media/active-directory-saas-ie-group-policy/create-gpo.png)

5.  **新しい GPO** プロンプトで、新しいグループ ポリシー オブジェクトの名前を入力します。

    ![新しい GPO の名前を設定する](./media/active-directory-saas-ie-group-policy/name-gpo.png)

6. グループ ポリシー オブジェクトを作成したを右クリックして **編集**します。

    ![新しい GPO を編集する](./media/active-directory-saas-ie-group-policy/edit-gpo.png)

##手順 3: インストール パッケージを割り当てる

1. 基づく拡張機能を配置するかどうかを判断 **コンピューターの構成** または **ユーザーの構成**します。 使用する場合 [コンピューターの構成](https://technet.microsoft.com/library/cc736413%28v=ws.10%29.aspx), 、拡張機能にどのユーザーがログオンに関係なく、コンピューターにインストールされます。 その一方で、 [ユーザーの構成](https://technet.microsoft.com/library/cc781953%28v=ws.10%29.aspx), 、ユーザーには、どのコンピューターにログオンしてインストールされている拡張子が付きます。

2. 左側のウィンドウで、 **グループ ポリシー管理エディター** ウィンドウで、選択した構成の種類に応じて、次のフォルダー パスのいずれかに移動します。
    - `Computer Configuration/Policies/Software Settings/`
    - `User Configuration/Policies/Software Settings/`

3. 右クリックして **ソフトウェアのインストール**, 選択してから、 **新規** > **パッケージしています.**

    ![新しいソフトウェア インストール パッケージを作成する](./media/active-directory-saas-ie-group-policy/new-package.png)

4. インストーラー パッケージを含む共有フォルダーに移動 [手順 1: 配布ポイントを作成](#step-1-create-the-distribution-point), .msi ファイルを選択してクリックして **開く**します。

    > [AZURE.IMPORTANT] これと同じサーバーには、共有が存在する場合は、ローカル ファイルのパスではなく、ネットワーク ファイルのパスを介して、.msi をアクセスしていることを確認します。

    ![共有フォルダーからインストール パッケージを選択する](./media/active-directory-saas-ie-group-policy/select-package.png)

5.  **ソフトウェアの展開** プロンプト、[ **割り当て** 、その展開方法です。 クリックして **OK**します。

    ![[割り当て] を選択し、[OK] をクリックする](./media/active-directory-saas-ie-group-policy/deployment-method.png)

選択した OU に拡張機能がデプロイされます。 [グループ ポリシー ソフトウェアのインストールの詳細についてはこちらを参照してください。](https://technet.microsoft.com/library/cc738858%28v=ws.10%29.aspx)

##手順 4: Internet Explorer の拡張機能を自動的に有効にする 

Internet Explorer のすべての拡張機能は、インストーラーを実行するだけでなく、明示的に有効にしてからでないと使用できません。 グループ ポリシーを使用してアクセス パネル拡張機能を有効にするのには、次の手順に従います。

1.  **グループ ポリシー管理エディター** ] ウィンドウで選択した構成の種類に応じて、次のパスのいずれかに移動して [手順 3: インストール パッケージを割り当てる](#step-3-assign-the-installation-package):
    - `Computer Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/Security Features/Add-on Management`
    - `User Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/Security Features/Add-on Management`

2. 右クリックして **アドオンの一覧**, を選択して **編集**します。
    ![アドオンの一覧を編集します。](./media/active-directory-saas-ie-group-policy/edit-add-on-list.png)

3.  **アドオンの一覧** ウィンドウで、 **有効**します。 その後、下にある、 **オプション** ] をクリックして **[表示...]**します。

    ![[有効] をクリックし、[表示...] をクリックする](./media/active-directory-saas-ie-group-policy/edit-add-on-list-window.png)

4.  **内容の表示** ウィンドウで、次の手順を実行します。

    1. 最初の列 (、 **値名** フィールド) をコピーし、次のクラス ID を貼り付けます。 `{030E9A3F-7B18-4122-9A60-B87235E4F59E}`

    2. 2 列目の (、 **値** フィールド)、次の値を入力します。 `1`

    3. をクリックして **OK** を閉じる、 **内容の表示** ウィンドウです。

    ![上で指定した値を入力する](./media/active-directory-saas-ie-group-policy/show-contents.png)

5. をクリックして **OK** 、変更を適用し、閉じる、 **アドオンの一覧** ウィンドウです。

選択した OU のコンピューターで拡張機能が有効になります。 [グループ ポリシーを使用して Internet Explorer のアドオンを有効または無効にする方法の詳細については、こちらを参照してください。](https://technet.microsoft.com/library/dn454941.aspx)

##手順 5 (省略可能): パスワードを保存するかどうかを確認するプロンプトを無効にする

ユーザーがアクセス パネル拡張機能を使用してサインインするときに、Internet Explorer でパスワードを保存するかどうかを確認する次のプロンプトが表示されることがあります。

![](./media/active-directory-saas-ie-group-policy/remember-password-prompt.png)

ユーザーに対してこのメッセージが表示されないようにする場合は、次の手順に従って、パスワードの保存によるオートコンプリートを禁止します。

1.  **グループ ポリシー管理エディター** ウィンドウで、以下のパスに移動します。 この構成設定はいるのみで使用可能な **ユーザーの構成**します。
    - `User Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/`

2. という名前の設定を見つけ、 **フォームのユーザー名とパスワードのオートコンプリート機能を有効にする**です。

    > [AZURE.NOTE] 以前のバージョンの Active Directory は、名前を使用してこの設定を一覧表示可能性があります **オートコンプリートにパスワードの保存を許可しない**します。 その設定の構成は、このチュートリアルで説明する設定とは異なります。

    ![これは [ユーザー設定] から探すことに注意](./media/active-directory-saas-ie-group-policy/disable-auto-complete.png)

3. 上記の設定を右クリックし、選択 **編集**します。

4. というタイトルのウィンドウで **フォームのユーザー名とパスワードのオートコンプリート機能を有効にする**, [ **無効になっている**します。

    ![[無効] を選択](./media/active-directory-saas-ie-group-policy/disable-passwords.png)

5. クリックして **OK** をこれらの変更を適用し、ウィンドウを閉じます。

これで、ユーザーは資格情報を保存することも、オートコンプリートを使用して前に保存された資格情報にアクセスすることもできなくなります。 ただし、このポリシーでは、他の種類のフォーム フィールド (検索フィールドなど) でのオートコンプリートは引き続き許可されます。

> [AZURE.WARNING] このポリシーは、いくつか資格情報を格納するユーザーが選択した後、このポリシーが有効な場合 *いない* 既に保存されている資格情報をオフにします。

##手順 6: デプロイのテスト

拡張機能のデプロイメントが成功したかどうかを確認するには、次の手順に従います。

1. 使用して展開されている場合 **コンピューターの構成**, で選択した OU に属しているクライアント コンピューターにサインオン [手順 2: グループ ポリシー オブジェクトを作成](#step-2-create-the-group-policy-object)します。 使用して展開されている場合 **ユーザーの構成**, 、その OU に属するユーザーとしてサインインすることを確認します。

2. そのコンピューターでグループ ポリシーの変更が完全に更新されるには、2 回サインインする必要がある場合があります。 は強制的に更新を開き、 **コマンド プロンプト** ウィンドウと、次のコマンドを実行します。 `gpupdate /force`

3. インストールが行われるには、コンピューターを再起動する必要があります。 起動には、通常の拡張機能のインストールよりかなり長い時間がかかる可能性があります。

4. 再起動後は、開く **Internet Explorer**します。 ウィンドウの右上隅で、[上] をクリックして **ツール** (歯車のアイコン) をクリックして **アドオンを管理する**です。

    ![[ツール] > [アドオンの管理] に移動する](./media/active-directory-saas-ie-group-policy/manage-add-ons.png)

5.  **アドオンの管理** ウィンドウで、いることを確認、 **アクセス パネルの拡張機能** がインストールされていることと、 **ステータス** に設定されている **有効**します。

    ![Access Panel Extension がインストールされ、有効になっていることを確認する](./media/active-directory-saas-ie-group-policy/verify-install.png)

[AZURE.INCLUDE [saas-toc](../../includes/active-directory-saas-toc.md)]

