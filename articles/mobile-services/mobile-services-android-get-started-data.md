<properties
    pageTitle="Android (JavaScript バックエンド) 上のデータを使ってみる |Microsoft Azure"
    description="モバイル サービスを使用して Android アプリ (JavaScript バックエンド) でデータを活用する方法について説明します。"
    services="mobile-services"
    documentationCenter="android"
    authors="RickSaling"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/05/2015"
    ms.author="ricksal"/>

# 既存の Android アプリへの Mobile Services の追加 (JavaScript バックエンド)

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

## 概要

<div class="dev-onpage-video-clear clearfix">
<div class="dev-onpage-left-content">

<p>このトピックでは、Azure Mobile Services を使用して Android アプリに永続データを追加する方法について説明します。 このチュートリアルでは、メモリにデータを格納するアプリケーションをダウンロードした後、新しいモバイル サービスを作成し、ローカルではなく Azure Mobile Services にデータを格納し、更新できるようにそれをアプリケーションに統合します。その後、Microsoft Azure クラシック ポータルを使用してアプリケーションの実行中にデータに加えられた変更を表示します。</p>

</div>


<div class="dev-onpage-video-wrapper">
<a href="http://channel9.msdn.com/Series/Windows-Azure-Mobile-Services/Android-Getting-Started-With-Data-Connecting-your-app-to-Windows-Azure-Mobile-Services" target="_blank" class="label">チュートリアルを見る</a> <a style="background-image: url('/media/devcenter/mobile/videos/mobile-android-get-started-data-180x120.png') !important;" href="http://channel9.msdn.com/Series/Windows-Azure-Mobile-Services/Android-Getting-Started-With-Data-Connecting-your-app-to-Windows-Azure-Mobile-Services" target="_blank" class="dev-onpage-video"><span class="icon">ビデオを再生する</span></a><span class="time">15:32</span></div>
</div>


<p>このチュートリアルでは Azure Mobile Services における Android アプリケーションからのデータの保存と取得方法についての詳細を分かりやすく説明します。 したがって、Mobile Services のクイック スタート チュートリアルで完了している手順の多くについても説明します。 モバイル サービスを初めて使用する場合は、最初にチュートリアル「 <a href="/en-us/develop/mobile/tutorials/get-started-android">Mobile Services の使用</a>.</p>

## 前提条件

このチュートリアルを完了するには、以下が必要です。

- Azure アカウント。 アカウントがない場合は、無料試用版のアカウントを数分で作成することができます。 詳細については、「 <a href="http://www.windowsazure.com/en-us/pricing/free-trial/?WT.mc_id=AED8DE357" target="_blank">Azure 無料評価版</a>.


- the [Azure Mobile Services Android SDK];
- the <a  href="https://developer.android.com/sdk/index.html" target="_blank">Android SDK を含む Android Studio 統合開発環境</a>、Android 4.2 以降のバージョンが必要です。 ダウンロードした GetStartedWithData プロジェクトを実行するには、Android 4.2 以降のバージョンが必要です。 ただし、Mobile Services SDK は、Android 2.2 以降で動作します。

## サンプル コード

完成したソース コードを表示するには、次のように移動します。 <a href="https://github.com/Azure/mobile-services-samples/tree/master/GettingStartedWithData/AndroidStudio">ここ</a>.

## GetStartedWithData プロジェクトをダウンロードする

###サンプル コードの取得

[AZURE.INCLUDE [download-android-sample-code](../../includes/download-android-sample-code.md)]

### サンプル コードの確認と実行

[AZURE.INCLUDE [mobile-services-android-run-sample-code](../../includes/mobile-services-android-run-sample-code.md)]

## Azure クラシック ポータルで新しいモバイル サービスを作成します。

[AZURE.INCLUDE [mobile-services-create-new-service-data](../../includes/mobile-services-create-new-service-data.md)]

## 新しいテーブルをモバイル サービスに追加する

[AZURE.INCLUDE [mobile-services-create-new-service-data-2](../../includes/mobile-services-create-new-service-data-2.md)]

## モバイル サービスをデータ アクセスに使用するためにアプリケーションを更新する

