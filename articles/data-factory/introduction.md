---
title: "Wprowadzenie do usługi Azure Data Factory | Microsoft Docs"
description: "Dowiedz się więcej o usłudze Azure Data Factory, czyli usłudze integracji danych w chmurze, która służy do aranżacji i automatyzacji przenoszenia i przekształcania danych."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/29/2017
ms.author: shlo
ms.openlocfilehash: 58e141498ed5cbaa110622d949a9627c98827ac3
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/01/2017
---
# <a name="introduction-to-azure-data-factory"></a>Wprowadzenie do usługi Azure Data Factory
 
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Wersja 1 — ogólnie dostępna](v1/data-factory-introduction.md)
> * [Wersja 2 — wersja zapoznawcza](introduction.md)

W świecie danych big data dane nieprzetworzone i niezorganizowane często są przechowywane w relacyjnych, nierelacyjnych i innych systemach magazynowania. Jednak same nieprzetworzone dane nie mają prawidłowego kontekstu ani znaczenia umożliwiającego zapewnienie istotnych informacji analitykom, specjalistom z zakresu danych i osobom podejmującym decyzje biznesowe. 

Dane big data wymagają usługi, która umożliwia organizację i operacjonalizację procesów mających na celu przekształcenie tych ogromnych zbiorów nieprzetworzonych danych w praktyczną analizę biznesową. Azure Data Factory to zarządzana usługa w chmurze, stworzona z myślą o tych kompleksowych, hybrydowych projektach typu wyodrębnianie-transformacja-ładowanie (ETL), wyodrębnianie-ładowanie-transformacja (ELT) i integracji danych.

Na przykład wyobraź sobie producenta gier, który zbiera petabajty dzienników gier tworzonych przez gry w chmurze. Firma chce analizować te dzienniki w celu uzyskania szczegółowych informacji dotyczących preferencji klientów, danych demograficznych oraz zachowania użytkowników podczas korzystania z gier. Dzięki temu firma będzie mogła identyfikować możliwości sprzedaży droższych produktów i sprzedaży wiązanej, opracowywać nowe atrakcyjne funkcje pozytywnie wpływające na rozwój biznesowy oraz oferować klientom udoskonalone środowisko gier.

Do analizy tych dzienników firma musi użyć danych referencyjnych, np. informacji o kliencie, grze i kampanii marketingowej, które znajdują się w lokalnym magazynie danych. Firma chce korzystać z tych danych z lokalnego magazynu danych, łącząc je z dodatkowymi danymi dzienników znajdującymi się w magazynie danych w chmurze. 

Aby wyodrębnić szczegółowe informacje, chce przetworzyć połączone dane przy użyciu klastra Spark w chmurze (Azure HDInsight), a następnie opublikować przekształcone dane w magazynie danych w chmurze, takim jak Azure SQL Data Warehouse, co ułatwi tworzenie raportów na ich podstawie. Firma zamierza zautomatyzować przepływ pracy, a także monitorować go i zarządzać nim w cyklu dziennym. Poza tym chce wykonywać przepływ pracy, gdy pliki znajdą się w kontenerze usługi Blob Storage.

Azure Data Factory to platforma, która umożliwia pracę z takimi scenariuszami danych. Jest to *oparta na chmurze usługa integracji danych, za pomocą której można tworzyć oparte na danych przepływy pracy w chmurze służące do organizowania i automatyzowania przenoszenia i przekształcania danych*. Za pomocą usługi Azure Data Factory można tworzyć oparte na danych przepływy pracy (nazywane potokami) i ustalać ich harmonogram. Te przepływy mogą pozyskiwać dane z różnych magazynów danych, przetwarzać i przekształcać je za pomocą usług obliczeniowych, takich jak Azure HDInsight Hadoop, Spark, Azure Data Lake Analytics i Azure Machine Learning. 

