---
title: "Planista wdrożenia usługi Azure Site Recovery dla oprogramowania VMware na platformie Azure | Microsoft Docs"
description: "Jest to podręcznik użytkownika planisty wdrożenia usługi Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: nsoneji
manager: garavd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 08/28/2017
ms.author: nisoneji
ms.openlocfilehash: 60b0641076c2fa8ed2feb5c64e7b119519f46cf4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-site-recovery-deployment-planner"></a>Planista wdrożenia usługi Azure Site Recovery
Ten artykuł to podręcznik użytkownika planisty wdrożenia usługi Azure Site Recovery dla wdrożeń produkcyjnych oprogramowania VMware na platformie Azure.

## <a name="overview"></a>Omówienie

Zanim zaczniesz chronić maszyny wirtualne oprogramowania VMware za pomocą usługi Site Recovery, przydziel odpowiednią przepustowość zgodnie z częstotliwością dziennych zmian danych, aby osiągnąć założony cel punktu odzyskiwania. Pamiętaj o lokalnym wdrożeniu odpowiedniej liczby serwerów konfiguracji i serwerów przetwarzania.

Musisz także utworzyć właściwą liczbę kont magazynów platformy Azure odpowiedniego typu. Możesz utworzyć konta magazynu w warstwie Standardowa lub Premium, biorąc pod uwagę wzrost w zakresie źródłowych serwerów produkcyjnych z powodu zwiększania użycia w czasie. Wybierz typ magazynu dla maszyny wirtualnej w oparciu o charakterystyki obciążenia (np. operacje we/wy odczytu/zapisu na sekundę (IOPS) lub współczynnik zmian danych) oraz limity usługi Site Recovery.

Planista wdrożenia usługi Site Recovery w publicznej wersji zapoznawczej to narzędzie wiersza polecenia dostępne obecnie tylko na potrzeby replikacji z oprogramowania VMware na platformę Azure. To narzędzie pozwala zdalnie profilować maszyny wirtualne oprogramowania VMware (bez żadnego wpływu na środowisko produkcyjne), aby poznać wymagania dotyczące przepustowości i magazynu Azure Storage dla udanej replikacji oraz testu pracy w trybie failover. Narzędzie możesz uruchomić bez instalowania składników usługi Site Recovery w środowisku lokalnym. Jednak w celu uzyskania dokładnych wyników osiągniętej przepływności zaleca się uruchomienie planisty na serwerze z systemem Windows Server spełniającym minimalne wymagania dotyczące serwera konfiguracji usługi Site Recovery, który trzeba będzie wdrożyć na początku procesu wdrażania w środowisku produkcyjnym.

Narzędzie udostępnia następujące szczegóły:

**Ocena zgodności**

* Ocena uprawnień maszyny wirtualnej na podstawie liczby dysków, rozmiaru dysku, liczby operacji we/wy na sekundę, współczynnika zmian i typu rozruchu (EFI/BIOS)
* Szacowana przepustowość sieci wymagana na potrzeby replikacji przyrostowej

**Zapotrzebowanie na przepustowość sieci w porównaniu z oceną celu punktu odzyskiwania**

* Szacowana przepustowość sieci wymagana na potrzeby replikacji przyrostowej
* Przepływność, którą usługa Site Recovery może uzyskać między środowiskiem lokalnym i platformą Azure
* Liczba maszyn wirtualnych do przetworzenia wsadowego na podstawie szacowanej przepustowości wymagana do ukończenia replikacji początkowej w określonym czasie

**Wymagania dotyczące infrastruktury platformy Azure**

* Wymagania dotyczące typu magazynu (konta w warstwie Standardowa lub Premium) dla poszczególnych maszyn wirtualnych
* Łączna liczba kont magazynu w warstwie Standardowa i Premium do skonfigurowania na potrzeby replikacji
* Propozycje nazw kont magazynu oparte na wskazówkach usługi Azure Storage
* Rozmieszczanie konta magazynu dla wszystkich maszyn wirtualnych
* Liczba rdzeni platformy Azure do skonfigurowania przed rozpoczęciem pracy w trybie failover lub testu pracy w trybie failover w ramach subskrypcji
* Zalecany rozmiar poszczególnych maszyn wirtualnych platformy Azure

**Wymagania dotyczące infrastruktury lokalnej**
* Wymagana liczba serwerów konfiguracji i serwerów przetwarzania do wdrożenia w środowisku lokalnym

>[!IMPORTANT]
>
>Ponieważ obciążenie będzie prawdopodobnie zwiększać się wraz z upływem czasu, wszystkie poprzednie obliczenia narzędzia są wykonywane przy założeniu 30-procentowego współczynnika wzrostu wartości charakterystyk obciążenia i przy użyciu wartości 95. percentyla wszystkich metry profilowania (operacje we/wy odczytu/zapisu na sekundę, współczynnik zmian danych itd.). Oba te elementy (współczynnik wzrostu i obliczenie wartości percentyla) można konfigurować. Więcej informacji na temat współczynnika wzrostu można znaleźć w sekcji „Zagadnienia związane ze współczynnikiem wzrostu”. Więcej informacji na temat wartości percentyla można znaleźć w sekcji „Wartość percentyla użyta w obliczeniach”.
>

## <a name="requirements"></a>Wymagania
Narzędzie obejmuje dwa główne etapy — profilowanie i generowanie raportu. Jest też dostępny trzeci etap umożliwiający obliczanie tylko przepływności. Wymagania dotyczące serwera, z którego inicjowane jest profilowanie i pomiar przepływności, zostały przedstawione w poniższej tabeli:

