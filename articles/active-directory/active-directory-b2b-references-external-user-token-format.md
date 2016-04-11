<properties
   pageTitle="Azure Active Directory B2B コラボレーション プレビューの外部ユーザー トークンの形式 | Microsoft Azure"
   description="Azure Active Directory B2B は、会社のアプリケーションにビジネス パートナーが選択的にアクセスできるようにすることで、会社間のリレーションシップをサポートします"
   services="active-directory"
   authors="viv-liu"
   manager="cliffdi"
   editor=""
   tags=""/>

<tags
   ms.service="active-directory"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="na"
   ms.date="10/27/2015"
   ms.author="viviali"/>

# Azure Active Directory (Azure AD) B2B コラボレーション プレビューの外部ユーザー トークンの形式
クレームの標準的な Azure AD のトークンが記載されている、 [サポートされているトークンとクレームの種類](active-directory-token-and-claims.md) azure.microsoft.com の記事です。

認証されている B2B コラボレーション外部ユーザーは、要求は次のとおりです。<br/>
- **OID:** リソース テナントからのオブジェクト ID<br/>
- **TID**: リソースのテナントのテナント ID<br/>
- **発行者**: これは、リソースのテナント<br/>
- **IDP**: これは、ユーザーのホームのテナント<br/>
- **AltSecId**: これは、代替セキュリティ ID は、不透明にします。<br/>

## 関連記事:
Azure B2B コラボレーションに関する他の記事を参照してください。

- [Azure AD B2B コラボレーションとは](active-directory-b2b-what-is-azure-ad-b2b.md)
- [動作のしくみ](active-directory-b2b-how-it-works.md)
- [詳細なチュートリアル](active-directory-b2b-detailed-walkthrough.md)
- [CSV ファイル形式リファレンス](active-directory-b2b-references-csv-file-format.md)
- [外部ユーザー オブジェクト属性の変更](active-directory-b2b-references-external-user-object-attribute-changes.md)
- [現在のプレビューの制限事項](active-directory-b2b-current-preview-limitations.md)

