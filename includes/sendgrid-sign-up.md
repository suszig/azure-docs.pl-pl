Azure ユーザーは 1 か月あたり 25,000 通の電子メールを無料で利用できます。 無料毎月 25,000 通の電子メールにより利用できるように高度なレポートおよび分析と [すべての Api][] (Web、SMTP、イベント、解析など)。 SendGrid の提供するその他のサービスについては、次を参照してください。、 [SendGrid の特徴][] ページです。

### SendGrid アカウントにサインアップするには

1. ログイン、 [Azure 管理ポータル][]します。

2. 管理ポータルの下のウィンドウで、クリックして **新規**します。

    ![command-bar-new][command-bar-new]

3. クリックして **Marketplace**します。

    ![sendgrid-store][sendgrid-store]

4.  **アプリケーションとサービスを選択して** ダイアログで、 **SendGrid** 右矢印をクリックします。

5.  **アプリケーションと個人用設定サービス** ダイアログの [、 **SendGrid** プランにサインアップするを選択します。

6. 識別する名前を入力して、 **SendGrid** Azure 設定でサービス、または既定値の使用 **SendGridEmailDelivery.Simplified.SMTPWebAPI**します。 名前は 1 ～ 100 文字にする必要があります。また、名前に含めることができるのは英数字、ダッシュ、ピリオド、およびアンダースコアのみです。 この名前は、Azure ストアのサブスクライブ項目の一覧で一意になっている必要があります。

    ![store-screen-2][store-screen-2]

7. リージョンの値を選択します (たとえば [米国西部])。

8. 右矢印をクリックします。

9.  **購入の確認** タブで、プランと料金情報および法律条項を確認します。 条項に同意したら、チェックマークをクリックします。 チェック マークをクリックして、SendGrid アカウントが開始されます、 [SendGrid provisioning process]します。

    ![store-screen-3][store-screen-3]

10. 購入を確認したら、アドオン ダッシュ ボードにリダイレクトして、メッセージが表示 **SendGrid の購入**します。

    ![sendgrid-purchasing-message][sendgrid-purchasing-message]

    SendGrid アカウントが直ちにプロビジョニングされ、メッセージが表示 **アドオン SendGrid の購入が完了しました**します。 アカウントと資格情報が作成されました。 この時点で、電子メールを送信する準備が整っています。 

    サブスクリプション プランを変更するか、SendGrid 連絡先設定を表示するには、SendGrid サービスの名前をクリックし、SendGrid Marketplace ダッシュボードを開きます。 

    ![sendgrid-add-on-dashboard][sendgrid-add-on-dashboard]

    SendGrid を使用して電子メールを送信するには、(ユーザー名とパスワード)、アカウントの資格情報を指定する必要があります。

### SendGrid の資格情報を確認するには ###

1. クリックして **接続情報]**します。

    ![sendgrid-connection-info-button][sendgrid-connection-info-button]

2.  *接続情報]* ダイアログ ボックスで、コピー、 **パスワード** し、このチュートリアルの後半で使用するユーザー名。

    ![sendgrid-connection-info][sendgrid-connection-info]

    電子メール配信設定を指定する] をクリックして、 **管理** ] ボタンをクリックします。 これで、SendGrid のコントロール パネルにリダイレクトされます。 

    ![sendgrid-control-panel][sendgrid-control-panel]

    SendGrid の概要の詳細については、次を参照してください。 [SendGrid の概要][]します。

<!--images-->

[command-bar-new]: ./media/sendgrid-sign-up/sendgrid_BAR_NEW.PNG
[sendgrid-store]: ./media/sendgrid-sign-up/sendgrid_offerings_store.png
[store-screen-2]: ./media/sendgrid-sign-up/sendgrid_store_scrn2.png
[store-screen-3]: ./media/sendgrid-sign-up/sendgrid_store_scrn3.png
[sendgrid-purchasing-message]: ./media/sendgrid-sign-up/sendgrid_purchasing_message.png
[sendgrid-add-on-dashboard]: ./media/sendgrid-sign-up/sendgrid_add-on_dashboard.png
[sendgrid-connection-info]: ./media/sendgrid-sign-up/sendgrid_connection_info.png
[sendgrid-connection-info-button]: ./media/sendgrid-sign-up/sendgrid_connection_info_button.png
[sendgrid-control-panel]: ./media/sendgrid-sign-up/sendgrid_control_panel.png

<!--Links-->

[SendGrid Features]: http://sendgrid.com/features
[Azure Management Portal]: https://manage.windowsazure.com
[SendGrid Getting Started]: http://sendgrid.com/docs
[SendGrid Provisioning Process]: https://support.sendgrid.com/hc/articles/200181628-Why-is-my-account-being-provisioned-
[all APIs]: https://sendgrid.com/docs/API_Reference/index.html