Ponadto umożliwiają publikowanie danych wyjściowych w magazynach danych, np. Azure SQL Data Warehouse, w celu użycia przez aplikacje analizy biznesowej. Ostatecznie dzięki usłudze Azure Data Factory nieprzetworzone dane mogą być organizowane w praktyczne magazyny i usługi data lake ułatwiające podejmowanie właściwych decyzji biznesowych.

![Widok najwyższego poziomu usługi Data Factory](media/introduction/big-picture.png)

> [!NOTE]
> Ten artykuł dotyczy wersji 2 usługi Data Factory, która jest obecnie dostępna w wersji zapoznawczej. Jeśli używasz dostępnej ogólnie wersji 1 usługi Data Factory, zobacz [Introduction to Data Factory version 1](v1/data-factory-introduction.md) (Wprowadzenie do usługi Data Factory w wersji 1).

## <a name="how-does-it-work"></a>Jak to działa?
Potoki (oparte na danych przepływy pracy) w usłudze Azure Data Factory zwykle wykonują następujące cztery kroki:

![Cztery kroki przepływu pracy opartego na danych](media/introduction/four-steps-of-a-workflow.png)

### <a name="connect-and-collect"></a>Łączenie i zbieranie

Przedsiębiorstwa dysponują danymi różnych typów, znajdującymi się w różnych magazynach lokalnych i w chmurze, ze strukturą pełną i częściową, przychodzącymi w różnych interwałach i w różnym tempie. 

Pierwszy krok tworzenia systemu uzyskiwania informacji polega na połączeniu wszystkich wymaganych źródeł danych i systemów przetwarzania, takich jak usługi SaaS, bazy danych, udziały plików oraz internetowe usługi FTP. Następnym krokiem jest przeniesienie danych w miarę potrzeb do centralnej lokalizacji w celu ich dalszego przetwarzania.

Firmy, które nie korzystają z usługi Data Factory, muszą tworzyć niestandardowe składniki umożliwiające przepływ danych lub projektować własne usługi, aby zintegrować źródła danych i systemy przetwarzania. Takie podejście jest kosztowne, a integracja i utrzymanie systemów stwarza trudności. Ponadto rozwiązanie to rzadko zapewnia mechanizmy kontrolne oraz funkcje monitorowania i wysyłania alertów na poziomie korporacyjnym. Możliwości te są natomiast dostępne w przypadku w pełni zarządzanej usługi.

Usługa Data Factory udostępnia [działanie kopiowania](copy-activity-overview.md) w potoku danych, które pozwala przenosić dane z lokalnych magazynów danych i źródeł danych w chmurze do centralnego magazynu danych w chmurze w celu przeprowadzenia kolejnych etapów analizy. Na przykład można pobrać dane z usługi Azure Data Lake Store i przekształcić je później za pomocą usługi obliczeniowej Azure Data Lake Analytics. Można też pobrać dane z usługi Azure Blob Storage, aby przekształcić je później przy użyciu klastra usługi Azure HDInsight na platformie Hadoop.

### <a name="transform-and-enrich"></a>Przekształcanie i wzbogacanie
Po umieszczeniu danych w centralnym magazynie danych w chmurze zebrane dane należy przetworzyć lub przekształcić za pomocą usług obliczeniowych, takich jak HDInsight Hadoop, Spark, Data Lake Analytics i Machine Learning. Przekształcone dane powinny być generowane niezawodnie i zgodnie z możliwym do utrzymania i kontrolowania harmonogramem, aby zapewnić środowiskom produkcyjnym zaufane dane.

### <a name="publish"></a>Publikowanie
Po przekształceniu danych pierwotnych do postaci gotowej dla działań biznesowych należy załadować je do magazynu Azure Data Warehouse, bazy danych Azure SQL Database, bazy danych Azure CosmosDB lub dowolnego aparatu analizy, z którego mogą korzystać użytkownicy biznesowi przy użyciu swoich narzędzi do analizy biznesowej.