[AZURE.INCLUDE [mobile-services-android-getting-started-with-data](../../includes/mobile-services-android-getting-started-with-data.md)]


## 新しいモバイル サービスに対するアプリケーションのテスト

バック エンド ストレージの Mobile Services を使用するようにアプリケーションを更新した後は、Android エミュレーターまたは Android フォンを使用して、Mobile Services に対してアプリケーションをテストします。

1.  **実行** ] メニューのをクリックして **アプリケーションを実行する** プロジェクトを開始します。

    これにより、モバイル サービスから項目を取得するクエリを、クライアント ライブラリを使用して送信するアプリケーションが Android SDK でビルドされ、実行されます。

5. 前回と同様に、意味のあるテキストを入力し、クリックして **追加**します。

    これにより、新しい項目が挿入としてモバイル サービスに送信されます。

3.  [Azure classic portal], をクリックして **Mobile Services**, 、モバイル サービスをクリックします。

4. クリックして、 **データ** ] タブの [ **参照**します。

    ![][9]

    注意して、 **TodoItem** テーブルには、モバイル サービスによって生成されたいくつかの値を持つデータには今すぐが含まれていて、アプリケーションの TodoItem クラスに対応テーブルに列が自動的に追加されています。

これは、時点で、 **データを使ってみる** Android 向けチュートリアルです。

## トラブルシューティング

### Android SDK バージョンの検証

[AZURE.INCLUDE [Verify SDK](../../includes/mobile-services-verify-android-sdk-version.md)]



## 次のステップ

このチュートリアルでは、Android アプリで Mobile Services のデータを操作できるようにするための基本について説明しました。

次は、このチュートリアルで作成した GetStartedWithData アプリケーションに基づく次のいずれかのチュートリアルを行うことをお勧めします。

* [データ検証および変更スクリプトを使用]
  <br/>モバイル サービスでサーバー スクリプトを使用して、アプリケーションから送信されたデータを検証および変更する方法について説明します。

* [ページングを使用したクエリの改善]
  <br/>クエリ内でページングを使用して、単一の要求で渡されるデータの量を制御する方法について説明します。

データ シリーズを完了した後は、次に示す、Android に関する他のチュートリアルを行うことをお勧めします。

* [認証の使用の開始を取得]
    <br/>アプリケーションのユーザーを認証する方法について説明します。

* [開始プッシュ通知の使用]
  <br/>Mobile Services を使用してアプリケーションにごく基本的なプッシュ通知を送信する方法について説明します。

<!-- Anchors. -->
[Download the Android app project]: #download-app
[Create the mobile service]: #create-service
[Add a data table for storage]: #add-table
[Update the app to use Mobile Services]: #update-app
[Test the app against Mobile Services]: #test-app
[Next Steps]:#next-steps

<!-- Images. -->
[8]: ./media/mobile-services-android-get-started-data/mobile-dashboard-tab.png
[9]: ./media/mobile-services-android-get-started-data/mobile-todoitem-data-browse.png
[12]: ./media/mobile-services-android-get-started-data/mobile-eclipse-project.png
[13]: ./media/mobile-services-android-get-started-data/mobile-quickstart-startup-android.png
[14]: ./media/mobile-services-android-get-started-data/mobile-services-import-android-workspace.png
[15]: ./media/mobile-services-android-get-started-data/mobile-services-import-android-project.png


<!-- URLs. -->
[Validate and modify data with scripts]: /develop/mobile/tutorials/validate-modify-and-augment-data-dotnet
[Refine queries with paging]: /develop/mobile/tutorials/add-paging-to-data-android
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started-android
[Get started with data]: /develop/mobile/tutorials/get-started-with-data-android
[Get started with authentication]: /develop/mobile/tutorials/get-started-with-users-android
[Get started with push notifications]: /develop/mobile/tutorials/get-started-with-push-android

[Azure classic portal]: https://manage.windowsazure.com/
[Azure Mobile Services Android SDK]: http://aka.ms/Iajk6q
[GitHub]:  http://go.microsoft.com/fwlink/p/?LinkID=282122
[Android SDK]: https://go.microsoft.com/fwLink/p/?LinkID=280125

