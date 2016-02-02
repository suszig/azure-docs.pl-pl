<properties
    pageTitle="PowerApps Enterprise への Twitter API の追加 | Microsoft Azure"
    description="組織の App Service 環境で、新しい Twitter API を作成または構成します。"
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


# 組織の App Service 環境での新しい Twitter API の作成

## Azure ポータルでの API の作成

1. [Azure ポータル](https://portal.azure.com/), 、仕事用アカウントでサインインします。 たとえばでサインイン *yourUserName*@*yourcompany ' と*. com です。 これにより、会社のサブスクリプションに自動的にサインインされます。

2. 選択 **参照** タスク バーで。  
![][14]

3. 一覧で、PowerApps を検索または入力するスクロールできる *powerapps*:  
![][15]

4. **PowerApps**, [ **管理 Api**:  
![登録されている API の参照][1]

5. **管理 Api**, [ **追加** 新しい API を追加します。    
![Add API][2]

6. API のわかりやすい**名前**を入力します。

7. **ソース**, [ **利用可能な Api** 構築済みの Api を選択して [ **Twitter**:  
![Twitter API の選択][3]

8. 選択 **の設定 - 必要な設定を構成する**:    
![Twitter API の設定の構成][4]

9. Twitter アプリケーションの*コンシューマー キー*と*コンシューマー シークレット*を入力します。 これらがない場合は、このトピックの「PowerApps で使用する Twitter アプリケーションの登録」を参照して、必要なキーとシークレットの値を作成します。
    > [AZURE.IMPORTANT] **リダイレクト URL** を保存しておいてください。 この値は、このトピックで後ほど必要になる場合があります。

10. **[OK]** をクリックして、手順を完了します。

完了すると、App Service 環境に新しい Twitter API が追加されます。


## 省略可能: PowerApps で使用する Twitter アプリケーションの登録

キーとシークレットの値が割り当てられた既存の Twitter アプリケーションがない場合は、次の手順に従ってアプリケーションを作成し、必要な値を取得します。

1. 移動して [https://apps.twitter.com/](https://apps.twitter.com) twitter アカウントを使用してサインインします。

2. 選択 **新しいアプリを作成する**:    
![Twitter アプリ ページ][6]

3. **[Create an application]** では次のようにします。

    a) の値を入力 **名**します。  
    b) の値を入力 **説明**します。  
    c) の値と入力して **web サイト**します。  
    d) セット、 **コールバック url** (」を参照)、Azure ポータルで新しい Twitter API を追加したときに受信したリダイレクト URL にします。  
    e) 開発者契約に同意して、選択 **Twitter アプリケーションを作成する**します。

    ![Twitter アプリの作成][7]

4. アプリが正常に作成されると、アプリのページにリダイレクトされます。

新しい Twitter アプリが作成されます。 Azure ポータルの Twitter API 構成でこのアプリケーションを使用できます。

## まとめと次のステップ

このトピックでは、Twitter API を PowerApps Enterprise に追加しました。 次に、この API をユーザーのアプリケーションに追加できるように、ユーザーに API へのアクセスを許可します。

[接続を追加し、ユーザー アクセスを付与](powerapps-manage-api-connection-user-access.md)





[1]: ./media/powerapps-create-api-twitter/browse-to-registered-apis.PNG 
[2]: ./media/powerapps-create-api-twitter/add-api.PNG 
[3]: ./media/powerapps-create-api-twitter/select-twitter-api.PNG 
[4]: ./media/powerapps-create-api-twitter/configure-twitter-api.PNG 
[6]: ./media/powerapps-create-api-twitter/twitter-apps-page.PNG 
[7]: ./media/powerapps-create-api-twitter/twitter-app-create.PNG 
[14]: ./media/powerapps-create-api-sqlserver/browseall.png 
[15]: ./media/powerapps-create-api-sqlserver/allresources.png 

