<properties
    pageTitle="JavaScript バックエンド モバイル サービスでのユーザーのサービス側承認 | Microsoft Azure"
    description="Azure モバイル サービスの JavaScript バックエンドでユーザーを承認する方法を学習します。"
    services="mobile-services"
    documentationCenter=""
    authors="krisragh"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.topic="article"
    ms.devlang="javascript"
    ms.date="11/30/2015"
    ms.author="krisragh"/>


# モバイル サービスでのユーザーのサービス側承認

> [AZURE.SELECTOR-LIST (Platform | Backend)]
- [(Any | .NET)](mobile-services-dotnet-backend-service-side-authorization.md)
- [(Any | Javascript)](mobile-services-javascript-backend-service-side-authorization.md)


このトピックでは、サーバー側スクリプトを使用してユーザーを承認する方法を示します。 このチュートリアルでは、Azure モバイル サービスでスクリプトを登録し、ユーザー ID に基づいてクエリをフィルターし、ユーザーに自分のデータのみへのアクセスを提供します。 ユーザー ID によるユーザーのクエリ結果のフィルター処理は、承認の最も基本的な形式です。 またシナリオによっては、特定のユーザーのアクセスを許可するエンドポイントなど、より詳細なユーザーの承認情報を確認するために、ユーザーまたはロールのテーブルを作成することもできます。

このチュートリアルでは、Mobile Services クイック スタートに基づいており、[既存の Mobile Services アプリケーションに認証の追加] のチュートリアルに基づいて記述します。 最初に [既存の Mobile Services アプリケーションに認証の追加] を入力してください。

## <a name="register-scripts"></a>スクリプトを登録します。

1. [Azure クラシック ポータル] にログオン] をクリックして **Mobile Services**, 、モバイル サービスで順にクリックします。 **[データ]** タブをクリックし、**TodoItem** テーブルをクリックします。

2. **[スクリプト]** をクリックし、**挿入**操作を選択して、既存のスクリプトを次の関数に置き換え、**[保存]** をクリックします。

        function insert(item, user, request) {
          item.userId = user.userId;
          request.execute();
        }

    このスクリプトは、挿入前に項目に認証済みユーザーのユーザー ID を追加します。
    >[AZURE.NOTE] 確認して [動的スキーマ](https://msdn.microsoft.com/library/azure/jj193175.aspx) が有効になっています。 それ以外の場合、*userId* 列は自動的には追加されません。 新しいモバイル サービスの既定ではこの設定は有効になります。

3. 同様に、既存の**読み取り**操作を次の関数で置き換えます。 このスクリプトは、ユーザーが自身に挿入した項目だけを受信できるように、返された TodoItem オブジェクトをフィルターします。

        function read(query, user, request) {
           query.where({ userId: user.userId });
           request.execute();
        }


## <a name="test-app"></a>アプリケーションをテストします。

1. 前のチュートリアルの _TodoItem_ テーブルに既に項目がある場合にクライアント側のアプリケーションを実行すると、アイテムが返されないことに注意してください。 これは、ユーザーの ID 列を含まない前の項目が挿入され、現在は null 値であるために発生します。 新しく追加された項目が、_TodoItem_ テーブル内に関連付けられている userId 値を持っていることを確認します。

2. その他のログイン アカウントがある場合は、アプリケーションを終了して削除し、もう一度実行することによって、ユーザーは自分のデータのみ表示できることを確認します。 ログインの資格情報ダイアログ ボックスが表示される場合、別のログインを入力し、前のログインで入力した項目が表示されないことを確認します。







[register server scripts]: #register-scripts 
[next steps]: #next-steps 
[windows push notifications & live connect]: http://go.microsoft.com/fwlink/p/?LinkID=257677 
[mobile services server script reference]: http://go.microsoft.com/fwlink/p/?LinkId=262293 
[my apps dashboard]: http://go.microsoft.com/fwlink/p/?LinkId=262039 
[add authentication to existing mobile services app]: /develop/mobile/tutorials/get-started-with-users-ios 
[azure classic portal]: https://manage.windowsazure.com/ 

