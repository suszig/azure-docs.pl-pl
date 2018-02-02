---
title: "Włącz lub wyłącz LinkedIn integracji aplikacji pakietu Office w usłudze Azure Active Directory | Dokumentacja firmy Microsoft"
description: "Wyjaśniono, jak włączyć lub wyłączyć integrację LinkedIn dla aplikacji firmy Microsoft w usłudze Azure Active Directory"
services: active-directory
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: 
ms.devlang: 
ms.topic: article
ms.date: 01/30/2018
ms.author: curtand
ms.reviewer: beengen
ms.custom: it-pro
ms.openlocfilehash: 5ebc44d0ef6200baeacf4f1f8c4371e2d1eed9db
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/01/2018
---
# <a name="linkedin-integration-for-office-applications"></a>Integracja LinkedIn aplikacji pakietu Office
W tym artykule wyjaśniono, jak ograniczyć użytkowników, do których LinkedIn integracji znajduje się w usłudze Azure Active Directory (Azure AD). Integracja LinkedIn jest domyślnie włączona, gdy jest ona dodawana do dzierżawy, który umożliwia użytkownikom uzyskiwanie dostępu do danych publicznych LinkedIn w niektóre z aplikacji firmy Microsoft. Każdy użytkownik niezależnie można łączyć swoje konta firmowego lub szkolnego konta LinkedIn.

> [!IMPORTANT]
> Integracja LinkedIn nie jest wdrażana na wszystkich dzierżaw usługi Azure AD w tym samym czasie. Po jego jest wdrażana z dzierżawą platformy Azure, LinkedIn integracji jest domyślnie włączona. Integracja LinkedIn jest niedostępna dla lokalnych przejdź, suwerenne i dzierżawcy dla instytucji rządowych. 

## <a name="linkedin-integration-from-the-user-perspective"></a>Integracja LinkedIn z punktu widzenia użytkownika
Łącząc ich konta LinkedIn z pracą lub konta służbowego, użytkownicy w Twojej organizacji [pozwalają LinkedIn danych](https://www.linkedin.com/help/linkedin/answer/84077) do użycia w aplikacji firmy Microsoft i usług, które oferuje organizacji. [Użytkownicy mogą odłączyć kont](https://www.linkedin.com/help/linkedin/answer/85097), które powoduje usunięcie uprawnienie LinkedIn udostępniania danych firmy Microsoft. Integracja LinkedIn używa publicznie dostępnych informacji profil LinkedIn. [Użytkownicy mogą kontrolować sposób wyświetlania swój własny profil LinkedIn](https://www.linkedin.com/help/linkedin/answer/83) przy użyciu ustawień prywatności LinkedIn, w tym profilu można wyświetlić w aplikacji firmy Microsoft.

## <a name="privacy-considerations"></a>Zagadnienia dotyczące ochrony prywatności
Włączanie integracji LinkedIn w usłudze Azure AD umożliwia aplikacji firmy Microsoft i usługi, aby uzyskać dostęp do pewnych informacji LinkedIn użytkowników. Odczyt [zasady zachowania poufności informacji firmy Microsoft](https://privacy.microsoft.com/privacystatement/) dowiedzieć się więcej o zagadnienia dotyczące ochrony prywatności podczas włączania integracji LinkedIn w usłudze Azure AD. 

## <a name="manage-linkedin-integration"></a>Zarządzanie LinkedIn integracji
LinkedIn integracji dla przedsiębiorstw, które jest domyślnie włączona w usłudze Azure AD. Włączanie integracji LinkedIn zezwala wszystkim użytkownikom w organizacji do używania funkcji LinkedIn w ramach usług firmy Microsoft, takich jak wyświetlanie LinkedIn profilów w programie Outlook. Wyłączenie integracji LinkedIn usuwa funkcje LinkedIn z usług i aplikacji firmy Microsoft i zatrzymuje danych udostępnianie między LinkedIn i organizacji za pomocą usług firmy Microsoft.

### <a name="enable-or-disable-linkedin-integration-for-your-organization-in-the-azure-portal"></a>Włącz lub wyłącz integrację LinkedIn dla Twojej organizacji w portalu Azure

1. Zaloguj się do [Centrum administracyjnego usługi Azure Active Directory](https://aad.portal.azure.com/) przy użyciu konta, które jest administratorem globalnym dla dzierżawy usługi Azure AD.
2. Wybierz **użytkowników i grup**.
3. Na **użytkowników i grup** bloku, wybierz opcję **ustawienia użytkownika**.
4. W obszarze **integracji LinkedIn**, wybierz pozycję **tak** lub **nr** Aby włączyć lub wyłączyć integrację LinkedIn.
   ![Włączanie integracji LinkedIn](./media/linkedin-integration/LinkedIn-integration.PNG)

### <a name="enable-or-disable-linkedin-integration-for-your-organizations-office-2016-apps-using-group-policy"></a>Włącz lub wyłącz integrację LinkedIn dla aplikacji pakietu Office 2016 w organizacji za pomocą zasad grupy

1. Pobierz [plików szablonów administracyjnych pakietu Office 2016 (ADMX/ŚILS)](https://www.microsoft.com/download/details.aspx?id=49030)
2. Wyodrębnij **ADMX** pliki i skopiuj je do Twojej **centralnym repozytorium**.
3. Zarządzanie zasadami grupy Otwórz.
4. Utwórz obiekt zasad grupy z następującymi ustawieniami: **Konfiguracja użytkownika** > **Szablony administracyjne** > **Microsoft Office 2016**  >  **Różne** > **umożliwiają integrację LinkedIn**.
5. Wybierz **włączone** lub **wyłączone**.
  * Gdy zasady są **włączone**, **funkcji Pokaż LinkedIn w aplikacjach pakietu Office** znaleźć w oknie dialogowym Opcje pakietu Office 2016 jest włączone. Oznacza to również użytkowników w organizacji można użyć funkcji LinkedIn w swoich aplikacjach pakietu Office.
  * Gdy zasady są **wyłączone**, **funkcji Pokaż LinkedIn w aplikacjach pakietu Office** znaleziono ustawienia w opcjach programu Office 2016 okna dialogowego ustawiono stan wyłączone, a użytkownicy końcowi nie można zmienić tego ustawienia. Użytkownicy w Twojej organizacji nie można użyć funkcji LinkedIn w aplikacjach pakietu Office 2016. 

Zasady grupy dotyczy tylko pakiet Office 2016 aplikacji na komputerze lokalnym. Użytkownicy mogą zobaczyć funkcje LinkedIn w profilu karty w usłudze Office 365, nawet wtedy, gdy wyłączenie LinkedIn w swoich aplikacjach pakietu Office 2016. 

### <a name="learn-more"></a>Dowiedz się więcej 
* [LinkedIn informacji i funkcji w aplikacjach firmy Microsoft](https://go.microsoft.com/fwlink/?linkid=850740)

* [Centrum pomocy LinkedIn](https://www.linkedin.com/help/linkedin)

## <a name="next-steps"></a>Kolejne kroki
Aby zobaczyć bieżące ustawienia integracji LinkedIn w portalu Azure, użyj następującego łącza:

[Konfigurowanie integracji LinkedIn](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/UserSettings) 