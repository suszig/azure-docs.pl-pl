<properties
    pageTitle="Azure Active Directory B2C プレビュー: カスタム属性 | Microsoft Azure"
    description="Azure Active Directory B2C でカスタム属性を使用してコンシューマーに関する情報を収集する方法"
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
    ms.date="09/22/2015"
    ms.author="swkrish"/>

#  Azure Active Directory B2C プレビュー: カスタム属性を使用してコンシューマーに関する情報を収集する

[AZURE.INCLUDE [active-directory-b2c-preview-note](../../includes/active-directory-b2c-preview-note.md)]

Azure Active Directory (AD) B2C ディレクトリには、組み込みの属性セットが用意されています (名、姓、市区町村、郵便番号など)。ただし、どのようなコンシューマー向けアプリケーションにも、コンシューマーから収集する情報 (属性) について固有の要件があります。 Azure AD B2C では、ディレクトリを拡張できます (具体的には、各コンシューマー アカウントで保持される属性セットの拡張)。 カスタム属性を作成することができます、 [Azure プレビュー ポータル](https://portal.azure.com/) し、次のように、サインアップのポリシーで使用します。 読み取りし、ように、Azure AD Graph API を使用してこれらの属性を記述することができますも [ここ](active-directory-b2c-devquickstarts-graph-dotnet.md)します。

> [AZURE.NOTE]
カスタム属性を使用して [Azure AD Graph API ディレクトリ スキーマ拡張機能](https://msdn.microsoft.com/library/azure/dn720459.aspx) 内部的なです。

## カスタム属性を作成する方法

1. [Azure プレビュー ポータルで B2C 機能ブレードに移動](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. クリックして **ユーザー属性**します。
3. クリックして **+ 追加** ブレードの上部にあります。
4. 提供、 **名前** カスタム属性 (たとえば、"ShoeSize") と、必要に応じて、 **説明**します。 クリックして **作成**します。 以上で終わりです。

    > [AZURE.NOTE]
    "String"のみ **データ型** 現在利用可能なです。 将来的には、他のデータ型 (DateTime、Integer など) も追加される予定です。

カスタム属性の一覧で提供されて **ユーザー属性** し、サインアップのポリシーで使用します。

## サインアップ ポリシーでカスタム属性を使用する方法

1. 上の B2C 機能ブレードに移動、 [Azure プレビュー ポータル](htts://portal.azure.com/)します。 読み取り [ここ](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) これを行う方法です。
2. クリックして **サインアップ ポリシー**します。
3. クリックしてサインアップ ポリシーを開きます (例: "B2C_1_SiUp")。 クリックして **編集** ブレードの上部にあります。
4. クリックして **サインアップ属性** カスタム属性 (たとえば、"ShoeSize") を選択します。 Click **OK**.
5. クリックして **アプリケーション クレーム** カスタム属性を選択します。 Click **OK**. 
6. クリックして **保存** ブレードの上部にあります。 以上で終わりです。

ポリシーで [今すぐ実行] 機能を使用して、コンシューマー エクスペリエンスを確認できます。 コンシューマーのサインアップ時に収集される属性の一覧、およびアプリケーションに返送されるトークンに、"ShoeSize" が表示されるようになります。

