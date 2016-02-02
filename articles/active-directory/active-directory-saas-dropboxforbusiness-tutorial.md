<properties 
    pageTitle="チュートリアル: Azure Active Directory と Dropbox for Business の統合 | Microsoft Azure" 
    description="ビジネス用 Azure Active Directory に Dropbox を使用して、シングル サインオン、自動化されたプロビジョニング、および詳細を有効にする方法を説明します。" 
    services="active-directory" 
    authors="MarkusVi"  
    documentationCenter="na" 
    manager="stevenpo"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="10/22/2015" 
    ms.author="markvi" />


# チュートリアル: Azure Active Directory と Dropbox for Business の統合

このチュートリアルでは、Azure と Dropbox for Business の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Dropbox for Business のテスト テナント

このチュートリアルを完了すると、Azure AD ユーザーに割り当てた Dropbox Dropbox for Business 会社サイト (サービス プロバイダーが開始したサインオン)、またはを使用して、アプリケーションにシングル サインオンできるビジネス用、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。

このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Dropbox for Business のアプリケーション統合の有効化
2.  シングル サインオンの構成
3.  ユーザー プロビジョニングの構成
4.  ユーザーの割り当て

![シナリオ](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769508.png "Scenario")



## Dropbox for Business のアプリケーション統合の有効化

このセクションでは、Dropbox for Business のアプリケーション統合を有効にする方法について説明します。

### Dropbox for Business のアプリケーション統合を有効にするには、次の手順を実行します。

1.  Azure 管理ポータルの左側のナビゲーション ウィンドウで、**[Active Directory]** をクリックします。

    ![Active Directory](./media/active-directory-saas-dropboxforbusiness-tutorial/IC700993.png "Active Directory")

2.  **[ディレクトリ]** の一覧から、ディレクトリ統合を有効にするディレクトリを選択します。

3.  アプリケーション ビューを開くには、ディレクトリ ビューでトップ メニューの **[アプリケーション]** をクリックします。

    ![アプリケーション](./media/active-directory-saas-dropboxforbusiness-tutorial/IC700994.png "Applications")

4.  ページの下部にある **[追加]** をクリックします。

    ![アプリケーションの追加](./media/active-directory-saas-dropboxforbusiness-tutorial/IC749321.png "Add application")

5.  **[実行する内容]** ダイアログで、**[ギャラリーからアプリケーションを追加します]** をクリックします。

    ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-dropboxforbusiness-tutorial/IC749322.png "Add an application from gallerry")

6.  **検索ボックス**に、「**Dropbox for Business**」と入力します。

    ![アプリケーション ギャラリー](./media/active-directory-saas-dropboxforbusiness-tutorial/IC701010.png "Application gallery")

7.  結果ウィンドウで **[Dropbox for Business]** を選択し、**[完了]** をクリックしてアプリケーションを追加します。

    ![Dropbox for Business](./media/active-directory-saas-dropboxforbusiness-tutorial/IC701011.png "Dropbox for Business")
## シングル サインオンの構成

このセクションでは、SAML プロトコルに基づくフェデレーションを使用して、ユーザーが Azure AD のアカウントで Dropbox for Business に対する認証を行うことができるようにする方法を説明します。

この手順の途中で、Base-64 でエンコードされた証明書を Dropbox for Business テナントにアップロードする必要があります。 この手順に慣れていない場合は、次を参照してください。 [バイナリ証明書をテキスト ファイルに変換する方法](http://youtu.be/PlgrzUZ-Y1o)します。

### シングル サインオンを構成するには、次の手順に従います。

1.  Azure AD ポータルでの **Dropbox for Business** アプリケーション統合ページで、] をクリックして **でのシングル サインオンを構成する** を開くには、 ** [シングル サインオン ** ダイアログ。

    ![シングル サインオンの構成](./media/active-directory-saas-dropboxforbusiness-tutorial/IC749323.png "Configure single sign-on")

2.  **どのような Dropbox for Business にサインオン** ] ページで、[ **Microsoft Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

    ![シングル サインオンの構成](./media/active-directory-saas-dropboxforbusiness-tutorial/IC749327.png "Configure single sign-on")

3.  **[アプリケーション URL の構成]** ページで、次の手順を実行します。

     3.1. サインオンを Dropbox for business テナントです。<br><br>  ![シングル サインオンを構成します。](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769509.png "Configure single sign-on")

     3.2. 左側のナビゲーション ウィンドウでをクリックして **Admin Console**します。<br><br>  ![シングル サインオンを構成します。](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769510.png "Configure single sign-on")

     3.3. **Admin Console**, 、] をクリックして **認証** 左側のナビゲーション ウィンドウです。<br><br>  ![シングル サインオンを構成します。](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769511.png "Configure single sign-on")

     3.4. **シングル サインオン** ] セクションの [ **でのシングル サインオンを有効にする**, 、] をクリックし、 **詳細** このセクションを展開します。<br><br>  ![シングル サインオンを構成します。](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769512.png "Configure single sign-on")

     3.5. URL をコピーする] の横に **電子メール アドレスを入力するユーザーがサインインできるかに直接移動して**します。<br><br>  ![シングル サインオンを構成します。](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769513.png "Configure single sign-on")

     3.6. Azure ポータルでの **DropBox for business サインイン** URL] ボックスに、URL を貼り付けます。<br><br>  ![シングル サインオンを構成します。](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769514.png "Configure single sign-on")

