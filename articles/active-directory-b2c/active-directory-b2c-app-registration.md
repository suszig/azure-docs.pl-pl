<properties
    pageTitle="Azure Active Directory B2C プレビュー: アプリケーションの登録 | Microsoft Azure"
    description="Azure Active Directory B2C にアプリケーションを登録する方法"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/08/2015"
    ms.author="swkrish"/>

# Azure Active Directory B2C プレビュー: アプリケーションを登録する方法

[AZURE.INCLUDE [active-directory-b2c-preview-note](../../includes/active-directory-b2c-preview-note.md)]

## 前提条件

コンシューマーのサインアップおよびサインインを受け付けるアプリケーションを作成するには、最初にアプリケーションを Azure Active Directory B2C テナントに登録する必要があります。 以下の手順を使用して、独自のテナントを取得する [ここ](active-directory-b2c-get-started.md)します。 この記事のすべての手順を実行した場合、B2C 機能ブレードがスタート画面にピン留めされているはずです。

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## B2C 機能ブレードに移動する

次の 2 つの方法のどちらかで、B2C 機能ブレードに移動できます。

### 1.Azure プレビュー ポータルで直接アクセスする

スタート画面にピン留め B2C 機能ブレードがあれば表示されますにサインインするとすぐに、 [Azure プレビュー ポータル](https://portal.azure.com/) B2C テナントのグローバル管理者として。

B2C 機能ブレードに移動して直接アクセスすることもできます [https://portal.azure.com/{tenant}.onmicrosoft.com/?Microsoft_AAD_B2CAdmin=true#blade/Microsoft_AAD_B2CAdmin/TenantManagementBlade/id/{tenant}.onmicrosoft.com](https://portal.azure.com/{tenant}.onmicrosoft.com/?Microsoft_AAD_B2CAdmin=true#blade/Microsoft_AAD_B2CAdmin/TenantManagementBlade/id/{tenant}.onmicrosoft.com) 、 **{tenant}** はテナントの作成時 (たとえば、contosob2c) に使用して、B2C テナントのグローバル管理者としてサインイン名で置き換えられます。 後で参照できるように、このリンクをブックマークできます (下記の注をご覧ください)。

    > [AZURE.IMPORTANT]
    You need to be a Global Administrator of the B2C tenant to be able to access the B2C features blade. A Global Administrator from any other tenant or a User from any tenant cannot access it.

### 2.Azure ポータルからアクセスする

サインイン、 [Azure ポータル](https://manage.windowsazure.com/) サブスクリプション管理者として (これは、会社または学校のアカウントまたは Azure にサインアップするために使用するアカウントと同じ Microsoft アカウント)。 左側の Active Directory 拡張機能に移動し、B2C テナントをクリックします。  **クイック スタート** (最初のタブを開きます)] タブをクリックして **B2C の管理設定** [ **管理**します。 新しいブラウザー ウィンドウまたはタブで、B2C 機能ブレードが開きます。

検索することも、 **B2C の管理設定** リンク (で、 **B2C 管理** セクション) で、 **構成** ] タブをクリックします。

## アプリケーションを登録する

1. Azure プレビュー ポータルで [B2C 機能ブレードでのクリックして **アプリケーション**します。
2. クリックして **+ 追加** ブレードの上部にあります。
3.  **名前** アプリケーションのコンシューマーにアプリケーションが記述されます。 たとえば、"Contoso B2C app" などと入力します。
4. Web ベースのアプリケーションを作成している場合、 **web アプリを含める]、[web API** に切り替える **はい**します。  **応答 Url** ようなアプリケーションの要求トークンを Azure AD B2C が返すはエンドポイントには。 たとえば、「`https://localhost:44321/`」のように入力します。 作成 (およびコピー) します、アプリケーションには、セキュリティで保護する必要があるサーバー側コンポーネント (API) が含まれている場合、 **アプリケーション シークレット** をクリックしても、 **キーの生成** ] ボタンをクリックします。

    > [AZURE.NOTE]
    **アプリケーション シークレット** は、重要なセキュリティ資格情報。

5. モバイル アプリケーションを作成している場合を切り替える、 **Include ネイティブ クライアント** に切り替える **はい**します。 既定値をコピー **リダイレクト URI** 自動的に作成しています。
6. クリックして **作成** アプリケーションを登録します。
7. 作成したアプリケーションをクリックし、グローバル一意識別ダウン コピー **アプリケーション ID** 後続のコードで使用します。

## クイック スタート アプリケーションを構築する

アプリケーションを Azure AD B2C に登録したので、いずれかのクイック スタート チュートリアルを実行して使い始めることができます。 以下に、推奨事項をいくつか示します。

[AZURE.INCLUDE [active-directory-v2-quickstart-table](../../includes/active-directory-b2c-quickstart-table.md)]

