---
title: "Rejestrować dane Analytics | Dokumentacja firmy Microsoft"
description: "Więcej informacji na temat sposobu analizy dzienników prywatności i zabezpiecza dane."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: a33bb05d-b310-4f2c-8f76-f627e600c8e7
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/03/2017
ms.author: magoedte
ms.openlocfilehash: 91af774560860b35913e57b49fb7a1dd59f5640f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="log-analytics-data-security"></a>Rejestrować analizy danych
Firma Microsoft dba o ochronę poufności Twoich informacji i zabezpieczania danych, podczas dostarczania oprogramowania i usług, które ułatwiają zarządzanie infrastruktury IT organizacji. Wiemy, że gdy powierzyć danych do innych osób, tej relacji zaufania wymaga rygorystyczne zabezpieczeń. Firma Microsoft przestrzega surowych wymogów z zakresu zabezpieczeń i zgodności — od kodu po działanie usługi.

Zabezpieczenia i ochrona danych ma najwyższy priorytet w firmie Microsoft. Skontaktuj się z nami w przypadku jakichkolwiek pytań, sugestie lub problemy związane z dowolnej z poniższych informacji, w tym nasze zasady zabezpieczeń w [opcje pomocy technicznej platformy Azure](http://azure.microsoft.com/support/options/).

W tym artykule opisano, jak dane są zbierane, przetwarzane i zabezpieczane przez usługi Analiza dzienników w Operations Management Suite (OMS). Korzystania z agentów, aby połączyć się z usługą sieci web, użyj programu System Center Operations Manager do zbierania danych operacyjnych lub pobrać danych diagnostycznych platformy Azure na potrzeby używania przez analizy dzienników. Zebrane dane są wysyłane przez Internet przy użyciu uwierzytelniania opartego na certyfikatach & SSL 3 z usługą analizy dzienników, która jest hostowana na platformie Microsoft Azure. Dane są kompresowane przez agenta, przed ich wysłaniem.

Usługi analizy dzienników bezpiecznie zarządza danych oparte na chmurze za pomocą następujących metod:

* Podział danych
* przechowywanie danych
* Zabezpieczenia fizyczne
* Zarządzanie zdarzeniami
* Zgodność
* Certyfikaty standardów zabezpieczeń

## <a name="data-segregation"></a>Podział danych
Dane klienta są przechowywane logicznie oddzielnie dla każdego składnika przez usługę. Wszystkie dane są otagowane informacjami o organizacji. To tagowanie jest obecne przez cały cykl życia danych i jest wymuszane w każdej warstwie usługi. Każdy klient ma dedykowany blob systemu Azure, która przechowuje dane długoterminowej

## <a name="data-retention"></a>Przechowywanie danych
Indeksowane dziennik wyszukiwania danych są przechowywane i przechowywane zgodnie z planem cenową. Aby uzyskać więcej informacji, zobacz [cennik analizy dziennika](https://azure.microsoft.com/pricing/details/log-analytics/).

Microsoft powoduje usunięcie danych klienta 30 dni, po zamknięciu obszar roboczy OMS. Microsoft spowoduje również usunięcie konta magazynu Azure, w którym znajdują się dane. Dane klienta zostanie usunięty, nie ma dysków fizycznych zostaną zniszczone.

W poniższej tabeli wymieniono niektóre z dostępnych rozwiązań w OMS i przykłady typów danych zbieranych.

| **Rozwiązania** | **Typy danych** |
| --- | --- |
| Ocena konfiguracji |Dane konfiguracji, metadane i dane o stanie |
| Planowanie pojemności |Dane dotyczące wydajności i metadane |
| Oprogramowanie chroniące przed złośliwym kodem |Dane konfiguracji i metadane |
| Ocena aktualizacji systemu |Metadane i stan danych. |
| Zarządzanie dziennikami |Zdefiniowane przez użytkownika dzienniki zdarzeń, dzienniki zdarzeń systemu Windows i/lub dzienniki programu IIS |
| Śledzenie zmian |Spis oprogramowania i metadanych usługi systemu Windows |
| SQL i oceny usługi Active Directory |Wyniki danych usługi WMI, dane rejestru dane dotyczące wydajności i dynamicznego zarządzania programu SQL Server |

W poniższej tabeli przedstawiono przykłady typów danych:

| **Typ danych** | **Pola** |
| --- | --- |
| Alerty |Alert nazwa, opis alertu, identyfikatorze BaseManagedEntityId, identyfikator problemu, IsMonitorAlert, RuleId, ResolutionState, priorytet, ważność, kategoria, właściciel, ResolvedBy, TimeRaised, TimeAdded, LastModified, LastModifiedBy, LastModifiedExceptRepeatCount, TimeResolved, RepeatCount TimeResolutionStateLastModified, TimeResolutionStateLastModifiedInDB, |
| Konfiguracja |CustomerID, identyfikator agenta, identyfikator jednostki, ManagedTypeID, ManagedTypePropertyID, CurrentValue, ChangeDate |
| Wydarzenie |Identyfikator zdarzenia, EventOriginalID, BaseManagedEntityInternalId, RuleId, PublisherId, PublisherName, FullNumber, numer, kategoria, ChannelLevel, LoggingComputer, EventData, EventParameters, TimeGenerated, TimeAdded <br>**Uwaga:** podczas zapisywania zdarzenia za pomocą pola niestandardowe w dzienniku zdarzeń systemu Windows OMS zbiera je. |
| Metadane |Identyfikatorze BaseManagedEntityId, ObjectStatus, jednostka organizacyjna, ActiveDirectoryObjectSid, PhysicalProcessors, NetworkName, adres IP, ForestDNSName, NetbiosComputerName, VirtualMachineName, LastInventoryDate, HostServerNameIsVirtualMachine, IP Adres, NetbiosDomainName, LogicalProcessors, DNSName, nazwa wyświetlana, DomainDnsName, ActiveDirectorySite, PrincipalName, OffsetInMinuteFromGreenwichTime |
| Wydajność |Nazwa obiektu, CounterName, PerfmonInstanceName, PerformanceDataId, PerformanceSourceInternalID, SampleValue, TimeSampled, TimeAdded |
| Stan |StateChangeEventId, StateId, NewHealthState, OldHealthState, kontekstu, TimeGenerated, TimeAdded, StateId2, identyfikatorze BaseManagedEntityId, elementu MonitorId, HealthState, LastModified, LastGreenAlertGenerated, DatabaseTimeModified |

## <a name="physical-security"></a>Zabezpieczenia fizyczne
Analiza dzienników w usługę jest obsługiwanego przez personel firmy Microsoft i wszystkich działań są rejestrowane i może być sprawdzona. Usługa całkowicie działa na platformie Azure i jest zgodny z Azure wspólne kryteria engineering. Możliwość wyświetlania szczegółów dotyczących zabezpieczeń fizycznych zasobów platformy Azure na stronie 18 [Przegląd zabezpieczeń systemu Microsoft Azure](http://download.microsoft.com/download/6/0/2/6028B1AE-4AEE-46CE-9187-641DA97FC1EE/Windows%20Azure%20Security%20Overview%20v1.01.pdf). Prawa fizyczny dostęp do bezpiecznego obszarów są zmieniane w ciągu jednego dnia roboczego dla każdego, kto nie ma już odpowiedzialność za usługę, w tym transfer i kończenie działania. Informacje o globalnej infrastruktury fizycznej używamy w [Datacenters Microsoft](https://www.microsoft.com/en-us/server-cloud/cloud-os/global-datacenters.aspx).

## <a name="incident-management"></a>Zarządzanie zdarzeniami
OMS ma procesu zarządzania zdarzeniami, które wszystkich usług firmy Microsoft jest zgodna. Podsumowując, firma Microsoft:

* Korzystają z modelu udostępnionego odpowiedzialność, gdzie część odpowiedzialność zabezpieczeń należy do firmy Microsoft i części należy do klienta
* Zarządzanie zdarzeniami zabezpieczeń platformy Azure
  * Uruchom postępowania w przypadku wykrycia zdarzenia
  * Oceń wpływ i ważność incydentu przez członków zespołu na wywołanie odpowiedzi na zdarzenia. Oparte na dowód, oceny może lub nie może powodować dalszych eskalacji do zespołu odpowiedź zabezpieczeń.
  * Diagnozowanie zdarzenia przez ekspertów odpowiedź zabezpieczeń do przeprowadzenia badania techniczne lub śledczej, określania strategii zawierania, ograniczenia i obejście. Jeśli zespół zabezpieczeń uznaje się, że dane klienta może stać się widoczne dla bezprawnego lub nieautoryzowane osoby, równolegle rozpocznie się wykonywanie równoległe procesu powiadomienie o zdarzeniu klienta.  
  * Ustabilizowania i odzyskać incydentu. Odpowiedzi na zdarzenia zespołu tworzy plan odzyskiwania, aby ograniczyć ten problem. Kryzysami zawierania kroków, takich jak poddawania wpływ na systemy mogą wystąpić natychmiast i równolegle z diagnostyki. Dłuższy okres środki zaradcze mogą być planowane występujące po upływie bezpośrednie ryzyko.  
  * Zamknięcie incydentu i przeprowadzanie których post. Odpowiedzi na zdarzenia zespołu tworzy zwłok, który zawiera szczegółowe informacje o zdarzeniu, o zamiar Popraw zasady, procedury i procesy w celu zapobiegania ponownemu zdarzenia.
* Powiadamia klientów o przypadki naruszenia zabezpieczeń
  * Określ zakres wpływ na klientów i zapewnienie każdy, kto jest w pełni funkcjonalne tak szczegółowe powiadomienia, jak to możliwe
  * Utwórz powiadomienie, aby zapewnić klientom szczegółowe informacje o za mało, aby mogli wykonywać dochodzenia ich end i spełniać żadnych zobowiązań, wprowadzone do użytkownikom końcowym podczas nie opóźnienia proces powiadamiania.
  * Potwierdź i zadeklarować zdarzenia, w razie potrzeby.
  * Powiadamia klientów o zdarzenia powiadomienia niezwłocznie nieuzasadnione i zgodnie z żadnych zobowiązań prawnych lub umownych. Powiadomienia zdarzenia zabezpieczeń są dostarczane do jednej lub kilku administratorów klienta w jakikolwiek sposób, który wybiera firmy Microsoft, w tym za pośrednictwem poczty e-mail.
* Należy przeprowadzić gotowości zespołu i szkolenia
  * Personel firmy Microsoft są wymagane do ukończenia zabezpieczeń i szkolenia świadomości, co ułatwia ich do identyfikacji i zgłaszać problemy z zabezpieczeniami podejrzane.  
  * Operatorzy pracy z usługą Microsoft Azure mają zobowiązań szkolenia dodanie otaczającego ich dostęp do ważnych systemów hosting danych klienta.
  * Specjalne szkolenia dotyczące ich ról odbierania personel odpowiedź zabezpieczeń firmy Microsoft

Gdy dojdzie do utraty danych klienta, powiadomimy każdego klienta w ciągu jednego dnia. Jednak utraty danych klienta nigdy nie wystąpił błąd związany z usługą OMS. Ponadto firma Microsoft zachowuje kopie danych, który został utworzony i jest geograficznie rozproszonych.

Aby uzyskać więcej informacji dotyczących sposobu Microsoft reaguje na przypadki naruszenia zabezpieczeń, zobacz [Response zabezpieczeń firmy Microsoft Azure w chmurze](https://gallery.technet.microsoft.com/Azure-Security-Response-in-dd18c678/file/150826/1/Microsoft Azure Security Response in the cloud.pdf).

## <a name="compliance"></a>Zgodność
OMS oprogramowania rozwoju i usługi zespołu informacji zabezpieczeń i nadzór nad program obsługuje jej wymagania biznesowe i stosuje przepisom eksportowym obowiązującym zgodnie z opisem w [Microsoft Azure Trust Center](https://azure.microsoft.com/support/trust-center/) i [ Centrum zaufania Microsoft zgodności](https://www.microsoft.com/en-us/TrustCenter/Compliance/default.aspx). Jak OMS ustanawia wymagania dotyczące zabezpieczeń, identyfikuje kontroli zabezpieczeń zarządza i monitoruje ryzyka są także opisane istnieje. Co rok, możemy przeglądu zasady, normy, procedury i wskazówek.

Każdego członka zespołu programowanie OMS odbiera szkolenia formalnego aplikacji w zakresie zabezpieczeń. Wewnętrznie używamy system kontroli wersji dla rozwoju oprogramowania. Każdy projekt oprogramowania jest chroniona przez system kontroli wersji.

Firma Microsoft ma zabezpieczeń i zgodności zespołu nadzoruje i ocenia wszystkich usług firmy Microsoft. Zabezpieczenia informatyków uzupełnić zespołu i nie są one powiązane z działów engineering, które opracowanie OMS. Biuro zabezpieczeń ma swoje własne łańcuch zarządzania i przeprowadzanie niezależnych ocen produktów i usług w celu zapewnienia bezpieczeństwa i zgodności.

Zarząd firmy Microsoft jest powiadamiany o coroczny raport o wszystkie programy zabezpieczeń informacji w firmie Microsoft.

Zespół rozwoju i usługi oprogramowania OMS aktywnie współpracuje z zespołów Legal firmy Microsoft i zgodności i innymi partnerami z branży można uzyskać różne certyfikaty.

## <a name="certifications-and-attestations"></a>Certyfikaty i poświadczenia
Analiza dzienników OMS spełnia następujące wymagania:

* [ISO/IEC 27001](http://www.iso.org/iso/home/standards/management-standards/iso27001.htm)
* [ISO/IEC 27018:2014](http://www.iso.org/iso/home/store/catalogue_tc/catalogue_detail.htm?csnumber=61498)
* [ISO 22301](https://azure.microsoft.com/en-us/blog/iso22301/)
* [Standard zabezpieczeń danych Industry (PCI zgodne) karta płatności (PCI DSS)](https://www.microsoft.com/en-us/TrustCenter/Compliance/PCI) przez Radę standardów PCI zabezpieczeń.
* [Typ usługi organizacji formantów (SOC) 1 1 i SOC 2 typu 1](https://www.microsoft.com/en-us/TrustCenter/Compliance/SOC1-and-2) zgodne
* [HIPAA i HITECH](https://www.microsoft.com/en-us/TrustCenter/Compliance/HIPAA) dla firm, które ma umowy skojarzyć HIPAA biznesowe
* Wspólne kryteria Engineering systemu Windows
* Wiarygodne technologie komputerowe firmy Microsoft (witryna może być w języku angielskim)
* Jak usługa Azure składniki używane przez OMS stosować się do wymagań dotyczących zgodności platformy Azure. Więcej w [zgodności Centrum zaufania Microsoft](https://www.microsoft.com/en-us/TrustCenter/Compliance/default.aspx).

> [!NOTE]
> W niektórych certyfikaty/poświadczenia, analizy dzienników jest wyświetlana w obszarze jego poprzednią nazwę *usługi Operational Insights*.
>
>


## <a name="cloud-computing-security-data-flow"></a>Chmura obliczeniowa zabezpieczeń przepływu danych
Na poniższym diagramie przedstawiono architekturę zabezpieczeń chmury jako przepływ informacji w firmie i jak jest zabezpieczony, ponieważ jest przenoszony z usługą analizy dzienników ostatecznie odebrane przez użytkownika w portalu OMS. Więcej informacji na temat każdego kroku następuje diagramu.

![Obraz OMS zbierania danych i zabezpieczeń](./media/log-analytics-security/log-analytics-security-diagram.png)

## <a name="1-sign-up-for-log-analytics-and-collect-data"></a>1. Zarejestruj się, aby zbieranie danych i analizy dzienników
Dla organizacji w celu wysyłania danych do analizy dzienników możesz skonfigurować agentów systemu Windows, uruchomionych na maszynach wirtualnych platformy Azure lub OMS agentów dla systemu Linux. Jeśli używasz agenty programu Operations Manager, następnie używasz Kreatora konfiguracji w konsoli operacje je skonfigurować. Użytkownicy (które mogą być, inne poszczególnych użytkowników lub grupy osób) Utwórz co najmniej jednego konta OMS (OMS obszarów roboczych) i zarejestrować agentów przy użyciu jednej z następujących kont:

* [Identyfikator organizacji](../active-directory/sign-up-organization.md)
* [Konto Microsoft - Outlook pakietu Office na żywo, MSN](http://www.microsoft.com/account/default.aspx)

Obszar roboczy OMS jest gdzie dane są zbierane, zagregowane, poddane analizie i przedstawiony. Obszar roboczy służy głównie w celu danych partycji i każdego obszaru roboczego jest unikatowa. Na przykład możesz chcieć danych produkcyjnych zarządzanych za pomocą jednego obszarem roboczym pakietu OMS i badanie danych zarządzanych za pomocą innego obszaru roboczego. Obszary robocze również pomóc administratora kontroli dostępu użytkownika do danych. Każdy obszar roboczy może mieć wiele kont użytkowników skojarzonych z nim, a wszystkie konta użytkowników mogą uzyskiwać dostęp do wielu obszarów roboczych OMS. Możesz utworzyć obszarów roboczych na podstawie regionu centrum danych. Każdego obszaru roboczego są replikowane do innych centrów danych w regionie, przede wszystkim dotyczące dostępności usług OMS.

Po ukończeniu pracy Kreatora konfiguracji, każdej grupie zarządzania programu Operations Manager dla programu Operations Manager, ustanawia połączenie z usługą analizy dzienników. Następnie używasz Kreatora dodawania komputerów do wyboru, które komputery w grupie zarządzania mogą wysyłać dane do usługi. Dla innych typów agenta, każdy bezpieczny sposób łączy się z usługą OMS.

Cała komunikacja między połączonych systemów i usługą analizy dzienników jest zaszyfrowany.  Protokół TLS (HTTPS) jest używany do szyfrowania.  Proces SDL firmy Microsoft jest zachowana w celu upewnij się, że analizy dzienników jest aktualny i najnowsze osiągnięcia w protokołów kryptograficznych.

Każdy typ agenta zbiera dane analizy dziennika. Typ danych zbieranych jest zależy od typów rozwiązań używane. Wyświetlane podsumowanie zbierania danych w [rozwiązań dodać analizy dzienników z galerii rozwiązań](log-analytics-add-solutions.md). Ponadto bardziej szczegółowe informacje o kolekcji jest dostępna w przypadku większości rozwiązań. Rozwiązanie to pakiet wstępnie zdefiniowanych widoków, zapytania wyszukiwania dziennika zasady zbierania danych i przetwarzania logiki. Tylko administratorzy mogą używać analizy dzienników do importowania rozwiązania. Po zaimportowaniu rozwiązania jest przenoszony do serwerów zarządzania programu Operations Manager (jeśli jest używany), a następnie do wszystkich wybranych agentów. W efekcie agentów zbierania danych.

## <a name="2-send-data-from-agents"></a>2. Wysyłanie danych z agentów
Zarejestruj wszystkie typy agenta przy użyciu klucza rejestracji i jest ustanowić bezpiecznego połączenia między agentem i usługą analizy dzienników przy użyciu uwierzytelniania opartego na certyfikatach oraz protokołu SSL z portem 443. OMS używa tajnego magazynu do generowania i Obsługa kluczy. Klucze prywatne są obracane co 90 dni i są przechowywane na platformie Azure i są zarządzane przez operacje platformy Azure, którzy postępuj zgodnie z ograniczeniami praktyk przepisami i zgodności.

Z programem Operations Manager należy zarejestrować obszaru roboczego z usługą analizy dzienników i jest ustanowić bezpiecznego połączenia HTTPS między serwerem zarządzania programu Operations Manager.

Agentów z systemem Windows uruchomionych na maszynach wirtualnych platformy Azure klucz magazynu tylko do odczytu jest używany do odczytu zdarzeń diagnostycznych w tabelach platformy Azure.

W przypadku nie można nawiązać komunikacji z usługą dowolnej przyczyny każdego agenta, zebrane dane są przechowywane lokalnie w tymczasowej pamięci podręcznej, a serwer zarządzania próbuje ponownie wysłać dane co osiem minut przez 2 godziny. Agenta buforowane dane są chronione przez Magazyn poświadczeń systemu operacyjnego. Jeśli usługa nie może przetworzyć danych po dwóch godzinach, agentów może umieścić w kolejce danych. Jeśli zapełnienia kolejki OMS uruchamia porzucenie typy danych, począwszy od danych dotyczących wydajności. Limit kolejki agenta jest klucz rejestru, można je zmodyfikować, w razie potrzeby. Zebrane dane jest skompresowany i wysyłane do usługi, pomijanie lokalnych baz danych, więc nie dodaje żadnych obciążenia do nich. Po wysłaniu zebranych danych, zostanie ono usunięte z pamięci podręcznej.

Jak opisano powyżej, dane z agentów są wysyłane za pośrednictwem protokołu SSL do centrów danych Microsoft Azure. Opcjonalnie można używać usługi ExpressRoute, aby zapewnić dodatkową ochronę danych. ExpressRoute to sposób bezpośredniego połączenia z platformą Azure od istniejącej sieci WAN, takich jak wiele protokołów etykietę przełączania sieci VPN (MPLS), pochodzącymi z dostawcą usługi sieciowej. Aby uzyskać więcej informacji, zobacz [ExpressRoute](https://azure.microsoft.com/services/expressroute/).

## <a name="3-the-log-analytics-service-receives-and-processes-data"></a>3. Usługi analizy dzienników odbiera i przetwarza dane
Usługi analizy dzienników zapewnia, że dane przychodzące jest z zaufanego źródła, sprawdzając poprawność certyfikatów i integralności danych za pomocą uwierzytelniania systemu Azure. Nieprzetworzone dane są następnie przechowywane jako obiektu blob w [magazyn Microsoft Azure](../storage/common/storage-introduction.md) i nie jest zaszyfrowany. Jednak każdy obiekt blob magazynu Azure ma zestaw unikatowego zestawu kluczy, który jest dostępny tylko dla tego użytkownika. Typ danych przechowywanych zależy od typów rozwiązań, które zostały zaimportowane i używane do zbierania danych. Usługi analizy dzienników przetwarza danych pierwotnych dla obiektu blob magazynu Azure.

## <a name="4-use-log-analytics-to-access-the-data"></a>4. Umożliwia dostęp do danych analizy dzienników
Aby móc zalogować się Log Analytics w portalu OMS za pomocą konta organizacyjnego lub konta Microsoft, które należy wcześniej skonfigurować. Cały ruch między portalu OMS i analizy dzienników w OMS są wysyłane za pośrednictwem bezpiecznego kanału HTTPS. Korzystając z portalu OMS, identyfikator sesji jest generowany na kliencie użytkownika (przeglądarki sieci web), a dane są przechowywane w lokalnej pamięci podręcznej, dopóki sesja zostanie zakończona. Zakończone, pamięci podręcznej są usuwane. Pliki cookie po stronie klienta, które nie zawierają informacji umożliwiających identyfikację użytkownika, nie są automatycznie usuwane. Pliki cookie dotyczące sesji są oznaczane HTTPOnly i są zabezpieczone. Po upływie wstępnie określoną bezczynności sesji portalu OMS zostało przerwane.

Za pomocą portalu OMS, można eksportować dane do pliku CSV i można uzyskać dostępu do danych przy użyciu interfejsy API wyszukiwania. Eksport CSV jest ograniczone do 50 000 wierszy na eksportu i danych interfejsu API jest ograniczony do 5000 wierszy na wyszukiwania.

## <a name="next-steps"></a>Następne kroki
* [Wprowadzenie do analizy dzienników](log-analytics-get-started.md) Dowiedz się więcej na temat analizy dzienników i rozpocząć pracę w minutach.
