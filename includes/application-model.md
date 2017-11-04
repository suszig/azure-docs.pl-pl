# <a name="compute"></a>Wystąpienia obliczeniowe
Platforma Azure umożliwia wdrażanie i monitorowanie kod aplikacji uruchomionych w centrum danych firmy Microsoft. Usługa hostowana podczas tworzenia aplikacji i uruchom go na platformie Azure, kod i konfiguracja łącznie nosi nazwę platformy Azure. Usługi hostowanej są łatwe w zarządzaniu, skalowanie w górę i w dół, skonfiguruj ponownie i aktualizuje do nowych wersji kodu aplikacji. Ten artykuł skupia się na platformie Azure, hostowana usługa modelu aplikacji.<a id="compare" name="compare"></a>

## Spis treści<a id="_GoBack" name="_GoBack"></a>
* [Zalety modelu aplikacji Azure][Azure Application Model Benefits]
* [Usługa hostowana podstawowe koncepcje][Hosted Service Core Concepts]
* [Zagadnienia dotyczące projektowania usług hostingowych][Hosted Service Design Considerations]
* [Projektowanie aplikacji w celu skalowania][Designing your Application for Scale]
* [Definicja usługi hostowanej i konfiguracji][Hosted Service Definition and Configuration]
* [Plik definicji usługi][The Service Definition File]
* [Plik konfiguracji usługi][The Service Configuration File]
* [Tworzenie i wdrażanie usług hostingowych][Creating and Deploying a Hosted Service]
* [Odwołania][References]

## <a id="benefits"></a>Korzyści modelu aplikacji azure
Podczas wdrażania aplikacji jako usługi hostowanej platformy Azure tworzy jeden lub więcej maszynach wirtualnych (VM zawierające kod aplikacji) i maszyn wirtualnych jest uruchamiany na komputerach fizycznych znajdujących się w jednym z centrum danych Azure. Żądania klientów do aplikacji hostowanej wprowadzania centrum danych, modułu równoważenia obciążenia dystrybuuje te żądania do maszyn wirtualnych. Gdy aplikacja jest hostowana na platformie Azure, pobiera trzy kluczowe korzyści:

* **Wysoka dostępność.** Wysoka dostępność oznacza, że Azure gwarantuje, że aplikacja działa jak to możliwe i jest w stanie odpowiadać na żądania klientów. Jeśli kończy aplikacji (z powodu nieobsługiwanego wyjątku, na przykład), a następnie Azure wykryje to i będzie ono automatycznie ponownie uruchomić aplikację. Jeśli komputer aplikacja działa w ramach awaria sprzętu, następnie Azure również wykryje to i automatycznie utworzyć nową maszynę Wirtualną na innym komputerze fizycznym pracy i uruchomić kod z tego miejsca. Uwaga: Aby Twojej aplikacji w celu uzyskania umową dotyczącą poziomu usług firmy Microsoft dostępnych w poziomie 99,95%, musi mieć co najmniej dwie maszyny wirtualne z systemami kod aplikacji. Dzięki temu jedną maszynę Wirtualną do przetworzenia żądania klientów podczas Azure przenosi kodu z niepowodzeniem maszyny Wirtualnej do nowego, dobrym maszyny Wirtualnej.
* **Skalowalność.** Azure pozwala łatwo i dynamicznie zmieniać liczbę maszyn wirtualnych uruchamiania kodu aplikacji do obsługi rzeczywiste obciążenie na aplikację. Dzięki temu można dostosować aplikację na obciążenia klientów są umieszczenie na niej podczas płatności tylko dla maszyn wirtualnych należy, jeśli są potrzebne. Jeśli chcesz zmienić liczbę maszyn wirtualnych Azure odpowiada w ciągu minut, dzięki czemu można dynamicznie zmieniać liczbę maszyn wirtualnych z systemem tak często, zgodnie z potrzebami.
* **Możliwości zarządzania.** Ponieważ Azure to platforma jako usługa (PaaS) oferty, zarządza infrastruktury (samym sprzęcie, energii elektrycznej i sieci) wymagane do prawidłowego tych komputerów z systemem. Azure zarządza także platformy, zapewniając aktualności systemu operacyjnego z wszystkie prawidłowe poprawki i aktualizacje zabezpieczeń, a także aktualizacje składników, takich jak .NET Framework i Internet Information Server. Ponieważ wszystkie maszyny wirtualne są uruchomione systemu Windows Server 2008, platforma Azure udostępnia dodatkowe funkcje, takie jak monitorowanie diagnostycznych, obsługa usług pulpitu zdalnego, zapory i konfiguracja magazynu certyfikatów. Te funkcje są udostępniane bez dodatkowych kosztów. W rzeczywistości po uruchomieniu aplikacji na platformie Azure licencji systemu operacyjnego (OS) systemu Windows Server 2008 jest dołączony. Ponieważ wszystkie maszyny wirtualne są systemem Windows Server 2008, wszelki kod, który działa w systemie Windows Server 2008 działa bez problemu podczas uruchamiania na platformie Azure.

