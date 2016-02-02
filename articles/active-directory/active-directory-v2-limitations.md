<properties
    pageTitle="アプリ モデル 2.0 の制約事項 | Microsoft Azure"
    description="Azure AD v2.0 アプリ モデルの一連の制限事項について記載しています。"
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


# アプリ モデル v2.0 プレビュー: 制限事項

v2.0 アプリ モデルのいくつかの機能は、パブリック プレビューの時点ではまだサポートされていません。いずれの制限も v2.0 アプリ モデルが一般公開リリースとなる前に排除される予定ですが、パブリック プレビューの時点でアプリ開発を行う場合は注意が必要です。
> [AZURE.NOTE]
    この情報は、v2.0 アプリ モデルのパブリック プレビューに関するものです。 一般的に使用できる Azure AD と統合する方法については、サービスを参照してください、 [Azure Active Directory 開発者ガイド 』](active-directory-developers-guide.md)します。

## 実稼働アプリのサポート

v2.0 アプリ モデルに統合されたアプリを実稼働レベルのアプリとして一般リリースすることは避けてください。 現時点で v2.0 アプリ モデルはパブリック プレビューとなっています。いずれ重大な変更が行われる可能性もあります。サービスの SLA も保証されません。 問題が発生してもサポートを受けることはできません。 リスクを承知のうえで開発段階のサービスを利用する場合は、アプリまたはサービスのスコープについて検討する必要があります。@AzureAD にご連絡ください。

## アプリに関する制限事項

v2.0 アプリ モデルのパブリック プレビューでは現在、次のタイプのアプリはサポートされていません。 サポートされている種類のアプリについてを参照してください [今回](active-directory-v2-flows.md)します。

##### シングル ページ アプリ (JavaScript)

