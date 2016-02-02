<properties 
    pageTitle="Azure Active Directory のアプリケーションに対す SAML に基づいたシングル サインオンをデバッグする方法 | Microsoft Azure" 
    description="Azure Active Directory のアプリケーションに対す SAML に基づいたシングル サインオンをデバッグする方法について説明します " 
    services="active-directory" 
    authors="asmalser-msft"  
    documentationCenter="na" manager="stevenpo"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="11/18/2015" 
    ms.author="asmalser" />


# Azure Active Directory のアプリケーションに対す SAML に基づいたシングル サインオンをデバッグする方法

SAML ベースのアプリケーション統合をデバッグするときなどのツールを使用すると便利、多くの場合、 [Fiddler](http://www.telerik.com/fiddler) SAML 要求、SAML 応答と、アプリケーションに発行する実際の SAML トークンを表示します。 SAML トークンを確認することで、必要な属性、SAML サブジェクト内のユーザー名、および発行者 URI のすべてが予想どおりに送信されたことを確認できます。

![][1]

SAML トークンを含む Azure AD からの応答は通常、1 つ https://login.windows.net から HTTP 302 リダイレクト後に発生し、送信する、構成されている **応答 URL** アプリケーションのです。

SAML トークンは、行を選択し、右側のパネルで **[インスペクター]、[WebForms]** の順に選択することで表示できます。 そこから、**SAMLResponse** 値を右クリックし、**[TextWizard に送信]** を選択します。 次に、**[変換]** メニューの **[Base64 から]** を選択してトークンをデコードし、その内容を確認します。

**注**: この HTTP 要求の内容を Fiddler で表示するには、HTTPS トラフィックの暗号化解除を構成することを求めるプロンプトが表示される可能性があります。この場合は、構成を実行する必要があります。



[1]: ./media/active-directory-saml-debugging/fiddler.png 

