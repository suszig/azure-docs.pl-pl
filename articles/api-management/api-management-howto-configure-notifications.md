<properties 
    pageTitle="Azure API Management で通知と電子メール テンプレートを構成する方法" 
    description="Azure API Management で通知と電子メール テンプレートを構成する方法について説明します。" 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="12/07/2015" 
    ms.author="sdanie"/>

# Azure API Management で通知と電子メール テンプレートを構成する方法

API Management には、特定のイベントの通知を構成する機能と、API Management インスタンスの管理者および開発者との通信に使用される電子メール テンプレートを構成する機能があります。 このトピックでは、使用可能なイベントの通知を構成する方法について説明し、これらのイベントに使用できる電子メール テンプレートを構成する方法の概要を示します。

## <a name="publisher-notifications"> </a>発行者通知の構成

通知を構成するにはクリックして **管理** 、API Management サービスの Azure クラシック ポータルで。 API Management パブリッシャー ポータルが表示されます。

![パブリッシャー ポータル][api-management-management-console]

>API Management サービス インスタンスを作成がない場合は、次を参照してください。 [API Management サービス インスタンスの作成][] で、 [Azure API Management を使ってみる][] チュートリアルです。

クリックして **通知** から、 **API Management** 使用可能な通知を表示するには、左のメニュー。

![発行者通知][api-management-publisher-notifications]

通知に対して、次のイベントを構成できます。

-   **サブスクリプション要求 (承認が必要)** -指定された電子メール受信者およびユーザーに承認を必要とする API 成果物のサブスクリプション要求に関する電子メール通知を受信します。
-   **新しいサブスクリプション** -指定された電子メール受信者およびユーザーに新しい API 成果物のサブスクリプションに関する電子メール通知を受信します。
-   **アプリケーション ギャラリー要求** -指定された電子メール受信者およびユーザーは、新しいアプリケーションがアプリケーション ギャラリーに送信されたときに、電子メール通知に表示されます。
-   **[Bcc]** -指定された電子メール受信者およびユーザーに開発者に送信されるすべての電子メールの電子メール ブラインド カーボン コピーを受信します。
-   **新しい問題またはコメント** - 指定された電子メール受信者およびユーザーに問題が新しい場合に電子メール通知を受信するか、コメントが開発者ポータルに送信されました。
-   **アカウントの削除メッセージ** -指定された電子メール受信者およびユーザーは、アカウントが閉じられたときに、電子メール通知に表示されます。
-   **サブスクリプション クォータ制限間近** -次の電子メール受信者およびユーザーは、サブスクリプションの使用量の使用量クォータに近づいたときに、電子メール通知に表示されます。

それぞれのイベントに対し、電子メール アドレス テキスト ボックスを使用して電子メール受信者を指定するか、または一覧からユーザーを選択できます。

通知先の電子メール アドレスを指定するには、電子メール アドレス テキスト ボックスに電子メール アドレスを入力します。 複数の電子メール アドレスを指定するには、電子メール アドレスをコンマで区切ります。

![通知の受信者][api-management-email-addresses]

通知を受け取るユーザーを指定する] をクリックして **[受信者の追加**, 、] をクリックして、通知が、ユーザーは、横のチェック ボックス **OK**します。

>一覧には管理者のみが表示される点に注意してください。

通知の受信者を構成すると、次のようにクリックします。 **保存** に更新された通知の受信者を適用します。

>外に移動する場合、 **発行者通知** の変更保存されていない場合、パブリッシャー ポータル] タブ警告します。

## <a name="email-templates"> </a>電子メール テンプレートの構成

API Management には、サービスの管理および使用に関連して送信する電子メール メッセージ向けの電子メール テンプレートが用意されています。 次の電子メール テンプレートが用意されています。

-   承認されたアプリケーション ギャラリーへの送信
-   開発者向けのお別れのあいさつ
-   開発者向けのクォータ制限間近通知
-   ユーザーの招待
-   問題に追加された新しいコメント
-   新しい問題の受信
-   アクティブ化された新しいサブスクリプション
-   サブスクリプションの更新の確認
-   サブスクリプション要求の拒否
-   サブスクリプション要求の受信

これらのテンプレートは、必要に応じて変更できます。

表示して、API Management インスタンスの電子メール テンプレートの構成、クリックして **通知** から、 **API Management** 選択し、左側のメニュー、 **電子メール テンプレート** ] タブをクリックします。

![Email templates][api-management-email-templates]

表示または特定のテンプレートを変更、選択してから、 **テンプレート** ボックスの一覧です。

![電子メール テンプレートの一覧][api-management-email-templates-list]

それぞれの電子メール テンプレートは、件名がプレーンテキスト形式で記述され、本文定義が HTML 形式で記述されています。 必要に応じてそれぞれの項目をカスタマイズできます。

![電子メール テンプレート エディター][api-management-email-template]

 **パラメーター** 、パラメーターの一覧が表示されると件名または本文に挿入されます、電子メールが送信されるときに、指定された値を交換してください。 パラメーターを挿入するには、パラメーターを挿入する場所にカーソルを置いた状態で、パラメーター名の左側の矢印をクリックします。

をクリックして **プレビュー** または **テスト送信** には、電子メールの参照やテスト電子メールの送信の方法を参照してください。

>プレビュー時またはテスト用の電子メールの送信時は、パラメーターは実際の値で置き換えられません。
>
>電子メール テンプレートに変更を保存するにはクリックして **保存**, 、またはキャンセルの変更] をクリックする **キャンセル**します。



[api-management-management-console]: ./media/api-management-howto-configure-notifications/api-management-management-console.png
[api-management-publisher-notifications]: ./media/api-management-howto-configure-notifications/api-management-publisher-notifications.png
[api-management-email-addresses]: ./media/api-management-howto-configure-notifications/api-management-email-addresses.png


[api-management-email-templates]: ./media/api-management-howto-configure-notifications/api-management-email-templates.png
[api-management-email-templates-list]: ./media/api-management-howto-configure-notifications/api-management-email-templates-list.png
[api-management-email-template]: ./media/api-management-howto-configure-notifications/api-management-email-template.png


[Configure publisher notifications]: #publisher-notifications
[Configure email templates]: #email-templates

[How to create and use groups]: api-management-howto-create-groups.md
[How to associate groups with developers]: api-management-howto-create-groups.md#associate-group-developer

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