### <a name="monitor"></a>Monitorowanie
Po pomyślnym utworzeniu i wdrożeniu potoku integracji danych, zapewniającego wartość biznesową danym przetworzonym, należy monitorować zaplanowane działania i potoki pod kątem współczynników powodzenia i niepowodzenia. Usługa Azure Data Factory dysponuje wbudowaną obsługą monitorowania potoków przy użyciu narzędzi Azure Monitor, API, PowerShell, Microsoft Operations Management Suite i paneli kondycji w witrynie Azure Portal.

## <a name="whats-different-in-version-2"></a>Czym się różni wersja 2?
Usługa Azure Data Factory w wersji 2 opiera się na oryginalnej usłudze przenoszenia i przekształcania danych Azure Data Factory, obsługując jednocześnie większy zestaw scenariuszy integracji danych w chmurze. Usługa Azure Data Factory w wersji 2 oferuje następujące możliwości:

- Sterowanie przepływem i skalą
- Wdrażanie i uruchamianie pakietów usługi SQL Server Integration Services (SSIS) na platformie Azure

Po wydaniu wersji 1 dowiedzieliśmy się, że klienci muszą projektować kompleksowe scenariusze integracji danych hybrydowych, które wymagają przenoszenia i przetwarzania danych w chmurze, lokalnie i na maszynach wirtualnych w chmurze. Wymagania te wprowadziły potrzebę przekazywania i przetwarzania danych w bezpiecznych środowiskach sieci wirtualnych oraz ich skalowania wraz z mocą obliczeniową na żądanie.

Potoki danych stały się kluczową częścią analityki biznesowej, w związku z czym dostrzegliśmy, że te krytyczne działania związane z danymi wymagają elastycznych harmonogramów, obsługujących przyrostowe obciążenia danych oraz uruchamianie wyzwalane przez zdarzenia. Co więcej, wraz ze wzrostem złożoności tych operacji rosną także wymagania wobec usługi, dotyczące obsługi typowych paradygmatów przepływów pracy, w tym gałęzi, pętli i przetwarzania warunkowego.

Wersja 2 umożliwia migrowanie istniejących pakietów SSIS do chmury. Nowa funkcja „Integration Runtimes” pozwala na przeniesienie pakietu SSIS w postaci usługi platformy Azure zarządzanej przez usługę ADF. Wprowadzenie funkcji SSIS IR w wersji 2 umożliwia wykonywanie, monitorowanie i tworzenie pakietów SSIS oraz zarządzanie nimi w chmurze.

### <a name="control-flow-and-scale"></a>Sterowanie przepływem i skalą 
W celu zapewnienia obsługi różnych przepływów i wzorców integracji w nowoczesnym magazynie danych, usługa Data Factory wprowadziła nowy, elastyczny model potoku danych, który nie jest już powiązany z danymi szeregów czasowych. Ta wersja umożliwia modelowanie zdarzeń warunkowych oraz tworzenie gałęzi w przepływie sterowania potoku danych, jak również jawne przekazywanie parametrów wewnątrz i między tymi przepływami.

Teraz możesz utworzyć dowolny styl przepływu wymagany przez integrację danych oraz uruchamiać go na żądanie lub w sposób powtarzalny, zgodnie z harmonogramem. Kilka typowych przepływów dostępnych obecnie, które wcześniej nie były możliwe, to:   

- Przepływ sterowania:
    - Łączenie działań w sekwencje w ramach potoku
    - Rozgałęzianie działań w ramach potoku
    - Parametry
        - Parametry można definiować na poziomie potoku, a argumenty przekazywać w trakcie wywoływania potoku na żądanie lub przy użyciu wyzwalacza.
        - Działania mogą wykorzystywać argumenty przekazywane do potoku.
    - Przekazywanie stanów niestandardowych
        - Dane wyjściowe działania, w tym jego stan, mogą być wykorzystywane przez kolejne działania w potoku.
    - Tworzenie pętli kontenerów
        - Dla każdego 
