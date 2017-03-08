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
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 2/21/2017
ms.author: nisoneji
translationtype: Human Translation
ms.sourcegitcommit: d9dad6cff80c1f6ac206e7fa3184ce037900fc6b
ms.openlocfilehash: 4e444deaa84c7f02608f4910e31f7033df51a73b
ms.lasthandoff: 03/06/2017


---
#<a name="azure-site-recovery-deployment-planner"></a>Planista wdrożenia usługi Azure Site Recovery
Ten podręcznik użytkownika dotyczy planisty wdrożenia usługi Azure Site Recovery dla wdrożeń produkcyjnych oprogramowania VMware na platformie Azure.


##<a name="overview"></a>Omówienie

Zanim zaczniesz chronić maszyny wirtualne oprogramowania VMware za pomocą usługi Azure Site Recovery, musisz przydzielić odpowiednią przepustowość zgodnie z częstotliwością dziennych zmian danych, aby osiągnąć założony cel punktu odzyskiwania. Musisz wdrożyć lokalnie odpowiednią liczbę serwerów konfiguracji i serwerów przetwarzania. Musisz także utworzyć właściwą liczbę docelowych kont usługi Azure Storage odpowiedniego typu w warstwie Standardowa lub Premium, uwzględniając wzrost na źródłowych serwerach produkcyjnych spowodowany przez użycie zwiększające się wraz z upływem czasu. Typ magazynu jest określany dla poszczególnych maszyn wirtualnych na podstawie charakterystyki obciążenia (operacji we/wy odczytu i zapisu na sekundę oraz współczynnika zmian danych) i limitów usługi Azure Site Recovery.  

Planista wdrożenia usługi Azure Site Recovery w publicznej wersji zapoznawczej to narzędzie wiersza polecenia dostępne obecnie tylko na potrzeby replikacji z oprogramowania VMware na platformę Azure. To narzędzie pozwala zdalnie profilować maszyny wirtualne oprogramowania VMware (bez żadnego wpływu na środowisko produkcyjne), aby poznać wymagania dotyczące przepustowości i magazynu Azure Storage dla udanej replikacji oraz testu pracy w trybie failover.  Możesz uruchomić narzędzie bez instalowania składników lokalnych usługi Azure Site Recovery, jednak w celu uzyskania dokładnych wyników osiągniętej przepływności zaleca się uruchomienie planisty na serwerze z systemem Windows Server spełniającym minimalne wymagania dotyczące serwera konfiguracji usługi Azure Site Recovery, który trzeba będzie wdrożyć na początku procesu wdrażania w środowisku produkcyjnym.

Narzędzie udostępnia następujące szczegóły:

**Ocena zgodności**<br>
* Ocena uprawnień maszyny wirtualnej na podstawie liczby dysków, rozmiaru dysku, liczby operacji we/wy na sekundę i współczynnika zmian

**Ocena zapotrzebowania na przepustowość sieci względem celu punktu odzyskiwania**<br>
* Szacowana przepustowość sieci wymagana na potrzeby replikacji przyrostowej<br>
* Przepływność, którą usługa Azure Site Recovery może uzyskać między środowiskiem lokalnym i platformą Azure<br>
* Liczba maszyn wirtualnych do przetworzenia wsadowego na podstawie szacowanej przepustowości wymagana do ukończenia replikacji początkowej w określonym czasie<br>

**Wymagania dotyczące infrastruktury platformy Microsoft Azure**<br>
* Wymagania dotyczące typu magazynu (w warstwie Standardowa lub Premium) dla poszczególnych maszyn wirtualnych<br>
* Łączna liczba kont magazynu w warstwie Standardowa i Premium do aprowizacji na potrzeby replikacji<br>
* Sugestie dotyczące nazw kont magazynu oparte na wskazówkach usługi Azure Storage<br>
* Położenie konta magazynu każdej maszyny wirtualnej<br>
* Liczba rdzeni platformy Microsoft Azure do aprowizacji przed rozpoczęciem pracy w trybie failover lub testu pracy w trybie failover w ramach subskrypcji<br>
* Zalecany rozmiar maszyny wirtualnej platformy Microsoft Azure dla każdej lokalnej maszyny wirtualnej<br>

**Wymagania dotyczące infrastruktury lokalnej**<br>
* Wymagana liczba serwerów konfiguracji i serwerów przetwarzania do wdrożenia lokalnie<br>

