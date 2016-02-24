<properties
    pageTitle="Azure AD Connect: は、内部設置型 id を Azure Active Directory と統合します。 | Microsoft Azure"
    description="これは、Azure AD Connect の内容と使用する理由を説明するページです。"
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="12/02/2015"
    ms.author="andkjell;billmath"/>

# オンプレミス ID と Azure Active Directory の統合
Azure AD Connect は、Windows Server Active Directory などのオンプレミス ID システムを Azure Active Directory と統合し、ユーザーを Office 365、Azure、数千の Saas アプリケーションに接続するツールです。 このトピックでは、エンド ユーザーが既存の会社アプリへのアクセスに使っているのと同じ ID でクラウド サービスにアクセスできるようにするうえで必要なコンポーネントを準備およびデプロイする方法を包括的に示します。

![What is Azure AD Connect](./media/active-directory-aadconnect/arch.png)

## Azure AD Connect を使用する理由
オンプレミスのディレクトリと Azure AD を統合すると、クラウドとオンプレミス両方のリソースにアクセスするための共通の ID が提供されるため、ユーザーの生産性が向上します。  この統合によって、ユーザーや組織には次の利点があります。

- ユーザーは、単一の ID を使ってオンプレミスのアプリケーションとクラウド サービス (Office 365 など) にアクセスできます。

- 単一のツールにより、同期とサインインのための容易なデプロイメントを実現できます。

- それぞれのシナリオに適した最新の機能が手に入ります。 Azure AD Connect は、DirSync や Azure AD Sync など、旧バージョンの ID 統合ツールの後継ツールです。 詳細については、次を参照してください。 [ディレクトリ統合ツールの比較](active-directory-aadconnect-get-started-tools-comparison.md)します。


### Azure AD Connect の動作

Azure Active Directory Connect は 3 つの主要な部分で構成されます。  同期サービスとは、オプションの Active Directory フェデレーション サービスの一部は監視の一部を使用して [Azure AD Connect Health](active-directory-aadconnect-health.md)します。

<center>![Azure AD Connect スタック](./media/active-directory-aadconnect-how-it-works/AADConnectStack2.png)
</center>

- 同期 - この部分で構成された、コンポーネントおよび機能としてリリースされていた [Dirsync と Azure AD Sync](active-directory-aadconnect-get-started-tools-comparison.md)します。  これは、ユーザーとグループの作成を担当している部分です。  オンプレミス環境のユーザーとグループに関する情報とクラウド内の情報が一致することも保証します。
- AD FS - これは Azure AD Connect のオプションの部分であり、オンプレミスの AD FS インフラストラクチャを使用して、ハイブリッド環境をセットアップするために使用できます。  この部分は、ドメイン参加 SSO、AD のログインのポリシーの適用、スマート カードやサード パーティの MFA などを含む複雑なデプロイメントに対応するために、組織で使用できます。
- 正常性の監視 - Azure AD Connect Health により、AD FS サーバーを強力に監視し、このアクティビティを Azure ポータルで一元的に表示できます。  追加情報を参照してください。 [Azure Active Directory Connect Health](active-directory-aadconnect-health.md)します。

## Azure AD Connect のインストール