最近多く見かけるようになったシングル ページ アプリのフロントエンドは、主に JavaScript で作成されています。AngularJS、Ember.js、Durandal などの SPA フレームワークが使われることも少なくありません。一般に利用可能な Azure AD サービスを使用してこれらのアプリをサポートしている、 [OAuth 2.0 の暗黙的なフロー](active-directory-v2-protocols.md#oauth2-implicit-flow) -ただし、この流れになっていません v2.0 アプリ モデルで使用できます。 サポートされるまでには、あと少し時間がかかります。

使用して認証を実装するには取得 v2.0 アプリ モデルを扱う SPA 関心を持っている場合は、 [web アプリのフロー](active-directory-v2-flows.md#web-apps)します。 ただしこの方法は推奨されません。そのような使い方に関するドキュメントも限られています。 SPA シナリオの把握したい場合、これをチェック アウトできます、 [コード サンプルの一般提供の Azure AD SPA](active-directory-devquickstarts-angular.md)します。

##### デーモン/サーバー サイド アプリ

長時間実行されるプロセスを含んだアプリや、ユーザーの介入なしで動作するアプリも、セキュリティで保護されたリソース (Web API など) にアクセスする必要があります。 これらのアプリを認証およびなくを使用してアプリケーションの id (ユーザーの委任された id) を使用してトークンを取得、 [フローの OAuth 2.0 クライアント資格情報](active-directory-v2-protocols.md#oauth2-client-credentials-grant-flow)します。

v2.0 アプリ モデルでは、このフローが現在サポートされていません。つまりアプリがトークンを取得できるのは、対話的なユーザー サインイン フローの後に限られます。 近い将来、クライアント資格情報フローが追加されます。 クライアントを表示したい場合フローで、一般的に使用可能な資格情報を Azure AD アプリ モデルをチェック アウト、 [GitHub 上のデーモン サンプル](https://github.com/AzureADSamples/Daemon-DotNet)します。

##### Web API の連鎖 (On-Behalf-Of)

v2.0 アプリ モデルによって保護された Web API から、同じように保護された別の下流の Web API を呼び出す手法は、多くのアーキテクチャで使われています。 このシナリオは、バックエンドの Web API から Microsoft Online サービス (Office 365、Graph API など) を呼び出すネイティブ クライアントでよく見られます。

呼ばれる、OAuth 2.0 Jwt ベアラー資格情報付与を使用してこのチェーンの Web API のシナリオをサポートできる、 [On-Behalf-Of フロー](active-directory-v2-protocols.md#oauth2-on-behalf-of-flow)します。 しかし現時点では v2.0 アプリ モデル プレビューに On-Behalf-Of フローが実装されていません。 一般的に使用できる Azure AD でのこのフローの動作を確認するサービス型で、チェック アウト、 [GitHub の代理としてのコード サンプル](https://github.com/AzureADSamples/WebAPI-OnBehalfOf-DotNet)します。

##### スタンドアロン Web API

V2.0 アプリ モデルのプレビューにする機能がある [OAuth トークンを使用してセキュリティで保護された Web API をビルド](active-directory-v2-flows.md#web-apis) v2.0 エンドポイントからです。 しかし、その Web API は、同じアプリケーション ID を共有するクライアントからしかトークンを受け取ることができません。 複数の異なるクライアントからのアクセスを受け付けるサード パーティ Web サービスを構築することはできません。

同じアプリ Id のよく知られているクライアントからのトークンを受け付ける Web API を構築する方法を表示するで v2.0 アプリ モデルの Web API のサンプルを参照してください。 当社 [作業の開始](active-directory-appmodel-v2-overview.md#getting-started) セクションです。

## ユーザーに関する制限事項

現在、v2.0 アプリ モデルで構築されたすべてのアプリは、Microsoft アカウントまたは Azure AD アカウントを持つすべてのユーザーに公開されます。 どちらかのタイプのアカウントを持つユーザーであれば、対象のアプリにアクセス (またはアプリをインストール) して v2.0 アプリ モデルで資格情報を入力し、アプリのアクセス許可要求に同意することができます。 特定のユーザーからのサインインを拒否するようにアプリのコードを記述することはできますが、それらのユーザーがアプリに同意することまでは防止できません。

実質的には、アプリにサインインできるユーザーの種類を制限することができません。 (1 つの組織内のユーザーに限定される) 基幹業務アプリ、(Azure AD アカウントを持った) 社内ユーザーのみが利用できるアプリ、(Microsoft アカウントを持つ) コンシューマーのみが利用できるアプリを構築することはできません。

## アプリの登録に関する制限事項

この時点では、v2.0 アプリ モデルと統合するすべてのアプリがで新しいアプリケーションの登録を作成する必要があります [apps.dev.microsoft.com](https://apps.dev.microsoft.com)します。 既存の Azure AD アプリまたは Microsoft アカウント アプリは、v2.0 アプリ モデルとの互換性がありません。新しいアプリ登録ポータル以外のポータルで登録されたアプリも同様です。 一般提供版の Azure AD サービスから v2.0 アプリ モデルへのアプリの移行パスはありません。

同様に、新しいアプリ登録ポータルで登録されたアプリは、v2.0 アプリ モデル専用となります。 アプリ登録ポータルで、Azure AD または Microsoft アカウント サービスと適切に連携するアプリを作成することはできません。

現在、新しいアプリケーション登録ポータルで登録されるアプリは、ごく限られた redirect_uri 値に限定されています。 Web アプリとサービスの redirect_uri がスキームで始まる必要がありますか `https`, 、その他のプラットフォームの redirect_uri がハード コーディングされた値を使用する必要があります `urn: ietf:oauth:2.0:oob`します。

新しいアプリケーションの登録ポータルでアプリを登録する方法についてを参照してください [今回](active-directory-v2-app-registration.md)します。

## サービスと API に関する制限事項

V2.0 アプリ モデル現在サポートしているサインイン、新しいアプリケーションの登録ポータルに登録されているアプリの一覧にある指定 [の認証フローがサポートされている](active-directory-v2-flows.md)します。 ただしこれらのアプリで取得できるのは、ごく限られたリソースの OAuth 2.0 アクセス トークンだけです。 v2.0 エンドポイントから発行される access_tokens の対象は次のいずれかに限定されます。

- トークンを要求したアプリ。 論理的に複数の異なるコンポーネントまたは階層で構成されている場合にアプリが取得できるのはそれ自身の access_token となります。 このシナリオの動作を表示するには、チェック アウト、 [作業の開始](active-directory-appmodel-v2-overview.md#getting-started) チュートリアルです。
- Outlook のメール、予定表、連絡先の REST Api、これらはすべては https://outlook.office.com を参照します。 これらの Api にアクセスするアプリを記述する方法については、これらを参照してください。 [Office Getting Started](https://www.msdn.com/office/office365/howto/authenticate-Office-365-APIs-using-v2) チュートリアルです。

その他の Microsoft Online Services や独自の Web API およびサービスについても近い将来、サポートが追加される予定です。

## ライブラリと SDK に関する制限事項

v2.0 アプリ モデルのプレビュー版をサポートするライブラリが存在しない言語やプラットフォームも一部存在します。 認証ライブラリ群は現在、.NET、iOS、Android、NodeJS、JavaScript に限られます。 対応するコード サンプルとチュートリアルのそれぞれで使用可能な [概要](active-directory-appmodel-v2-overview.md#getting-started) セクションです。

アプリ v2.0 アプリを別の言語またはプラットフォームを使用するモデルと統合する場合を参照してください、 [OAuth 2.0 と OpenID Connect プロトコル参照](active-directory-v2-protocols.md) v2.0 エンドポイントと通信するために必要な HTTP メッセージを作成する方法の指示をします。

## プロトコルに関する制限事項

v2.0 アプリ モデルは、Open ID Connect および OAuth 2.0 をサポートしています。ただし、各プロトコルの一部の機能がまだ v2.0 アプリ モデルには組み込まれていません。次に例をいくつか示します。

- OpenID Connect の完全なサポート `プロンプト` パラメーター
- OpenID Connect `login_hint` パラメーター
- OpenID Connect `domain_hint` パラメーター
- OpenID Connect `end_sesssion_endpoint`

V2.0 アプリ モデルでサポートされるプロトコルの機能のスコープを理解するのに目を通して、 [OpenID Connect と OAuth 2.0 プロトコル リファレンス](active-directory-v2-protocols.md)します。





