<properties
    pageTitle="ユーザーを Azure RemoteApp コレクションに追加する | Microsoft Azure"
    description="Azure RemoteApp コレクションにユーザーを追加する方法について説明します。"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/05/2015"
    ms.author="elizapo" />


# Azure RemoteApp コレクションにユーザーを追加する方法

Azure RemoteApp のアプリをユーザーが表示および使用できるようにするには、コレクションへのアクセス権をユーザーに付与しておく必要があります。 これは簡単です。**[ユーザー アクセス]** タブで、対象となるユーザーのアカウント情報を入力し、チェック マークをクリックします。

どのようなアカウント情報が必要ですか。 作成したコレクション (クラウドまたはハイブリッド) のタイプとそのコレクションで Office 365 ProPlus を使用しているかどうかによって異なります。

## サポートされているユーザー ID

アプリケーションへのアクセスには、コレクションのタイプ (クラウドとハイブリッド) ごとに、異なるユーザー ID を使用できます。

RemoteApp のハイブリッド コレクションでは、オンプレミスの Active Directory ドメイン インフラストラクチャと Azure Active Directory テナントをディレクトリ統合でセットアップする必要があります (および必要に応じてシングル サインオン)。 さらに、オンプレミスのディレクトリに Active Directory オブジェクトを作成する必要があります。

RemoteApp のクラウド コレクションの場合、Azure Active Directory サポート ID を持つすべてのユーザーに、Microsoft アカウントを組み込むための RemoteApp へのユーザー アクセス権を付与できます。 次の表を参照してください。

Office 365 ユーザーは、Azure Active Directory のユーザーです。 そのユーザーに Azure Active Directory のハイブリッドのディレクトリ同期アカウントがあれば、RemoteApp ハイブリッド デプロイでのユーザー アクセス権を付与できます。

次の表は、コレクションでサポートされる ID と Active Directory の要件を確認するためのクイック リファレンスとして使用できます。

| ユーザー アカウント| クラウド| ハイブリッド|
|--------------|--------|------|
| Microsoft アカウント| はい| いいえ|
| Azure Active Directory (Azure AD)| | |
| Azure AD クラウドのみ| あり| いいえ|
| パスワード同期がある ADsync| あり| あり|
| パスワード同期がない ADsync| あり| いいえ|
| AD FS がある ADsync| あり| あり|
| サード パーティの Azure には、id プロバイダー (Ping など) がサポートされています。| はい| あり|
| Multi-Factor Authentication| あり| はい|

チェック アウト [詳細](remoteapp-ad.md) RemoteApp の Active Directory を構成する方法です。

> [AZURE.NOTE] Azure Active Directory ユーザーは、サブスクリプションに関連付けられているテナントに属している必要があります (サブスクリプションは、ポータルの **[設定]** タブで表示および変更できます。 参照してください [RemoteApp で使用される Azure Active Directory テナントの変更](remoteapp-changetenant.md) の詳細)。

## Office 365 ProPlus ユーザー アカウントの情報

コレクションに、Office 365 ProPlus のテンプレート イメージを使用しているか*または* Office 365 を使用するカスタム イメージを作成した場合は、サブスクリプションの既定のドメイン用の Office 365 サブスクリプションを持つ Azure Active Directory のユーザーのみを追加できます。 参照してください [Azure RemoteApp で Office 365 を使用して](remoteapp-o365.md) の詳細。





