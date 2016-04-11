MongoLab URI をコードに貼り付けることはできますが、管理しやすくするため環境内で構成することをお勧めします。 このようにすると、URI が変化した場合、コードに手を加えなくても Azure ポータルで変更できます。


1. Azure ポータルで [ **Web Apps**します。
1. Web Apps の一覧に表示されている Web アプリの名前をクリックします。  
![WebAppEntry][entry-website]  
Web アプリのダッシュボードが表示されます。

1. クリックして **構成** メニュー バーです。  
![WebAppDashboardConfig][focus-mongolab-websitedashboard-config]

1. 下方向へ [接続文字列] セクションまでスクロールします。  
![WebAppConnectionStrings][focus-mongolab-websiteconnectionstring]

1.  **名前**, 、「mongolab_uri」と入力してください。
1.  **値**, 、前のセクションで取得した接続文字列を貼り付けます。
1. 選択 **カスタム** で、 **型** ドロップダウン リスト (既定ではなく **SQLAzure**)。
1. クリックして **保存** ツールバーです。  
![SaveWebApp][button-website-save]

**注:** Azure では追加、 **customconnstr \_ という** 上のコードは、この変数にプレフィックス **\_mongolab_uri します。**

[entry-website]: ./media/howto-save-connectioninfo-mongolab/entry-website.png
[focus-mongolab-websitedashboard-config]: ./media/howto-save-connectioninfo-mongolab/focus-mongolab-websitedashboard-config.png
[focus-mongolab-websiteconnectionstring]: ./media/howto-save-connectioninfo-mongolab/focus-mongolab-websiteconnectionstring.png
[button-website-save]: ./media/howto-save-connectioninfo-mongolab/button-website-save.png

