---
title: Microsoft Azure Active Directory pojedynczego logowania jednokrotnego wtyczki Admin przewodnik | Dokumentacja firmy Microsoft
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
ms.openlocfilehash: af949d1db8af37a534a16364f9f0763479c436e4
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/16/2018
---
# <a name="microsoft-azure-active-directory-single-sign-on-plugin-admin-guide"></a>Microsoft Azure Active Directory pojedynczego logowania jednokrotnego wtyczki Admin przewodnik

## <a name="table-of-contents"></a>Spis treści

1. **[OMÓWIENIE](#overview)**
2. **[JAK TO DZIAŁA](#how-it-works)**
3. **[AUDIENCE](#audience)**
4. **[WARTOŚCI DOMYŚLNE](#assumptions)**
5. **[PREREQUISITES](#prerequisites)**
6. **[OBSŁUGIWANE WERSJE JIRA I ZLEWISKA](#supported-versions-of-jira-and-confluence)**
7. **[INSTALACJA](#installation)**
8. **[KONFIGURACJA WTYCZKI](#plugin-configuration)**
9. **[WYJAŚNIENIE POLA NA EKRANIE KONFIGURACJI DODATKU:](#field-explanation-for-add---on-configuration-screen:)**
10. **[ROZWIĄZYWANIE PROBLEMÓW](#troubleshooting)**

## <a name="overview"></a>Przegląd

Te dodatki umożliwiają klientom usługi Microsoft Azure AD użycie swoją nazwę organizacji i hasło do logowania do Atlassian Jira i zlewiska serwera na podstawie produktów. Implementuje SAML 2.0 na podstawie logowania jednokrotnego.

## <a name="how-it-works"></a>Jak to działa

Gdy użytkownik chce się zalogować do Atlassian Jira lub zlewiska aplikacji, zostanie wyświetlona **Zaloguj się za pomocą usługi Azure AD** przycisk na stronę logowania. Po kliknięciu na nim, są one wymagane do logowania przy użyciu usługi Azure AD organizacja strony logowania.

Gdy użytkownik jest uwierzytelniony, ich należy zalogować się do aplikacji. Jeśli zostali już uwierzytelnieni z Identyfikatorem organizacji i hasło, a następnie bezpośrednio logowania do aplikacji. Należy również zauważyć, że logowanie działa za pośrednictwem JIRA i zlewiska. Jeśli użytkownicy są zalogowani do aplikacji JIRA zlewiska również jest otwarty w tym samym oknie przeglądarki, należy zalogować się raz i nie trzeba ponownie podaj poświadczenia dla innych aplikacji. Użytkownicy, można także uzyskać produktu Atlassian za pośrednictwem myapps w ramach konta platformy Azure i powinny być rejestrowane bez ponownego podawania poświadczeń.

> [!NOTE]
> Inicjowanie obsługi użytkowników nie odbywa się przy użyciu tego dodatku.

## <a name="audience"></a>Grupy odbiorców

Administratorzy JIRA i zlewiska, które są planowane jest używanie tej wtyczki do włączenia funkcji logowania jednokrotnego przy użyciu usługi Azure AD.

## <a name="assumptions"></a>Wartości domyślne

* Wystąpienie JIRA/zlewiska jest obsługujące protokół HTTPS
* Użytkownicy, zostały już utworzone w JIRA/zlewiska
* Użytkownicy mają przypisane w JIRA/zlewiska roli
* Administratorzy mają dostęp do informacji wymaganych do skonfigurowania wtyczki
* JIRA/zlewiska są dostępne spoza sieci firmowej również
* Dodaj na działa tylko wersję lokalną JIRA i zlewiska

## <a name="prerequisites"></a>Wymagania wstępne

Uwaga następujące wymagania wstępne, aby kontynuować instalację dodatku:

* JIRA/zlewiska są zainstalowane na Windows 64-bitowej wersji
* Wersje JIRA/zlewiska są obsługujące protokół HTTPS
* Należy pamiętać, obsługiwana wersja wtyczki w poniższej sekcji "Obsługiwane wersje".
* JIRA/zlewiska jest dostępny w Internecie.
* Poświadczenia administratora dla JIRA/zlewiska
* Poświadczenia administratora dla usługi Azure AD
* WebSudo powinno zostać wyłączone w JIRA i zlewiska

## <a name="supported-versions-of-jira-and-confluence"></a>Obsługiwane wersje JIRA i zlewiska

Od tej chwili obsługiwane są następujące wersje JIRA i zlewiska:

* Podstawowe JIRA i oprogramowania: 6.0 do 7.2.0
* JIRA działu: 3.2 do 3.0
* Zlewiska: 5.10 do 5.0

## <a name="installation"></a>Instalacja

Administrator powinien kroków podaną poniżej, aby zainstalować dodatek plug-in:

1. Zaloguj się do swojego wystąpienia JIRA/zlewiska jako administrator
    
2. Przejdź do JIRA/zlewiska administracji i kliknij dodatków.
    
3. Z Atlassian rynek, wyszukaj **wtyczki logowania jednokrotnego SAML firmy Microsoft**
 
4. Odpowiednią wersję dodatku pojawia się podczas wyszukiwania.
 
5. Wybierz wtyczki i UPM instaluje takie same.
 
6. Po zainstalowaniu wtyczki wygląda na to, z użytkownika zainstalowane dodatki sekcji Zarządzanie dodatkami
 
7. Należy skonfigurować wtyczki, przed rozpoczęciem korzystania z niego.
 
8. Kliknij wtyczki i widoczny przycisk Konfiguruj.
 
9. Kliknij na niej Podaj dane wejściowe konfiguracji
    
## <a name="plugin-configuration"></a>Konfiguracja wtyczki

Poniższa ilustracja przedstawia przykładowy na ekranie konfiguracji dodatku w JIRA i zlewiska
    
![Konfiguracja dodatku](./media/ms-confluence-jira-plugin-adminguide/jira.png)

### <a name="field-explanation-for-add-on-configuration-screen"></a>Wyjaśnienie pola na ekranie konfiguracji dodatku:

1.   Adres URL metadanych: Adres URL, aby pobrać metadanych federacji z usługą Azure AD
 
2.   Identyfikator: Używany przez usługę Azure AD, aby sprawdzić źródła żądania. Mapuje element identyfikatora w usłudze Azure AD. To jest uzyskiwany w wyniku wtyczki jako https://<domain:port>/ automatycznie
 
3.   Adres URL odpowiedzi: Użyj adresu URL odpowiedzi w Twojej IdP zainicjować logowania SAML. Mapuje element adres URL odpowiedzi w usłudze Azure AD. To jest uzyskiwany w wyniku wtyczki jako https://<domain:port>/plugins/servlet/saml/auth automatycznie
 
4.   Zaloguj się na adres URL: Użyj logowania na adres URL w Twojej IdP zainicjować logowania SAML. Mapuje element rejestracji jednokrotnej w usłudze Azure AD. To jest uzyskiwany w wyniku wtyczki jako https://<domain:port>/plugins/servlet/saml/auth automatycznie
 
5.   Identyfikator jednostki IdP: Identyfikator jednostki, która korzysta z dostawców tożsamości. To jest wypełniana podczas adres URL metadanych został rozwiązany.
 
6.   Adres URL logowania: Adres URL logowania z użytkownika IdP. Jest to pochodzą z usługi Azure AD, gdy adres URL metadanych został rozwiązany.
 
7.   Adres URL wylogowania: adres URL wylogowania z dostawców Twojej tożsamości. Jest to pochodzą z usługi Azure AD, gdy adres URL metadanych został rozwiązany.
 
8.   Certyfikat X.509: Twoje IdP certyfikatu X.509. Jest to pochodzą z usługi Azure AD, gdy adres URL metadanych został rozwiązany.
 
9.   Nazwa przycisku logowania: Nazwa przycisku Zaloguj organizacja chce wyświetlać. Ten tekst jest pokazywana użytkownikom na przycisku Zaloguj na ekranie logowania.
 
10.   Lokalizacje identyfikator użytkownika SAML: Gdzie identyfikator użytkownika jest oczekiwany w odpowiedzi SAML. Albo można w NameID lub w nazwie atrybutu niestandardowego. Ten identyfikator ma być JIRA/zlewiska identyfikator użytkownika.
 
11.   Nazwa atrybutu: Nazwa atrybutu, których można oczekiwać identyfikator użytkownika.
 
12.   Włącz Home odnajdowania obszaru: Określ, czy ta flaga firmy przy użyciu logowania za pomocą usług AD FS.
 
13.   Nazwa domeny: Podaj nazwę domeny, w tym miejscu w przypadku logowania za pomocą usług AD FS
 
14.   Włączanie rejestracji jednokrotnej limit: Sprawdź tego pakietu office, jeśli chcesz wylogować się z usługi Azure AD, gdy użytkownik zaloguje z JIRA/zlewiska.

### <a name="troubleshooting"></a>Rozwiązywanie problemów

* W przypadku uzyskiwania wiele błędów certyfikatów
    
    * Zaloguj się do usługi Azure AD i usunąć wielu certyfikatów dostępnych dla aplikacji. Upewnij się, że istnieje tylko jeden certyfikat jest obecny.

* Certyfikat jest zbliża się data wygaśnięcia w usłudze Azure AD.
    
    * Dodatki zajmie się automatycznego przerzucania certyfikatu. Gdy certyfikat jest wygasnąć, nowego świadectwa powinna być oznaczona jako aktywny i nieużywanych certyfikat powinien zostać usunięty. Gdy użytkownik próbuje zalogować się do JIRA w tym scenariuszu, dodatek pobiera nowy certyfikat i Zapisz w dodatku.

* Jak wyłączyć WebSudo (Wyłącz sesji bezpiecznego administratora)
    
    * JIRA: Zabezpieczanie administratora, który sesji (to znaczy potwierdzenie hasła przed uzyskaniem dostępu do funkcji administracyjnych) są domyślnie włączone. Jeśli chcesz wyłączyć tę opcję w wystąpieniu JIRA, można wyłączyć tę funkcję, określając następujący wiersz w pliku jira config.properties: "ira.websudo.is.disabled = true"
    
    * Zlewiska: Postępuj zgodnie z krokami opisanymi w następujący adres URL https://confluence.atlassian.com/doc/configuring-secure-administrator-sessions-218269595.html

* Pola, które mają zostać wypełnione przez adres URL metadanych nie są pobierania wypełnione.
    
    * Sprawdź, czy adres URL jest poprawny. Sprawdź, czy zamapowaniu poprawne dzierżawy i identyfikator aplikacji.
    
    * Trafień na adres URL z przeglądarki i zobacz, czy otrzymujesz XML metadanych federacji.

* Wewnętrzny błąd serwera:
    
    * Przejdź do dzienników w katalogu dzienników instalacji. Gdy użytkownik próbuje zalogować się przy użyciu usługi Azure AD z logowania jednokrotnego w przypadku uzyskiwania błąd, dzienniki mogą udostępniać informacje pomocy technicznej przedstawione poniżej w tym dokumencie.

* Identyfikator użytkownika nie można odnaleźć błąd, gdy użytkownik próbuje zalogować
    
    * Użytkownik nie jest tworzony w JIRA/zlewiska, aby utworzyć takie same.

* Błąd: nie odnaleziono w usłudze Azure AD aplikacji
    
    * Zobacz, jeśli odpowiedni adres URL jest mapowany do aplikacji w usłudze Azure AD.

* Szczegóły dotyczące obsługi: dotrzeć do nas na: [zespołu integracji usługi Azure AD SSO](<mailto:SaaSApplicationIntegrations@service.microsoft.com>). Odpowiemy w obrębie 24 do 48 godzin.
    
    * Może też wiązać biletu pomocy technicznej w firmie Microsoft za pośrednictwem kanału portalu Azure.