<properties 
    pageTitle="Azure API Management で開発者ポータルの外観をカスタマイズする方法" 
    description="Azure API Management で開発者ポータルの外観をカスタマイズする方法について説明します。" 
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
    ms.date="12/03/2015" 
    ms.author="sdanie"/>

# Azure API Management で開発者ポータルの外観をカスタマイズする方法

開発者ポータルの外観の色、フォント、サイズ、文字間隔、その他の側面は、スタイルのルールによって定義されます。 これらのルールのセットは、ページの構造要素 (ヘッダー、メニュー、コンテンツ本文、ページ タイトルなど) ごとに存在します。このチュートリアルでは、スタイルのルールを変更する方法について説明します。

スタイル ルールを編集するには、をクリックして **外観** から、 **デベロッパー ポータル** パブリッシャー ポータルでのメニュー。 [ **カスタマイズの開始** 、スタイル エディターを有効にします。

ご利用のブラウザーが、開発者ポータル内に置かれている非表示のページに切り替わり、サンプル コンテンツや、サイト上のいたるところに使用されている、スタイル上のさまざまなルールが例示されます。 スタイル エディターを開くには、ページの左端に設けられた縦長の薄い灰色の領域上にカーソルを移動します。 次のようにエディター ツール バーが表示されます。 

![Customization toolbar][api-management-customization-toolbar]

スタイル ルールを編集するための 2 つのメイン モードには、- **すべてのルールを編集** ルール anywhere を使用して、スタイルのすべての一覧が表示されます。 一方 **ピック要素** にしているだけのその特定の要素のスタイルを表示] ページから要素を選択することができます。

このセクションでは、特定の要素 (たとえば、ページ ヘッダー) のスタイルのみを変更することにします。 クリックして、 **ピック要素** スタイル エディターのツールバーからオプションをクリックして **をカスタマイズする要素を選択**します。 要素にマウス カーソルを重ねると、その要素が強調表示され、クリックした場合に適用される要素のスタイルが表示されます。 

ヘッダー内のページ タイトルを表すテキスト上にマウス カーソルを移動してクリックします。 名前とカテゴリを持つ一連のスタイル ルールがスタイル エディター内に表示されます。

それぞれのルールは、選択された要素のスタイルに関するプロパティを表します。 たとえば、上で選択したヘッダー テキストの場合、テキストのサイズは @font-size-h1 で定義され、フォント (代替候補を含む) の名前は @headings-font-family で定義されています。

> 慣れている場合 [ブートス トラップ](http://getbootstrap.com/), 、これらのルールが実際には [LESS 変数](http://getbootstrap.com/css/) 開発者ポータルで使用されている bootstrap テーマです。

ここで、見出しテキストの色を変更してみましょう。 内のエントリを選択して、 **@headings-color** フィールドし、#000000 を入力します。 これは、黒色を表す 16 進コードです。 すると、テキスト ボックスの端に正方形のカラー インジケーターが表示されます。 このインジケーターをクリックするとカラー ピッカーが表示され、色を選択することができます。

![Color picker][api-management-customization-toolbar-color-picker]

クリックして、選択した要素のスタイルに変更を加えたら後は **変更のプレビュー** 画面に結果を確認します。 この時点では、管理者にしか表示されません。 をクリックすると、これらの変更を全員が閲覧するには、 **発行** 、スタイル エディターでボタンをクリックし、変更を確認します。

![Publish form][api-management-customization-toolbar-publish-form]

> その他の要素をページで、手順を適用同じヘッダーのと同様のスタイル ルールを変更する] をクリックして **要素を選択** 、スタイル エディターから、目的を画面に表示されるスタイル ルールの値に変更を開始する要素を選択します。


[Next steps]: #next-steps


[api-management-customization-toolbar]: ./media/api-management-howto-customize-look-and-feel/api-management-customization-toolbar.png
[api-management-customization-toolbar-color-picker]: ./media/api-management-howto-customize-look-and-feel/api-management-customization-toolbar-color-picker.png
[api-management-customization-toolbar-publish-form]: ./media/api-management-howto-customize-look-and-feel/api-management-customization-toolbar-publish-form.png