[Azure AD Connect のダウンロードにあります [Microsoft ダウンロード センター](http://go.microsoft.com/fwlink/?LinkId=615771)します。


| 解決策 | シナリオ |
| ----- | ----- |
| 開始する前に | <li>[Azure AD Connect: ハードウェアと前提条件](active-directory-aadconnect-prerequisites.md)</li> |
| [簡単設定](active-directory-aadconnect-get-started-express.md) | <li>AD フォレストが 1 つがある場合、既定オプションお勧めします。</li> <li>パスワード同期を使用して同じパスワードを使用してユーザーのサインインします。</li>
| [カスタマイズした設定](active-directory-aadconnect-get-started-custom.md) | <li>複数のフォレストがある場合に使用されます。 オンプレミスの多くをサポートしている [トポロジ](active-directory-aadconnect-topologies.md)します。</li><li>フェデレーション用の ADFS などのサインイン オプションをカスタマイズするか、サード パーティの ID プロバイダーを使います。</li><li>フィルター処理やライトバックなどの同期機能をカスタマイズします。</li>
| [DirSync からのアップグレード](active-directory-aadconnect-dirsync-upgrade-get-started.md) | <li>ある場合は、既存の DirSync サーバーが既に実行されています。</li>
| Azure AD Sync からのアップグレード | <li>これは、シームレスなインプレース アップグレードです。</li>


[インストール後に](active-directory-aadconnect-whats-next.md) が期待どおりになり、ユーザー ライセンスを割り当てることを確認する必要があります。

### Azure AD Connect のインストールの次のステップ

| トピック |  |
| --------- | --------- |
| Azure AD Connect のダウンロード | [Azure AD Connect のダウンロード](http://go.microsoft.com/fwlink/?LinkId=615771) |
| Express 設定を使用したインストール | [Azure AD Connect の高速インストール](active-directory-aadconnect-get-started-express.md) |
| カスタマイズした設定を使用したインストール | [Azure AD Connect のカスタム インストール](active-directory-aadconnect-get-started-custom.md) |
| DirSync からのアップグレード | [Azure AD 同期ツール (DirSync) からのアップグレード](active-directory-aadconnect-dirsync-upgrade-get-started.md) |
| インストール後 | [インストールの確認とライセンスの割り当て ](active-directory-aadconnect-whats-next.md) |

### Azure AD Connect のインストールの詳細

準備する [運用](active-directory-aadconnectsync-operations.md) 上の問題です。 簡単には、その代替の場合、スタンバイ サーバーを持つ場合、 [災害](active-directory-aadconnectsync-operations.md#disaster-recovery)します。 頻繁な構成を変更する場合に立てる必要があります、 [ステージング モード](active-directory-aadconnectsync-operations.md#staging-mode) サーバーです。

| トピック |  |
| --------- | --------- |
| サポートされているトポロジ | [Azure AD Connect のトポロジ](active-directory-aadconnect-topologies.md) |
| 設計概念 | [Azure AD Connect の設計概念](active-directory-aadconnect-design-concepts.md) |
| インストールで使用するアカウント | [Azure AD Connect の資格情報とアクセス許可の詳細](active-directory-aadconnect-accounts-permissions.md) |
| 運用計画 | [Azure AD Connect Sync: 操作タスクおよび考慮事項](active-directory-aadconnectsync-operations.md) |
| ユーザーのサインイン オプション | [Azure AD Connect ユーザーのサインイン オプション](active-directory-aadconnect-user-signin.md) |

## 機能の構成
Azure AD Connect には、必要に応じて有効にすることができる機能や、既定で有効になっている機能があります。 一部の機能については、シナリオやトポロジに応じて追加の構成が必要になることがあります。

[フィルタ リング](active-directory-aadconnectsync-configure-filtering.md) は Azure AD に同期オブジェクトを制限するときに使用します。 既定では、すべてのユーザー、連絡先、グループ、Windows 10 コンピューターが同期されますが、ドメイン、OU、または属性に基づいて制限することができます。

[パスワード同期](active-directory-aadconnectsync-implement-password-synchronization.md) Active Directory を Azure AD のパスワード ハッシュを同期します。 これにより、ユーザーは、同じパスワードをオンプレミスとクラウドで使用できるようになります。しかも、それを 1 か所で管理できます。 オンプレミスの Active Directory が使用されるため、独自のパスワード ポリシーを使用することができます。

[パスワード ライトバック](active-directory-passwords-getting-started.md) にできますが、変更、クラウドでパスワードをリセットし、オンプレミスのパスワード ポリシーを適用します。

[デバイスの書き戻し](active-directory-aadconnect-get-started-custom-device-writeback.md) に書き戻す内部設置型 Active Directory に条件付きアクセスのために使用できるように Azure AD に登録されているデバイスを許可します。

 [偶発的な削除を防ぐ](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md) 機能が既定で有効にし、多数の同時に削除からクラウド ディレクトリを保護します。 既定では実行ごとに 500 件の削除が許可されていますが、組織のサイズに応じて変更できます。

### 機能を構成する次のステップ

| トピック |  |
| --------- | --------- |
| フィルター処理の構成 | [Azure AD Connect Sync: フィルター処理の構成](active-directory-aadconnectsync-configure-filtering.md) |
| パスワードの同期 | [Azure AD Connect Sync: パスワード同期の実装](active-directory-aadconnectsync-implement-password-synchronization.md) |
| パスワード ライトバック | [パスワード管理の概要](active-directory-passwords-getting-started.md) |
| デバイスの書き戻し | [Azure AD Connect でのデバイスの書き戻しの有効化](active-directory-aadconnect-get-started-custom-device-writeback.md) |
| 誤って削除されないように保護する | [Azure AD Connect Sync: 誤って削除されないように保護する](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md) |

## Azure AD Connect Sync のカスタマイズ
Azure AD Connect Sync には、ほとんどのお客様とトポロジに対応した既定の構成が設定されています。 とはいえ、既定の構成ではうまくいかず、調整が必要な場面も必ず存在します。 このセクションとリンク先のトピックにまとめられているように、構成は変更できます。

かどうかはするが機能しない同期トポロジでは、基本と」の説明に従って使用される用語を理解するを開始する前に、 [技術的概念](active-directory-aadconnectsync-technical-concepts.md)します。 Azure AD Connect は、MIIS2003、ILM2007、FIM2010 が進化したものです。 同じ要素もあるものの、多数の変更が加えられています。

 [既定の構成](active-directory-aadconnectsync-understanding-default-configuration.md) ある可能性があります複数フォレスト構成の前提としています。 これらのトポロジでは、ユーザー オブジェクトが別のフォレスト内の連絡先として表されることがあります。 ユーザーは、別のリソース フォレストにリンクされたメールボックスを持っている場合もあります。 既定の構成の動作については、「 [ユーザーおよび連絡先](active-directory-aadconnectsync-understanding-users-and-contacts.md)します。

同期の構成モデルと呼ばれる [宣言型のプロビジョニング](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md)します。 高度な属性フローを使用している [関数](active-directory-aadconnectsync-functions-reference.md) を属性の変換を表現します。 Azure AD Connect に付属するツールを使って、構成全体を確認、検証できます。 構成を変更する必要がある場合は必ず従って、 [ベスト プラクティス](active-directory-aadconnectsync-best-practices-changing-default-configuration.md) これらが利用できるように、新しいリリースを採用しやすくなるようにします。

### Azure AD Connect Sync のカスタマイズの次のステップ

| トピック |  |
| --------- | --------- |
| 技術的概念 | [Azure AD Connect Sync: 技術的概念](active-directory-aadconnectsync-technical-concepts.md) |
| 既定の構成について | [Azure AD Connect Sync: 既定の構成について](active-directory-aadconnectsync-understanding-default-configuration.md) |
| ユーザーと連絡先について | [Azure AD Connect Sync: ユーザーと連絡先について](active-directory-aadconnectsync-understanding-users-and-contacts.md) |
| 宣言型のプロビジョニング | [Azure AD Connect Sync: 宣言型のプロビジョニングの式について](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md) |
| 宣言型のプロビジョニングの関数リファレンス | [Azure AD Connect Sync: 関数リファレンス](active-directory-aadconnectsync-functions-reference.md) |
| ベスト プラクティス | [既定の構成の変更するためのベスト プラクティス](active-directory-aadconnectsync-best-practices-changing-default-configuration.md) |

## 詳細情報とリファレンス

| トピック |  |
| --------- | --------- |
| バージョン履歴 | [バージョン履歴](active-directory-aadconnect-version-history.md) |
| DirSync、Azure ADSync、Azure AD Connect の比較 | [ディレクトリ統合ツールの比較](active-directory-aadconnect-get-started-tools-comparison.md) |
| 同期される属性 | [同期される属性](active-directory-aadconnectsync-attributes-synchronized.md) |
| Azure AD Connect Health を使用した監視 | [Azure AD Connect Health](active-directory-aadconnect-health.md) |
| よく寄せられる質問 | [Azure AD Connect の FAQ](active-directory-aadconnect-faq.md) |


**その他のリソース**


オンプレミスのディレクトリのクラウドへの拡張に関する 2015 年のプレゼンテーションをご覧ください。

[AZURE.VIDEO microsoft-ignite-2015-extending-on-premises-directories-to-the-cloud-made-easy-with-azure-active-directory-connect]