4. **Dropbox for Business でのシングル サインオンの構成** ] ページで [ **証明書のダウンロード**, 、お使いのコンピューターに証明書ファイルを保存します。<br><br>  ![シングル サインオンを構成します。](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769515.png "Configure single sign-on")

5. Dropbox for Business テナントに、 **シングル サインオン** のセクション、 **認証** ] ページで、次の手順を実行: <br><br>  ![でのシングル サインオンを構成する](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769516.png "Configure single sign-on")

     5.1. **[必須項目です]** をクリックします。

     5.2. Azure ポータルで、**[Dropbox for Business でのシングル サインオンの構成]** ダイアログ ページの **[サインイン ページ URL]** の値をコピーし、**[サインイン URL]** ボックスに貼り付けます。

     5.3. ダウンロードした証明書から **Base-64 でエンコードされた**ファイルを作成します。 > [AZURE です。ヒント:] 詳細については、次を参照してください。 [バイナリ証明書をテキスト ファイルに変換する方法](http://youtu.be/PlgrzUZ-Y1o)します。

     5.4. **[証明書の選択]** をクリックし、**Base-64 でエンコードされた証明書ファイル**を参照します。

     5.5. **[変更の保存]** をクリックして、DropBox for Business テナントでの構成を完了します。

6. Azure AD ポータルで、[シングル サインオンの構成確認] を選び、**[完了]** をクリックして **[シングル サインオンの構成]** ダイアログを閉じます。<br><br>  ![シングル サインオンを構成します。](./media/active-directory-saas-dropboxforbusiness-tutorial/IC749329.png "Configure single sign-on")





## ユーザー プロビジョニングの構成

このセクションでは、Dropbox for Business への Active Directory ユーザー アカウントのユーザー プロビジョニングを有効にする方法を説明します。


### ユーザー プロビジョニングを構成するには、次の手順に従います。

1. Azure 管理ポータルの **Dropbox for Business** アプリケーション統合ページで、**[ユーザー プロビジョニングの構成]** をクリックして、**[ユーザー プロビジョニングの構成]** ダイアログを開きます。

2. 有効にする、ユーザー プロビジョニングの DropBox にビジネスのページを開くには、Dropbox にサインイン ダイアログの Azure AD とリンクするユーザー プロビジョニングを有効にする] クリックします。<br><br> ![ユーザー プロビジョニング](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769517.png "User provisioning")

3. **Azure AD とリンクする Dropbox へのサインイン** ] ダイアログで、Dropbox for Business テナントにサインインします。<br><br> ![ユーザー プロビジョニング](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769518.png "User provisioning")

4. クリックして **許可** を Azure AD を Dropbox へのアクセスを付与します。<br><br> ![ユーザー プロビジョニング](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769519.png "User provisioning")

5. 構成を完了するにはクリックして、 **完了** ] ボタンをクリックします。<br><br> ![ユーザー プロビジョニング](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769520.png "User provisioning")




## ユーザーの割り当て

構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

### ユーザーを Dropbox for Business に割り当てるには、次の手順を実行します。

1.  Azure AD ポータルで、テスト アカウントを作成します。

2.  **Dropbox for Business * * アプリケーション統合ページで、をクリックして * * ユーザーの割り当て**します。

    ![ユーザーの割り当て](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769521.png "Assign users")

3.  テスト ユーザーを選択し、**[割り当て]**、**[はい]** の順にクリックして、割り当てを確定します。

    ![はい](./media/active-directory-saas-dropboxforbusiness-tutorial/IC767830.png "Yes")



10 分間待機し、アカウントが Dropbox for Business に同期されていることを確認します。

最初の検証手順として、Azure 管理ポータルの **Dropbox for Business** アプリケーション統合ページで **[ダッシュボード]** をクリックして、プロビジョニングの状態を確認できます。

<br><br>  ![ユーザーを割り当てください。](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769522.png "Assign users")


正常に完了したユーザー プロビジョニング サイクルは、関連する状態で示されます。

<br><br>  ![ユーザーを割り当てください。](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769523.png "Assign users")


シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。
アクセス パネルの詳細については、次を参照してください。 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)します。




## その他のリソース

* [Azure Active Directory による SaaS アプリを統合する方法に関するチュートリアルの一覧](active-directory-saas-tutorial-list.md)
* [アプリケーション アクセスと Azure Active Directory でのシングル サインオンとは](active-directory-appssoaccess-whatis.md)




