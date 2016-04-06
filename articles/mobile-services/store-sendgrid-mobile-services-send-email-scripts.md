<properties
    pageTitle="SendGrid を使用した電子メールの送信 | Microsoft Azure"
    description="SendGrid サービスを使用して Azure Mobile Services アプリケーションから電子メールを送信する方法について説明します。"
    services="mobile-services"
    documentationCenter=""
    authors="Erikre"
    manager="sendgrid"
    editor=""/>


<tags 
    ms.service="mobile-services" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="11/30/2015" 
    ms.author="Erikre"/>


# SendGrid を使用した Mobile Services からの電子メールの送信

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

&nbsp;


このトピックでは、モバイル サービスに電子メール機能を追加する方法について説明します。 このチュートリアルでは、サーバー側スクリプトを追加し、SendGrid を使用して電子メールを送信します。 完了すると、モバイル サービスは、レコードが挿入されるたびに電子メールを送信します。

SendGrid は、 [cloud-based email service] 信頼性を提供する [transactional email delivery], 、スケーラビリティ、およびリアルタイム分析のためカスタム統合も容易柔軟な Api 機能します。 詳細については、次を参照してください。 <http://sendgrid.com>します。

このチュートリアルでは、電子メール機能を有効にするための、次の基本的な手順について説明します。

1. [SendGrid アカウントを作成する]
2. [電子メールを送信するためのスクリプトを追加する]
3. [データを挿入して電子メールを受け取る]

このチュートリアルは、モバイル サービスのクイック スタートに基づいています。 このチュートリアルを開始する前に完了しておく必要 [Get started with Mobile Services]します。

## <a name="sign-up"></a>新しい SendGrid アカウントを作成する

[AZURE.INCLUDE [sendgrid-sign-up](../../includes/sendgrid-sign-up.md)]

## <a name="add-script"></a>電子メールを送信する新しいスクリプトを登録する

1. ログオン、 [Azure classic portal], をクリックして **Mobile Services**, 、モバイル サービスをクリックします。

2. Azure クラシック ポータルで、クリックして、 **データ** ] タブでをクリックし、 **TodoItem** テーブルです。

    ![][1]

3.  **Todoitem**, をクリックして、 **スクリプト** ] タブで選択し、 **挿入**します。

    ![][2]

    これで、挿入が発生したときに呼び出される関数が表示されます、 **TodoItem** テーブルです。

4. 挿入関数を次のコードに置き換えます。

        var SendGrid = require('sendgrid').SendGrid;

        function insert(item, user, request) {
            request.execute({
                success: function() {
                    // After the record has been inserted, send the response immediately to the client
                    request.respond();
                    // Send the email in the background
                    sendEmail(item);
                }
            });

            function sendEmail(item) {
                var sendgrid = new SendGrid('**username**', '**password**');

                sendgrid.send({
                    to: '**email-address**',
                    from: '**from-address**',
                    subject: 'New to-do item',
                    text: 'A new to-do was added: ' + item.text
                }, function(success, message) {
                    // If the email failed to send, log it as an error so we can investigate
                    if (!success) {
                        console.error(message);
                    }
                });
            }
        }

5. スクリプトに含まれるプレースホルダーを次に示す正しい値に置き換えます。

    - **_ユーザー名_ と _パスワード_**: SendGrid の資格情報」で指定した [Create a SendGrid account]します。

    - **_電子メール アドレス_**: 電子メールの送信先アドレス。 実際のアプリケーションでは、テーブルを使用して、電子メール アドレスを保存および取得することができます。 アプリケーションをテストする際には、自分の電子メールを使用してください。

    - **_差出人アドレス_**: 電子メールの送信元アドレス。 組織に属する登録済みドメイン アドレスの使用を検討してください。

     > [AZURE.NOTE] 登録済みドメインがあるない場合は、形式で、モバイル サービスのドメインを代わりに使用できます *@ 通知_モバイル サービスでは、_.azure-mobile.net*します。 ただし、モバイル サービス ドメインに送信されたメッセージは無視されます。

6. クリックして、 **保存** ] ボタンをクリックします。 レコードが挿入されるたびに電子メールを送信するスクリプトが構成されましたが、 **TodoItem** テーブルです。

## <a name="insert-data"></a>テスト データを挿入して電子メールを受け取る

1. クライアント アプリケーション プロジェクトで、クイック スタート アプリケーションを実行します。

    このトピックでは、クイック スタートの Windows ストア バージョンを示します。

2. アプリケーションで、テキストを入力 **Insert a TodoItem**, 、クリックして **保存**します。

    ![][3]

3. 次に示す通知のような電子メールが届きます。

    ![][4]

    これで、SendGrid を使用して電子メールを送信できるようにモバイル サービスを構成できました。

## <a name="nextsteps"> </a>次のステップ

これをモバイル サービスで SendGrid 電子メール サービスを使用するがいかに簡単かきましたが、
これらのリンクを SendGrid の詳細について参照してください。

-   SendGrid API に関するドキュメント:
    <https://sendgrid.com/docs>
-   Azure ユーザー向けの SendGrid 特別プラン:
    <https://sendgrid.com/windowsazure.html>

<!-- Anchors. -->
[Create a SendGrid account]: #sign-up
[Add a script to send email]: #add-script
[Insert data to receive email]: #insert-data

<!-- Images. -->
[1]: ./media/store-sendgrid-mobile-services-send-email-scripts/mobile-portal-data-tables.png
[2]: ./media/store-sendgrid-mobile-services-send-email-scripts/mobile-insert-script-push2.png
[3]: ./media/store-sendgrid-mobile-services-send-email-scripts/mobile-quickstart-push1.png
[4]: ./media/store-sendgrid-mobile-services-send-email-scripts/mobile-receive-email.png

<!-- URLs. -->
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started
[sign up page]: https://sendgrid.com/windowsazure.html
[Multiple User Credentials page]: https://sendgrid.com/credentials
[Azure classic portal]: https://manage.windowsazure.com/
[cloud-based email service]: https://sendgrid.com/email-solutions
[transactional email delivery]: https://sendgrid.com/transactional-email




