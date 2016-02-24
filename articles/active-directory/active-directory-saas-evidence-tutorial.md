<properties
    pageTitle="チュートリアル: Azure Active Directory と Evidence.com の統合 | Microsoft Azure"
    description="Azure Active Directory と Evidence.com の間でシングル サインオンを構成する方法について説明します。"
    services="active-directory"
    documentationCenter=""
    authors="asmalser-msft"
    manager="stevenpo"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/10/2015"
    ms.author="asmalser"/>


# チュートリアル: Azure Active Directory と Evidence.com の統合

このチュートリアルでは、Azure Active Directory (AAD) と Syncplicity の間でのシングル サインオンを設定する方法を説明します。 このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。
    
* 有効な Microsoft Azure サブスクリプション
* シングル サインオン、Evidence.com サブスクリプション (電子メール earlyaccess@evidence.com で SAML ベースのシングル サインオンが有効になっていない場合) を有効になっています。

このチュートリアルを完了すると、Evidence.com に割り当てられている AAD ユーザーは、AAD アクセス パネルを使用してアプリケーションへのシングル サインオンが可能になります。

## 目的のディレクトリへの Evidence.com の追加

このセクションでは、Evidence.com を統合アプリケーションとして Azure Active Directory に追加する方法を説明します。

**Evidence のアプリケーション統合を有効にするには**

1.   [Azure クラシック ポータル](https://manage.windowsazure.com), 、左側のナビゲーション ウィンドウで、次のようにクリックします。 **Active Directory**します。

2.   **ディレクトリ** 一覧で、ディレクトリ統合を有効にディレクトリを選択します。

3.  ディレクトリ ビューで、[アプリケーション] ビューを開くにはクリックして **アプリケーション** 上部のメニュー。

4.  アプリケーション ギャラリーを開くにはクリックして **追加**, 、クリックして **ギャラリーからアプリケーションを追加**します。

5.  検索ボックスに入力 **Evidence.com**します。

6.  結果ウィンドウで [ **Evidence.com**, 、クリックして **完了** アプリケーションを追加します。


## シングル サインオンの構成

このセクションでは、SAML プロトコルに基づくフェデレーションを使用して、Evidence.com で Azure Active Directory のユーザー アカウントを使用してユーザーを認証できるようにする方法を説明します。

**シングル サインオンを構成するには、次の手順に従います。**

1.  Azure クラシック ポータルで Evidence.com を追加したら、クリックして **シングル サインオンを構成する**です。 
 
2.  次の画面で選択 **Azure AD シングル サインオン**, 、] をクリックし、 **次**します。

3.  [アプリケーション URL の構成] 画面で、URL を入力、Evidence.com テナントの URL にユーザーがサインインする場所 (例: https://yourtenant.evidence.com をクリック **次**します。 

4.  クリックして、 **証明書のダウンロード** リンク、および、ローカル ドライブに保存します。 この証明書やメタデータの Url (エンティティの ID、SSO サインイン URL とサインアウト URL) は Evidence.com サイトに SSO を設定するために使用されます。 

5.  別の web ブラウザーのウィンドウで、Evidence.com へのログインに移動してテナントの管理者として **Admin** ] タブ
      
6.  をクリックして **代理店の Single Sign On**
 
7.  選択 **SAML ベースでのシングル サインオン**
 
8.  コピー、 **発行者 URL**, 、**シングル サインオンの** と **シングル サインアウト** Azure クラシック ポータルで Evidence.com の対応するフィールドに表示される値。

9.  手順 4、Notepad.exe などのテキスト エディターを使用して証明書を開き、コピーして、内容を貼り付ける、 **セキュリティ証明書** ボックス。 

10. Evidence.com の構成を保存します。
 
11. Azure クラシック ポータルで確認 **前述のとおり、シングル サインオン構成されているいることを確認**します。 これをオンにすると、このアプリケーションのチェック ボックスの機能を、現在の証明書で開始できるようになります。
 
12. シングル サインオンの確認] ページで、をクリックして **完了**します。  


## Evidence.com テスト ユーザーの作成

Azure AD ユーザーがサインインできるようにするには、ユーザーを Evidence.com アプリケーションにプロビジョニングする必要があります。 このセクションでは、Evidence.com で Azure AD ユーザー アカウントを作成する方法について説明します。

**Evidence.com でユーザー アカウントをプロビジョニングするには**

1.  Web ブラウザー ウィンドウで、Evidence.com 企業サイトに管理者としてログインします。

2.  移動 **Admin** ] タブをクリックします。

3.  をクリックして **ユーザーを追加**します。

4.  クリックして、 **追加** ] ボタンをクリックします。

5.   **電子メール アドレス** Azure ad アクセス権を付与するユーザー、ユーザーのユーザー名のと一致する必要があります追加したユーザーのです。 ユーザー名と電子メール アドレスは、組織内の同じ値ではない場合、行うこともできます、 **Evidence.com > 属性 > シングル サインオン** Evidence.com に、電子メール アドレスに送信される nameidenitifer を変更する場合は、Azure 旧ポータルのセクションです。


## Evidence.com へのユーザーの割り当て

プロビジョニングされた AAD ユーザーがアクセス パネルに Evidence.com を表示できるようにするには、Azure クラシック ポータル内でアクセス権を割り当てる必要があります。

**Evidence.com にユーザーを割り当てるには**

1.  Azure クラシック ポータルで Evidence.com のクイック スタート ページでクリックして **Evidence.com にユーザーを割り当てる**します。
 
2.   **表示** ] メニューの [Evidence.com に、ユーザーまたはグループを割り当てるし、チェック マークをクリックするかどうかを選択します。
 
3.   **ユーザー** 一覧で、割り当てる Evidence.com するグループ化するユーザーを選択します。
 
4.  ページ フッター内をクリックして、 **割り当てる** ] ボタンをクリックします。


