<properties
    pageTitle="PowerApps Enterprise への Salesforce API の追加 | Microsoft Azure"
    description="組織の App Service 環境で、新しい Salesforce API を作成または構成します。"
    services=""
    suite="powerapps"
    documentationCenter="" 
    authors="rajeshramabathiran"
    manager="dwerde"
    editor=""/>

<tags
   ms.service="powerapps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na" 
   ms.date="11/25/2015"
   ms.author="litran"/>


# 組織の App Service 環境での新しい Salesforce API の作成

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

7. **ソース**, [ **利用可能な Api** 構築済みの Api を選択して [ **Salesforce**:  
![Salesforce API を選択する][3]

8. 選択 **の設定 - 必要な設定を構成する**:  
![Dropbox API の設定を構成する][7]

9. Salesforce アプリケーションの*アプリケーション キー*と*アプリケーション シークレット*を入力します。 これらがない場合は、このトピックの「PowerApps で使用する Salesforce アプリケーションの登録」を参照して、必要なキーとシークレットの値を作成します。
    > [AZURE.IMPORTANT] **リダイレクト URL** を保存しておいてください。 この値は、このトピックで後ほど必要になる場合があります。

10. **[OK]** をクリックして、手順を完了します。

完了すると、App Service 環境に新しい Salesforce API が追加されます。


## 省略可能: PowerApps で使用する Salesforce アプリケーションの登録

キーとシークレットの値が割り当てられた既存の Salesforce アプリケーションがない場合は、次の手順に従ってアプリケーションを作成し、必要な値を取得します。

1. 開いている [Salesforce 開発者のホーム ページ ][5], 、Salesforce アカウントでサインインします。

2. ホーム ページで、プロファイルを選択し、[ **セットアップ**:  
![Salesforce のホーム ページ][6]

3. **[Create]** を選択し、**[Apps]** を選択します。  **アプリ** ] ページで、[ **新規** [ **接続アプリ**:  
![Salesforce がアプリを作成する][7]

4. **[New Connected App]** で次のようにします。

    a) の値を入力してください。 **接続型アプリケーション名**します。  
    b) の値を入力してください。 **API 名**します。  
    c) の値を入力してください。 **Contact Email**します。  
    d) _a (OAuth 設定を有効にする) _、[選択 **OAuth 設定を有効にする**, 、設定と、 **コールバック URL** (」を参照)、Azure ポータルで新しい Salesforce API を追加したときに受信したリダイレクト URL にします。

5. (_S) OAuth scopes_、下に次のスコープを追加、 **OAuth スコープの選択**:

    - 自分の Chatter データにアクセスして管理する (chatter_api)
    - 自分のデータにアクセスして管理する (api)
    - 自分の一意の識別子へのアクセスを許可する (openid)
    - いつでも自分に代わって要求を実行する (refresh_token、offline_access)

6. **保存** 変更します。  
![Salesforce の新しいアプリ][8]

新しい Salesforce アプリが作成されます。 Azure ポータルの Salesforce API 構成でこのアプリケーションを使用できます。

## まとめと次のステップ

このトピックでは、Salesforce API を PowerApps Enterprise に追加しました。 次に、この API をユーザーのアプリケーションに追加できるように、ユーザーに API へのアクセスを許可します。

[接続を追加し、ユーザー アクセスを付与](powerapps-manage-api-connection-user-access.md)



[1]: ./media/powerapps-create-api-salesforce/browse-to-registered-apis.PNG 
[2]: ./media/powerapps-create-api-salesforce/add-api.PNG 
[3]: ./media/powerapps-create-api-salesforce/select-salesforce-api.PNG 
[4]: ./media/powerapps-create-api-salesforce/configure-salesforce-api.PNG 
[5]: https://developer.salesforce.com 
[6]: ./media/powerapps-create-api-salesforce/salesforce-developer-homepage.PNG 
[7]: ./media/powerapps-create-api-salesforce/salesforce-create-app.PNG 
[8]: ./media/powerapps-create-api-salesforce/salesforce-new-app.PNG 
[14]: ./media/powerapps-create-api-salesforce/browseall.png 
[15]: ./media/powerapps-create-api-salesforce/allresources.png 

