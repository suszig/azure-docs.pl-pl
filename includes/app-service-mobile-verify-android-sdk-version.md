Android SDK の開発が継続中であるため、Android Studio にインストールされている Android SDK のバージョンが、コードのバージョンと一致しない可能性があります。 このチュートリアルで参照している Android SDK は、記事の執筆時点で最新のバージョンであるバージョン 23 です。 SDK の新しいリリースが登場するにつれてバージョン番号が大きくなる可能性があるので、入手可能な最新バージョンを使用することをお勧めします。

バージョンの不一致の場合に見られる現象は、次の 2 つです。

1. ビルドまたはプロジェクトを再構築する場合などの Gradle エラー メッセージが表示"**ターゲット Google Inc.:Google APIs:n が見つかりませんでした。**"です。

2. `import` ステートメントに基づく解決が必要なコード内の標準の Android オブジェクトによって、エラー メッセージが生成される場合があります。

このいずれかが発生した場合、Android Studio にインストールされている Android SDK のバージョンが、ダウンロードしたプロジェクトの SDK ターゲットと一致していない可能性があります。  バージョンを確認するには、次の変更を加えます。


1. Android Studio で、[ **ツール** = > **Android** = > **SDK Manager**します。 SDK プラットフォームの最新バージョンをまだインストールしていない場合は、これをクリックしてインストールします。 バージョン番号をメモしておきます。

2. プロジェクト エクスプ ローラー] タブの [ **Gradle スクリプト**, 、ファイルを開く **build.gradle (modeule: app)**します。 いることを確認、 **compileSdkVersion** と **buildToolsVersion** がインストールされている最新の SDK バージョンに設定します。 タグは、次のようになります。
 
            compileSdkVersion 'Google Inc.:Google APIs:23'
            buildToolsVersion "23.0.0"
    
3. Android Studio Project Explorer でプロジェクト ノードを右クリックし、選択 **プロパティ**, 、左の列を選択して **Android**します。 いることを確認、 **Project Build Target** と同じ SDK バージョンに設定されている、 **targetSdkVersion**します。

4. Android Studio では Eclipse と異なり、ターゲットの SDK バージョンと最小の SDK バージョンを指定するためにマニフェスト ファイルを使用しません。

