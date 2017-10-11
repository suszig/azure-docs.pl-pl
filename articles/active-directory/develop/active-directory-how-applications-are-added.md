---
title: "Jak aplikacje są dodawane do usługi Azure Active Directory."
description: "W tym artykule opisano, jak aplikacje są dodawane do wystąpienia usługi Azure Active Directory."
services: active-directory
documentationcenter: 
author: shoatman
manager: kbrint
editor: 
ms.assetid: 3321d130-f2a8-4e38-b35e-0959693f3576
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/09/2016
ms.author: shoatman
ms.custom: aaddev
ms.openlocfilehash: 6ffcfcb7ed071a12b0b3495ad534fd00f6d6ad99
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="how-and-why-applications-are-added-to-azure-ad"></a>Metody i przyczyny dodawania aplikacji do usługi Azure AD
Z początkowo puzzling czynności podczas przeglądania listy aplikacji w wystąpieniu usługi Azure Active Directory jest zrozumienie, skąd pochodzą te aplikacje i dlaczego są one dostępne.  W tym artykule zapewni wysokiego poziomu omówienie sposobu znajdują się w katalogu aplikacji i umożliwiają z kontekstem, który pomoże zrozumieć, jak pochodzi aplikacja w katalogu.

## <a name="what-services-does-azure-ad-provide-to-applications"></a>Jakie usługi Azure AD są dostępne w aplikacji?
Aplikacje są dodawane do usługi Azure AD wykorzystać co najmniej jeden z nich zapewnia.  Te usługi obejmują:

* Aplikacja uwierzytelniania i autoryzacji
* Uwierzytelnianie użytkownika & autoryzacji
* Logowanie jednokrotne (SSO) przy użyciu Federacji lub haseł
* Inicjowanie obsługi użytkowników & synchronizacji
* Kontrola dostępu oparta na rolach; Użyj katalogu do definiowania ról aplikacji do pełnienia ról na podstawie sprawdzeń autoryzacji w aplikacji.
* usługi autoryzacji oAuth (używane przez usługi Office 365 i innych aplikacji firmy Microsoft do autoryzowania dostępu do interfejsów API/zasobów.)
* Publikowanie aplikacji & proxy; Publikowanie aplikacji w sieci prywatnej do Internetu

## <a name="how-are-applications-represented-in-the-directory"></a>Jak aplikacje są reprezentowane w katalogu?
Aplikacje są reprezentowane w usłudze Azure AD przy użyciu obiektów 2: obiekt główny usługi i obiektu aplikacji.  Istnieje jeden obiekt aplikacji, w zarejestrowany w "home" / directory "właściciela" lub "publikowania" i przynajmniej jedna obiekty główne reprezentujący aplikacji w każdym katalogu, w którym działa usługa.  

Obiekt aplikacji zawiera opis aplikacji do usługi Azure AD (usługa wielodostępnej) i może zawierać żadnego z następujących czynności: (*Uwaga*: to nie jest kompletną listą.)

* Nazwa, Logo i wydawca
* Klucze tajne (symetrycznego i/lub asymetrycznego klucze służące do uwierzytelniania aplikacji)
* Zależności interfejsu API (oAuth)
* Interfejsy API/zasobów/zakresów opublikowane (oAuth)
* Aplikacja ról (RBAC)
* Metadane logowania jednokrotnego i konfiguracji (rejestracji jednokrotnej SSO)
* Inicjowanie obsługi administracyjnej metadanych i konfiguracji użytkownika
* Metadane serwera proxy i konfiguracji

Nazwy głównej usługi jest rekordem aplikacji w każdym katalogu, gdzie aplikacja działa, łącznie z jego katalogu macierzystego.  Nazwy głównej usługi:

* Odwołuje się do obiektu aplikacji za pomocą właściwości identyfikatora aplikacji
* Rejestruje lokalnych użytkowników i grupy przypisań ról aplikacji
* Rejestruje uprawnienia użytkownika i administratora lokalnego przyznanych do aplikacji
  * Na przykład: uprawnień dla aplikacji dostęp do poczty e-mail użytkowników
* Rejestruje zasady lokalne, łącznie z zasad dostępu warunkowego
* Rejestruje lokalne alternatywne ustawienia lokalne dla aplikacji
  * Reguły przekształcania oświadczeń
  * Mapowanie atrybutów (Inicjowanie obsługi użytkowników)
  * Dzierżawcy ról określonej aplikacji (Jeśli aplikacja obsługuje role niestandardowe)
  * Nazwa/Logo

### <a name="a-diagram-of-application-objects-and-service-principals-across-directories"></a>Diagram obiektów aplikacji i nazwy główne usług między katalogami
![Diagram pokazujący sposób aplikacji obiekty i usługi podmiotów istniejących wystąpieniami usługi Azure AD.][apps_service_principals_directory]

Jak to pokazano na powyższym diagramie.  Firma Microsoft udostępnia dwa katalogi wewnętrznie (po lewej stronie) używany do publikowania aplikacji.