>[!IMPORTANT]
>
>Wszystkie obliczenia w narzędziu są wykonywane przy założeniu, że współczynnik wzrostu w charakterystyce obciążenia wynosi 30%, ze względu na możliwe zwiększone użycie wraz z upływem czasu oraz przy użyciu 95. percentyla wszystkich metryk profilowania (operacji we/wy odczytu i zapisu na sekundę, współczynnika zmian itp.). Oba te parametry (współczynnik wzrostu i obliczenie procentowe) można konfigurować. Dowiedz się więcej o [współczynniku wzrostu](site-recovery-deployment-planner.md#growth-factor) i [wartości percentyla używanych w obliczeniach](site-recovery-deployment-planner.md#percentile-value-used-for-the-calculation).
>


## <a name="requirements"></a>Wymagania
Narzędzie obejmuje dwa główne etapy — profilowanie i generowanie raportu. Jest też dostępny trzeci etap umożliwiający tylko obliczanie przepływności. Poniżej przedstawiono wymagania dotyczące serwera, na którym rozpoczyna się profilowanie/pomiar przepływności.

| Wymaganie | Opis|
|---|---|
|Profilowanie i pomiar przepływności| <br>System operacyjny: Microsoft Windows Server 2012 R2 <br>Rozmiar zgodny z podanym [rozmiarem](https://aka.ms/asr-v2a-on-prem-components) serwera konfiguracji lub większy<br>Konfiguracja maszyny: 8 wirtualnych procesorów CPU, 16 GB pamięci RAM, dysk twardy o rozmiarze 300 GB <br />[Microsoft .NET Framework 4.5](https://aka.ms/dotnet-framework-45)<br>[VMware vSphere PowerCLI 6.0 R3](https://developercenter.vmware.com/tool/vsphere_powercli/6.0)<br>[Pakiet Microsoft Visual C++ Redistributable dla programu Visual Studio 2012](https://aka.ms/vcplusplus-redistributable)<br> Dostęp do platformy Microsoft Azure przez Internet z tego serwera<br> Konto usługi Microsoft Azure Storage<Br>Dostęp administratora na serwerze<br>Co najmniej 100 GB wolnego miejsca na dysku (przy założeniu 1000 maszyn wirtualnych ze średnio 3 dyskami profilowanymi przez 30 dni)|
| Generowanie raportu| Dowolny komputer z systemem Windows lub Windows Server i programem Microsoft Excel 2013 lub nowszym |
| Uprawnienia użytkowników | Uprawnienia tylko do odczytu dla konta użytkownika używanego do uzyskiwania dostępu do serwera VMware vCenter/vSphere podczas profilowania|


> [!NOTE]
>
> Narzędzie umożliwia profilowanie tylko maszyn wirtualnych z dyskami VMDK i RDM. Nie pozwala profilować maszyn wirtualnych z dyskami iSCSI i NFS. Usługa Azure Site Recovery obsługuje dyski iSCSI i NFS w przypadku serwerów VMware, jeśli planista wdrożenia nie znajduje się na gościu i nie profiluje tylko przy użyciu liczników wydajności programu vCenter, jednak narzędzie nie ma wglądu w typy dysków.
>


##<a name="download"></a>Do pobrania
[Pobierz](https://aka.ms/asr-deployment-planner) najnowszą wersję planisty wdrożenia usługi Azure Site Recovery w ramach publicznej wersji zapoznawczej.  Narzędzie jest spakowane w formacie zip.  Bieżąca wersja narzędzia obsługuje tylko replikację z oprogramowania VMware na platformę Azure.

Skopiuj plik zip na serwer z systemem Windows Server, z którego chcesz uruchomić narzędzie. Możesz uruchomić narzędzie na dowolnym serwerze z systemem Windows Server 2012 R2 z dostępem do sieci w celu połączenia się z serwerem VMware vCenter lub hostem VMware vSphere ESXi, na którym znajdują się maszyny wirtualne do profilowania, jednak zaleca się uruchomienie narzędzia na serwerze, którego konfiguracja sprzętowa jest zgodna ze [wskazówkami dotyczącymi rozmiaru serwera konfiguracji](https://aka.ms/asr-v2a-on-prem-components).  Jeśli wdrożono już lokalnie składniki usługi Azure Site Recovery, uruchom narzędzie na serwerze konfiguracji. Zaleca się używanie takiej samej konfiguracji sprzętowej na serwerze konfiguracji (z wbudowanym serwerem przetwarzania) i na serwerze, na którym jest uruchamiane narzędzie, aby osiągana przepływność zgłaszana przez narzędzie była zgodna z rzeczywistą przepływnością, którą usługa Azure Site Recovery może osiągnąć podczas replikacji — obliczenie przepływności zależy od dostępnej przepustowości sieci na serwerze i konfiguracji sprzętowej serwera (procesorów CPU, ilości miejsca do magazynowania itp.). Jeśli uruchomisz narzędzie na dowolnym innym serwerze, zostanie obliczona przepływność między tym serwerem i platformą Microsoft Azure. Ponadto konfiguracja sprzętowa serwera może nie być zgodna z serwerem konfiguracji, dlatego osiągana przepływność zgłaszana przez narzędzie nie będzie dokładna.

Wyodrębnij folder zip. Zawiera on wiele plików i podfolderów. Plik wykonywalny narzędzia nosi nazwę ASRDeploymentPlanner.exe i znajduje się w folderze nadrzędnym.

Przykład: skopiuj plik zip na dysk E:\ i wyodrębnij go.
E:\ASR Deployment Planner-Preview_v1.0.zip

E:\ASR Deployment Planner-Preview_v1.0\ ASR Deployment Planner-Preview_v1.0\ ASRDeploymentPlanner.exe

##<a name="capabilities"></a>Możliwości
Narzędzie wiersza polecenia (ASRDeploymentPlanner.exe) można uruchomić w jednym z trzech następujących trybów:

1.    Profilowanie  
2.    Generowanie raportu
3.    Uzyskiwanie informacji o przepływności

Najpierw musisz uruchomić narzędzie w trybie profilowania, aby zebrać współczynnik zmian danych i liczbę operacji we/wy na sekundę maszyn wirtualnych.  Następnie uruchom narzędzie w celu wygenerowania raportu, aby uzyskać wymagania dotyczące przepustowości i ilości miejsca do magazynowania.

##<a name="profiling"></a>Profilowanie
W trybie profilowania narzędzie planisty wdrożenia łączy się z hostami serwera vCenter lub hostami vSphere ESXi w celu zbierania danych wydajności maszyny wirtualnej.

* Profilowanie nie wpływa na wydajność produkcyjnych maszyn wirtualnych, ponieważ nie jest nawiązywane bezpośrednie połączenie z produkcyjną maszyną wirtualną. Wszystkie dane wydajności są zbierane z serwera vCenter lub hosta vSphere ESXi.
* Na serwer vCenter lub hosta vSphere EXSi jest przesyłane co 15 minut zapytanie, aby upewnić się, że profilowanie ma niewielki wpływ na wydajność serwera. Nie ma to jednak wpływu na dokładność profilowania, ponieważ narzędzie zapisuje dane liczników wydajności z każdej minuty.

####<a name="create-a-list-of-virtual-machines-to-profile"></a>Tworzenie listy maszyn wirtualnych do profilowania
Najpierw musisz utworzyć listę maszyn wirtualnych, które chcesz profilować. Wszystkie nazwy maszyn wirtualnych na hoście VMware vCenter lub VMware vSphere ESXi można uzyskać za pomocą poniższych poleceń programu VMware vSphere PowerCLI. Można też ręcznie utworzyć plik z listą przyjaznych nazw/adresów IP maszyn wirtualnych do profilowania.

1.    Zaloguj się do maszyny wirtualnej, na której jest zainstalowany program VMware vSphere PowerCLI
2.    Otwórz konsolę programu VMware vSphere PowerCLI
3.    Upewnij się, że zasady wykonywania skryptu nie są wyłączone. Jeśli są one wyłączone, uruchom konsolę programu VMware vSphere PowerCLI w trybie administratora i uruchom następujące polecenie, aby włączyć zasady:

            Set-ExecutionPolicy –ExecutionPolicy AllSigned

4.    Uruchom dwa poniższe polecenia, aby uzyskać wszystkie nazwy maszyn wirtualnych na hoście VMware vCenter lub VMware vSphere ESXi, i zapisz je w pliku txt.
Zamień wartości &lsaquo;server name&rsaquo;, &lsaquo;user name&rsaquo;, &lsaquo;password&rsaquo; i &lsaquo;outputfile.txt&rsaquo; na własne wartości.

            Connect-VIServer -Server <server name> -User <user name> -Password <password>

            Get-VM |  Select Name | Sort-Object -Property Name >  <outputfile.txt>


5.    Otwórz plik wyjściowy w Notatniku. Skopiuj nazwy wszystkich maszyn wirtualnych, które chcesz profilować, do innego pliku (np. lista_mw_profilowania.txt), umieszczając jedną nazwę maszyny wirtualnej w wierszu. Ten plik będzie używany jako źródło danych wejściowych dla parametru -VMListFile narzędzia wiersza polecenia

    ![Planista wdrożenia](./media/site-recovery-deployment-planner/profile-vm-list.png)


####<a name="start-profiling"></a>Rozpoczynanie profilowania
Po utworzeniu listy maszyn wirtualnych do profilowania można uruchomić narzędzie w trybie profilowania. Poniżej przedstawiono listę obowiązkowych i opcjonalnych parametrów narzędzia w trybie profilowania. Parametry ujęte w nawiasy kwadratowe „[]” są opcjonalne.

ASRDeploymentPlanner.exe -Operation StartProfiling /?

| Nazwa parametru | Opis |
|---|---|
| -Operation |      StartProfiling |
| -Server | W pełni kwalifikowana nazwa domeny lub adres IP serwera vCenter/hosta ESXi, którego maszyny wirtualne mają być profilowane.|
| -User | Nazwa użytkownika łączącego się z serwerem vCenter/hostem ESXi. Użytkownik musi mieć co najmniej uprawnienia tylko do odczytu.|
| -VMListFile |    Plik z listą maszyn wirtualnych, które mają być profilowane. Można użyć bezwzględnej lub względnej ścieżki pliku. Ten plik musi zawierać jedną nazwę lub jeden adres IP maszyny wirtualnej na wiersz. Nazwa maszyny wirtualnej określona w pliku musi być taka sama jak nazwa maszyny wirtualnej na serwerze vCenter lub hoście ESXi. <br> Na przykład plik „lista_mw.txt” zawiera informacje o następujących maszynach wirtualnych:<br>maszyna_wirtualna_A <br>10.150.29.110<br>maszyna_wirtualna_B |
| -NoOfDaysToProfile | Czas trwania profilowania w dniach. Zaleca się przeprowadzanie profilowania przez więcej niż 15 dni, aby umożliwić zaobserwowanie wzorca obciążenia w środowisku względem danego okresu i użycie go do udostępnienia dokładnego zalecenia |
| [-Directory] |    Ścieżka UNC lub ścieżka katalogu lokalnego, w której są przechowywane dane profilowania wygenerowane podczas profilowania. Jeśli nie zostanie ona podana, jako katalog domyślny zostanie użyty katalog o nazwie „ProfiledData” w bieżącej ścieżce. |
| [-Password ] | Hasło umożliwiające połączenie się z serwerem vCenter/hostem ESXi. Jeśli nie określisz go teraz, zostanie wyświetlony monit o podanie hasła podczas wykonywania polecenia.|
|  [-StorageAccountName]  | Nazwa konta usługi Azure Storage umożliwiającego znalezienie osiągalnej przepływności na potrzeby replikacji danych ze środowiska lokalnego na platformę Azure. Narzędzie przekazuje dane testowe na to konto magazynu w celu obliczenia przepływności.|
| [-StorageAccountKey] | Klucz konta usługi Azure Storage umożliwiający dostęp do konta magazynu. W witrynie Azure Portal przejdź do pozycji Konta magazynu > [Nazwa konta magazynu] > Ustawienia > Klucze dostępu > Klucz1 (lub Podstawowy klucz dostępu w przypadku klasycznego konta magazynu). |

Zaleca się profilowanie maszyn wirtualnych przez co najmniej 15–30 dni. Podczas profilowania jest uruchomiony proces ASRDeploymentPlanner.exe. Czas profilowania w narzędziu jest określany w dniach. Jeśli chcesz przeprowadzić profilowanie w ciągu kilku godzin lub minut w celu szybkiego przetestowania narzędzia w publicznej wersji zapoznawczej, musisz przekonwertować wartość czasu na równoważność liczby dni.  Aby na przykład profilowanie trwało 30 minut, musisz wprowadzić wartość 30 / (60 * 24) = 0,021 dnia.  Minimalny dozwolony czas profilowania to 30 minut.

Podczas profilowania można przekazać nazwę i klucz konta usługi Azure Storage umożliwiającego znalezienie osiągalnej przepływności usługi Azure Site Recovery podczas replikacji z serwera konfiguracji/serwera przetwarzania na platformę Azure. Jeśli nazwa i klucz konta usługi Azure Storage nie zostaną przekazane podczas profilowania, narzędzie nie obliczy osiągalnej przepływności.


Możesz uruchomić wiele wystąpień narzędzia dla różnych zestawów maszyn wirtualnych. Upewnij się, że nazwy maszyn wirtualnych nie powtarzają się w żadnym zestawie profilowania. Jeśli na przykład profilowano dziesięć maszyn wirtualnych (MW1–MW10) i po kilku dniach chcesz przeprowadzić profilowanie kolejnych pięciu maszyn wirtualnych (MW11–MW15), możesz uruchomić narzędzie z poziomu innej konsoli wiersza polecenia dla drugiego zestawu maszyn wirtualnych (MW11–MW15). Upewnij się, że drugi zestaw maszyn wirtualnych nie zawiera żadnych nazw maszyn wirtualnych z pierwszego wystąpienia profilowania, lub użyj innego katalogu wyjściowego dla drugiego przebiegu. Jeśli dwa wystąpienia narzędzia są używane na potrzeby profilowania tych samych maszyn wirtualnych i korzystają z tego samego katalogu wyjściowego, zostanie wygenerowany niepoprawny raport.

Konfiguracja maszyn wirtualnych jest przechwytywana raz na początku operacji profilowania i zapisywana w pliku o nazwie VMDetailList.xml. Te informacje są używane podczas generowania raportu. Nie są przechwytywane żadne zmiany konfiguracji maszyn wirtualnych (np. zwiększenie liczby rdzeni, dysków, kart interfejsów sieciowych itp.) między rozpoczęciem i zakończeniem profilowania. Jeśli konfiguracja profilowanej maszyny wirtualnej zmieniła się w trakcie profilowania (w publicznej wersji zapoznawczej), poniżej przedstawiono obejście pozwalające uzyskać najnowsze informacje o maszynach wirtualnych podczas generowania raportu.   

* Wykonanie kopii zapasowej pliku „VMdetailList.xml” i usunięcie go z jego bieżącej lokalizacji.
* Przekazanie argumentów -User i -Password podczas generowania raportu.

Polecenie profilowania generuje kilka plików w katalogu profilowania — nie usuwaj żadnego z nich, ponieważ może to mieć wpływ na generowanie raportu.

#####<a name="example-1-to-profile-virtual-machines-for-30-days-and-find-the-throughput-from-on-premises-to-azure"></a>Przykład 1: profilowanie maszyn wirtualnych przez 30 dni i znajdowanie przepływności między środowiskiem lokalnym i platformą Azure
ASRDeploymentPlanner.exe **-Operation** StartProfiling -Directory “E:\vCenter1_ProfiledData” **-Server** vCenter1.contoso.com **-VMListFile** “E:\vCenter1_ProfiledData\Lista_mw_profilowania_1.txt”  **-NoOfDaysToProfile**  30  **-User** Użytkownik_vCenter_1 **-StorageAccountName**  asrspfarm1 **-StorageAccountKey** Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==

#####<a name="example-2-to-profile-virtual-machines-for-15-days"></a>Przykład 2: profilowanie maszyn wirtualnych przez 15 dni
ASRDeploymentPlanner.exe **-Operation** StartProfiling **-Directory** “E:\vCenter1_ProfiledData” **-Server** vCenter1.contoso.com **-VMListFile** “E:\vCenter1_ProfiledData\Lista_mw_profilowania_1.txt”  **-NoOfDaysToProfile**  15  -User Użytkownik_vCenter_1

#####<a name="example-3-to-profile-virtual-machines-for-1-hour-for-a-quick-test-of-the-tool"></a>Przykład 3: profilowanie maszyn wirtualnych przez 1 godzinę w celu szybkiego przetestowania narzędzia
ASRDeploymentPlanner.exe **-Operation** StartProfiling **-Directory** “E:\vCenter1_ProfiledData” **-Server** vCenter1.contoso.com **-VMListFile** “E:\vCenter1_ProfiledData\Lista_mw_profilowania_1.txt”  **-NoOfDaysToProfile**  0,04  **-User** Użytkownik_vCenter_1


>[!NOTE]
>
> * Jeśli serwer, na którym jest uruchomione narzędzie, został ponownie uruchomiony lub uległ awarii albo jeśli zamknięto narzędzie za pomocą klawiszy Ctrl+C, profilowane dane zostaną zachowane. Z tego powodu może brakować profilowanych danych z ostatnich 15 minut. Po ponownym uruchomieniu serwera musisz ponownie uruchomić narzędzie w trybie profilowania.
>
> * Jeśli przekazano nazwę i klucz konta usługi Azure Storage, narzędzie zmierzy przepływność na ostatnim etapie profilowania. Jeśli narzędzie zostanie zamknięte przed bezpiecznym zakończeniem profilowania, przepływność nie zostanie obliczona. Możesz uruchomić operację GetThroughput w konsoli wiersza polecenia, aby znaleźć informacje o przepływności przed wygenerowaniem raportu. W przeciwnym razie wygenerowany raport nie będzie zawierał informacji o osiągniętej przepływności.
>

##<a name="generate-report"></a>Generowanie raportu
Narzędzie generuje plik XLSM (plik programu Excel z włączoną obsługą makr) z danymi wyjściowymi raportu, w którym zawarto wszystkie zalecenia dotyczące wdrożenia. Raport nosi nazwę DeploymentPlannerReport_<Unique Numeric Identifier>.xlsm i znajduje się w określonym katalogu.

Po zakończeniu profilowania możesz uruchomić narzędzie w trybie generowania raportu. Poniżej przedstawiono listę obowiązkowych i opcjonalnych parametrów narzędzia w trybie generowania raportu. Parametry ujęte w nawiasy kwadratowe „[]” są opcjonalne.

ASRDeploymentPlanner.exe -Operation GenerateReport /?

|Nazwa parametru | Opis |
|-|-|
| -Operation | GenerateReport |
| -Server |  W pełni kwalifikowana nazwa domeny lub adres IP serwera vCenter/vSphere (nazwa lub adres IP muszą być dokładnie takie same jak użyte podczas profilowania) z profilowanymi maszynami wirtualnymi, dla których ma zostać wygenerowany raport. Jeśli podczas profilowania użyto serwera vCenter, nie można użyć serwera vSphere podczas generowania raportu i odwrotnie.|
| -VMListFile | Plik z listą profilowanych maszyn wirtualnych, dla których ma zostać wygenerowany raport. Można użyć bezwzględnej lub względnej ścieżki pliku. Ten plik musi zawierać jedną nazwę lub jeden adres IP maszyny wirtualnej na wiersz. Nazwy maszyn wirtualnych określone w pliku muszą być takie same jak nazwy maszyn wirtualnych na serwerze vCenter lub hoście ESXi i zgodne z nazwami użytymi podczas profilowania.|
| [-Directory] | Ścieżka UNC lub ścieżka katalogu lokalnego, w której są przechowywane profilowane dane (pliki wygenerowane podczas profilowania). Te dane są wymagane do wygenerowania raportu. Jeśli nie określisz ścieżki, zostanie użyty katalog „ProfiledData”. |
| [-GoalToCompleteIR] |    Czas w godzinach, w którym musi zostać ukończona replikacja początkowa profilowanych maszyn wirtualnych. Wygenerowany raport zawiera liczbę maszyn wirtualnych, dla których można ukończyć replikację początkową w określonym czasie. Wartość domyślna to 72 godziny. |
| [-User] | Nazwa użytkownika łączącego się z serwerem vCenter/vSphere. Umożliwia ona pobieranie najnowszych informacji o konfiguracji maszyn wirtualnych, takich jak liczba dysków, liczba rdzeni czy liczba kart interfejsów sieciowych, do użycia w raporcie. Jeśli nie podasz nazwy, zostaną użyte informacje o konfiguracji zebrane na początku profilowania. |
| [-Password] | Hasło umożliwiające połączenie się z serwerem vCenter/hostem ESXi. Jeśli nie określisz go jako parametru, później zostanie wyświetlony monit o podanie hasła podczas wykonywania polecenia. |
| [-DesiredRPO] | Żądany cel punktu odzyskiwania w minutach. Wartość domyślna to 15 minut.|
| [-Bandwidth] | Przepustowość w Mb/s. Ten parametr służy do obliczania celu punktu odzyskiwania, który można osiągnąć dla określonej przepustowości. |
| [-StartDate]  | Data i godzina rozpoczęcia w 24-godzinnym formacie DD-MM-RRRR:GG:MM. Oprócz parametru „StartDate” należy także określić parametr „EndDate”. W przypadku określenia dat wygenerowany raport będzie dotyczył profilowanych danych zebranych od daty rozpoczęcia do daty zakończenia. |
| [-EndDate] | Data i godzina zakończenia w 24-godzinnym formacie DD-MM-RRRR:GG:MM. Oprócz parametru „EndDate” należy także określić parametr „StartDate”. W przypadku określenia dat wygenerowany raport będzie dotyczył profilowanych danych zebranych od daty rozpoczęcia do daty zakończenia. |
| [-GrowthFactor] |Współczynnik wzrostu w postaci wartości procentowej. Wartość domyślna to 30%.  |


##### <a name="example-1-to-generate-report-with-default-values-when-profiled-data-is-on-the-local-drive"></a>Przykład 1: generowanie raportu przy użyciu wartości domyślnych, gdy profilowane dane znajdują się na dysku lokalnym
ASRDeploymentPlanner.exe **-Operation** GenerateReport **-Server** vCenter1.contoso.com **-Directory** “E:\vCenter1_ProfiledData” **-VMListFile** “E:\vCenter1_ProfiledData\Lista_mw_profilowania_1.txt”


##### <a name="example-2-to-generate-report-when-profiled-data-is-on-a-remote-server-user-should-have-readwrite-access-on-the-remote-directory"></a>Przykład 2: generowanie raportu, gdy profilowane dane znajdują się na serwerze zdalnym Użytkownik musi mieć uprawnienia odczytu i zapisu w katalogu zdalnym.
ASRDeploymentPlanner.exe **-Operation** GenerateReport **-Server** vCenter1.contoso.com **-Directory** “\\PS1-W2K12R2\vCenter1_ProfiledData” **-VMListFile** “\\PS1-W2K12R2\vCenter1_ProfiledData\Lista_mw_profilowania_1.txt”

##### <a name="example-3-generate-report-with-specific-bandwidth-and-goal-to-complete-ir-within-specified-time"></a>Przykład 3: generowanie raportu przy użyciu określonej przepustowości i celu ukończenia replikacji początkowej w określonym czasie
ASRDeploymentPlanner.exe **-Operation** GenerateReport **-Server** vCenter1.contoso.com **-Directory** “E:\vCenter1_ProfiledData” **-VMListFile** “E:\vCenter1_ProfiledData\Lista_mw_profilowania_1.txt” **-Bandwidth** 100 **-GoalToCompleteIR** 24

##### <a name="example-4-generate-report-with-5-growth-factor-instead-of-the-default-30"></a>Przykład 4: generowanie raportu przy użyciu współczynnika wzrostu 5% zamiast domyślnego współczynnika 30%
ASRDeploymentPlanner.exe **-Operation** GenerateReport **-Server** vCenter1.contoso.com **-Directory** “E:\vCenter1_ProfiledData” **-VMListFile** “E:\vCenter1_ProfiledData\Lista_mw_profilowania_1.txt” **-GrowthFactor** 5

##### <a name="example-5-generate-report-with-a-subset-of-profiled-data-say-you-have-30-days-of-profiled-data-and-want-to-generate-the-report-for-only-20-days"></a>Przykład 5: generowanie raportu przy użyciu podzestawu profilowanych danych. Załóżmy, że masz profilowane dane z 30 dni i chcesz wygenerować raport tylko z 20 dni.
ASRDeploymentPlanner.exe **-Operation** GenerateReport **-Server** vCenter1.contoso.com **-Directory** “E:\vCenter1_ProfiledData” **-VMListFile** “E:\vCenter1_ProfiledData\Lista_mw_profilowania_1.txt” **-StartDate**  10-01-2017:12:30 **-EndDate** 19-01-2017:12:30

##### <a name="example-6-generate-report-for-5-minutes-rpo"></a>Przykład 6: generowanie raportu w przypadku 5-minutowego celu punktu odzyskiwania.
ASRDeploymentPlanner.exe **-Operation** GenerateReport **-Server** vCenter1.contoso.com **-Directory** “E:\vCenter1_ProfiledData” **-VMListFile** “E:\vCenter1_ProfiledData\Lista_mw_profilowania_1.txt” **-DesiredRPO** 5

### <a name="percentile-value-used-for-the-calculation"></a>Wartość percentyla używana do obliczenia
**Jaka domyślna wartość percentyla metryk wydajności zebranych podczas profilowania jest używana podczas generowania raportu?**

Domyślnie narzędzie używa wartości 95. percentyla operacji we/wy odczytu i zapisu na sekundę, operacji we/wy zapisu na sekundę i współczynnika zmian danych zebranych podczas profilowania wszystkich maszyn wirtualnych. Dzięki temu wymagania dotyczące docelowego konta usługi Azure Storage i przepustowości źródłowej nie są określane przy użyciu wartości szczytowej w 100. percentylu, która może występować na maszynach wirtualnych z powodu tymczasowych zdarzeń, takich jak zadanie wykonywania kopii zapasowej uruchamiane raz dziennie, okresowe indeksowanie bazy danych lub działanie generowania raportu analizy, ani innego podobnego krótkotrwałego zdarzenia, które występuje w danym momencie podczas profilowania. Korzystanie z wartości 95. percentyla oddaje rzeczywistą charakterystykę obciążenia i zapewnia najlepszą wydajność, gdy te obciążenia działają na platformie Microsoft Azure. Nie musisz często zmieniać tej wartości, ale w przypadku wybrania jeszcze niższej wartości, np. 90. percentyla, możesz zaktualizować ten plik konfiguracji „ASRDeploymentPlanner.exe.config” w folderze domyślnym i zapisać go w celu wygenerowania nowego raportu dotyczącego istniejących profilowanych danych.

        &lsaquo;add key="WriteIOPSPercentile" value="95" /&rsaquo;>      
        &lsaquo;add key="ReadWriteIOPSPercentile" value="95" /&rsaquo;>      
        &lsaquo;add key="DataChurnPercentile" value="95" /&rsaquo;

### <a name="growth-factor"></a>Współczynnik wzrostu
**Dlaczego należy brać pod uwagę współczynnik wzrostu podczas planowania wdrożenia?**

Uwzględnianie wzrostu w charakterystyce obciążenia jest niezwykle ważne przy zakładaniu potencjalnego wzrostu użycia wraz z upływem czasu. Jest to spowodowane tym, że w przypadku zmiany charakterystyki po włączeniu ochrony nie ma możliwości przełączenia do innego konta usługi Azure Storage w celu zapewnienia ochrony bez wyłączenia i ponownego włączenia ochrony. Na przykład obecnie maszyna wirtualna mieści się w ramach konta replikacji magazynu w warstwie Standardowa, ale trzy miesiące później będzie konieczne wyłączenie i ponowne włączenie ochrony dla konta magazynu w warstwie Premium z powodu wzrostu liczby użytkowników aplikacji działającej na maszynie wirtualnej w przypadku zwiększenia współczynnika zmian na maszynie wirtualnej, co będzie wymagać przejścia na usługę Premium Storage, aby usługa Azure Site Recovery mogła używać nowego, wyższego współczynnika zmian. Dlatego zdecydowanie zaleca się planowanie wzrostu podczas planowania wdrożenia. Wartość domyślna to 30%. Najlepiej znasz wzorzec użycia swoich aplikacji i projekcje wzrostu. Możesz odpowiednio zmienić tę wartość podczas generowania raportu. Możesz wygenerować wiele raportów dla różnych współczynników wzrostu i tych samych profilowanych danych, aby sprawdzić, jakie zalecenia dotyczące docelowego konta usługi Azure Storage i przepustowości źródłowej będą dla Ciebie optymalne.

Wygenerowany raport programu Microsoft Excel zawiera następujące arkusze:

* [Dane wejściowe](site-recovery-deployment-planner.md#input)
* [Zalecenia](site-recovery-deployment-planner.md#recommendations-with-desired-rpo-as-input)
* [Zalecenia — dane wejściowe przepustowości](site-recovery-deployment-planner.md#recommendations-with-available-bandwidth-as-input)
* [Rozmieszczenie maszyny wirtualnej względem magazynu](site-recovery-deployment-planner.md#vm-storage-placement)
* [Zgodne maszyny wirtualne](site-recovery-deployment-planner.md#compatible-vms)
* [Niezgodne maszyny wirtualne](site-recovery-deployment-planner.md#incompatible-vms)

![Planista wdrożenia](./media/site-recovery-deployment-planner/dp-report.png)


##<a name="get-throughput"></a>Uzyskiwanie informacji o przepływności
Aby oszacować przepływność osiągalną dla usługi Azure Site Recovery podczas replikacji między środowiskiem lokalnym i platformą Azure, uruchom narzędzie w trybie uzyskiwania informacji o przepływności (GetThroughput). Narzędzie oblicza przepływność na serwerze, na którym jest uruchomione narzędzie (idealnym rozwiązaniem jest serwer zgodny ze wskazówkami dotyczącymi rozmiaru serwera konfiguracji).  Jeśli wdrożono już lokalnie składniki infrastruktury usługi Azure Site Recovery, uruchom narzędzie na serwerze konfiguracji.

Otwórz konsolę wiersza polecenia i przejdź do folderu narzędzia do planowania wdrożenia usługi ASR.  Uruchom program ASRDeploymentPlanner.exe z poniższymi parametrami. Parametry ujęte w nawiasy kwadratowe „[]” są opcjonalne.

ASRDeploymentPlanner.exe -Operation GetThroughput /?

|Nazwa parametru | Opis |
|-|-|
| -operation | GetThroughput |
| [-Directory] | Ścieżka UNC lub ścieżka katalogu lokalnego, w której są przechowywane profilowane dane (pliki wygenerowane podczas profilowania). Te dane są wymagane do wygenerowania raportu. Jeśli nie określisz ścieżki, zostanie użyty katalog „ProfiledData”.  |
| -StorageAccountName | Nazwa konta usługi Azure Storage umożliwiającego znalezienie użytej przepustowości na potrzeby replikacji danych ze środowiska lokalnego na platformę Azure. Narzędzie przekazuje dane testowe na to konto magazynu w celu określenia użytej przepustowości. |
| -StorageAccountKey | Klucz konta usługi Azure Storage umożliwiający dostęp do konta magazynu. W witrynie Azure Portal przejdź do pozycji Konta magazynu > [Nazwa konta magazynu] > Ustawienia > Klucze dostępu > Klucz1 (lub Podstawowy klucz dostępu w przypadku klasycznego konta magazynu). |
| -VMListFile | Plik z listą maszyn wirtualnych, które mają być profilowane, na potrzeby obliczenia użytej przepustowości. Można użyć bezwzględnej lub względnej ścieżki pliku. Ten plik musi zawierać jedną nazwę lub jeden adres IP maszyny wirtualnej na wiersz. Nazwy maszyn wirtualnych określone w pliku muszą być takie same jak nazwy maszyn wirtualnych na serwerze vCenter lub hoście ESXi.<br>Na przykład plik „lista_mw.txt” zawiera informacje o następujących maszynach wirtualnych:<br>maszyna_wirtualna_A <br>10.150.29.110<br>maszyna_wirtualna_B|

Narzędzie tworzy kilka plików „asrvhdfile<#>.vhd” (gdzie znak # to numer) o rozmiarze 64 MB w określonym katalogu.  Następnie przekazuje te pliki na konto usługi Azure Storage w celu znalezienia informacji o przepływności. Po zmierzeniu przepływności narzędzie usuwa wszystkie te pliki z konta usługi Azure Storage i serwera lokalnego. Jeśli narzędzie zostanie z jakiegoś powodu zamknięte w trakcie obliczania przepływności, pliki nie zostaną usunięte z usługi Azure Storage lub serwera lokalnego i trzeba będzie usunąć je ręcznie.

Przepływność jest mierzona w danym momencie i jest maksymalną przepływnością osiągalną dla usługi Azure Site Recovery podczas replikacji, jeśli wszystkie pozostałe czynniki pozostają bez zmian. Jeśli na przykład aplikacja zacznie używać większej przepustowości w tej samej sieci, rzeczywista przepływność będzie się zmieniać podczas replikacji. Jeśli używasz polecenia GetThroughput na serwerze konfiguracji, narzędzie nie uwzględnia chronionych maszyn wirtualnych i replikacji w toku. Wynik pomiaru przepływności różni się w przypadku uruchomienia operacji GetThroughput, gdy chronione maszyny wirtualne mają wysoki i niski współczynnik zmian danych.  Zaleca się uruchomienie narzędzia w różnych momentach podczas profilowania, aby sprawdzić osiągalną przepływność w różnym czasie. Raport narzędzia zawiera ostatnio zmierzoną przepływność.


##### <a name="example"></a>Przykład
ASRDeploymentPlanner.exe **-Operation** GetThroughput **-Directory** E:\vCenter1_ProfiledData **-VMListFile** E:\ProfileVMList1_ProfiledData\Lista_mw_profilowania_1.txt  **-StorageAccountName ** asrspfarm1 **-StorageAccountKey** by8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==

>[!NOTE]
>
> * Uruchom narzędzie na serwerze z magazynem i procesorem CPU o takiej samej charakterystyce jako serwer konfiguracji
>
> * W przypadku replikacji aprowizuj przepustowość zalecaną do osiągnięcia celu punktu odzyskiwania przez 100% czasu. Jeśli nie widzisz wzrostu osiągniętej przepływności zgłoszonej przez narzędzie nawet po aprowizowaniu odpowiedniej przepustowości, sprawdź następujące kwestie:
>
> a. Sprawdź, czy w sieci jest używana technologia QoS (Quality of Service) ograniczająca przepływność usługi Azure Site Recovery
>
> b. Sprawdź, czy magazyn usługi Azure Site Recovery znajduje się w najbliższym obsługiwanym fizycznym regionie świadczenia usługi Microsoft Azure, aby zminimalizować opóźnienie sieci
>
> c. Sprawdź charakterystykę magazynu lokalnego pod kątem konieczności zmodernizowania sprzętu (np. zamiany dysku twardego na dysk SSD itp.)
>
> d. Zmień ustawienia usługi Azure Site Recovery na serwerze przetwarzania, aby [zwiększyć przepustowość sieci na potrzeby replikacji](./site-recovery-plan-capacity-vmware.md#control-network-bandwidth).
>

##<a name="recommendations-with-desired-rpo-as-input"></a>Zalecenia dotyczące danych wejściowych w postaci żądanego celu punktu odzyskiwania

###<a name="profiled-data"></a>Profilowane dane

![Planista wdrożenia](./media/site-recovery-deployment-planner/profiled-data-period.png)

**Okres profilowanych danych** to czas trwania profilowania. Domyślnie narzędzie pobiera wszystkie profilowane dane na potrzeby obliczenia, o ile nie zostanie wygenerowany raport za okres wyznaczony przy użyciu opcji StartDate i EndDate podczas generowania raportu.

**Nazwa serwera** to nazwa lub adres IP hosta VMware vCenter lub ESXi, dla którego jest generowany raport dotyczący maszyn wirtualnych.

**Żądany cel punktu odzyskiwania** to cel punktu odzyskiwania dla danego wdrożenia. Domyślnie wymagana przepustowość sieci jest obliczana dla wartości celu punktu odzyskiwania równych 15, 30 i 60 minut. Odpowiednie wartości są aktualizowane w arkuszu zgodnie z wybraną wartością. W przypadku użycia parametru DesiredRPOinMin podczas generowania raportu ta wartość jest wyświetlana na tej liście rozwijanej żądanego celu punktu odzyskiwania.

###<a name="profiling-overview"></a>Przegląd profilowania

![Planista wdrożenia](./media/site-recovery-deployment-planner/profiling-overview.png)

**Łączna liczba profilowanych maszyn wirtualnych** to całkowita liczba maszyn wirtualnych, których profilowane dane są dostępne. Jeśli parametr VMListFile zawiera nazwy nieprofilowanych maszyn wirtualnych, te maszyny wirtualne nie zostaną uwzględnione podczas generowania raportów i zostaną wykluczone z łącznej liczby profilowanych maszyn wirtualnych.

**Zgodne maszyny wirtualne** to liczba maszyn wirtualnych, które mogą być chronione na platformie Azure przy użyciu usługi Azure Site Recovery. Jest to łączna liczba zgodnych maszyn wirtualnych, dla których są obliczane wymagana przepustowość sieci, liczba kont usługi Azure Storage, liczba rdzeni platformy Microsoft Azure oraz liczba serwerów konfiguracji i dodatkowych serwerów przetwarzania. Szczegóły każdej zgodnej maszyny wirtualnej są dostępne w arkuszu zgodnych maszyn wirtualnych w raporcie.

**Niezgodne maszyny wirtualne** to liczba profilowanych maszyn wirtualnych, które nie są zgodne na potrzeby ochrony za pomocą usługi Azure Site Recovery. Przyczyny niezgodności wymieniono w poniższej sekcji dotyczącej niezgodnych maszyn wirtualnych. Jeśli parametr VMListFile zawiera nazwy nieprofilowanych maszyn wirtualnych, te maszyny wirtualne zostaną wykluczone z liczby niezgodnych maszyn wirtualnych. Dla tych maszyn wirtualnych jest wyświetlany tekst „Nie znaleziono danych” na końcu arkusza niezgodnych maszyn wirtualnych.

**Żądany cel punktu odzyskiwania** to żądany cel punktu odzyskiwania w minutach. Raport jest generowany dla trzech wartości celu punktu odzyskiwania: 15, 30 i 60 minut. Wartość domyślna to 15 minut. Zalecenie dotyczące przepustowości w raporcie zmienia się zgodnie z pozycją wybraną z listy rozwijanej żądanego celu punktu odzyskiwania w prawym górnym rogu arkusza. Jeśli raport został wygenerowany przy użyciu parametru „-DesiredRPO” z wartością niestandardową, ta wartość będzie wyświetlana jako domyślna na liście rozwijanej żądanego celu punktu odzyskiwania.

###<a name="required-network-bandwidth-mbps"></a>Wymagana przepustowość sieci (Mb/s)

![Planista wdrożenia](./media/site-recovery-deployment-planner/required-network-bandwidth.png)

**Aby osiągnąć cel punktu odzyskiwania przez 100% czasu:** jest to zalecana przepustowość (w Mb/s) do przydzielenia, która umożliwia spełnienie żądanego celu punktu odzyskiwania przez 100% czasu. Taka przepustowość musi zostać przeznaczona na stałą replikację przyrostową wszystkich zgodnych maszyn wirtualnych, aby uniknąć naruszeń celu punktu odzyskiwania.

**Aby osiągnąć cel punktu odzyskiwania przez 90% czasu**: jeśli nie możesz aprowizować przepustowości wymaganej do osiągnięcia żądanego celu punktu odzyskiwania przez 100% czasu, możesz aprowizować mniejszą przepustowość umożliwiającą osiągnięcie żądanego celu punktu odzyskiwania przez 90% czasu. Aby umożliwić poznanie skutków aprowizacji mniejszej przepustowości, w raporcie przedstawiono analizę warunkową liczby i czasu trwania naruszeń celu punktu odzyskiwania.

**Osiągnięta przepływność:** jest to przepływność między serwerem, na którym uruchomiono polecenie GetThroughput, i regionem świadczenia usługi Microsoft Azure, w którym znajduje się konto usługi Azure Storage. Ta wartość wskazuje przybliżoną przepływność, którą można osiągnąć podczas ochrony zgodnych maszyn wirtualnych za pomocą usługi Azure Site Recovery, jeśli charakterystyka magazynu i sieci serwera konfiguracji/serwera przetwarzania pozostają takie same jak w przypadku serwera, na którym uruchomiono narzędzie.    

W przypadku replikacji musisz aprowizować przepustowość zalecaną do osiągnięcia celu punktu odzyskiwania przez 100% czasu. Jeśli nie widzisz wzrostu osiągniętej przepływności zgłoszonej przez narzędzie nawet po aprowizowaniu odpowiedniej przepustowości, sprawdź następujące kwestie:

a.    Sprawdź, czy w sieci jest używana technologia QoS (Quality of Service) ograniczająca przepływność usługi Azure Site Recovery

b.    Sprawdź, czy magazyn usługi Azure Site Recovery znajduje się w najbliższym obsługiwanym fizycznym regionie świadczenia usługi Microsoft Azure, aby zminimalizować opóźnienie sieci

c.    Sprawdź charakterystykę magazynu lokalnego pod kątem konieczności zmodernizowania sprzętu (np. zamiany dysku twardego na dysk SSD itp.)

d. Zmień ustawienia usługi Azure Site Recovery na serwerze przetwarzania, aby [zwiększyć przepustowość sieci na potrzeby replikacji](./site-recovery-plan-capacity-vmware.md#control-network-bandwidth).

Jeśli narzędzie jest uruchamiane na serwerze konfiguracji/serwerze przetwarzania, który już ma chronione maszyny wirtualne, uruchom narzędzie kilka razy, ponieważ osiągnięta przepływność zmienia się zależnie od współczynnika zmian danych przetwarzanych w konkretnym momencie.

W przypadku wszystkich wdrożeń usługi Azure Site Recovery w przedsiębiorstwach zaleca się użycie usługi [ExpressRoute](https://aka.ms/expressroute).

###<a name="required-azure-storage-accounts"></a>Wymagane konta usługi Azure Storage
Ten wykres przedstawia łączną liczbę kont usługi Azure Storage (w warstwach Standardowa i Premium) wymaganych do ochrony wszystkich zgodnych maszyn wirtualnych.  Kliknij link [Zalecany plan rozmieszczania maszyn wirtualnych](site-recovery-deployment-planner.md#vm-storage-placement), aby sprawdzić, które konto magazynu ma być używane dla każdej maszyny wirtualnej.  

![Planista wdrożenia](./media/site-recovery-deployment-planner/required-azure-storage-accounts.png)

###<a name="required-number-of-azure-cores"></a>Wymagana liczba rdzeni platformy Azure
Jest to łączna liczba rdzeni do aprowizowania przed rozpoczęciem pracy w trybie failover lub testem pracy w trybie failover dla wszystkich zgodnych maszyn wirtualnych. Jeśli w ramach subskrypcji nie jest dostępna wystarczająca liczba rdzeni, usługa Azure Site Recovery nie może utworzyć maszyn wirtualnych w czasie pracy w trybie failover lub testu pracy w trybie failover.

![Planista wdrożenia](./media/site-recovery-deployment-planner/required-number-of-azure-cores.png)

###<a name="required-on-premises-infrastructure"></a>Wymagana infrastruktura lokalna
Jest to łączna liczba serwerów konfiguracji i dodatkowych serwerów przetwarzania do skonfigurowania w celu zapewnienia ochrony wszystkich zgodnych maszyn wirtualnych. Narzędzie zaleca dodatkowe serwery na podstawie obsługiwanych [limitów](https://aka.ms/asr-v2a-on-prem-components) największej konfiguracji — dziennego współczynnika zmian lub maksymalnej liczby chronionych maszyn wirtualnych (przy założeniu średnio trzech dysków na maszynę wirtualną) zależnie od tego, który z tych limitów zostanie osiągnięty jako pierwszy na serwerze konfiguracji lub dodatkowym serwerze przetwarzania. Szczegółowe informacje o łącznym dziennym współczynniku zmian oraz łącznej liczbie chronionych dysków znajdują się w arkuszu [Dane wejściowe](site-recovery-deployment-planner.md#input).

![Planista wdrożenia](./media/site-recovery-deployment-planner/required-on-premises-infrastructure.png)

###<a name="what-if-analysis"></a>Analiza warunkowa
Ta analiza przedstawia liczbę naruszeń, które mogą wystąpić podczas profilowania w przypadku aprowizowania mniejszej przepustowości umożliwiającej osiągnięcie żądanego celu punktu odzyskiwania tylko przez 90% czasu. W danym dniu może wystąpić wiele naruszeń celu punktu odzyskiwania. Wykres przedstawia szczytową wartość celu punktu odzyskiwania w określonym dniu.
Na podstawie tej analizy można zdecydować, czy liczba naruszeń celu punktu odzyskiwania we wszystkich dniach i szczytowa wartość celu punktu odzyskiwania na dzień jest dopuszczalna przy określonej mniejszej przepustowości. Jeśli przepustowość jest dopuszczalna, można przydzielić mniejszą przepustowość na potrzeby replikacji. W przeciwnym razie przydziel większą przepustowość zgodnie z sugestią, aby osiągnąć żądany cel punktu odzyskiwania przez 100% czasu.

![Planista wdrożenia](./media/site-recovery-deployment-planner/what-if-analysis.png)

###<a name="recommended-vm-batch-size-for-initial-replication"></a>Zalecany rozmiar partii maszyn wirtualnych na potrzeby replikacji początkowej
Ta sekcja zawiera zalecenia dotyczące liczby maszyn wirtualnych, które mogą być chronione równolegle w celu ukończenia replikacji początkowej w ciągu 72 godzin (wartość z możliwością konfigurowania — aby ją zmienić, użyj parametru GoalToCompleteIR podczas generowania raportu) przy sugerowanej przepustowości umożliwiającej osiągnięcie żądanego celu punktu odzyskiwania przez 100% czasu po aprowizowaniu.  Wykres przedstawia zakres wartości przepustowości i obliczony rozmiar partii maszyn wirtualnych umożliwiające ukończenie replikacji początkowej w ciągu 72 godzin na podstawie wykrytego średniego rozmiaru wszystkich zgodnych maszyn wirtualnych.  

W publicznej wersji zapoznawczej raport nie zawiera informacji o tym, które maszyny wirtualne mają być uwzględniane w partii. Możesz użyć rozmiaru dysku widocznego w arkuszu zgodnych maszyn wirtualnych, aby znaleźć rozmiar każdej maszyny wirtualnej, i wybrać maszyny wirtualne dla partii albo na podstawie charakterystyki znanych obciążeń.  Czas ukończenia replikacji początkowej zmienia się proporcjonalnie do rzeczywistego rozmiaru dysku maszyny wirtualnej, używanego miejsca na dysku i dostępnej przepływności sieci.

![Planista wdrożenia](./media/site-recovery-deployment-planner/recommended-vm-batch-size.png)

###<a name="growth-factor-and-percentile-values-used"></a>Używane wartości współczynnika wzrostu i percentyla
W tej sekcji w dolnej części arkusza pokazano wartość percentyla używaną przez wszystkie liczniki wydajności profilowanych maszyn wirtualnych (domyślnie jest używany 95. percentyl) i współczynnik wzrostu w procentach używany we wszystkich obliczeniach (wartość domyślna to 30%).

![Planista wdrożenia](./media/site-recovery-deployment-planner/max-iops-and-data-churn-setting.png)

##<a name="recommendations-with-available-bandwidth-as-input"></a>Zalecenia dotyczące danych wejściowych w postaci dostępnej przepustowości

![Planista wdrożenia](./media/site-recovery-deployment-planner/profiling-overview-bandwidth-input.png)

Może wystąpić sytuacja, w której nie można aprowizować więcej niż określoną przepustowość na potrzeby replikacji usługi Azure Site Recovery. Narzędzie umożliwia wprowadzenie dostępnej przepustowości (za pomocą parametru -Bandwidth podczas generowania raportu) i uzyskać osiągalny cel punktu odzyskiwania w minutach. Osiągalna wartość celu punktu odzyskiwania pozwala zdecydować, czy trzeba aprowizować dodatkową przepustowość, czy rozwiązanie odzyskiwania po awarii może używać tego celu punktu odzyskiwania.

![Planista wdrożenia](./media/site-recovery-deployment-planner/achievable-rpos.png)

##<a name="input"></a>Dane wejściowe
Strona danych wejściowych zawiera przegląd profilowanego środowiska VMware.

![Planista wdrożenia](./media/site-recovery-deployment-planner/Input.png)

**Data rozpoczęcia i data zakończenia** to daty rozpoczęcia i zakończenia profilowania danych na potrzeby generowania raportu. Domyślnie data rozpoczęcia to data rozpoczęcia profilowania, a data zakończenia to data zatrzymania profilowania.  Mogą to być wartości „StartDate” i „EndDate”, jeśli raport jest generowany przy użyciu tych parametrów. Data rozpoczęcia i data zakończenia: są to daty rozpoczęcia i zakończenia profilowania danych na potrzeby generowania raportu. Domyślnie data rozpoczęcia to data rozpoczęcia profilowania, a data zakończenia to data zatrzymania profilowania.  Mogą to być wartości „StartDate” i „EndDate”, jeśli raport jest generowany przy użyciu tych parametrów.

**Łączna liczba dni profilowania** to całkowita liczba dni profilowania od daty rozpoczęcia do daty zakończenia, dla których jest generowany raport. Łączna liczba dni profilowania to całkowita liczba dni profilowania od daty rozpoczęcia do daty zakończenia, dla których jest generowany raport.

**Liczba zgodnych maszyn wirtualnych** to łączna liczba zgodnych maszyn wirtualnych, dla których są obliczane wymagana przepustowość sieci oraz wymagana liczba kont usługi Azure Storage, rdzeni platformy Microsoft Azure oraz serwerów konfiguracji i dodatkowych serwerów przetwarzania.
Łączna liczba dysków na wszystkich zgodnych maszynach wirtualnych to całkowita liczba dysków wszystkich zgodnych maszyn wirtualnych. Jest ona używana jako dane wejściowe do określania liczby serwerów konfiguracji i dodatkowych serwerów przetwarzania do użycia we wdrożeniu.

**Średnia liczba dysków na zgodną maszynę wirtualną** to średnia liczba dysków obliczana dla wszystkich zgodnych maszyn wirtualnych.

**Średni rozmiar dysku (GB)** to średni rozmiar dysku obliczany dla wszystkich zgodnych maszyn wirtualnych.

**Żądany cel punktu odzyskiwania (w minutach)** to domyślny cel punktu odzyskiwania lub przekazana wartość parametru „DesiredRPO” podczas generowania raportu, które umożliwiają oszacowanie wymaganej przepustowości.

**Żądana przepustowość (Mb/s)** to przekazana wartość parametru „Bandwidth” podczas generowania raportu umożliwiająca oszacowanie osiągalnego celu punktu odzyskiwania.

**Zaobserwowany dzienny typowy współczynnik zmian danych (GB)** to średni współczynnik zmian danych zaobserwowany we wszystkie dni profilowania. Jest ona używana jako dane wejściowe do określania liczby serwerów konfiguracji i dodatkowych serwerów przetwarzania do użycia we wdrożeniu.


##<a name="vm-storage-placement"></a>Rozmieszczenie maszyny wirtualnej względem magazynu

![Planista wdrożenia](./media/site-recovery-deployment-planner/vm-storage-placement.png)

**Typ magazynu dysków** to konto usługi Azure Storage w warstwie Standardowa lub Premium używane do replikacji wszystkich odpowiednich maszyn wirtualnych wymienionych w kolumnie „Maszyny wirtualne do rozmieszczenia”.

**Sugerowany prefiks** to sugerowany&3;-znakowy prefiks, którego można użyć w nazwie konta usługi Azure Storage. Możesz użyć własnego prefiksu, ale sugestia w narzędziu jest zgodna z [konwencją nazewnictwa partycji kont usługi Azure Storage](https://aka.ms/storage-performance-checklist).

**Sugerowana nazwa konta** wskazuje, jak powinna wyglądać nazwa konta usługi Azure Storage po dołączeniu sugerowanego prefiksu. Zamień nazwę w nawiasach „<>” na niestandardową nazwę.

**Konto magazynu dzienników:** wszystkie dzienniki replikacji są przechowywane na koncie usługi Azure Storage w warstwie Standardowa. W przypadku replikacji maszyn wirtualnych do konta usługi Azure Storage w warstwie Premium należy aprowizować dodatkowe konto usługi Azure Storage w warstwie Standardowa na potrzeby przechowywania dzienników. Jedno konto magazynu dzienników w warstwie Standardowa może być używane przez wiele kont magazynu replikacji w warstwie Premium. Maszyny wirtualne replikowane do kont magazynu w warstwie Standardowa używają tego samego konta magazynu dla dzienników.

**Sugerowana nazwa konta dzienników** wskazuje, jak powinna wyglądać nazwa konta dzienników usługi Azure Storage po dołączeniu sugerowanego prefiksu. Zamień nazwę w nawiasach „<>” na niestandardową nazwę.

**Podsumowanie rozmieszczania** zawiera podsumowanie łącznego obciążenia maszyn wirtualnych na koncie usługi Azure Storage podczas replikacji i pracy w trybie failover lub testu pracy w trybie failover. Obejmuje ono łączną liczbę maszyn wirtualnych zamapowanych na konto usługi Azure Storage, łączną liczbę operacji we/wy odczytu i zapisu na sekundę dla wszystkich maszyn wirtualnych umieszczanych na tym koncie usługi Azure Storage, łączną liczbę operacji we/wy zapisu na sekundę (replikacja), łączny aprowizowany rozmiar wszystkich dysków oraz łączną liczbę dysków.

**Maszyny wirtualne do rozmieszczenia** to lista wszystkich maszyn wirtualnych, które powinny zostać umieszczone na danym koncie usługi Azure Storage w celu uzyskania optymalnej wydajności i optymalnego użycia.

##<a name="compatible-vms"></a>Zgodne maszyny wirtualne
![Planista wdrożenia](./media/site-recovery-deployment-planner/compatible-vms.png)

**Nazwa maszyny wirtualnej** to nazwa lub adres IP maszyny wirtualnej użyte w parametrze VMListFile podczas generowania raportu. Ta kolumna obejmuje też dyski (VMDK) dołączone do maszyn wirtualnych.

**Zgodność maszyny wirtualnej** ma dwie wartości: Tak/Tak.* Wartość Tak* dotyczy sytuacji, gdy maszyna wirtualna może korzystać z [usługi Azure Storage w warstwie Premium](https://aka.ms/premium-storage-workload) z profilowanym dyskiem o wysokim współczynniku zmian lub dużej liczbie operacji we/wy na sekundę dysku z kategorii P20 lub P30, ale rozmiar dysku powoduje, że jest on mapowany w dół do kategorii P10 lub P20. Usługa Azure Storage określa, jakiego typu dysk usługi Premium Storage ma zostać zamapowany na dysk na podstawie rozmiaru, tj. mniej niż 128 GB to kategoria P10, od 128 do 512 GB to kategoria P20, a od 512 GB do 1023 GB to kategoria P30. Jeśli więc charakterystyka obciążenia dysku kwalifikuje dysk do kategorii P20 lub P30, ale ze względu na rozmiar dysk jest mapowany w dół na niższą kategorię dysku usługi Premium Storage, narzędzie ustawia dla tej maszyny wirtualnej wartość Tak i zaleca zmianę rozmiaru dysku źródłowego na zalecany typ dysku usługi Premium Storage lub zmianę docelowego typu dysku po zakończeniu pracy w trybie failover.
Dostępne typy magazynu do Standardowa i Premium.

**Sugerowany prefiks** to&3;-znakowy prefiks konta usługi Azure Storage

**Konto magazynu** to nazwa używająca sugerowanego prefiksu

**Operacje we/wy odczytu i zapisu na sekundę (ze współczynnikiem wzrostu)** to szczytowa liczba operacji we/wy na sekundę dla obciążenia na dysku (domyślnie jest używany 95. percentyl) wraz z przyszłym współczynnikiem wzrostu (wartość domyślna to 30%). Łączna liczba operacji we/wy odczytu i zapisu na sekundę maszyny wirtualnej nie zawsze jest sumą operacji z poszczególnych dysków maszyny wirtualnej, ponieważ szczytowa liczba operacji we/wy odczytu i zapisu na sekundę maszyny wirtualnej to wartość szczytowa sumy operacji we/wy odczytu i zapisu na sekundę poszczególnych dysków z każdej minuty okresu profilowania.

**Współczynnik zmian danych w Mb/s (ze współczynnikiem wzrostu)** to szczytowy współczynnik zmian danych na dysku (domyślnie jest używany 95. percentyl) wraz z przyszłym współczynnikiem wzrostu (wartość domyślna to 30%). Łączny współczynnik zmian danych maszyny wirtualnej nie zawsze jest sumą współczynników zmian danych z poszczególnych dysków maszyny wirtualnej, ponieważ szczytowy współczynnik zmian danych maszyny wirtualnej to wartość szczytowa sumy współczynników zmian poszczególnych dysków z każdej minuty okresu profilowania.

**Rozmiar maszyny wirtualnej platformy Azure** to idealnie zamapowany rozmiar maszyny wirtualnej obliczeń platformy Azure dla tej lokalnej maszyny wirtualnej. To mapowanie jest wykonywane na podstawie pamięci lokalnej maszyny wirtualnej, liczby dysków/rdzeni/kart interfejsów sieciowych i liczby operacji we/wy odczytu i zapisu na sekundę — zalecenie to zawsze najmniejszy rozmiar maszyny wirtualnej platformy Azure zgodny z całą charakterystyką lokalnej maszyny wirtualnej.

**Liczba dysków** to łączna liczba dysków (VMDK) maszyny wirtualnej

**Rozmiar dysku (GB)** to łączny aprowizowany rozmiar wszystkich dysków maszyny wirtualnej. W narzędziu jest też wyświetlany rozmiar poszczególnych dysków maszyny wirtualnej.

**Rdzenie** to liczba rdzeni procesora CPU maszyny wirtualnej.

**Pamięć (MB)** to pamięć RAM maszyny wirtualnej.

**Karty interfejsów sieciowych** to liczba kart interfejsów sieciowych maszyny wirtualnej.

##<a name="incompatible-vms"></a>Niezgodne maszyny wirtualne

![Planista wdrożenia](./media/site-recovery-deployment-planner/incompatible-vms.png)

**Nazwa maszyny wirtualnej** to nazwa lub adres IP maszyny wirtualnej użyte w parametrze VMListFile podczas generowania raportu. Ta kolumna obejmuje też dyski (VMDK) dołączone do maszyn wirtualnych.

**Zgodność maszyny wirtualnej** wskazuje, dlaczego podana maszyna wirtualna nie jest zgodna na potrzeby użycia z usługą Azure Site Recovery. Podane przyczyny dotyczą poszczególnych niezgodnych dysków maszyny wirtualnej. Poniżej przedstawiono możliwe przyczyny na podstawie opublikowanych [limitów](https://aka.ms/azure-storage-scalbility-performance) usługi Azure Storage.

* Rozmiar dysku większy niż 1023 GB — usługa Azure Storage obecnie nie obsługuje dysków większych niż 1 TB
* Łączny rozmiar maszyny wirtualnej (replikacja i test pracy w trybie failover) przekracza obsługiwany limit rozmiaru konta usługi Azure Storage (35 TB). Zwykle dzieje się tak, gdy maszyna wirtualna ma jeden dysk, którego charakterystyka wydajności przekracza maksymalne obsługiwane limity platformy Microsoft Azure lub usługi Azure Site Recovery dla magazynu w warstwie Standardowa, który wypycha maszynę wirtualną do strefy usługi Premium Storage. Jednak maksymalny obsługiwany rozmiar konta usługi Azure Storage w warstwie Premium jest równy 35 TB i jedna chroniona maszyna wirtualna nie może być chroniona na wielu kontach magazynu. Gdy test pracy w trybie failover jest przeprowadzany na chronionej maszynie wirtualnej, jest wykonywany na tym samym koncie magazynu, na którym jest wykonywana replikacja, dlatego konieczne jest aprowizowanie dwa razy większego rozmiaru dysku, aby umożliwić postęp replikacji i jednoczesne powodzenie testu pracy w trybie failover.
* Źródłowe operacje we/wy na sekundę przekraczają obsługiwany limit operacji we/wy na sekundę usługi Azure Storage równy 5000 operacji na dysk
* Źródłowe operacje we/wy na sekundę przekraczają obsługiwany limit operacji we/wy na sekundę usługi Azure Storage równy 80 000 operacji na maszynę wirtualną
* Średni współczynnik zmian danych przekracza obsługiwany limit współczynnika zmian danych usługi Azure Site Recovery równy 10 MB/s dla średniego rozmiaru operacji we/wy na dysku
* Łączny współczynnik zmian danych dla wszystkich dysków na maszynie wirtualnej przekracza maksymalny obsługiwany limit współczynnika zmian danych usługi Azure Site Recovery równy 54 MB/s na maszynę wirtualną
* Średnia liczba operacji we/wy zapisu na sekundę przekracza obsługiwany limit operacji we/wy na sekundę usługi Azure Site Recovery równy 840 operacji na dysk
* Obliczony magazyn migawek przekracza obsługiwany limit magazynu migawek równy 10 TB

**Operacje we/wy odczytu i zapisu na sekundę (ze współczynnikiem wzrostu)** to szczytowa liczba operacji we/wy na sekundę dla obciążenia na dysku (domyślnie jest używany 95. percentyl) wraz z przyszłym współczynnikiem wzrostu (wartość domyślna to 30%). Łączna liczba operacji we/wy odczytu i zapisu na sekundę maszyny wirtualnej nie zawsze jest sumą operacji z poszczególnych dysków maszyny wirtualnej, ponieważ szczytowa liczba operacji we/wy odczytu i zapisu na sekundę maszyny wirtualnej to wartość szczytowa sumy operacji we/wy odczytu i zapisu na sekundę poszczególnych dysków z każdej minuty okresu profilowania.

**Współczynnik zmian danych w Mb/s (ze współczynnikiem wzrostu)** to szczytowy współczynnik zmian danych na dysku (domyślnie jest używany 95. percentyl) wraz z przyszłym współczynnikiem wzrostu (wartość domyślna to 30%). Łączny współczynnik zmian danych maszyny wirtualnej nie zawsze jest sumą współczynników zmian danych z poszczególnych dysków maszyny wirtualnej, ponieważ szczytowy współczynnik zmian danych maszyny wirtualnej to wartość szczytowa sumy współczynników zmian poszczególnych dysków z każdej minuty okresu profilowania.

**Liczba dysków** to łączna liczba dysków (VMDK) maszyny wirtualnej

**Rozmiar dysku (GB)** to łączny aprowizowany rozmiar wszystkich dysków maszyny wirtualnej. W narzędziu jest też wyświetlany rozmiar poszczególnych dysków maszyny wirtualnej.

**Rdzenie** to liczba rdzeni procesora CPU maszyny wirtualnej.

**Pamięć (MB)** to pamięć RAM maszyny wirtualnej.

**Karty interfejsów sieciowych** to liczba kart interfejsów sieciowych maszyny wirtualnej.


##<a name="azure-site-recovery-limits"></a>Limity usługi Azure Site Recovery

**Cel magazynu replikacji** | **Średni rozmiar źródłowych operacji we/wy na dysku** |**Średni źródłowy współczynnik zmian danych na dysku** | **Łączny współczynnik zmian danych na dysku dziennie**
---|---|---|---
Standard Storage | 8 KB    | 2 MB/s | 168 GB na dysk
Dysk Premium P10 | 8 KB    | 2 MB/s | 168 GB na dysk
Dysk Premium P10 | 16 KB | 4 MB/s |    336 GB na dysk
Dysk Premium P10 | 32 KB lub więcej | 8 MB/s | 672 GB na dysk
Dysk Premium P20/P30 | 8 KB    | 5 MB/s | 421 GB na dysk
Dysk Premium P20/P30 | 16 KB lub więcej |10 MB/s    | 842 GB na dysk


Są to średnie wartości przy założeniu 30-procentowego nakładania się operacji we/wy. Usługa Azure Site Recovery może obsługiwać większą przepływność na podstawie zakresu nakładania się na siebie, większego rozmiaru operacji zapisu i rzeczywistego zachowania związanego z obciążeniem operacji we/wy. Powyższe wartości uzyskano przy założeniu typowej listy prac dla ok. 5 minut, tj. przekazane dane zostaną przetworzone i punkt odzyskiwania zostanie utworzony w ciągu 5 minut.

Powyższe opublikowane limity są oparte na naszych testach, ale nie obejmują wszystkich możliwych kombinacji operacji we/wy aplikacji. Rzeczywiste wyniki będą się różnić w zależności od kombinacji operacji we/wy aplikacji. Aby uzyskać najlepsze wyniki nawet po zakończeniu planowania wdrożenia, zawsze zaleca się dokładne przetestowanie aplikacji przy użyciu testu pracy w trybie failover w celu uzyskania prawdziwych informacji o wydajności.

##<a name="release-notes"></a>Informacje o wersji
Planista wdrożenia usługi Azure Site Recovery w publicznej wersji zapoznawczej 1.0 ma następujące znane problemy, które zostaną rozwiązane w przyszłych aktualizacjach.

* Narzędzie działa tylko w przypadku replikacji z oprogramowania VMware na platformę Azure. Nie działa w przypadku wdrożeń funkcji Hyper-V na platformie Azure. W przypadku wdrożeń funkcji Hyper-V na platformie Azure użyj [narzędzia planisty wydajności funkcji Hyper-V](./site-recovery-capacity-planning-for-hyper-v-replication.md).
* Operacja GetThroughput nie jest obsługiwana w regionach świadczenia usługi Microsoft Azure Administracja USA i Chiny.
* Narzędzie nie może profilować maszyn wirtualnych, jeśli serwer vCenter ma co najmniej dwie maszyny wirtualne o takiej samej nazwie lub takim samym adresie IP na różnych hostach ESXi. W tej wersji narzędzie pomija profilowanie w przypadku zduplikowanych nazw/adresów IP maszyn wirtualnych w parametrze VMListFile. Aby obejść ten problem, profiluj maszyny wirtualne przy użyciu hosta ESXi, a nie serwera vCenter. Musisz uruchomić jedno wystąpienie każdego hosta ESXi.

