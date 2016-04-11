<properties
   pageTitle="Internet Explorer 用アクセス パネル拡張機能のトラブルシューティング | Microsoft Azure"
   description="グループ ポリシーを使用してマイ アプリ ポータル用の Internet Explorer アドオンをデプロイする方法。"
   services="active-directory"
   documentationCenter=""
   authors="liviodlc"
   manager="stevenpo"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/28/2015"
   ms.author="liviodlc"/>

#Internet Explorer 用アクセス パネル拡張機能のトラブルシューティング

この記事は、次の問題をトラブルシューティングする際に役立ちます。

- Internet Explorer を使用中にマイ アプリ ポータルからアプリにアクセスできない。
- ソフトウェアをインストール済みでも "ソフトウェアをインストールしてください" というメッセージが表示される。

管理者の場合は、関連項目: [グループ ポリシーを使用して Internet Explorer 用のアクセス パネルの拡張機能を展開する方法](active-directory-saas-ie-group-policy.md)

##診断ツールの実行

アクセス パネルの診断ツールをダウンロードし実行することで、アクセス パネル拡張機能のインストールに関する問題を診断することができます。

1. [ここをクリックして診断ツールをダウンロードします。](https://account.activedirectory.windowsazure.com/applications/AccessPanelExtensionDiagnosticTool/AccessPanelExtensionDiagnosticTool.zip)

2. ファイルを開き、キーを押します **すべて抽出** ] ボタンをクリックします。

    ![Press Extract All](./media/active-directory-saas-ie-troubleshooting/extract1.png)

3. キーを押します、 **抽出** 続行する] ボタンをクリックします。

    ![Press Extract](./media/active-directory-saas-ie-troubleshooting/extract2.png)

4. という名前のファイルを右クリックし、ツールを実行する **AccessPanelExtensionDiagnosticTool**, 選択してから、 **プログラムから開く > Microsoft Windows ベースのスクリプト ホスト**します。

    ![Open with > Microsoft Windows Based Script Host](./media/active-directory-saas-ie-troubleshooting/open_tool.png)

5. インストールの問題点を示す次の診断ウィンドウが表示されます。

    ![A sample of the diagnostic window](./media/active-directory-saas-ie-troubleshooting/tool_preview.png)

6. をクリックして"**はい**"プログラムが検出された問題を修正するようにします。

7. これらの変更を保存するには、すべての Internet Explorer ウィンドウを閉じるし、Internet Explorer をもう一度開きます。<br />まだアプリにアクセスできない場合は、次の手順を実行します。

##アクセス パネル拡張機能が有効になっていることを確認する

Internet Explorer でアクセス パネル拡張機能が有効になっていることを確認するには:

1. Internet Explorer で、をクリックして、 **歯車アイコン** ウィンドウの右上隅にします。 選択し、 **インターネット オプション**します。<br />(以前のバージョンの Internet Explorer で、この [を検索できます **ツール > [インターネット オプション**します。

    ![Go to Tools > Internet Options](./media/active-directory-saas-ie-troubleshooting/internetoptions.png)

2. をクリックして、 **プログラム** ] タブから、[、 **アドオンを管理する** ] ボタンをクリックします。

    ![Click Manage Add-Ons](./media/active-directory-saas-ie-troubleshooting/internetoptions_programs.png)

3. このダイアログ ボックスで選択 **アクセス パネルの拡張機能** ] をクリックし、 **を有効にする** ] ボタンをクリックします。

    ![Click Enable](./media/active-directory-saas-ie-troubleshooting/enableaddon.png)

4. 変更内容を保存するために、すべての Internet Explorer ウィンドウを閉じてから再び Internet Explorer を開きます。

##InPrivate ブラウズで拡張機能を有効にする

InPrivate ブラウズ モードを使用している場合:

1. Internet Explorer で、をクリックして、 **歯車アイコン** ウィンドウの右上隅にします。 選択し、 **インターネット オプション**します。<br />(以前のバージョンの Internet Explorer で、この [を検索できます **ツール > [インターネット オプション**します。

    ![A sample of the diagnostic window](./media/active-directory-saas-ie-troubleshooting/inprivateoptions.png)

2. 移動して、 **プライバシー** ] タブ、[ **をオフに** というラベルが付いたチェック ボックスをオン **InPrivate ブラウズが開始されるときに、ツールバーと拡張機能を無効にします。**</p>

    ![Uncheck Disable toolbars and extensions when InPrivate Browsing starts](./media/active-directory-saas-ie-troubleshooting/enabletoolbars.png)

3. 変更内容を保存するために、すべての Internet Explorer ウィンドウを閉じてから再び Internet Explorer を開きます。

##アクセス パネル拡張機能のアンインストール

コンピューターからアクセス パネル拡張機能をアンインストールするには:

1. キーボード キーを押して、 **Windows キー** を [スタート] メニューを開きます。 メニューが開いているときに好きな文字を入力して、検索を行うことができます。 [コントロール パネル] を入力し、開きます、 **コントロール パネルの [** 検索結果に表示される場合。

    ![Search for Control Panel](./media/active-directory-saas-ie-troubleshooting/search_sm.png)

2. コントロール パネルの右上隅で変更、 **によって表示** オプションを **大きいアイコン**します。 検索して] をクリックし、 **プログラムと機能** ] ボタンをクリックします。

    ![Chang the view to show Large Icons](./media/active-directory-saas-ie-troubleshooting/control_panel.png)

3. リストから、次のように選択します。 **アクセス パネルの拡張機能**, 、] をクリックし、 **アンインストール** ] ボタンをクリックします。

    ![Click Uninstall](./media/active-directory-saas-ie-troubleshooting/uninstall.png)

4. 拡張機能をもう一度インストールして、問題が解決されたかどうかを確認することができます。

拡張機能のアンインストールの問題が発生する場合は、削除することもを使用して、 [Microsoft Fix It](https://go.microsoft.com/?linkid=9779673) ツールです。

[AZURE.INCLUDE [saas-toc](../../includes/active-directory-saas-toc.md)]


