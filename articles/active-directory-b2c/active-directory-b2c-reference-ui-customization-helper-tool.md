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

この記事では、対応する、 [メイン UI のカスタマイズの記事](active-directory-b2c-reference-ui-customization.md) Azure Active Directory B2C にします。以下の手順では、用意されているサンプルの HTML および CSS の内容を使用して、ページの UI カスタマイズ機能を試してみる方法について説明します。

[AZURE.INCLUDE [active-directory-b2c-preview-note](../../includes/active-directory-b2c-preview-note.md)]

## Azure AD B2C テナントを取得する

何かをカスタマイズできますが、前にする必要があります。 [Azure AD B2C テナントを取得する](active-directory-b2c-get-started.md) 既にお持ちでない場合。

## サインアップ ポリシーを作成する

2 つのページをカスタマイズするサンプルの内容を指定している、 [サインアップ ポリシー](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy): [IDP の選択] ページ](active-directory-b2c-reference-ui-customization.md#identity-provider-selection-page) と [ローカル アカウントのサインアップ ページ](active-directory-b2c-reference-ui-customization.md#local-account-sign-up-page)します。  [サインアップ ポリシーの作成](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy), 、ローカル アカウント (電子メール アドレス)、Facebook、および Google + として追加 **Id プロバイダー**します。 サンプルの HTML コンテンツが受け付ける IDP はこれらだけです。

## アプリケーションを登録する

必要があります [アプリケーションを登録する](active-directory-b2c-app-registration.md) B2C テナントのポリシーを実行するために使用できます。 アプリケーションを登録した後、サインアップ ポリシーを実際に実行するにはいくつかのオプションを使用できます。

- クイック スタート アプリケーションの一覧表示する Azure AD B2C のいずれかのビルド [ここ](active-directory-b2c-overview.md#getting-started)します。
- ビルド済みを使用して [Azure AD B2C のプレイ グラウンド](https://aadb2cplayground.azurewebsites.net) アプリケーションです。 使用して B2C テナントでアプリケーションを登録する必要があります、プレイ グラウンドの使用を選択した場合、 **リダイレクト URI** `https://aadb2cplayground.azurewebsites.net/`
- 使用して、 **を今すぐ実行** ] ボタンをクリックして、ポリシーで、 [Azure プレビュー ポータル](https://portal.azure.com)します。

## ポリシーをカスタマイズする

ポリシーの外観と操作性をカスタマイズするには、最初に Azure AD B2C の固有の規則を使用して HTML および CSS ファイルを作成する必要があります。 その後は、静的なコンテンツを、Azure AD B2C がアクセスできる公開されている場所にアップロードできます。 そのような場所としては、独自の専用 Web サーバー、Azure BLOB ストレージ、Azure CDN、その他の静的リソース ホスティング プロバイダーを使用できます。 唯一の要件は、コンテンツが HTTPS 経由で使用でき、CORS を使用してアクセスできることです。 静的コンテンツを Web で公開した後は、この場所を参照してエンド ユーザーにそのコンテンツを表示するようにポリシーを編集できます。  [メイン UI のカスタマイズの記事](active-directory-b2c-reference-ui-customization.md) Azure AD B2C カスタマイズ機能のしくみについて詳しく説明します。

このチュートリアルでは、既にいくつかのサンプル コンテンツを作成し、Azure BLOB ストレージでホストしてあります。 サンプル コンテンツは、架空の会社 "Contoso B2C" のテーマでの非常に基本的なカスタマイズです。 独自のポリシーで試してみるには、次の手順を実行します。

1. テナントへのサインイン、 [Azure プレビュー ポータル](https://portal.azure.com) B2C 機能ブレードに移動します。
2. **[サインアップ ポリシー]** をクリックし、作成したサインアップ ポリシー ("b2c_1_sign_up" など) をクリックします。
3. **[ページの UI のカスタマイズ]**、**[ID プロバイダーの選択ページ]** の順にクリックします。
4. **[カスタム テンプレートを使用する]** を **[はい]** に切り替えます。  **カスタム ページ URI** フィールドに「 `https://contosob2c.blob.core.windows.net/static/Index.html`します。 **[OK]** をクリックします。
5. **[ローカル アカウントのサインアップ ページ]** をクリックします。 **[カスタム テンプレートを使用する]** を **[はい]** に切り替えます。  **カスタム ページ URI** フィールドに「 `https://contosob2c.blob.core.windows.net/static/EmailVerification.html`します。 **[OK]** を 2 回クリックして、UI カスタマイズ ブレードを閉じます。
6. **[保存]** をクリックします。

カスタマイズしたポリシーを試すことができるようになります。 独自のアプリケーションまたは AAD B2C プレイグラウンドを使用することもできますが、単にポリシー ブレードで **[今すぐ実行]** コマンドをクリックするのでもかまいません。 ドロップダウンでアプリケーションを選び、適切なリダイレクト URI を選びます。 **[今すぐ実行]** ボタンをクリックします。 新しいブラウザー タブが開き、アプリケーションへのサインアップについて、新しいコンテンツが所定の場所に置かれた状態のユーザー エクスペリエンスを確認できます。

## サンプル コンテンツを Azure BLOB ストレージにアップロードする

Azure BLOB ストレージを使用してページのコンテンツをホストする場合は、独自のストレージ アカウントを作成し、B2C ヘルパー ツールを使用してファイルをアップロードできます。

#### ストレージ アカウントの作成

1. サインイン、 [Azure プレビュー ポータル](https://portal.azure.com/)します。
2. **[+ 新規]**、**[データ + ストレージ]**、**[ストレージ アカウント]** の順にクリックします。 Azure BLOB ストレージ アカウントを作成するには、Azure サブスクリプションが必要です。 無料評価版にサインアップすること [ここ](https://azure.microsoft.com/pricing/free-trial/)します。
3. ストレージ アカウントの **[名前]** を指定し ("contoso" など)、適切な **[価格レベル]**、**[リソース グループ]**、**[サブスクリプション]** を選択します。 **[スタート画面にピン留めする]** がオンになっていることを確認します。 **[作成]** をクリックします。
4. スタート画面に戻り、作成したストレージ アカウントをクリックします。
5. **[概要]** セクションで、**[コンテナー]**、**[+ 追加]** の順にクリックします。
6. コンテナーの **[名前]** ("b2c" など) を指定し、**[アクセスの種類]** として **[BLOB]** を選択します。 **[OK]** をクリックします。
7. 作成したコンテナーは、**[BLOB]** ブレードに一覧表示されます。 コンテナーの URL に注意してください。たとえばのようになります `https://contoso.blob.core.windows.net/b2c`します。 **[BLOB]** ブレードを閉じます。
8. [ストレージ アカウント] のブレードで、**[キー]** をクリックし、**[ストレージ アカウント名]** および **[プライマリ アクセス キー]** フィールドの値をメモしておきます。

> [AZURE.NOTE]
    **プライマリ アクセス キー**は、重要なセキュリティ資格情報です。

#### ヘルパー ツールとサンプル ファイルをダウンロードする

ダウンロードすることができます、 [ここ .zip として Azure Blob ストレージ ヘルパー ツールとサンプル ファイル](https://github.com/azureadquickstarts/b2c-azureblobstorage-client/archive/master.zip) または GitHub から複製します。

```
git clone https://github.com/azureadquickstarts/b2c-azureblobstorage-client
```

このリポジトリが含まれています、 `sample_templates\contoso` ディレクトリで、サンプル HTML、CSS、およびイメージが含まれています。 これらのテンプレートで独自の Azure BLOB ストレージ アカウントを参照するには、HTML ファイルを編集する必要があります。 開いている `Index.htnml` と `EmailValidation.html` とのすべてのインスタンスを置換 `https://localhost` の前の手順でコピーしたコンテナーの URL を使用しています。 ドメインの下の Azure AD によって、HTML を処理できる場合は、HTML ファイルの絶対パスを使用する必要がある `https://login.microsoftonline.com`します。

#### サンプル ファイルをダウンロードする

同じリポジトリで解凍し `B2CAzureStorageClient.zip` を実行し、 `B2CAzureStorageClient.exe` ファイルの場所。 このプログラムは単に、指定されたディレクトリ内のすべてのファイルをストレージ アカウントにアップロードし、それらのファイルに対する CORS アクセスを有効にします。 上記の手順を実行した場合、HTML および CSS ファイルはストレージ アカウントを指すようになります。 ストレージ アカウントの名前は、前の部分であるに注意してください `>.blob.core.windows.net`, など、 `contoso`します。 アクセスしようとして、コンテンツが正しくアップロードされていることを確認できる `https://{storage-account-name}.blob.core.windows.net/{container-name}/Index.html` をブラウザーでします。 使用しても [http://test-cors.org/](http://test-cors.org/) いる内容は、CORS が有効になっているかどうかを確認する (XHR 状態探して: 結果 200)。

#### 再びポリシーをカスタマイズする

独自のストレージ アカウントにサンプル コンテンツをアップロードしたので、それを参照するようにサインアップ ポリシーを編集する必要があります。説明する手順、 [「のポリシーをカスタマイズする」](#customize-your-policy) 、自分のストレージ アカウントの Url を使用して、上記セクションです。場所など、 `Index.html` ファイル `< url の-your-コンテナー >/Index.html`します。

これで、**[今すぐ実行]** ボタンまたは独自のアプリケーションを使用して、ポリシーを再度実行できるようになります。どちらの場合も同じサンプルの HTML と CSS を使用したので、結果はほぼ同じになるはずです。ただし今度は、ポリシーは Azure BLOB ストレージの独自のインスタンスを参照しているので、自由にファイルを編集して再アップロードできます。HTML と CSS をカスタマイズする方法の詳細についてを参照してください、 [メイン UI のカスタマイズの記事](active-directory-b2c-reference-ui-customization.md)します。