* Jeden dla Microsoft Apps (katalog usług firmy Microsoft)
* Jeden dla wstępnie zintegrowanych aplikacji firm 3 (Galeria aplikacji katalogu)

Aplikacja wydawców/dostawców, którzy integracji z usługą Azure AD muszą mieć katalogu publikowania.  (Niektóre katalogu SAAS).

Aplikacje, które Dodaj siebie obejmują:

* Aplikacje, które utworzono (zintegrowane w usłudze AAD)
* Aplikacje połączone dla single-sign-on
* Aplikacje, które można publikować przy użyciu serwera proxy aplikacji usługi Azure AD.

### <a name="a-couple-of-notes-and-exceptions"></a>Kilka uwag i wyjątków
* Nie wszystkie jednostki usługi wskazywać z powrotem obiektów aplikacji.  Huh? Gdy usługi Azure AD został pierwotnie utworzony usług do aplikacji były bardziej ograniczone i nazwy głównej usługi został wystarczające do ustalenia tożsamości aplikacji.  Oryginalnej nazwy głównej usługi jest w kształcie bliżej konta usługi Windows Server Active Directory.  Z tego powodu przez nadal można utworzyć nazwy główne usług przy użyciu programu Azure AD PowerShell bez tworzenia obiektu aplikacji.  Interfejsu API programu Graph wymaga obiektu aplikacji przed utworzeniem usługę podmiotu zabezpieczeń.
* Nie wszystkie informacje opisane powyżej obecnie uwidocznioną programowo.  Poniżej są dostępne tylko w interfejsie użytkownika:
  * Reguły przekształcania oświadczeń
  * Mapowanie atrybutów (Inicjowanie obsługi użytkowników)
