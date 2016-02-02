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


# Internet Explorer 用アクセス パネル拡張機能のトラブルシューティング

この記事は、次の問題をトラブルシューティングする際に役立ちます。

- Internet Explorer を使用中にマイ アプリ ポータルからアプリにアクセスできない。
- ソフトウェアをインストール済みでも "ソフトウェアをインストールしてください" というメッセージが表示される。

管理者の場合は、関連項目: [グループ ポリシーを使用して Internet Explorer 用のアクセス パネルの拡張機能を展開する方法](active-directory-saas-ie-group-policy.md)

## 診断ツールの実行

アクセス パネルの診断ツールをダウンロードし実行することで、アクセス パネル拡張機能のインストールに関する問題を診断することができます。

1. [ここをクリックすると、診断ツールをダウンロードします。](https://account.activedirectory.windowsazure.com/applications/AccessPanelExtensionDiagnosticTool/AccessPanelExtensionDiagnosticTool.zip)

2. ファイルを開いて **[すべて展開]** クリックします。

    ![Press Extract All](./media/active-directory-saas-ie-troubleshooting/extract1.png)

3. **[展開]** をクリックして続行します。

    ![Press Extract](./media/active-directory-saas-ie-troubleshooting/extract2.png)

4. **AccessPanelExtensionDiagnosticTool** という名前のファイルを右クリックし、**[プログラムから開く] > [Microsoft Windows Based Script Host]** の順にクリックしてツールを実行します。

    ![Open with > Microsoft Windows Based Script Host](./media/active-directory-saas-ie-troubleshooting/open_tool.png)

5. インストールの問題点を示す次の診断ウィンドウが表示されます。

    ![A sample of the diagnostic window](./media/active-directory-saas-ie-troubleshooting/tool_preview.png)

6. **[はい]** をクリックして、見つかった問題をプログラムに修正させます。

7. これらの変更を保存するには、すべての Internet Explorer ウィンドウを閉じるし、Internet Explorer をもう一度開きます。<br />も、アプリにアクセスできない場合は、次の手順を実行してください。

## アクセス パネル拡張機能が有効になっていることを確認する

Internet Explorer でアクセス パネルが有効になっていることを確認するには:

1. Internet Explorer で、ウィンドウの右上隅にある **[歯車]** アイコンをクリックします。 選択し、 **インターネット オプション**.<br />(以前のバージョンの Internet Explorer で、この [を検索できます **ツール > [インターネット オプション**します。

    ![Go to Tools > Internet Options](./media/active-directory-saas-ie-troubleshooting/internetoptions.png)

2. **[プログラム]** タブをクリックして、**[アドオンの管理]** をクリックします。

    ![Click Manage Add-Ons](./media/active-directory-saas-ie-troubleshooting/internetoptions_programs.png)

3. このダイアログで、**[アクセス パネル拡張機能]** を選択して **[有効にする]** ボタンをクリックします。

    ![Click Enable](./media/active-directory-saas-ie-troubleshooting/enableaddon.png)

4. 変更内容を保存するために、すべての Internet Explorer ウィンドウを閉じてから再び Internet Explorer を開きます。

## InPrivate ブラウズで拡張機能を有効にする

InPrivate ブラウズ モードを使用している場合:

1. Internet Explorer で、ウィンドウの右上隅にある **[歯車]** アイコンをクリックします。 選択し、 **インターネット オプション**.<br />(以前のバージョンの Internet Explorer で、この [を検索できます **ツール > [インターネット オプション**します。

    ![A sample of the diagnostic window](./media/active-directory-saas-ie-troubleshooting/inprivateoptions.png)

2. 移動して、 **プライバシー** ] タブ、[ **をオフに** というラベルが付いたチェック ボックスをオン **InPrivate ブラウズが開始されるときに、ツールバーと拡張機能を無効にする**</p>

    ![InPrivate ブラウズが開始されるときに、無効にするツールバーと拡張機能をオフにして](./media/active-directory-saas-ie-troubleshooting/enabletoolbars.png)

3. 変更内容を保存するために、すべての Internet Explorer ウィンドウを閉じてから再び Internet Explorer を開きます。

## アクセス パネル拡張機能のアンインストール

コンピューターからアクセス パネル拡張機能をアンインストールするには:

1. キーボードの **Windows キー**を押して [スタート] メニューを開きます。 メニューが開いているときに好きな文字を入力して、検索を行うことができます。 「コントロール パネル」と入力して、**[コントロール パネル]** が検索結果に表示されたらそれを開きます。

    ![Search for Control Panel](./media/active-directory-saas-ie-troubleshooting/search_sm.png)

2. コントロール パネルの右上隅にある **[表示方法]** を、**[大きいアイコン]** に変更します。 **[プログラムと機能]** を見つけてクリックします。

    ![Chang the view to show Large Icons](./media/active-directory-saas-ie-troubleshooting/control_panel.png)

3. リストから **[アクセス パネル拡張機能]** を選択し、**[アンインストール]** をクリックします。

    ![Click Uninstall](./media/active-directory-saas-ie-troubleshooting/uninstall.png)

4. 拡張機能をもう一度インストールして、問題が解決されたかどうかを確認することができます。

拡張機能のアンインストールの問題が発生する場合は、削除することもを使用して、 [Microsoft Fix It](https://go.microsoft.com/?linkid=9779673) ツールです。

[AZURE.INCLUDE [saas-toc](../../includes/active-directory-saas-toc.md)]





