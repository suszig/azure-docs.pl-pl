<properties
    pageTitle="PowerApps Enterprise への Dynamics CRM Online API の追加 | Microsoft Azure"
    description="組織の App Service 環境で、新しい Dynamics CRM Online API を作成または構成します。"
    services=""
    suite="powerapps"
    documentationCenter=""
    authors="schabungbam"
    manager="dwrede"
    editor=""/>

<tags
   ms.service="powerapps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="11/25/2015"
   ms.author="sameerch"/>


# 組織の App Service 環境での新しい Dynamics CRM Online API の作成

## Azure ポータルでの API の作成

1. [Azure ポータル](https://portal.azure.com), 、仕事用アカウントでサインインします。 たとえばでサインイン *yourUserName*@*yourcompany ' と*. com です。 これにより、会社のサブスクリプションに自動的にサインインされます。

2. 選択 **参照** タスク バーで。  
![][1]

3. 一覧で、PowerApps を検索または入力するスクロールできる *powerapps*:  
![][2]

4. **PowerApps**, [ **管理 Api**:  
![登録されている API の参照][3]

5. **管理 Api**, [ **追加** 新しい API を追加します。  
![Add API][4]

6. API のわかりやすい**名前**を入力します。

7. **ソース**, [ **利用可能な Api** 構築済みの Api を選択して [ **Dynamics CRM Online**:
![Dynamics CRM Online API の選択][5]

8. 選択 **の設定 - 必要な設定を構成する**:
![Dynamics CRM Online API 設定の構成][6]

9. Dynamics CRM Online Azure Active Directory (AAD) アプリケーションの**クライアント ID** と**アプリケーション キー**を入力します。 これらがない場合は、このトピックの「PowerApps Dynamics CRM Online API で使用する AAD アプリケーションの登録」を参照して、必要な ID とシークレットの値を作成します。
    > [AZURE.IMPORTANT] **リダイレクト URL** を保存しておいてください。 この値は、このトピックで後ほど必要になる場合があります。

10. **[OK]** をクリックして、手順を完了します。

完了すると、App Service 環境に新しい Dynamics CRM Online API が追加されます。

## PowerApps Dynamics CRM Online API で使用する AAD アプリケーションの登録

1. 開いている、 [Azure ポータル](https://portal.azure.com)します。

2. **[参照]** をクリックし、**[Active Directory]** を選択します。
    > [AZURE.NOTE] これにより、Azure クラシック ポータルで Active Directory が開きます。  

3. 組織のテナントの名前を選択します。  
![Azure Active Directory の起動][7]

4. 選択、 **アプリケーション** タブをクリックし、選択 **追加**:  
![AAD テナント アプリケーション][8]

5. **[アプリケーションの追加]** で次の操作を行います。

    a) を入力、 **名** アプリケーションです。  
    b) のままにして、アプリケーションの入力として **Web**します。  
    c) 選択 **次**します。

    ![AAD アプリケーションの追加 - アプリケーション情報][9]

6. **[アプリケーションのプロパティ]** で次の操作を行います。

    a) アプリケーションの**サインオン URL** を入力します。 PowerApps の AAD で認証するためにサインオン url を設定 _https://login.windows.net_します。  
    b) を入力する有効な **APP ID URI** アプリ用です。  
    c) **[OK]** を選択します。

    ![AAD アプリケーションの追加 - アプリケーションのプロパティ][10]

7. 操作が正常に完了すると、新しい AAD アプリケーションにリダイレクトされます。 選択 **構成**:  
![Contoso AAD アプリケーション][11]

8. 設定、 **応答 URL** (」を参照)、Azure ポータルで新しい Dynamics CRM Online API を追加したときに受信したリダイレクト URL に _OAuth 2_ セクションの下。  
![Contoso AAD アプリケーションの構成][12]

9. [**保存**] を選択します。

新しい Azure Active Directory アプリケーションが作成されます。 Azure ポータルの Dynamics CRM Online API 構成でこのアプリケーションを使用できます。

## まとめと次のステップ

このトピックでは、Dynamics CRM Online API を PowerApps Enterprise に追加しました。 次に、この API をユーザーのアプリケーションに追加できるように、ユーザーに API へのアクセスを許可します。

[接続を追加し、ユーザー アクセスを付与](powerapps-manage-api-connection-user-access.md)




[1]: ./media/powerapps-create-api-crmonline/browseall.png 
[2]: ./media/powerapps-create-api-crmonline/allresources.png 
[3]: ./media/powerapps-create-api-crmonline/browse-to-registered-apis.PNG 
[4]: ./media/powerapps-create-api-crmonline/add-api.PNG 
[5]: ./media/powerapps-create-api-crmonline/select-crmonline-api.PNG 
[6]: ./media/powerapps-create-api-crmonline/configure-crmonline-settings.PNG 
[7]: ./media/powerapps-create-api-crmonline/launch-aad.PNG 
[8]: ./media/powerapps-create-api-crmonline/aad-tenant-applications.PNG 
[9]: ./media/powerapps-create-api-crmonline/aad-tenant-applications-add-appinfo.PNG 
[10]: ./media/powerapps-create-api-crmonline/aad-tenant-applications-add-app-properties.PNG 
[11]: ./media/powerapps-create-api-crmonline/contoso-aad-app.PNG 
[12]: ./media/powerapps-create-api-crmonline/contoso-aad-app-configure.PNG 