## <a id="concepts"></a>Hostowanej usługi podstawowe koncepcje
Gdy aplikacja jest wdrożona jako usługi hostowanej na platformie Azure, uruchamia jedno lub więcej *ról.* A *roli* odwołujący się do plików aplikacji i konfiguracji. Można zdefiniować co najmniej jedną rolę dla aplikacji, każde z nich własny zestaw plików aplikacji i konfiguracji. Dla każdej roli w aplikacji, można określić liczbę maszyn wirtualnych, lub *wystąpień roli*, aby uruchomić. Na poniższej ilustracji przedstawiono dwa proste przykłady aplikacji formę usługi hostowanej przy użyciu ról i wystąpień ról.

##### <a name="figure-1-a-single-role-with-three-instances-vms-running-in-an-azure-data-center"></a>Rysunek 1: Jedną rolę z trzech wystąpień (VM) w centrum danych platformy Azure
![Obraz][0]

##### <a name="figure-2-two-roles-each-with-two-instances-vms-running-in-an-azure-data-center"></a>Rysunek 2: Dwie role z dwoma wystąpieniami (VM), w centrum danych platformy Azure
![Obraz][1]

Wystąpienia roli zwykle przetwarzania żądań klientów internetowych wprowadzania centrum danych, przez co to jest nazywany *wejściowy punkt końcowy*. Pojedynczej roli może mieć wartości 0 lub więcej wejściowych punktów końcowych. Każdy punkt końcowy określa protokół (HTTP, HTTPS lub TCP) i portu. Często, aby skonfigurować rolę ma dwa wejściowych punktów końcowych: HTTP nasłuchuje na porcie 80 i HTTPS nasłuchuje na porcie 443. Na poniższej ilustracji przedstawiono przykład dwóch różnych ról z różnych kierowanie żądań klientów do ich wejściowych punktów końcowych.

![Obraz][2]

Podczas tworzenia usługi hostowanej na platformie Azure, przypisano publicznie adresowanego adresu IP, której klienci mogą używać do niego dostęp. Podczas tworzenia usługi hostowanej, musisz także wybrać prefiksu adresu URL, który jest zamapowany na ten adres IP. Ten prefiks musi być unikatowa, jak są zasadniczo rezerwowania *prefiks*. cloudapp.net adres URL, tak że nikt została ona. Klienci komunikują się z wystąpienia roli przy użyciu adresu URL. Będzie zazwyczaj nie dystrybucji lub publikowania platformy Azure *prefiks*. cloudapp.net adresu URL. Zamiast tego zostanie zakupu nazwy DNS rejestratora DNS wyboru i skonfiguruj nazwę DNS do przekierowywania żądań klientów na adres URL usługi Azure. Aby uzyskać więcej informacji, zobacz [Konfigurowanie niestandardowej nazwy domeny w usłudze Azure][Configuring a Custom Domain Name in Azure].

## <a id="considerations"></a>Zagadnienia dotyczące projektowania usług hostowanych
Podczas projektowania aplikacji do uruchamiania w środowisku chmury, istnieje kilka kwestii, które można traktować takich jak czas oczekiwania, wysokiej dostępności i skalowalności.

Decydowanie o lokalizacji kod aplikacji jest ważną kwestią podczas uruchamiania usługi hostowanej na platformie Azure. Są często wdrażania aplikacji w centrach danych, które znajdują się najbliżej klientów zmniejszenia opóźnienia i uzyskać najlepszą wydajność, możliwości.
Jednak możesz wybrać zbliżonej do firmy lub bliżej danych centrum danych, jeśli masz obawy jurysdykcyjnych lub prawne dotyczące danych oraz jej lokalizację. Który może obsługiwać kod aplikacji są sześciu centrów danych na całym świecie. W poniższej tabeli przedstawiono dostępne lokalizacje:

