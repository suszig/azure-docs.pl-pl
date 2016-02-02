<properties
    pageTitle="PowerApps Enterprise への Microsoft Translator API の追加 | Microsoft Azure"
    description="組織の App Service 環境で、新しい Microsoft Translator API を作成または構成します。"
    services=""
    suite="powerapps"
    documentationCenter="" 
    authors="linhtranms"
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


# 組織の App Service 環境での新しい Microsoft Translator API の作成

## Azure ポータルでの API の作成

1. [Azure ポータル](https://portal.azure.com/), 、仕事用アカウントでサインインします。 たとえばでサインイン *yourUserName*@*yourcompany ' と*. com です。 これにより、会社のサブスクリプションに自動的にサインインされます。

2. 選択 **参照** タスク バーで。  
![][7]

3. 一覧で、PowerApps を検索または入力するスクロールできる *powerapps*:  
![][8]

4. **PowerApps**, [ **管理 Api**:  
![登録されている API の参照][1]

5. **管理 Api**, [ **追加** 新しい API を追加します。  
![Add API][2]

6. API のわかりやすい**名前**を入力します。

7. **ソース**, [ **利用可能な Api** 構築済みの Api を選択して [ **Microsoft Translator**:  
![Microsoft Translator API の選択][3]

8. 選択 **の設定 - 必要な設定を構成する**:  
![Microsoft Translator API 設定の構成][4]

9. Microsoft Translator アプリケーションの*クライアント ID* と*クライアント シークレット*を入力します。 これらがない場合は、このトピックの「PowerApps で使用する Microsoft Translator アプリケーションの登録」を参照して、必要な ID とシークレットの値を作成します。

9. **[OK]** をクリックして、手順を完了します。

完了すると、App Service 環境に新しい Microsoft Translator API が追加されます。


## 省略可能: PowerApps で使用する Microsoft Translator アプリケーションの登録

ID とシークレットの値が割り当てられた既存の Microsoft Translator アプリケーションがない場合は、次の手順に従ってアプリケーションを作成し、必要な値を取得します。


1. 移動して [Azure データ マーケットの開発者用のページ ][5] し、Microsoft アカウントでサインインします。

2. **[登録]** をクリックします。

3. **[アプリケーションの登録]**で次の操作を行います。

    a) の値を入力 **クライアント Id**します。  
    b) を入力、 **名** 、アプリケーションのです。  
    c) のダミー値を入力 **url をリダイレクトする**します。 たとえば、入力 *https://contosoredirecturl*します。  
    d) を入力、 **説明**します。  
    e) 選択 **作成**します。

    ![アプリケーションの登録][6]

新しい Microsoft Translator アプリケーションが作成されます。 Azure ポータルの Microsoft Translator API 構成でこのアプリケーションを使用できます。


## まとめと次のステップ

このトピックでは、Microsoft Translator API を PowerApps Enterprise に追加しました。 次に、この API をユーザーのアプリケーションに追加できるように、ユーザーに API へのアクセスを許可します。

[接続を追加し、ユーザー アクセスを付与](powerapps-manage-api-connection-user-access.md)




[1]: ./media/powerapps-create-api-microsofttranslator/browse-to-registered-apis.PNG 
[2]: ./media/powerapps-create-api-microsofttranslator/add-api.PNG 
[3]: ./media/powerapps-create-api-microsofttranslator/select-microsofttranslator-api.PNG 
[4]: ./media/powerapps-create-api-microsofttranslator/configure-microsofttranslator-api.PNG 
[5]: https://datamarket.azure.com/developer/applications/ 
[6]: ./media/powerapps-create-api-microsofttranslator/register-your-application.PNG 
[7]: ./media/powerapps-create-api-microsofttranslator/browseall.png 
[8]: ./media/powerapps-create-api-microsofttranslator/allresources.png 