| Wymaganie dotyczące serwera | Opis|
|---|---|
|Profilowanie i pomiar przepływności| <ul><li>System operacyjny: Microsoft Windows Server 2012 R2<br>(w idealnej sytuacji spełniający co najmniej [zalecenia dotyczące rozmiaru serwera konfiguracji](https://aka.ms/asr-v2a-on-prem-components))</li><li>Konfiguracja maszyny: 8 wirtualnych procesorów CPU, 16 GB pamięci RAM, dysk twardy o rozmiarze 300 GB</li><li>[Microsoft .NET Framework 4.5](https://aka.ms/dotnet-framework-45)</li><li>[VMware vSphere PowerCLI 6.0 R3](https://aka.ms/download_powercli)</li><li>[Pakiet Microsoft Visual C++ Redistributable dla programu Visual Studio 2012](https://aka.ms/vcplusplus-redistributable)</li><li>Dostęp do platformy Azure przez Internet z tego serwera</li><li>Konto magazynu Azure</li><li>Dostęp administratora na serwerze</li><li>Minimalnie 100 GB wolnego miejsca na dysku (przy założeniu 1000 maszyn wirtualnych z średnio trzema dyskami na każdej z nich i profilowanych przez 30 dni)</li><li>Ustawienia poziomu statystyk programu VMware vCenter powinny być na poziomie 2 lub wyższym</li><li>Zezwalaj na port 443: planista wdrożenia usługi ASR używa tego portu do nawiązywania połączenia z serwerem vCenter/hostem ESXi</ul></ul>|
| Generowanie raportu | Dowolny komputer z systemem Windows lub Windows Server i programem Microsoft Excel 2013 lub nowszym |
| Uprawnienia użytkowników | Uprawnienia tylko do odczytu dla konta użytkownika używanego do uzyskiwania dostępu do serwera VMware vCenter/hosta VMware vSphere ESXi podczas profilowania |

> [!NOTE]
>
>Narzędzie może profilować tylko maszyny wirtualne z dyskami VMDK i RDM. Nie pozwala ono profilować maszyn wirtualnych z dyskami iSCSI ani NFS. Usługa Site Recovery obsługuje dyski iSCSI i NFS w przypadku serwerów VMware, ponieważ planista wdrożenia nie znajduje się na gościu i profiluje tylko przy użyciu liczników wydajności programu vCenter, jednak narzędzie nie ma wglądu w te typy dysków.
>

## <a name="download-and-extract-the-public-preview"></a>Pobieranie i wyodrębnianie publicznej wersji zapoznawczej
1. Pobierz najnowszą wersję [planisty wdrożenia usługi Site Recovery w ramach publicznej wersji zapoznawczej](https://aka.ms/asr-deployment-planner).  
Narzędzie jest spakowane w folderze ZIP. Bieżąca wersja narzędzia obsługuje tylko replikację z oprogramowania VMware do platformy Azure.

2. Skopiuj folder ZIP na serwer z systemem Windows, z którego chcesz uruchomić narzędzie.  
Narzędzie można uruchomić z systemu Windows Server 2012 R2, jeśli serwer ma dostęp do sieci umożliwiający połączenie z serwerem vCenter/hostem vSphere ESXi przechowującym maszyny wirtualne do profilowania. Zalecamy jednak uruchamianie narzędzia na serwerze, którego konfiguracja sprzętowa spełnia [wytyczne dotyczące określania rozmiaru serwera konfiguracji](https://aka.ms/asr-v2a-on-prem-components). Jeśli wdrożono już lokalnie składniki usługi Site Recovery, uruchom narzędzie na serwerze konfiguracji.

 Na serwerze, na którym uruchamiasz narzędzie, zalecamy korzystanie z takiej samej konfiguracji sprzętu jak serwer konfiguracji (z wbudowanym serwerem przetwarzania). Dzięki takiej konfiguracji masz pewność, że osiągnięta przepływność zgłaszana przez narzędzie jest zgodna z rzeczywistą przepływnością, którą usługa Site Recovery może osiągnąć podczas replikacji. Obliczanie przepływności zależy od dostępnej przepustowości sieci na serwerze i konfiguracji sprzętu (procesor CPU, magazyn itd.) serwera. Jeśli uruchamiasz narzędzie z innego serwera, obliczana jest przepływność z tego serwera na platformę Microsoft Azure. Ponadto ponieważ konfiguracja sprzętu serwera może różnić się od konfiguracji serwera konfiguracji, dane osiągniętej przepływności zgłaszanej przez narzędzie mogą być niedokładne.

3. Wyodrębnij folder ZIP.  
Folder zawiera wiele plików i podfolderów. Plik wykonywalny nosi nazwę ASRDeploymentPlanner.exe i znajduje się w folderze nadrzędnym.

    Przykład:  
    Skopiuj plik ZIP na dysk E:\ i wyodrębnij go.
   E:\ASR Deployment Planner-Preview_v1.2.zip

    E:\ASR Deployment Planner-Preview_v1.2\ ASR Deployment Planner-Preview_v1.2\ ASRDeploymentPlanner.exe

## <a name="capabilities"></a>Możliwości
Narzędzie wiersza polecenia (ASRDeploymentPlanner.exe) możesz uruchomić w jednym z trzech następujących trybów:

1. Profilowanie  
2. Generowanie raportu
3. Uzyskiwanie informacji o przepływności

Najpierw uruchom narzędzie w trybie profilowania, aby zebrać informacje na temat współczynnika zmian danych i operacji we/wy na sekundę maszyny wirtualnej. Następnie uruchom narzędzie w celu wygenerowania raportu, aby uzyskać wymagania dotyczące przepustowości i magazynu.

## <a name="profiling"></a>Profilowanie
W trybie profilowania narzędzie planisty wdrożenia łączy się z serwerem vCenter/hostem vSphere ESXi w celu zebrania danych wydajności maszyny wirtualnej.

* Profilowanie nie wpływa na wydajność produkcyjnych maszyn wirtualnych, ponieważ nie nawiązujesz z nimi bezpośredniego połączenia. Wszystkie dane wydajności są zbierane z serwera vCenter/hosta vSphere ESXi.
* Aby upewnić się, że profilowanie ma niewielki wpływ na wydajność serwera, co 15 minut narzędzie przesyła zapytanie na serwer vCenter/do hosta vSphere ESXi. Interwał zapytań nie ma wpływu na dokładność profilowania, ponieważ narzędzie zapisuje dane liczników wydajności z każdej minuty.

### <a name="create-a-list-of-vms-to-profile"></a>Tworzenie listy maszyn wirtualnych do profilowania
Najpierw musisz utworzyć listę maszyn wirtualnych, które chcesz profilować. Wszystkie nazwy maszyn wirtualnych na serwerze vCenter/hoście vSphere ESXi można uzyskać za pomocą poniższych poleceń programu VMware vSphere PowerCLI, wykonując kroki poniższej procedury. Alternatywnie można wyświetlić w pliku listę przyjaznych nazw lub adresów IP maszyn wirtualnych, które chcesz profilować ręcznie.

1. Zaloguj się do maszyny wirtualnej z zainstalowanym programem VMware vSphere PowerCLI.
2. Otwórz konsolę programu VMware vSphere PowerCLI.
3. Upewnij się, że zasady wykonywania skryptu zostały włączone. Jeśli zostały wyłączone, uruchom konsolę programu VMware vSphere PowerCLI w trybie administratora, a następnie włącz zasady, uruchamiając następujące polecenie:

            Set-ExecutionPolicy –ExecutionPolicy AllSigned

4. Może być konieczne uruchomienie poniższego polecenia, jeśli element Connect-VIServer nie został rozpoznany jako nazwa polecenia cmdlet.
 
            Add-PSSnapin VMware.VimAutomation.Core 

5. Aby pobrać wszystkie nazwy maszyn wirtualnych na serwerze vCenter/hoście vSphere ESXi i zapisać listę w pliku TXT, uruchom dwa poniższe polecenia.
Zamień wartości &lsaquo;server name&rsaquo;, &lsaquo;user name&rsaquo;, &lsaquo;password&rsaquo; i &lsaquo;outputfile.txt&rsaquo; na własne wartości.

            Connect-VIServer -Server <server name> -User <user name> -Password <password>

            Get-VM |  Select Name | Sort-Object -Property Name >  <outputfile.txt>

6. Otwórz plik wyjściowy w Notatniku, a następnie skopiuj nazwy wszystkich maszyn wirtualnych do profilowania do innego pliku (np. ProfileVMList.txt) tak, aby w każdym wierszu znajdowała się jedna nazwa maszyny wirtualnej. Ten plik jest używany jako źródło danych wejściowych dla parametru *-VMListFile* narzędzia wiersza polecenia.

    ![Lista nazw maszyn wirtualnej w planiście wdrożenia](./media/site-recovery-deployment-planner/profile-vm-list.png)

### <a name="start-profiling"></a>Rozpoczynanie profilowania
Po utworzeniu listy maszyn wirtualnych do profilowania można uruchomić narzędzie w trybie profilowania. Poniżej przedstawiono listę obowiązkowych i opcjonalnych parametrów narzędzia w trybie profilowania.

ASRDeploymentPlanner.exe -Operation StartProfiling /?

| Nazwa parametru | Opis |
|---|---|
| -Operation | StartProfiling |
| -Server | W pełni kwalifikowana nazwa domeny lub adres IP serwera vCenter/hosta vSphere ESXi, którego maszyny wirtualne mają być profilowane.|
| -User | Nazwa użytkownika łączącego się z serwerem vCenter/hostem vSphere ESXi. Użytkownik musi mieć minimalnie uprawnienia tylko do odczytu.|
| -VMListFile | Plik zawierający listę maszyn wirtualnych do profilowania. Można użyć bezwzględnej lub względnej ścieżki pliku. Ten plik powinien zawierać jedną nazwę/jeden adres IP maszyny wirtualnej w każdym wierszu. Nazwa maszyny wirtualnej określona w pliku powinna być taka sama jak nazwa maszyny wirtualnej na serwerze vCenter/hoście vSphere ESXi.<br>Na przykład plik VMList.txt zawiera informacje o następujących maszynach wirtualnych:<ul><li>maszyna_wirtualna_A</li><li>10.150.29.110</li><li>maszyna_wirtualna_B</li><ul> |
| -NoOfDaysToProfile | Czas trwania profilowania w dniach. Zalecamy przeprowadzanie profilowania przez więcej niż 15 dni, aby umożliwić zaobserwowanie wzorca obciążenia w środowisku względem danego okresu i użycie go do udostępnienia dokładnego zalecenia. |
| -Directory | (Opcjonalnie) Ścieżka UNC (Universal Naming Convention) lub ścieżka katalogu lokalnego, w której są przechowywane dane profilowania wygenerowane podczas profilowania. Jeśli nazwa katalogu nie zostanie podana, jako katalog domyślny zostanie użyty katalog o nazwie „ProfiledData” w bieżącej ścieżce. |
| -Password | (Opcjonalnie) Hasło do użycia podczas nawiązywania połączenia z serwerem vCenter/hostem vSphere ESXi. Jeśli nie określisz go teraz, monit o podanie hasła zostanie wyświetlony podczas wykonywania polecenia.|
| -StorageAccountName | (Opcjonalnie) Nazwa konta magazynu używana do wyszukiwania osiągalnej przepływności na potrzeby replikacji danych ze środowiska lokalnego na platformę Azure. Narzędzie przekazuje dane testowe na to konto magazynu w celu obliczenia przepływności.|
| -StorageAccountKey | (Opcjonalnie) Klucz konta magazynu używany do uzyskiwania dostępu do konta magazynu. W witrynie Azure Portal przejdź do pozycji Konta magazynu > <*Nazwa_konta_magazynu*> > Ustawienia > Klucze dostępu > Klucz1 (lub podstawowy klucz dostępu w przypadku klasycznego konta magazynu). |
| -Environment | (Opcjonalnie) Jest to docelowe środowisko konta usługi Azure Storage. Ten parametr może przyjmować jedną z trzech wartości — AzureCloud, AzureUSGovernment i AzureChinaCloud. Wartością domyślną jest AzureCloud. Użyj tego parametru, jeśli Twoim docelowym regionem świadczenia usługi Azure są chmury wersji platformy Azure dla administracji USA lub chińska wersja platformy Azure. |


Zalecamy, aby maszyny wirtualne były profilowane przez co najmniej 15 do 30 dni. Podczas profilowania jest uruchomiony proces ASRDeploymentPlanner.exe. Czas profilowania w narzędziu jest określany w dniach. Jeśli chcesz przeprowadzić profilowanie w ciągu kilku godzin lub minut w celu szybkiego przetestowania narzędzia w publicznej wersji zapoznawczej, musisz przekonwertować wartość czasu na równoważność liczby dni. Aby na przykład profilowanie trwało 30 minut, musisz wprowadzić wartość 30/(60*24) = 0,021 dnia. Minimalny dozwolony czas profilowania to 30 minut.

Podczas profilowania można przekazać nazwę i klucz konta magazynu w celu znalezienia osiągalnej przepływności usługi Site Recovery podczas replikacji z serwera konfiguracji lub serwera przetwarzania na platformę Azure. Jeśli nazwa i klucz konta magazynu nie zostaną przekazane podczas profilowania, narzędzie nie obliczy osiągalnej przepływności.

Możesz uruchomić wiele wystąpień narzędzia dla różnych zestawów maszyn wirtualnych. Upewnij się, że nazwy maszyn wirtualnych nie powtarzają się w żadnym zestawie profilowania. Jeśli na przykład profilowano dziesięć maszyn wirtualnych (MW1–MW10) i po kilku dniach chcesz przeprowadzić profilowanie kolejnych pięciu maszyn wirtualnych (MW11–MW15), możesz uruchomić narzędzie z poziomu innej konsoli wiersza polecenia dla drugiego zestawu maszyn wirtualnych (MW11–MW15). Upewnij się, że drugi zestaw maszyn wirtualnych nie zawiera żadnych nazw maszyn wirtualnych z pierwszego wystąpienia profilowania, lub użyj innego katalogu wyjściowego dla drugiego przebiegu. Jeśli dwa wystąpienia narzędzia są używane na potrzeby profilowania tych samych maszyn wirtualnych i korzystają z tego samego katalogu wyjściowego, zostanie wygenerowany niepoprawny raport.

Konfiguracja maszyn wirtualnych jest przechwytywana raz na początku operacji profilowania i zapisywana w pliku o nazwie VMDetailList.xml. Te informacje są używane podczas generowania raportu. Żadne zmiany konfiguracji maszyny wirtualnej (np. zwiększenie liczby rdzeni, dysków lub kart sieciowych) wprowadzone od początku do końca okresu profilowania nie są przechwytywane. Jeśli konfiguracja profilowanej maszyny wirtualnej zmieniła się w trakcie profilowania w publicznej wersji zapoznawczej, poniżej przedstawiono obejście pozwalające uzyskać najnowsze szczegóły maszyny wirtualnej podczas generowania raportu:

* Utworzenie kopii zapasowej pliku VMdetailList.xml i usunięcie pliku z jego bieżącej lokalizacji.
* Przekazanie argumentów -User i -Password podczas generowania raportu.

Polecenie profilowania powoduje wygenerowanie kilku plików w katalogu profilowania. Nie usuwaj żadnych plików, ponieważ wpływa to na proces generowania raportów.

#### <a name="example-1-profile-vms-for-30-days-and-find-the-throughput-from-on-premises-to-azure"></a>Przykład 1: profilowanie maszyn wirtualnych przez 30 dni i znajdowanie przepływności między środowiskiem lokalnym i platformą Azure
```
ASRDeploymentPlanner.exe -Operation StartProfiling -Directory “E:\vCenter1_ProfiledData” -Server vCenter1.contoso.com -VMListFile “E:\vCenter1_ProfiledData\ProfileVMList1.txt”  -NoOfDaysToProfile  30  -User vCenterUser1 -StorageAccountName  asrspfarm1 -StorageAccountKey Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==
```

#### <a name="example-2-profile-vms-for-15-days"></a>Przykład 2: profilowanie maszyn wirtualnych przez 15 dni

```
ASRDeploymentPlanner.exe -Operation StartProfiling -Directory “E:\vCenter1_ProfiledData” -Server vCenter1.contoso.com -VMListFile “E:\vCenter1_ProfiledData\ProfileVMList1.txt”  -NoOfDaysToProfile  15  -User vCenterUser1
```

#### <a name="example-3-profile-vms-for-1-hour-for-a-quick-test-of-the-tool"></a>Przykład 3: profilowanie maszyn wirtualnych przez 1 godzinę w celu szybkiego przetestowania narzędzia
```
ASRDeploymentPlanner.exe -Operation StartProfiling -Directory “E:\vCenter1_ProfiledData” -Server vCenter1.contoso.com -VMListFile “E:\vCenter1_ProfiledData\ProfileVMList1.txt”  -NoOfDaysToProfile  0.04  -User vCenterUser1
```

>[!NOTE]
>
>* Jeśli serwer z działającym narzędziem został ponownie uruchomiony lub uległ awarii albo jeśli zamknięto narzędzie za pomocą klawiszy Ctrl+C, profilowane dane zostaną zachowane. Może jednak brakować profilowanych danych z ostatnich 15 minut. W takiej sytuacji uruchom narzędzie jeszcze raz w trybie profilowania po ponownym uruchomieniu serwera.
>* Jeśli przekazano nazwę i klucz konta magazynu, narzędzie mierzy przepływność na ostatnim etapie profilowania. Jeśli narzędzie zostanie zamknięte przed ukończeniem profilowania, przepływność nie zostanie obliczona. Aby znaleźć przepływność przed wygenerowaniem raportu, można uruchomić operację GetThroughput w konsoli wiersza polecenia. W przeciwnym razie wygenerowany raport nie będzie zawierać informacji o przepływności.


## <a name="generate-a-report"></a>Generowanie raportu
Narzędzie generuje plik programu Microsoft Excel z włączoną obsługą makr (plik XLSM) jako dane wyjściowe raportu zawierające podsumowanie wszystkich zaleceń dotyczące wdrożenia. Raport nosi nazwę DeploymentPlannerReport_<*unikatowy_identyfikator_numeryczny*>.xlsm i jest umieszczany w wybranym katalogu.

Po zakończeniu profilowania możesz uruchomić narzędzie w trybie generowania raportu. Poniższa tabela zawiera listę obowiązkowych i opcjonalnych parametrów narzędzia uruchamianego w trybie generowania raportu.

`ASRDeploymentPlanner.exe -Operation GenerateReport /?`

|Nazwa parametru | Opis |
|-|-|
| -Operation | GenerateReport |
| -Server |  W pełni kwalifikowana nazwa domeny lub adres IP serwera vCenter/vSphere (nazwa lub adres IP muszą być takie same jak użyte podczas profilowania) z profilowanymi maszynami wirtualnymi, dla których ma zostać wygenerowany raport. Pamiętaj, że jeśli podczas profilowania użyto serwera vCenter, nie można użyć serwera vSphere podczas generowania raportu i odwrotnie.|
| -VMListFile | Plik zawierający listę profilowanych maszyn wirtualnych, dla których będzie generowany raport. Można użyć bezwzględnej lub względnej ścieżki pliku. Ten plik powinien zawierać jedną nazwę lub jeden adres IP maszyny wirtualnej w każdym wierszu. Nazwy maszyn wirtualnych określone w pliku powinny być takie same jak nazwy maszyn wirtualnych na serwerze vCenter/hoście vSphere ESXi i zgodne z nazwami użytymi podczas profilowania.|
| -Directory | (Opcjonalnie) Ścieżka UNC lub ścieżka katalogu lokalnego, w której są przechowywane profilowane dane (pliki wygenerowane podczas profilowania). Te dane są wymagane do wygenerowania raportu. Jeśli nie określisz nazwy, zostanie użyty katalog „ProfiledData”. |
| -GoalToCompleteIR | (Opcjonalnie) Czas w godzinach, w którym musi zostać ukończona replikacja początkowa profilowanych maszyn wirtualnych. Wygenerowany raport zawiera liczbę maszyn wirtualnych, dla których można ukończyć replikację początkową w określonym czasie. Wartość domyślna to 72 godziny. |
| -User | (Opcjonalnie) Nazwa użytkownika do użycia podczas nawiązywania połączenia z serwerem vCenter/vSphere. Nazwa jest używana do pobierania najnowszych informacji o konfiguracji maszyn wirtualnych, takich jak liczba dysków, liczba rdzeni czy liczba kart sieciowych, do zastosowania w raporcie. Jeśli nie podasz nazwy, zostaną użyte informacje o konfiguracji zebrane na początku profilowania. |
| -Password | (Opcjonalnie) Hasło do użycia podczas nawiązywania połączenia z serwerem vCenter/hostem vSphere ESXi. Jeśli nie określisz hasła jako parametru, później zostanie wyświetlony monit o podanie hasła podczas wykonywania polecenia. |
| -DesiredRPO | (Opcjonalnie) Żądany cel punktu odzyskiwania w minutach. Wartość domyślna to 15 minut.|
| -Bandwidth | Przepustowość w Mb/s. Parametr służący do obliczania celu punktu odzyskiwania, który można osiągnąć dla określonej przepustowości. |
| -StartDate | (Opcjonalnie) Data i godzina rozpoczęcia w formacie DD-MM-RRRR:GG:MM (format 24-godzinny). Oprócz parametru *StartDate* należy także określić parametr *EndDate*. W przypadku określenia wartości parametru StartDate wygenerowany raport będzie dotyczyć profilowanych danych zebranych od daty StartDate do daty EndDate. |
| -EndDate | (Opcjonalnie) Data i godzina zakończenia w 24-godzinnym formacie DD-MM-RRRR:GG:MM. Oprócz parametru *EndDate* należy także określić parametr *StartDate*. W przypadku określenia wartości parametru EndDate wygenerowany raport będzie dotyczyć profilowanych danych zebranych od daty StartDate do daty EndDate. |
| -GrowthFactor | (Opcjonalnie) Współczynnik wzrostu wyrażony jako wartość procentowa. Wartość domyślna to 30 procent. |
| -UseManagedDisks | (Opcjonalnie) UseManagedDisks — Yes/No. Wartość domyślna to Yes. Liczba maszyn wirtualnych, które można umieścić w ramach pojedynczego konta magazynu, jest obliczana z uwzględnieniem tego, czy na potrzeby pracy w trybie failover/testu pracy w trybie failover jest używany dysk zarządzany, czy niezarządzany. |

#### <a name="example-1-generate-a-report-with-default-values-when-the-profiled-data-is-on-the-local-drive"></a>Przykład 1: generowanie raportu przy użyciu wartości domyślnych, gdy profilowane dane znajdują się na dysku lokalnym
```
ASRDeploymentPlanner.exe -Operation GenerateReport -Server vCenter1.contoso.com -Directory “\\PS1-W2K12R2\vCenter1_ProfiledData” -VMListFile “\\PS1-W2K12R2\vCenter1_ProfiledData\ProfileVMList1.txt”
```

#### <a name="example-2-generate-a-report-when-the-profiled-data-is-on-a-remote-server"></a>Przykład 2: generowanie raportu, gdy profilowane dane znajdują się na serwerze zdalnym
Użytkownik musi mieć uprawnienia odczytu/zapisu w katalogu zdalnym.
```
ASRDeploymentPlanner.exe -Operation GenerateReport -Server vCenter1.contoso.com -Directory “\\PS1-W2K12R2\vCenter1_ProfiledData” -VMListFile “\\PS1-W2K12R2\vCenter1_ProfiledData\ProfileVMList1.txt”
```

#### <a name="example-3-generate-a-report-with-a-specific-bandwidth-and-goal-to-complete-ir-within-specified-time"></a>Przykład 3: generowanie raportu przy użyciu określonej przepustowości i celu ukończenia replikacji początkowej w określonym czasie
```
ASRDeploymentPlanner.exe -Operation GenerateReport -Server vCenter1.contoso.com -Directory “E:\vCenter1_ProfiledData” -VMListFile “E:\vCenter1_ProfiledData\ProfileVMList1.txt” -Bandwidth 100 -GoalToCompleteIR 24
```

#### <a name="example-4-generate-a-report-with-a-5-percent-growth-factor-instead-of-the-default-30-percent"></a>Przykład 4: generowanie raportu przy użyciu współczynnika wzrostu 5 procent zamiast domyślnego współczynnika 30 procent
```
ASRDeploymentPlanner.exe -Operation GenerateReport -Server vCenter1.contoso.com -Directory “E:\vCenter1_ProfiledData” -VMListFile “E:\vCenter1_ProfiledData\ProfileVMList1.txt” -GrowthFactor 5
```

#### <a name="example-5-generate-a-report-with-a-subset-of-profiled-data"></a>Przykład 5: generowanie raportu przy użyciu podzestawu profilowanych danych
Załóżmy, że masz profilowane dane z 30 dni i chcesz wygenerować raport tylko z 20 dni.
```
ASRDeploymentPlanner.exe -Operation GenerateReport -Server vCenter1.contoso.com -Directory “E:\vCenter1_ProfiledData” -VMListFile “E:\vCenter1_ProfiledData\ProfileVMList1.txt” -StartDate  01-10-2017:12:30 -EndDate 01-19-2017:12:30
```

#### <a name="example-6-generate-a-report-for-5-minute-rpo"></a>Przykład 6: generowanie raportu w przypadku 5-minutowego celu punktu odzyskiwania
```
ASRDeploymentPlanner.exe -Operation GenerateReport -Server vCenter1.contoso.com -Directory “E:\vCenter1_ProfiledData” -VMListFile “E:\vCenter1_ProfiledData\ProfileVMList1.txt”  -DesiredRPO 5
```

## <a name="percentile-value-used-for-the-calculation"></a>Wartość percentyla używana do obliczenia
**Jaka domyślna wartość percentyla metryk wydajności zebranych podczas profilowania jest używana przez narzędzie podczas generowania raportu?**

Domyślnie narzędzie używa wartości 95. percentyla operacji we/wy odczytu i zapisu na sekundę, operacji we/wy zapisu na sekundę i współczynnika zmian danych zebranych podczas profilowania wszystkich maszyn wirtualnych. Dzięki tej metryce masz pewność, że ewentualna szczytowa wartość 100. percentyla, która może wystąpić na maszynach wirtualnych z powodu tymczasowych zdarzeń, nie będzie używana do określania wymagań dotyczących docelowego konta magazynu ani przepustowości źródłowej. Zdarzenie tymczasowe to na przykład uruchamiane raz dziennie zadanie tworzenia kopii zapasowej, działanie polegające na okresowym indeksowaniu bazy danych lub generowaniu raportu analitycznego albo inne podobne, krótkotrwałe zdarzenia występujące w danym momencie czasowym.

Korzystanie z wartości 95. percentyla oddaje rzeczywistą charakterystykę obciążenia i zapewnia najlepszą wydajność, gdy te obciążenia działają na platformie Azure. Nie przewidujemy konieczności zmiany tej wartości numerycznej. Jeśli nie zmieniasz wartości (np. na 90. percentyl), możesz zaktualizować plik konfiguracji *ASRDeploymentPlanner.exe.config* w folderze domyślnym i zapisać go w celu wygenerowania nowego raportu dotyczącego istniejących profilowanych danych.
```
<add key="WriteIOPSPercentile" value="95" />      
<add key="ReadWriteIOPSPercentile" value="95" />      
<add key="DataChurnPercentile" value="95" />
```

## <a name="growth-factor-considerations"></a>Zagadnienia związane ze współczynnikiem wzrostu
**Dlaczego podczas planowania wdrożenia warto wziąć pod uwagę współczynnik wzrostu?**

Uwzględnianie wzrostu w charakterystyce obciążenia jest niezwykle ważne przy zakładaniu potencjalnego wzrostu użycia wraz z upływem czasu. W przypadku zmiany charakterystyki po włączeniu ochrony nie ma możliwości przełączenia do innego konta magazynu w celu zapewnienia ochrony bez wyłączenia i ponownego włączenia ochrony.

Załóżmy na przykład, że dzisiaj maszyna wirtualna mieści się na standardowym koncie replikacji magazynu. W ciągu następnych trzech miesięcy prawdopodobnie wystąpią następujące zmiany:

* Wzrośnie liczba użytkowników aplikacji uruchamianej na maszynie wirtualnej.
* Zwiększony wynikowy współczynnik zmiany danych na maszynie wirtualnej będzie wymagać przeniesienia jej do magazynu w warstwie Premium, aby replikacja w usłudze Site Recovery przebiegała wystarczająco szybko.
* W konsekwencji trzeba będzie wyłączyć i ponownie włączyć ochronę konta magazynu w warstwie Premium.

Zdecydowanie zalecamy zaplanowanie wzrostu podczas planowania wdrożenia i jeśli wartość domyślna to 30 procent. Ty najlepiej znasz wzorzec użycia swoich aplikacji i projekcje wzrostu, możesz więc odpowiednio zmienić tę wartość podczas generowania raportu. Ponadto możesz wygenerować wiele raportów dla różnych współczynników wzrostu i tych samych profilowanych danych oraz określić, które zalecenia dotyczące docelowego konta magazynu i przepustowości źródłowej będą dla Ciebie optymalne.

Wygenerowany raport programu Microsoft Excel zawiera następujące informacje:

* [Dane wejściowe](site-recovery-deployment-planner.md#input)
* [Zalecenia](site-recovery-deployment-planner.md#recommendations-with-desired-rpo-as-input)
* [Zalecenia — dane wejściowe przepustowości](site-recovery-deployment-planner.md#recommendations-with-available-bandwidth-as-input)
* [Rozmieszczenie maszyny wirtualnej względem magazynu](site-recovery-deployment-planner.md#vm-storage-placement)
* [Zgodne maszyny wirtualne](site-recovery-deployment-planner.md#compatible-vms)
* [Niezgodne maszyny wirtualne](site-recovery-deployment-planner.md#incompatible-vms)

![Planista wdrożenia](./media/site-recovery-deployment-planner/dp-report.png)

## <a name="get-throughput"></a>Uzyskiwanie informacji o przepływności

Aby oszacować przepływność osiągalną dla usługi Site Recovery podczas replikacji między środowiskiem lokalnym i platformą Azure, uruchom narzędzie w trybie uzyskiwania informacji o przepływności (GetThroughput). Narzędzie oblicza przepływność z serwera, na którym uruchomiono to narzędzie. W idealnej sytuacji ten serwer jest oparty na przewodniku dotyczącym zmian rozmiaru serwera konfiguracji. Jeśli wdrożono już lokalnie składniki infrastruktury usługi Site Recovery, uruchom narzędzie na serwerze konfiguracji.

Otwórz konsolę wiersza polecenia i przejdź do folderu narzędzia do planowania wdrożenia usługi Site Recovery. Uruchom program ASRDeploymentPlanner.exe z poniższymi parametrami.

`ASRDeploymentPlanner.exe -Operation GetThroughput /?`

|Nazwa parametru | Opis |
|-|-|
| -Operation | GetThroughput |
| -Directory | (Opcjonalnie) Ścieżka UNC lub ścieżka katalogu lokalnego, w której są przechowywane profilowane dane (pliki wygenerowane podczas profilowania). Te dane są wymagane do wygenerowania raportu. Jeśli nie określono nazwy katalogu, jest używany katalog „ProfiledData”. |
| -StorageAccountName | Nazwa konta magazynu używana w celu znalezienia użytej przepustowości na potrzeby replikacji danych ze środowiska lokalnego na platformę Azure. Narzędzie przekazuje dane testowe na to konto magazynu w celu określenia użytej przepustowości. |
| -StorageAccountKey | Klucz konta magazynu używany do uzyskiwania dostępu do konta magazynu. W witrynie Azure Portal przejdź do pozycji Konta magazynu > <*Nazwa_konta_magazynu*> > Ustawienia > Klucze dostępu > Klucz1 (lub podstawowy klucz dostępu w przypadku klasycznego konta magazynu). |
| -VMListFile | Plik zawierający listę maszyn wirtualnych, które mają być profilowane, na potrzeby obliczenia użytej przepustowości. Można użyć bezwzględnej lub względnej ścieżki pliku. Ten plik powinien zawierać jedną nazwę/jeden adres IP maszyny wirtualnej w każdym wierszu. Nazwy maszyn wirtualnych określone w pliku powinny być takie same jak nazw maszyn wirtualnych na serwerze vCenter/hoście vSphere ESXi.<br>Na przykład plik VMList.txt zawiera informacje o następujących maszynach wirtualnych:<ul><li>VM_A</li><li>10.150.29.110</li><li>VM_B</li></ul>|
| -Environment | (Opcjonalnie) Jest to docelowe środowisko konta usługi Azure Storage. Ten parametr może przyjmować jedną z trzech wartości — AzureCloud, AzureUSGovernment i AzureChinaCloud. Wartością domyślną jest AzureCloud. Użyj tego parametru, jeśli Twoim docelowym regionem świadczenia usługi Azure są chmury wersji platformy Azure dla administracji USA lub chińska wersja platformy Azure. |

Narzędzie tworzy kilka plików asrvhdfile<#>.vhd (gdzie znak „#” to liczba plików) o rozmiarze 64 MB w określonym katalogu. Narzędzie przekazuje te pliki na konto magazynu w celu znalezienia informacji o przepływności. Po zmierzeniu przepływności narzędzie usuwa wszystkie pliki z konta magazynu i serwera lokalnego. Jeśli działanie narzędzia zostanie z jakiegokolwiek powodu przerwane podczas obliczania przepływności, nie usunie ono plików z magazynu ani z serwera lokalnego. Trzeba będzie usunąć je ręcznie.

Przepływność jest mierzona w określonym momencie i jest maksymalną przepływnością osiągalną dla usługi Site Recovery podczas replikacji, jeśli wszystkie pozostałe czynniki pozostają bez zmian. Jeśli na przykład aplikacja zacznie używać większej przepustowości w tej samej sieci, rzeczywista przepływność będzie się zmieniać podczas replikacji. Jeśli używasz polecenia GetThroughput na serwerze konfiguracji, narzędzie nie uwzględnia chronionych maszyn wirtualnych ani replikacji w toku. Wynik pomiaru przepływności jest inny, jeśli operacja GetThroughput została uruchomiona przy wysokim współczynniku zmian danych chronionych maszyn wirtualnych. Zalecamy uruchomienie narzędzia w różnych momentach podczas profilowania, aby sprawdzić osiągalne poziomy przepływności w różnym czasie. Raport narzędzia zawiera ostatnio zmierzoną przepływność.

### <a name="example"></a>Przykład
```
ASRDeploymentPlanner.exe -Operation GetThroughput -Directory  E:\vCenter1_ProfiledData -VMListFile E:\vCenter1_ProfiledData\ProfileVMList1.txt  -StorageAccountName  asrspfarm1 -StorageAccountKey by8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==
```

>[!NOTE]
>
> Uruchom narzędzie na serwerze z magazynem i procesorem CPU o takiej samej charakterystyce jak serwer konfiguracji.
>
> W przypadku replikacji ustaw zalecaną przepustowość tak, aby spełniała wymagania celu punktu odzyskiwania przez 100 procent czasu. Jeśli nie widzisz wzrostu osiągniętej przepływności zgłoszonej przez narzędzie po ustawieniu odpowiedniej przepustowości, sprawdź następujące kwestie:
>
>  1. Ustal, czy w sieci jest używana technologia QoS (Quality of Service) ograniczająca przepływność usługi Site Recovery.
>
>  2. Ustal, czy magazyn usługi Site Recovery znajduje się w najbliższym obsługiwanym fizycznym regionie platformy Microsoft Azure, aby zminimalizować opóźnienie sieci.
>
>  3. Sprawdź charakterystyki magazynu lokalnego, aby ustalić, czy można ulepszyć sprzęt (np. zastąpić dysk twardy dyskiem SSD).
>
>  4. Zmień ustawienia usługi Site Recovery na serwerze przetwarzania, aby [zwiększyć przepustowość sieci na potrzeby replikacji](./site-recovery-plan-capacity-vmware.md#control-network-bandwidth).

## <a name="recommendations-with-desired-rpo-as-input"></a>Zalecenia dotyczące danych wejściowych w postaci żądanego celu punktu odzyskiwania

### <a name="profiled-data"></a>Profilowane dane

![Widok profilowanych danych w planiście wdrożenia](./media/site-recovery-deployment-planner/profiled-data-period.png)

**Okres profilowanych danych**: okres, podczas którego działało profilowanie. Domyślnie narzędzie uwzględnia w obliczeniu wszystkie profilowane dane, o ile nie jest generowany raport za okres wyznaczony przy użyciu opcji StartDate i EndDate podczas generowania raportu.

**Nazwa serwera**: nazwa lub adres IP hosta VMware vCenter lub ESXi, dla którego jest generowany raport dotyczący maszyn wirtualnych.

**Żądany cel punktu odzyskiwania**: cel punktu odzyskiwania dla danego wdrożenia. Domyślnie wymagana przepustowość sieci jest obliczana dla wartości celu punktu odzyskiwania równych 15, 30 i 60 minut. Odpowiednie wartości są aktualizowane w arkuszu zgodnie z wybraną wartością. W przypadku użycia parametru *DesiredRPOinMin* podczas generowania raportu ta wartość jest wyświetlana w obszarze wyniku żądanego celu punktu odzyskiwania.

### <a name="profiling-overview"></a>Omówienie profilowania

![Wyniki profilowania w planiście wdrożenia](./media/site-recovery-deployment-planner/profiling-overview.png)

**Łączna liczba profilowanych maszyn wirtualnych**: całkowita liczba maszyn wirtualnych, których profilowane dane są dostępne. Jeśli parametr VMListFile zawiera nazwy nieprofilowanych maszyn wirtualnych, te maszyny wirtualne nie zostaną uwzględnione podczas generowania raportów i zostaną wykluczone z łącznej liczby profilowanych maszyn wirtualnych.

**Zgodne maszyny wirtualne**: liczba maszyn wirtualnych, które mogą być chronione na platformie Azure przy użyciu usługi Site Recovery. Jest to łączna liczba zgodnych maszyn wirtualnych, dla których są obliczane: wymagana przepustowość sieci, liczba kont magazynu, liczba rdzeni platformy Azure oraz liczba serwerów konfiguracji i dodatkowych serwerów przetwarzania. Szczegóły wszystkich maszyn wirtualnych są dostępne w sekcji „Zgodne maszyny wirtualne”.

**Niezgodne maszyny wirtualne**: liczba profilowanych maszyn wirtualnych, które nie są zgodne na potrzeby ochrony za pomocą usługi Site Recovery. Przyczyny niezgodności wymieniono w sekcji „Niezgodne maszyny wirtualne”. Jeśli plik VMListFile zawiera nazwy maszyn wirtualnych, które nie były profilowane, te maszyny wirtualne są wykluczane z liczby niezgodnych maszyn wirtualnych. Dla tych maszyn wirtualnych jest wyświetlany tekst „Nie znaleziono danych” na końcu sekcji „Niezgodne maszyny wirtualne”.

**Żądany cel punktu odzyskiwania**: żądany cel punktu odzyskiwania w minutach. Raport jest generowany dla trzech wartości celu punktu odzyskiwania: 15 (ustawienie domyślne), 30 i 60 minut. Zalecenie dotyczące przepustowości w raporcie zmienia się zgodnie z pozycją wybraną z listy rozwijanej żądanego celu punktu odzyskiwania w prawym górnym rogu arkusza. Jeśli raport został wygenerowany przy użyciu parametru *-DesiredRPO* z wartością niestandardową, ta wartość będzie wyświetlana jako domyślna na liście rozwijanej żądanego celu punktu odzyskiwania.

### <a name="required-network-bandwidth-mbps"></a>Wymagana przepustowość sieci (Mb/s)

![Wymagana przepustowość sieci w planiście wdrożenia](./media/site-recovery-deployment-planner/required-network-bandwidth.png)

**Aby osiągnąć cel punktu odzyskiwania przez 100 procent czasu:** zalecana przepustowość (w Mb/s) do przydzielenia, która umożliwia spełnienie żądanego celu punktu odzyskiwania przez 100 procent czasu. Taka przepustowość musi zostać przeznaczona na stałą replikację przyrostową wszystkich zgodnych maszyn wirtualnych, aby uniknąć naruszeń celu punktu odzyskiwania.

**Aby osiągnąć cel punktu odzyskiwania przez 90 procent czasu**: jeśli z powodów związanych z cenami połączeń szerokopasmowych lub innych nie możesz ustawić przepustowości wymaganej do osiągnięcia żądanego celu punktu odzyskiwania przez 100 procent czasu, możesz wybrać mniejszą przepustowość umożliwiającą osiągnięcie żądanego celu punktu odzyskiwania przez 90 procent czasu. Aby umożliwić poznanie skutków ustawienia mniejszej przepustowości, w raporcie przedstawiono analizę warunkową liczby i czasu trwania naruszeń celu punktu odzyskiwania.

**Osiągnięta przepływność:** przepływność między serwerem, na którym uruchomiono polecenie GetThroughput, i regionem platformy Microsoft Azure, w którym znajduje się konto magazynu. Ta wartość przepływności wskazuje przybliżony poziom, który można osiągnąć podczas ochrony zgodnych maszyn wirtualnych za pomocą usługi Site Recovery, jeśli charakterystyka magazynu i sieci serwera konfiguracji/serwera przetwarzania pozostają takie same jak w przypadku serwera, na którym uruchomiono narzędzie.

W przypadku replikacji należy ustawić zalecaną przepustowość tak, aby spełniała wymagania celu punktu odzyskiwania przez 100 procent czasu. Jeśli nie widzisz wzrostu osiągniętej przepływności zgłoszonej przez narzędzie po ustawieniu przepustowości, sprawdź następujące kwestie:

1. Ustal, czy w sieci jest używana technologia QoS (Quality of Service) ograniczająca przepływność usługi Site Recovery.

2. Ustal, czy magazyn usługi Site Recovery znajduje się w najbliższym obsługiwanym fizycznym regionie platformy Microsoft Azure, aby zminimalizować opóźnienie sieci.

3. Sprawdź charakterystyki magazynu lokalnego, aby ustalić, czy można ulepszyć sprzęt (np. zastąpić dysk twardy dyskiem SSD).

4. Zmień ustawienia usługi Site Recovery na serwerze przetwarzania, aby [zwiększyć przepustowość sieci na potrzeby replikacji](./site-recovery-plan-capacity-vmware.md#control-network-bandwidth).

Jeśli narzędzie zostało uruchomione na serwerze konfiguracji lub serwerze przetwarzania, który ma już chronione maszyny wirtualne, należy uruchomić narzędzie kilka razy. Wartość osiągniętej przepływności zmienia się w zależności od wielkości współczynnika zmian przetwarzanego w danym momencie.

W przypadku wszystkich wdrożeń usługi Site Recovery w przedsiębiorstwach zalecamy użycie usługi [ExpressRoute](https://aka.ms/expressroute).

### <a name="required-storage-accounts"></a>Wymagane konta magazynu
Ten wykres przedstawia łączną liczbę kont magazynu (w warstwach Standardowa i Premium) wymaganych do ochrony wszystkich zgodnych maszyn wirtualnych. Aby dowiedzieć się, którego konta magazynu używać dla poszczególnych maszyn wirtualnych, zobacz sekcję „Rozmieszczenie maszyny wirtualnej względem magazynu”.

![Wymagane konta magazynu w planiście wdrożenia](./media/site-recovery-deployment-planner/required-azure-storage-accounts.png)

### <a name="required-number-of-azure-cores"></a>Wymagana liczba rdzeni platformy Azure
Wynik to łączna liczba rdzeni do skonfigurowania przed rozpoczęciem pracy w trybie failover lub testem pracy w trybie failover dla wszystkich zgodnych maszyn wirtualnych. Jeśli liczba rdzeni w ramach subskrypcji jest zbyt mała, usługa Site Recovery nie może utworzyć maszyn wirtualnych w czasie pracy w trybie failover lub testu pracy w trybie failover.

![Wymagana liczba rdzeni platformy Azure w planiście wdrożenia](./media/site-recovery-deployment-planner/required-number-of-azure-cores.png)

### <a name="required-on-premises-infrastructure"></a>Wymagana infrastruktura lokalna
Ta wartość to łączna liczba serwerów konfiguracji i dodatkowych serwerów przetwarzania do skonfigurowania w celu zapewnienia ochrony wszystkich zgodnych maszyn wirtualnych. W zależności od obsługiwanych [zaleceń dotyczących rozmiaru serwera konfiguracji](https://aka.ms/asr-v2a-on-prem-components) narzędzie może zalecić dodatkowe serwery. Zalecenie to opiera się na większej z następujących wartości: dziennego współczynnika zmian lub maksymalnej liczby chronionych maszyn wirtualnych (przy założeniu średnio trzech dysków na maszynę wirtualną) zależnie od tego, który z tych limitów zostanie osiągnięty jako pierwszy na serwerze konfiguracji lub dodatkowym serwerze przetwarzania. Szczegółowe informacje o łącznym dziennym współczynniku zmian oraz łącznej liczbie chronionych dysków znajdują się w sekcji „Dane wejściowe”.

![Wymagana infrastruktura lokalna w planiście wdrożenia](./media/site-recovery-deployment-planner/required-on-premises-infrastructure.png)

### <a name="what-if-analysis"></a>Analiza warunkowa
Ta analiza przedstawia liczbę naruszeń, które mogą wystąpić podczas profilowania w przypadku ustawienia mniejszej przepustowości umożliwiającej osiągnięcie żądanego celu punktu odzyskiwania tylko przez 90 procent czasu. W danym dniu może wystąpić co najmniej jedno naruszenie. Wykres przedstawia szczyt celu punktu odzyskiwania w danym dniu.
Na podstawie tej analizy można zdecydować, czy liczba naruszeń celu punktu odzyskiwania we wszystkich dniach i szczytowa wartość celu punktu odzyskiwania na dzień jest dopuszczalna przy określonej mniejszej przepustowości. Jeśli tak, można przydzielić mniejszą przepustowość na potrzeby replikacji. W przeciwnym razie przydziel większą przepustowość zgodnie z propozycją, aby osiągnąć żądany cel punktu odzyskiwania przez 100 procent czasu.

![Analiza warunkowa w planiście wdrożenia](./media/site-recovery-deployment-planner/what-if-analysis.png)

### <a name="recommended-vm-batch-size-for-initial-replication"></a>Zalecany rozmiar partii maszyn wirtualnych na potrzeby replikacji początkowej
W tej sekcji zalecamy liczbę maszyn wirtualnych, które mogą być chronione równolegle w celu ukończenia replikacji początkowej w ciągu 72 godzin przy proponowanej wydajności, aby spełniać wymagania celu punktu odzyskiwania przez 100 procent ustawianego czasu. Tę wartość można konfigurować. Aby zmienić ją podczas generowania raportu, użyj parametru *GoalToCompleteIR*.

Wykres w tym miejscu przedstawia zakres wartości przepustowości i obliczony rozmiar partii maszyn wirtualnych umożliwiające ukończenie replikacji początkowej w ciągu 72 godzin na podstawie wykrytego średniego rozmiaru wszystkich zgodnych maszyn wirtualnych.

W publicznej wersji zapoznawczej raport nie zawiera informacji o tym, które maszyny wirtualne mają być uwzględniane w partii. Możesz użyć rozmiaru dysku widocznego w sekcji „Zgodne maszyny wirtualne”, aby znaleźć rozmiar każdej maszyny wirtualnej i wybrać maszyny wirtualne dla partii albo na podstawie charakterystyki znanych obciążeń. Czas ukończenia replikacji początkowej zmienia się proporcjonalnie do rzeczywistego rozmiaru dysku maszyny wirtualnej, używanego miejsca na dysku i dostępnej przepływności sieci.

![Zalecany rozmiar partii zadań maszyny wirtualnej](./media/site-recovery-deployment-planner/recommended-vm-batch-size.png)

### <a name="growth-factor-and-percentile-values-used"></a>Używane wartości współczynnika wzrostu i percentyla
W tej sekcji w dolnej części arkusza pokazano wartość percentyla używaną przez wszystkie liczniki wydajności profilowanych maszyn wirtualnych (domyślnie jest używany 95. percentyl) i współczynnik wzrostu (wartość domyślna to 30 procent) używany we wszystkich obliczeniach.

![Używane wartości współczynnika wzrostu i percentyla](./media/site-recovery-deployment-planner/max-iops-and-data-churn-setting.png)

## <a name="recommendations-with-available-bandwidth-as-input"></a>Zalecenia dotyczące danych wejściowych w postaci dostępnej przepustowości

![Zalecenia dotyczące danych wejściowych w postaci dostępnej przepustowości](./media/site-recovery-deployment-planner/profiling-overview-bandwidth-input.png)

Może wystąpić sytuacja, w której nie można ustawić przepustowości większej niż x Mb/s na potrzeby replikacji usługi Site Recovery. Narzędzie umożliwia wprowadzenie dostępnej przepustowości (za pomocą parametru -Bandwidth podczas generowania raportu) i uzyskać osiągalny cel punktu odzyskiwania w minutach. Osiągalna wartość celu punktu odzyskiwania pozwala zdecydować, czy trzeba ustawić dodatkową przepustowość, czy rozwiązanie odzyskiwania po awarii może używać tego celu punktu odzyskiwania.

![Osiągalny cel punktu odzyskiwania dla przepustowości wynoszącej 500 Mb/s](./media/site-recovery-deployment-planner/achievable-rpos.png)

## <a name="input"></a>Dane wejściowe
Arkusz danych wejściowych zawiera omówienie profilowanego środowiska VMware.

![Omówienie profilowanego środowiska VMware](./media/site-recovery-deployment-planner/Input.png)

**Data rozpoczęcia** i **Data zakończenia**: daty rozpoczęcia i zakończenia profilowania danych na potrzeby generowania raportu. Domyślnie data rozpoczęcia to data rozpoczęcia profilowania, a data zakończenia to data zatrzymania profilowania. Mogą to być wartości „StartDate” i „EndDate”, jeśli raport jest generowany przy użyciu tych parametrów.

**Łączna liczba dni profilowania**: całkowita liczba dni profilowania od daty rozpoczęcia do daty zakończenia, dla których jest generowany raport.

**Liczba zgodnych maszyn wirtualnych**: łączna liczba zgodnych maszyn wirtualnych, dla których są obliczane wymagana przepustowość sieci oraz wymagana liczba kont magazynu, rdzeni platformy Microsoft Azure oraz serwerów konfiguracji i dodatkowych serwerów przetwarzania.

**Łączna liczba dysków na wszystkich zgodnych maszynach wirtualnych**: liczba używana jako dane wejściowe do określania liczby serwerów konfiguracji i dodatkowych serwerów przetwarzania do użycia we wdrożeniu.

**Średnia liczba dysków na zgodną maszynę wirtualną**: średnia liczba dysków obliczana dla wszystkich zgodnych maszyn wirtualnych.

**Średni rozmiar dysku (GB)**: średni rozmiar dysku obliczany dla wszystkich zgodnych maszyn wirtualnych.

**Żądany cel punktu odzyskiwania (w minutach)**: domyślny cel punktu odzyskiwania lub przekazana wartość parametru „DesiredRPO” podczas generowania raportu, które umożliwiają oszacowanie wymaganej przepustowości.

**Żądana przepustowość (Mb/s)**: przekazana wartość parametru „Bandwidth” podczas generowania raportu umożliwiająca oszacowanie osiągalnego celu punktu odzyskiwania.

**Zaobserwowany dzienny typowy współczynnik zmian danych (GB)**: średni współczynnik zmian danych zaobserwowany we wszystkie dni profilowania. Jest on używany jako dane wejściowe do określania liczby serwerów konfiguracji i dodatkowych serwerów przetwarzania do użycia we wdrożeniu.


## <a name="vm-storage-placement"></a>Rozmieszczenie maszyny wirtualnej względem magazynu

![Rozmieszczenie maszyny wirtualnej względem magazynu](./media/site-recovery-deployment-planner/vm-storage-placement.png)

**Typ magazynu dysków**: konto magazynu w warstwie Standardowa lub Premium używane do replikacji wszystkich odpowiednich maszyn wirtualnych wymienionych w kolumnie **Maszyny wirtualne do rozmieszczenia**.

**Sugerowany prefiks**: proponowany 3-znakowy prefiks, którego można użyć w nazwie konta magazynu. Możesz użyć własnego prefiksu, ale propozycja narzędzia będzie zgodna z [konwencją nazewnictwa partycji dla kont magazynu](https://aka.ms/storage-performance-checklist).

**Sugerowana nazwa konta**: nazwa konta magazynu po uwzględnieniu proponowanego prefiksu. Zastąp nazwę w nawiasach kątowych (< i >) niestandardowymi danymi wejściowymi.

**Konto magazynu dzienników**: wszystkie dzienniki replikacji są przechowywane na standardowym koncie magazynu. W przypadku maszyn wirtualnych replikowanych do konta magazynu w warstwie Premium skonfiguruj dodatkowe konto magazynu w warstwie Standardowa na potrzeby magazynu dzienników. Jedno konto magazynu dzienników w warstwie Standardowa może być używane przez wiele kont magazynu replikacji w warstwie Premium. Maszyny wirtualne replikowane do kont magazynu w warstwie Standardowa używają tego samego konta magazynu dla dzienników.

**Sugerowana nazwa konta dzienników**: nazwa konta dzienników magazynu po uwzględnieniu proponowanego prefiksu. Zastąp nazwę w nawiasach kątowych (< i >) niestandardowymi danymi wejściowymi.

**Podsumowanie rozmieszczania**: podsumowanie łącznego obciążenia maszyn wirtualnych na koncie magazynu podczas replikacji i pracy w trybie failover lub testu pracy w trybie failover. Obejmuje ono łączną liczbę maszyn wirtualnych mapowanych na konto magazynu, łączną liczbę operacji we/wy odczytu i zapisu na sekundę dla wszystkich maszyn wirtualnych umieszczanych na tym koncie magazynu, łączną liczbę operacji we/wy zapisu na sekundę (replikacja), łączny skonfigurowany rozmiar wszystkich dysków oraz łączną liczbę dysków.

**Maszyny wirtualne do rozmieszczenia**: lista wszystkich maszyn wirtualnych, które powinny zostać umieszczone na danym koncie magazynu w celu uzyskania optymalnej wydajności i użycia.

## <a name="compatible-vms"></a>Zgodne maszyny wirtualne
![Arkusz kalkulacyjny programu Excel zawierający zgodne maszyny wirtualne](./media/site-recovery-deployment-planner/compatible-vms.png)

**Nazwa maszyny wirtualnej**: nazwa lub adres IP maszyny wirtualnej używany w pliku VMListFile podczas generowania raportu. Ta kolumna obejmuje też dyski (VMDK) dołączone do maszyn wirtualnych. Aby wyróżnić maszyny wirtualne vCenter o zduplikowanych nazwach lub adresach IP, nazwy zawierają nazwę hosta ESXi. Wymieniony host ESXi to host, na którym umieszczono maszynę wirtualną odnaleziono w trakcie okresu profilowania.

**Zgodność maszyny wirtualnej**: wartości to **Tak** i **Tak**\*. Wartość **Tak**\* jest przeznaczona dla wystąpień, w których maszyna wirtualna odpowiada usłudze [Azure Premium Storage](https://aka.ms/premium-storage-workload). Tutaj profilowany dysk o dużym współczynniku zmian lub dużej liczbie operacji we/wy należy do kategorii P20 lub P30, ale z powodu swojego rozmiaru jest mapowany w dół do kategorii P10 lub P20. Decyzja o tym, do którego typu dysku magazynu Premium będzie mapowany dysk, jest podejmowana na podstawie jego rozmiaru na poziomie konta magazynu. Na przykład:
* Mniej niż 128 GB — P10.
* 128 GB do 512 GB — P20.
* 512 GB do 1024 GB — P30.
* 1025 GB do 2048 GB — P40.
* 2049 GB do 4095 GB — P50.

Jeśli charakterystyki obciążenia dysku powodują umieszczenie go w kategorii P20 lub P30, ale z powodu rozmiaru jest mapowany w dół do niższego typu magazynu Premium, narzędzie oznacza daną maszynę wirtualną jako **Tak**\*. Narzędzie zaleca również zmianę rozmiaru dysku źródłowego tak, aby mieścił się w zalecanym typie dysku Premium Storage lub zmianę docelowego typu dysku po zakończeniu pracy w trybie failover.

**Typ magazynu**: dostępne typy magazynu to Standardowa i Premium.

**Sugerowany prefiks**: 3-znakowy prefiks konta magazynu.

**Konto magazynu**: nazwa uwzględniająca proponowany prefiks konta magazynu.

**Operacje we/wy odczytu i zapisu na sekundę (ze współczynnikiem wzrostu)**: liczba operacji we/wy odczytu i zapisu na sekundę dla szczytowego obciążenia na dysku (domyślnie jest używany 95. percentyl) wraz z przyszłym współczynnikiem wzrostu (wartość domyślna to 30 procent). Pamiętaj, że łączna liczba operacji we/wy odczytu i zapisu maszyny wirtualnej nie zawsze jest sumą operacji we/wy odczytu i zapisu z poszczególnych dysków maszyny wirtualnej, ponieważ szczytowa liczba operacji we/wy odczytu i zapisu maszyny wirtualnej to wartość szczytowa sumy operacji we/wy odczytu i zapisu poszczególnych dysków z każdej minuty okresu profilowania.

**Współczynnik zmian danych w Mb/s (ze współczynnikiem wzrostu)**: szczytowy współczynnik zmian danych na dysku (domyślnie jest używany 95. percentyl) wraz z przyszłym współczynnikiem wzrostu (wartość domyślna to 30 procent). Pamiętaj, że łączny współczynnik zmian danych maszyny wirtualnej nie zawsze jest sumą współczynników zmian danych z poszczególnych dysków maszyny wirtualnej, ponieważ szczytowy współczynnik zmian danych maszyny wirtualnej to wartość szczytowa sumy współczynników zmian poszczególnych dysków z każdej minuty okresu profilowania.

**Rozmiar maszyny wirtualnej platformy Azure**: idealnie zamapowany rozmiar maszyny wirtualnej usług Azure Cloud Services dla tej lokalnej maszyny wirtualnej. Mapowanie jest oparte na wielkości pamięci, liczbie dysków/rdzeni/kart sieciowych oraz liczbie operacji we/wy zapisu i odczytu lokalnej maszyny wirtualnej. Zawsze zalecany jest najmniejszy rozmiar maszyny wirtualnej platformy Azure zgodny ze wszystkimi charakterystykami lokalnej maszyny wirtualnej.

**Liczba dysków**: łączna liczba dysków (VMDK) maszyny wirtualnej.

**Rozmiar dysku (GB)**: łączny skonfigurowany rozmiar wszystkich dysków maszyny wirtualnej. W narzędziu jest też wyświetlany rozmiar poszczególnych dysków maszyny wirtualnej.

**Rdzenie**: liczba rdzeni procesora CPU maszyny wirtualnej.

**Pamięć (MB)**: pamięć RAM maszyny wirtualnej.

**Karty sieciowe**: liczba kart sieciowych maszyny wirtualnej.

**Typ rozruchu**: typ rozruchu maszyny wirtualnej. Dozwolone wartości to BIOS i EFI. Obecnie usługa Azure Site Recovery obsługuje tylko typ rozruchu BIOS. Wszystkie maszyny wirtualne o typie rozruchu EFI są wymienione w arkuszu niezgodnych maszyn wirtualnych.

**Typ systemu operacyjnego**: typ systemu operacyjnego maszyny wirtualnej. Dozwolone wartości to Windows, Linux i inny.

## <a name="incompatible-vms"></a>Niezgodne maszyny wirtualne

![Arkusz kalkulacyjny programu Excel zawierający niezgodne maszyny wirtualne](./media/site-recovery-deployment-planner/incompatible-vms.png)

**Nazwa maszyny wirtualnej**: nazwa lub adres IP maszyny wirtualnej używany w pliku VMListFile podczas generowania raportu. Ta kolumna obejmuje też dyski VMDK dołączone do maszyn wirtualnych. Aby wyróżnić maszyny wirtualne vCenter o zduplikowanych nazwach lub adresach IP, nazwy zawierają nazwę hosta ESXi. Wymieniony host ESXi to host, na którym umieszczono maszynę wirtualną odnaleziono w trakcie okresu profilowania.

**Zgodność maszyny wirtualnej**: wskazuje, dlaczego dana maszyna wirtualna nie jest zgodna na potrzeby użycia z usługą Site Recovery. Niezgodność każdego dysku na podstawie opublikowanych [limitów magazynów](https://aka.ms/azure-storage-scalbility-performance) może wynikać z dowolnej spośród następujących przyczyn:

* Rozmiar dysku jest większy niż 4095 GB. Usługa Azure Storage obecnie nie obsługuje dysków danych większych niż 4095 GB.
* Rozmiar dysku systemu operacyjnego jest większy niż 2048 GB. Usługa Azure Storage obecnie nie obsługuje dysków systemów operacyjnych większych niż 2048 GB.
* Typ rozruchu to EFI. Obecnie usługa Azure Site Recovery obsługuje tylko maszyny wirtualne o typie rozruchu BIOS.

* Łączny rozmiar maszyny wirtualnej (suma replikacji i testu pracy w trybie failover) przekracza obsługiwany limit rozmiaru konta magazynu (35 TB). Ta niezgodność występuje przeważnie, jeśli wartość charakterystyki wydajności pojedynczego dysku maszyny wirtualnej przekracza maksymalny obsługiwany limit standardowego magazynu platformy Azure lub usługi Site Recovery. Takie wystąpienie powoduje przeniesienie do strefy magazynów Premium Storage. Jednak maksymalny obsługiwany rozmiar konta magazynu Premium Storage jest równy 35 TB i jedna chroniona maszyna wirtualna nie może być chroniona na wielu kontach magazynu. Zauważ również, że przeprowadzenie testu pracy w trybie failover na chronionej maszynie wirtualnej powoduje również uruchomienie go na koncie magazynu z trwającą replikacją. W takiej sytuacji skonfiguruj podwojony rozmiar dysku na potrzeby równoległej kontynuacji replikacji i pomyślnie przeprowadzonego testu pracy w trybie failover.
* Źródłowe operacje we/wy na sekundę przekraczają obsługiwany limit operacji we/wy na sekundę magazynu wynoszący 5000 operacji na dysk.
* Źródłowe operacje we/wy na sekundę przekraczają obsługiwany limit operacji we/wy na sekundę maszyny wirtualnej wynoszący 80 000 operacji na dysk.
* Średni współczynnik zmian danych przekracza obsługiwany limit współczynnika zmian danych usługi Site Recovery wynoszący 10 MB/s dla średniego rozmiaru operacji we/wy na dysku.
* Łączny współczynnik zmian danych dla wszystkich dysków na maszynie wirtualnej przekracza maksymalny obsługiwany limit współczynnika zmian danych usługi Site Recovery wynoszący 54 MB/s na maszynę wirtualną.
* Średnia liczba operacji we/wy zapisu na sekundę przekracza obsługiwany limit operacji we/wy na sekundę usługi Site Recovery wynoszący 840 operacji na dysk.
* Obliczony magazyn migawek przekracza obsługiwany limit magazynu migawek wynoszący 10 TB.

**Operacje we/wy odczytu i zapisu na sekundę (ze współczynnikiem wzrostu)**: liczba operacji we/wy na sekundę dla szczytowego obciążenia na dysku (domyślnie jest używany 95. percentyl) wraz z przyszłym współczynnikiem wzrostu (wartość domyślna to 30 procent). Pamiętaj, że łączna liczba operacji we/wy odczytu i zapisu maszyny wirtualnej nie zawsze jest sumą operacji we/wy odczytu i zapisu z poszczególnych dysków maszyny wirtualnej, ponieważ szczytowa liczba operacji we/wy odczytu i zapisu maszyny wirtualnej to wartość szczytowa sumy operacji we/wy odczytu i zapisu poszczególnych dysków z każdej minuty okresu profilowania.

**Współczynnik zmian danych w Mb/s (ze współczynnikiem wzrostu)**: szczytowy współczynnik zmian danych na dysku (domyślnie jest używany 95. percentyl) wraz z przyszłym współczynnikiem wzrostu (wartość domyślna to 30 procent). Pamiętaj, że łączny współczynnik zmian danych maszyny wirtualnej nie zawsze jest sumą współczynników zmian danych z poszczególnych dysków maszyny wirtualnej, ponieważ szczytowy współczynnik zmian danych maszyny wirtualnej to wartość szczytowa sumy współczynników zmian poszczególnych dysków z każdej minuty okresu profilowania.

**Liczba dysków**: łączna liczba dysków VMDK maszyny wirtualnej.

**Rozmiar dysku (GB)**: łączny skonfigurowany rozmiar wszystkich dysków maszyny wirtualnej. W narzędziu jest też wyświetlany rozmiar poszczególnych dysków maszyny wirtualnej.

**Rdzenie**: liczba rdzeni procesora CPU maszyny wirtualnej.

**Pamięć (MB)**: wielkość pamięci RAM maszyny wirtualnej.

**Karty sieciowe**: liczba kart sieciowych maszyny wirtualnej.

**Typ rozruchu**: typ rozruchu maszyny wirtualnej. Dozwolone wartości to BIOS i EFI. Obecnie usługa Azure Site Recovery obsługuje tylko typ rozruchu BIOS. Wszystkie maszyny wirtualne o typie rozruchu EFI są wymienione w arkuszu niezgodnych maszyn wirtualnych.

**Typ systemu operacyjnego**: typ systemu operacyjnego maszyny wirtualnej. Dozwolone wartości to Windows, Linux i inny.


## <a name="site-recovery-limits"></a>Limity usługi Site Recovery

**Cel magazynu replikacji** | **Średni rozmiar źródłowych operacji we/wy na dysku** |**Średni źródłowy współczynnik zmian danych na dysku** | **Łączny współczynnik zmian danych na dysku dziennie**
---|---|---|---
Standard Storage | 8 KB | 2 MB/s | 168 GB na dysk
Dysk Premium P10 | 8 KB | 2 MB/s | 168 GB na dysk
Dysk Premium P10 | 16 KB | 4 MB/s | 336 GB na dysk
Dysk Premium P10 | 32 KB lub większy | 8 MB/s | 672 GB na dysk
Dysk Premium P20 lub P30 | 8 KB  | 5 MB/s | 421 GB na dysk
Dysk Premium P20 lub P30 | 16 KB lub większy |10 MB/s | 842 GB na dysk

Są to średnie wartości przy założeniu 30-procentowego nakładania się operacji we/wy. Usługa Site Recovery może obsługiwać większą przepływność na podstawie zakresu nakładania się na siebie, większego rozmiaru operacji zapisu i rzeczywistego zachowania związanego z obciążeniem operacji we/wy. Poprzednie liczby zakładają typowe zaległości wynoszące około pięć minut. Oznacza to, że przekazane dane są przetwarzane i punkt odzyskiwania jest tworzony w ciągu pięciu minut.

Limity te są oparte na naszych testach, ale nie obejmują wszystkich możliwych kombinacji operacji we/wy aplikacji. Rzeczywiste wyniki mogą różnić w zależności od kombinacji operacji we/wy aplikacji. Aby uzyskać najlepsze wyniki nawet po zakończeniu planowania wdrożenia, zawsze zalecamy dokładne przetestowanie aplikacji przy użyciu testu pracy w trybie failover w celu uzyskania prawdziwych informacji o wydajności.

## <a name="updating-the-deployment-planner"></a>Aktualizowanie planisty wdrożenia
Aby zaktualizować planistę wdrożenia, wykonaj następujące czynności:

1. Pobierz najnowszą wersję [planisty wdrożenia usługi Azure Site Recovery](https://aka.ms/asr-deployment-planner).

2. Skopiuj folder ZIP na serwer, na którym chcesz go uruchomić.

3. Wyodrębnij folder ZIP.

4. Wykonaj jedną z następujących czynności:
 * Jeśli najnowsza wersja nie zawiera poprawki profilowania i profilowanie jest już w toku w bieżącej wersji planisty, kontynuuj profilowanie.
 * Jeśli najnowsza wersja zawiera poprawkę profilowania, zalecamy zatrzymanie profilowania w bieżącej wersji, a następnie jego ponowne uruchomienie w nowej wersji.

  >[!NOTE]
  >
  >W przypadku uruchamiania profilowania w nowej wersji przekaż taką samą ścieżkę katalogu wyjściowego, aby narzędzie dołączało dane profilu do istniejących plików. Do wygenerowania raportu zostanie użyty kompletny zestaw profilowanych danych. Jeśli przekażesz inny katalog danych wyjściowych, zostaną utworzone nowe pliki, a stare profilowane dane nie będą używane podczas generowania raportu.
  >
  >Każdy nowy planista wdrożenia jest aktualizacją zbiorczą pliku ZIP. Nie musisz kopiować najnowszych plików do poprzedniego folderu. Można utworzyć nowy folder i użyć go.


## <a name="version-history"></a>Historia wersji

### <a name="131"></a>1.3.1
Aktualizacja: 19 lipca 2017 r.

Dodano następującą nową funkcję:

* Dodano obsługę dużych dysków (powyżej 1 TB) na potrzeby generowania raportów. Teraz można użyć planisty wdrożenia, aby zaplanować replikację maszyn wirtualnych z dyskami o rozmiarze większym niż 1 TB (maksymalnie 4095 GB).
Dowiedz się więcej na temat [obsługi dużych dysków w usłudze Azure Site Recovery](https://azure.microsoft.com/en-us/blog/azure-site-recovery-large-disks/)


### <a name="13"></a>1.3
Aktualizacja: 9 maja 2017 r.

Dodano następującą nową funkcję:

* Dodano obsługę dysku zarządzanego na potrzeby generowania raportów. Liczba maszyn wirtualnych, które można umieścić w ramach pojedynczego konta magazynu, jest obliczana na podstawie tego, czy dla pracy w trybie failover/testu pracy w trybie failover wybrano dysk zarządzany.        


### <a name="12"></a>1.2
Zaktualizowano: 7 kwietnia 2017 r.

Dodano następujące poprawki:

* Dodano test typu rozruchu (BIOS lub EFI) dla każdej maszyny wirtualnej, aby określić, czy maszyna wirtualna jest zgodna, czy niezgodna z funkcją ochrony.
* Dodano informacje o systemie operacyjnym dla każdej maszyny wirtualnej na arkuszach zgodnych maszyn wirtualnych i niezgodnych maszyn wirtualnych.
* Operacja GetThroughput jest teraz obsługiwana w regionach świadczenia usługi Microsoft Azure Administracja USA i Chiny.
* Dodano kilka testów wymagań wstępnych dla serwerów vCenter i ESXi.
* Po skonfigurowaniu ustawień regionalnych jako inne niż angielskie generowany był nieprawidłowy raport.


### <a name="11"></a>1.1
Aktualizacja: 9 marca 2017 r.

Rozwiązano następujące problemy:

* Narzędzie nie może profilować maszyn wirtualnych, jeśli serwer vCenter ma co najmniej dwie maszyny wirtualne o tej samej nazwie lub adresie IP na różnych hostach ESXi.
* Kopiowanie i wyszukiwanie zostało wyłączone dla arkuszy zgodnych maszyn wirtualnych i niezgodnych maszyn wirtualnych.

### <a name="10"></a>1.0
Aktualizacja: 23 lutego 2017 r.

Planista wdrożenia usługi Azure Site Recovery w publicznej wersji zapoznawczej 1.0 ma następujące znane problemy (które zostaną rozwiązane w przyszłych aktualizacjach):

* Narzędzie działa tylko w przypadku replikacji z oprogramowania VMware na platformę Azure. Nie działa w przypadku wdrożeń funkcji Hyper-V na platformie Azure. W przypadku wdrożeń funkcji Hyper-V na platformie Azure użyj [narzędzia planisty wydajności funkcji Hyper-V](./site-recovery-capacity-planning-for-hyper-v-replication.md).
* Operacja GetThroughput nie jest obsługiwana w regionach świadczenia usługi Microsoft Azure Administracja USA i Chiny.
* Narzędzie nie może profilować maszyn wirtualnych, jeśli serwer vCenter ma co najmniej dwie maszyny wirtualne o tej samej nazwie lub adresie IP na różnych hostach ESXi. W tej wersji narzędzie pomija profilowanie w przypadku zduplikowanych nazw lub adresów IP w parametrze VMListFile. Obejście polega na profilowaniu maszyn wirtualnych przy użyciu hosta ESXi zamiast serwera vCenter. Musisz uruchomić jedno wystąpienie każdego hosta ESXi.
