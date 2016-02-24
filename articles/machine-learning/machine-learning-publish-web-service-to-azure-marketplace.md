<properties 
    pageTitle="Azure Marketplace に Machine Learning Web service を発行する | Microsoft Azure" 
    description="Azure Marketplace に Azure Machine Learning Web サービスを発行する方法" 
    services="machine-learning" 
    documentationCenter="" 
    authors="LuisCabrer" 
    manager="paulettm" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/01/2015" 
    ms.author="bharaths"/>

# Azure Marketplace への Azure Machine Learning Web サービスの発行 

Azure Marketplace は、外部の顧客が使用する有料または無料のサービスとして Azure Machine Learning Web サービスを発行する機能を提供します。 この記事では、プロセスの概要と使用開始のためのガイドラインへのリンクを示します。 このプロセスを使用することで、他の開発者が Web サービスをアプリケーション内で使用できるようになります。


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## 発行プロセスの概要 

Azure Machine Learning Web サービスを Azure Marketplace に発行するための手順を次に示します。

1. Machine Learning Request-Response サービス (RRS) を作成して発行する
2. サービスを運用環境にデプロイし、API キーと OData のエンドポイント情報を取得する
3. 公開された web サービスの URL への公開を使用して [Azure Marketplace (データ マーケット)](https://publish.windowsazure.com/workspace/) 
4. 送信すると、プランの内容が確認され、顧客が購入するにはこのサービスが承認される必要があります。 発行プロセスには数営業日かかります。 

## チュートリアル
###手順 1: Machine Learning Request-Response サービス (RRS) を作成して発行する###
 これを見てを行ってください場合は、これを行わない既に、 [順を追って](machine-learning-walkthrough-5-publish-web-service.md)します。

###手順 2: サービスを運用環境にデプロイし、API キーと OData のエンドポイント情報を取得する###
1.  [Azure Classic Portal](http://manage.windowsazure.com), を選択、 **MACHINE LEARNING** 左側のナビゲーション バーからオプションし、新しいワークスペースを選択します。 

2. をクリックして、 **WEB サービス** タブをクリックし、marketplace に発行する web サービスを選択します。

    ![Azure Marketplace][workspace]

3. マーケットプレースで使用するエンドポイントを選択します。 追加のエンドポイントを作成していない場合は選択できます、 **既定** エンドポイント。

4. 参照してください、エンドポイントでクリックするとができます、 **API キー**します。 この情報は後の手順 3 で必要になりますので、コピーしておきます。

    ![Azure Marketplace][apikey]

5. をクリックして、 **要求/応答** メソッドは、この時点で、marketplace にバッチ実行サービスの公開はサポートされていません。 [要求/応答] メソッドの API ヘルプ ページが表示されます。

6. コピー、 **OData エンドポイント アドレス**, 、手順 3 で後でこの情報は必要があります。

    ![Azure Marketplace][odata]




サービスを運用環境にデプロイします。



###手順 3: 発行済みの Web サービスの URL を使用して、Azure Marketplace (データ マーケット) に発行する###

1.  移動 [Azure Marketplace (データ マーケット)](http://datamarket.azure.com/home) 
2.  をクリックして、 **発行** ページの上部にあるリンクです。 この操作により、 [Microsoft Azure 発行ポータル](https://publish.windowsazure.com)
3.  をクリックして、 **パブリッシャー** 発行者として登録するにはセクションです。
4.  新しいオファーを作成するときに選択 **Data Services**, 、クリックして **新しいデータ サービスを作成する**です。 
 
    ![Azure Marketplace][image1]

    <br />


5.   **プラン** 料金プランなどのサービスに関する情報を提供します。 無料サービスまたは有料サービスのいずれを提供するかを決定します。 有料にするには、銀行や税などの支払い情報を入力します。

6.   **マーケティング** 、タイトルとプランの説明などプランに関する情報を提供します。

7.   **価格** することができますの特定の国のプランの価格を設定するか、システム「料金」提案します。

8.  **データ サービス** ] タブ、[ **Web サービス** として、 **データ ソース**します。

    ![Azure Marketplace][image2]

9.  上記の手順 2 で説明したように、Azure クラシック ポータルから、Web サービスの URL と API キーを取得します。

10. マーケットプ レース データ サービスのセットアップ] ダイアログ ボックスに貼り付け、OData エンドポイント アドレスを **サービス URL** テキスト ボックスです。

11.  **認証**, 、選択 **ヘッダー** として、 **認証スキーム**します。

    - 「Authorization」と入力、 **ヘッダー名**します。
    -  **ヘッダー値**, "Bearer"を入力して、(引用符を除く) をクリックして、 **領域** バー、および API キーを貼り付けます。
    - 選択、 **このサービスは OData** チェック ボックスをオンします。
    - クリックして **接続のテスト** 接続をテストします。

12.  **カテゴリ**, 、ように **Machine Learning** が選択されています。

13. 後は、プランに関するメタデータをすべて入力] をクリックして **発行**, 、し **Push to Staging**します。 この時点で、修正する必要のある問題が残っている場合は通知されます。

14. すべての未解決の問題の完了を確認したら、をクリックして **を運用環境にプッシュする承認の要求**します。 発行プロセスには数営業日かかります。 


[image1]:./media/machine-learning-publish-web-service-to-azure-marketplace/image1.png
[image2]:./media/machine-learning-publish-web-service-to-azure-marketplace/image2.png
[workspace]:./media/machine-learning-publish-web-service-to-azure-marketplace/selectworkspace.png
[apikey]:./media/machine-learning-publish-web-service-to-azure-marketplace/apikey.png
[odata]:./media/machine-learning-publish-web-service-to-azure-marketplace/odata.png
 

