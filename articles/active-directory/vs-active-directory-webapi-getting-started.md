<properties 
    pageTitle="Azure Active Directory と Visual Studio 接続済みサービスの概要 (Web API プロジェクト) | Microsoft Azure" 
    description="Visual Studio 接続済みサービスを使用して Azure Active Directory を接続または作成した後に、Web API プロジェクトで Azure AD の使用を開始する方法について説明します。" 
    services="active-directory"
    documentationCenter="" 
    authors="TomArcher" 
    manager="douge" 
    editor="tglee"/>
  
<tags 
    ms.service="active-directory" 
    ms.workload="web" 
    ms.tgt_pltfrm="vs-getting-started" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/03/2015" 
    ms.author="tarcher"/>

# Azure Active Directory と Visual Studio 接続済みサービスの概要 (Web API プロジェクト)

> [AZURE.SELECTOR]
> - [Getting Started (概要)](vs-active-directory-webapi-getting-started.md)
> - [変更内容](vs-active-directory-webapi-what-happened.md)

##コントローラーへのアクセスに対して認証を要求する
 
プロジェクト内のすべてのコント ローラーが修飾された、 **Authorize** 属性です。 この属性により、ユーザーがこれらのコントローラーによって定義された API にアクセスする際に認証が求められます。 これらのコントローラーに匿名でアクセスできるようにするには、コントローラーからこの属性を削除します。 より細かなレベルでアクセス許可を設定するには、コントローラー クラスではなく、認証を必要とするそれぞれのメソッドに対してこの属性を割り当てます。

[Azure Active Directory の詳細を確認する](http://azure.microsoft.com/services/active-directory/)
 
