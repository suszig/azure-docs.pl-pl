---
title: "Konfigurowanie odzyskiwania po awarii maszyn wirtualnych VMware lub serwerów fizycznych do lokacji dodatkowej | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano sposób replikowania lokalnych maszyn wirtualnych VMware lub systemem Windows lub Linux serwerów fizycznych do lokacji dodatkowej, przy użyciu usługi Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: b2cba944-d3b4-473c-8d97-9945c7eabf63
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/05/2017
ms.author: raynew
ms.openlocfilehash: 8cfaa56735c1f4e2e01b58fdde2ad0e77b388762
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2017
---
# <a name="set-up-disaster-recovery-of-vmware-virtual-machines-or-physical-servers-to-a-secondary-site"></a>Konfigurowanie odzyskiwania po awarii maszyn wirtualnych VMware lub serwerów fizycznych do lokacji dodatkowej


InMage Scout w usłudze Azure Site Recovery zapewnia w czasie rzeczywistym replikacji między lokacjami lokalnymi VMware. InMage Scout znajduje się w ramach subskrypcji usługi Azure Site Recovery.

Jeśli nie masz subskrypcji platformy Azure, [utworzyć bezpłatne konto](https://azure.microsoft.com/pricing/free-trial/) przed rozpoczęciem.


## <a name="create-a-vault"></a>Tworzenie magazynu
1. Zaloguj się do witryny [Azure Portal](https://portal.azure.com/) > **Recovery Services**.
2. Kliknij przycisk Nowy > zarządzania > kopii zapasowych i odzyskiwania lokacji (OMS). Alternatywnie możesz kliknąć przycisk Przeglądaj > magazynu usług odzyskiwania > Dodaj.
3. W polu **Nazwa** wprowadź przyjazną nazwę identyfikującą magazyn. Jeśli masz więcej niż jedną subskrypcję, wybierz jedną z nich.
4. W **grupy zasobów** Utwórz nową grupę zasobów lub wybierz istniejący. Określ region platformy Azure, aby Wypełnij wymagane pola.
5. W **lokalizacji**, wybierz region geograficzny magazynu. Aby sprawdzić obsługiwane regiony, zobacz [cennik usługi Azure Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/).
6. Jeśli chcesz, aby szybko uzyskać dostęp do magazynu z poziomu pulpitu nawigacyjnego kliknij polecenie Przypnij do pulpitu nawigacyjnego, a następnie kliknij przycisk Utwórz.
7. Nowy magazyn będzie wyświetlany na pulpicie nawigacyjnym > wszystkie zasoby i na głównej usług odzyskiwania magazyny strony.

## <a name="configure-the-vault-and-download-inmage-scout-components"></a>Konfigurowanie magazynu i pobrać InMage Scout składników
1. W usługach odzyskiwania magazynów wybierz magazyn i kliknij przycisk **ustawienia**.
2. W **ustawienia** > **wprowadzenie** kliknij **usługi Site Recovery** > krok 1: **przygotowanie infrastruktury**  >  **Cel ochrony**.
3. W **cel ochrony** wybierz do odzyskiwania lokacji, a następnie wybierz opcję Tak, z programem VMware vSphere Hypervisor. Następnie kliknij przycisk OK.
4. W **Instalatora Scout**, kliknij przycisk pobierania, aby pobrać GA InMage Scout 8.0.1 oprogramowania i klucz rejestracji. Pliki instalacyjne dla wszystkich wymaganych składników znajdują się w pliku zip pobranego.

## <a name="step-3-install-component-updates"></a>Krok 3: Instalowanie aktualizacje składników
Przeczytaj informacje o najnowszych [aktualizacje](#updates). Zainstaluj pliki aktualizacji na serwerach w następującej kolejności:

1. Serwer RX w razie potrzeby
2. Serwery konfiguracji
3. Serwery procesów
4. Głównych serwerów docelowych
5. serwery vContinuum
6. Serwer źródłowy (z systemem Windows i Linux Server)

Zainstaluj aktualizacje w następujący sposób:

1. Pobierz [aktualizacji](https://aka.ms/asr-scout-update5) pliku zip. Ten plik zip zawiera następujące pliki:

   * RX_8.0.4.0_GA_Update_4_8725872_16Sep16.tar.GZ
   * CX_Windows_8.0.4.0_GA_Update_4_8725865_14Sep16.exe
   * UA_Windows_8.0.5.0_GA_Update_5_11525802_20Apr17.exe
   * UA_RHEL6 64_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz
   * vCon_Windows_8.0.5.0_GA_Update_5_11525767_20Apr17.exe
   * Usługa bits update4 UA systemem RHEL5 OL5, OL6, SUSE 10, SUSE 11: UA_<Linux OS>_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz
2. Wyodrębnij pliki z rozszerzeniem .zip.<br>
3. **Dla serwera RX**: kopiowanie **RX_8.0.4.0_GA_Update_4_8725872_16Sep16.tar.gz** serwerowi RX i wyodrębnij go. W folderze wyodrębnionego Uruchom **/Install**.<br>
4. **Na serwerze server/procesu konfiguracji**: kopiowanie **CX_Windows_8.0.4.0_GA_Update_4_8725865_14Sep16.exe** do konfiguracji serwera i serwera przetwarzania. Kliknij dwukrotnie, aby go uruchomić.<br>
5. **Dla systemu Windows server główny cel**: Aby zaktualizować unified agent, skopiuj **UA_Windows_8.0.5.0_GA_Update_5_11525802_20Apr17.exe** na głównym serwerze docelowym. Kliknij go dwukrotnie, aby go uruchomić. Należy pamiętać, że unified agent również zastosowanie do serwera źródłowego Jeśli źródło nie zostanie zaktualizowane do Update4. Należy zainstalować go na serwerze źródłowym, jak również, jak zostało to opisane w dalszej części tej listy.<br>
6. **Serwer vContinuum**: kopiowanie **vCon_Windows_8.0.5.0_GA_Update_5_11525767_20Apr17.exe** na serwer vContinuum.  Upewnij się, że zostało zamknięte kreatora vContinuum. Kliknij dwukrotnie plik, aby go uruchomić.<br>
7. **Dla systemu Linux główny serwer docelowy**: Aby zaktualizować unified agent, skopiuj **UA_RHEL6 64_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz** do wzorca serwerze docelowym i wyodrębnij go. W folderze wyodrębnionego Uruchom **/Install**.<br>
8. **Dla serwera źródłowego z systemem Windows**: nie trzeba zainstalować agenta aktualizacji 5 w źródle, jeśli jest już uruchomiona aktualizacji w wersji 4. Jeśli działa mniej niż 4 aktualizacji, należy zastosować agent aktualizacji 5.
Aby zaktualizować unified agent, skopiuj **UA_Windows_8.0.5.0_GA_Update_5_11525802_20Apr17.exe** do serwera źródłowego. Kliknij go dwukrotnie, aby go uruchomić. <br>
9. **Dla serwera źródłowego Linux**: Aby zaktualizować unified agent, skopiuj odpowiednią wersję pliku UA na serwer z systemem Linux i wyodrębnij go. W folderze wyodrębnionego Uruchom **/Install**.  Przykład: RHEL 6,7 x 64 serwera, skopiować **UA_RHEL6 64_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz** do serwera i wyodrębnij go. W folderze wyodrębnionego Uruchom **/Install**.

## <a name="step-4-set-up-replication"></a>Krok 4: Konfigurowanie replikacji
1. Konfigurowanie replikacji między źródłem i docelowych witryn VMware.
2. Aby uzyskać wskazówki Użyj dokumentacji InMage Scout, który jest pobierany z produktem. Można też dostęp do dokumentacji w następujący sposób:

   * [Informacje o wersji](https://aka.ms/asr-scout-release-notes)
   * [Macierz zgodności](https://aka.ms/asr-scout-cm)
   * [Podręcznik użytkownika](https://aka.ms/asr-scout-user-guide)
   * [Podręcznik użytkownika RX](https://aka.ms/asr-scout-rx-user-guide)
   * [Przewodnik szybkiego instalacji](https://aka.ms/asr-scout-quick-install-guide)

## <a name="updates"></a>Aktualizacje
### <a name="azure-site-recovery-scout-801-update-5"></a>Usługi Azure Site Recovery Scout 8.0.1 aktualizacji 5
Aktualizacja Scout 5 jest aktualizacją zbiorczą. Ma ona wszystkie poprawki programu aktualizację1 do update4 i następujące nowe poprawki błędów i rozszerzenia.
Poprawki, które są dodawane z update4 Scout odzyskiwania lokacji do update5 są specyficzne dla docelowego elementu głównego i vContinuum składników. Jeśli wszystkie źródła serwery, główny cel, konfiguracji serwera, serwer przetwarzania i ODBIERANIA są już na update4 Scout odzyskiwania lokacji należy zastosować aktualizację 5 tylko na głównym serwerze docelowym. 

**Nowa funkcja obsługi platformy**
* SUSE Linux Enterprise Server 11 z dodatkiem Service Pack 4(SP4)

> [!NOTE]
> SLES 11 z dodatkiem SP4 64 bitowej **InMage_UA_8.0.1.0_SLES11-SP4-64_GA_13Apr2017_release.tar.gz** jest dostarczana z pakietem Scout GA podstawowej **InMage_Scout_Standard_8.0.1 GA.zip**. Pobierz pakiet Scout GA z portalu, jak wspomniano w [krok 1](#step-1-create-a-vault).
>

**Poprawki błędów i rozszerzenia**

* Zwiększyć niezawodność obsługi klastra systemu Windows
    * Niekiedy stałej-także niektóre P2V MSCS stają się dysków klastrowych RAW po odzyskaniu
    * Odzyskiwanie klastra Fixed-P2V MSCS zakończy się niepowodzeniem z powodu niezgodności kolejności dysku
    * Klastrze Fixed-MSCS dodać dyski, które operacja kończy się niepowodzeniem z niezgodność rozmiaru dysku
    * Klaster MSCS źródła Fixed-z sprawdzanie gotowości mapowania RDM jednostki LUN nie powiedzie się rozmiar weryfikacji
    * Fixed-jednego węzła klastra ochrony kończy się niepowodzeniem z powodu problemu niezgodność SCSI 
    * Fixed-ponownego włączenia ochrony serwera klastra systemu Windows P2V kończy się niepowodzeniem, jeśli występują dyski klastra docelowego. 
    
* Podczas ochrony powrotu po awarii, jeśli wybrane MT nie jest na tym samym serwerze ESXi z komputera chronionego źródła (podczas przekazywania ochrony), vContinuum przejmuje niewłaściwy MT podczas odzyskiwania powrotu po awarii i następnie operacja odzyskiwania nie powiedzie się.

> [!NOTE]
> 
> * Powyżej klastra P2V poprawki dotyczą tylko tych MSCS klastry fizyczne świeżo chronione przez update5 Scout odzyskiwania lokacji. Do korzystania z klastra poprawki w klastrze P2V MSCS już chronione z aktualizacjami starsza, należy wykonać kroki uaktualniania, które są wymienione w sekcji 12, uaktualnienie chronione klastrów P2V MSCS do Scout aktualizacji 5 [informacje o wersji](https://aka.ms/asr-scout-release-notes) .
> 
> * Ponownego włączenia ochrony klastra MSCS fizycznych mogą ponownie wykorzystać istniejące dyski docelowe tylko jeśli w chwili ponownego włączenia ochrony, ten sam zestaw dysków są aktywne na każdym z węzłów klastra, jak kiedy pierwotnie chroniony. Jeśli nie, nie będą wymagane ręczne wykonanie czynności wymienionych w sekcji 12 [informacje o wersji](https://aka.ms/asr-scout-release-notes) przenoszenia dysków po stronie docelowej datastore poprawną ścieżkę do ponownie ich użyć w czasie ponownego włączenia ochrony. Jeśli Wznów klastrze MSCS P2V w trybie bez następujące kroki uaktualniania następnie utworzy nowy dysk na serwerze docelowym ESXi. Należy ręcznie usunąć stary dysków z magazynu danych.
> 
> * Po każdej zmianie źródła SLES11 lub poprawnego działania ponownego SLES11 z dowolnym serwerem pakiet usługi, a następnie ręcznie należy zaznaczyć jeden **głównego** dysku pary replikacji do ponownej synchronizacji, ponieważ nie otrzymasz powiadomienie w interfejsie użytkownika CX. Jeśli nie zostanie zaznaczone na dysku głównym ponownej synchronizacji, mogą pojawić się problemy z integralnością (Podpisane) danych.
> 

### <a name="azure-site-recovery-scout-801-update-4"></a>Usługi Azure Site Recovery Scout 8.0.1 Update 4
Aktualizacja Scout 4 jest aktualizacją zbiorczą. Ma ona wszystkie poprawki programu aktualizację1 do update3 i następujące nowe poprawki błędów i rozszerzenia.

**Nowa funkcja obsługi platformy**

* Dodano obsługę dla vCenter/vSphere w wersji 6.0 i 6.1 6.2
* Dodano obsługę dla następujących systemów operacyjnych Linux
  * Red Hat Enterprise Linux (RHEL) 7.0, 7.1 i 7.2
  * CentOS 7.0, 7.1 i 7.2
  * Red Hat Enterprise Linux (RHEL) 6.8
  * CentOS 6.8

> [!NOTE]
> RHEL/CentOS 7 64-bitowym **InMage_UA_8.0.1.0_RHEL7-64_GA_06Oct2016_release.tar.gz** jest dostarczana z pakietem Scout GA podstawowej **InMage_Scout_Standard_8.0.1 GA.zip**. Pobierz pakiet Scout GA z portalu, jak wspomniano w [krok 1](#step-1-create-a-vault).
>
>

**Poprawki błędów i rozszerzenia**

* Ulepszone zamykania obsługi dla następujących systemów operacyjnych Linux i klony zapobiec problemom niechciane ponownej synchronizacji.
  * Red Hat Enterprise Linux (RHEL) 6.x
  * Oracle Linux (OL) 6.x
* Dla systemu Linux wykonaj dostęp do folderu, który uprawnienia w katalogu instalacji agenta ujednoliconego obecnie są ograniczone tylko do użytkowników lokalnych.
* W systemie Windows chronometrażu się problem podczas wystawiania wspólnej zakładka spójności rozproszone na silnie załadować aplikacji rozproszonych, takich jak klastry SQL i punktu udziału.
* Dodano dziennika powiązane poprawki w Instalatorze podstawowej CX.
* Link do pobierania vCLI 6.0 VMware jest dodawany do podstawowej Instalatora Windows głównego celu.
* Podczas pracy awaryjnej i odzyskiwania po awarii ćwiczenia, należy dodać więcej kontroli i dzienniki, aby zmiany konfiguracji sieci.
* Informacje dotyczące przechowywania pewnym czasie nie zgłoszono CX.  
* Dla klastra fizycznego woluminu zmieniać rozmiar operacji za pomocą Kreatora vContinuum kończy się niepowodzeniem podczas zmniejszania rozmiaru woluminu źródłowego.
* Klaster ochrony nie powiodło się z powodu błędu "Nie można odnaleźć podpisu dysku" kiedy dysk klastrowy znajduje się dysk PRDM.
* cxps transportu awarii serwera z powodu wyjątku out-of-range.
* Nazwa serwera i IP kolumn są teraz o zmiennym rozmiarze stronie instalacji wypychanej vContinuum kreatora.
* Rozszerzenia interfejsu API ODBIERANIA
  * Zawiera pięć najnowszych dostępnych wspólnej spójności punktów (gwarantowaną jedynie znaczniki).
  * Zapewnia wydajność i szczegóły wolnego miejsca dla wszystkich chronionych urządzeń.
  * Zawiera stan sterownika Scout na serwerze źródłowym.

> [!NOTE]
> * **InMage_Scout_Standard_8.0.1_GA.zip** teraz pakiet podstawowy został zaktualizowany Instalator podstawowej CX **InMage_CX_8.0.1.0_Windows_GA_26Feb2015_release.exe** i podstawowej Instalatora Windows główny cel **InMage_Scout_vContinuum_MT_8.0.1.0_Windows_GA_26Feb2015_release.exe**. Nowa instalacja wszystkich nowych CX i docelowego elementu głównego Windows GA usługi bits możesz używać.
> * Aktualizacja 4, które mogą być bezpośrednio stosowane w 8.0.1 po
> * Serwer konfiguracji i aktualizacje ODBIERANIA, nie można obniżyć po są one stosowane w systemie.
>
>

### <a name="azure-site-recovery-scout-801-update-3"></a>Usługi Azure Site Recovery Scout 8.0.1 Update 3
Aktualizacja 3 obejmuje następujące poprawki i ulepszenia:

* Serwer konfiguracji i ODBIERANIA nie można zarejestrować się w magazynie usługi Site Recovery, gdy są one za serwer proxy.
* Liczbę godzin, które nie zostały spełnione cel punktu odzyskiwania (RPO) nie zostały zaktualizowane w raport o kondycji.
* Serwer konfiguracji nie jest synchronizowany z ODBIERANIA, gdy szczegóły sprzętu ESX lub szczegóły sieci zawiera wszystkie znaki UTF-8.
* Kontrolery domeny systemu Windows Server 2008 R2 nie rozruchu po odzyskaniu.
* Synchronizacja w trybie offline nie działa zgodnie z oczekiwaniami.
* Po przełączeniu maszyny wirtualnej (VM) usuwania replikacji pary zablokowała w Interfejsie użytkownika CX przez długi czas, a użytkowników nie może ukończyć powrót po awarii lub wznowić działanie.
* Ogólne migawki, które zostały zoptymalizowane operacje, które są wykonywane przez zadanie spójności umożliwiają zmniejszenie aplikacji rozłącza podobnie jak klienci SQL.
* Działanie narzędzia spójności (VACP.exe) została zwiększona dzięki zmniejszeniu użycia pamięci, który jest wymagany do tworzenia migawek w systemie Windows.
* Instalacji wypychanej usługi awarii (Crash), jeśli pole hasło jest większa niż 16 znaków.
* vContinuum nie jest sprawdzanie i monitowanie o nowe poświadczenia vCenter po zmianie poświadczeń.
* W systemie Linux Menedżer pamięci podręcznej głównego celu (cachemgr) nie jest pobieranie plików z serwera przetwarzania, co powoduje ograniczenie pary replikacji.
* Jeśli kolejność dysku fizycznego trybu failover klastra (MSCS) nie jest taka sama na wszystkich węzłach, replikacji nie jest ustawiona dla niektórych woluminów klastra.
  <br/>Należy pamiętać, że klaster musi być przełączona do trybu mógł korzystać z tej poprawki.  
* Funkcje SMTP nie działa zgodnie z oczekiwaniami po uaktualnieniu RX z Scout 7.1 do Scout 8.0.1.
* Statystyka więcej zostały dodane w dzienniku na śledzenie czasu podjętą do jego ukończenia operacji wycofywania.
* Dodano obsługę dla systemów operacyjnych Linux na serwerze źródłowym:
  * Red Hat Enterprise Linux (RHEL) 6 aktualizacji 7
  * CentOS 6 aktualizacji 7
* CX i ODBIERANIA interfejsu użytkownika umożliwia teraz wyświetlanie powiadomień dla pary, który przechodzi do trybu mapy bitowej.
* RX zostały dodane następujące poprawki zabezpieczeń:

| **Opis problemu** | **Procedury implementacji** |
| --- | --- |
| Obejście autoryzacji za pomocą parametru naruszeniu |Ograniczony dostęp do innych niż odpowiednich użytkowników. |
| Sfałszowaniem żądań Cross-site |Zaimplementowane koncepcji token strony, losowo generowany dla każdej strony. <br/>Dzięki temu zostanie wyświetlony: <li> Istnieje tylko jednego logowania wystąpienia dla tego użytkownika.</li><li>Nie obsługuje odświeżania strony — nastąpi przekierowanie do pulpitu nawigacyjnego.</li> |
| Przekazywanie złośliwego pliku |Pliki ograniczone do niektórych rozszerzeń. Dozwolone są rozszerzenia: 7z, aiff, asf, avi, bmp, csv, doc, docx, fla, flv, gif, gz, gzip, jpeg, jpg, dziennika środek mov, mp3, mp4, mpc, mpeg, mpg, ods odt, pdf, png, ppt, pptx, pxd, qt, pamięci ram, rar, rm, rmi, rmvb, rtf, sdc, sitd, swf, sxc, sxw, tar, tgz, tif, tiff, txt, vsd, wav, wma, wmv, xls, xlsx, xml, a zip. |
| Trwałe skryptów krzyżowych |Dodano wejściowych operacji sprawdzania poprawności. |

> [!NOTE]
> * Wszystkie aktualizacje usługi Site Recovery kumulują się. Aktualizacja 3 ma wszystkie poprawki Update 1 i Update 2. Aktualizacja 3, które mogą być bezpośrednio stosowane w 8.0.1 po
> * Serwer konfiguracji i aktualizacje ODBIERANIA, nie można obniżyć po są one stosowane w systemie.
>
>

### <a name="azure-site-recovery-scout-801-update-2-update-03dec15"></a>Usługi Azure Site Recovery Scout 8.0.1 aktualizacji 2 (aktualizacja, 03 będzie gru 15)
Poprawki w wersji Update 2 obejmują:

* **Serwer konfiguracji**: rozwiązać problemu, który uniemożliwił wolnego funkcji zliczania 31 dni roboczych zgodnie z oczekiwaniami, gdy serwer konfiguracji został zarejestrowany w usłudze Site Recovery.
* **Unified agent**: Rozwiąż rozwiązuje problem w aktualizacji 1, które spowodowały aktualizacji nie są instalowane na głównym serwerze docelowym gdy zostało uaktualnione z wersji 8.0 lub 8.0.1.

### <a name="azure-site-recovery-scout-801-update-1"></a>Usługi Azure Site Recovery Scout 8.0.1 Update 1
Aktualizacja 1 obejmuje następujące poprawki i nowe funkcje:

* 31 dni wolne ochrony dla każdego wystąpienia serwera. Dzięki temu można przetestować funkcje lub skonfigurować Weryfikacja koncepcji.
  * Wszystkie operacje na serwerze, w tym tryb failover i powrotu po awarii, mogą w pierwszym 31 dni, rozpoczynając od godziny, serwer najpierw chronionej za pomocą Scout odzyskiwania lokacji.
  * Z 32 dzień roku każdego chronionego serwera będą naliczane opłaty według stawki standardowe wystąpienie do ochrony Azure Site Recovery do lokacji należące do klientów.
  * W dowolnym momencie liczba chronionych serwerów, które są aktualnie pobieranych jest dostępny na stronie pulpitu nawigacyjnego w magazynie usługi Azure Site Recovery.
* Dodać obsługę vSphere interfejsu wiersza polecenia (vCLI) 5.5 Update 2.
* Obsługa dodany w systemach operacyjnych Linux na serwerze źródłowym:
  * RHEL 6 aktualizacji 6
  * RHEL 5 zaktualizować 11
  * CentOS 6 aktualizacji 6
  * CentOS 5 aktualizacji 11
* Wprowadzono poprawki błędów w celu rozwiązania następujących problemów:
  * Rejestracji magazynu w konfiguracji serwera lub serwera RX kończy się niepowodzeniem.
  * Wolumin klastra nie są wyświetlane zgodnie z oczekiwaniami, gdy klastrowane maszyny wirtualne są przełączona do trybu podczas one.
  * Powrót po awarii nie powiedzie się, gdy główny serwer docelowy jest hostowany na innym serwerze ESXi z lokalnych maszyn wirtualnych produkcji.
  * Uprawnienia do pliku konfiguracji są zmieniane po uaktualnieniu do 8.0.1, który ma wpływ na operacje i ochrony.
  * Próg ponownej synchronizacji nie są wymuszane zgodnie z oczekiwaniami, która prowadzi do replikacji niespójne zachowanie.
  * Ustawienia celu punktu odzyskiwania nie są wyświetlane poprawnie w interfejsie serwera konfiguracji. Wartość kompresji danych niepoprawnie wyświetlana jest wartość skompresowane.
  * Operacja usuwania nie powoduje usunięcia, zgodnie z oczekiwaniami w Kreatorze vContinuum i replikacji nie jest usuwany z interfejsu serwera konfiguracji.
  * W Kreatorze vContinuum dysk jest automatycznie usunięte po kliknięciu **szczegóły** w widoku dysku podczas ochrony MSCS maszyn wirtualnych.
  * W tym scenariuszu, fizyczna wirtualna (P2V) wymagane usługi HP, takie jak CIMnotify i CqMgHost, nie są przenoszone do ręcznego w maszynie wirtualnej odzyskiwania. Powoduje to dodatkowe rozruchu.
  * Ochronę maszyny wirtualnej systemu Linux nie powiedzie się, gdy istnieje więcej niż 26 dysków na głównym serwerze docelowym.

## <a name="next-steps"></a>Następne kroki
Zadawania pytań, które masz na [forum usług odzyskiwania Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).
