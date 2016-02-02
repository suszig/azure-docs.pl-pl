<properties
    pageTitle="Azure Active Directory: テナント サポートの作成に関するトピック | Microsoft Azure"
    description="Azure Active Directory テナントまたは Azure Active Directory B2C テナントの作成 - 問題と解決策"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="msmbaldwin"
    editor="curtand"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/24/2015"
    ms.author="swkrish"/>


# Azure AD テナントまたは Azure AD B2C テナントの作成 - 問題と解決策

[AZURE.INCLUDE [active-directory-b2c-preview-note](../../includes/active-directory-b2c-preview-note.md)]

## Azure AD テナントの作成

Azure AD テナントの初めての作成が失敗した場合は、もう一度やり直してください。 問題が解決しない場合は、サポートにお問い合わせください。

## Azure AD B2C テナントの作成 (プレビュー)

既知の中に発生する可能性のある問題は、 [Azure AD B2C テナントの作成](active-directory-b2c-get-started.md)します。

- Azure AD B2C テナントがテナントの一覧に表示されない場合は、もう一度やり直してください。
- Azure AD B2C テナントがテナントの一覧に表示されても、"B2C テナント 'contosob2c' の作成を完了できませんでした" というエラー メッセージが表示されます。これを参照してください [リンク](http://go.microsoft.com/fwlink/?LinkID=624192&clcid=0x409) 詳細ガイダンスは"を次のいずれかの操作を行います。
    - このリンクを使用して、Azure プレビュー ポータルで B2C 機能ブレードに移動 [https://portal.azure.com/{tenant}.onmicrosoft.com/?Microsoft_AAD_B2CAdmin=true#blade/Microsoft_AAD_B2CAdmin/TenantManagementBlade/id/{tenant}.onmicrosoft.com](https://portal.azure.com/{directory}.onmicrosoft.com/?Microsoft_AAD_B2CAdmin=true#blade/Microsoft_AAD_B2CAdmin/TenantManagementBlade/id/{tenant}.onmicrosoft.com), ここで、 **{tenant}** はグローバル管理者として、テナントの作成時 (たとえば、contosob2c) およびサインインに使用される名前で置き換えられます。 ブレードの上部にある **[B2C の機能を有効にする]** をクリックし、**[OK]** をクリックします。 Azure AD B2C テナントを使用できるようになります。
    - 作成したテナントを削除し、もう一度やり直します。
- 上記の解決策で問題が解決しない場合は、サポートに問い合わせてください。 読み取り [ここ](active-directory-b2c-support.md) Azure AD B2C のサポート要求をファイルに説明します。





