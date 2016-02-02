<properties
    pageTitle="Azure Active Directory B2C プレビュー: 概要 | Microsoft Azure"
    description="Azure Active Directory B2C によるコンシューマー向けアプリケーションの開発"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="msmbaldwin"
    editor=""/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/28/2015"
    ms.author="swkrish"/>


# Azure Active Directory B2C プレビュー: アプリケーションにコンシューマーをサインアップおよびサインインする

**Azure Active Directory B2C** は、コンシューマー向け Web アプリケーションおよびモバイル アプリケーション用の包括的なクラウド ID 管理ソリューションです。 億単位のコンシューマー ID まで拡張可能な高可用性のグローバル サービスです。 エンタープライズ レベルのセキュリティ保護されたプラットフォームに構築された Azure Active Directory B2C は、アプリケーション、ビジネス、およびコンシューマーを保護された状態に保ちます。

これまで、開発したアプリケーションにコンシューマーがサインアップおよびサインインすることを望むアプリケーション開発者は、独自のコードを記述していました。開発者は、オンプレミスのデータベースまたはシステムを使用して、ユーザー名とパスワードを保存していました。Azure Active Directory B2C では、セキュリティ保護された標準ベースのプラットフォームと機能豊富で拡張可能なポリシーのセットを使用して、より簡単にコンシューマーの ID 管理をアプリケーションに統合できます。Azure Active Directory B2C を使用すると、コンシューマーは、既存のソーシャル アカウント (Facebook、Google、Amazon、LinkedIn) を使用するか、または新しい資格情報 (電子メール アドレスとパスワードまたはユーザー名とパスワード) を作成することによって、アプリケーションにサインアップできます。後者は「ローカル アカウント」と呼ばれます。

Azure Active Directory B2C はプレビュー状態です。 プレビュー期間中、Azure Active Directory B2C を使ってみてフィードバックやご意見をお送りください。 フィードバックに基づいて、サービス向上のために、大幅な変更を加える場合があります。 この期間中は、プレビューを使用して運用アプリケーションをリリースしないようにしてください。 使用してご意見を得 [User Voice](http://feedback.azure.com/forums/169401-azure-active-directory)します。

## Getting Started (概要)

コンシューマーのサインアップおよびサインインを受け付けるアプリケーションを作成するには、最初にアプリケーションを Azure Active Directory B2C テナントに登録する必要があります。これで説明する手順を使用して、独自のテナントを取得する [記事](active-directory-b2c-get-started.md)します。

使用して直接、プロトコル メッセージを送信するかを選択して、Azure Active Directory B2C サービスに対してアプリケーションを記述することができます [OAuth 2.0](active-directory-b2c-protocols.md#oauth2-authorization-code-flow) または [オープン ID 接続](active-directory-b2c-protocols.md#openid-connect-sign-in-flow) する処理のために、ライブラリを使用して、または (以下、好きなプラットフォームを選択および開始)。

[AZURE.INCLUDE [active-directory-b2c-quickstart-table](../../includes/active-directory-b2c-quickstart-table.md)]

## 新機能

今後の Azure Active Directory B2C プレビューに変更については、このページを頻繁に確認してください。 すべての更新については、@AzureAD からツイートもしていきます。

- については、 [拡張可能なポリシー フレームワーク](active-directory-b2c-reference-policies.md) とは、種類のポリシーの作成し、アプリケーションで使用することができますが。
- 現在 [プレビューの制限事項、制限および制約](active-directory-b2c-limitations.md)します。

## 方法

Azure Active Directory B2C プレビューの特定の機能を使用する方法を理解してください。

- 構成 ([Facebook](active-directory-b2c-setup-fb-app.md), 、[Google +](active-directory-b2c-setup-goog-app.md), 、[Amazon](active-directory-b2c-setup-amzn-app.md) と [LinkedIn](active-directory-b2c-setup-li-app.md)) 消費者向けアプリケーションで使用するアカウントします。
- [カスタム属性を使用して、コンシューマーに関する情報を収集](active-directory-b2c-reference-custom-attr.md)します。
- [、消費者向けアプリケーションで多要素認証を有効にする](active-directory-b2c-reference-mfa.md)します。
- [セットアップ セルフ サービス パスワードのリセット、消費者向け](active-directory-b2c-reference-sspr.md)します。
- [、および外観のサインアップ、サインインのおよびその他のコンシューマー向けのページはカスタマイズ](active-directory-b2c-reference-ui-customization.md) Azure Active Directory B2C によって処理されます。
- [を使用して Azure Active Directory Graph API プログラムを使用して作成、読み取り、更新および消費者を削除する](active-directory-b2c-devquickstarts-graph-dotnet.md) 、Azure Active Directory B2C テナントにします。

## リファレンス

以下のリンクは、サービスの詳細を調べるのに役立ちます。

- を参照してください [価格情報を Active Directory B2C](https://azure.microsoft.com/pricing/details/active-directory-b2c)
- 使用してスタック オーバーフローに関するヘルプを表示、 [azure active ディレクトリ](http://stackoverflow.com/questions/tagged/azure-active-directory) または [adal](http://stackoverflow.com/questions/tagged/adal) タグ。
- プレビューを使用して、上でアイデアが得 [User Voice](http://feedback.azure.com/forums/169401-azure-active-directory) の目安にします。 識別しやすいように、投稿のタイトルに "AzureADB2C:" という言葉を入れてください。
- Azure Active Directory B2C は、"App Model v2.0" と呼ばれるアプリケーション登録モデルを使用して、業界標準のプロトコル (OpenID Connect、OAuth 2.0) をサポートします。
  - [アプリ モデル バージョン 2.0 プロトコル リファレンス](active-directory-b2c-reference-protocols.md)
  - [アプリ モデル バージョン 2.0 のトークンの参照](active-directory-b2c-reference-tokens.md)
- [Azure Active Directory B2C よく寄せられる質問](active-directory-b2c-faqs.md)
- [Azure Active Directory B2C に対するファイルのサポートの要求](active-directory-b2c-support.md)





