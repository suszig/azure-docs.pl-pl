---
title: "Migrowanie z usługi kontroli dostępu na platformie Azure (ACS)"
description: "Opcje przenoszenia aplikacje i usługi wylogowuje usłudze kontroli dostępu platformy Azure | Microsoft Azure"
services: active-directory
documentationcenter: dev-center-name
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/14/2017
ms.author: dastrock
ms.openlocfilehash: e32cac7feda929a63c4a80fc0078b221117eb2b5
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/18/2017
---
# <a name="migrating-from-azure-access-control-service-acs"></a>Migrowanie z usługi kontroli dostępu na platformie Azure (ACS)

Microsoft Azure Active Directory kontroli dostępu (znanej także jako usługa kontroli dostępu lub ACS) została wycofana w listopadzie 2018.  Aplikacje i usługi aktualnie używa ACS konieczne pełni migrację do mechanizm uwierzytelniania inny przed tą datą. Ten dokument zawiera zalecenia dotyczące bieżącego klientów jako planują zastąpić ich używania usług ACS. Jeśli nie używasz obecnie ACS, nie trzeba podejmować żadnych działań.


## <a name="brief-acs-overview"></a>Krótki przegląd usług ACS

ACS jest usługi uwierzytelniania w chmurze zapewnia łatwy sposób uwierzytelniania i autoryzacji użytkowników uzyskujących dostęp do aplikacji sieci web i usług, zezwalając wiele funkcji uwierzytelniania i autoryzacji, aby uwzględnić poza swój kod. Jest używany głównie przez deweloperów i architektów klientów platformy .NET, aplikacji sieci web ASP.NET i usługi sieci web WCF.

Przypadki użycia ACS mogą być podzielone na trzy główne kategorie:

- Uwierzytelniania niektórych usług chmurowych firmy Microsoft, w tym usługi Azure Service Bus, Dynamics CRM i inne. Aplikacje klienckie może uzyskać tokenów z usługi kontroli dostępu, który może służyć do uwierzytelniania tych usługach można wykonywać różne akcje.
- Dodawanie uwierzytelniania do aplikacji sieci web, w różnych niestandardowych & opakowanych odmiany (np. witrynę Sharepoint). Przy użyciu uwierzytelniania "pasywny" ACS, aplikacji sieci web mogą obsługiwać Zaloguj się za pomocą konta z Google, Facebook, Yahoo, Microsoft konta (Live ID), usługi Azure Active Directory i usług AD FS.
- Zabezpieczanie usług sieci web na wbudowanych niestandardowych z tokenów wystawionych przez usługi ACS. Przy użyciu uwierzytelniania "active", usług sieci web można upewnij się, że ich tylko zezwalają na dostęp z znane aplikacje klienckie, które uwierzytelniane w usługach ACS.

Wszystkie te przypadki użycia i ich strategii migracji zalecane została szczegółowo opisana w poniższych sekcjach. W większość przypadków istotne zmiany kodu są wymagane do migracji istniejącej aplikacji i usług do nowszej technologii. Zalecane jest rozpoczęcie planowania i wykonywania migracji natychmiast, aby uniknąć potencjalnych awarii lub przestoju.

> [!WARNING]
> W większości przypadków istotne zmiany kodu są wymagane do migracji istniejącej aplikacji i usług do poziomu nowych technologii. Zalecane jest rozpoczęcie planowania i wykonywania migracji natychmiast, aby uniknąć potencjalnych awarii lub przestoju.

Pod względem architektury ACS całkowicie składa się z następujących składników:

- Bezpieczne usługi tokenu (STS), który odbiera żądania uwierzytelniania i w zamian wystawia tokeny zabezpieczające.
- Klasycznym portalu Azure, który służy do tworzenia, usuwania i włączanie i wyłączanie usług ACS przestrzeni nazw.
- Oddzielne ACS portalu zarządzania, który służy do dostosowywania i konfigurowanie zachowania do przestrzeni nazw usługi ACS.
- Usługa zarządzania, która może służyć do automatyzowania funkcje portali.
- Token reguły aparat przekształcania, która może służyć do definiowania złożonej logiki do manipulowania format danych wyjściowych z tokenów wystawionych przez usługi ACS.

Aby korzystać z tych składników, należy utworzyć co najmniej jeden ACS **przestrzeni nazw**. Przestrzeń nazw jest dedykowane wystąpienie usługi ACS, które usługi i aplikacje komunikować się z; jest odizolowana od wszystkich innych usług ACS klientom, którzy używać własnej przestrzeni nazw. Przestrzeń nazw usługi ACS ma dedykowany adres URL, tak samo, jak:

