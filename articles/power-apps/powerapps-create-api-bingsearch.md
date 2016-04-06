<properties
    pageTitle="PowerApps Enterprise への Bing Search API の追加 | Microsoft Azure"
    description="組織の App Service 環境での新しい Bing Search API の作成または構成"
    services=""
    suite="powerapps"
    documentationCenter="" 
    authors="LinhTran"
    manager="gautamt"
    editor=""/>

<tags
   ms.service="powerapps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na" 
   ms.date="11/25/2015"
   ms.author="litran"/>

# 組織の App Service 環境での新しい Bing Search API の作成

## Azure ポータルでの API の作成

1.  [Azure ポータル](https://portal.azure.com/), 、仕事用アカウントでサインインします。 たとえばでサインイン *yourUserName*@*yourcompany ' と*. com です。 これにより、会社のサブスクリプションに自動的にサインインされます。
 
2. 選択 **参照** タスク バーで。  
![][4]

3. 一覧で、PowerApps を検索または入力するスクロールできる *powerapps*:  
![][5]  

4.  **PowerApps**, [ **管理 Api**:  
![登録されている api を参照します。][2]

2.  **管理 Api**, [ **追加** 新しい API を追加します。  
![[API を追加します。][3]

3. わかりやすい名前を入力 **名前** した API 向けです。 

4.  **ソース**, [ **使用可能な API** 構築済みの Api を選択して [ **Bing Search**:  

    a) 選択 **の設定 - 必要な設定を構成する**です。  
    
    b) を入力 *アカウント キー*します。 Bing 検索キーを取得していない場合は、無料作成 [Bing Search オファー][1] キーを取得します。 

    c) 選択 **OK**します。 

5. 選択 **OK** の手順を完了します。 

完了すると、App Service 環境に新しい Bing Search API が追加されます。


## まとめと次のステップ
このトピックでは、Bing Search API を PowerApps Enterprise に追加しました。 次に、この API をユーザーのアプリケーションに追加できるように、ユーザーに API へのアクセスを許可します。 

[接続を追加し、ユーザーにアクセスを許可する](powerapps-manage-api-connection-user-access.md)

<!--References-->
[1]: https://datamarket.azure.com/dataset/bing/search
[2]: ./media/powerapps-create-api-dropbox/browse-to-registered-apis.PNG
[3]: ./media/powerapps-create-api-dropbox/add-api.PNG
[4]: ./media/powerapps-create-api-dropbox/browseall.png
[5]: ./media/powerapps-create-api-dropbox/allresources.png