<table border="2" cellspacing="0" cellpadding="5" style="border: 2px solid #000000;">

<tbody>

<tr>

<td style="width: 100px;">
**Kraj/Region**

</td>

<td style="width: 200px;">
**Regiony podrzędne**

</td>
</tr>

<tr>

<td>
Stany Zjednoczone

</td>

<td>
Południowo-środkowe & północno-środkowe

</td>
</tr>

<tr>

<td>
Europa

</td>

<td>
Północna & Zachodnia

</td>
</tr>

<tr>

<td>
Azja

</td>

<td>
Południowo-Wschodnia & Region Azji i Pacyfiku

</td>
</tr>
</tbody>
</table>
Podczas tworzenia usługi hostowanej, należy wybrać podregionu wskazującą lokalizację, w którym kod do wykonania.

Aby osiągnąć wysoką dostępność i skalowalność, niezwykle ważne jest zachowane dane aplikacji w centralnym repozytorium dostępne dla wielu wystąpień ról. Aby ułatwić z tym, Azure oferuje różne opcje magazynowania, takich jak obiekty BLOB, tabel i bazy danych SQL. Zobacz [ofert magazynu danych na platformie Azure] [ Data Storage Offerings in Azure] artykułu, aby uzyskać więcej informacji na temat tych technologii magazynowania. Na poniższym rysunku przedstawiono, jak usługa równoważenia obciążenia w centrum danych Azure rozdziela żądania klientów do innej roli wystąpień, które mają dostęp do tego samego magazynu danych.

![Obraz][3]

Zazwyczaj chcesz zlokalizować kod aplikacji i danych w tym samym centrum danych, ponieważ dzięki temu małe opóźnienia (lepszą wydajność) gdy kod aplikacji uzyskuje dostęp do danych. Ponadto możesz są nie naliczane opłaty dotyczące przepustowości podczas przenoszenia danych w tym samym centrum danych.

## <a id="scale"></a>Projektowania aplikacji skali
Czasami możesz pobrać pojedynczą aplikacją (na przykład prostych witryn sieci web) i jest obsługiwana w systemie Azure. Ale często, aplikacja może składać się z kilku ról czy współpracują ze sobą. Na przykład na poniższej ilustracji, istnieją dwa wystąpienia roli witryny sieci Web, trzy wystąpienia roli kolejność przetwarzania i jedno wystąpienie roli Report Generator. Te role są wszystkie pracujących razem i kodu dla wszystkich z nich mogą być umieszczone razem i wdrożony jako pojedyncza jednostka Azure.

![Obraz][4]

Głównym celem podziału aplikacji w różnych ról, każdy uruchomiony na własny zestaw wystąpień roli (to znaczy maszyn wirtualnych) jest niezależne skalowanie ról. Na przykład w sezonie przedświątecznych wielu klientów może być zakupu produktów w firmie, należy zwiększyć liczbę wystąpień roli roli użytkownika witryny sieci Web, a także liczbę wystąpień roli rolą użytkownika kolejności przetwarzania. Po świąt może wystąpić wiele produktów zwrócone, więc nadal trzeba wiele wystąpień witryny sieci Web, ale mniej wystąpień kolejności przetwarzania. W dalszej części roku konieczne może być kilka witryny sieci Web i kolejność przetwarzania wystąpień. We wszystkich tego może być konieczne tylko jedno wystąpienie generatora raportów. Elastyczność wdrożeń opartej na rolach na platformie Azure pozwala łatwo dostosować aplikację na potrzeby biznesowe.

Jest to często mają one rolę, którą wystąpień w usłudze hostowanej komunikują się ze sobą. Na przykład rola witryny sieci Web akceptuje kolejność klienta, ale następnie rozwiązania odciążania kolejność przetwarzania do wystąpień roli kolejności przetwarzania. Najlepszy sposób, aby przekazać formularza pracy jeden zestaw wystąpień roli do innego zbioru wystąpień jest przy użyciu technologii kolejkowania dostarczany przez platformę Azure, usługa kolejki lub kolejek usługi Service Bus. Korzystanie z kolejki jest krytyczną częścią wątku, w tym miejscu. Kolejki umożliwia hostowanej usługi do skalowania jego role niezależnie umożliwia zrównoważenie obciążenia względem kosztów. Jeśli liczba wiadomości w kolejce zwiększa się wraz z upływem czasu, można skalować liczby wystąpień roli kolejności przetwarzania. Jeśli liczba wiadomości w kolejce zmniejsza się wraz z upływem czasu, można skalować liczbę wystąpień roli kolejności przetwarzania. Dzięki temu tylko płatność dla wystąpień musi obsługiwać rzeczywistego obciążenia.

