---
title: "Cloud App Discovery zabezpieczeń i zagadnienia dotyczące ochrony prywatności | Dokumentacja firmy Microsoft"
description: "W tym temacie opisano zagadnienia dotyczące zabezpieczeń i prywatności związane z Cloud App Discovery."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
ms.assetid: 2fce5c82-d3de-4097-808f-40214768df9e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 7775bad1503e62e1887e6d85f67c3107f48866bf
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/16/2018
---
# <a name="cloud-app-discovery-security-and-privacy-considerations"></a>Cloud App Discovery zabezpieczeń i zagadnienia dotyczące ochrony prywatności
W tym temacie wyjaśniono, jak dane są zbierane, przetwarzane i zabezpieczane w ramach usługi Azure Active Directory Cloud App Discovery. Firma Microsoft dba o ochronę poufności Twoich informacji i zabezpieczania danych. Microsoft jest zgodna bezpiecznego programowania cyklu życia rozwiązania dla świadczenia usługi. Zabezpieczenia i ochrona danych ma najwyższy priorytet w firmie Microsoft.

> [!TIP] 
> Zapoznaj się z nowego bez agenta Cloud App Discovery w usłudze Azure Active Directory (Azure AD), która rozszerzono przez [Integracja z usługą Microsoft Cloud App Security](https://portal.cloudappsecurity.com). 

## <a name="overview"></a>Przegląd
Usługa cloud App Discovery to funkcja usługi Azure AD i jest hostowana na platformie Microsoft Azure.  
Cloud App Discovery endpoint agent służy do zbierania danych aplikacji z IT zarządzanych komputerów. Zebrane dane są bezpiecznie wysyłane za pośrednictwem kanału szyfrowanego z usługą Azure AD Cloud App Discovery. Dane organizacji Cloud App Discovery będzie widoczne w portalu Azure. 

![Jak działa usługa Cloud App Discovery](./media/active-directory-cloudappdiscovery-security-and-privacy-considerations/cad01.png) 

Poniższe sekcje wykonaj bezpiecznego przepływu informacji z organizacji z usługą Cloud App Discovery a ostatecznie jest przesyłany do portalu Cloud App Discovery.

## <a name="collecting-data-from-your-organization"></a>Zbieranie danych z Twojej organizacji
Aby można było używać funkcji wykrywania aplikacji w chmurze Azure Active Directory, aby uzyskać wgląd w aplikacje używane przez pracowników w organizacji, należy najpierw wdrożyć agenta programu Azure AD Cloud App Discovery punktu końcowego na komputerach w organizacji.

Administratorzy dzierżawy usługi Azure Active Directory (lub ich delegata) można pobrać pakiet instalacyjny agenta z portalu Azure. Agenta można zostać ręcznie zainstalowane lub zainstalowany na wielu komputerach w organizacji za pomocą programu SCCM lub zasad grupy.

Aby uzyskać dalsze instrukcje na temat opcji wdrażania, zobacz [Podręcznik wdrażania zasad grupy chmury aplikacji odnajdywania](http://social.technet.microsoft.com/wiki/contents/articles/30965.cloud-app-discovery-group-policy-deployment-guide.aspx).


### <a name="data-collected-by-the-agent"></a>Dane zebrane przez agenta
Informacje przedstawione na poniższej liście są zbierane przez agenta, po nawiązaniu połączenia do aplikacji sieci Web. Informacje są zbierane tylko w przypadku aplikacji, które administrator skonfigurował odnajdywania. Można edytować listę aplikacji w chmurze, które agent monitoruje za pośrednictwem Cloud App Discovery w usłudze Azure AD w programie Microsoft [portalu Azure](https://portal.azure.com/)w obszarze **ustawienia**->**zbierania danych**  -> **Listy aplikacji kolekcji**. 

**Informacje o kategorii**: informacje o użytkowniku  
**Opis elementu**: nazwa użytkownika systemu Windows procesu, który zgłosił żądanie do docelowej aplikacji sieci Web (na przykład domena azwa_użytkownika) oraz identyfikatora zabezpieczeń systemu Windows (SID) użytkownika.

**Informacje o kategorii**: przetworzyć informacji  
**Opis elementu**: Nazwa procesu, który wysłał żądanie do docelowej aplikacji sieci Web (na przykład iexplore.exe)

**Informacje o kategorii**: informacje o komputerze  
**Opis elementu**: Nazwa NetBIOS komputera, na którym jest zainstalowany agent.

**Informacje o kategorii**: informacje o aplikacji ruchu  
**Opis elementu**: następujące informacje o połączeniu:

* Źródło (komputer lokalny) i docelowe adresy IP i numery portów
* Publiczny adres IP organizacji, przez który żądanie trafia.
* Czas żądania
* Wielkość ruchu sieciowego wysłanych i odebranych
* Wersja IP (4 lub 6)
* Tylko w połączeniach TLS: Nazwa hosta docelowego z rozszerzeń wskaźnika nazwy serwera lub certyfikatu serwera.

Poniższe informacje HTTP:

* — Metoda (GET, POST itp.)
* Protokół (HTTP/1.1, itp.)
* Ciąg agenta użytkownika
* Nazwa hosta
* Docelowy identyfikator URI (z wyłączeniem ciągu zapytania)
* Informacje o typie zawartości
* Adresy URL odwołania (z wyjątkiem ciągu zapytania)

> [!NOTE]
> Powyższe informacje HTTP są zbierane dla wszystkich połączeń niezaszyfrowanych.
> Dla połączeń TLS te informacje są przechwytywane tylko, gdy ustawienie "Głęboka inspekcja" jest włączona w portalu. Ustawienie ma wartość "ON" domyślnie.
> Aby uzyskać więcej informacji, zobacz poniżej i [uzyskiwanie uruchomiona z Cloud App Discovery](http://social.technet.microsoft.com/wiki/contents/articles/30962.getting-started-with-cloud-app-discovery.aspx)
> 
> 

Oprócz dane, które agent zbiera dane o aktywności sieci również zbiera anonimowe dane o
* Konfiguracja sprzętu i oprogramowania
* Raporty o błędach
* Dane dotyczące sposobu używania agenta.


### <a name="how-the-agent-works"></a>Jak działa agent
Instalacja agenta obejmuje dwa składniki:

* Składnik trybu użytkownika
* Składnik sterowników trybu jądra (driver platformy filtrowania systemu Windows)

Jeśli najpierw zainstalowano agenta przechowuje zaufanego certyfikatu komputera na komputerze, który następnie używa do ustanowienia bezpiecznego połączenia z usługą Cloud App Discovery. Agent okresowo pobiera konfigurację zasad z usługi Cloud App Discovery za pośrednictwem tego bezpiecznego połączenia. Zasady zawiera informacje na temat monitora i czy automatycznego aktualizowania powinno być włączone, między innymi które aplikacje w chmurze.

Jak ruch w sieci Web są wysyłane i odbierane na komputerze w programie Internet Explorer i Chrome, agenta Cloud App Discovery analizuje ruch i wyodrębnia istotne metadanych (zobacz **danych zbieranych przez agenta** powyższej sekcji).  
Co minutę, agent przekazywania zebranych metadanych z usługą Cloud App Discovery za pośrednictwem kanału szyfrowanego.

Składnik sterowników przechwytuje szyfrowanego ruchu sieciowego i umieszczony w strumieniu zaszyfrowane. Więcej szczegółów w **przechwycenia danych z zaszyfrowanego połączenia (głęboka inspekcja)** poniższej sekcji.

### <a name="respecting-user-privacy"></a>Przestrzeganie zasady zachowania poufności użytkownika
Naszym celem jest zapewnienie Administratorzy narzędzia można ustawić równowagi między szczegółowe światłowodowa do użycia i użytkownika prywatności aplikacji zależnie od potrzeb organizacji. W tym celu firma Microsoft udostępnia następujące pokrętła na stronie ustawień w portalu:

* **Zbieranie danych**: Administratorzy mogą wybrać określić kategorie aplikacji, które chcą, aby uzyskać dane odnajdywania w lub aplikacji.
* **Głęboka inspekcja**: Administratorzy mogą wybrać do określenia, czy agent zbiera ruchu HTTP na potrzeby połączeń SSL/TLS (alias **"Głęboka inspekcja"**). Więcej informacji na temat to w następnej sekcji.
* **Zgoda opcje**: Administratorzy mogą używać portalu Cloud App Discovery do zdecyduj, czy chcesz powiadomić użytkowników zbierania danych przez agenta i czy wymagać od użytkownika zgodę przed uruchomieniem agenta zbierania danych użytkownika.

Cloud App Discovery endpoint agent tylko zbiera informacje opisane w **danych zbieranych przez agenta** powyższej sekcji.

### <a name="intercepting-data-from-encrypted-connections-deep-inspection"></a>Przechwycenia danych z zaszyfrowanego połączenia (głęboka inspekcja)
Jak wspomniano wcześniej, Administratorzy mogą skonfigurować agenta do monitorowania danych z zaszyfrowanego połączenia ("głęboka inspekcja"). TLS ([Transport Layer Security](https://msdn.microsoft.com/library/windows/desktop/aa380516%28v=vs.85%29.aspx)) jest jednym z najbardziej popularne protokoły w Internecie dzisiaj. Przez szyfrowania komunikacji z protokołem TLS, klient może ustanowić kanał bezpieczeństwo i prywatność komunikacji z serwerem sieci web; Protokoły TLS zapewnia ochronę niezbędne do przekazywania poświadczeń uwierzytelniania i zapobiec ujawnieniu informacji poufnych.

Gdy end-to-end bezpiecznego zaszyfrowanego kanału udostępniane przez protokoły TLS umożliwia ważne zabezpieczenia i ochrona prywatności, protokół jest często użyte do celów złośliwego lub nefarious. Zbyt duża, w rzeczywistości tego TLS jest często określany jako "Protokół uniwersalnych obejścia zapory". Przyczyny problemu są nie może sprawdzić komunikację TLS, ponieważ dane warstwy aplikacji są szyfrowane przy użyciu protokołu SSL większości zapór. Wiedzy o to, osoby atakujące często wykorzystać protokół TLS w celu dostarczenia złośliwego ładunków użytkownikowi pewność, że nawet najbardziej inteligentnego zapór warstwy aplikacji są całkowicie ukryta protokołu TLS i po prostu musi przekazywać TLS komunikacji między hostami. Użytkownicy końcowi często wykorzystać protokół TLS w celu obejścia kontroli dostępu, wymuszane przez ich firmowej zapory i serwery proxy, za pomocą jego nawiązywanie publicznych serwerów proxy i tunelowania-TLS protokołów za pośrednictwem zapory, które w przeciwnym razie może zostać zablokowany przez zasady.

Głęboka inspekcja pozwala agentowi Cloud App Discovery na działanie jako zaufany man-in--middle. Po wysłaniu żądania klienta jest dostęp do zasobu chronionego protokołu HTTPS, sterownik Endpoint Agent przechwytuje połączenia i ustanawia nowego połączenia z serwerem docelowym w celu pobiera swojego certyfikatu SSL w imieniu klienta. Agent następnie sprawdza, czy certyfikat mogą być zaufane (przez sprawdzanie, czy nie został odwołany i wykonywania innych operacje sprawdzania certyfikatów), a jeśli te przebiegu, a następnie Agent Endpoint kopiuje dane z certyfikatu serwera i tworzy własny serwer certyfikat--znany pod nazwą certyfikatu przechwytywaniu — za pomocą tych informacji. Przechwycenie certyfikatu jest podpisem na bieżąco przez agenta punktu końcowego za pomocą certyfikatu głównego, który jest instalowany w magazynie zaufanych certyfikatów systemu Windows. Ten certyfikat główny z podpisem własnym jest oznaczony jako eksportowalny z systemem innym niż i jest ma listy ACL dla administratorów. Jest on przeznaczony do nigdy nie opuszczają komputer, na którym został utworzony. Aplikacja kliencka użytkownika końcowego odebrania certyfikatu przechwytywaniu go go ufa ponieważ można pomyślnie zweryfikować łańcucha certyfikatów do certyfikatu głównego. Ten proces jest przeważnie przezroczysty od użytkownika końcowego punktu widzenia z kilku zastrzeżeniami zgodnie z poniższym opisem.

Włączając głęboka inspekcja agenta Cloud App Discovery Endpoint Agent można odszyfrować i sprawdzić komunikację TLS zaszyfrowane, dzięki czemu usługa zmniejszenia szumu i szczegółowymi informacjami na temat użycia aplikacji w chmurze zaszyfrowane.

#### <a name="a-word-of-caution"></a>Wyraz Uwaga
Przed włączeniem głęboka inspekcja, zdecydowanie zaleca się komunikacji zamiaru prawne i działu HR i uzyskać zgodę. Zapoznanie się użytkownika końcowego prywatnej szyfrowanie komunikacji może być poufnych podmiotu, oczywiste ze względu na. Przed produkcji wdrożenie głębokiej inspekcji upewnij się, że zabezpieczeń firmy i wskaż, będą poddawane szyfrowaną komunikację zostały zaktualizowane zasady dopuszczalnych działań. Powiadomienie użytkownika i wykluczania witryn uznany za poufnych (np. bankowości i medyczne witryny) również może być konieczne, jeśli konfigurujesz Cloud App Discovery, aby monitorować je. Jak wspomniano powyżej, Administratorzy mogą używać portalu Cloud App Discovery, aby wybrać, czy chcesz powiadomić użytkowników zbierania danych przez agenta i wymaganie zgody użytkownika przed uruchomieniem agenta zbierania danych użytkownika.

### <a name="known-issues-and-drawbacks"></a>Znane problemy i wady
Istnieje kilka przypadków, w którym zatrzymania protokołu TLS może mieć wpływ na środowisko użytkownika końcowego:

* Rozszerzonej weryfikacji zastosowanie certyfikatów renderowania zielony jako wizualny sygnał odwiedzają zaufanych witryn sieci web na pasku adresu przeglądarki sieci web. Kontroli TLS nie może mieć takiej Weryfikacją w certyfikat, który wystawia klientowi, więc witryn sieci web, które korzystają z Weryfikacją certyfikatów będą działały normalnie, ale na pasku adresu nie będzie wyświetlany zielony.  
* Publicznego klucza przypinanie (określane również jako przypinania certyfikatu) mają pomóc chronić użytkowników przed atakami man-in--middle i nieautoryzowane urzędów certyfikacji. Gdy certyfikat główny dla lokacji przypiętych nie pasuje do jednej z znanej dobrej urzędu certyfikacji, przeglądarka odrzuca połączenie z powodu błędu. Ponieważ zatrzymania protokołu TLS jest w rzeczywistości man-in--middle, te połączenia nie powiedzie się.
* Jeśli użytkownik kliknie ikonę blokady w przeglądarce paska adresu przeglądarki, aby sprawdzić informacje o lokacji, nie zobaczy łańcuch końcówką urząd certyfikacji używany do podpisywania certyfikatu witryny sieci Web, ale zamiast zaufanego łańcuch certyfikatów z systemu Windows Magazyn certyfikatów.

Aby ograniczyć liczbę wystąpień tych problemów, firma Microsoft zachować informacje o usługi w chmurze i aplikacje klienckie znane rozszerzonej weryfikacji lub przypinanie klucza publicznego i agenta Endpoint w celu uniknięcia przechwytywaniu wpływ na połączenia. Nawet w takich przypadkach jednak nadal będą odbierane raporty dotyczące użycia tych aplikacji w chmurze oraz ilość transferowanych danych, ale ponieważ nie są one głębokiej inspekcji, będą dostępne nie szczegóły dotyczące używania aplikacji.

## <a name="sending-data-to-cloud-app-discovery"></a>Wysyłanie danych do Cloud App Discovery
Gdy metadane zostały zebrane przez agenta, przez jedną minutę lub dopóki nie osiągnie rozmiar to 5 MB buforowane dane są buforowane na komputerze. Następnie jest skompresowany i wysyłane za pośrednictwem bezpiecznego połączenia z usługą Cloud App Discovery.

Jeśli agent nie może skomunikować się z usługą Cloud App Discovery jakiegokolwiek powodu, zebranych metadane są przechowywane w pamięci podręcznej z pliku lokalnego jest możliwy tylko przez użytkowników uprzywilejowanych na komputerze (na przykład do grupy Administratorzy).  
Agent automatycznie podejmie próbę ponownego wysłania metadane w pamięci podręcznej, dopóki pomyślnie zostały odebrane przez usługę Cloud App Discovery.

## <a name="receiving-the-data-at-the-service-end"></a>Odbieranie danych z końcem usługi
Agenci uwierzytelniania Cloud App Discovery usługi przy użyciu certyfikatu uwierzytelniania klienta z określonej maszyny powyżej i przekazuje dane za pośrednictwem kanału szyfrowanego.  
Usługa Cloud App Discovery potoku analizy przetwarza metadanych dla każdego klienta oddzielnie logicznie podział na wszystkich etapach potoku analizy.
Przeanalizowane metadanych dysków różne raporty w portalu.

Nieprzetworzone metadanych i przeanalizowane metadane są przechowywane przez 180 dni. Ponadto klienci mogą wybrać do przechwytywania przeanalizowane metadanych w ich wybór konta magazynu obiektów blob platformy Azure.
Jest to przydatne w trybie offline analizy metadanych, a także dłużej przechowywania danych.

## <a name="accessing-the-data-using-the-azure-portal"></a>Uzyskiwanie dostępu do danych za pomocą portalu Azure
W celu zabezpieczenia metadanych zbierane domyślnie tylko administratorzy globalni dzierżawcy mają dostęp do funkcji Cloud App Discovery w portalu Azure.  
Jednak administratorzy mogą wybrać delegować dostęp do innych użytkowników lub grup.

> [!NOTE]
> Aby uzyskać więcej informacji, zobacz [pobierania uruchomiona z Cloud App Discovery](http://social.technet.microsoft.com/wiki/contents/articles/30962.getting-started-with-cloud-app-discovery.aspx)
> 
> 


Każdy użytkownik, uzyskiwanie dostępu do danych w portalu muszą mieć licencję z licencją Azure AD Premium.

## <a name="additional-resources"></a>Dodatkowe zasoby
* [Jak odnajdywać niezatwierdzone aplikacje w chmurze używanych mojej organizacji](active-directory-cloudappdiscovery-whatis.md)
* [Indeks artykułów dotyczących zarządzania aplikacjami w usłudze Azure Active Directory](active-directory-apps-index.md)

