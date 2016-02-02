<properties
   pageTitle="アプリケーションの管理に関するドキュメント ライブラリ | Microsoft Azure"
   description="操作方法、トラブルシューティング、およびよく寄せられる質問に対するテクニカル リファレンス リンクを提供する Azure Active Directory のアプリケーション管理トピック"
   services="active-directory"
   documentationCenter=""
   authors="kgremban"
   manager="stevenpo"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="12/03/2015"
   ms.author="kgremban"/>


# アプリケーションの管理に関するドキュメント ライブラリ

このページには、Azure Active Directory でのシングル サインオンの主な機能に関する有用なリンクが含まれています。 各リンクは、次のカテゴリに分類されています。

- **概要:** ユース ケースとトピックの紹介を含む、機能の概要。
- **操作方法:** 特定のシナリオで使用を開始するための詳細な手順。
- **トラブルシューティング:** 一般的な問題を診断するためのヒント。
- **よく寄せられる質問:** 一般的な質問や懸念事項に対する回答。

## Cloud App Discovery

| | |
| ------ | ------ |
| 概要| [自分の組織内で使用される承認されていないクラウド アプリを検出する方法](active-directory-cloudappdiscovery-whatis.md)|
| 操作方法| [Cloud App Discovery の概要](http://social.technet.microsoft.com/wiki/contents/articles/30962.getting-started-with-cloud-app-discovery.aspx) <br><br> [Cloud App Discovery のセキュリティとプライバシーの考慮事項](active-directory-cloudappdiscovery-security-and-privacy-considerations.md) <br><br> [Cloud App Discovery グループ ポリシー デプロイ ガイド](http://social.technet.microsoft.com/wiki/contents/articles/30965.cloud-app-discovery-group-policy-deployment-guide.aspx) <br><br> [Cloud App Discovery System Center 展開ガイド](http://social.technet.microsoft.com/wiki/contents/articles/30968.cloud-app-discovery-system-center-deployment-guide.aspx) <br><br> [プロキシ サービス用レジストリ設定を Cloud App Discovery](active-directory-cloudappdiscovery-registry-settings-for-proxy-services.md)|
| FAQ| [Cloud App Discovery: よく寄せられる質問](http://social.technet.microsoft.com/wiki/contents/articles/24037.cloud-app-discovery-frequently-asked-questions.aspx)|

## アプリの割り当て

| | |
| ------ | ------ |
| 概要| [Azure Active Directory グループによるリソースへのアクセスを管理します。](active-directory-manage-groups.md)|
| 操作方法| [グループの動的メンバーシップを構成する簡単なルールを作成する](active-directory-accessmanagement-simplerulegroup.md) <br><br> [SaaS アプリケーションへのアクセスを管理するグループを使用して](active-directory-accessmanagement-group-saasapps.md) <br><br> [セルフ サービス アプリケーション アクセス管理のための Azure AD 設定](active-directory-accessmanagement-self-service-group-management.md) <br><br> [管理グループの所有者](active-directory-accessmanagement-managing-group-owners.md) <br><br> [Azure Active Directory でセキュリティ グループを管理する](active-directory-accessmanagement-manage-groups.md) <br><br> [専用の Azure Active Directory でグループ](active-directory-accessmanagement-dedicated-groups.md) <br><br> [属性を使用して、高度なルールを作成する](active-directory-accessmanagement-groups-with-advanced-rules.md) <br><br> [ユーザー プロビジョニングとプロビジョニング解除の Azure Active Directory による SaaS アプリへの自動化](active-directory-saas-app-provisioning.md) <br><br> [Azure Active Directory による SaaS アプリを統合する方法に関するチュートリアルの一覧](active-directory-saas-tutorial-list.md)|

## フェデレーション シングル サインオン

| | |
| ------ | ------ |
| 概要| [AD FS でフェデレーションを構成します。](active-directory-aadconnect-get-started-custom.md)
| 操作方法| [でのシングル サインオンを使用した DirSync](https://msdn.microsoft.com/library/azure/dn441213.aspx) <br><br> [Microsoft Azure では、Windows Server Active Directory とのフェデレーションをサポート](https://azure.microsoft.com/blog/windows-azure-now-supports-federation-with-windows-server-active-directory/) <br><br> [Azure Active Directory による SaaS アプリを統合する方法に関するチュートリアルの一覧](active-directory-saas-tutorial-list.md) <br><br> [Active Directory Marketplace](https://azure.microsoft.com/marketplace/active-directory/)|
| トラブルシューティング| [トラブルシューティング Azure AD ディレクトリ同期ツールの構成ウィザード](http://social.technet.microsoft.com/wiki/contents/articles/19100.troubleshooting-azure-ad-dirsync-tool-configuration-wizard-failed-to-get-address-for-method-createidentityhandle2.aspx) <br><br> [Azure Active Directory 同期ツールのインストールおよび構成ウィザードのエラーのトラブルシューティング方法](https://support.microsoft.com/kb/2684395) <br><br> [ディレクトリ同期のトラブルシューティング](https://msdn.microsoft.com/library/azure/jj151787.aspx) <br><br> [でのシングル サインオンのトラブルシューティングを行う](https://msdn.microsoft.com/library/azure/jj151834.aspx)|
| FAQ| [Azure Active Directory でのシングル サインオン用の証明書を管理するフェデレーション](active-directory-sso-certs.md) <br><br> [Azure Active Directory Connect の FAQ](active-directory-aadconnect-faq.md)|

## パスワード シングル サインオン

| | |
| ------ | ------ |
| 操作方法| [内部設置型 id と Azure Active Directory の統合](active-directory-aadconnect.md) <br><br> [DirSync とパスワード同期](https://msdn.microsoft.com/library/azure/dn441214.aspx)|
| トラブルシューティング| [トラブルシューティング Azure AD ディレクトリ同期ツールの構成ウィザード](http://social.technet.microsoft.com/wiki/contents/articles/19100.troubleshooting-azure-ad-dirsync-tool-configuration-wizard-failed-to-get-address-for-method-createidentityhandle2.aspx) <br><br> [Azure Active Directory 同期ツールのインストールおよび構成ウィザードのエラーのトラブルシューティング方法](https://support.microsoft.com/kb/2684395) <br><br> [ディレクトリ同期のトラブルシューティング](https://msdn.microsoft.com/library/azure/jj151787.aspx)|
| FAQ| [Azure Active Directory Connect の FAQ](active-directory-aadconnect-faq.md)|

## リッチ アプリのプロビジョニング

| | |
| ------ | ------ |
| 概要| [ユーザー プロビジョニングと Azure Active Directory による SaaS アプリをプロビジョニング解除の自動化します。](active-directory-saas-app-provisioning.md)|
| 操作方法| [チュートリアル: と Concur の Azure Active Directory 統合](active-directory-saas-concur-tutorial.md) <br><br> [チュートリアル: DocuSign と Azure Active Directory 統合](active-directory-saas-docussign-tutorial.md) <br><br> [チュートリアル: Azure Active Directory 統合と Dropbox for Business](active-directory-saas-dropboxforbusiness-tutorial.md) <br><br> [チュートリアル: Azure Active Directory と Google Apps を統合する方法](active-directory-saas-google-apps-tutorial.md) <br><br> [チュートリアル: Azure Active Directory と Salesforce を統合する方法](active-directory-saas-salesforce-tutorial.md) <br><br> [チュートリアル: Azure Active Directory 統合と ServiceNow と](active-directory-saas-servicenow-tutorial.md) <br><br> [チュートリアル: 受信同期の構成の Workday](active-directory-saas-workday-inbound-tutorial.md)|

## Azure AD アプリ ギャラリー

| | |
| ------ | ------ |
| 操作方法| [Azure Active Directory アプリケーション ギャラリーでのアプリケーションの一覧を表示する](active-directory-app-gallery-listing.md) <br><br> [と Azure AD SAML 構成のセルフ サービス「を自分のアプリ」](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx)|

## カスタム アプリ

| | |
| ------ | ------ |
| 概要| [Azure AD SAML 構成のセルフ サービス「を自分のアプリ」](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx)|
| 操作方法| [チュートリアル: Azure Active Directory と Salesforce を統合する方法](active-directory-saas-salesforce-tutorial.md)|

## アプリ プロキシ

| | |
| ------ | ------ |
| 概要| [内部設置型アプリケーションをセキュリティで保護されたリモート アクセスを提供する方法](active-directory-application-proxy-get-started.md)|
| 操作方法| [を有効にする Azure AD アプリケーション プロキシ](active-directory-application-proxy-enable.md) <br><br> [Azure AD アプリケーション プロキシを使用してアプリケーションを発行](active-directory-application-proxy-publish.md) <br><br> [条件付きアクセスと作業](active-directory-application-proxy-conditional-access.md) <br><br> [Azure AD アプリケーション プロキシのカスタム ドメインの使用](active-directory-application-proxy-custom-domains.md) <br><br> [アプリケーション プロキシで認識アプリケーションにクレームを含む作業](active-directory-application-proxy-claims-aware-apps.md) <br><br> [でのシングル サインオンアプリケーション プロキシ](active-directory-application-proxy-sso-using-kcd.md)|
| トラブルシューティング| [アプリケーション プロキシをトラブルシューティングします。](active-directory-application-proxy-troubleshoot.md)|

## アプリの起動エクスペリエンス

| | |
| ------ | ------ |
| 概要| [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)|
| 操作方法| [Azure Active Directory アクセス パネルの [EMS](http://blogs.msdn.com/b/haddy_el-haggan_blog/archive/2015/04/02/azure-active-directory-access-panel-ems.aspx) <br><br> [と Azure Active Directory の統合](active-directory-how-to-integrate.md)|





