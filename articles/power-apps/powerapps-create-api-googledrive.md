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

1. [Azure ポータル](https://portal.azure.com/), 、仕事用アカウントでサインインします。 たとえばでサインイン *yourUserName*@*yourcompany ' と*. com です。 これにより、会社のサブスクリプションに自動的にサインインされます。

2. 選択 **参照** タスク バーで。  
![][15]

3. 一覧で、PowerApps を検索または入力するスクロールできる *powerapps*:  
![][16]

4. **PowerApps**, [ **管理 Api**:  
![登録されている API の参照][1]

5. **管理 Api**, [ **追加** 新しい API を追加します。  
![Add API][2]

6. API のわかりやすい**名前**を入力します。

7. **ソース**, [ **利用可能な Api** 構築済みの Api を選択して [ **Google ドライブ**:  
![Google Drive API を選択する][3]

8. 選択 **の設定 - 必要な設定を構成する**:  
![Google Drive API の設定を構成する][4]

9. Google Drive アプリケーションの*アプリケーション キー*と*アプリケーション シークレット*を入力します。 これらがない場合は、このトピックの「PowerApps で使用する Google Drive アプリの登録」を参照して、必要なキーとシークレットの値を作成します。
    > [AZURE.IMPORTANT] **リダイレクト URL** を保存しておいてください。 この値は、このトピックで後ほど必要になる場合があります。

10. **[OK]** をクリックして、手順を完了します。

完了すると、App Service 環境に新しい Google Drive API が追加されます。


## 省略可能: PowerApps で使用する Google Drive アプリの登録

キーとシークレットの値が割り当てられた既存の Google Drive アプリがない場合は、次の手順に従ってアプリケーションを作成し、必要な値を取得します。

1. サインイン [Google 開発者コンソールの ][5]:  
![Google Developers Console][6]

2. **[Create an empty project]** を選択します。

3. アプリケーションの名前を入力、使用条件に同意して選択 **作成**:  
![新しい Google Drive プロジェクトを作成する][7]

4. 新しいプロジェクトの作成に成功、次のように選択します **Google Api を使用して**:。  
![Google API を使用する][8]

5. 概要ページで、選択 **ドライブ API**:  
![Google Drive API の概要][9]

6. 選択 **API を有効にする**:  
![Google Drive API を有効にする][10]

7. ドライブの API を有効にする方法を選択 **資格情報**, を選択して **OAuth 2.0 クライアント ID**:  
![資格情報を追加する][12]

8. **[Configure consent screen]** を選択します。

9. **OAuth 同意画面** ] タブで、入力、 **製品名**, を選択して **保存**:  
![Configure consent screen][13]

10. [Create Client ID] ページで次のようにします。

    a) で **アプリケーションの種類**, [ **Web アプリケーション**します。  
    b)、クライアントの名前を入力します。  
    c) (」を参照)、Azure ポータルで新しい Google ドライブ API を追加したときに受信したリダイレクト URL にリダイレクト URL を設定します。  
    d)] を選択 **作成**します。

    ![クライアント ID を作成する][14]

11. 登録したアプリケーションのクライアント ID とクライアント シークレットが表示されます。

新しい Google Drive アプリが作成されます。 Azure ポータルの Google Drive API 構成でこのアプリケーションを使用できます。

## まとめと次のステップ

このトピックでは、Google Drive API を PowerApps Enterprise に追加しました。 次に、この API をユーザーのアプリケーションに追加できるように、ユーザーに API へのアクセスを許可します。

[接続を追加し、ユーザー アクセスを付与](powerapps-manage-api-connection-user-access.md)




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

