---
title: "Microsoft Azure Active Directory pojedynczego logowania jednokrotnego wtyczki — często zadawane pytania | Dokumentacja firmy Microsoft"
description: "Informacje o sposobie konfigurowania rejestracji jednokrotnej między usługą Azure Active Directory i usługi Microsoft Azure Active Directory rejestracji jednokrotnej dla JIRA."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 4b663047-7f88-443b-97bd-54224b232815
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/06/2018
ms.author: jeedes
ms.openlocfilehash: 571fbd5078f66375f6e81cba2a790121366f9d60
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/16/2018
---
# <a name="microsoft-azure-active-directory-single-sign-on-plugin-faq"></a>Microsoft Azure Active Directory pojedynczego logowania jednokrotnego wtyczki — często zadawane pytania 

## <a name="1-whats-the-microsoft-sso-add-on"></a>1. Co to jest dodatek Microsoft logowania jednokrotnego?

Ten dodatek zapewnia rejestrację jednokrotną JIRA (w tym JIRA Core, oprogramowanie JIRA, JIRA działu) i oprogramowania lokalnego zlewiska Atlassian firmy. Dodatek współpracuje z usługą Azure AD jako dostawca tożsamości.

## <a name="2-add-on-works-with-which-atlassian-products"></a>2. Dodatek współpracuje z produktów, które Atlassian?

Od tej chwili dodatek współpracuje z wersjami lokalnymi JIRA i zlewiska.

## <a name="3-does-this-add-on-work-on-cloud-version"></a>3. Ten dodatek działa w chmurze wersji?

Nie. Obsługiwane są tylko wersje lokalnymi JIRA i zlewiska.

## <a name="4-which-versions-of-jira-and-confluence-are-supported"></a>4. Które wersje JIRA i zlewiska są obsługiwane?

Poniżej znajduje się lista wszystkich wersji, które są obsługiwane:

* Podstawowe JIRA i oprogramowania: 6.0 do 7.2.2 
* JIRA działu: 3.2 do 3.0 
* Zlewiska: 5.10 do 5.0

## <a name="5-is-this-add-on-free-or-paid"></a>5. Jest to bezpłatna dodatek lub zapłacone?

Jest to bezpłatny dodatek i mogą być instalowane z Atlassian handlową.

## <a name="6-do-i-need-to-restart-jiraconfluence-once-i-deploy-the-add-on"></a>6. Należy ponownie uruchomić JIRA/zlewiska po zainstalować dodatek

Ponowne uruchomienie nie jest wymagane po wdrożeniu dodatku. Można uruchomić przy użyciu dodatku natychmiast po wdrożeniu.

## <a name="7-how-do-i-get-support-for-the-add-on"></a>7. Jak uzyskać pomoc techniczną dla dodatku?

Dotrzeć do nas na: <email> . Otrzymasz odpowiedź w ciągu godziny <>. Może też wiązać biletu pomocy technicznej w firmie Microsoft za pośrednictwem kanału portalu Azure. Można również wywołać nam na: <Number> między <> używam do <> pm w dni robocze.

## <a name="8-would-this-add-on-work-on-mac-or-ubuntu-installation-of-jira-and-confluence"></a>8. Ten dodatek będzie działać w instalacji Mac lub Ubuntu JIRA i zlewiska?

Ten dodatek tylko w przypadku 64-bitowe systemu Windows server instalacji JIRA i zlewiska została przetestowana.

## <a name="9-does-this-add-on-work-with-other-idps-than-azure-ad"></a>9. Ten dodatek działa z IdPs innych niż Azure AD?

Nie. Dodatek działa tylko z usługą Azure AD.

## <a name="10-what-version-of-saml-does-the-add-on-work-with"></a>10. Która wersja SAML dodatek działa z?

Dodatek współpracuje z SAML 2.0.

## <a name="11-does-the-add-on-do-use-provisioning-as-well"></a>11. Dodatek należy używać inicjowania obsługi administracyjnej, a także czy?

Nie. Od tej chwili dodatek zapewnia tylko SAML 2.0 logowania jednokrotnego. Użytkownik ma być aprowizowana w aplikacji przed logowanie SSO.

## <a name="12-are-cluster-versions-of-jira-and-confluence-supported-by-add-on"></a>12. Czy wersjach klastra JIRA i zlewiska obsługiwane przez dodatek?

Nie. Dodatek współpracuje z wersjami lokalnymi JIRA i zlewiska.

## <a name="13-would-this-plugin-work-with-http-version-of-jira-and-confluence"></a>13. Ten dodatek plug-in będzie działać z wersją protokołu HTTP JIRA i zlewiska?

Nie. Działa dodatek z protokołu HTTPS włączone tylko instalacji.

## <a name="14-do-i-need-to-buy-license-of-the-add-on"></a>14. Należy kupić licencję dodatek?

Jest to bezpłatny dodatek.