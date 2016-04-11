    <properties
    pageTitle="Add owners and users to a DevTest Lab | Microsoft Azure"
    description="Securely add a user who is not in your subscription to your Azure DevTest Lab."
    services="devtest-lab,virtual-machines"
    documentationCenter="na"
    authors="tomarcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="devtest-lab"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="11/01/2015"
    ms.author="tarcher"/>

# DevTest ラボへの所有者とユーザーの追加

## 概要

DevTest ラボへのアクセスは、Azure のロール ベースのアクセス制御 (RBAC) で制御します。 検索 [ロール-ベースのアクセス制御 (RBAC)](https://azure.microsoft.com/searchresults?query=Role%20Based%20Access%20Control%20%28RBAC%29) の詳細については、Azure プレビュー ポータルで。

DevTest ラボへのアクセスを許可するには 2 つのロールを使用します。

 - **所有者**: ユーザーに割り当て、 **所有者** Azure のサブスクリプション レベルのロールがある場合は、ラボ管理など、監視機能に完全にアクセスします。

     > [AZURE.NOTE] **所有者** DevTest ラボでは、サブスクリプション レベル以外の RBAC レベルで割り当てられているロールはサポートされていません。 ユーザー権利の割り当て、 **所有者** DevTest ラボでのロールがサポートされていません。

 -  **DevTest ラボ ユーザー**: ユーザーに割り当て、 **DevTest ラボ ユーザー** ロールは、作成、更新、および指定されたラボで仮想マシンを削除します。 ユーザーには、いずれかを指定できる *内部* (サブスクリプションの Azure Active Directory のメンバー)、または *外部* (取引先組織のメンバーなど、Azure AD のメンバーではないユーザー)。
    -  A **DevTest ラボ ユーザー** からロールを割り当てる必要がある、 **ユーザーの追加** ラボのタイルです。
    -  内のユーザー、 **DevTest ラボ ユーザー** ロールに割り当てられているラボでこれらの操作を内のみ実行できます。  
    たとえば、 **DevTest ラボ ユーザー** サブスクリプションの仮想マシン サービスを使用して仮想マシンを作成することはできません。 仮想マシンの作成は、DevTest ラボ アカウントから行う場合のみ許可されます。
    - *外部* ユーザー (つまり @hotmail.com、@live.com、@msn.com、@passport.com、@outlook.com、または特定の国の一種) は、Microsoft アカウントのドメインのいずれかのアカウントが必要です。

## ラボへの所有者の追加

DevTest ラボでは、ラボを含む Azure サブスクリプションの所有者がこれらのラボの所有者と見なされます。 Azure プレビュー ポータルでラボのブレードを使用して DevTest ラボに他の所有者を追加できますが、これは現在サポートされていません。 

既にラボを作成している、または新しいラボを作成する Azure サブスクリプションに所有者を追加するには、次の手順に従います。

1. サインイン、 [Azure プレビュー ポータル](http://portal.azure.com)します。

1. 左側のナビゲーションでタップ **サブスクリプション**します。

    ![Subscriptions link](./media/devtest-lab-add-devtest-user/subscriptions.png)
    
1. ラボを含めるサブスクリプションをタップします。

1. タップして、 **アクセス** アイコン。 

    ![Access users](./media/devtest-lab-add-devtest-user/access-users.png)

1.  **ユーザー** ブレードで、tap **追加**します。

    ![ユーザーの追加](./media/devtest-lab-add-devtest-user/devtest-users-blade.png)

1.  **ロールを選択して** ブレードで、tap **所有者**します。

1. 入力、 **ユーザー** を所有者として追加するユーザーの電子メールのテキスト ボックスです。 ユーザーが見つからない場合は、問題を示すエラー メッセージが表示されます。 そのユーザーが下に表示される、ユーザーが見つかった場合、 **ユーザー** テキスト ボックスです。

1. 見つかったユーザー名をタップします。

1. タップ **選択**します。

1. タップ **[ok]** を閉じる、 **アクセスを追加する** ブレードです。

1. 戻ると、 **ユーザー** ブレードで、ユーザーを所有者として追加されていることを確認します。 これで、このユーザーはこのサブスクリプションで作成されたすべてのラボの所有者となり、所有者タスクを実行できるようになります。 

## ラボへの DevTest ラボ ユーザーの追加

DevTest ラボ ユーザーをラボに追加するには、次の手順に従います。

1. サインイン、 [Azure プレビュー ポータル](http://portal.azure.com)します。

1. タップ **参照**します。

1. タップ **DevTest ラボ**します。

1. ラボの一覧で目的のラボをタップします。   

1. タップして、 **アクセス** アイコン。

    ![User access](./media/devtest-lab-add-devtest-user/devtest-lab-home-blade.png)

1.  **ユーザー** ブレードで、tap **追加**します。

    ![ユーザーの追加](./media/devtest-lab-add-devtest-user/devtest-users-blade.png)

1.  **ロールを選択して** ブレードで、tap **DevTest ラボ ユーザー**

1.  **ユーザーを追加する** ブレード。

    1.  **ユーザーを追加する** ブレード組み込みのユーザーの一覧が表示されます。 目的のユーザーが既に一覧にある場合は、そのユーザーの行をタップして選択するだけです。 そのユーザーが選択されていることを示すチェックマークが左側に表示されます。 複数のユーザーを選択するには、保持、 **& lt;Ctrl キーを押し >** キーながら、各ユーザーをクリックします。 ユーザーの選択を解除するには、保持、 **& lt;Ctrl キーを押し >** キーし、[ユーザー] をクリックします。 ブレードの下部のカウンターが、選択したユーザーの数を示します。

    1. 目的のユーザーが一覧にない場合は、入力の有効な Microsoft 電子メール アカウント、 **ユーザー** テキスト ボックスです。 下のユーザーに表示されます電子メール アドレスが有効である場合、 **ユーザー** テキスト ボックスです。 そのユーザーをタップして選択します。   

    1. ラボを追加] をタップするユーザーを選択すると、 **選択**します。

    1. タップ **[ok]** を閉じる、 **アクセスを追加する** ブレードです。

    1.  **ユーザー** ブレードは、追加のロールとユーザーが表示されます。
