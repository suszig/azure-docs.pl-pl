<properties
    pageTitle="Azure API Management での開発者ポータルのカスタマイズ | Microsoft Azure"
    description="Azure API Management での開発者ポータルのカスタマイズ方法について説明します。"
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
    ms.topic="get-started-article"
    ms.date="12/07/2015"
    ms.author="sdanie"/>

# Azure API Management での開発者ポータルのカスタマイズ

このガイドでは、Azure API Management における開発者ポータルのルック アンド フィールをお客様のブランドに合わせてカスタマイズする方法について説明します。

## <a name="change-page-headers"> </a>ページ ヘッダーのテキストまたはロゴを変更する

ポータルの代表的なカスタマイズとして、すべてのページの一番上に表示されるテキストを貴社の会社名またはロゴに置き換えることが挙げられます。

開発者ポータル内のコンテンツに対する変更は、Azure クラシック ポータルから発行者ポータルにアクセスして行います。 クリックして、API パブリッシャー ポータルにアクセス **管理** 、API Management サービスの Azure クラシック ポータルで。

![パブリッシャー ポータル][api-management-management-console]

開発者ポータルは、コンテンツ管理システム (CMS) がベースとなっています。 すべてのページに表示されるヘッダーは、"ウィジェット" と呼ばれる特殊なタイプのコンテンツです。 そのウィジェットの内容を編集する] をクリックして **ウィジェット** から、 **デベロッパー ポータル** 、左側のメニューを選択し、 **ヘッダー** リストからウィジェットです。

![Widgets header][api-management-widgets-header]

内から編集することは、ヘッダーの内容、 **本文** フィールドです。 テキストを"Fabrikam Developer Portal"に変更し、クリックして **保存** ページの下部にあります。

以後、この新しいヘッダーが開発者ポータル内のすべてのページに表示されます。

> パブリッシャー ポータルで開発者ポータルを開くにはクリックして **デベロッパー ポータル** 上部のバーにします。

## <a name="change-headers-styling"> </a>ヘッダーのスタイルを変更する

色、フォント、サイズ、文字間隔など、ポータル ページのスタイルに関連したあらゆる要素は、スタイルのルールによって定義されます。 スタイルを編集する] をクリックして **外観** から、 **デベロッパー ポータル** メニューをクリックして、パブリッシャー ポータルに **カスタマイズの開始** 、スタイル エディターを有効にします。

ブラウザーが開発者ポータル内に置かれている非表示のページに切り替わり、サンプル コンテンツや、サイト上のいたるところに使用されている、スタイル上のさまざまなルールが例示されます。 スタイル エディターを開くには、ページの左端に設けられた縦長の薄い灰色の領域上にカーソルを移動します。 エディター ツール バーが表示されます。

![Customization toolbar][api-management-customization-toolbar]

スタイル ルールの編集の 2 つのメイン モードがあります- **すべてのルールを編集** すべてのスタイル ルールの一覧を表示中に使用される任意の場所、 **ピック要素** を使用しているページから要素を選択することができ、その要素のスタイルのみが表示されます。

このセクションでは、ヘッダーのスタイルのみを変更することにします。 クリックして、 **ピック要素** スタイル エディターのツールバーからオプションをクリックして **をカスタマイズする要素を選択**します。 要素にマウスを重ねると、その要素が強調表示されるようになり、クリックした場合にどの要素のスタイルが編集対象となるかを把握できます。 ヘッダー内の会社名を表すテキスト (前セクションの手順でいうと "Fabrikam Developer Portal") 上にマウス カーソルを移動してクリックします。 名前とカテゴリを持つ一連のスタイル ルールがスタイル エディター内に表示されます。

それぞれのルールは、選択された要素のスタイルに関するプロパティを表します。 たとえば、上で選択したヘッダー テキストの場合、テキストのサイズは @font-size-h1 で定義され、フォント (代替候補を含む) の名前は @headings-font-family で定義されています。

> 慣れている場合 [ブートス トラップ][], 、これらのルールが実際には [LESS 変数][] 開発者ポータルで使用されている bootstrap テーマです。

見出しテキストの色を変更してみましょう。 内のエントリを選択して、 **@headings-color** フィールドと型 **#000000**します。 これは、黒色を表す 16 進コードです。 すると、テキスト ボックスの端に正方形のカラー インジケーターが表示されます。 このインジケーターをクリックするとカラー ピッカーが表示され、色を選択することができます。

![Color picker][api-management-customization-toolbar-color-picker]

クリックして、選択した要素のスタイルに変更が終わったら、 **変更のプレビュー** 画面に結果を確認します。 この時点では、管理者にしか表示されません。 これらの変更をすべてのユーザーに表示するには、クリックして、 **発行** 、スタイル エディターでボタンをクリックし、変更を確認します。

![[発行] メニュー
][api-management-customization-toolbar-publish-form]

> ページの他の要素に適用されるスタイル ルールを変更するには、ヘッダーの場合と同じ手順を実行します。 クリックして **要素を選択** 、スタイル エディターから、目的を画面に表示されるスタイル ルールの値に変更を開始する要素を選択します。

## <a name="edit-page-contents"> </a>ページの内容を編集する

開発者ポータルは、自動的に生成されるページ ([API]、[成果物]、[アプリケーション]、[問題] など) と手動で作成するコンテンツとで構成されます。 コンテンツ管理システムがベースとなっているため、そのようなコンテンツを必要に応じて作成することができます。

コンテンツのページをクリックして、既存のすべての一覧を表示する **コンテンツ** から、 **デベロッパー ポータル** パブリッシャー ポータルでのメニュー。

![Manage content][api-management-customization-manage-content]

クリックして、 **ようこそ** 開発者ポータルのホーム ページに表示される内容を編集するページです。 必要なプレビューし、必要に応じてクリックして変更を加える **[今すぐ発行** すべてのユーザーに表示されるようにします。

> ホーム ページには、最上部にバナーを表示できる特殊なレイアウトが使用されています。 このバナーを編集はできません、 **コンテンツ** セクションです。 このバナーを編集する] をクリックして **ウィジェット** から、 **デベロッパー ポータル** メニューの [ **ホーム ページ** から、 **現在のレイヤー** ドロップダウン リストし、開きます、 **バナー** 項目の下にある、 **セクションの機能を備えた**します。 このウィジェットの内容は、他のページとまったく同じように編集できます。

## <a name="next-steps"> </a>次のステップ

-   [その他のトピックもチェックして、 [高度な API の構成を使ってみる][] チュートリアルです。

[Change the text/logo in the page headers]: #change-page-headers
[Change the styling of the headers]: #change-headers-styling
[Edit the contents of a page]: #edit-page-contents
[Next steps]: #next-steps

[Azure Classic Portal]: https://manage.windowsazure.com/

[api-management-management-console]: ./media/api-management-customize-portal/api-management-management-console.png
[api-management-widgets-header]: ./media/api-management-customize-portal/api-management-widgets-header.png
[api-management-customization-toolbar]: ./media/api-management-customize-portal/api-management-customization-toolbar.png
[api-management-customization-toolbar-color-picker]: ./media/api-management-customize-portal/api-management-customization-toolbar-color-picker.png
[api-management-customization-toolbar-publish-form]: ./media/api-management-customize-portal/api-management-customization-toolbar-publish-form.png
[api-management-customization-manage-content]: ./media/api-management-customize-portal/api-management-customization-manage-content.png


[Get started with advanced API configuration]: api-management-get-started-advanced.md
[bootstrap]: http://getbootstrap.com/
[LESS variables]: http://getbootstrap.com/css/


