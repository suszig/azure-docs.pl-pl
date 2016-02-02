<properties
    pageTitle="Logic Apps での SFTP コネクタの使用 | Microsoft Azure App Service"
    description="SFTP コネクタまたは API アプリを作成、構成して、Azure App Service のロジック アプリで使用する方法"
    authors="anuragdalmia"
    manager="dwrede"
    editor=""
    services="app-service\logic"
    documentationCenter=""/>

<tags
    ms.service="app-service-logic"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="11/30/2015"
    ms.author="sameerch"/>


# SFTP コネクタの使用開始とロジック アプリへの追加

SFTP コネクタを使用して、SFTP サーバー間でデータを移動します。 ファイルのダウンロード、アップロード、一覧取得を SFTP サーバー間で行うことができます。

ロジック アプリはさまざまなデータ ソースを基にトリガーでき、フローの一環としてデータの取得と処理のためのコネクタを提供します。 この SFTP コネクタをビジネス ワークフローに追加し、ロジック アプリ内のそのワークフローの一部としてデータを処理できます。

## Logic App 用の SFTP コネクタを作成する

コネクタは、ロジック アプリ内で作成することも、Azure Marketplace から直接作成することもできます。 Marketplace からコネクタを作成するには、次の操作を実行します。

1. Azure のスタート画面で、**[Marketplace]** を選択します。
2. "SFTP コネクタ" を検索して選択し、**[作成]** を選択します。
3. 次のように、SFTP コネクタを構成します。  
![][1]
    - **[場所]** - コネクタをデプロイする地域を選択します。
    - **[サブスクリプション]** - コネクタを作成するサブスクリプションを選択します。
    - **[リソース グループ]** - コネクタを格納するリソース グループを選択するか、作成します。
    - **[Web ホスティング プラン]** - Web ホスティング プランを選択するか、作成します。
    - **[価格レベル]** - コネクタの価格レベルを選択します。
    - **[名前]** - SFTP コネクタの名前を指定します。
    - **パッケージの設定**
        - **[サーバー アドレス]** - SFTP サーバーの名前または IP アドレスを指定します。
        - **[Accept Any SSH Server HostKey (任意の SSH サーバーのホストキーを受け入れる)]** - サーバーから任意の SSH パブリック ホスト キーのフィンガープリントを受け入れるかどうかを決定します。 false に設定すると、ホスト キーは [SSH Server Host Key Finger Print (SSH サーバー ホスト キー フィンガープリント)] のプロパティで指定されたキーと一致します。
        - **[SSH Server HostKey (SSH サーバー ホストキー)]** - SSH サーバーのパブリック ホスト キーのフィンガープリントを指定します (*省略可能*)。
        - **[ルート フォルダー]** - ルート フォルダーのパスを指定します。 空白の場合は、既定のルートに設定されます。
        - **[Encrypt Cipher (暗号化)]** - 暗号を指定します (*省略可能*)。
        - **[サーバー ポート]** - SFTP サーバーのポート番号を指定します。
4. [作成] をクリックします。 新しい SFTP コネクタが作成されます。

