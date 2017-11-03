---
title: "Zarządzanie po migracji — baza danych SQL Azure | Dokumentacja firmy Microsoft"
description: "Informacje o sposobie zarządzania bazą danych po zakończeniu migracji do usługi Azure SQL Database."
services: sql-database
documentationcenter: 
author: joesackmsft
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: migrate
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Inactive
ms.date: 10/14/2016
ms.author: Joe.Sack
ms.suite: SQL
ms.prod_service: sql-database
ms.component: management
ms.openlocfilehash: 663ab4aaf229f8a88d1116b34ccb74450aa66c9d
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/31/2017
---
# <a name="how-should-i-manage-my-azure-sql-database-after-migration"></a>Jak zarządzać bazy danych Azure SQL po zakończeniu migracji?

*Często zadawane pytania dotyczące zarządzania inwestycji bazy danych SQL Azure*

Dlatego ostatnio przejściu baz danych programu SQL Server do bazy danych SQL Azure lub planowane jest możliwe, że przenoszenie bardzo szybko. Po przejściu, co to jest dalej? Biorąc pod uwagę, że baza danych SQL jest *platforma jako usługa*, Microsoft obsługuje kilku obszarów w Twoim imieniu. Ale jak dokładnie to zmienia rozwiązania firmy wokół kluczowych obszarach, takich jak zabezpieczenia, ciągłość prowadzenia działalności biznesowej, konserwacji bazy danych, optymalizowanie wydajności, monitorowania i inne? 

Celem tego artykułu jest krótkiej formie organizowania zasobów i wskazówki, które należy do wprowadzania zmiany do zarządzania inwestycji bazy danych SQL. Główne obszary, w tym artykule opisano ciągłość prowadzenia działalności biznesowej, zabezpieczeń, konserwacji bazy danych i monitorowania przepływu danych dotyczących wydajności i. Najważniejsze kwestie, które różnią się między programu SQL Server i bazy danych SQL i wyróżnienia operacyjne najlepsze rozwiązania, które pomogą Ci zmaksymalizować korzyści i zminimalizować ryzyko omówione zostaną następujące czynności. 

## <a name="manage-business-continuity-after-migration"></a>Zarządzanie ciągłość prowadzenia działalności biznesowej po migracji

### <a name="how-do-i-create-and-manage-backups-on-sql-database"></a>Jak tworzyć i zarządzać kopiami zapasowymi w bazie danych SQL 
Bazy danych SQL automatycznie tworzy kopię zapasową bazy danych dla Ciebie i udostępnia możliwość przywracania do dowolnego punktu w czasie w okresie przechowywania. Okres przechowywania to 35 dni, standardowa i Premium baz danych i 7 dni do podstawowej bazy danych. Ponadto funkcja długoterminowe przechowywanie umożliwia przechowywania kopii zapasowej plików dłużej (do 10 lat), a następnie Przywróć z tych kopii zapasowych w dowolnym momencie. Ponadto kopie zapasowe bazy danych są geograficznie replikowane w celu zapewnienia możliwości przywracaniem geograficznym w dowolnym regionie, w przypadku katastrofy lub klęski regionalne. Zobacz [omówienie ciągłości działalności biznesowej](sql-database-business-continuity.md).

### <a name="how-do-i-ensure-business-continuity-in-the-event-of-a-datacenter-level-disaster-or-a-regional-catastrophe"></a>Jak zapewnić ciągłość prowadzenia działalności biznesowej w przypadku awarii centrum danych na poziomie lub regionalnych katastrofy? 

Kopie zapasowe bazy danych są geograficznie replikowane w celu zapewnienia możliwości przywracaniem geograficznym w dowolnym regionie, w przypadku katastrofy lub klęski regionalne. Zobacz [omówienie ciągłości działalności biznesowej](sql-database-business-continuity.md). Ponadto bazy danych SQL oferuje możliwość utrzymania aktywnie replikowane geograficznie pomocnicze bazy danych w innym regionie. Ich konfiguracji w grupie automatycznej pracy w trybie Failover sprawdzi, że baz danych automatycznie pracy awaryjnej na serwerze pomocniczym w scenariuszu po awarii. Jeśli grupa automatycznej pracy w trybie failover nie jest skonfigurowana, aplikacja musi aktywnie monitorowania do awarii i zainicjuj tryb failover na serwerze pomocniczym. 
### <a name="sql-server-provided-me-readable-secondary-replicas-can-i-access-the-secondaries-on-sql-database"></a>SQL Server podane mnie do odczytu replikach pomocniczych, można uzyskać dostęp do pomocniczych baz danych w bazie danych SQL? 