- Przepływy na podstawie wyzwalaczy
    - Potoki mogą być wywoływane na żądanie lub zgodnie z zegarem.
- Przepływy delta
    - Parametry umożliwiają zdefiniowanie górnego limitu dla kopii delta podczas przenoszenia tabel wymiarów i referencyjnych z magazynu relacyjnego, lokalnie i w chmurze, w celu załadowania danych do usługi Data Lake. 

Więcej informacji znajdziesz w artykule [Rozgałęzianie działań i tworzenie łańcuchów działań w potoku usługi Data Factory](tutorial-control-flow.md).

### <a name="deploy-ssis-packages-to-azure"></a>Wdrażanie pakietów usług SSIS na platformie Azure 
Jeśli chcesz przenieść obciążenia SSIS, możesz utworzyć fabrykę danych w wersji 2 i zainicjować usługę Azure-SSIS Integration Runtime (IR). Azure-SSIS IR to w pełni zarządzany klaster maszyn wirtualnych platformy Azure (węzłów), których przeznaczeniem jest uruchamianie pakietów SSIS w chmurze. Instrukcje krok po kroku znajdują się w samouczku [Wdrażanie pakietów usług SQL Server Integration Services na platformie Azure](tutorial-deploy-ssis-packages-azure.md). 
 

### <a name="sdks"></a>Zestawy SDK
Jeśli jesteś użytkownikiem zaawansowanym i szukasz interfejsu programowego, wersja 2 oferuje bogaty pakiet zestawów SDK, które umożliwiają tworzenie i monitorowanie potoków oraz zarządzanie nimi przy użyciu ulubionego środowiska IDE.

- *.NET SDK*: zestaw .NET SDK został zaktualizowany do wersji 2. 
- *PowerShell*: polecenia cmdlet programu PowerShell zostały zaktualizowane do wersji 2. Polecenia cmdlet w wersji 2 mają w nazwie **DataFactoryV2**. Na przykład: Get-AzureRmDataFactoryV2. 
- *Python SDK*: ten zestaw SDK jest nowym składnikiem w wersji 2.
- *REST API*: interfejs API REST został zaktualizowany w wersji 2.  

Zestawy SDK, które zostały zaktualizowane w wersji 2, nie są zgodne wstecz z klientami w wersji 1. 

### <a name="monitoring"></a>Monitorowanie
Obecnie wersja 2 obsługuje monitorowanie fabryk danych przy użyciu samych zestawów SDK. Portal nie obsługuje jeszcze monitorowania fabryk danych w wersji 2. 

## <a name="load-the-data-into-a-lake"></a>Ładowanie danych do usługi data lake
Usługa Data Factory oferuje ponad 30 łączników umożliwiających ładowanie danych ze środowisk hybrydowych i heterogenicznych na platformę Azure. [Przewodnik dotyczący wydajności i dostosowywania](copy-activity-performance.md) zawiera najnowsze wyniki dotyczące wydajności pochodzące z testów wewnętrznych oraz wskazówki dotyczące dostosowywania. 

Ostatnio wprowadziliśmy również wysoką dostępność i skalowalność w ramach samodzielnie hostowanego produktu Integration Runtime instalowanego w prywatnym środowisku sieciowym. Naszym celem było spełnienie wymagań dużych przedsiębiorców (warstwa 1) dotyczących lepszej dostępności i skalowalności.

## <a name="top-level-concepts-in-version-2"></a>Koncepcje najwyższego poziomu w wersji 2
Subskrypcja platformy Azure może zawierać jedno lub więcej wystąpień usługi Azure Data Factory (lub fabryk danych). Usługa Azure Data Factory składa się z czterech głównych składników. Ich współdziałanie pozwala udostępnić platformę umożliwiającą tworzenie opartych na danych przepływów pracy wraz z etapami służącymi do przenoszenia i przekształcania danych.