5. [参照] を使用して作成した API アプリへの参照] の [API Apps]-> [ <Name of the API App just created> [セキュリティ] コンポーネントが構成されていないことを確認できます。  
![][2]
6. [セキュリティ] コンポーネントをクリックし、SFTP のコネクタのセキュリティ (ユーザー名、パスワード、秘密キー、PPK ファイル パスワード) を構成します。
[セキュリティ] で、"Password"または"Privatekey"または"MutliFactor"の承認タブを選択し、必要なプロパティを指定します。  
![][3]  
![][4]  
![][5]
6. セキュリティ構成が保存されたら、同じリソース グループ内に、この SFTP コネクタを使用するロジック アプリを作成できます。

## Logic App で SFTP コネクタを使用する

API アプリが作成されたら、Logic App のトリガーやアクションとして SFTP コネクタを使用できます。 そのためには、次の手順を実行する必要があります。

1.  新しいロジック アプリを作成し、SFTP コネクタが含まれるのと同じリソース グループを選択します。  
![][6]
2.  [トリガーとアクション]、Logic Apps デザイナーを開き、フローを構成するを開きます。  
![][7]
3.  SFTP コネクタは、右側のギャラリーでの API Apps [このリソース グループ] セクションで表示されます。  
![][8]
4.  [SFTP コネクタ] をクリックして、SFTP コネクタの API アプリをエディターにドロップできます。

5.  これで、フローで SFTP コネクタを使用できるようになりました。 今後は、SFTP トリガー ("TriggerOnFileAvailable") を使って取得したファイルを、フローの他のアクションで使用できます。
    > [AZURE.IMPORTANT] SFTP トリガー "TriggerOnFileAvailable" は、取得したファイルを処理した後、そのファイルを削除します。

6.  SFTP トリガーの入力プロパティを次のように構成します。

    - **[フォルダー パス]** - ファイルを取得する必要があるフォルダーのパスを指定します。
    - **[ファイルの種類: テキストまたはバイナリ]** - ファイルの種類を選択します。
    - **ファイル マスク** -ファイルを取得するために適用するファイル マスクを指定します。 ' *' を指定したフォルダー内のすべてのファイルを取得します。
    - **[ファイル マスクの除外]** - ファイルを除外するために適用するファイル マスクを指定します。 [ファイル マスク] のプロパティも設定した場合は、先に [ファイル マスクの除外] のプロパティが適用されます。

    ![][9]  
    ![][10]

7.  フローで SFTP アクションを使用する方法もほぼ同じです。 [ファイルのアップロード] アクションを使用して SFTP サーバーにファイルをアップロードできます。 [ファイルのアップロード] アクションの入力プロパティを次のように構成します。

    - **[コンテンツ]** - アップロードするファイルのコンテンツを指定します
    - **[コンテンツ転送エンコード]** - [なし] または [Base64] を指定します。
    - **[ファイル パス]** - アップロードするファイルのファイル パスを指定します。
    - **[上書き]** - ファイルが既に存在する場合に上書きするには、"true" を指定します
    - ** 存在する場合は追加 **-"true"または"false"を指定します。 "true" に設定すると、ファイルが存在する場合にファイルにデータが追加されます。 "false" に設定すると、ファイルが存在する場合にファイルは上書きされます。
    - **[一時フォルダー]** - 指定した場合、アダプターは、ファイルを "一時フォルダー パス" にアップロードします。アップロードが完了すると、ファイルは "フォルダー パス" に移動されます。 移動操作がアトミックであるためには、"一時フォルダー パス" が "フォルダー パス" と同じ物理ディスクにある必要があります。 一時フォルダーは、"存在する場合に追加" プロパティが無効の場合にのみ使用できます。

    ![][11]  
    ![][12]

## コネクタでできること

コネクタが作成されたため、ロジック アプリを使用してコネクタをビジネス ワークフローに追加できます。 を参照してください [Logic Apps とは?](app-service-logic-what-are-logic-apps.md).
>[AZURE.NOTE] Azure Logic Apps と Azure アカウントにサインアップする前に開始する場合に、 [ロジック アプリを実行してください](https://tryappservice.azure.com/?appservice=logic), 、App Service で有効期間の短いスターター ロジック アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

表示、Swagger の REST API リファレンス [コネクタと API Apps のリファレンス](http://go.microsoft.com/fwlink/p/?LinkId=529766)します。

パフォーマンス統計をレビューし、コネクタに対するセキュリティを制御することもできます。 参照してください [の管理し、監視、組み込みの API Apps とコネクタ](app-service-logic-monitor-your-connectors.md)します。




[1]: ./media/app-service-logic-connector-sftp/img1.PNG 
[2]: ./media/app-service-logic-connector-sftp/img2.PNG 
[3]: ./media/app-service-logic-connector-sftp/img3.PNG 
[4]: ./media/app-service-logic-connector-sftp/img4.PNG 
[5]: ./media/app-service-logic-connector-sftp/img5.PNG 
[6]: ./media/app-service-logic-connector-sftp/img6.PNG 
[7]: ./media/app-service-logic-connector-sftp/img7.png 
[8]: ./media/app-service-logic-connector-sftp/img8.png 
[9]: ./media/app-service-logic-connector-sftp/img9.PNG 
[10]: ./media/app-service-logic-connector-sftp/img10.PNG 
[11]: ./media/app-service-logic-connector-sftp/img11.PNG 
[12]: ./media/app-service-logic-connector-sftp/img12.PNG 

