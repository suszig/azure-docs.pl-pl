---
title: "Łącznik programu PowerShell | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano, jak skonfigurować łącznik usługi firmy Microsoft Windows PowerShell."
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: 6dba8e34-a874-4ff0-90bc-bd2b0a4199b5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 0e5ccf5a38072e31d85bbc63eb0c608b0c34cfc2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="windows-powershell-connector-technical-reference"></a>Informacje techniczne dotyczące łącznika usługi Windows PowerShell
W tym artykule opisano łącznik usługi Windows PowerShell. Artykuł dotyczy następujących produktów:

* Program Microsoft Identity Manager 2016 (MIM2016)
* Forefront Identity Manager 2010 R2 (FIM2010R2)
  * Należy użyć poprawki 4.1.3671.0 lub nowszym [KB3092178](https://support.microsoft.com/kb/3092178).

MIM2016 i FIM2010R2 łącznika jest dostępna do pobrania z [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=717495).

## <a name="overview-of-the-powershell-connector"></a>Omówienie łącznika programu PowerShell
Łącznik programu PowerShell pozwala na Integrowanie usługi synchronizacji z systemami zewnętrznymi, które oferują środowiska Windows PowerShell na podstawie interfejsów API. Łącznik zapewnia mostka między możliwości agenta zarządzania na podstawie rozmów extensible łączności 2 framework (ECMA2) i programu Windows PowerShell. Aby uzyskać więcej informacji na temat ECMA framework zobacz [Extensible odwołanie agenta zarządzania 2.2 łączności](https://msdn.microsoft.com/library/windows/desktop/hh859557.aspx).

### <a name="prerequisites"></a>Wymagania wstępne
Przed użyciem łącznika, upewnij się, że masz następujące czynności na serwerze synchronizacji:

* Framework Microsoft .NET 4.5.2 lub nowszej
* Program Windows PowerShell 2.0, 3.0 lub 4.0

Zasady wykonywania na serwerze usługi synchronizacji musi być skonfigurowane i umożliwiają łącznika na uruchamianie skryptów środowiska Windows PowerShell. Jeśli skrypty uruchamia łącznika są podpisane cyfrowo, konfigurowanie zasad wykonywania, uruchamiając polecenie:  
`Set-ExecutionPolicy -ExecutionPolicy RemoteSigned`

## <a name="create-a-new-connector"></a>Utwórz nowy łącznik
Aby utworzyć łącznik programu Windows PowerShell w usługi synchronizacji, musisz podać szereg skryptów programu Windows PowerShell, których wykonywanie czynności wymaganych przez usługę synchronizacji. W zależności od źródła danych, gdy połączysz się i funkcje, które są wymagane skrypty, którą musi implementować zależy. W tej części opisano poszczególne skrypty, które można zaimplementować i gdy są one wymagane.

Łącznik programu Windows PowerShell służących do przechowywania wszystkich skryptów w bazie danych usługi synchronizacji. Gdy użytkownik może uruchamiać skrypty, które są przechowywane w systemie plików, łatwiej Wstaw treść każdego skryptu bezpośrednio w konfiguracji łącznika.

Aby utworzyć łącznik programu PowerShell w **usługi synchronizacji** wybierz **agenta zarządzania** i **Utwórz**. Wybierz **środowiska PowerShell (Microsoft)** łącznika.

![Tworzenie łącznika](./media/active-directory-aadconnectsync-connector-powershell/createconnector.png)

### <a name="connectivity"></a>Łączność
Podaj parametry konfiguracji do nawiązywania połączenia z systemem zdalnym. Te wartości są bezpiecznie przechowywane przez usługę synchronizacji i udostępnione skryptów środowiska Windows PowerShell uruchamianych łącznika.

![Łączność](./media/active-directory-aadconnectsync-connector-powershell/connectivity.png)

Można skonfigurować następujące parametry połączenia:

**Łączność**

| Parametr | Wartość domyślna | Przeznaczenie |
| --- | --- | --- |
| Serwer |<Blank> |Nazwa serwera, która łącznik należy nawiązać połączenie. |
| Domeny |<Blank> |Domena poświadczenia, aby zapisać do użycia, gdy jest uruchamiany łącznik. |
| Użytkownik |<Blank> |Nazwa użytkownika poświadczeń przechowywanie do użycia, gdy jest uruchamiany łącznik. |
| Hasło |<Blank> |Hasła poświadczenia, aby zapisać do użycia, gdy jest uruchamiany łącznik. |
| Personifikuj konto łącznika |False |W przypadku wartości PRAWDA Usługa synchronizacji działa skryptów programu Windows PowerShell w kontekście dostarczone poświadczenia. Jeśli to możliwe, zalecane jest **$Credentials** parametr jest przekazywany do każdej skryptu jest używany zamiast personifikacji. Aby uzyskać więcej informacji o dodatkowe uprawnienia, które są wymagane, aby użyć tej opcji, zobacz [dodatkowej konfiguracji personifikacji](#additional-configuration-for-impersonation). |
| Załaduj profil użytkownika, gdy personifikacji |False |Instruuje system Windows do załadowania profilu użytkownika łącznika poświadczeń podczas personifikacji. Jeśli nazwa personifikowanego użytkownika ma profil mobilny, łącznik nie ładuje profilu mobilnego. Aby uzyskać więcej informacji o dodatkowe uprawnienia, które są wymagane do tego parametru należy użyć, zobacz [dodatkowej konfiguracji personifikacji](#additional-configuration-for-impersonation). |
| Typ logowania, gdy personifikacji |Brak |Typ logowania podczas personifikacji. Aby uzyskać więcej informacji, zobacz [dwLogonType] [ dw] dokumentacji. |
| Tylko skrypty podpisane |False |Jeśli PRAWDA, łącznik programu Windows PowerShell sprawdza, czy każdy skrypt ma nieprawidłowy podpis cyfrowy. W przypadku wartości FAŁSZ, upewnij się, że zasady wykonywania środowiska Windows PowerShell dla serwera usługi synchronizacji jest RemoteSigned lub bez ograniczeń. |

**Typowe modułu**  
Łącznik umożliwia przechowywanie udostępnionych moduł programu Windows PowerShell w konfiguracji. Łącznik, uruchomienie skryptu, moduł programu Windows PowerShell został wyodrębniony do systemu plików, aby można było importować przez każdego skryptu.

W przypadku skryptów importu, eksportu i synchronizacja haseł wspólnego modułu został wyodrębniony do folderu MAData łącznika. Schemat, sprawdzanie poprawności hierarchii i partycji skryptów odnajdywania wspólny moduł został wyodrębniony do folderu % TEMP %. W obu przypadkach wyodrębnionego skryptu wspólnego modułu nosi nazwę zgodnie z ustawieniem nazwa pospolita skryptu modułu.

Aby załadować moduł o nazwie FIMPowerShellConnectorModule.psm1 z folderu MAData, użyj następujących instrukcji:`Import-Module (Join-Path -Path [Microsoft.MetadirectoryServices.MAUtils]::MAFolder -ChildPath "FIMPowerShellConnectorModule.psm1")`

Aby załadować moduł o nazwie FIMPowerShellConnectorModule.psm1 z folderu % TEMP %, należy użyć następującej instrukcji:`Import-Module (Join-Path -Path $env:TEMP -ChildPath "FIMPowerShellConnectorModule.psm1")`

**Sprawdzanie poprawności parametru**  
Skrypt do weryfikowania jest opcjonalny skrypt programu Windows PowerShell, który może służyć do zapewnienia, że parametry konfiguracji łącznika określone przez administratora są prawidłowe. Sprawdzania poprawności serwera, poświadczeń połączenia i parametry połączenia są typowe zastosowania skryptu weryfikacji. Skrypt do weryfikowania jest wywoływana po wykonaniu następujące karty i okna dialogowe zostaną zmodyfikowane:

* Łączność
* Parametry globalne
* Partycja konfiguracji

Skrypt do weryfikowania otrzymuje następujące parametry z łącznika:

| Nazwa | Typ danych | Opis |
| --- | --- | --- |
| ConfigParameterPage |[ConfigParameterPage][cpp] |Na karcie Konfiguracja lub okna dialogowego, która wyzwoliła żądania weryfikacji. |
| ConfigParameters |[KeyedCollection] [ keyk] [ciągu [ConfigParameter][cp]] |Tabela parametry konfiguracji łącznika. |
| Poświadczenie |[PSCredential][pscred] |Zawiera wszystkie poświadczenia wprowadzane przez administratora, na karcie połączenie. |

Skrypt do weryfikowania powinien zwrócić pojedynczy obiekt ParameterValidationResult do potoku.

**Odnajdywanie schematu**  
Skrypt wykrywania schematu jest obowiązkowe. Ten skrypt zwraca typów obiektów, atrybuty i ograniczenia atrybutu, używane przez usługę synchronizacji podczas konfigurowania reguły przepływu atrybutu. Skrypt wykrywania schematu jest uruchamiany podczas tworzenia łącznika i wypełnia schematu łącznika. Służy również przez akcję Odśwież schemat Menedżera usługi synchronizacji.

Skrypt wykrywania schematu otrzymuje następujące parametry z łącznika:

| Nazwa | Typ danych | Opis |
| --- | --- | --- |
| ConfigParameters |[KeyedCollection] [ keyk] [ciągu [ConfigParameter][cp]] |Tabela parametry konfiguracji łącznika. |
| Poświadczenie |[PSCredential][pscred] |Zawiera wszystkie poświadczenia wprowadzane przez administratora, na karcie połączenie. |

Skrypt musi zwracać jedną [schematu] [ schema] obiektu do potoku. Obiekt schematu składa się z [elementu SchemaType] [ schemaT] obiektów, które reprezentują typy obiektów (na przykład: użytkowników i grup). Obiekt elementu SchemaType przetrzymuje kolekcję [SchemaAttribute] [ schemaA] obiektów, które reprezentują atrybutów (na przykład: imię, nazwisko i adres pocztowy) tego typu.

**Dodatkowe parametry**  
Oprócz standardowych ustawień konfiguracji można zdefiniować dodatkowe niestandardowych ustawień konfiguracji specyficznych dla wystąpienia łącznika. Te parametry można określić w łącznik partycji, lub wykonywania kroku poziomy i dostępne w odpowiednich skrypt programu Windows PowerShell. Ustawienia konfiguracji niestandardowej mogą być przechowywane w bazie danych usługi synchronizacji w formacie zwykłego tekstu lub może być zaszyfrowany. Usługa synchronizacji są automatycznie szyfrowane i odszyfrowywane bezpieczne ustawienia konfiguracji, gdy jest to wymagane.

Aby określić ustawienia konfiguracji niestandardowej, nazwę każdego parametru należy oddzielić przecinkami (,).

Aby uzyskać dostęp do niestandardowych ustawień konfiguracji ze skryptu, musi sufiks nazwy od znaku podkreślenia ( \_ ) i zakres parametru (Global, partycji lub RunStep). Na przykład aby uzyskać dostęp do parametru globalnych, należy użyć następujący fragment kodu:`$ConfigurationParameters["FileName_Global"].Value`

### <a name="capabilities"></a>Możliwości
Karta możliwości projektanta agenta zarządzania definiuje zachowanie i funkcjonalność łącznika. Nie można zmodyfikować wybranych na tej karcie, gdy łącznik został utworzony. Ta tabela zawiera listę ustawień możliwości.

![Możliwości](./media/active-directory-aadconnectsync-connector-powershell/capabilities.png)

| Możliwości | Opis |
| --- | --- |
| [Styl nazwy wyróżniającej][dnstyle] |Wskazuje, jeśli łącznik obsługuje nazwy wyróżniającej, a jeśli tak, jakie stylu. |
| [Typ eksportu][exportT] |Określa typ obiektów, które są przedstawiane skrypt eksportu. <li>AttributeReplace — obejmuje pełny zestaw wartości dla atrybutu wielowartościowego zmiany atrybutu.</li><li>AttributeUpdate — obejmuje tylko delty do atrybutu wielowartościowego zmiany atrybutu.</li><li>MultivaluedReferenceAttributeUpdate — zawiera pełen zestaw wartości dla atrybutów wielowartościowych-reference i delty tylko dla atrybutów wielowartościowych odwołania.</li><li>ObjectReplace — zawiera wszystkie atrybuty obiektu, gdy dowolne zmiany atrybutów</li> |
| [Normalizacji danych][DataNorm] |Powoduje, że usługa synchronizacji do normalizacji atrybuty zakotwiczenia przed są udostępniane do skryptów. |
| [Potwierdzenie obiektu][oconf] |Określa zachowanie oczekującego importowania w usługi synchronizacji. <li>Normalny — domyślne zachowanie, która oczekuje wszystkich wyeksportowanych zmiany należy potwierdzić za pośrednictwem importu</li><li>NoDeleteConfirmation — obiekt jest usunięty, nie ma żadnych oczekującego importowania wygenerowany.</li><li>NoAddAndDeleteConfirmation — obiekt jest tworzony lub usunięty, nie ma żadnych oczekującego importowania wygenerowany.</li> |
| Użyj nazwy domeny jako swojej kotwicy |Jeśli styl nazwę wyróżniającą LDAP atrybutu zakotwiczenia przestrzeni łącznika jest również nazwę wyróżniającą. |
| Równoczesne wykonywanie operacji kilka łączników |Po zaznaczeniu tej opcji, jednocześnie uruchomić wiele łączników programu Windows PowerShell. |
| Partycje |Po zaznaczeniu tej opcji, łącznik obsługuje wiele partycji i odnajdywania partycji. |
| Hierarchii |Po zaznaczeniu tej opcji, łącznik obsługuje strukturę hierarchiczną styl LDAP. |
| Włącz importu |Po zaznaczeniu tej opcji, łącznik importuje dane za pomocą skryptów importu. |
| Włącz Import zmian |Po zaznaczeniu tej opcji, łącznik może wysłać żądanie delty ze skryptów importu. |
| Włącz eksportu |Po zaznaczeniu tej opcji, łącznik eksportuje danych za pomocą skryptów eksportu. |
| Włącz pełną eksportu |Po zaznaczeniu tej opcji, skrypty eksportu obsługę eksportowania łącznika całego miejsca. Aby użyć tej opcji, musi być także zaznaczone włączyć eksportu. |
| Brak wartości odwołania w pierwszej eksportu — dostęp próbny |Po zaznaczeniu tej opcji, atrybutów odwołania są eksportowane w drugim przebiegu eksportu. |
| Włącz zmiany nazwy obiektu |Po zaznaczeniu tej opcji, można zmodyfikować nazwy wyróżniającej. |
| Usuń Dodaj zastępowania |Po zaznaczeniu tej opcji, Usuń — Dodaj operacje są eksportowane jako jednego zastąpienia. |
| Włącz operacje na hasłach |Po zaznaczeniu tej opcji, skrypty synchronizacji hasła są obsługiwane. |
| Włącz hasło eksportu w pierwszej — dostęp próbny |Po zaznaczeniu tej opcji, ustawić podczas inicjowania obsługi haseł są eksportowane po utworzeniu obiektu. |

### <a name="global-parameters"></a>Parametry globalne
Karta parametry globalne w Projektancie agenta zarządzania można skonfigurować skrypty programu Windows PowerShell, które są uruchamiane przez łącznik. Można również skonfigurować wartości globalnych ustawień konfiguracji niestandardowej zdefiniowane na karcie połączenie.

**Odnajdywanie partycji**  
Partycja to oddzielne przestrzeni nazw w obrębie jednego udostępnionego schematu. W każdej domenie, na przykład w usłudze Active Directory jest partycji w obrębie jednego lasu. Partycja to logiczne grupowanie do importowania i eksportowania operacji. Importowanie i eksportowanie mieć partycji, które kontekst i wszystkie operacje odbywa się w tym kontekście. Partycje ma reprezentować hierarchii w protokole LDAP. Nazwa wyróżniająca partycji jest używany w imporcie można zweryfikować, że wszystkie zwracane obiekty znajdują się w zakresie partycji. Nazwa wyróżniająca partycji służy również podczas inicjowania obsługi administracyjnej z metaverse przestrzeni łącznika do określenia partycji, który obiekt powinna być skojarzona z podczas eksportowania.

Skrypt odnajdywania partycji otrzymuje następujące parametry z łącznika:

| Nazwa | Typ danych | Opis |
| --- | --- | --- |
| ConfigParameters |[KeyedCollection][keyk][ciągu [ConfigParameter][cp]] |Tabela parametry konfiguracji łącznika. |
| Poświadczenie |[PSCredential][pscred] |Zawiera wszystkie poświadczenia wprowadzane przez administratora, na karcie połączenie. |

Skrypt musi zwracać jedną [partycji] [ part] obiektu lub [T] listę obiektów partycji w potoku.

**Odnajdowanie hierarchii**  
Skrypt odnajdowania hierarchii jest używana tylko w przypadku możliwości styl nazwę wyróżniającą LDAP. Skrypt jest używany umożliwiają Przeglądaj i wybierz zestaw kontenerów, który jest uznawany za w lub poza zakresem dla importu i eksportu operacji. Skrypt powinien dostarczać wyłącznie listę węzłów, które są bezpośrednimi elementami podrzędnymi węzła głównego do skryptu.

Skrypt odnajdowania hierarchii otrzymuje następujące parametry z łącznika:

| Nazwa | Typ danych | Opis |
| --- | --- | --- |
| ConfigParameters |[KeyedCollection][keyk][ciągu [ConfigParameter][cp]] |Tabela parametry konfiguracji łącznika. |
| Poświadczenie |[PSCredential][pscred] |Zawiera wszystkie poświadczenia wprowadzane przez administratora, na karcie połączenie. |
| ParentNode |[HierarchyNode][hn] |Węzeł główny hierarchii, w którym skrypt powinien zwrócić bezpośrednich działań podrzędnych. |

Skrypt musi zwracać jedną obiektu HierarchyNode pojedynczy element potomny lub listę obiektów podrzędnych HierarchyNode [T] do potoku.

#### <a name="import"></a>Import
Łączniki, które obsługują operacji importowania musi implementować trzy skryptów.

**Rozpocznij importu**  
Skrypt importu begin jest uruchamiany na początku krok Uruchom importu. W tym kroku możesz nawiązać połączenie z systemu źródłowego i wykonaj czynności przygotowawcze przed importowaniem danych z połączenia systemu.

Skrypt importu begin otrzymuje następujące parametry z łącznika:

| Nazwa | Typ danych | Opis |
| --- | --- | --- |
| ConfigParameters |[KeyedCollection][keyk][ciągu [ConfigParameter][cp]] |Tabela parametry konfiguracji łącznika. |
| Poświadczenie |[PSCredential][pscred] |Zawiera wszystkie poświadczenia wprowadzane przez administratora, na karcie połączenie. |
| OpenImportConnectionRunStep |[OpenImportConnectionRunStep][oicrs] |Informuje o skryptu o typie partycji importu Uruchom (delta lub pełne), hierarchii, znak wodny i oczekiwany rozmiar. |
| Typy |[Schemat][schema] |Schemat dla przestrzeni łącznika, który jest importowany. |

Skrypt musi zwracać jedną [OpenImportConnectionResults] [ oicres] obiekt do potoku, na przykład:`Write-Output (New-Object Microsoft.MetadirectoryServices.OpenImportConnectionResults)`

**Importowanie danych**  
Skryptu importu danych jest wywoływana przez łącznik, dopóki skrypt wskazuje, że nie ma więcej danych do zaimportowania. Łącznik programu Windows PowerShell ma rozmiar strony 9999 obiektów. Jeśli skrypt zwraca więcej niż 9999 obiekty do importu, musi obsługiwać stronicowania. Ujawnia łącznika, gdy właściwość danych niestandardowych, której można się z magazynem znaku wodnego tak, aby zawsze skryptu importu danych zostanie wywołana, skrypt zostanie wznowione Importowanie obiektów, w którym przerwał.

Importuj dane skrypt otrzymuje następujące parametry z łącznika:

| Nazwa | Typ danych | Opis |
| --- | --- | --- |
| ConfigParameters |[KeyedCollection][keyk][ciągu [ConfigParameter][cp]] |Tabela parametry konfiguracji łącznika. |
| Poświadczenie |[PSCredential][pscred] |Zawiera wszystkie poświadczenia wprowadzane przez administratora, na karcie połączenie. |
| GetImportEntriesRunStep |[ImportRunStep][irs] |Zawiera znak wodny (CustomData) używanego podczas stronicowanej importów i importuje delta. |
| OpenImportConnectionRunStep |[OpenImportConnectionRunStep][oicrs] |Informuje o skryptu o typie partycji importu Uruchom (delta lub pełne), hierarchii, znak wodny i oczekiwany rozmiar. |
| Typy |[Schemat][schema] |Schemat dla przestrzeni łącznika, który jest importowany. |

Skrypt importu danych musi być zapisana listy [[CSEntryChange][csec]] obiektu do potoku. Ta kolekcja składa się z CSEntryChange atrybuty, które reprezentują każdy obiekt importowany. Podczas wykonywania pełnego importu tej kolekcji ma pełny zestaw CSEntryChange obiektów, które mają wszystkie atrybuty dla każdego obiektu. Podczas Delta zaimportować obiektu CSEntryChange albo powinna zawierać atrybut poziomu wystąpiły dla każdego obiektu do zaimportowania lub pełną reprezentacji obiektów, które zostały zmienione (tryb Zastąp).

**Importuj zakończenia**  
Po zakończeniu importowania, uruchom skrypt zakończenia importowania jest uruchamiany. Ten skrypt należy wykonać wszelkie Oczyszczanie zadań niezbędnych (na przykład, zamknij połączeń z systemami) i odpowiadać na awarie.

Końcu skryptu importu otrzymuje następujące parametry z łącznika:

| Nazwa | Typ danych | Opis |
| --- | --- | --- |
| ConfigParameters |[KeyedCollection][keyk][ciągu [ConfigParameter][cp]] |Tabela parametry konfiguracji łącznika. |
| Poświadczenie |[PSCredential][pscred] |Zawiera wszystkie poświadczenia wprowadzane przez administratora, na karcie połączenie. |
| OpenImportConnectionRunStep |[OpenImportConnectionRunStep][oicrs] |Informuje o skryptu o typie partycji importu Uruchom (delta lub pełne), hierarchii, znak wodny i oczekiwany rozmiar. |
| CloseImportConnectionRunStep |[CloseImportConnectionRunStep][cecrs] |Informuje o skrypt o przyczyny, dla której importowania została zakończona. |

Skrypt musi zwracać jedną [CloseImportConnectionResults] [ cicres] obiekt do potoku, na przykład:`Write-Output (New-Object Microsoft.MetadirectoryServices.CloseImportConnectionResults)`

#### <a name="export"></a>Eksportowanie
Taki sam jak architektura importu łącznika, łączniki, które obsługują eksportu musi implementować trzy skryptów.

**Rozpoczęcie eksportu**  
Skrypt eksportu begin jest uruchamiane na początku etap uruchamiania eksportu. W tym kroku możesz nawiązać połączenie z systemu źródłowego i przeprowadzanie żadnych czynności przygotowawczych przed wyeksportowaniem danych do system połączony.

Skrypt eksportu begin otrzymuje następujące parametry z łącznika:

| Nazwa | Typ danych | Opis |
| --- | --- | --- |
| ConfigParameters |[KeyedCollection][keyk][ciągu [ConfigParameter][cp]] |Tabela parametry konfiguracji łącznika. |
| Poświadczenie |[PSCredential][pscred] |Zawiera wszystkie poświadczenia wprowadzane przez administratora, na karcie połączenie. |
| OpenExportConnectionRunStep |[OpenExportConnectionRunStep][oecrs] |Informuje o skryptu o typie partycji uruchamiania eksportu (delta lub pełne), hierarchii i oczekiwany rozmiar. |
| Typy |[Schemat][schema] |Schemat dla przestrzeni łącznika, który został wyeksportowany. |

Skrypt nie może zwracać żadnych danych wyjściowych do potoku.

**Eksportowanie danych**  
Usługa synchronizacji wywołuje skrypt eksportowanie danych tyle razy, ile jest konieczny do przetworzenia wszystkich oczekujących operacji eksportowania. Jeśli przestrzeni łącznika ma oczekujące operacje eksportowania więcej niż rozmiar strony łącznika, skrypt eksportu danych może zostać wywołana wiele razy i prawdopodobnie wiele razy dla tego samego obiektu.

Skrypt eksportu danych otrzymuje następujące parametry z łącznika:

| Nazwa | Typ danych | Opis |
| --- | --- | --- |
| ConfigParameters |[KeyedCollection][keyk][ciągu [ConfigParameter][cp]] |Tabela parametry konfiguracji łącznika. |
| Poświadczenie |[PSCredential][pscred] |Zawiera wszystkie poświadczenia wprowadzane przez administratora, na karcie połączenie. |
| CSEntries |IList[CSEntryChange][csec] |Lista przestrzeni łącznika obiekty z Oczekujące operacje eksportu do przetworzenia w trakcie tego przebiegu. |
| OpenExportConnectionRunStep |[OpenExportConnectionRunStep][oecrs] |Informuje o skryptu o typie partycji uruchamiania eksportu (delta lub pełne), hierarchii i oczekiwany rozmiar. |
| Typy |[Schemat][schema] |Schemat dla przestrzeni łącznika, który został wyeksportowany. |

Skrypt eksportu danych musi zwracać [PutExportEntriesResults] [ peeres] obiektu do potoku. Ten obiekt nie musi obejmować informacje wyników dla każdego wyeksportowanego łącznika, o ile nie wystąpił błąd lub zmianę atrybutu zakotwiczenia. Na przykład, aby zwrócić obiekt PutExportEntriesResults do potoku:`Write-Output (New-Object Microsoft.MetadirectoryServices.PutExportEntriesResults)`

**Końcowy eksportu**  
Po zakończeniu eksportu uruchomić, działanie skryptu zakończenia eksportowania. Ten skrypt należy wykonać wszelkie Oczyszczanie zadań niezbędnych (na przykład, zamknij połączeń z systemami) i odpowiadać na awarie.

Skrypt eksportu końcowy otrzymuje następujące parametry z łącznika:

| Nazwa | Typ danych | Opis |
| --- | --- | --- |
| ConfigParameters |[KeyedCollection][keyk][ciągu [ConfigParameter][cp]] |Tabela parametry konfiguracji łącznika. |
| Poświadczenie |[PSCredential][pscred] |Zawiera wszystkie poświadczenia wprowadzane przez administratora, na karcie połączenie. |
| OpenExportConnectionRunStep |[OpenExportConnectionRunStep][oecrs] |Informuje o skryptu o typie partycji uruchamiania eksportu (delta lub pełne), hierarchii i oczekiwany rozmiar. |
| CloseExportConnectionRunStep |[CloseExportConnectionRunStep][cecrs] |Informuje o skrypt o przyczyny, dla której eksport został zakończony. |

Skrypt nie może zwracać żadnych danych wyjściowych do potoku.

#### <a name="password-synchronization"></a>Synchronizacja haseł
Łączniki programu Windows PowerShell może służyć jako miejsce docelowe dotyczących resetowania zmiany hasła.

Skrypt hasła otrzymuje następujące parametry z łącznika:

| Nazwa | Typ danych | Opis |
| --- | --- | --- |
| ConfigParameters |[KeyedCollection][keyk][ciągu [ConfigParameter][cp]] |Tabela parametry konfiguracji łącznika. |
| Poświadczenie |[PSCredential][pscred] |Zawiera wszystkie poświadczenia wprowadzane przez administratora, na karcie połączenie. |
| Partycja |[Partycji][part] |Partycji katalogu, który CSEntry znajduje się w. |
| CSEntry |[CSEntry][cse] |Wpis obszaru łącznika dla obiektu, który otrzymał zmiany hasła lub zresetować. |
| Typ operacji |Ciąg |Wskazuje, czy operacja jest zresetowanie (**UstawianieHasła**) lub zmiany (**Element ChangePassword**). |
| PasswordOptions |[PasswordOptions][pwdopt] |Flagi określające, hasło danego zresetować zachowanie. Ten parametr jest dostępna tylko jeśli jest OperationType **UstawianieHasła**. |
| Stare_hasło |Ciąg |Wypełniane przy użyciu obiektu stare hasło do zmiany hasła. Ten parametr jest dostępna tylko jeśli jest OperationType **Element ChangePassword**. |
| NoweHasło |Ciąg |Wypełniane przy użyciu obiektu nowego hasła, który powinien zostać skonfigurowany skryptu. |

Skrypt hasło ma nie zwróciła żadnych wyników do potoku środowiska Windows PowerShell. Jeśli wystąpi błąd w skrypcie hasła, skrypt powinien zgłosić jedną następujące wyjątki, aby poinformować usługę synchronizacji o problemie:

* [PasswordPolicyViolationException] [ pwdex1] — wygenerowany, jeśli hasło nie spełnia zasad haseł w system połączony.
* [PasswordIllFormedException] [ pwdex2] — wygenerowany, jeśli hasło nie jest możliwa do system połączony.
* [PasswordExtension] [ pwdex3] — wyjątek dla wszystkich błędów w skrypcie hasła.

## <a name="sample-connectors"></a>Łączniki próbki
Pełny przegląd łączniki przykładowe dostępne dla [Windows PowerShell łącznika próbki łącznika kolekcji][samp].

## <a name="other-notes"></a>Inne uwagi
### <a name="additional-configuration-for-impersonation"></a>Dodatkowe konfiguracje dla personifikacji
Przyznaj się, że użytkownik, który jest spersonifikować następujące uprawnienia na serwerze usługi synchronizacji:

Dostęp do odczytu do następujących kluczy rejestru:

* HKEY_USERS\\\Software\Microsoft\PowerShell [SynchronizationServiceServiceAccountSID]
* HKEY_USERS\\\Environment [SynchronizationServiceServiceAccountSID]

Aby ustalić identyfikatora zabezpieczeń (SID) konta usługi synchronizacji usługi, uruchom następujące polecenia programu PowerShell:

```
$account = New-Object System.Security.Principal.NTAccount "<domain>\<username>"
$account.Translate([System.Security.Principal.SecurityIdentifier]).Value
```

Dostęp do odczytu do następujących folderów systemu plików:

* %ProgramFiles%\Microsoft forefront Identity Manager\2010\Synchronization Service\Extensions
* %ProgramFiles%\Microsoft forefront Identity Manager\2010\Synchronization Service\ExtensionsCache
* %ProgramFiles%\Microsoft forefront Identity Manager\2010\Synchronization Service\MaData\\{łącznik}

Zastąp symbol zastępczy {łącznik} nazwę łącznika programu Windows PowerShell.

## <a name="troubleshooting"></a>Rozwiązywanie problemów
* Aby uzyskać informacje o tym, jak włączyć rejestrowanie rozwiązywać problemy z łącznika, zobacz [jak włączyć ETW Tracing łączników](http://go.microsoft.com/fwlink/?LinkId=335731).

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[cpp]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.configparameterpage.aspx
[keyk]: https://msdn.microsoft.com/library/ms132438.aspx
[cp]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.configparameter.aspx
[pscred]: https://msdn.microsoft.com/library/system.management.automation.pscredential.aspx
[schema]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.schema.aspx
[schemaT]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.schematype.aspx
[schemaA]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.schemaattribute.aspx
[dnstyle]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.madistinguishednamestyle.aspx
[exportT]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.maexporttype.aspx
[DataNorm]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.manormalizations.aspx
[oconf]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.maobjectconfirmation.aspx
[dw]: https://msdn.microsoft.com/library/windows/desktop/aa378184.aspx
[part]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.partition.aspx
[hn]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.hierarchynode.aspx
[oicrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.openimportconnectionrunstep.aspx
[cecrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.closeexportconnectionrunstep.aspx
[oicres]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.openimportconnectionresults.aspx
[cecrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.closeexportconnectionrunstep.aspx
[cicres]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.closeimportconnectionresults.aspx
[oecrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.openexportconnectionrunstep.aspx
[irs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.importrunstep.aspx
[cse]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.csentry.aspx
[csec]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.csentrychange.aspx
[peeres]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.putexportentriesresults.aspx
[pwdopt]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordoptions.aspx
[pwdex1]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordpolicyviolationexception.aspx
[pwdex2]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordillformedexception.aspx
[pwdex3]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordextensionexception.aspx
[samp]: http://go.microsoft.com/fwlink/?LinkId=394291