Tak, funkcja aktywna replikacja geograficzna jest używana do tworzenia do odczytu replikach pomocniczych. 

### <a name="how-does-my-disaster-recovery-plan-change-from-on-premise-to-sql-database"></a>Jak planu odzyskiwania po awarii zmienia z lokalną bazą danych SQL? 
Implementacje programu SQL Server wymaga aktywnego zarządzania kopii zapasowych przy użyciu funkcji, takich jak klaster pracy awaryjnej, dublowania bazy danych, replikacji, aby wysyłanie dziennika lub tylko zwykły podstawowego pliku BACPAC kopii zapasowych. Jednak w bazie danych SQL, kopie zapasowe są w pełni zarządzany przez firmę Microsoft i tylko może mieć planów odzyskiwania po awarii i skonfigurowana i działa za pomocą kilku kliknięć w portalu Azure (lub kilku poleceń programu PowerShell). ‌
### <a name="in-the-event-of-disaster-how-do-i-recover-my-databases"></a>W przypadku awarii, w jaki sposób przywrócić baz danych? 
Baza danych SQL pozwala automatycznie przywrócenia baz danych do dowolnego punktu w czasie w ciągu ostatnich 35 dni. Jest to opcja, jeśli utratę danych lub stają przed po awarii związanych z aplikacją. 

W przypadku regionalnej awarii, są narażeni skonfigurowanie dodatkowej bazy danych replikacją geograficzną, można odzyskać z geograficznie pomocnicze bazy danych w innym regionie. W czasie rzeczywistym dostępu do aplikacji możesz w trybie Failover na serwerze pomocniczym geograficzną, w innym regionie ręcznie. Alternatywnie Jeśli istnieje grupa pracy awaryjnej automatycznie skonfigurowane, tej pracy awaryjnej do dodatkowej geograficzna odbywa się automatycznie w przypadku awarii. Jeśli nie masz replikowane geograficznie pomocnicze bazy danych skonfigurowane, nadal można odzyskać bazy danych z automatycznego replikowanych plików kopii zapasowej (wbudowanej funkcji Konfiguracja nie jest potrzebna), o stosunkowo dłuższy czas odzyskiwania (RTO 12 godzin), jak i po jednym Godzina utraty danych. 

### <a name="are-the-failovers-to-secondary-transparent-how-does-my-application-handle-database-failovers"></a>Tryb failover na dodatkowej są niewidoczne? Jak Moja aplikacja obsługuje bazy danych w tryb failover? 
Jeśli masz skonfigurowane grupy pracy awaryjnej automatycznie trybu failover na dodatkowej jest niewidoczny. Jednak jeśli nie masz, następnie aplikacji musi zawierać logikę do monitorowania dostępności serwera podstawowego, a następnie ręcznie przełączyć się na serwerze pomocniczym. 
 
## <a name="manage-security-after-migration"></a>Zarządzanie zabezpieczeniami po migracji

### <a name="how-can-i-restrict-access-to-my-sql-database"></a>Jak ograniczyć dostęp do bazy danych SQL 
 
Istnieje kilka sposobów zablokowanie łączności dostępu do baz danych SQL. 
1. Limit ruchu za pośrednictwem Internetu Express trasy powoduje dedykowanego fiber sieć platformy Azure, aby dane nie są przesyłane za pośrednictwem Internetu. Istnieje również możliwość konfigurowania cross łączności region przy użyciu Express route. Poniższe linki zawierają opis Express Route bardziej szczegółowo: 
 - [Wprowadzenie na Express Route](../expressroute/expressroute-introduction.md)
 - [Wymagania wstępne](../expressroute/expressroute-prerequisites.md) 
 - [Przepływy pracy](../expressroute/expressroute-workflows.md) 
 
