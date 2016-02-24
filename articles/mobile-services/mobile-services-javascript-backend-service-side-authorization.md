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

> [AZURE.SELECTOR-LIST (プラットフォーム | バックエンド)]
- [(任意 | .NET)](mobile-services-dotnet-backend-service-side-authorization.md)
- [(任意 | Javascript)](mobile-services-javascript-backend-service-side-authorization.md)

このトピックでは、サーバー側スクリプトを使用してユーザーを承認する方法を示します。 このチュートリアルでは、Azure モバイル サービスでスクリプトを登録し、ユーザー ID に基づいてクエリをフィルターし、ユーザーに自分のデータのみへのアクセスを提供します。 ユーザー ID によるユーザーのクエリ結果のフィルター処理は、承認の最も基本的な形式です。 またシナリオによっては、特定のユーザーのアクセスを許可するエンドポイントなど、より詳細なユーザーの承認情報を確認するために、ユーザーまたはロールのテーブルを作成することもできます。

このチュートリアルは、Mobile Services クイック スタートに基づいており、記事に基づき、 [Add Authentication to Existing Mobile Services App] チュートリアルです。 完了してください [Add Authentication to Existing Mobile Services App] 最初です。

## <a name="register-scripts"></a>スクリプトを登録する

1. ログオン、 [Azure classic portal], 、クリックして **Mobile Services**, 、モバイル サービスをクリックします。 クリックして、 **データ** ] タブのをクリックして、 **TodoItem** テーブルです。

2. をクリックして **スクリプト**, を選択、 **挿入** 操作が既存のスクリプトを次の関数に置き換え、クリックして **保存**します。 

        function insert(item, user, request) {
          item.userId = user.userId;
          request.execute();
        }

    このスクリプトは、挿入前に項目に認証済みユーザーのユーザー ID を追加します。

    >[AZURE.NOTE] 確認して [動的スキーマ](https://msdn.microsoft.com/library/azure/jj193175.aspx) が有効になっています。 それ以外の場合、 *userId* 列が自動的に追加されていません。 新しいモバイル サービスの既定ではこの設定は有効になります。

3. 同様に、置き換える既存 **読み取り** 操作を次の関数です。 このスクリプトは、ユーザーが自身に挿入した項目だけを受信できるように、返された TodoItem オブジェクトをフィルターします。

        function read(query, user, request) {
           query.where({ userId: user.userId });
           request.execute();
        }

## <a name="test-app"></a>アプリケーションをテストする

1. 既に項目と実行する、クライアント側のアプリケーションがあることに注意してください、 _TodoItem_ テーブルの前のチュートリアルでは、項目が返されない。 これは、ユーザーの ID 列を含まない前の項目が挿入され、現在は null 値であるために発生します。 新しくことを確認、追加された項目に関連付けられている userId 値がある、 _TodoItem_ テーブルです。

2. その他のログイン アカウントがある場合は、アプリケーションを終了して削除し、もう一度実行することによって、ユーザーは自分のデータのみ表示できることを確認します。 ログインの資格情報ダイアログ ボックスが表示される場合、別のログインを入力し、前のログインで入力した項目が表示されないことを確認します。

<!-- Anchors. -->
[Register server scripts]: #register-scripts
[Next Steps]:#next-steps

<!-- Images. -->

<!-- URLs. -->

[Windows Push Notifications & Live Connect]: http://go.microsoft.com/fwlink/p/?LinkID=257677
[Mobile Services server script reference]: http://go.microsoft.com/fwlink/p/?LinkId=262293
[My Apps dashboard]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Add Authentication to Existing Mobile Services App]: /develop/mobile/tutorials/get-started-with-users-ios

[Azure classic portal]: https://manage.windowsazure.com/
 