Kolejki są także niezawodności. Podczas skalowania liczby wystąpień roli Order Processing, Azure decyduje o tym, które wystąpienia, aby zakończyć. Może podjąć decyzję o zakończeniu wystąpienie jest w trakcie przetwarzania komunikatu w kolejce. Jednak ponieważ przetwarzanie komunikatu nie zakończy się pomyślnie, komunikat jest widoczna ponownie w innej kolejności przetwarzania wystąpienia roli, który przejmuje on i przetwarza je. Z powodu kolejki wiadomości widoczność komunikaty dotrą do przetworzenie po pewnym czasie. Kolejka działa również jako modułu równoważenia obciążenia rozkładając skutecznie jego wiadomości do roli wszystkich wystąpień, które poprosić wiadomości.

Dla wystąpień roli witryny sieci Web, można monitorować ruch do nich i zdecydować skalować liczbę w górę lub w dół, a także. Kolejki umożliwia skalowanie liczby wystąpień roli witryny sieci Web, niezależnie od wystąpień roli kolejności przetwarzania. To jest bardzo zaawansowane i zapewnia dużą elastyczność. Oczywiście jeśli aplikacja zawiera dodatkowe role, możesz dodać dodatkowe kolejki jako kanał do komunikacji między nimi, aby można było korzystać z tej samej skalowanie i kosztów korzyści.

## <a id="defandcfg"></a>Hostowanej definicji usługi i konfiguracji
Wdrażanie usługi hostowanej na platformie Azure wymaga również mieć pliku definicji usługi i pliku konfiguracji usługi. Oba te pliki są plikami XML i umożliwiają deklaratywnie Określ opcje wdrażania usługi hostowanej. Plik definicji usługi opisano wszystkie role, które tworzą hostowanej usługi i sposób komunikacji. Pliku konfiguracji usługi określa liczbę wystąpień dla poszczególnych ról i ustawienia używane do konfigurowania poszczególnych wystąpień roli.