2. Wybierz, które zasoby łączenia z bazą danych SQL: 

   Domyślnie Twoja baza danych SQL jest skonfigurowane i umożliwiają "wszystkich usług platformy Azure" — co oznacza, że wszystkie maszyny wirtualne na platformie Azure może próbować połączyć się z bazą danych.  Nadal musi nastąpić uwierzytelnienie wszystkie nazwy logowania. Jeśli nie chcesz bazę danych, aby być dostępna dla wszystkich adresów IP platformy Azure, możesz wyłączyć "Zezwalaj na wszystkich usług platformy Azure" i użyć [punktów końcowych usługi sieci Wirtualnej](sql-database-vnet-service-endpoint-rule-overview.md) ograniczyć dostęp przychodzący do bazy danych z tylko zasobów platformy Azure, które znajdują się w danym Podsieci sieci Wirtualnej platformy Azure. 

   ![Punkty końcowe usługi sieci Wirtualnej](./media/sql-database-manage-after-migration/vnet-service-endpoints.png) 

   Opcja alternatywna jest świadczeniem [zastrzeżone adresy IP](../virtual-network/virtual-networks-reserved-public-ip.md) dla maszyn wirtualnych i dozwolonych tych określonych adresów IP maszyny Wirtualnej na serwerze ustawienia zapory. (Na przykład w portalu Azure, zobacz zrzut ekranu poniżej). Przypisując zastrzeżonych adresów IP, należy zapisać problemy o można zaktualizować reguł zapory ze zmianą adresów IP. 

