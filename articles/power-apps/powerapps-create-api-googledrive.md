<properties
    pageTitle="PowerApps Enterprise への Google Drive API の追加 | Microsoft Azure"
    description="組織の App Service 環境で、新しい Google Drive API を作成または構成します。"
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

# 組織の App Service 環境での新しい Google Drive API の作成

## Azure ポータルでの API の作成

1.  [Azure ポータル](https://portal.azure.com/), 、仕事用アカウントでサインインします。 たとえばでサインイン *yourUserName*@*yourcompany ' と*. com です。 これにより、会社のサブスクリプションに自動的にサインインされます。
 
2. 選択 **参照** タスク バーで。  
![][15]

3. 一覧で、PowerApps を検索または入力するスクロールできる *powerapps*:  
![][16]  

4.  **PowerApps**, [ **管理 Api**:  
![登録されている api を参照する][1]

5.  **管理 Api**, [ **追加** 新しい API を追加します。  
![API の追加][2]

6. わかりやすい名前を入力 **名前** した API 向けです。  
    
7.  **ソース**, [ **利用可能な Api** 構築済みの Api を選択して [ **Google ドライブ**:  
![選択 google api][3]

8. 選択 **の設定 - 必要な設定を構成する**:  
![google ドライブ API 設定の構成][4]

9. 入力 *アプリ キー* と *App Secret* Google ドライブ アプリケーションのです。 これらがない場合は、このトピックの「PowerApps で使用する Google Drive アプリの登録」を参照して、必要なキーとシークレットの値を作成します。  

    > [AZURE.IMPORTANT] 保存、 **URL をリダイレクトする**です。 この値は、このトピックで後ほど必要になる場合があります。

10. 選択 **OK** の手順を完了します。

完了すると、App Service 環境に新しい Google Drive API が追加されます。


## 省略可能: PowerApps で使用する Google Drive アプリの登録

キーとシークレットの値が割り当てられた既存の Google Drive アプリがない場合は、次の手順に従ってアプリケーションを作成し、必要な値を取得します。 

1. サインイン [Google Developers Console] [5]。  
![Google 開発者コンソール][6]

2. 選択 **空のプロジェクトを作成する**です。 

3. アプリケーションの名前を入力、使用条件に同意して選択 **作成**:  
![新しいプロジェクトの google ドライブの作成][7]

4. 新しいプロジェクトの作成に成功、次のように選択します **Google Api を使用して**:。  
![Google api を使用][8]

5. 概要ページで、選択 **ドライブ API**:  
![Google ドライブ API の概要][9]

6. 選択 **API を有効にする**:  
![Google ドライブ API を有効にする][10]

7. ドライブの API を有効にする方法を選択 **資格情報**, を選択して **OAuth 2.0 クライアント ID**:  
![資格情報の追加][12]

8. 選択 **構成同意画面**します。

9.  **OAuth 同意画面** タブで、入力、 **製品名**, を選択して **保存**:  
![同意画面を構成する][13]

10. [Create Client ID] ページで次のようにします。  

    a) で **アプリケーションの種類**, [ **Web アプリケーション**します。  
    b)、クライアントの名前を入力します。  
    c) (」を参照)、Azure ポータルで新しい Google ドライブ API を追加したときに受信したリダイレクト URL にリダイレクト URL を設定します。  
    d)] を選択 **作成**します。  

    ![クライアント id の作成][14] 

11. 登録したアプリケーションのクライアント ID とクライアント シークレットが表示されます。

新しい Google Drive アプリが作成されます。 Azure ポータルの Google Drive API 構成でこのアプリケーションを使用できます。 

## まとめと次のステップ
このトピックでは、Google Drive API を PowerApps Enterprise に追加しました。 次に、この API をユーザーのアプリケーションに追加できるように、ユーザーに API へのアクセスを許可します。 

[接続を追加し、ユーザーにアクセスを許可する](powerapps-manage-api-connection-user-access.md)


<!--References-->
[1]: ./media/powerapps-create-api-googledrive/browse-to-registered-apis.PNG
[2]: ./media/powerapps-create-api-googledrive/add-api.PNG
[3]: ./media/powerapps-create-api-googledrive/select-googledrive-api.PNG
[4]: ./media/powerapps-create-api-googledrive/configure-googledrive-api.PNG
[5]: https://console.developers.google.com/
[6]: ./media/powerapps-create-api-googledrive/google-developers-console.PNG
[7]: ./media/powerapps-create-api-googledrive/googledrive-create-project.PNG
[8]: ./media/powerapps-create-api-googledrive/use-google-apis.PNG
[9]: ./media/powerapps-create-api-googledrive/googledrive-api-overview.PNG
[10]: ./media/powerapps-create-api-googledrive/enable-googledrive-api.PNG
[11]: ./media/powerapps-create-api-googledrive/googledrive-api-credentials.PNG
[12]: ./media/powerapps-create-api-googledrive/googledrive-api-credentials-add.PNG
[13]: ./media/powerapps-create-api-googledrive/configure-consent-screen.PNG
[14]: ./media/powerapps-create-api-googledrive/create-client-id.PNG
[15]: ./media/powerapps-create-api-googledrive/browseall.png
[16]: ./media/powerapps-create-api-googledrive/allresources.png
