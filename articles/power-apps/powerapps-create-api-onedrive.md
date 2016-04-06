<properties
    pageTitle="PowerApps Enterprise への OneDrive API の追加 | Microsoft Azure"
    description="組織の App Service 環境で、新しい OneDrive API を作成または構成します。"
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

# 組織の App Service 環境での新しい OneDrive API の作成

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
    
7.  **ソース**, [ **利用可能な Api** 構築済みの Api を選択して [ **OneDrive**:  
![OneDrive api を選択します。][3]

8. 選択 **の設定 - 必要な設定を構成する**:  
![OneDrive API 設定を構成します。][4]

9. 入力、 *アプリ キー* と *App Secret* OneDrive アプリのです。 これらがない場合は、このトピックの「PowerApps で使用する OneDrive アプリケーションの登録」を参照して、必要なキーとシークレットの値を作成します。  

    > [AZURE.IMPORTANT] 保存、 **URL をリダイレクトする**です。 この値は、このトピックで後ほど必要になる場合があります。

10. 選択 **OK** の手順を完了します。

完了すると、App Service 環境に新しい OneDrive API が追加されます。

## 省略可能: PowerApps で使用する OneDrive アプリケーションの登録

キーとシークレットの値が割り当てられた既存の OneDrive アプリケーションがない場合は、次の手順に従ってアプリケーションを作成し、必要な値を取得します。 

1. 移動して、 [アプリケーション作成ページ][5] で _Microsoft アカウント デベロッパー センター_ を使用してサインインし、 _Microsoft アカウント_します。

2. 入力、 **アプリケーション名**, を選択して **同意**:  
![OneDrive の新しいアプリ][6]

3. 設定ページで次の操作を行います。  

    a) 選択 **API 設定**します。  
    b) (」を参照)、Azure ポータルで新しい OneDrive API を追加したときに受信したリダイレクト URL にリダイレクト URL を設定します。  
    c) 選択 **保存**します。  

    ![OneDrive アプリケーションの API 設定][7]

新しい OneDrive アプリケーションが作成されます。 Azure ポータルの OneDrive API 構成でこのアプリケーションを使用できます。 

## まとめと次のステップ
このトピックでは、OneDrive API を PowerApps Enterprise に追加しました。 次に、この API をユーザーのアプリケーションに追加できるように、ユーザーに API へのアクセスを許可します。 

[接続を追加し、ユーザーにアクセスを許可する](powerapps-manage-api-connection-user-access.md)

<!--References-->
[1]: ./media/powerapps-create-api-onedrive/browse-to-registered-apis.PNG
[2]: ./media/powerapps-create-api-onedrive/add-api.PNG
[3]: ./media/powerapps-create-api-onedrive/select-onedrive-api.PNG
[4]: ./media/powerapps-create-api-onedrive/configure-onedrive-api.PNG
[5]: https://account.live.com/developers/applications/create
[6]: ./media/powerapps-create-api-onedrive/onedrive-new-app.PNG
[7]: ./media/powerapps-create-api-onedrive/onedrive-app-api-settings.PNG
[14]: ./media/powerapps-create-api-onedrive/browseall.png
[15]: ./media/powerapps-create-api-onedrive/allresources.png