3. Unikaj Udostępnianie portu 1433 poza Azure

   Uruchom narzędzia SSMS przy użyciu usługi Azure [usługi Azure RemoteApp](https://www.microsoft.com/cloud-platform/azure-remoteapp-client-apps). Ta sytuacja nie wymaga otwarcia połączenia wychodzące do portu 1433, adres IP jest statyczny, więc bazy danych może być otwarty tylko programów RemoteApp, obsługuje wiele Multi-Factor Authentication (MFA) i jest wielu użytkowników. 

### <a name="what-authentication-methods-are-offered-in-sql-database"></a>Jakie metody uwierzytelniania są oferowane w bazie danych SQL?

Metody uwierzytelniania głównego oferowane w bazie danych SQL i magazyn danych SQL są uwierzytelniania usługi Azure Active Directory i uwierzytelniania SQL. Azure Active Directory (AD) jest scentralizowane usługi zarządzania tożsamościami i dostępem i SQL jest tylko jedna z wielu usług Azure, które są zintegrowane z usługą Azure AD. Zaletą scentralizowane zarządzanych usług jest, czy we wszystkich usługach Azure, używane w przypadku prostszych uwierzytelniania jest udostępniany poświadczeń użytkownika. Umożliwia także bazy danych SQL i magazyn danych SQL, zapewniają uwierzytelnianie wieloskładnikowe i konta użytkownika gościa w ramach domeny usługi Azure AD. 

Jeśli masz już usługi Active Directory — lokalny, można było wykonać Federację katalogu w usłudze Azure Active Directory, aby rozszerzyć katalog na platformie Azure. 


|||
|---|---|
| Jeśli…|Baza danych Azure SQL / magazyn danych Azure SQL|
| Nie chce używać usługi Azure Active Directory (AD) na platformie Azure|Użyj [uwierzytelnianie SQL](sql-database-security-overview.md)|
| Lokalne AD używane na serwerze SQL|[Federację AD z usługą Azure AD](../active-directory/connect/active-directory-aadconnect.md)i korzystać z uwierzytelniania usługi Azure AD. Dzięki temu można użyć rejestracji jednokrotnej.|
| Musisz wymusić uwierzytelnianie wieloskładnikowe (MFA)|Uwierzytelniania MFA można wymagać jako zasady za pomocą [dostępu warunkowego Microsoft](sql-database-conditional-access.md)i użyj [uwierzytelniania usługi Azure AD uniwersalnych z obsługą uwierzytelniania Wieloskładnikowego](sql-database-ssms-mfa-authentication.md).|
| Mają konta gościa z konta Microsoft (live.com, outlook.com) lub z innych domen (gmail.com)|Użyj [uwierzytelniania usługi Azure AD uniwersalnych](sql-database-ssms-mfa-authentication.md) w magazynie danych/bazy danych SQL, która wykorzystuje [współpracy B2B usługi Azure AD](../active-directory/active-directory-b2b-what-is-azure-ad-b2b.md).|
| Zalogować się do systemu Windows z domeny federacyjnej przy użyciu poświadczeń usługi Azure AD|Użyj [zintegrowane uwierzytelnianie usługi Azure AD](sql-database-aad-authentication-configure.md).|
| Zalogować się do systemu Windows przy użyciu poświadczeń z domeny nie jest Sfederowane przy użyciu usługi Azure|Użyj [uwierzytelnianie hasłem usługi Azure AD](sql-database-aad-authentication-configure.md).|
| Usługi warstwy środkowej, które mogą połączyć się z bazą danych SQL Azure lub magazynu danych|Użyj [token uwierzytelniania usługi Azure AD](sql-database-aad-authentication-configure.md).
|||

### <a name="how-can-i-limit-access-to-sensitive-data-in-my-databases-from-the-application-side"></a>Jak ograniczyć dostęp do poufnych danych w baz danych ze strony aplikacji? 

Aby zapobiec nieautoryzowanemu możliwość wyświetlania danych poufnych, Brak dostępnych kilka opcji w bazie danych SQL: 

- [Zawsze zaszyfrowane](/sql/relational-databases/security/encryption/always-encrypted-database-engine) jest formą szyfrowania po stronie klienta, który szyfruje poufnych kolumny w bazie danych (tak, aby były w tekstu szyfrowanego dla administratorów bazy danych i nieautoryzowanym użytkownikom). Klucz dla zawsze szyfrowane są przechowywane po stronie klienta, więc tylko autoryzowani klienci można odszyfrować poufnych kolumn. Zaszyfrowane zawsze obsługuje porównywanie równości dzisiaj, więc DBAs mogą w dalszym ciągu zapytania zaszyfrowanych kolumn jako część ich poleceń SQL. Zawsze zaszyfrowane mogą być używane z różnych opcji magazynu kluczy, takich jak [usługi Azure Key Vault](sql-database-always-encrypted-azure-key-vault.md), magazynu certyfikatów systemu Windows i lokalnej sprzętowych modułów zabezpieczeń.
- [Dynamiczne maskowanie danych](sql-database-dynamic-data-masking-get-started.md) to funkcja maskowania danych ograniczająca ujawnienie danych poufnych przez maskowania go użytkownikom bez uprawnień na warstwie aplikacji. Zdefiniuj reguły maskowania, którą można utworzyć wzorzec maskowania (na przykład do pokazania tylko ostatnie 4 cyfry identyfikatora krajowych i oznacz rest jako x) i zidentyfikować użytkowników, którzy mogą być wykluczone z zasady maskowania.
- [Zabezpieczenia na poziomie wiersza](/sql/relational-databases/security/row-level-security) umożliwia kontrolę dostępu do wierszy w tabeli bazy danych na podstawie użytkownika wykonywania zapytania (grupy członkostwa lub wykonywania context). Ograniczenie dostępu odbywa się na warstwie bazy danych, zamiast w warstwie aplikacji, aby uprościć logiki aplikacji. 

### <a name="what-encryption-options-do-i-have-in-sql-database-and-what-actors-does-the-encryption-protect-from"></a>Jakie opcje szyfrowania są dostępne w bazie danych SQL, i jakie podmiotów Szyfrowanie chroni z?
Istnieją trzy technologii szyfrowania główne, które są dostępne w bazie danych SQL: 
- [Zawsze zaszyfrowane](/sql/relational-databases/security/encryption/always-encrypted-database-engine) (który jest wymieniony w powyższym pytanie): szyfruje poufnych kolumn w tabeli kompleksowe od nieautoryzowanych klientów na dysku fizycznym. Serwera i administratorów danych nie widzi poufnych danych, ponieważ klucze szyfrowania są przechowywane na kliencie. 
- [Funkcja przezroczystego szyfrowania danych](/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql) (funkcji TDE): szyfrowanie magazynowanych, które są szyfrowane na poziomie bazy danych i chroni pliki danych, plików dziennika i skojarzonych kopii zapasowych przed kradzieżą nośnik fizyczny. Funkcji TDE jest domyślnie włączona dla wszystkich nowo utworzone bazy danych.
 
  Na poniższym diagramie przedstawiono omówienie szyfrowania Wybór technologii.

   ![Omówienie szyfrowania](./media/sql-database-manage-after-migration/overview-encryption.png)

### <a name="how-should-i-manage-encryption-keys-in-the-cloud"></a>Jak zarządzać kluczami szyfrowania w chmurze? 
Dostępne są opcje zarządzania kluczami zawsze zaszyfrowane (szyfrowanie po stronie klienta) i przezroczystego szyfrowania danych (szyfrowanie w stanie spoczynku). Zaleca się regularne zmienić klucze szyfrowania, a z częstotliwością która wyrównuje z zgodności i przepisów wewnętrznych wymagań.

- **Zawsze zaszyfrowane**: Brak [hierarchii dwa klucza](/sql/relational-databases/security/encryption/overview-of-key-management-for-always-encrypted) w zawsze zaszyfrowane - kolumnę poufne dane są szyfrowane za AES 256 klucza szyfrowania kolumny (CEK), który jest natomiast szyfrowany za pomocą klucza głównego kolumny (CMK). Sterowniki klientów przewidzianych zawsze zaszyfrowane ma żadne ograniczenia dotyczące długości CMKs.

  Zaszyfrowaną wartość CEK są przechowywane w bazie danych, a CMK jest przechowywany w magazynie kluczy zaufanych, takich jak magazynu certyfikatów systemu Windows, usługi Azure Key Vault lub sprzętowego modułu zabezpieczeń. 
  
  Zarówno [CEK i CMK](/sql/relational-databases/security/encryption/rotate-always-encrypted-keys-using-powershell) można obracać. Obracanie CEK może być intensywnie czas w zależności od rozmiaru tabel zawierających zaszyfrowanych kolumn. Dlatego bardzo ostrożnie zaplanować CEK obrotów. Z drugiej strony, obracanie CMK nie zakłóca wydajność bazy danych i może odbywać się z rolami rozdzielone.

  Na poniższym diagramie przedstawiono opcje magazynu kluczy dla kluczy głównych kolumn w zawsze zaszyfrowane 

   ![Zawsze zaszyfrowane CMK przechowywania dostawców](./media/sql-database-manage-after-migration/always-encrypted.png)

- **Funkcji przezroczystego szyfrowania danych (TDE)**: Brak hierarchii dwa klucza w funkcji TDE — dane w każdej bazie danych użytkownika są szyfrowane przez symetrycznego AES 256 unikatowy bazy danych klucza szyfrowania bazy danych (jeśli Istnieje), który z kolei są szyfrowane za pomocą serwera unikatowy RSA asymetryczne 2048 klucza głównego. 

  Domyślnie klucza głównego przezroczystego szyfrowania danych jest zarządzane przez usługę bazy danych SQL dla wygody. Jeśli organizacja będzie potrzebować kontrolę nad klucza głównego, istnieje możliwość użycia [usługi Azure Key Vault](/sql/relational-databases/security/encryption/transparent-data-encryption-byok-azure-sql) jako magazynu kluczy. 

  Za pomocą usługi Azure Key Vault, organizacji zakłada kontrolę nad formanty klawisza inicjowania obsługi administracyjnej, obracanie i uprawnienia. [Obracanie lub przełączenie typ klucza głównego funkcji TDE](/sql/relational-databases/security/encryption/transparent-data-encryption-byok-azure-sql-key-rotation) jest szybkie, jak tylko ponownie zaszyfrowanego klucza szyfrowania danych. 

  Dla organizacji mających separacji ról między zabezpieczeń i zarządzanie danymi administratora zabezpieczeń można udostępnić materiału klucza dla klucza głównego funkcji TDE w usłudze Azure Key Vault i podaj identyfikator klucza usługi Azure Key Vault administratora bazy danych do użycia dla szyfrowanie przechowywanych na serwerze. 

## <a name="monitoring-and-compliance-after-migration"></a>Monitorowanie i zgodność po migracji

### <a name="how-do-i-monitor-database-activities-in-sql-database"></a>Jak monitorować działania bazy danych w bazie danych SQL?
Istnieje kilka monitorowania wbudowane bazy danych SQL, które śledzić zabezpieczeń oraz inne zdarzenia w bazie danych:
- [Inspekcja SQL](sql-database-auditing.md) umożliwia zbieranie dzienników inspekcji zdarzeń bazy danych na koncie magazynu Azure.
- [Wykrywanie zagrożeń SQL](sql-database-threat-detection.md) pozwala wykryć podejrzane działania, które wskazują możliwe złośliwymi działaniami dostępu, do naruszenia lub wykorzystać w bazie danych. Wykrywanie zagrożeń bazy danych SQL działa wiele zestawów algorytmy, które Wykrywanie potencjalnych luk w zabezpieczeniach i ataki, a także wzorce dostępu nietypowych bazy danych (takich jak dostęp z nietypowych lokalizacji lub przez nieznane podmiot zabezpieczeń). Funkcjonariusze lub innych administratorów wyznaczonych otrzymywać wiadomość e-mail z powiadomieniem po wykryciu zagrożenia w bazie danych. Każde powiadomienie zawiera szczegółowe informacje o podejrzanych działaniach i zalecenia dotyczące dalszego zbadania i ograniczyć zagrożenie. 
- [Oceny luk w zabezpieczeniach SQL](sql-vulnerability-assessment.md) jest bazą danych skanowania i raportowania usługi, która umożliwia monitorowanie stanu zabezpieczeń baz danych na dużą skalę oraz identyfikację zagrożenia bezpieczeństwa i rozchodzenia z planu bazowego zabezpieczeń zdefiniowane przez użytkownika. Po każdym skanowaniu listę niestandardowych można wykonać kroki i skrypty korygowania podano oraz raport oceny, który może służyć za pomoc w celu zapewnienia zgodności. 
- [Aplikacja synchronizacji zabezpieczeń SQL OMS](https://github.com/Microsoft/Azure-SQL-DB-auditing-OMS-integration) wykorzystuje publiczne interfejsy API Operations Management Suite (OMS) do push dzienników inspekcji SQL zestawie OMS analizy dzienników i możliwość definiowania alerty o wykryciu niestandardowych w tym: 
 - Kafelek inspekcji bazy danych SQL & pulpitu nawigacyjnego udostępniających jasny i spójny raportu działań bazy danych. 
 - Analiza dzienników SQL analizowanie aktywności bazy danych i badanie odchylenia i anomalie, które mogą wskazywać podejrzane naruszenia zabezpieczeń.
 - Zaawansowane określone reguły alertów obserwowanych zdarzeń, które mogą powodować poczty e-mail, element runbook automatyzacji Webhook i Azure alerty (tj. zmiany hasła, after-hours, określonych poleceń SQL).
- [Centrum zabezpieczeń Azure](../security-center/security-center-intro.md) oferuje zarządzania scentralizowanych zabezpieczeń między obciążenia działające na platformie Azure i lokalnego, a w innych chmur. Możesz wyświetlić czy są skonfigurowane na wszystkie zasoby niezbędne ochrony bazy danych SQL, takich jak inspekcji i przezroczystego szyfrowania danych i utworzyć zasady na podstawie własnych wymagań. 

### <a name="is-sql-database-compliant-with-any-regulatory-requirements-and-how-does-that-help-with-my-own-organizations-compliance"></a>Jest zgodna z wymagań regulacyjnych bazy danych SQL, i w jaki sposób który pomaga z własnej organizacji zgodności? 
Baza danych SQL Azure jest zgodne z przepisami compliances zakresem. Aby wyświetlić najnowsze zbiór compliances, które zostały spełnione, odwiedź stronę [Microsoft Trust Center](https://www.microsoft.com/trustcenter/compliance/complianceofferings) oraz rozwijanie compliances, które są ważne dla organizacji, aby zobaczyć, czy baza danych SQL Azure jest objęte zgodne Azure usługi. Należy pamiętać, że chociaż baza danych SQL Azure mogą mieć certyfikat potwierdzający usługi zgodne, pomaga w zgodności organizacji usługi ale nie automatycznie gwarantuje on. 

## <a name="database-maintenance-and-monitoring-after-migration"></a>Monitorowanie po migracji i konserwacji bazy danych

### <a name="how-do-i-monitor-growth-in-data-size-and-resource-utilization"></a>Jak monitorować wzrostu danych rozmiaru i użycia zasobów?

- Monitor metryki można wyświetlić informacje z bazy danych rozmiaru i użycia zasobów na wykresie "Monitorowanie" w portalu Azure. 

  ![Wykres monitorowania](./media/sql-database-manage-after-migration/monitoring-chart.png)

- Aby uzyskać lepszy wgląd i przechodzenie do szczegółów kwerendy, można użyć "Zapytania szczegółowe informacje o wydajności" dostępne w portalu Azure. Wymaga to czy "Magazynu zapytań" jest aktywny w bazie danych.

  ![Szczegółowe informacje o wydajności zapytań](./media/sql-database-manage-after-migration/query-performance-insight.png)

- Alternatywnie można wyświetlić metryki przy użyciu dynamicznych widoków zarządzania (widoków DMV) zbyt — za pomocą [sys.dm_db_resource_stats](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database) i [sys.resource_stats](/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database). 

### <a name="how-often-do-i-need-to-run-consistency-checks-like-dbcccheckdb"></a>Jak często konieczne, uruchom kontrolę spójności, takich jak DBCC_CHECKDB?
DBCC_CHECKDB sprawdza integralność logiczny i fizyczny wszystkich obiektów w bazie danych. Nie trzeba wykonywać tych kontroli, ponieważ są one zarządzane przez firmę Microsoft Azure. Aby uzyskać więcej informacji, zobacz [integralność danych w bazie danych SQL Azure](https://azure.microsoft.com/blog/data-integrity-in-azure-sql-database/)

## <a name="monitor-performance-and-resource-utilization-after-migration"></a>Monitorowanie wydajności i użycia zasobów po migracji

### <a name="how-do-i-monitor-performance-and-resource-utilization-in-azure-sql-database"></a>Jak monitorować wydajność i użycie zasobów w bazie danych SQL Azure
Umożliwia monitorowanie wydajności i użycia zasobów w bazie danych SQL Azure przy użyciu następujących metod:

- **Azure portal**: Azure portal zawiera wykorzystanie pojedynczej bazy danych wybraniu bazy danych, a następnie klikając na wykresie w okienku przeglądu. Można zmodyfikować na wykresie, aby wyświetlić wiele metryk, takich jak procent użycia procesora CPU, procent użycia jednostek DTU, dane we/wy wartość procentowa i sesje oraz jako procent rozmiaru bazy danych. 
  ![wykorzystanie zasobów](./media/sql-database-manage-after-migration/resource-utilization.png)

  Z tego wykresu można również skonfigurować alerty przez zasób. Te alerty umożliwiają odpowiadanie na warunkach zasobów z wiadomości e-mail, zapisać punktu końcowego HTTPS/HTTP lub wykonania akcji. Zobacz [monitorowanie wydajności bazy danych w bazie danych SQL Azure](sql-database-single-database-monitor.md) szczegółowe informacje.

- **Widoki**: można zbadać [sys.dm_db_resource_stats](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database) dynamiczny widok zarządzania do zwrócenia historii statystyk zużycia zasobów z ostatniej godziny i [sys.resource_stats](/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database) System widoku wykazu zwracanego historii przez ostatnie 14 dni. 

- **Szczegółowe informacje o wydajności zapytań**: [szczegółowe informacje o wydajności zapytań](sql-database-query-performance.md) umożliwia wyświetlenie historii najważniejszych zapytań korzystanie z zasobów i długotrwałe zapytania dotyczące określonej bazy danych. Ta funkcja wymaga [magazyn zapytań](/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store) być włączone i aktywnej bazy danych.

- **Azure Analytics SQL (wersja zapoznawcza) w analizy dzienników**: [Azure Log Analytics](../log-analytics/log-analytics-azure-sql.md) służy do zbierania i wizualizacji kluczowych miar wydajności usługi Azure SQL Azure, Obsługa maksymalnie 150 000 baz danych SQL Azure i 5000 elastycznej SQL Pule na obszar roboczy. Służy on do monitorowania i otrzymywać powiadomienia. Można monitorować bazy danych SQL Azure i metryki elastycznej puli w wielu subskrypcji platformy Azure i pule elastyczne i mogą służyć do identyfikowania problemów w każdej warstwie stosu aplikacji. 

### <a name="how-do-i-ensure-i-am-using-the-appropriate-service-tier-and-performance-level"></a>Jak zapewnić, że używam odpowiednią warstwę i poziom wydajności usługi?
Monitor [sys.dm_db_resource_stats](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database) i [sys.resource_stats](/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database) dynamicznych widoków zarządzania aby zrozumieć zużycie Procesora, operacji We/Wy i pamięci. Można również użyć bazy danych SQL [szczegółowe informacje o wydajności zapytań](sql-database-query-performance.md) wyświetlić zużycia zasobów. Jeśli używasz spójnie na wysoki odsetek dostępne zasoby, powinny rozważ przeniesienie na wyższy poziom wydajności w ramach istniejącego warstwy usług lub Przenieś do wyższego poziomu usługi. Z drugiej strony Jeśli używane są stale niski procent dostępnych zasobów, może rozważ przeniesienie na niższy poziom wydajności lub warstwy usługi.

### <a name="i-am-seeing-performance-issues-how-does-my-azure-sql-database-troubleshooting-methodology-differ-from-sql-server"></a>Widzę problemy z wydajnością. Jaka jest różnica Moje metodologia rozwiązywania problemów z bazy danych SQL Azure z programu SQL Server?
Wiele aspektów metodologia rozwiązywania problemów z wydajności jest taka sama w bazie danych SQL Azure, ale będzie pewne różnice. Na przykład, jeśli widzisz spadku ogólnej wydajności monitorowania [sys.dm_db_resource_stats](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database) i [sys.resource_stats](/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database) dynamicznych widoków zarządzania aby zrozumieć, Procesora, operacji We/Wy i pamięci Użycie. Należy zmienić poziom wydajności i/lub usługa warstwy, w zależności od potrzeb obciążenia.
Aby rozbudowany zestaw zaleceń dotyczących dostrajania problemy z wydajnością, zobacz [dostrajania wydajności w bazie danych SQL Azure](sql-database-performance-guidance.md). 

### <a name="do-i-need-to-maintain-indexes-and-statistics"></a>Należy utrzymywać indeksy i statystyki?
Baza danych SQL Azure nie przechowuje indeksy i statystyki automatycznie w ramach usługi. Ponosisz odpowiedzialność za utrzymanie indeksy i Statystyki planowania. Następujący artykuł metod automatyzacji Azure, szczegóły kilka opcji Planowanie zadań konserwacji bazy danych SQL Azure.

## <a name="data-movement-after-migration"></a>Przenoszenie danych po migracji

### <a name="how-do-i-export-and-import-data-as-bacpac-files-from-azure-sql-database"></a>Jak wyeksportować i zaimportować dane jako pliki pliku BACPAC z bazy danych SQL Azure 

- **Eksportuj**: baza danych SQL Azure można wyeksportować w formacie pliku BACPAC z portalu Azure.

  ![Eksportowanie jako pliku BACPAC](./media/sql-database-export/database-export.png)

- **Importuj**: można zaimportować pliku BACPAC pliku do bazy danych przy użyciu portalu Azure.

  ![Importowanie pliku BACPAC](./media/sql-database-import/import.png)

### <a name="how-do-i-synchronize-data-between-azure-sql-database-sql-server-2016--2012"></a>Sposób synchronizacji danych między Azure SQL bazy danych programu SQL Server 2016 / 2012?
[Synchronizacji danych](sql-database-sync-data.md) funkcja pomaga w synchronizacji danych dwukierunkowo między wieloma lokalnych baz danych programu SQL Server i bazy danych SQL Azure. Jednak ponieważ wyzwalacz na podstawie spójność ostateczna jest gwarantowana (bez utraty danych), ale nie gwarantuje spójności transakcji. 

## <a name="next-steps"></a>Następne kroki
Dowiedz się więcej o [baza danych Azure SQL](sql-database-technical-overview.md).
