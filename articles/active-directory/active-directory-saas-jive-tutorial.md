<properties 
    pageTitle="チュートリアル: Azure Active Directory と Jive の統合 | Microsoft Azure" 
    description="Azure Active Directory で Jive を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。" 
    services="active-directory" 
    authors="markusvi"  
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

#チュートリアル: Azure Active Directory と Jive の統合

  
このチュートリアルでは、Azure と Jive の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

-   有効な Azure サブスクリプション
-   Jive のテナント
  
このチュートリアルで説明するシナリオは、次の要素で構成されています。

1.  Jive のアプリケーション統合の有効化
2.  ユーザー プロビジョニングの構成

##Jive のアプリケーション統合の有効化
  
このセクションでは、Jive のアプリケーション統合を有効にする方法について説明します。

###Jive のアプリケーション統合を有効にするには、次の手順を実行します。

1.  左側のナビゲーション ウィンドウで、Azure 管理ポータルで、クリックして **Active Directory**します。

    ![Active Directory](./media/active-directory-saas-jive-tutorial/IC700993.png "Active Directory")

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

    ![アプリケーション](./media/active-directory-saas-jive-tutorial/IC700994.png "Applications")

4.  開くには、 **アプリケーション ギャラリー**, 、] をクリックして **アプリの追加**, 、クリックして **組織で使用するためのアプリケーションの追加**します。

    ![どの操作を行いますか。](./media/active-directory-saas-jive-tutorial/IC700995.png "What do you want to do?")

5.   **検索ボックス**, 、型 **Jive**します。

    ![Jive](./media/active-directory-saas-jive-tutorial/IC701001.png "Jive")

6.  結果ウィンドウで [ **Jive**, 、クリックして **完了** アプリケーションを追加します。

    ![Jive](./media/active-directory-saas-jive-tutorial/IC701005.png "Jive")
##ユーザー プロビジョニングの構成
  
このセクションでは、Active Directory のユーザー アカウントのプロビジョニングを Jive に対して有効にする方法を説明します。  
この手順の一環として、Jive.com からの要求に必要なユーザー セキュリティ トークンを指定する必要があります。
  
次のスクリーン ショットは、Azure AD の関連ダイアログの例を示しています。

![ユーザー プロビジョニングの構成](./media/active-directory-saas-jive-tutorial/IC698794.png "Configure User Provisioning")

###ユーザー プロビジョニングを構成するには、次の手順に従います。

1.  Azure の管理ポータルでの **Jive** アプリケーション統合ページでは、をクリックして **ユーザー プロビジョニングの構成** を開くには、 **ユーザー プロビジョニングの構成** ] ダイアログ。

2.   **自動ユーザー プロビジョニングを有効にする Jive 資格情報の入力** ] ページで、次の構成設定を提供します。

    1.   **Jive 管理ユーザー名** ] ボックスに、Jive アカウント名を入力、 **システム管理者** で割り当てられている Jive.com プロファイル。

    2.   **Jive 管理者パスワード** ] ボックスに、このアカウントのパスワードを入力します。

    3.   **Jive テナント URL** ] ボックスに、Jive テナント URL を入力します。

        >[AZURE.NOTE] Jive テナント URL は、Jive にログインし、組織で使用する URL です。  
        通常、URL には、次の形式: **www.\<organization\>.jive.com**します。

    4.  クリックして **検証** 構成を確認します。

    5.  をクリックして、 **次** を開く] ボタン、 **確認** ページです。

3.   **確認** ] ページで、構成を保存してチェック マークをクリックします。
  
ここで、テスト アカウントを作成して 10 分間待機し、アカウントが Jive.com と同期されていることを確認します。