## <a id="def"></a>Pliku definicji usługi
Jak mogę wspomniano wcześniej, definicji usługi (CSDEF) plik jest plik XML, który opisano różne role wchodzące w skład pełnej aplikacji. Zakończenie schematu dla pliku XML można znaleźć tutaj: [] [http://msdn.microsoft.com/library/windowsazure/ee758711.aspx].
Plik CSDEF zawiera element sieć Web lub proces roboczy dla każdej roli, który ma w aplikacji. Wdrażania roli rolę sieci web (za pomocą elementu sieć Web) oznacza, że kod będzie działać w wystąpieniu roli zawierającego system Windows Server 2008 i Internet Information Server (IIS).
Wdrażanie roli, zgodnie z rolą proces roboczy (za pomocą elementu proces roboczy) oznacza, że wystąpienie roli będzie miał systemu Windows Server 2008 na nim (usługi IIS nie zostanie zainstalowany).

Na pewno możesz utworzyć i wdrożyć rolę procesu roboczego, który używa mechanizmu innych do nasłuchiwania przychodzących żądań sieci web (na przykład kodu można tworzyć i używać .NET HttpListener). Ponieważ wystąpień ról są systemem Windows Server 2008, kodu można wykonać żadnych operacji, które są dostępne dla aplikacji w systemie Windows Server
2008.

Dla każdej roli możesz zaznaczyć wymagany rozmiar maszyny Wirtualnej, która powinna być używana w wystąpieniach tej roli. W poniższej tabeli przedstawiono różne obecnie dostępne rozmiary maszyn wirtualnych i atrybuty każdego:

<table border="2" cellspacing="0" cellpadding="5" style="border: 2px solid #000000;">

<tbody>

<tr align="left" valign="top">

<td>
**Rozmiar maszyny Wirtualnej**

</td>

<td>
**PROCESOR CPU**

</td>

<td>
**PAMIĘĆ RAM**

</td>

<td>
**Dysku**

</td>

<td>
**Szczytowa sieciowych operacji We/Wy**

</td>
</tr>

<tr align="left" valign="top">

<td>
**Bardzo małe**

</td>

<td>
1,0 1 GHz

</td>

<td>
768 MB

</td>

<td>
20GB

</td>

<td>
\~5 MB/s

</td>
</tr>

<tr align="left" valign="top">

<td>
**Małe**

</td>

<td>
1 x 1,6 GHz

</td>

<td>
1,75 GB

</td>

<td>
225GB

</td>

<td>
\~100 MB/s

</td>
</tr>

<tr align="left" valign="top">

<td>
**Średnia liczba godzin**

</td>

<td>
2 procesory 1,6 GHz

</td>

<td>
3,5 GB

</td>

<td>
490GB

</td>

<td>
\~200 MB/s

</td>
</tr>

<tr align="left" valign="top">

<td>
**Duże**

</td>

<td>
4 x 1,6 GHz

</td>

<td>
7 GB

</td>

<td>
1TB

</td>

<td>
\~400 MB/s

</td>
</tr>

<tr align="left" valign="top">

<td>
**Bardzo duża**

</td>

<td>
8 x 1,6 GHz

</td>

<td>
14 GB

</td>

<td>
2TB

</td>

<td>
\~800 MB/s

</td>
</tr>
</tbody>
</table>
Naliczane są opłaty co godzinę dla każdej maszyny Wirtualnej jest używany jako wystąpienia roli i są również opłatę dla dowolnych danych czy wystąpienia roli wysłać poza centrum danych. Nie są naliczane opłaty za danych wprowadzanych centrum danych. Aby uzyskać więcej informacji zobacz [cennik usługi Azure] [cennik usługi Azure]. Ogólnie rzecz biorąc zaleca się używać wielu wystąpień roli małych przeciwieństwie kilka wystąpień dużą, tak aby aplikacja jest bardziej odporny na awarie. Po wszystkich ma mniejszą liczbę wystąpień roli, bardziej Fatalne a błąd w jednej z nich do ogólnej aplikacji. Również jak wspomniano wcześniej, należy wdrożyć co najmniej dwa wystąpienia dla każdej roli w celu pobrania 99,95% umowę dotyczącą poziomu usług udostępniane przez firmę Microsoft.

Plik definicji (CSDEF) usługi jest również, gdzie należy określić wiele atrybutów na temat każdej roli w aplikacji. Oto niektóre przydatne bardziej elementów dostępne:

* **Certyfikaty**. Użytkownik korzysta z certyfikatów do szyfrowania danych lub jeśli usługi sieci web obsługuje protokół SSL. Wszystkie certyfikaty muszą zostać przekazana do usługi Azure. Aby uzyskać więcej informacji, zobacz [zarządzania certyfikatami w usłudze Azure][Managing Certificates in Azure]. To ustawienie XML instaluje wcześniej przekazane certyfikaty do magazynu certyfikatów dla wystąpienia roli, dzięki czemu są one mogą być używane przez kod aplikacji.
* **Nazwy ustawień konfiguracji**. Dla wartości, które mają Twoje aplikacje do odczytu podczas uruchamiania wystąpienia roli. Wartość rzeczywistego ustawienia konfiguracji jest ustawiana w pliku konfiguracji (CSCFG) usługi, która może być aktualizowana w dowolnym momencie bez konieczności ponownego wdrażania kodu. W rzeczywistości może kodu aplikacji w taki sposób, aby wykryć wartości zmienione konfiguracji bez jakiegokolwiek przestoju.
* **Wejściowe punkty końcowe**. W tym miejscu określić żadnych HTTP, HTTPS lub TCP punktów końcowych (z portami), które chcesz udostępnić do na zewnątrz za pośrednictwem sieci *prefiks*. cloadapp.net adresu URL. Gdy Azure wdraża roli użytkownika, zostanie ona skonfigurowana zapora w wystąpieniu roli automatycznie.
* **Wewnętrznych punktów końcowych**. W tym miejscu możesz określić wszelkie HTTP lub TCP punkty końcowe, które mają widoczne dla innych wystąpień roli, które zostały wdrożone w ramach aplikacji. Wewnętrznych punktów końcowych Zezwalaj wystąpień roli w aplikacji do komunikowania się ze sobą, ale nie są dostępne dla wystąpienia roli, znajdujących się poza aplikacji.
* **Zaimportuj moduły**. Te opcjonalnie zainstalować składniki przydatna na wystąpienia roli. Składniki istnieje diagnostycznych monitorowania, pulpitu zdalnego i Azure Connect (dzięki czemu wystąpienia roli do dostępu do zasobów lokalnych za pośrednictwem bezpiecznego kanału).
* **Lokalny magazyn**. To przydziela podkatalogu w wystąpieniu roli aplikacji do użycia. Jest on opisany bardziej szczegółowo w [ofert magazynu danych na platformie Azure] [ Data Storage Offerings in Azure] artykułu.
* **Zadania uruchamiania**. Zadania uruchamiania zapewniają sposób, aby zainstalować wstępnie wymagane składniki w wystąpieniu roli, zgodnie z jego rozruch. Zadania można uruchomić z podwyższonym poziomem uprawnień jako administrator, jeśli jest to wymagane.

## <a id="cfg"></a>Pliku konfiguracji usługi
Plik konfiguracji (CSCFG) usługi jest plik XML, który opisano ustawienia, które można zmienić bez ponownego wdrażania aplikacji. Zakończenie schematu dla pliku XML można znaleźć tutaj: [http://msdn.microsoft.com/library/windowsazure/ee758710.aspx][http://msdn.microsoft.com/library/windowsazure/ee758710.aspx].
Plik CSCFG zawiera element roli dla każdej roli w aplikacji. Oto niektóre elementy, które można określić w pliku CSCFG:

* **Wersja systemu operacyjnego**. Ten atrybut służy do wybierania wersji systemu operacyjnego (OS), który ma używane dla wszystkich wystąpień roli uruchamiania kodu aplikacji. Ten system operacyjny jest nazywany *systemu operacyjnego gościa*, i każda nowa wersja obejmuje najnowsze poprawki zabezpieczeń i aktualizacje dostępne w chwili zwolnienia jest system operacyjny gościa. Jeśli ustawiono wartość atrybutu osVersion "\*", a następnie Azure automatycznie zaktualizuje system operacyjny gościa na każdym z wystąpienia roli jako udostępnienie nowej wersji systemu operacyjnego gościa. Można jednak zrezygnować z aktualizacji automatycznych, wybierając gościa określonych wersji systemu operacyjnego. Na przykład ustawienie wartości atrybutu osVersion "WA-GOŚCIA-OS-2.8\_201109-01" powoduje, że wszystkie wystąpienia roli można pobrać opisano na tej stronie sieci web: [http://msdn.microsoft.com/library/hh560567.aspx] [http://msdn.microsoft.com/library/hh560567.aspx]. Aby uzyskać więcej informacji o wersji systemu operacyjnego gościa, zobacz [Zarządzanie uaktualnienia do systemu operacyjnego gości Azure].
* **Wystąpienia**. Wartość tego elementu wskazuje liczbę wystąpień roli, który ma zostać udostępnione uruchamiania kodu dla określonej roli. Ponieważ można przekazać nowy plik CSCFG Azure (bez ponownego wdrażania aplikacji), jest trivially proste, zmień wartość dla tego elementu i przekazać nowy plik CSCFG dynamiczne zwiększanie lub zmniejszanie liczby wystąpień roli uruchamiania kodu aplikacji . Dzięki temu można łatwo skalować aplikację w górę lub w dół zapotrzebowanie spełniają rzeczywistego obciążenia podczas również kontroli ilości są naliczane opłaty dotyczące uruchomionych wystąpień roli.
* **Ustawienie wartości konfiguracji**. Ten element określa wartości ustawień (zgodnie z definicją w pliku CSDEF). Rola użytkownika mogą odczytywać te wartości, jest uruchomiona. Te wartości ustawienia konfiguracji są zwykle używane do parametrów połączenia z bazą danych SQL lub do magazynu Azure, ale mogą być używane w jakimkolwiek celu, której wymagasz.

## <a id="hostedservices"></a>Tworzenie i wdrażanie usług hostingowych
Tworzenie hostowanej usługi wymaga, należy najpierw przejść do [portalu zarządzania Azure] i Centrum usługi hostowanej przez określenie prefiks DNS i danych, należy udostępnić ostatecznie kodu w. Następnie w środowisku projektowania tworzenie pliku definicji (CSDEF) usługi, utworzyć kod aplikacji i pakietów (zip), wszystkich tych plików do pliku pakietu (CSPKG) usługi. Należy również przygotować usługę pliku konfiguracji (CSCFG). Aby wdrożyć roli użytkownika, możesz przekazać pliki CSCFG i CSPKG z interfejs API zarządzania usługami Azure. Po wdrożeniu usługi Azure, zostanie udostępniania wystąpień roli w centrum danych (na podstawie konfiguracji danych), Wyodrębnij kod aplikacji z pakietu, skopiuj go do wystąpień roli i rozruch wystąpienia. Teraz kod jest uruchomiona.

Na poniższym rysunku przedstawiono tworzonych na komputerze deweloperskim plików CSCFG i CSPKG. Plik CSPKG zawiera plik CSDEF i kod dwie role. Po przekazaniu plików CSCFG i CSPKG przy interfejs API zarządzania usługami Azure, Azure tworzy wystąpień roli w centrum danych. W tym przykładzie pliku CSCFG wskazane, że Azure należy utworzyć trzy wystąpienia roli \#1 i dwóch wystąpień roli \#2.

![Obraz][5]

Aby uzyskać więcej informacji na temat wdrażania, uaktualnianie i ponowne skonfigurowanie poszczególnych ról, zobacz [wdrażanie i aktualizowanie aplikacji Azure] [ Deploying and Updating Azure Applications] artykułu.<a id="Ref" name="Ref"></a>

## <a id="references"></a>Odwołań
* [Tworzenie hostowanej usługi platformy Azure][Creating a Hosted Service for Azure]
* [Zarządzanie usługami hostowanej na platformie Azure][Managing Hosted Services in Azure]
* [Migrowanie aplikacji Azure][Migrating Applications to Azure]
* [Konfigurowanie aplikacji Azure][Configuring an Azure Application]

<div style="width: 700px; border-top: solid; margin-top: 5px; padding-top: 5px; border-top-width: 1px;">

<p>Napisane przez Jeffrey Richter (Wintellect)</p>

</div>

[Azure Application Model Benefits]: #benefits
[Hosted Service Core Concepts]: #concepts
[Hosted Service Design Considerations]: #considerations
[Designing your Application for Scale]: #scale
[Hosted Service Definition and Configuration]: #defandcfg
[The Service Definition File]: #def
[The Service Configuration File]: #cfg
[Creating and Deploying a Hosted Service]: #hostedservices
[References]: #references
[0]: ./media/application-model/application-model-3.jpg
[1]: ./media/application-model/application-model-4.jpg
[2]: ./media/application-model/application-model-5.jpg
[Configuring a Custom Domain Name in Azure]: http://www.windowsazure.com/develop/net/common-tasks/custom-dns/
[Data Storage Offerings in Azure]: http://www.windowsazure.com/develop/net/fundamentals/cloud-storage/
[3]: ./media/application-model/application-model-6.jpg
[4]: ./media/application-model/application-model-7.jpg

[Azure Pricing]: http://www.windowsazure.com/pricing/calculator/
[Managing Certificates in Azure]: http://msdn.microsoft.com/library/windowsazure/gg981929.aspx
[http://msdn.microsoft.com/library/windowsazure/ee758710.aspx]: http://msdn.microsoft.com/library/windowsazure/ee758710.aspx
[http://msdn.microsoft.com/library/hh560567.aspx]: http://msdn.microsoft.com/library/hh560567.aspx
[Zarządzanie uaktualnienia do systemu operacyjnego gości Azure]: http://msdn.microsoft.com/library/ee924680.aspx
[portalu zarządzania Azure]: http://manage.windowsazure.com/
[5]: ./media/application-model/application-model-8.jpg
[Deploying and Updating Azure Applications]: http://www.windowsazure.com/develop/net/fundamentals/deploying-applications/
[Creating a Hosted Service for Azure]: http://msdn.microsoft.com/library/gg432967.aspx
[Managing Hosted Services in Azure]: http://msdn.microsoft.com/library/gg433038.aspx
[Migrating Applications to Azure]: http://msdn.microsoft.com/library/gg186051.aspx
[Configuring an Azure Application]: http://msdn.microsoft.com/library/windowsazure/ee405486.aspx
