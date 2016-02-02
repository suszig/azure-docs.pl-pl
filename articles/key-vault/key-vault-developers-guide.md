<properties
   pageTitle="Azure Key Vault 開発者ガイド | Microsoft Azure"
   description="開発者は、Azure Key Vault を使用して、Microsoft Azure 環境内で暗号化キーを管理できます。 "
   services="key-vault"
   documentationCenter=""
   authors="BrucePerlerMS"
   manager="mbaldwin"
   editor="bruceper" />
<tags
   ms.service="key-vault"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="11/06/2015"
   ms.author="bruceper" />


# Azure Key Vault 開発者ガイド

> [AZURE.VIDEO azure-key-vault-developer-quick-start]

開発者は、Azure Key Vault を使用して、Microsoft Azure 環境内で暗号化キーを管理できます。 Key Vault は、複数の種類のキーとアルゴリズムをサポートし、重要な顧客キーに対してハードウェア セキュリティ モジュール (HSM) と連携させて使用できます。 さらに、Key Vault を使用して、サイズが制限されたオクテット オブジェクトであるシークレットを、特定のセマンティクスを使用せずに安全に保存できます。 キーとシークレットを混在させて Key Vault に保存できます。 オブジェクトに対するアクセス制御は、種類別に管理されます。

承認されたユーザーは、以下を実行できます。

- 使用して暗号キーの管理 [作成](https://msdn.microsoft.com/library/azure/dn903634.aspx), 、[インポート](https://msdn.microsoft.com/library/azure/dn903626.aspx), 、[更新](https://msdn.microsoft.com/library/azure/dn903616.aspx), 、[削除](https://msdn.microsoft.com/library/azure/dn903611.aspx) およびその他の操作

- 使用して機密情報を管理 [取得](https://msdn.microsoft.com/library/azure/dn903633.aspx), 、[更新] (https://msdn.microsoft.com/library/azure/dn986818.aspx、 [Delete](https://msdn.microsoft.com/library/azure/dn903613.aspx) およびその他の操作

- Use cryptographic keys with [Sign](https://msdn.microsoft.com/library/azure/dn878096.aspx)/[Verify](https://msdn.microsoft.com/library/azure/dn878082.aspx), [WrapKey](https://msdn.microsoft.com/library/azure/dn878066.aspx)/[UnwrapKey](https://msdn.microsoft.com/library/azure/dn878079.aspx) and [Encrypt](https://msdn.microsoft.com/library/azure/dn878060.aspx)/[Decrypt](https://msdn.microsoft.com/library/azure/dn878097.aspx) operations

Key Vault に対する操作は、Azure Active Directory によって認証され、許可されます。

## Key Vault のプログラミング

プログラマー向けの Key Vault 管理システムは、REST を基盤とするさまざまなインターフェイスで構成されます。
[Key Vault REST API リファレンス](https://msdn.microsoft.com/library/azure/dn903609.aspx)

| [![.NET](./media/key-vault-developers-guide/net.png)] (https://msdn.microsoft.com/library/azure/dn903301.aspx)| [![Node.js](./media/key-vault-developers-guide/nodejs.png)] (http://azure.github.io/azure-sdk-for-node/azure-arm-keyvault/latest)
|:--:|:--:|
| [.NET](https://msdn.microsoft.com/library/azure/dn903301.aspx)| [Node.js](http://azure.github.io/azure-sdk-for-node/azure-arm-keyvault/latest)

## Key Vault の管理

Azure Key Vault コンテナー (資格情報コンテナー) は、次の記事の説明に従って、REST、PowerShell または CLI を使用して管理できます。

- [作成し、残りの部分と Key Vault の管理](https://msdn.microsoft.com/library/azure/mt620024.aspx)
- [作成し、PowerShell を使用した Key Vault の管理](key-vault-get-started.md)
- [作成し、CLI を使用した Key Vault の管理](key-vault-manage-with-cli.md)


## 方法

次の記事では、タスクの具体的なガイダンスを提供します。

- [生成し、Azure Key Vault の HSM 保護キーを転送する方法](key-vault-hsm-protected-keys.md)

## 例

- 次のダウンロードには、サンプル アプリケーション (HelloKeyVault) と、Azure Web サービスの例の両方が含まれています。 [Azure Key Vault のコード サンプル](http://www.microsoft.com/download/details.aspx?id=45343)
- このチュートリアルを使用すると、Azure の Web アプリケーションから Azure Key Vault を使用する方法について学習できます。 [Web アプリケーションからの Azure Key Vault の使用](key-vault-use-from-web-application.md)

## 対応ライブラリ

- [Microsoft Azure Key Vault のコア ライブラリ](http://www.nuget.org/packages/Microsoft.Azure.KeyVault.Core/1.0.0) の識別子からキーを検索およびキーを持つ操作の実行を IKey と IKeyResolver のインターフェイスを提供します。

- [Microsoft Azure Key Vault 拡張機能](http://www.nuget.org/packages/Microsoft.Azure.KeyVault.Extensions/1.0.0) Azure Key Vault の拡張機能を提供します。






