<properties
    pageTitle="アプリ モデル v2.0 の概要 | Microsoft Azure"
    description="Microsoft アカウントと Azure Active Directory の両方でサインインできるアプリケーション構築を紹介します。"
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
    ms.date="11/06/2015"
    ms.author="dastrock"/>

# アプリ モデル v2.0 プレビュー: Microsoft アカウント ユーザーおよび Azure AD ユーザーによる単一アプリでのサインイン

> [AZURE.NOTE]
    この情報は、v2.0 アプリ モデルのパブリック プレビューに関するものです。  一般的に使用できる Azure AD と統合する方法については、サービスを参照してください、 [Azure Active Directory 開発者ガイド 』](active-directory-developers-guide.md)します。

アプリケーション開発者はこれまで、Microsoft アカウントと Azure Active Directory の両方をサポートするには、2 つの異なるシステムを統合する必要がありました。 v2.0 アプリ モデルでは、ユーザーは、いずれのアカウントでもサインインできるようになりました。 1 回のシンプルな統合によって、個人アカウントと職場または学校のアカウントを両方持っている何百万ものユーザーを含む対象ユーザーに利用していただけるようになりました。

アプリで使用することも、 [Office 365 REST Api のセット](https://msdn.microsoft.com/office/office365/howto/authenticate-Office-365-APIs-using-v2) アカウントのいずれかの型を使用します。  現在これらの API には、Outlook のメール、連絡先、および予定表の API が含まれています。  その他のサービスは、近い将来に追加されます。
<!-- TODO: customer reference article -->
<!-- Several apps have already begun to bridge the gap between consumer and enterprise accounts, including: [Boomerang](), [TripIt](), & [Uber](). -->

v2.0 のアプリ モデルはプレビュー期間中です。  プレビュー期間中は、新しいアプリ モデルに関する皆様のフィードバックや感想をお聞かせください。  フィードバックに基づいて、サービス向上のために、大幅な変更を加える場合があります。  V2.0 アプリ モデルを使用して、この期間中に運用アプリを解除する必要がありません。
<!-- TODO: Get approval on how it looks  -->

## Getting Started (概要)
v2.0 のアプリ モデルを使用して、ご自分のアプリケーションを稼働する方法は 2 つあります。  使用してプロトコルは、直接メッセージを送信する選択できる [OAuth 2.0](active-directory-v2-protocols.md#oauth2-authorization-code-flow) または [オープン ID 接続](active-directory-v2-protocols.md#openid-connect-sign-in-flow)します。  別の方法としての作業を行う - 以下好みのプラットフォームを選択し、開始、ライブラリを使用することができます。
<!-- TODO: Finalize this table  -->

[AZURE.INCLUDE [active-directory-v2-quickstart-table](../../includes/active-directory-v2-quickstart-table.md)]

## 新機能
今後の v2.0 アプリ モデルのパブリック プレビューの変更については、このページを頻繁に確認してください。  すべての更新については、@AzureAD からツイートもしていきます。

- については、 [種類のアプリケーション モデルのバージョン 2.0 を使用して構築できるアプリ](active-directory-v2-flows.md)します。
- Azure Active Directory に慣れている開発者では、を確認してください、 [、プロトコルとバージョン 2.0 のアプリケーション モデルの違いへの更新プログラム](active-directory-v2-compare.md)します。
- 現在 [プレビューの制限事項、制限および制約](active-directory-v2-limitations.md)します。

## リファレンス
これらのリンクは、プラットフォームの詳細を確認する上で役立ちます。

- 使用してスタック オーバーフローに関するヘルプを表示、 [azure active ディレクトリ](http://stackoverflow.com/questions/tagged/azure-active-directory) または [adal](http://stackoverflow.com/questions/tagged/adal) タグ。
- プレビューを使用して、上でアイデアが得 [User Voice](http://feedback.azure.com/forums/169401-azure-active-directory) の目安にします。  識別しやすいように、投稿のタイトルに "AppModelv2:" という言葉を入れてください。
- [アプリ モデル v2.0 プロトコルのリファレンス](active-directory-v2-protocols.md)
- [アプリ モデル v2.0 トークンの参照](active-directory-v2-tokens.md)
- [Office 365 の REST API リファレンス](https://msdn.microsoft.com/office/office365/howto/authenticate-Office-365-APIs-using-v2)
- [v2 のエンドポイントでのスコープと同意](active-directory-v2-scopes.md)

<!-- TODO: These articles
- [ADAL Library Reference]()
- [v2 Endpoint FAQs](active-directory-v2-faq.md)
-->