* Aby uzyskać więcej szczegółowych informacji dotyczących nazwy głównej usługi i zapoznaj się z interfejsu API REST usługi Azure AD Graph dokumentacji referencyjnej obiektów aplikacji.  *Wskazówka*: Dokumentacja interfejsu API Azure AD Graph najbliższym elementem na odwołanie do schematu jest dla usługi Azure AD, która jest obecnie dostępna.  
  * [Aplikacji](https://msdn.microsoft.com/library/azure/dn151677.aspx)
  * [Nazwy głównej usługi](https://msdn.microsoft.com/library/azure/dn194452.aspx)

## <a name="how-are-apps-added-to-my-azure-ad-instance"></a>Jak aplikacje są dodawane do mojego wystąpienia usługi Azure AD?
Istnieje wiele sposobów, aplikacji mogą zostać dodane do usługi Azure AD:

* Dodaj aplikację z [galerii aplikacji programu Azure Active Directory](https://azure.microsoft.com/updates/azure-active-directory-over-1000-apps/)
* Zaloguj się w górę/w 3rd aplikacji firm zintegrowane z usługą Azure Active Directory (na przykład: [Smartsheet](https://app.smartsheet.com/b/home) lub [DocuSign](https://www.docusign.net/member/MemberLogin.aspx))
  * Podczas logowania się/użytkownicy są proszeni o nadaj uprawnienia dostępu swój profil do aplikacji i innych uprawnień.  Pierwszą osobą zgody powoduje, że nazwy głównej usługi reprezentujący aplikacji do dodania do katalogu.
* Zaloguj się w górę/w witrynie Microsoft online services, takich jak [usługi Office 365](http://products.office.com/)
  * Po zasubskrybowaniu usługi Office 365 lub rozpocząć wersji próbnej, nazwy główne usług są tworzone w katalogu reprezentujący różnych usług, które są używane do dostarczania wszystkie funkcje związane z usługą Office 365.
  * Niektóre usługi Office 365, takich jak SharePoint Utwórz jednostki usługi na podstawie w toku, aby umożliwić bezpiecznej komunikacji między składnikami, włącznie z przepływami pracy.
* Dodaje projektujesz aplikację w portalu zarządzania Azure patrz: https://msdn.microsoft.com/library/azure/dn132599.aspx
* Dodaj aplikację, której tworzony jest przy użyciu programu Visual Studio, zobacz:
  * [Metody uwierzytelniania platformy ASP.Net](http://www.asp.net/visual-studio/overview/2013/creating-web-projects-in-visual-studio#orgauthoptions)
  * [Podłączonych usług](http://blogs.msdn.com/b/visualstudio/archive/2014/11/19/connecting-to-cloud-services.aspx)
* Dodawanie aplikacji do użycia przy użyciu [serwera Proxy aplikacji usługi Azure AD](https://msdn.microsoft.com/library/azure/dn768219.aspx)
* Łączenie aplikacji dla funkcji logowania jednokrotnego przy użyciu SAML lub hasło logowania jednokrotnego
* Wiele innych, łącznie z różnych uruchomień dewelopera na platformie Azure i/w Eksploratorze interfejsu API napotyka między centrami developer

## <a name="who-has-permission-to-add-applications-to-my-azure-ad-instance"></a>Kto ma uprawnienia do dodawania aplikacji do mojego wystąpienia usługi Azure AD?
Tylko administratorzy globalni mogą:

* Dodaj aplikacje z galerii aplikacji Azure AD (wstępnie zintegrowanych aplikacji firm 3)
* Publikowanie aplikacji przy użyciu serwera Proxy aplikacji usługi AD platformy Azure

Wszyscy użytkownicy w katalogu mają prawa do dodawania aplikacji, które są one opracowywanie i uznania za pośrednictwem aplikacji, które są udziału/udzielanie dostępu do swoich danych organizacji.  *Należy pamiętać o logowania użytkownika w górę/w aplikacji i udzielanie uprawnień może spowodować Trwa tworzenie nazwy głównej usługi.*

To może początkowo dźwięku dotyczące, ale zachowa na uwadze następujące:

* Aplikacje są w stanie korzystać z usługi Active Directory systemu Windows Server do uwierzytelniania użytkowników przez wiele lat bez konieczności aplikacji zarejestrowany/zapisywane w katalogu.  Teraz organizacji zostanie zwiększona wgląd w dokładnie ile aplikacje korzystają z katalogu i what for.
* Administrator zmiennych proces publikowania/rejestracji aplikacji nie jest konieczne.  Z usług federacyjnych Active Directory została prawdopodobnie administrator musiał dodać aplikację jako uzależniona imieniu deweloperów.  Deweloperzy mogą teraz samoobsługi.
* Użytkownicy logujący się do aplikacji przy użyciu konta organizacji do celów biznesowych/jest to przydatne.  Następnie opuszczenia organizacji spowoduje utratę dostępu do swojego konta w aplikacji, który używa.
* O rekord danych został udostępniony, z którym aplikacja jest to przydatne.  Dane są bardziej przenośne niż kiedykolwiek i o wyczyść rekord kto udostępnionych danych z aplikacji, które jest używany.
* Aplikacje, które używają usługi Azure AD do uwierzytelniania oAuth określić dokładnie jakie uprawnienia, które użytkownicy będą mogli udzielić aplikacji i które uprawnienia wymagają administratora, aby zaakceptować.  Powinien być kierowany bez informujący o tym, że tylko administratorzy mogą wyrazić zgodę na zakresy większych i bardziej znaczących uprawnienia.
* Użytkowników, dodawanie, dzięki czemu aplikacje dostępu do danych są poddawane inspekcji zdarzeń, aby wyświetlić raporty inspekcji w portalu zarządzania Azure, aby określić, jak aplikacja została dodana do katalogu.

**Uwaga:** *Microsoft sam działał używa konfiguracji domyślnej teraz wiele miesięcy.*

Biorąc pod uwagę, że dana jest możliwe uniemożliwić dodawanie aplikacji przez użytkowników w katalogu i wykonywać uznania za pośrednictwem jakie informacje są współużytkowane z aplikacji przez zmodyfikowanie konfiguracji katalogu w portalu zarządzania Azure.  Następująca konfiguracja są dostępne w portalu zarządzania Azure karty "Konfiguruj" w katalogu.

![Zrzut ekranu interfejsu użytkownika do konfigurowania ustawień zintegrowanych aplikacji][app_settings]

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Następne kroki
Dowiedz się więcej na temat sposobu dodawania aplikacji do usługi Azure AD i konfigurowanie usług dla aplikacji.

* Deweloperzy: [informacje o sposobie integracji aplikacji przy użyciu usługi AAD](https://msdn.microsoft.com/library/azure/dn151122.aspx)
* Deweloperzy: [Przejrzyj przykładowy kod dla aplikacji, zintegrowany z usługą Azure Active Directory w witrynie GitHub](https://github.com/AzureADSamples)
* Deweloperów i specjalistów IT: [zapoznaj się z dokumentacją interfejsu API REST dla usługi Azure Active Directory interfejsu API programu Graph](https://msdn.microsoft.com/library/azure/hh974478.aspx)
* Informatycy: [Dowiedz się, jak używać usługi Azure Active Directory wstępnie zintegrowanych aplikacji w galerii aplikacji](https://msdn.microsoft.com/library/azure/dn308590.aspx)
* Informatycy: [samouczki dotyczące konfigurowania określonych wstępnie zintegrowanych aplikacji](https://msdn.microsoft.com/library/azure/dn893637.aspx)
* Informatycy: [Dowiedz się, jak opublikować aplikację przy użyciu usługi Azure Active Directory serwera Proxy aplikacji](https://msdn.microsoft.com/library/azure/dn768219.aspx)

## <a name="see-also"></a>Zobacz też
* [Indeks artykułów dotyczących zarządzania aplikacjami w usłudze Azure Active Directory](../active-directory-apps-index.md)

<!--Image references-->
[apps_service_principals_directory]:../media/active-directory-how-applications-are-added/HowAppsAreAddedToAAD.jpg
[app_settings]:../media/active-directory-how-applications-are-added/IntegratedAppSettings.jpg
