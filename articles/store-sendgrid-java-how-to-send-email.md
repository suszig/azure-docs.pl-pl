<properties 
    pageTitle="SendGrid 電子メール サービスの使用方法 (Java) | Microsoft Azure" 
    description="Azure で SendGrid 電子メール サービスを使用して電子メールを送信する方法について説明します。コード サンプルは Java で記述されています。" 
    services="" 
    documentationCenter="java" 
    authors="thinkingserious" 
    manager="sendgrid" 
    editor="mollybos"/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="10/30/2014" 
    ms.author="elmer.thomas@sendgrid.com; erika.berkland@sendgrid.com; vibhork"/>

# Java から SendGrid を使用して電子メールを送信する方法

このガイドでは、Azure の SendGrid 電子メール サービスを使用して
一般的なプログラム タスクを実行する方法を紹介します。 サンプルは Java で記述
されています。 紹介するシナリオ **電子メールの作成**, 、* * を送信します。
電子メール**, 、* * 添付ファイルの追加**, 、**フィルターを使用して**, 、および * * の更新
* * プロパティ。 SendGrid と電子メールの送信の詳細については、
[次のステップ](#next-steps) セクションです。

## SendGrid 電子メール サービスとは

SendGrid は、[クラウド ベース電子メール サービス] を信頼性の高いを提供します。
[トランザクション電子メール配信]、スケーラビリティ、およびリアルタイム分析の柔軟な Api 機能
を備えているためカスタム統合も容易です。 SendGrid の一般的な使用シナリオを以下に
示します。

-   顧客に受信通知を自動送信する
-   顧客に広告メールを月 1 回送信するための
    配布リストを管理する
-   ブロックされた電子メールや顧客の応答性などを表すメトリックをリアルタイムで
    収集する
-   傾向を認識するために役立つレポートを生成する
-   顧客の問い合わせを転送する
- アプリケーションからの電子メール通知

詳細については、次を参照してください。 <http://sendgrid.com>します。

## SendGrid アカウントを作成する

[AZURE.INCLUDE [sendgrid-sign-up](../includes/sendgrid-sign-up.md)]

## 方法: javax.mail ライブラリを使用する

javax.mail ライブラリを
<http://www.oracle.com/technetwork/java/javamail> インポートして
コードにインポートします。 大まかに言えば、SMTP を使用して電子メールを送信するための javax.mail ライブラリを使用するプロセス
では、次の操作を実行します。

1.  SMTP サーバーなど、SMTP の値を指定します。
    これは、SendGrid では smtp.sendgrid.net です。

```
        import java.util.Properties;
        import javax.activation.*;
        import javax.mail.*;
        import javax.mail.internet.*;

        public class MyEmailer {
           private static final String SMTP_HOST_NAME = "smtp.sendgrid.net";
           private static final String SMTP_AUTH_USER = "your_sendgrid_username";
           private static final String SMTP_AUTH_PWD = "your_sendgrid_password";

           public static void main(String[] args) throws Exception{
              new MyEmailer().SendMail();
           }

           public void SendMail() throws Exception
           {
              Properties properties = new Properties();
              properties.put("mail.transport.protocol", "smtp");
              properties.put("mail.smtp.host", SMTP_HOST_NAME);
              properties.put("mail.smtp.port", 587);
              properties.put("mail.smtp.auth", "true");
              // …
```

2.  拡張、 *javax.mail.Authenticator*
    クラスを拡張し、
    *getPasswordAuthentication* メソッド
    SendGrid のユーザー名とパスワードを返します。

        private class SMTPAuthenticator extends javax.mail.Authenticator {
        public PasswordAuthentication getPasswordAuthentication() {
           String username = SMTP_AUTH_USER;
           String password = SMTP_AUTH_PWD;
           return new PasswordAuthentication(username, password);
        }

3.  認証された電子メール セッションを
    *javax.mail.Session* オブジェクトです。

        Authenticator auth = new SMTPAuthenticator();
        Session mailSession = Session.getDefaultInstance(properties, auth);

4.  メッセージを作成し、割り当てる **に**, 、**から**, 、**サブジェクト** と
    コンテンツの値を設定します。 これは、コードは「、 [操作方法: 電子メールを作成する](#bkmk_HowToCreateEmail) セクションです。
5.  メッセージを
    *javax.mail.Transport* オブジェクトです。 この
    に [操作方法: 電子メールを送信する ][how to: send an email]
    」セクションを参照してください。

## 方法: 電子メールを作成する

次のコードは電子メールに関する値を指定する方法を示しています。

    MimeMessage message = new MimeMessage(mailSession);
    Multipart multipart = new MimeMultipart("alternative");
    BodyPart part1 = new MimeBodyPart();
    part1.setText("Hello, Your Contoso order has shipped. Thank you, John");
    BodyPart part2 = new MimeBodyPart();
    part2.setContent(
        "<p>Hello,</p>
        <p>Your Contoso order has <b>shipped</b>.</p>
        <p>Thank you,<br>John</br></p>",
        "text/html");
    multipart.addBodyPart(part1);
    multipart.addBodyPart(part2);
    message.setFrom(new InternetAddress("john@contoso.com"));
    message.addRecipient(Message.RecipientType.TO,
       new InternetAddress("someone@example.com"));
    message.setSubject("Your recent order");
    message.setContent(multipart);

## 方法: 電子メールを送信する

次のコードは電子メールを送信する方法を示しています。

    Transport transport = mailSession.getTransport();
    // Connect the transport object.
    transport.connect();
    // Send the message.
    transport.sendMessage(message, message.getAllRecipients());
    // Close the connection.
    transport.close();

## 方法: 添付ファイルを追加する

次のコードは添付ファイルを追加する方法を示しています。

    // Local file name and path.
    String attachmentName = "myfile.zip";
    String attachmentPath = "c:\\myfiles\\"; 
    MimeBodyPart attachmentPart = new MimeBodyPart();
    // Specify the local file to attach.
    DataSource source = new FileDataSource(attachmentPath + attachmentName);
    attachmentPart.setDataHandler(new DataHandler(source));
    // This example uses the local file name as the attachment name.
    // They could be different if you prefer.
    attachmentPart.setFileName(attachmentName);
    multipart.addBodyPart(attachmentPart);

## 方法: フィルターを使用してフッター、追跡、および分析を有効にする

SendGrid では、"フィルター" を使用することで追加の電子メール機能を
*フィルター*します。 できます。
その設定を電子メール メッセージに追加することで、クリック追跡、Google 分析、
サブスクリプション追跡などの独自の機能を有効にすることができます。 すべてのフィルターの一覧については、
参照してください [フィルターの設定][]します。

-   次に、送信される電子メールの下部に表示される HTML テキストになる
    フッター フィルターを挿入する方法を示します。

        message.addHeader("X-SMTPAPI", 
            "{\"filters\": 
            {\"footer\": 
            {\"settings\": 
            {\"enable\":1,\"text/html\": 
            \"<html><b>Thank you</b> for your business.</html>\"}}}}");

-   別のフィルターの例として、クリック追跡があります。 電子メールのテキストに次のような
    ハイパーリンクが含まれており、そのクリック率を
    追跡するとします。

        messagePart.setContent(
            "Hello,
            <p>This is the body of the message. Visit 
            <a href='http://www.contoso.com'>http://www.contoso.com</a>.</p>
            Thank you.", 
            "text/html");

-   クリック追跡を有効にするには、次のコードを使用します。

        message.addHeader("X-SMTPAPI", 
            "{\"filters\": 
            {\"clicktrack\": 
            {\"settings\": 
            {\"enable\":1}}}}");


## 方法: 電子メールのプロパティを更新する

使用して一部の電子メールのプロパティを上書きできる **設定 * プロパティ *** または
使用して追加 **追加 * プロパティ ***します。

たとえば、**ReplyTo** アドレスを指定するには、次のコードを使用します。

    InternetAddress addresses[] = 
        { new InternetAddress("john@contoso.com"),
          new InternetAddress("wendy@contoso.com") };
    
    message.setReplyTo(addresses);

**Cc** 受信者を追加するには、次のコードを使用します。

    message.addRecipient(Message.RecipientType.CC, new 
    InternetAddress("john@contoso.com"));

## 方法: その他の SendGrid サービスを使用する

SendGrid の Web ベース API を使用して、Azure アプリケーションから
その他の SendGrid 機能を利用できます。 詳細については、
詳細についてを参照してください、 [SendGrid API に関するドキュメントの][]します。

## 次のステップ

これで、SendGrid 電子メール サービスの基本を学習できました。
さらに詳細な情報が必要な場合は、次のリンク先を参照してください。

* Azure デプロイで SendGrid を使用する方法を示すサンプル: [Azure デプロイで Java から SendGrid を使用して電子メールを送信する方法](store-sendgrid-java-how-to-send-email-example.md)
* SendGrid Java SDK: <https://sendgrid.com/docs/Code_Examples/java.html>
* SendGrid API に関するドキュメント: <https://sendgrid.com/docs/API_Reference/index.html>
* Azure ユーザー向けの SendGrid 特別プラン: <https://sendgrid.com/windowsazure.html>


[http://sendgrid.com]: https://sendgrid.com 
[http://sendgrid.com/pricing.html]: http://sendgrid.com/pricing.html 
[http://www.sendgrid.com/azure.html]: https://www.sendgrid.com/windowsazure.html 
[http://sendgrid.com/features]: https://sendgrid.com/features 
[http://www.oracle.com/technetwork/java/javamail]: http://www.oracle.com/technetwork/java/javamail/index.html 
[filter settings]: https://sendgrid.com/docs/API_Reference/Web_API/filter_settings.html 
[sendgrid api documentation]: https://sendgrid.com/docs/API_Reference/index.html 
[http://sendgrid.com/azure.html]: https://sendgrid.com/windowsazure.html 
[cloud-based email service]: https://sendgrid.com/email-solutions 
[transactional email delivery]: https://sendgrid.com/transactional-email 

