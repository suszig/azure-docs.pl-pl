<properties
    pageTitle="Azure Active Directory のエディション | Microsoft Azure"
    description="Azure Active Directory の無料エディションと有料エディションの選択について説明します。"
    services="active-directory"
    documentationCenter=""
    authors="MarkusVi"
    manager="stevenpo"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/04/2015"
    ms.author="markvi"/>


# Azure Active Directory のエディション

Microsoft のオンライン ビジネス サービスはすべて、サインオンやその他の ID のニーズを Azure Active Directory に依存しています。 Microsoft のオンライン ビジネス サービス (Office 365、Microsoft Azure など) をサブスクライブすると、Azure Active Directory (Azure AD) と、以下で説明する Free エディションのすべての機能へのアクセス許可が与えられます。

Azure Active Directory は、従業員、パートナー、顧客向けにクラウドで包括的な ID およびアクセス管理機能を提供するサービスです。 このサービスには、開発者向けにディレクトリ サービス、高度な ID 管理、標準に準拠した機能豊富なプラットフォーム、および独自のアプリケーションや事前に統合された多数のアプリケーションに対応したアプリケーション アクセス管理が集約されています。 Azure Active Directory Free エディションでは、ユーザーとグループの管理、オンプレミスのディレクトリとの同期、Azure、Office 365、および Salesforce、Workday、Concur、DocuSign、Google Apps、Box、ServiceNow、Dropbox などの多くの人気のある SaaS アプリケーション間でのシングル サインオンを実現することができます。 Azure Active Directory に関する詳細についてを参照してください [Azure AD は](active-directory-whatis.md)します。


Azure Active Directory を強化するには、Azure Active Directory Basic エディションおよび Premium エディションを使用することで有料の機能を追加できます。 Azure Active Directory の有料エディションは、既存の無料のディレクトリ上に構築されます。セルフサービス、拡張された監視機能、セキュリティ レポート、Multi-Factor Authentication (MFA)、セキュリティで保護されたモバイル ユーザーのアクセスなどエンタープライズ クラスの機能を提供します。

Office 365 サブスクリプションには、下の比較表に記載されているその他の Azure Active Directory 機能が含まれます。

