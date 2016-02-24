<properties
   pageTitle="複数の Azure Active Directory ディレクトリの追加と管理 | Microsoft Azure"
   description="Azure Active Directory ディレクトリの追加と管理のための手順とベスト プラクティスについて説明します。ディレクトリを完全に独立したリソースとして説明しています。"
   services="active-directory"
   documentationCenter=""
   authors="curtand"
   manager="stevenpo"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="12/01/2015"
   ms.author="curtand"/>

# 複数の Azure Active Directory ディレクトリの追加と管理

Azure Active Directory (Azure AD) の各ディレクトリは、完全に独立したリソースです。つまり、対等であり、フル機能を備え、管理対象の他のディレクトリから論理的に独立しています。 ディレクトリ間に親子関係はありません。 このディレクトリ間の独立には、リソースの独立、管理上の独立、同期の独立があります。

##リソースの独立

後述する外部ユーザーの一部の例外を除き、あるディレクトリでリソースを作成または削除しても、別のディレクトリのリソースには影響しません。 あるディレクトリで "contoso.com" というカスタム ドメインを使用している場合、このドメインを他のディレクトリで使用することはできません。

##管理上の独立

場合は"Contoso"ディレクトリの管理者以外のユーザーは、[テスト ディレクトリ 'Test' を作成します。
- 既定では、ディレクトリを作成したユーザーがその新しいディレクトリ内の外部のユーザーとして追加され、そのディレクトリのグローバル管理者ロールを割り当てられます。
- "Contoso"ディレクトリの管理者は、"Test"ディレクトリに管理者特権を直接付与しない限りありません 'Test' の管理者具体的にこれらの権限。 'Test' を作成したユーザー アカウントを制御している場合、"Contoso"の管理者はディレクトリ 'Test' へのアクセスを制御できます。
- 変更した場合 (追加または削除) 1 つのディレクトリ内のユーザーの管理者の役割、変更は、ユーザーが別のディレクトリにある可能性がある管理者の役割を影響しません。

##同期の独立

各 Azure AD ディレクトリのいずれか 1 つのインスタンスから同期されているデータを取得するには、個別に構成できます。
  - ディレクトリ同期 (DirSync) ツールを 1 つの AD フォレストとデータを同期します。
  - Azure Active Directory コネクタの Forefront Identity Manager、1 つまたは複数の内部設置型フォレスト、および非 Azure AD のデータ ソースとデータを同期します。

##Azure AD ディレクトリを追加する

Azure クラシック ポータルで Azure AD ディレクトリを追加するに左側の [Azure Active Directory 拡張機能を選択し、タップ **追加**します。

> [AZURE.NOTE]   他の Azure リソースとは異なりのディレクトリは Azure サブスクリプションの子リソースではありません。 Azure サブスクリプションを取り消した場合や、期限切れを許可した場合でも、Azure PowerShell、Azure Graph API、Office 365 管理センターなどの他のインターフェイスを使用して、ディレクトリ データに引き続きアクセスできます。 また、ディレクトリに別のサブスクリプションを関連付けることもできます。

Azure AD のライセンスに関する問題とベスト プラクティスの広範な概要については、次を参照してください。 [内容は、Azure Active Directory ライセンス?](active-directory-licensing-what-is.md)します。

