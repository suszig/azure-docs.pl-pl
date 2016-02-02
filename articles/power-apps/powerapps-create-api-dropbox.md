<properties
    pageTitle="PowerApps Enterprise への Dropbox API の追加 | Microsoft Azure"
    description="組織の App Service 環境で、新しい Dropbox API を作成または構成します。"
    services=""
    suite="powerapps"
    documentationCenter="" 
    authors="linhtranms"
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


# 組織の App Service 環境での新しい Dropbox API の作成

## Azure ポータルでの API の作成

1. [Azure ポータル](https://portal.azure.com/), 、仕事用アカウントでサインインします。 たとえばでサインイン *yourUserName*@*yourcompany ' と*. com です。 これにより、会社のサブスクリプションに自動的にサインインされます。

2. 選択 **参照** タスク バーで。  
![][12]

3. 一覧で、PowerApps を検索または入力するスクロールできる *powerapps*:  
![][13]

4. **PowerApps**, [ **管理 Api**:  
![登録されている API の参照][4]

5. **管理 Api**, [ **追加** 新しい API を追加します。  
![Add API][5]

6. API のわかりやすい**名前**を入力します。

7. **ソース**, [ **利用可能な Api** を事前に構築された Api に表示し、選択 **Dropbox**:  
![Dropbox API の選択][6]

8. 選択 **の設定 - 必要な設定を構成する**:  
![Dropbox API の設定を構成する][7]

9. Dropbox アプリケーションの**アプリケーション キー**と**アプリケーション シークレット**の値を入力します。 これらがまだない場合は、このトピックの「PowerApps で使用する Dropbox アプリケーションの登録」を参照して、必要なキーとシークレットの値を作成します。
    > [AZURE.IMPORTANT] **リダイレクト URL** を保存しておいてください。 この値は、このトピックで後ほど必要になる場合があります。

10. **[OK]** をクリックして、手順を完了します。


完了すると、App Service 環境に新しい Dropbox API が追加されます。


## 省略可能: PowerApps で使用する Dropbox アプリケーションの登録

キーとシークレットの値が割り当てられた既存の Dropbox アプリケーションがない場合は、次の手順に従ってアプリケーションを作成し、必要な値を取得します。

1. 移動して [Dropbox ][1] 自分のアカウントを使用してサインインします。

2. Dropbox の開発者向けサイトに移動し、選択 **My Apps**:  
![Dropbox 開発者向けサイト][8]

3. 選択 **アプリを作成する**:  
![Dropbox でのアプリケーションの作成][9]

4. **[Create a new app on the Dropbox platform]** で次の操作を行います。

    a) で **選択 API**, [ **Dropbox API**します。  
    b) **必要なアクセスの種類を選択する**, [ **完全 Dropbox...**します。  
    c) アプリケーションの名前を入力します。

    ![Dropbox のアプリケーション作成ページ 1][10]

5. アプリケーション設定ページで次の操作を行います。

    **[OAuth 2]** セクションで、**[Redirect URL]** を、(このトピックで) Azure ポータルで新しい Dropbox API を追加したときに受け取ったリダイレクト URL に設定します。 **[追加]** を選択します。  
    b) 選択 **表示** 表示へのリンク、 **アプリケーション シークレット**:

    ![Dropbox のアプリケーション作成ページ 2][11]

新しい Dropbox アプリケーションが作成されます。 Azure ポータルの Dropbox API 構成でこのアプリケーションを使用できます。


## まとめと次のステップ

このトピックでは、Dropbox API を PowerApps Enterprise に追加しました。 次に、この API をユーザーのアプリケーションに追加できるように、ユーザーに API へのアクセスを許可します。

[接続を追加し、ユーザー アクセスを付与](powerapps-manage-api-connection-user-access.md)



[1]: https://www.dropbox.com/login 
[2]: https://www.dropbox.com/developers/apps/create 
[3]: https://www.dropbox.com/developers/apps 
[4]: ./media/powerapps-create-api-dropbox/browse-to-registered-apis.PNG 
[5]: ./media/powerapps-create-api-dropbox/add-api.PNG 
[6]: ./media/powerapps-create-api-dropbox/select-dropbox-api.PNG 
[7]: ./media/powerapps-create-api-dropbox/configure-dropbox-api.PNG 
[8]: ./media/powerapps-create-api-dropbox/dropbox-developer-site.PNG 
[9]: ./media/powerapps-create-api-dropbox/dropbox-create-app.PNG 
[10]: ./media/powerapps-create-api-dropbox/dropbox-create-app-page1.PNG 
[11]: ./media/powerapps-create-api-dropbox/dropbox-create-app-page2.PNG 
[12]: ./media/powerapps-create-api-dropbox/browseall.png 
[13]: ./media/powerapps-create-api-dropbox/allresources.png 