> [AZURE.NOTE] これらのエディションの価格オプションについては、次を参照してください。 [Azure Active Directory 料金](https://azure.microsoft.com/pricing/details/active-directory/)します。<br>Azure Active Directory Premium および Azure Active Directory Basic は現在サポートされていません中国でします。詳細については、Azure Active Directory フォーラムからお問い合わせください。


- **Azure Active Directory Basic**: このエディションは、クラウド優先のニーズを持つタスク ワーカー向けに設計されており、クラウド中心のアプリケーション アクセスおよびセルフサービス ID 管理のソリューションを提供します。 Azure Active Directory Basic エディションでは、グループベースのアクセス管理、クラウド アプリケーション向けのセルフサービスのパスワード リセット、Azure Active Directory アプリケーション プロキシ (Azure Active Directory を使用してオンプレミス Web アプリケーションを発行するため) などの生産性の強化とコスト削減の機能が提供され、すべてがアップタイム 99.9% のエンタープライズレベルの SLA によって保証されます。

- **Azure Active Directory Premium**: Azure Active Directory Premium エディションは、より要求の厳しい ID およびアクセスの管理を必要とする組織を支援することを目的として、機能豊富なエンタープライズレベルの ID 管理機能が追加され、ハイブリッド ユーザーがオンプレミスの機能とクラウドの機能にシームレスにアクセスできるようになっています。 このエディションには、クラウド内のアプリケーション アクセス、セルフサービスの ID とアクセスの管理 (IAM)、ID 保護とセキュリティに関して、ハイブリッド環境のインフォメーション ワーカーと ID 管理者が必要とするすべてが含まれています。 また、動的なグループやセルフサービス グループ管理のような高度な管理と委任のリソースをサポートします。 さらに、Microsoft Identity Manager (オンプレミスの ID およびアクセス管理スイート) が含まれており、オンプレミス ユーザー向けのセルフ サービスのパスワード リセットなどのソリューションを実現するクラウドの書き戻し機能を提供します。

サインアップして今すぐ Active Directory Premium を使用して、次を参照してください。 [Azure Active Directory Premium の概要](active-directory-get-started-premium.md)します。

> [AZURE.NOTE] 
>さまざまな Azure Active Directory の機能を "従量課金制" のエディションで使用できます。
>
>-Active Directory B2C は、コンシューマー向けアプリケーションの id およびアクセス管理ソリューションです。 詳細については、「 [Azure Active Directory B2C](https://azure.microsoft.com/documentation/services/active-directory-b2c/)

>-  Azure Multi-Factor Authentication は、ユーザーごとまたは認証プロバイダーごとに使用できます。 詳細については、「 [Azure 多要素認証何ですか?](multi-factor-authentication.md)


<br>

| 機能の種類| 特徴| Free エディション| Basic エディション| Premium Edition| Office 365 アプリのみ|
| --- | --- | --- | --- | --- | --- |
| **共通機能**| ディレクトリ オブジェクト [1]| 最大で 500,000 のオブジェクト| オブジェクトの制限なし| オブジェクトの制限なし| Office 365 ユーザー アカウントにはオブジェクトの制限なし|
| | [ユーザーとグループの管理 (追加/更新/削除) のユーザー ベースのプロビジョニング](active-directory-administer.md), 、[デバイスの登録](active-directory-conditional-access-device-registration-overview.md)| ![○][12]| ![○][12]| ![○][12]| ![○][12]|
| | [SaaS アプリケーション、カスタム アプリケーション、アプリケーション プロキシ アプリへの SSO](active-directory-enable-sso-scenario.md)| 10 個のアプリ (ユーザーあたり) [2]| 10 個のアプリ (ユーザーあたり) [2]| 制限なし| 10 個のアプリ (ユーザーあたり) [2]|
| | [クラウド ユーザーに対するセルフ_サービスのパスワードを変更します。](active-directory-passwords-update-your-own-password.md)| ![○][12]| ![○][12]| ![○][12]| ![○][12]|
| | [内部設置型ディレクトリと Azure Active Directory 間の同期用の接続します。](active-directory-aadconnect.md)| ![○][12]| ![○][12]| ![○][12]| ![○][12]|
| | **プレビュー**:[ B2B コラボレーション](active-directory-b2b-collaboration-overview.md)| ![○][12]| ![○][12]| ![○][12]| ![○][12]|
| | [セキュリティ]、[使用状況レポート](active-directory-view-access-usage-reports.md)| 基本レポート| 基本レポート| 詳細レポート| 基本レポート|
| **Premium および Basic の機能**| [グループ ベースのアプリケーション アクセス管理とプロビジョニング](active-directory-accessmanagement-group-saasapps.md)| | ![○][12]| ![○][12]| |
| | [クラウド ユーザーのセルフ サービス パスワード リセット](active-directory-passwords.md)| | ![○][12]| ![○][12]| ![○][12]|
| | [会社のブランド化 (ログオン ページとアクセス パネルのカスタマイズ)](active-directory-add-company-branding.md)| | ![○][12]| ![○][12]| ![○][12]|
| | [アプリケーション プロキシ](active-directory-application-proxy-get-started.md)| | ![○][12]| ![○][12]| |
| | [高可用性 SLA アップタイム (99.9%)](https://azure.microsoft.com/support/legal/sla/)| | ![○][12]| ![○][12]| ![○][12]|
| **Premium のみの機能**| セルフサービスによるグループ管理/セルフサービスによるアプリケーションの追加/動的なグループ| | | ![○][12]| |
| | [内部設置型のセルフ サービス パスワード リセット、変更、ロックを解除の書き戻し](active-directory-passwords-getting-started.md/#enable-users-to-reset-or-change-their-ad-passwords)| | | ![○][12]| |
| | [多要素認証 (クラウドと内部設置型)](multi-factor-authentication.md)| | | ![○][12]| Office 365 アプリ用のクラウドのみに限定|
| | [ユーザー ライセンスの Microsoft Identity Manager (MIM) と MIM サーバー [3](http://www.microsoft.com/server-cloud/products/microsoft-identity-manager/default.aspx)| | | ![○][12]| |
| | [Cloud App Discovery](active-directory-cloudappdiscovery-whatis.md)| | | ![○][12]| |
| | [Azure Active Directory Connect Health](active-directory-aadconnect-health.md)| | | ![○][12]| |
| | グループ アカウントの自動パスワード ロールオーバー| | | ![○][12]| |
| | **プレビュー**: 条件付きアクセス| | | ![○][12]| |
| | **プレビュー**: Azure AD Privileged Identity Management| | | ![○][12]| |
| **Windows 10 および Azure AD 参加に関連する機能**| Azure AD、デスクトップ SSO、Azure AD 用の Microsoft Passport、管理者による Bitlocker 回復への Windows 10 デバイスの参加| ![○][12]| ![○][12]| ![○][12]| ![○][12]|
| | MDM 自動の登録、セルフ サービスの Bitlocker の回復、Azure AD 参加を使用して Windows 10 デバイスへの追加のローカル管理者| | | ![○][12]| |





[1] 既定の使用量クォータは 150,000 オブジェクトです。 1 個のオブジェクトは、一意の識別名によって表される、ディレクトリ サービス内の 1 つのエントリです。 オブジェクトの例としては、認証目的に使用されるユーザー エントリなどがあります。 この既定のクォータを超えるオブジェクト数が必要な場合は、サポートにお問い合わせください。 ディレクトリ サービスとして Azure Active Directory を利用する Office 365、Microsoft Intune、その他の有料 Microsoft オンライン サービスには、500,000 オブジェクトの制限は適用されません。

[2] Azure AD Free と Azure AD Basic で、SaaS アプリへのアクセス権が割り当てられているエンド ユーザーは、アクセス パネルで最大 10 個のアプリを表示でき、アプリへの SSO アクセスを取得できます。 管理者は SSO を構成して、Free と Basic のユーザーに対して、SaaS アプリへのアクセス権を何個でも割り当てることができますが、エンド ユーザーはアクセス パネルで一度に 10 個のアプリしか表示できません。

[3] Microsoft Identity Manager サーバー ソフトウェアの使用権は、Windows Server ライセンス (任意のエディション) と共に付与されます。 Microsoft Identity Manager は、Windows Server OS で実行されるため、サーバーで Windows Server の有効なライセンス コピーが実行されている限り、そのサーバー上に Microsoft Identity Manager をインストールでき、使用することができます。 Microsoft Identity Manager サーバーに他の個別のライセンスは不要です。



## 参照トピック

- [Azure Active Directory Premium の概要](active-directory-get-started-premium.md)
- [会社のブランド設定サインイン ページとアクセス パネル ページの追加します。](active-directory-add-company-branding.md)
- [アクセスおよび使用状況レポートを表示します。](active-directory-view-access-usage-reports.md)




[12]: ./media/active-directory-editions/ic195031.png 

