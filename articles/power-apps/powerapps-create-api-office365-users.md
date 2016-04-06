<properties
    pageTitle="PowerApps Enterprise への Office 365 Users API の追加 | Microsoft Azure"
    description="組織の App Service 環境で、新しい Office 365 Users API を作成または構成します。"
    services=""
    suite="powerapps"
    documentationCenter="" 
    authors="rajeshramabathiran"
    manager="dwrede"
    editor=""/>

<tags
   ms.service="powerapps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na" 
   ms.date="11/25/2015"
   ms.author="litran"/>

# 組織の App Service 環境での新しい Office 365 Users API の作成

## Azure ポータルでの API の作成

1.  [Azure ポータル](https://portal.azure.com/), 、仕事用アカウントでサインインします。 たとえばでサインイン *yourUserName*@*yourcompany ' と*. com です。 これにより、会社のサブスクリプションに自動的にサインインされます。
 
2. 選択 **参照** タスク バーで。  
![][14]

3. 一覧で、PowerApps を検索または入力するスクロールできる *powerapps*:  
![][15]  

4.  **PowerApps**, [ **管理 Api**:    
![登録されている api を参照します。][1]

5.  **管理 Api**, [ **追加** 新しい API を追加します。  
![[API を追加します。][2]

6. わかりやすい名前を入力 **名前** した API 向けです。  
    
7.  **ソース**, [ **利用可能な Api** 構築済みの Api を選択して [ **Office 365 ユーザー**:  
![Office 365 ユーザーの api を選択します。][3]

8. 選択 **の設定 - 必要な設定を構成する**:  
![Office 365 ユーザー API 設定を構成します。][4]

9. 入力、 *クライアント Id* と *クライアント シークレット* 、Office 365 Azure Active Directory (AAD) アプリケーションのです。 これらがない場合は、このトピックの「PowerApps Office 365 Users API で使用する AAD アプリケーションの登録」を参照して、必要な ID とシークレットの値を作成します。  

    > [AZURE.IMPORTANT] 保存、 **URL をリダイレクトする**です。 この値は、このトピックで後ほど必要になる場合があります。  

10. 選択 **OK** の手順を完了します。

完了すると、App Service 環境に新しい Office 365 Users API が追加されます。

## 省略可能: PowerApps Office 365 Users API で使用する AAD アプリケーションの登録

キーとシークレットの値が割り当てられた既存の AAD アプリケーションがない場合は、次の手順に従ってアプリケーションを作成し、必要な値を取得します。 

1. 開いている [Azure ポータル][5]します。

2. 選択 **参照** し、[ **Active Directory**:  

    > [AZURE.NOTE] これは、Azure クラシック ポータルで Active Directory を開きます。  

3. 組織のテナントの名前を選択します。  
![Azure Active Directory の起動][6]

4. 選択、 **アプリケーション** タブをクリックし、選択 **追加**:  
![AAD テナント アプリケーション][7]

5.  **アプリケーション追加**:  

    a) を入力、 **名前** アプリケーションです。  
    b) のままにして、アプリケーションの入力として **Web**します。  
    c) 選択 **次**します。  

    ![AAD アプリケーションの追加 - アプリケーション情報][8]

6.  **アプリケーションのプロパティで**:  

    a) を入力、 **サインオン URL** 、アプリケーションのです。 PowerApps の AAD で認証するためにサインオン url を設定 _https://login.windows.net_します。  
    b) を入力する有効な **APP ID URI** アプリ用です。  
    c) 選択 **OK**します。  

    ![AAD アプリケーションの追加 - アプリケーションのプロパティ][9]

7. 操作が正常に完了すると、新しい AAD アプリケーションにリダイレクトされます。 選択 **構成**:  
![Contoso の AAD アプリケーション][10]

8. 設定、 **応答 URL** 下にある、 _OAuth 2_ (」を参照)、Azure ポータルで新しい Office 365 ユーザーの API を追加したときに受信したリダイレクト URL のセクションです。 選択 **アプリケーション追加**:  
![Contoso の AAD アプリケーションを構成します。][11]

9.  **他のアプリケーションに対するアクセス許可** ウィンドウで、 **Office 365 統合 API (プレビュー)**, を選択し、 **OK**します。

10. 構成] ページに戻ることに注意して _Office 365 統合 API (プレビュー)_ に追加、 _他のアプリケーションに対するアクセス許可_ ] ボックスの一覧です。

11. 選択 **委任されたアクセス許可** の _Office 365 統合 API (プレビュー)_, を選択し、 **すべてのユーザーの基本的なプロファイルを読み取る** 権限です。

新しい Azure Active Directory アプリケーションが作成されます。 Azure ポータルの Office 365 Users API 構成でこのアプリケーションを使用できます。 

## まとめと次のステップ
このトピックでは、Office 365 Users API を PowerApps Enterprise に追加しました。 次に、この API をユーザーのアプリケーションに追加できるように、ユーザーに API へのアクセスを許可します。 

[接続を追加し、ユーザーにアクセスを許可する](powerapps-manage-api-connection-user-access.md)


<!--References-->
[1]: ./media/powerapps-create-api-office365-users/browse-to-registered-apis.PNG
[2]: ./media/powerapps-create-api-office365-users/add-api.PNG
[3]: ./media/powerapps-create-api-office365-users/select-office365-users-api.PNG
[4]: ./media/powerapps-create-api-office365-users/configure-office365-users-api.PNG
[5]: https://portal.azure.com
[6]: ./media/powerapps-create-api-office365-users/launch-aad.PNG
[7]: ./media/powerapps-create-api-office365-users/aad-tenant-applications.PNG
[8]: ./media/powerapps-create-api-office365-users/aad-tenant-applications-add-appinfo.PNG
[9]: ./media/powerapps-create-api-office365-users/aad-tenant-applications-add-app-properties.PNG
[10]: ./media/powerapps-create-api-office365-users/contoso-aad-app.PNG
[11]: ./media/powerapps-create-api-office365-users/contoso-aad-app-configure.PNG