### <a name="pipeline"></a>Potok
Fabryka danych może obejmować jeden lub wiele potoków. Potoki to logiczne grupy działań, które wspólnie wykonują pracę. które umożliwiają wykonanie zadania. Na przykład potok może zawierać grupę działań, które pozwalają pozyskać dane z obiektu blob platformy Azure, a następnie uruchomić zapytanie programu Hive w klastrze usługi HDInsight w celu podzielenia danych. 

Zaletą korzystania z potoku jest możliwość zarządzania zestawem działań, a nie pojedynczymi zadaniami. Działania w potoku można ze sobą połączyć w sposób sekwencyjny, mogą też działać niezależnie i równolegle.

### <a name="activity"></a>Działanie
Działania reprezentują krok przetwarzania w potoku. Można na przykład użyć działania kopiowania w celu skopiowania danych z jednego magazynu danych do drugiego. Podobnie można użyć działania programu Hive, które uruchomi zapytanie programu Hive w klastrze usługi Azure HDInsight, aby przekształcić lub przeanalizować dane. Usługa Data Factory obsługuje trzy typy działań: działania przenoszenia danych, działania przekształcania danych i działania sterowania.

### <a name="datasets"></a>Zestawy danych
Zestawy danych reprezentują struktury w magazynach danych. Struktury te po prostu wskazują na dane, które mają być używane w działaniach jako dane wejściowe lub wyjściowe. 

### <a name="linked-services"></a>Połączone usługi
Połączone usługi działają podobnie do parametrów połączenia, umożliwiając definiowanie informacji wymaganych przez usługę Data Factory do nawiązywania połączeń z zasobami zewnętrznymi. Mechanizm ten działa następująco: połączona usługa zawiera definicję połączenia ze źródłem danych, a zestaw danych reprezentuje strukturę danych. Na przykład połączona usługa Azure Storage określa parametry połączenia, które umożliwiają połączenie z kontem usługi Azure Storage. Natomiast zestaw danych usługi Azure Blob określa kontener obiektów blob oraz folder, który zawiera dane.

Połączone usługi w usłudze Fabryka danych służą do dwóch celów:

- Reprezentowanie **magazynu danych**, w tym między innymi bazy danych na lokalnym serwerze SQL Server, bazy danych Oracle, udziału plików lub konta usługi Azure Blob Storage. Listę obsługiwanych magazynów danych zamieszczono w artykule na temat [działania kopiowania](copy-activity-overview.md).

- Reprezentowanie **zasobu obliczeniowego**, który może hostować wykonywanie działania. Na przykład działanie HDInsightHive jest wykonywane w klastrze HDInsight na platformie Hadoop. Listę działań przekształcania i obsługiwanych środowisk obliczeniowych zamieszczono w artykule dotyczącym [przekształcania danych](transform-data.md).

### <a name="triggers"></a>Wyzwalacze
Wyzwalacze reprezentują jednostki przetwarzania, które określają, kiedy należy rozpocząć wykonanie potoku. Istnieją różne typy wyzwalaczy dla różnych typów zdarzeń. W wersji zapoznawczej obsługiwany jest wyzwalacz zaplanowany zgodnie z zegarem. 


### <a name="pipeline-runs"></a>Uruchomienia potoków
Uruchomienie potoku to wystąpienie wykonania potoku. Uruchomienia potoku są tworzone zazwyczaj przez przekazanie argumentów do parametrów zdefiniowanych w potokach. Argumenty można przekazać ręcznie lub w ramach definicji wyzwalacza.

### <a name="parameters"></a>Parametry
Parametry to pary klucz-wartość w konfiguracji tylko do odczytu.  Parametry są definiowane w potoku. Argumenty dla zdefiniowanych parametrów są przekazywane w trakcie wykonania z kontekstu uruchomienia utworzonego przez wyzwalacz lub potok wykonany ręcznie. Działania w ramach potoku wykorzystują wartości parametrów.

