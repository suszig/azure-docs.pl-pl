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


# グループ ポリシーを使用して Internet Explorer 用アクセス パネル拡張機能をデプロイする方法

このチュートリアルでは、グループ ポリシーを使用して Internet Explorer 用のアクセス パネル拡張機能をユーザーのコンピューターにリモートでインストールする方法を示します。 この拡張機能を使用して構成されているアプリにサインインする必要がある Internet Explorer のユーザーに必要な [シングル サインオンをパスワードに基づく](active-directory-appssoaccess-whatis.md#password-based-single-sign-on)します。

管理者にはこの拡張機能のデプロイメントを自動化することをお勧めします。 自動化しないと、ユーザーは自分で拡張機能をダウンロードしてインストールする必要があるので、ユーザー エラーが発生しやすく、管理者のアクセス許可が必要になります。 このチュートリアルでは、グループ ポリシーを使用してソフトウェアのデプロイメントを自動化する方法について説明します。 [グループ ポリシーについて説明します。](https://technet.microsoft.com/windowsserver/bb310732.aspx)

アクセス パネルの拡張機能が可能な [Chrome](https://go.microsoft.com/fwLink/?LinkID=311859) と [Firefox](https://go.microsoft.com/fwLink/?LinkID=626998), 、インストールする管理者のアクセス許可が必要なのです。

## 前提条件

- 設定している [Active Directory ドメイン サービス](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx), 、ユーザーのコンピューターをドメインに参加しているとします。
- グループ ポリシー オブジェクト (GPO) を編集するには、"設定の編集" アクセス許可が必要です。 既定では、Domain Administrators、Enterprise Administrators、Group Policy Creator Owners の各セキュリティ グループ メンバーにはこのアクセス許可があります。 [詳細について説明します。](https://technet.microsoft.com/library/cc781991%28v=ws.10%29.aspx)

## 手順 1: 配布ポイントを作成する

最初に、拡張機能をリモートでインストールするすべてのコンピューターからアクセスできるネットワークの場所に、インストーラー パッケージを置く必要があります。 そのためには、次の手順に従います。

1. サーバーに管理者としてログオンします。

2. **[サーバー マネージャー]** ウィンドウで、**[ファイルおよび記憶域サービス]** に移動します。

    ![[ファイル サービスおよびストレージ サービス] を開く](./media/active-directory-saas-ie-group-policy/files-services.png)

3. **[共有]** タブに移動します。 **[タスク]**、**[新しい共有...]** の順にクリックします。

    ![[ファイル サービスおよびストレージ サービス] を開く](./media/active-directory-saas-ie-group-policy/shares.png)

4. **[新しい共有ウィザード]** を完了して、ユーザーのコンピューターからアクセスできるようにアクセス許可を設定します。 [共有について説明します。](https://technet.microsoft.com/library/cc753175.aspx)

5. 次の Microsoft Windows インストーラー パッケージ (.msi ファイル) をダウンロードします [アクセス パネル Extension.msi] (https://account.activedirectory.windowsazure.com/Applications/Installers/x64/Access パネル Extension.msi)。

6. インストーラー パッケージを共有上の目的の場所にコピーします。

    ![.msi ファイルを共有にコピーする](./media/active-directory-saas-ie-group-policy/copy-package.png)

8. クライアント コンピューターが共有のインストーラー パッケージにアクセスできることを確認します。

## 手順 2: グループ ポリシー オブジェクトを作成する

1. Active Directory ドメイン サービス (AD DS) のインストールをホストしているサーバーにログオンします。

2. サーバー マネージャーで、**[ツール]**、**[グループ ポリシーの管理]** に移動します。

    ![[ツール] > ](./media/active-directory-saas-ie-group-policy/tools-gpm.png)

3. **[グループ ポリシーの管理**] の左側のウィンドウで組織単位 (OU) 階層を表示し、グループ ポリシーを適用するスコープを決定します。 たとえば、テストのために数ユーザーにデプロイする場合は小さい OU を選択し、組織全体にデプロイする場合は最上位レベルの OU を選択します。
    > [AZURE.NOTE] 組織単位 (OU) を作成または編集する場合は、サーバー マネージャーに戻り、**[ツール]**、**[Active Directory ユーザーとコンピューター]** に移動します。

4. OU を選択して右クリックし、**[このドメインに GPO を作成し、このコンテナーにリンクする...]** を選択します。

    ![新しい GPO を作成する](./media/active-directory-saas-ie-group-policy/create-gpo.png)

5. **[新しい GPO]** プロンプトで、新しいグループ ポリシー オブジェクトの名前を入力します。

    ![新しい GPO の名前を設定する](./media/active-directory-saas-ie-group-policy/name-gpo.png)

6. 作成したグループ ポリシー オブジェクトを右クリックし、**[編集]** を選択します。

    ![新しい GPO を編集する](./media/active-directory-saas-ie-group-policy/edit-gpo.png)

## 手順 3: インストール パッケージを割り当てる

1. **[コンピューターの構成]** と **[ユーザーの構成]** のどちらを基にして拡張機能をデプロイするかを決めます。 使用する場合 [コンピューターの構成](https://technet.microsoft.com/library/cc736413%28v=ws.10%29.aspx), 、拡張機能にどのユーザーがログオンに関係なく、コンピューターにインストールされます。 その一方で、 [ユーザーの構成](https://technet.microsoft.com/library/cc781953%28v=ws.10%29.aspx), 、ユーザーには、どのコンピューターにログオンしてインストールされている拡張子が付きます。

2. **[グループ ポリシー管理エディター]** の左側のウィンドウで、選択した構成の種類に応じて、次のどちらかのフォルダー パスに移動します。
    - `コンピューターの構成/ポリシー/ソフトウェアの設定/`
    - `ユーザーの構成/ポリシー/ソフトウェア設定/`

3. **[ソフトウェアのインストール]** を右クリックして、**[新規]**、**[パッケージ...]** の順に選択します。

    ![新しいソフトウェア インストール パッケージを作成する](./media/active-directory-saas-ie-group-policy/new-package.png)

4. インストーラー パッケージを含む共有フォルダーに移動 [手順 1: 配布ポイントを作成](#step-1-create-the-distribution-point), .msi ファイルを選択してクリックして **開く**します。
    > [AZURE.IMPORTANT] 共有が同じサーバー上に存在する場合は、ローカル ファイル パスではなく、ネットワーク ファイル パスから .msi にアクセスしていることを確認します。

    ![共有フォルダーからインストール パッケージを選択する](./media/active-directory-saas-ie-group-policy/select-package.png)

5. **[ソフトウェアの展開]** プロンプトで、デプロイメント方法として **[割り当て]** を選択します。 次に、 **[OK]** をクリックします

    ![[割り当て] を選択し、](./media/active-directory-saas-ie-group-policy/deployment-method.png)

選択した OU に拡張機能がデプロイされます。 [グループ ポリシー ソフトウェア インストールの詳細を表示します。](https://technet.microsoft.com/library/cc738858%28v=ws.10%29.aspx)

## 手順 4: Internet Explorer の拡張機能を自動的に有効にする

Internet Explorer のすべての拡張機能は、インストーラーを実行するだけでなく、明示的に有効にしてからでないと使用できません。 グループ ポリシーを使用してアクセス パネル拡張機能を有効にするのには、次の手順に従います。

1. **グループ ポリシー管理エディター** ] ウィンドウで選択した構成の種類に応じて、次のパスのいずれかに移動して [手順 3: インストール パッケージを割り当てる](#step-3-assign-the-installation-package):
    - `コンピューター構成/ポリシー/管理用テンプレート/windows コンポーネント/インターネット エクスプ ローラー/セキュリティの機能/アドオンの管理`
    - `ユーザー構成/ポリシー/管理用テンプレート/windows コンポーネント/インターネット エクスプ ローラー/セキュリティの機能/アドオンの管理`

2. 右クリックして **アドオンの一覧**, を選択して **編集**します。
    ![アドオンの一覧を編集する](./media/active-directory-saas-ie-group-policy/edit-add-on-list.png)

3. **[アドオンの一覧]** ウィンドウで、**[有効]** を選択します。 次に、**[オプション]** セクションで **[表示...]** をクリックします。

    ![[有効] をクリックし、](./media/active-directory-saas-ie-group-policy/edit-add-on-list-window.png)

4. **[表示するコンテンツ]** ウィンドウで、次の手順を実行します。

    1. 最初の列 (、 **値名** フィールド) をコピーし、次のクラス ID を貼り付けます: `{030E9A3F-7B18-4122-9A60-B87235E4F59E}`

    2. 2 列目の (、 **値** フィールド)、次の値を入力: `1`

    3. **[OK]** をクリックして **[表示するコンテンツ]** ウィンドウを閉じます。

    ![上で指定した値を入力する](./media/active-directory-saas-ie-group-policy/show-contents.png)

5. **[OK]** をクリックして変更を適用し、**[アドオンの一覧]** ウィンドウを閉じます。

選択した OU のコンピューターで拡張機能が有効になります。 [グループ ポリシーを使用して有効にするか、Internet Explorer のアドオンを無効にする方法の詳細について説明します。](https://technet.microsoft.com/library/dn454941.aspx)

## 手順 5 (省略可能): パスワードを保存するかどうかを確認するプロンプトを無効にする

ユーザーがアクセス パネル拡張機能を使用してサインインするときに、Internet Explorer でパスワードを保存するかどうかを確認する次のプロンプトが表示されることがあります。

![](./media/active-directory-saas-ie-group-policy/remember-password-prompt.png)

ユーザーに対してこのメッセージが表示されないようにする場合は、次の手順に従って、パスワードの保存によるオートコンプリートを禁止します。

1. **グループ ポリシー管理エディター** ウィンドウで、次のパスに移動します。 この構成設定は **[ユーザー構成]** でのみ利用できます。
    - `ユーザーの構成/ポリシー/管理用テンプレート/windows コンポーネント/インターネット エクスプ ローラー/`

2. **[フォームのユーザー名とパスワードのオートコンプリート機能を有効にする]** という名前の設定を見つけます。
    > [AZURE.NOTE] 以前のバージョンの Active Directory では、この設定は、**[パスワードを保存するオートコンプリートを許可しない]** という名前で表示される場合があります。 その設定の構成は、このチュートリアルで説明する設定とは異なります。

    ![これは ](./media/active-directory-saas-ie-group-policy/disable-auto-complete.png)

3. 上記の設定を右クリックし、**[編集]** を選択します。

4. **[フォームのユーザー名とパスワードのオートコンプリート機能を有効にする]** というタイトルのウィンドウで、**[無効]** を選択します。

    ![[無効] を選択](./media/active-directory-saas-ie-group-policy/disable-passwords.png)

5. **[OK]** をクリックしてこれらの変更を適用し、ウィンドウを閉じます。

これで、ユーザーは資格情報を保存することも、オートコンプリートを使用して前に保存された資格情報にアクセスすることもできなくなります。 ただし、このポリシーでは、他の種類のフォーム フィールド (検索フィールドなど) でのオートコンプリートは引き続き許可されます。
> [AZURE.WARNING] ユーザーがいくつかの資格情報を保存した後でこのポリシーが有効になった場合、既に保存されている資格情報がこのポリシーによって消去されることはありません。**

## 手順 6: デプロイのテスト

拡張機能のデプロイメントが成功したかどうかを確認するには、次の手順に従います。

1. 使用して展開されている場合 **コンピューターの構成**, で選択した OU に属しているクライアント コンピューターにサインオン [手順 2: グループ ポリシー オブジェクトを作成](#step-2-create-the-group-policy-object)します。 **[ユーザーの構成]** を使用してデプロイした場合は、その OU に属しているユーザーとしてサインインします。

2. そのコンピューターでグループ ポリシーの変更が完全に更新されるには、2 回サインインする必要がある場合があります。 更新プログラムには、開く、 **コマンド プロンプト** ウィンドウと次のコマンドを実行します: `gpupdate/force`

3. インストールが行われるには、コンピューターを再起動する必要があります。 起動には、通常の拡張機能のインストールよりかなり長い時間がかかる可能性があります。

4. コンピューターが再起動したら、**Internet Explorer** を開きます。 ウィンドウの右上隅の **[ツール]** (歯車アイコン) をクリックし、**[アドオンの管理]** をクリックします。

    ![[ツール] > ](./media/active-directory-saas-ie-group-policy/manage-add-ons.png)

5. **[アドオンの管理]** ウィンドウで、**[Access Panel Extension]** がインストールされていて、**[状態]** が **[有効]** に設定されていることを確認します。

    ![Access Panel Extension がインストールされ、有効になっていることを確認する](./media/active-directory-saas-ie-group-policy/verify-install.png)

[AZURE.INCLUDE [saas-toc](../../includes/active-directory-saas-toc.md)]