```HTTP
https://mynamespace.accesscontrol.windows.net
```

Cała komunikacja z STS i operacji zarządzania są wykonywane na ten adres URL z innej ścieżki do różnych celów. Aby ustalić, jeśli aplikacje lub usługi, użyj ACS, monitorować cały ruch do `https://{namespace}.accesscontrol.windows.net`.  Cały ruch do tego adresu URL jest ruchu, który jest obsługiwany przez usługi ACS i musi być wstrzymana.  Jedynym wyjątkiem jest cały ruch do `https://accounts.accesscontrol.windows.net` -ruchu na ten adres URL jest już obsługiwany przez inną usługę i nie ma wpływu na amortyzacja ACS.  Należy również koniecznie zalogować się do klasycznego portalu Azure i sprawdź, czy wszystkie przestrzenie nazw ACS w ramach subskrypcji, w których jesteś właścicielem.  Przestrzenie nazw ACS są wymienione w **usługi Active Directory** usługi, w obszarze **specyfikatorów dostępu** kartę.

Aby uzyskać więcej informacji dotyczących usług ACS, zobacz [to zarchiwizowane dokumentacji w witrynie MSDN](https://msdn.microsoft.com/library/hh147631.aspx).

## <a name="retirement-schedule"></a>Wycofanie harmonogramu

Począwszy od 2017 listopada wszystkich składników usług ACS są pełni obsługiwane & operacyjną. Jedynym ograniczeniem jest to, że [nie można utworzyć nowych usług ACS przestrzeni nazw w klasycznym portalu Azure](https://azure.microsoft.com/blog/acs-access-control-service-namespace-creation-restriction/).

Oś czasu przestarzałą tych składników tego harmonogramu są następujące:

- **2017 listopada**: środowisko administratora usługi Azure AD w klasycznym portalu Azure [jest wycofywany](https://blogs.technet.microsoft.com/enterprisemobility/2017/09/18/marching-into-the-future-of-the-azure-ad-admin-experience-retiring-the-azure-classic-portal/). W tym momencie zarządzania przestrzeni nazw ACS będzie dostępna w tym nowe, dedykowany adres URL: `http://manage.windowsazure.com?restoreClassic=true`. To pozwala na wyświetlanie istniejących obszary nazw, włączanie/wyłączanie je i usunąć je, jeśli chcesz.
- **2018 kwietnia**: ACS przestrzeni nazw zarządzania jest nie są dostępne pod tym adresem URL w dedykowanym. W tym momencie nie jest możliwe Włącz/Wyłącz, usunąć lub wyliczenia obszary nazw usług ACS. Portal zarządzania ACS, jednak jest w pełni funkcjonalny i znajduje się w lokalizacji `https://{namespace}.accesscontrol.windows.net`. Nadal działa normalnie, a także wszystkich składników usług ACS.
- **2018 listopada**: ACS wszystkie składniki są trwale zamknąć. W tym portalu zarządzania usługi ACS, usługę zarządzania, STS i aparat reguł przekształcania tokenu. W tym momencie wszelkie żądania wysyłane do usługi ACS (znajdujący się w `{namespace}.accesscontrol.windows.net`) się nie powieść. Użytkownik powinien zostały zmigrowane wszystkie istniejące aplikacje i usługi do innych technologii przed tym okresie.


## <a name="migration-strategies"></a>Strategii migracji

W poniższych sekcjach opisano wysokiego poziomu zalecenia dotyczące migracji wylogowuje ACS do innych technologii firmy Microsoft.

### <a name="clients-of-microsoft-cloud-services"></a>Klienci usług chmurowych firmy Microsoft

Każdy z usług chmurowych firmy Microsoft, które akceptują tokeny wystawione przez usługi ACS teraz obsługuje co najmniej jeden z alternatywnych formy uwierzytelniania. Mechanizm uwierzytelniania poprawne różni się dla każdej usługi, tak więc zaleca się odwołujących się do szczegółowej dokumentacji w każdej z oficjalnego wskazówki dotyczące usług. Dla wygody każdego zestawu dokumentacji podano tutaj:

| Usługa | Wskazówki |
| ------- | -------- |
| Azure Service Bus | [Migrowanie do sygnatur dostępu współdzielonego](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-migrate-acs-sas) |
| Azure Relay | [Migrowanie do sygnatur dostępu współdzielonego](https://docs.microsoft.com/azure/service-bus-relay/relay-migrate-acs-sas) |
| Usługa Pamięć podręczna systemu Azure | [Migracja do usługi Azure Redis Cache](https://docs.microsoft.com/azure/redis-cache/cache-faq#which-azure-cache-offering-is-right-for-me) |
| Rynku danych Azure | [Migrowanie do interfejsów API usługi kognitywnych](https://docs.microsoft.com/azure/machine-learning/studio/datamarket-deprecation) |
| BizTalk Services | [Migracja do usługi Azure Logic Apps](https://docs.microsoft.com/azure/machine-learning/studio/datamarket-deprecation) |
| Usługi Azure Media Services | [Migrowanie do uwierzytelniania usługi Azure AD](https://azure.microsoft.com/blog/azure-media-service-aad-auth-and-acs-deprecation/) |
| Azure Backup | [Uaktualnienie agenta kopii zapasowej systemu Azure](https://docs.microsoft.com/azure/backup/backup-azure-file-folder-backup-faq) |

<!-- Dynamics CRM: Migrate to new SDK, Dynamics team handling privately -->
<!-- Azure RemoteApp deprecated in favor of Citrix: http://www.zdnet.com/article/microsoft-to-drop-azure-remoteapp-in-favor-of-citrix-remoting-technologies/ -->
<!-- Exchange push notifications are moving, customers don't need to move -->
<!-- Retail federation services are moving, customers don't need to move -->
<!-- Azure StorSimple: TODO -->
<!-- Azure SiteRecovery: TODO -->


### <a name="web-applications-using-passive-authentication"></a>Aplikacje sieci Web przy użyciu uwierzytelnianiem pasywnym

Dla aplikacji sieci web przy użyciu usług ACS do uwierzytelniania użytkowników ACS dostarczone następujące funkcje i możliwości do deweloperów & architektów aplikacji sieci web:

- Ścisła integracja z systemem Windows Identity Foundation (WIF).
- Federacja z Google, Facebook, Yahoo, Microsoft konta (Live ID), usługi Azure Active Directory i usług AD FS.
- Obsługa następujące protokoły uwierzytelniania: projekt OAuth 2.0 13, Ws-Trust i Ws-Federation.
- Obsługa następujące formaty tokenów: tokenu web JSON (JWT), SAML 1.1 SAML 2.0 i prosty sieci web token (SWT).
- Środowisko odnajdowania obszaru głównego zintegrowane WIF, dozwolony użytkowników i wybierz typ tworzonego konta używanego do logowania. To środowisko hostowanej przez aplikację sieci web i można swobodnie dostosowywać.
- Token transformacji dozwolone zaawansowane dostosowywanie oświadczeń odebranych przez aplikację sieci web z usługi kontroli dostępu, w tym:
    - przekazywanie za pośrednictwem oświadczeń od dostawców tożsamości
    - Dodawanie dodatkowych oświadczenia niestandardowe
    - proste logiki następnie Jeśli do wystawiania oświadczeń w niektórych warunkach

Niestety nie ma nie jednej usługi, która udostępnia wszystkie te funkcje równoważne.  Należy ocenić, jakie funkcje ACS można potrzebne, a następnie wybierz między [ **usługi Azure Active Directory** ](https://azure.microsoft.com/develop/identity/signin/) (Azure AD), [ **usługi Azure AD B2C** ](https://azure.microsoft.com/services/active-directory-b2c/), lub innej usługi uwierzytelniania w chmurze.

#### <a name="migrating-to-azure-active-directory"></a>Migracja do usługi Azure Active Directory

Jedną ścieżkę wziąć pod uwagę jest integracja usługi i aplikacje bezpośrednio w usłudze Azure Active Directory. Usługi Azure AD jest dostawca oparte na tożsamości w chmurze firmy Microsoft pracy & szkolnego konta — jest dostawca tożsamości usługi Office 365, Azure i o wiele więcej. Udostępnia podobne federacyjnych możliwości uwierzytelniania usługi ACS, ale nie obsługuje wszystkich funkcji usługi ACS. Podstawowy przykład jest federacji z dostawców tożsamości społecznościowych, takich jak Facebook, Google i Yahoo. Jeśli użytkownicy Zaloguj się przy użyciu tych typów poświadczeń, usługi Azure AD nie jest dla Ciebie. On również nie niekoniecznie Obsługa protokołów dokładnie tego samego uwierzytelniania jako ACS — podczas obu ACS i pomocy technicznej usługi Azure AD OAuth, na przykład są niewielkie różnice między każdym implementację, która wymaga modyfikowania kodu w ramach migracji.

Usługi Azure AD zapewnia jednak wiele potencjalnych korzyści klientom usług ACS. Natywnie obsługuje Microsoft pracy & służbowych hostowanych w chmurze, które są często używane przez klientów usług ACS.  Dzierżawa usługi Azure AD można również federacyjną do jednego lub więcej wystąpień z lokalnej usługi Active Directory za pomocą usług AD FS, co pozwala aplikacji na potrzeby uwierzytelniania zarówno użytkowników w chmurze, jak i hostowanych na lokalnych użytkowników.  Obsługuje ona również protokołu Ws-Federation, co stanowi stosunkowo prosta do integracji z aplikacji sieci web przy użyciu systemu Windows Identity Foundation (WIF).

W poniższej tabeli porównano funkcje ACS odpowiednie do aplikacji sieci web z dostępnych w usłudze Azure AD. Na wysokim poziomie **usługi Azure Active Directory jest prawdopodobnie Wybór właściwej do migracji, jeśli można tylko zezwala użytkownikom na logowanie z siecią firmową Microsoft & służbowych**.

| Możliwości | Obsługa usług ACS | Pomoc techniczna platformy Azure AD |
| ---------- | ----------- | ---------------- |
| **Typy kont** | | |
| Praca & szkolnego konta Microsoft | Obsługiwane | Obsługiwane |
| Konta z usługi Active Directory systemu Windows Server i usług AD FS | Obsługiwane za pośrednictwem federacji z dzierżawy usługi Azure AD <br /> Obsługiwane za pośrednictwem bezpośredniego federacji z usług AD FS | Obsługiwane wyłącznie za pośrednictwem federacji z dzierżawy usługi Azure AD | 
| Konta z innymi systemami zarządzania tożsamościami enterprise | Możliwe za pośrednictwem federacji z dzierżawy usługi Azure AD <br />Obsługiwane za pośrednictwem bezpośredniego federacyjnego | Możliwe za pośrednictwem federacji z dzierżawy usługi Azure AD |
| Konta Microsoft do użytku osobistego (Windows Live ID) | Obsługiwane | Obsługiwane za pośrednictwem usługi Azure AD w wersji 2.0 protokołu OAuth, ale nie za pośrednictwem innych protokołów. | 
| Facebook, Google i Yahoo kont | Obsługiwane | Nieobsługiwane jakikolwiek sposób. |
| **Protokoły i zgodności zestawu SDK** | | |
| Program Windows Identity Foundation (WIF) | Obsługiwane | Obsługiwane, ale ograniczone instrukcje dostępne. |
| WS-Federation | Obsługiwane | Obsługiwane |
| Uwierzytelnianie OAuth 2.0 | Obsługa wersji roboczej 13 | Obsługa RFC 6749, większość nowoczesnych specyfikacji. |
| WS-Trust | Obsługiwane | Nieobsługiwane |
| **Formaty tokenów** | | |
| (Jwt) tokenów sieci Web JSON | Obsługiwane w wersji Beta | Obsługiwane |
| Tokeny SAML 1.1 | Obsługiwane | Obsługiwane |
| Tokeny SAML 2.0 | Obsługiwane | Obsługiwane |
| Tokeny proste sieci Web (SWT) | Obsługiwane | Nieobsługiwane |
| **Dostosowania** | | |
| Można dostosować obszaru macierzystego/konto odnajdywania pobrania interfejsu użytkownika | Kod do pobrania, który można włączyć do aplikacji | Nieobsługiwane |
| Przekazywanie niestandardowych certyfikatów podpisywania tokenu | Obsługiwane | Obsługiwane |
| Dostosowywanie oświadczenia w tokenach | Przekazywanie danych wejściowych do oświadczeń od dostawców tożsamości<br />Uzyskaj token dostępu z dostawcy tożsamości jako oświadczenia<br />Wystawiać oświadczenia dane wyjściowe na podstawie wartości oświadczenia wejściowe<br />Wystawiać oświadczenia dane wyjściowe z wartości stałych | Nie można przekazywanie do oświadczeń od dostawców tożsamości federacyjnych<br />Nie można pobrać token dostępu z dostawcy tożsamości jako oświadczenia<br />Nie mogą wystawiać oświadczenia dane wyjściowe na podstawie wartości oświadczenia wejściowe<br />Mogą wystawiać oświadczenia dane wyjściowe z wartości stałych<br />Mogą wystawiać oświadczenia dane wyjściowe na podstawie właściwości użytkowników zsynchronizowane z usługą Azure AD |
| **Automatyzacja** | | |
| Automatyzowanie zadań konfiguracji i zarządzania nimi. | Obsługiwane za pośrednictwem usługi ACS zarządzania | Obsługiwane za pośrednictwem programu Microsoft Graph i Azure AD Graph API. |

Jeśli zdecydujesz się, że usługi Azure AD jest odpowiednią ścieżką do przodu dla usługi i aplikacje, należy zwrócić uwagę na dwa sposoby integracja aplikacji z usługą Azure AD.

Aby używać protokołu Ws-Federation/WIF do integracji z usługą Azure AD, zalecamy następujące [podejścia opisanego w tym artykule](https://docs.microsoft.com/azure/active-directory/application-config-sso-how-to-configure-federated-sso-non-gallery). Artykuł odnosi się do konfigurowania usługi Azure AD na podstawie SAML logowania jednokrotnego, ale działa w przypadku konfigurowania protokołu Ws-Federation oraz. Po tej metody wymaga licencji usługi Azure AD Premium, ale ma dwie zalety:

- Możesz pobrać pełną elastyczność dostosowywania tokenu usługi Azure AD. Dzięki temu można dostosować oświadczenia wydane przez usługę Azure AD są zgodne z tymi wystawione przez usługi ACS, szczególnie w tym oświadczenia Identyfikatora lub identyfikator nazwy użytkownika. Należy upewnić się, że wystawiony przez dopasowanie usługi Azure AD te wystawione przez usługi ACS, aby nadal odbierać spójne identyfikatory użytkowników nawet po zmianie technologii identyfikatory użytkowników.
- Można skonfigurować token podpisywania certyfikatu specyficzne dla aplikacji, którego okres istnienia kontrolujesz.

<!--

Possible nameIdentifiers from ACS (via AAD or ADFS):
- ADFS - Whatever ADFS is configured to send (email, UPN, employeeID, what have you)
- Default from AAD using App Registrations, or Custom Apps before ClaimsIssuance policy: subject/persistent ID
- Default from AAD using Custom apps nowadays: UPN
- Kusto can't tell us distribution, it's redacted

-->

> [!NOTE]
> Pozostawienie tej metody wymaga licencji usługi Azure AD Premium. Jeśli klienci usług ACS, wymagana jest licencja premium konfigurowania jednokrotnego dla aplikacji dotrzeć do nas i chętnie udostępnianie licencji dewelopera do użycia.

Informacje o innym podejściu jest wykonanie czynności opisanych [ten przykładowy kod](https://github.com/Azure-Samples/active-directory-dotnet-webapp-wsfederation), co daje nieco inne instrukcje na temat sposobu konfigurowania protokołu Ws-Federation. Ten przykładowy kod nie korzysta z wersji WIF, ale raczej oprogramowania pośredniczącego OWIN 4.5 programu ASP.NET. Jednak instrukcje dotyczące rejestracji aplikacji są prawidłowe dla aplikacji korzystających z programu WIF i nie wymagają licencji usługi Azure AD Premium.  Jeśli wybierzesz tego podejścia, heneed zrozumienie [podpisywania przerzucania kluczy w usłudze Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-signing-key-rollover). Ta metoda używa usługi Azure AD globalnego klucza do wydawania tokenów podpisywania. Domyślnie WIF nie odświeża automatycznie klucze podpisywania. Gdy usługi Azure AD obraca jego globalne klucze podpisywania, implementacji WIF trzeba przygotować aby zatwierdzić zmiany.

Jeśli można zintegrować z usługą Azure AD za pomocą protokoły OpenID Connect i OAuth, firma Microsoft zaleca w ten sposób.  Mamy szczegółową dokumentację i wskazówki dotyczące sposobu integracji usługi Azure AD w aplikacji sieci web dostępnych w naszym [przewodnik dewelopera usługi Azure AD](http://aka.ms/aaddev).

<!-- TODO: If customers ask about authZ, let's put a blurb on role claims here -->

#### <a name="migrating-to-azure-ad-b2c"></a>Migracja do usługi Azure AD B2C

Ścieżki migracji do rozważenia jest usługi Azure AD B2C.  B2C to uwierzytelniania w chmurze usługi, podobnie jak ACS, umożliwia deweloperom zewnętrzny ich logika zarządzania uwierzytelnianie i tożsamość usługi w chmurze.  Jest usługą płatną (z warstwami wolnego & premium) przeznaczone dla konsumentów skierowane w aplikacji, które może zawierać miliony aktywnych użytkowników.

Jak ACS, jedną z najbardziej atrakcyjnych funkcji B2C to obsługuje wiele różnych typów kont. Z B2C, użytkownik może logowania użytkowników z ich Facebook, Google, Microsoft, LinkedIn, GitHub, Yahoo kont itd. B2C obsługuje również "kontami lokalnymi", lub nazwy użytkownika i hasła, które użytkownicy będą tworzyć specjalnie dla aplikacji. Usługa Azure AD B2C są także zaawansowanych rozszerzeń, których można dostosowywać Twoje przepływy logowania. Nie, jednak obsługuje szerokość protokołów uwierzytelniania i tokenu formatów, które może wymagać ACS klientów. On również nie można uzyskać tokenów & zapytanie dodatkowe informacje o użytkowniku od dostawcy tożsamości, Microsoft lub w inny sposób.

W poniższej tabeli porównano funkcje ACS odpowiednie do aplikacji sieci web z tymi, które są dostępne w usłudze Azure AD B2C. Na wysokim poziomie **usługi Azure AD B2C jest prawdopodobnie warto wybrać do migracji, czy aplikacja jest ukierunkowane konsumenta, czy obsługuje wiele różnych typów kont.**

| Możliwości | Obsługa usług ACS | Obsługa usługi Azure AD B2C |
| ---------- | ----------- | ---------------- |
| **Typy kont** | | |
| Praca & szkolnego konta Microsoft | Obsługiwane | Obsługiwane za pośrednictwem zasad niestandardowych  |
| Konta z usługi Active Directory systemu Windows Server i usług AD FS | Obsługiwane za pośrednictwem bezpośredniego federacji z usług AD FS | Obsługiwane za pośrednictwem federacyjnego SAML za pomocą zasad niestandardowych |
| Konta z innymi systemami zarządzania tożsamościami enterprise | Obsługiwane za pośrednictwem bezpośredniego federacyjnych za pośrednictwem usługi Ws-Federation | Obsługiwane za pośrednictwem federacyjnego SAML za pomocą zasad niestandardowych |
| Konta Microsoft do użytku osobistego (Windows Live ID) | Obsługiwane | Obsługiwane | 
| Facebook, Google i Yahoo kont | Obsługiwane | Facebook & Google obsługiwane natywnie, Yahoo obsługiwane za pośrednictwem protokołu OpenID Connect federacyjnego za pomocą niestandardowych zasad |
| **Protokoły i zgodności zestawu SDK** | | |
| Program Windows Identity Foundation (WIF) | Obsługiwane | Nie jest obsługiwane. |
| WS-Federation | Obsługiwane | Nie jest obsługiwane. |
| Uwierzytelnianie OAuth 2.0 | Obsługa wersji roboczej 13 | Obsługa RFC 6749, większość nowoczesnych specyfikacji. |
| WS-Trust | Obsługiwane | Nie jest obsługiwane. |
| **Formaty tokenów** | | |
| (Jwt) tokenów sieci Web JSON | Obsługiwane w wersji Beta | Obsługiwane |
| Tokeny SAML 1.1 | Obsługiwane | Nieobsługiwane |
| Tokeny SAML 2.0 | Obsługiwane | Nieobsługiwane |
| Tokeny proste sieci Web (SWT) | Obsługiwane | Nieobsługiwane |
| **Dostosowania** | | |
| Można dostosować obszaru macierzystego/konto odnajdywania pobrania interfejsu użytkownika | Kod do pobrania, który można włączyć do aplikacji | Można swobodnie dostosowywać interfejsu użytkownika za pomocą niestandardowych CSS. |
| Przekazywanie niestandardowych certyfikatów podpisywania tokenu | Obsługiwane | Niestandardowe klucze podpisywania, certyfikaty nie obsługiwane za pomocą niestandardowych zasad. |
| Dostosowywanie oświadczenia w tokenach | Przekazywanie danych wejściowych do oświadczeń od dostawców tożsamości<br />Uzyskaj token dostępu z dostawcy tożsamości jako oświadczenia<br />Wystawiać oświadczenia dane wyjściowe na podstawie wartości oświadczenia wejściowe<br />Wystawiać oświadczenia dane wyjściowe z wartości stałych | Można przekazywanie oświadczeń od dostawców tożsamości. Zasady niestandardowe wymagane przez niektóre oświadczenia.<br />Nie można pobrać token dostępu z dostawcy tożsamości jako oświadczenia<br />Mogą wystawiać oświadczenia dane wyjściowe na podstawie wartości oświadczenia wejściowe za pomocą zasad niestandardowych<br />Mogą wystawiać oświadczenia dane wyjściowe z wartości stałych za pomocą zasad niestandardowych |
| **Automatyzacja** | | |
| Automatyzowanie zadań konfiguracji i zarządzania nimi. | Obsługiwane za pośrednictwem usługi ACS zarządzania | Tworzenie użytkowników, którzy mogą za pomocą interfejsu API Azure AD Graph. Nie można programowo utworzyć dzierżawcy usługi B2C, aplikacji lub zasad. |

Jeśli zdecydujesz się, że usługi Azure AD B2C jest odpowiednią ścieżką do przodu dla usługi i aplikacje, należy uruchomić z następującymi zasobami:

- [Dokumentacja usługi Azure AD B2C](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-overview)
- [Niestandardowe zasady usługi Azure AD B2C](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-overview-custom)
- [Usługa Azure AD B2C ceny](https://azure.microsoft.com/pricing/details/active-directory-b2c/)


#### <a name="other-migration-options"></a>Inne opcje migracji

Jeśli żadna usługi Azure AD, ani usługi Azure AD B2C spełnia wymagania dotyczące aplikacji sieci web, proszę dotrzeć do nas i możemy pomóc Ci zidentyfikować najlepsze ścieżki do przodu.

<!--

#### Migrating to Ping Identity or Auth0

In some cases, you may find that neither Azure AD nor Azure AD B2C is sufficient to replace ACS in your web applications without making major code changes.  Some common examples might include:

- web applications using WIF and/or WS-Federation for sign-in with social identity providers such as Google or Facebook.
- web applications performing direct federation to an enterprise IDP over the Ws-Federation protocol.
- web applications that require the access token issued by a social identity provider (such as Google or Facebook) as a claim in the tokens issued by ACS.
- web applications with complex token transformation rules that Azure AD or Azure AD B2C cannot reproduce.

In these cases, you may want to consider migrating your web application to another cloud authentication service. We recommend exploring the following options, as each provides capabilities similar to ACS:

- [Auth0](https://auth0.com/) has created [high-level migration guidance for customers of ACS](https://auth0.com/blog/windows-azure-acs-alternative-replacement/), and provides a feature-by-feature comparison of ACS vs. Auth0.
- Enterprise customers should consider [Ping Identity](https://www.pingidentity.com) as well. Reach out to us and we can connect you with a representative from Ping who is prepared to help identify potential solutions.

Our aim in working with Ping Identity & Auth0 is to ensure that all ACS customers have a path forward for their apps & services that minimizes the amount of work required to move off of ACS.

-->

<!--

## Sharepoint 2010, 2013, 2016

TODO: AAD only, use AAD SAML 1.1 tokens, when we bring it back online.
Other IDPs: use Auth0? https://auth0.com/docs/integrations/sharepoint.

-->

### <a name="web-services-using-active-authentication"></a>Usługi sieci Web przy użyciu uwierzytelniania w usłudze active

Zabezpieczone przy użyciu tokenów wystawionych przez usługi ACS usług sieci web usług ACS dostępne następujące funkcje i możliwości:

- Możliwość zarejestrowania co najmniej jeden **tożsamości usługi** w przestrzeni nazw usługi ACS, które mogą być używane do żądania tokenów.
- Obsługa protokołów służący do żądania tokenów przy użyciu następujących typów poświadczeń OAuth ZAWIJAJ & OAuth 2.0 projekt 13:
    - Proste hasło utworzone dla tożsamości usługi
    - A podpisany przy użyciu klucza symetrycznego lub X509 SWT certyfikatu
    - Token SAML przez dostawcę tożsamości zaufanych (zazwyczaj wystąpienia usług AD FS)
- Obsługa następujące formaty tokenów: tokenu web JSON (JWT), SAML 1.1 SAML 2.0 i prosty sieci web token (SWT).
- Reguły przekształcania tokenu proste

Usługa tożsamości w usłudze są zwykle używane do implementowania między serwerami (S2S), takich jak uwierzytelnianie.  

#### <a name="migrating-to-azure-active-directory"></a>Migracja do usługi Azure Active Directory

Nasze zalecenia dla tego typu przepływ uwierzytelniania jest przeprowadzenie migracji do [ **usługi Azure Active Directory** ](https://azure.microsoft.com/develop/identity/signin/) (Azure AD). Usługi Azure AD jest dostawca oparte na tożsamości w chmurze firmy Microsoft pracy & szkolnego konta — jest dostawca tożsamości usługi Office 365, Azure i o wiele więcej. Jednak może również służyć do uwierzytelniania serwera do serwera przy użyciu usługi Azure AD implementacji przyznania poświadczeń klienta OAuth.  W poniższej tabeli porównano możliwości ACS w serwera do uwierzytelniania za pomocą dostępnych w usłudze Azure AD.

| Możliwości | Obsługa usług ACS | Pomoc techniczna platformy Azure AD |
| ---------- | ----------- | ---------------- |
| Jak zarejestrować usługi sieci web | Utwórz jednostki uzależnionej w portalu zarządzania usługi ACS. | Tworzenie aplikacji sieci web usługi Azure AD w portalu Azure. |
| Jak zarejestrować klienta | Tworzenie tożsamości usługi w portalu zarządzania usługi ACS. | Utwórz inną aplikację sieci web usługi Azure AD w portalu Azure. |
| Protokół używany | Protokół OAuth ZAWIJAĆ<br />Przyznanie poświadczeń klienta OAuth 2.0 projekt 13 | Przyznanie poświadczeń klienta OAuth 2.0 |
| Metody uwierzytelniania klienta | Proste hasło<br />SWT podpisem<br />Tokenu SAML z dostawców tożsamości federacyjnych | Proste hasło<br />Podpisany token JWT. |
| Formaty tokenów | TOKEN JWT<br />SAML 1.1<br />SAML 2.0<br />SWT<br /> | Tylko JWT |
| Token przekształcania | Dodawanie oświadczenia niestandardowe<br />Logika wystawiania oświadczeń Jeżeli to proste | Dodawanie oświadczenia niestandardowe | 
| Automatyzowanie zadań konfiguracji i zarządzania nimi. | Obsługiwane za pośrednictwem usługi ACS zarządzania | Obsługiwane za pośrednictwem programu Microsoft Graph i Azure AD Graph API. |

Wskazówki dotyczące implementującej scenariusze do serwera można znaleźć w następujących zasobach:

- [Przewodnik dewelopera usługi Azure AD Service-to-Service sekcji](https://aka.ms/aaddev).
- [Demon przykładowy kod przy użyciu poświadczeń klienta proste hasło](https://github.com/Azure-Samples/active-directory-dotnet-daemon)
- [Demon przykładowy kod przy użyciu poświadczeń klienta certyfikatu](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential)

#### <a name="other-migration-options"></a>Inne opcje migracji

Jeśli usługi Azure AD nie spełnia wymagania usługi sieci web, zostaw komentarz, a pomożemy Ci zidentyfikować najlepsze plan w określonych przypadkach.

<!--

#### Migrating to Ping Identity or Auth0

In some cases, you may find that neither Azure AD's client credentials OAuth grant implementation is not sufficient to replace ACS in your architecture without major code changes.  Some common examples might include:

- server-to-server authentication using token formats other than JWTs.
- server-to-server authentication using an input token provided by an external identity provider.
- server-to-server authentication with token transformation rules that Azure AD cannot reproduce.

In these cases, you may want to consider migrating your web application to another cloud authentication service. We recommend exploring the following options, as each provides capabilities similar to ACS:

- [Auth0](https://auth0.com/) has created [high-level migration guidance for customers of ACS](https://auth0.com/blog/windows-azure-acs-alternative-replacement/), and provides a feature-by-feature comparison of ACS vs. Auth0.
- Enterprise customers should consider [Ping Identity](https://www.pingidentity.com) as well. Reach out to us and we can connect you with a representative from Ping who is prepared to help identify potential solutions.

Our aim in working with Ping Identity & Auth0 is to ensure that all ACS customers have a path forward for their apps & services that minimizes the amount of work required to move off of ACS.

-->

## <a name="questions-concerns--feedback"></a>Pytania, problemy i opinie

Zdajemy wielu klientów ACS nie będzie zawierał ścieżki migracji wyczyść po przeczytaniu tego artykułu czy może być konieczne pewne pomocy lub wskazówek dotyczących określania właściwego planu. Jeśli chcesz omówimy Twoje scenariusze migracji & pytania, zostaw komentarz na tej stronie.
