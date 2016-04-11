<properties
    pageTitle="Azure Active Directory B2C プレビュー: ページの UI カスタマイズ ヘルパー ツール | Microsoft Azure"
    description="Azure Active Directory B2C のページの UI カスタマイズ機能を試すために使用するヘルパー ツール"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="msmbaldwin"
    editor="curtand"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/08/2015"
    ms.author="swkrish"/>

# Azure Active Directory B2C プレビュー: ページのユーザー インターフェイス (UI) カスタマイズ機能を試すために使用するヘルパー ツール

この記事では、対応する、 [メイン UI のカスタマイズの記事](active-directory-b2c-reference-ui-customization.md) Azure Active Directory B2C にします。  以下の手順では、用意されているサンプルの HTML および CSS の内容を使用して、ページの UI カスタマイズ機能を試してみる方法について説明します。

[AZURE.INCLUDE [active-directory-b2c-preview-note](../../includes/active-directory-b2c-preview-note.md)]

## Azure AD B2C テナントを取得する

何かをカスタマイズできますが、前にする必要があります。 [Azure AD B2C テナントを取得する](active-directory-b2c-get-started.md) 既にお持ちでない場合。

## サインアップ ポリシーを作成する

2 つのページをカスタマイズするサンプルの内容を指定している、 [サインアップ ポリシー](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy): [IDP の選択] ページ](active-directory-b2c-reference-ui-customization.md#identity-provider-selection-page) と [ローカル アカウントのサインアップ ページ](active-directory-b2c-reference-ui-customization.md#local-account-sign-up-page)します。   [サインアップ ポリシーの作成](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy), 、ローカル アカウント (電子メール アドレス)、Facebook、および Google + として追加 **Id プロバイダー**します。  サンプルの HTML コンテンツが受け付ける IDP はこれらだけです。

## アプリケーションを登録する

必要があります [アプリケーションを登録する](active-directory-b2c-app-registration.md) B2C テナントのポリシーを実行するために使用できます。  アプリケーションを登録した後、サインアップ ポリシーを実際に実行するにはいくつかのオプションを使用できます。

- クイック スタート アプリケーションの一覧表示する Azure AD B2C のいずれかのビルド [ここ](active-directory-b2c-overview.md#getting-started)します。
- ビルド済みを使用して [Azure AD B2C のプレイ グラウンド](https://aadb2cplayground.azurewebsites.net) アプリケーションです。  使用して B2C テナントでアプリケーションを登録する必要があります、プレイ グラウンドの使用を選択した場合、 **リダイレクト URI** `https://aadb2cplayground.azurewebsites.net/`
- 使用して、 **を今すぐ実行** ] ボタンをクリックして、ポリシーで、 [Azure プレビュー ポータル](https://portal.azure.com)します。

## ポリシーをカスタマイズする

ポリシーの外観と操作性をカスタマイズするには、最初に Azure AD B2C の固有の規則を使用して HTML および CSS ファイルを作成する必要があります。  その後は、静的なコンテンツを、Azure AD B2C がアクセスできる公開されている場所にアップロードできます。  そのような場所としては、独自の専用 Web サーバー、Azure BLOB ストレージ、Azure CDN、その他の静的リソース ホスティング プロバイダーを使用できます。  唯一の要件は、コンテンツが HTTPS 経由で使用でき、CORS を使用してアクセスできることです。  静的コンテンツを Web で公開した後は、この場所を参照してエンド ユーザーにそのコンテンツを表示するようにポリシーを編集できます。   [メイン UI のカスタマイズの記事](active-directory-b2c-reference-ui-customization.md) Azure AD B2C カスタマイズ機能のしくみについて詳しく説明します。  

このチュートリアルでは、既にいくつかのサンプル コンテンツを作成し、Azure BLOB ストレージでホストしてあります。  サンプル コンテンツは、架空の会社 "Contoso B2C" のテーマでの非常に基本的なカスタマイズです。  独自のポリシーで試してみるには、次の手順を実行します。

1. テナントへのサインイン、 [Azure プレビュー ポータル](https://portal.azure.com) B2C 機能ブレードに移動します。
2. クリックして **サインアップ ポリシー** し、サインアップ ポリシー (たとえば、"b2c_1_sign_up") をクリックします。
3. クリックして **ページ UI のカスタマイズ** し **の Id プロバイダーの選択] ページ**します。
4. 表示/非表示、 **カスタム テンプレートを使用** に切り替える **はい**します。  **カスタム ページ URI** フィールドに「 `https://contosob2c.blob.core.windows.net/static/Index.html`します。 Click **OK**.
5. クリックして **ローカル アカウントのサインアップ ページ**します。 表示/非表示、 **カスタム テンプレートを使用** に切り替える **はい**します。  **カスタム ページ URI** フィールドに「 `https://contosob2c.blob.core.windows.net/static/EmailVerification.html`します。 クリックして **OK** 2 回ブレードを閉じ、UI のカスタマイズ。
6. クリックして **保存**します。

カスタマイズしたポリシーを試すことができるようになります。  お望みならをもクリックして場合に、独自のアプリケーションや AAD B2C のプレイ グラウンドを使用できます、 **を今すぐ実行** コマンド ポリシーのブレードをします。  ドロップダウンでアプリケーションを選び、適切なリダイレクト URI を選びます。 クリックして、 **を今すぐ実行** ] ボタンをクリックします。  新しいブラウザー タブが開き、アプリケーションへのサインアップについて、新しいコンテンツが所定の場所に置かれた状態のユーザー エクスペリエンスを確認できます。

## サンプル コンテンツを Azure BLOB ストレージにアップロードする

Azure BLOB ストレージを使用してページのコンテンツをホストする場合は、独自のストレージ アカウントを作成し、B2C ヘルパー ツールを使用してファイルをアップロードできます。  

#### ストレージ アカウントの作成

1. サインイン、 [Azure プレビュー ポータル](https://portal.azure.com/)します。
2. をクリックして **+ 新規** ]-> [ **データ + ストレージ** ]-> [ **ストレージ アカウント**します。 Azure BLOB ストレージ アカウントを作成するには、Azure サブスクリプションが必要です。 無料評価版にサインアップすること [ここ](https://azure.microsoft.com/pricing/free-trial/)します。
3. 提供、 **名前** 、ストレージ アカウント (たとえば、"contoso"など) およびに適切な選択の選択の **価格レベル**, 、**リソース グループ** と **サブスクリプション**します。 あることを確認、 **スタート画面にピン** チェック ボックスをオンします。 クリックして **作成**します。
4. スタート画面に戻り、作成したストレージ アカウントをクリックします。
5. 下にある、 **概要** ] をクリックして **コンテナー** し **+ 追加**します。
6. 提供、 **名前** コンテナー (たとえば、"b2c") と選択 **Blob** として、 **アクセスの種類**します。 Click **OK**.
7. 作成したコンテナーは、上、リストに表示される、 **Blob** ブレードです。 コンテナーの URL (`https://contoso.blob.core.windows.net/b2c` のような形式) をメモしておきます。 閉じる、 **Blob** ブレードです。
8. [ストレージ アカウント] ブレードをクリックして **キー** との値を書き留めて、 **ストレージ アカウント名** と **プライマリ アクセス キー** フィールドです。

> [AZURE.NOTE]
    **プライマリ アクセス キー** は、重要なセキュリティ資格情報。

#### ヘルパー ツールとサンプル ファイルをダウンロードする

ダウンロードすることができます、 [ここ .zip として Azure Blob ストレージ ヘルパー ツールとサンプル ファイル](https://github.com/azureadquickstarts/b2c-azureblobstorage-client/archive/master.zip) または GitHub から複製します。

```
git clone https://github.com/azureadquickstarts/b2c-azureblobstorage-client
```

このリポジトリには `sample_templates\contoso` ディレクトリが含まれており、そこには HTML、CSS、イメージの例が含まれています。  これらのテンプレートで独自の Azure BLOB ストレージ アカウントを参照するには、HTML ファイルを編集する必要があります。  `Index.htnml` および `EmailValidation.html` を開き、`https://localhost` のすべてのインスタンスを、前の手順でコピーした独自のコンテナーの URL に置き換えます。  この場合、HTML は Azure AD によってドメイン `https://login.microsoftonline.com` で提供されるので、HTML ファイルの絶対パスを使用する必要があります。

#### サンプル ファイルをダウンロードする

同じリポジトリで、`B2CAzureStorageClient.zip` を解凍し、その中にある `B2CAzureStorageClient.exe` ファイルを実行します。  このプログラムは単に、指定されたディレクトリ内のすべてのファイルをストレージ アカウントにアップロードし、それらのファイルに対する CORS アクセスを有効にします。  上記の手順を実行した場合、HTML および CSS ファイルはストレージ アカウントを指すようになります。  ストレージ アカウントの名前は、`blob.core.windows.net` の前の部分 (`contoso` など) です。  ブラウザーで `https://{storage-account-name}.blob.core.windows.net/{container-name}/Index.html` にアクセスしてみることで、コンテンツが正常にアップロードされていることを確認できます。 使用しても [http://test-cors.org/](http://test-cors.org/) いる内容は、CORS が有効になっているかどうかを確認する (XHR 状態探して: 結果 200)。

#### 再びポリシーをカスタマイズする

独自のストレージ アカウントにサンプル コンテンツをアップロードしたので、それを参照するようにサインアップ ポリシーを編集する必要があります。  説明する手順、 [「のポリシーをカスタマイズする」](#customize-your-policy) 、自分のストレージ アカウントの Url を使用して、上記セクションです。  たとえば、`Index.html` ファイルの場所は `<url-of-your-container>/Index.html` になります。  
        
使用すれば、 **を今すぐ実行** ボタンや、ポリシーを再度実行するのには、独自の applicaition です。  どちらの場合も同じサンプルの HTML と CSS を使用したので、結果はほぼ同じになるはずです。  ただし今度は、ポリシーは Azure BLOB ストレージの独自のインスタンスを参照しているので、自由にファイルを編集して再アップロードできます。  HTML と CSS をカスタマイズする方法の詳細についてを参照してください、 [メイン UI のカスタマイズの記事](active-directory-b2c-reference-ui-customization.md)します。