Zestaw danych to silnie typizowany parametr oraz obiekt wielokrotnego użytku, do którego można się odwoływać. Działanie może odwoływać się do zestawów danych oraz wykorzystywać właściwości określone w definicji zestawu danych.

Połączona usługa to również silnie typizowany parametr zawierający informacje o połączeniu z magazynem danych lub środowiskiem obliczeniowym. Również jest to obiekt wielokrotnego użytku, do którego można się odwoływać.

### <a name="control-flow"></a>Przepływ sterowania
Przepływ sterowania to inaczej organizacja działań potoku, która obejmuje działania połączone w sekwencję, rozgałęzienia, parametry zdefiniowane na poziomie potoku oraz argumenty przekazywane podczas wywoływania potoku na żądanie lub przy użyciu wyzwalacza. Zawiera również kontenery przekazywania stanów niestandardowych i pętli, czyli iteratory For-each.


Aby uzyskać więcej informacji o pojęciach związanych z usługą Data Factory, zobacz następujące artykuły:

- [Dataset and linked services (Zestaw danych i połączone usługi)](concepts-datasets-linked-services.md)
- [Potoki i działania](concepts-pipelines-activities.md)
- [Integration Runtime (Produkt Integration Runtime)](concepts-integration-runtime.md)

## <a name="supported-regions"></a>Obsługiwane regiony

Obecnie fabryki danych można tworzyć w regionach Wschodnie stany USA oraz Wschodnie stany USA 2. Jednak fabryka danych może mieć dostęp do magazynów danych i usług obliczeniowych w innych regionach świadczenia usługi Azure, aby przenosić dane między magazynami danych lub przetwarzać dane przy użyciu usług obliczeniowych.

Sama usługa Azure Data Factory nie przechowuje żadnych danych. Usługa umożliwia tworzenie opartych na danych przepływów pracy do aranżowania przenoszenia danych między obsługiwanymi magazynami danych oraz przetwarzania danych przy użyciu usług obliczeniowych w innych regionach lub w środowisku lokalnym. Umożliwia także monitorowanie przepływów pracy i zarządzanie nimi przy użyciu zarówno mechanizmów programowych, jak i interfejsu użytkownika.

Chociaż usługa Data Factory jest dostępna tylko w regionach Wschodnie stany USA i Wschodnie stany USA 2, to usługa zapewniająca przenoszenie danych w usłudze Data Factory jest dostępna globalnie w kilku regionach. Jeśli magazyn danych znajduje się za zaporą, dane są przenoszone przez bramę zarządzania danymi zainstalowaną w środowisku lokalnym.

Załóżmy na przykład, że środowiska obliczeniowe, takie jak klaster usługi Azure HDInsight i usługa Azure Machine Learning, są uruchamiane z regionu Europa Zachodnia. Można utworzyć i wykorzystać wystąpienie usługi Azure Data Factory w regionie Europa Północna oraz użyć go do planowania zadań wykonywanych w środowiskach obliczeniowych w regionie Europa Zachodnia. Wyzwolenie zadania w środowisku obliczeniowym zajmuje usłudze Data Factory kilka milisekund, ale czas uruchomienia zadania w środowisku obliczeniowym nie zmienia się.

## <a name="next-steps"></a>Następne kroki
Dowiedz się, jak utworzyć fabrykę danych, wykonując instrukcje krok po kroku zamieszczone w przewodnikach Szybki start dotyczących [programu PowerShell](quickstart-create-data-factory-powershell.md), [platformy .NET](quickstart-create-data-factory-dot-net.md), [języka Python](quickstart-create-data-factory-python.md), [interfejsu API REST](quickstart-create-data-factory-rest-api.md) i witryny Azure Portal. 
