
このチュートリアルを基づいて、 [GetStartedWithMobileServices アプリケーション](http://go.microsoft.com/fwlink/p/?LinkID=510826), 、Visual Studio 2013 のユニバーサル Windows アプリ プロジェクトであります。 このアプリケーションの UI は、追加された項目がメモリにローカルに格納される点を除き、モバイル サービスのクイック スタートで生成したアプリケーションと同じです。 

1. C# バージョンの GetStartedWithMobileServices サンプル アプリケーションをデベロッパー サンプル コード集のサイトからダウンロードします。 

2. Visual Studio 2013 では、ダウンロードしたプロジェクトを開き、GetStartedWithData.Shared プロジェクト フォルダーで検出された MainPage.xaml.cs ファイルを調べます。

    追加された **TodoItem** オブジェクトがメモリ内に格納されている **ObservableCollection & lt;TodoItem & gt;**します。

3. キーを押して、 **f5 キーを押して** キーをプロジェクトをリビルドし、アプリケーションを開始します。

4. アプリケーションにテキストを入力 **Insert a TodoItem**, 、クリックして **保存**します。

    ![](./media/mobile-services-windows-universal-dotnet-download-project/mobile-quickstart-startup.png) 

    保存されたテキストが表示されます。

5. Windows Phone 8.1 プロジェクトを右クリックし、をクリックして **スタートアップ プロジェクトとして設定**, 、キーを押します **f5 キーを押して** Windows Phone ストア アプリを起動します。  

    ![](./media/mobile-services-windows-universal-dotnet-download-project/mobile-quickstart-startup-wp8.png)

6. 手順 3 と 4 を繰り返して、サンプルが同じように動作することを確認します。
