
このチュートリアルを基づいて、 [GetStartedWithMobileServices アプリケーション](http://go.microsoft.com/fwlink/p/?LinkID=510826), 、Visual Studio 2013 のユニバーサル Windows アプリ プロジェクトであります。 このアプリケーションの UI は、追加された項目がメモリにローカルに格納される点を除き、モバイル サービスのクイック スタートで生成したアプリケーションと同じです。 

1. JavaScript バージョンの GetStartedWithMobileServices サンプル アプリケーションをデベロッパー サンプル コード集のサイトからダウンロードします。 

3. Visual Studio 2013 で、ダウンロードされたプロジェクトを開き、ソリューション エクスプローラーで js フォルダーを展開し、default.js ファイルを調べます。

    追加された **TodoItem** オブジェクトがメモリ内に格納されている `WinJS.Binding.List`します。

4. キーを押して、 **f5 キーを押して** キーをプロジェクトをリビルドし、アプリケーションを開始します。

5. アプリケーションにテキストを入力 **Insert a TodoItem**, 、クリックして **保存**します。

    ![](./media/mobile-services-windows-universal-dotnet-download-project/mobile-quickstart-startup.png) 

    保存されたテキストが表示されます。

6. Windows Phone 8.1 プロジェクトを右クリックし、をクリックして **スタートアップ プロジェクトとして設定**, 、キーを押します **f5 キーを押して** Windows Phone ストア アプリを起動します。  

    ![](./media/mobile-services-windows-universal-dotnet-download-project/mobile-quickstart-startup-wp8.png)

7. 手順 3 と 4 を繰り返して、サンプルが同じように動作することを確認します。
