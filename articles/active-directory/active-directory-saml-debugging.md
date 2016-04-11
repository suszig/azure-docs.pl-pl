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

#Azure Active Directory のアプリケーションに対する SAML に基づいたシングル サインオンをデバッグする方法

SAML ベースのアプリケーション統合をデバッグするときなどのツールを使用すると便利、多くの場合、 [Fiddler](http://www.telerik.com/fiddler) SAML 要求、SAML 応答と、アプリケーションに発行する実際の SAML トークンを表示します。 SAML トークンを確認することで、必要な属性、SAML サブジェクト内のユーザー名、および発行者 URI のすべてが予想どおりに送信されたことを確認できます。

![][1]

SAML トークンを含む Azure AD からの応答は通常、1 つ https://login.windows.net から HTTP 302 リダイレクト後に発生し、送信する、構成されている **応答 URL** アプリケーションのです。 
 
SAML トークンを表示するには次の行を選択し、選択して、 **インスペクター > WebForms** 、右側のパネルでタブをクリックします。 そこから、右クリックし、 **SAMLResponse** 値し、選択 **TextWizard に送信**します。 選択し、 **から Base64** から、 **変換** ] メニューのトークンをデコードし、その内容を参照してください。
 
**注**: この HTTP 要求の内容を確認するには、Fiddler が表示されることを行う必要がありますが、HTTPS トラフィックの暗号化解除を構成します。

<!--Image references-->
[1]: ./media/active-directory-saml-debugging/fiddler.png
