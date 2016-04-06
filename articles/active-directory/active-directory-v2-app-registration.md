<properties
    pageTitle="アプリ モデル v2.0 | Microsoft Azure"
    description="マイクロソフトのサインインを有効にして、アプリとアプリ モデルのバージョン 2.0 の統合のアプリを登録する方法。"
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/09/2015"
    ms.author="dastrock"/>

# アプリ モデル v2.0 プレビュー: Microsoft にアプリを登録する方法

MSA および Azure AD サインインの両方を受け付けるアプリを構築するには、最初に、アプリを Microsoft に登録する必要があります。  Azure AD または MSA で利用している既存のアプリは使用できなくなります。新しいアプリを作成しましょう。

> [AZURE.NOTE]
    この情報は、v2.0 アプリ モデルのパブリック プレビューに関するものです。  一般的に使用できる Azure AD と統合する方法については、サービスを参照してください、 [Azure Active Directory 開発者ガイド 』](active-directory-developers-guide.md)します。

## Microsoft アプリケーションの登録ポータルにアクセスしてください。
最初にすることはまずに移動 [https://apps.dev.microsoft.com](https://apps.dev.microsoft.com)します。  これは、マイクロソフトのアプリに関するあらゆることを管理できる新しいアプリ登録ポータルです。

Microsoft の個人または職場/学校アカウントのいずれかでサインインします。  いずれのアカウントもをお持ちでない場合は、新しい個人アカウントを新規登録します。 簡単に登録できますので、今すぐサインアップしましょう。

サインアップできましたか?  おそらく空の Microsoft アプリの一覧が表示されているはずです。  これを変更してみましょう。

<!-- TODO: Verify strings here -->
をクリックして **アプリを追加**, 、名前を付けます。  ポータルは、アプリケーションを割り当てられます、
後続のコードで使用するグローバル一意識別のアプリケーション Id。  Api を呼び出すことのアクセス トークンが必要な場合は、アプリには、サーバー側コンポーネントが含まれています。
(考えて: Office、Azure、またはサード パーティ) を作成します、* * アプリケーション
* * シークレットもします。
<!-- TODO: Link for app secrets -->

次に、アプリで使用するプラットフォームを追加します。
- Web ベース アプリケーションの提供、 **リダイレクト URI** サインイン メッセージを送信できます。
- モバイル アプリの場合、自動的に作成される既定のリダイレクト URI をメモしておきます。

必要に応じて、[プロファイル] セクションでサインイン ページの外観をカスタマイズできます。  クリックして **保存** に進む前にします。

## クイック スタート アプリケーションの構築
Microsoft アプリがある場合は、これで、このクイック スタートのいずれかを進むことができます。
チュートリアル アプリ モデル v2.0 で最大と実行をします。  ご紹介
推奨事項:

[AZURE.INCLUDE [active-directory-v2-quickstart-table](../../includes/active-directory-v2-quickstart-table.md)]


