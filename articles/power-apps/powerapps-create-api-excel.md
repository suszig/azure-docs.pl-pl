<properties
    pageTitle="PowerApps Enterprise への Excel API の追加 | Microsoft Azure"
    description="組織の App Service 環境での新しい Excel API の作成または構成"
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

# 組織の App Service 環境での新しい Excel API の作成

## Azure ポータルでの API の作成

1.  [Azure ポータル](https://portal.azure.com/), 、仕事用アカウントでサインインします。 たとえばでサインイン *yourUserName*@*yourcompany ' と*. com です。 これにより、会社のサブスクリプションに自動的にサインインされます。
 
2. 選択 **参照** タスク バーで。  
![][4]

3. 一覧で、PowerApps を検索または入力するスクロールできる *powerapps*:  
![][5]  

4.  **PowerApps**, [ **管理 Api**:  
![登録されている api を参照する][1]

5.  **管理 Api**, [ **追加** 新しい API を追加します。  
![API の追加][2]

6. わかりやすい名前を入力 **名前** した API 向けです。  
    
7.  **ソース**, [ **利用可能な Api** を事前に構築された Api を選択し、選択 **Excel**:  
![Excel の api を選択][3]

8. 選択 **OK** を次の手順を完了します。

完了すると、App Service 環境に新しい Excel API が追加されます。

## まとめと次のステップ
このトピックでは、Excel API を PowerApps Enterprise に追加しました。 次に、この API をユーザーのアプリケーションに追加できるように、ユーザーに API へのアクセスを許可します。 

[接続を追加し、ユーザーにアクセスを許可する](powerapps-manage-api-connection-user-access.md)



<!--References-->
[1]: ./media/powerapps-create-api-excel/browse-to-registered-apis.PNG
[2]: ./media/powerapps-create-api-excel/add-api.PNG
[3]: ./media/powerapps-create-api-excel/select-excel-api.PNG
[4]: ./media/powerapps-create-api-excel/browseall.png
[5]: ./media/powerapps-create-api-excel/allresources.png
