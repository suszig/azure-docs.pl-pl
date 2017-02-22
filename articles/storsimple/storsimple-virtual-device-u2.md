---
title: "Urządzenie wirtualne StorSimple z aktualizacją Update 2| Microsoft Docs"
description: "Informacje na temat tworzenia, wdrażania urządzenia wirtualnego StorSimple oraz zarządzania nim w sieci wirtualnej platformy Microsoft Azure. (Dotyczy urządzenia wirtualnego StorSimple z aktualizacją Update 2)."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: f37752a5-cd0c-479b-bef2-ac2c724bcc37
ms.service: storsimple
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/16/2016
ms.author: alkohli
translationtype: Human Translation
ms.sourcegitcommit: b84e07b26506149cf9475491b32b9ff3ea9ae80d
ms.openlocfilehash: c081f31acb7d8767343f41be59d75616fa14b2da


---
# <a name="deploy-and-manage-a-storsimple-virtual-device-in-azure"></a>Wdrażanie urządzenia wirtualnego StorSimple oraz zarządzanie nim na platformie Azure
## <a name="overview"></a>Omówienie
Urządzenie wirtualne z serii StorSimple 8000 zapewnia dodatkową funkcję dołączoną do rozwiązania Microsoft Azure StorSimple. Urządzenie wirtualne StorSimple działa na maszynie wirtualnej w sieci wirtualnej Microsoft Azure i służy do tworzenia kopii zapasowych oraz klonowania danych z hostów. W tym samouczku opisano wdrażanie urządzenia wirtualnego oraz zarządzanie nim na platformie Azure. Zawarte w nim informacje dotyczą wszystkich urządzeń wirtualnych z oprogramowaniem w wersji z aktualizacją Update 2 oraz w wersjach starszych.

#### <a name="virtual-device-model-comparison"></a>Porównanie modeli urządzenia wirtualnego
Dostępne są dwa modele urządzenia wirtualnego StorSimple: standardowy — 8010 (wcześniej zwany 1100) oraz premium — 8020 (wprowadzony w aktualizacji Update 2). Modele porównano w tabeli poniżej.

| Model urządzenia | 8010<sup>1</sup> | 8020 |
| --- | --- | --- |
| **Maksymalna pojemność** |30 TB |64 TB |
| **Maszyna wirtualna platformy Azure** |Standard_A3 (4 rdzenie, 7 GB pamięci) |Standard_DS3 (4 rdzenie, 14 GB pamięci) |
| **Zgodność wersji** |Wersje przed wprowadzeniem aktualizacji Update 2 lub nowsze |Wersje z aktualizacją Update 2 lub nowsze |
| **Dostępność w danym regionie** |Wszystkie regiony platformy Azure |Regiony platformy Azure obsługujące usługę Premium Storage<br></br>Aby wyświetlić listę regionów, zobacz [obsługiwane regiony dla serii 8020](#supported-regions-for-8020) |
| **Typ magazynu** |Używa usługi Azure Standard Storage dla dysków lokalnych<br></br> Informacje na temat [tworzenia konta Standard Storage](../storage/storage-create-storage-account.md) |Używa usługi Azure Premium Storage dla dysków lokalnych<sup>2</sup> <br></br>Informacje na temat [tworzenia konta Premium Storage](../storage/storage-premium-storage.md) |
| **Wskazówki dotyczące obciążenia** |Pobieranie plików z kopii zapasowych na poziomie elementu |Tworzenie chmur i scenariusze testowania, krótki czas oczekiwania, bardziej wydajne obciążenia <br></br>Urządzenie pomocnicze do odzyskiwania po awarii |

<sup>1</sup> *Poprzednia nazwa: 1100*.

<sup>2</sup> *Urządzenia 8010 i 8020 korzystają z usługi Azure Standard Storage dla warstwy chmury. Różnica istnieje tylko w warstwie lokalnej urządzenia*.

#### <a name="supported-regions-for-8020"></a>Obsługiwane regiony dla urządzenia 8020
Regiony usługi Premium Storage obsługiwane obecnie przez urządzenie 8020 wyszczególniono w poniższej tabeli. Lista ta będzie cały czas uaktualniania, w miarę jak usługa Premium Storage będzie udostępniana w kolejnych regionach.

| Mag. nr | Obecnie obsługiwany w regionach |
| --- | --- |
| 1 |Środkowe stany USA |
| 2 |Wschodnie stany USA |
| 3 |Wschodnie stany USA 2 |
| 4 |Zachodnie stany USA |
| 5 |Europa Północna |
| 6 |Europa Zachodnia |
| 7 |Azja Południowo-Wschodnia |
| 8 |Japonia Wschodnia |
| 9 |Japonia Zachodnia |
| 10 |Australia Wschodnia |
| 11 |Australia Południowo-Wschodnia* |
| 12 |Azja Wschodnia* |
| 13 |Południowo-środkowe stany USA* |

* Usługa Premium Storage została ostatnio uruchomiona w tych obszarach geograficznych.

W tym artykule opisano krok po kroku proces wdrażania urządzenia wirtualnego StorSimple w systemie Azure. Zapoznanie się z tym artykułem umożliwi:

* Zrozumienie, czym urządzenie wirtualne różni się od urządzenia fizycznego.
* Tworzenie i konfigurowanie urządzenia wirtualnego.
* Nawiązywanie połączenia z urządzeniem wirtualnym.
* Obsługę urządzenia wirtualnego.

Ten samouczek dotyczy wszystkich urządzeń wirtualnych StorSimple z aktualizacją Update 2 i nowszą.

## <a name="how-the-virtual-device-differs-from-the-physical-device"></a>Czym urządzenie wirtualne różni się od urządzenia fizycznego
Urządzenie wirtualne StorSimple jest wersją urządzenia StorSimple istniejącą tylko w formie oprogramowania, która działa w jednym węźle maszyny wirtualnej w programie Microsoft Azure. Urządzenie wirtualne obsługuje scenariusze odzyskiwania po awarii, w których urządzenie fizyczne nie jest dostępne, i jest przeznaczone do użycia podczas pobierania z kopii zapasowych na poziomie elementu, lokalnego odzyskiwania po awarii, a także scenariuszy tworzenia i testowania chmury.

#### <a name="differences-from-the-physical-device"></a>Różnice względem urządzenia fizycznego
W poniższej tabeli przedstawiono niektóre podstawowe różnice między wirtualnym i fizycznym urządzeniem StorSimple.

|  | Urządzenie fizyczne | Urządzenie wirtualne |
| --- | --- | --- |
| **Lokalizacja** |Znajduje się w centrum danych. |Działa w systemie Azure. |
| **Interfejsy sieciowe** |Ma sześć interfejsów sieciowych: DANE 0 do DANE 5. |Ma tylko jeden interfejs sieciowy: DANE 0. |
| **Rejestracja** |Rejestrowane w ramach konfigurowania. |Rejestracja jest osobnym zadaniem. |
| **Klucz szyfrowania danych usługi** |Należy wygenerować ponownie na urządzeniu fizycznym, a następnie zaktualizować urządzenie wirtualne za pomocą nowego klucza. |Nie można ponownie wygenerować z urządzenia wirtualnego. |

## <a name="prerequisites-for-the-virtual-device"></a>Wymagania wstępne dotyczące urządzeń wirtualnych
Poniższe sekcje zawierają opis wymagań wstępnych dotyczących konfiguracji urządzenia wirtualnego StorSimple. Przed wdrożeniem urządzenia wirtualnego zapoznaj się z [zagadnieniami dotyczącymi zabezpieczeń podczas używania urządzenia wirtualnego](storsimple-security.md#storsimple-virtual-device-security).

#### <a name="azure-requirements"></a>Wymagania systemu Azure
Przed zainicjowaniem obsługi urządzenia wirtualnego należy przygotować następujące elementy w środowisku platformy Azure:

* [Skonfiguruj sieć wirtualną na platformie Azure](../virtual-network/virtual-networks-create-vnet-classic-portal.md) dla urządzenia wirtualnego. W przypadku korzystania z usługi Premium Storage należy utworzyć sieć wirtualną w regionie platformy Azure obsługującym tę usługę. Więcej informacji na temat [regionów, które są obecnie obsługiwane przez urządzenia 8020](#supported-regions-for-8020).
* Zaleca się używanie domyślnego serwera DNS zapewnionego w systemie Azure zamiast określania własnej nazwy serwera DNS. Jeśli nazwa serwera DNS jest nieprawidłowa lub jeśli serwer DNS nie jest w stanie poprawnie rozpoznać adresów IP, tworzenie urządzenia wirtualnego zakończy się niepowodzeniem.
* Sieci typu punkt do lokacji i lokacja do lokacji są opcjonalne. W razie potrzeby można skonfigurować te opcje dla bardziej zaawansowanych scenariuszy.
* Można utworzyć [maszyny wirtualne Azure Virtual Machines](../virtual-machines/virtual-machines-linux-about.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (serwery hosta) w sieci wirtualnej, która może korzystać z woluminów udostępnionych przez urządzenie wirtualne. Serwery te muszą spełniać następujące wymagania:                             

  * Muszą być maszynami wirtualnymi z systemem Windows lub Linux z zainstalowanym oprogramowaniem iSCSI Initiator.
  * Muszą działać w tej samej sieci wirtualnej co urządzenie wirtualne.
  * Muszą mieć możliwość połączenia z obiektem docelowym iSCSI urządzenia wirtualnego za pośrednictwem wewnętrznego adresu IP urządzenia wirtualnego.
* Należy sprawdzić, czy skonfigurowano obsługę dla ruchu iSCSI i chmury w tej samej sieci wirtualnej.

#### <a name="storsimple-requirements"></a>Wymagania dotyczące urządzenia StorSimple
Przed utworzeniem urządzenia wirtualnego pobierz następujące aktualizacje usługi Azure StorSimple:

* Dodaj [rekordy kontroli dostępu](storsimple-manage-acrs.md) do maszyn wirtualnych, które będą serwerami hosta dla urządzenia wirtualnego.
* Użyj [konta magazynu](storsimple-manage-storage-accounts.md#add-a-storage-account) w tym samym regionie, w którym znajduje się urządzenie wirtualne. Jeśli konta usługi Storage są w różnych regionach, wydajność może zostać obniżona. Na urządzeniu wirtualnym można używać konta Standard lub Premium Storage. Więcej informacji na temat tworzenia [konta Standard Storage](../storage/storage-create-storage-account.md) lub [konta Premium Storage](../storage/storage-premium-storage.md)
* Do utworzenia urządzenia wirtualnego użyj innego konta magazynu niż to używane do danych. Użycie tego samego konta magazynu może spowodować obniżenie wydajności.

Upewnij się, że przed rozpoczęciem masz przygotowane następujące informacje:

* Konto klasycznego portalu Azure z poświadczeniami dostępu.
* Kopia klucza szyfrowania danych usługi z urządzenia fizycznego.

## <a name="create-and-configure-the-virtual-device"></a>Tworzenie i konfigurowanie urządzenia wirtualnego
Przed wykonaniem tych procedur upewnij się, że spełniono [wymagania wstępne dotyczące urządzeń wirtualnych](#prerequisites-for-the-virtual-device).

Po utworzeniu sieci wirtualnej, skonfigurowaniu usługi Menedżer StorSimple i zarejestrowaniu urządzenia fizycznego StorSimple w usłudze możesz wykonać następujące kroki, aby utworzyć i skonfigurować urządzenie wirtualne StorSimple.

### <a name="step-1-create-a-virtual-device"></a>Krok 1. Tworzenie urządzenia wirtualnego
Wykonaj poniższe kroki, aby utworzyć urządzenie wirtualne StorSimple.

[!INCLUDE [Create a virtual device](../../includes/storsimple-create-virtual-device-u2.md)]

Jeśli tworzenie urządzenia wirtualnego zakończy się niepowodzeniem w trakcie tego kroku, może to oznaczać brak łączności z Internetem. Aby uzyskać więcej informacji, przejdź do sekcji [rozwiązywania problemów z błędami łączności internetowej](#troubleshoot-internet-connectivity-errors) podczas tworzenia urządzenia wirtualnego.

### <a name="step-2-configure-and-register-the-virtual-device"></a>Krok 2: Konfigurowanie i rejestrowanie urządzenia wirtualnego
Przed rozpoczęciem tej procedury upewnij się, że masz kopię klucza szyfrowania danych usługi. Klucz szyfrowania danych usługi został utworzony podczas konfiguracji pierwszego urządzenia StorSimple i zapisany w bezpiecznej lokalizacji. Jeśli nie masz kopii klucza szyfrowania danych usługi, w celu uzyskania pomocy musisz skontaktować się z działem pomocy firmy Microsoft.

Wykonaj poniższe kroki, aby skonfigurować i zarejestrować urządzenie wirtualne StorSimple.

[!INCLUDE [Configure and register a virtual device](../../includes/storsimple-configure-register-virtual-device.md)]

### <a name="step-3-optional-modify-the-device-configuration-settings"></a>Krok 3 (opcjonalny): Modyfikowanie ustawień konfiguracji dla urządzeń
W poniższej sekcji opisano ustawienia konfiguracji dla urządzeń wymagane dla urządzenia wirtualnego StorSimple, jeśli chcesz użyć protokołu CHAP, przystawki StorSimple Snapshot Manager lub zmienić hasło administratora urządzenia.

#### <a name="configure-the-chap-initiator"></a>Konfigurowanie inicjatora protokołu CHAP
Ten parametr zawiera poświadczenia, których urządzenie wirtualne (obiekt docelowy) oczekuje od inicjatorów (serwerów) próbujących uzyskać dostęp do woluminów. Inicjatory zapewnią nazwę użytkownika protokołu CHAP i hasło protokołu CHAP w celu zidentyfikowania się względem urządzenia podczas tego uwierzytelniania. Aby uzyskać szczegółowy opis kroków, przejdź do tematu [Configure CHAP for your device](storsimple-configure-chap.md#unidirectional-or-one-way-authentication) (Konfigurowanie protokołu CHAP dla urządzenia).

#### <a name="configure-the-chap-target"></a>Konfigurowanie obiektu docelowego protokołu CHAP
Ten parametr zawiera poświadczenia, które są używane przez urządzenie wirtualne, gdy inicjator z włączonym protokołem CHAP żąda uwierzytelniania wzajemnego lub dwukierunkowego. Urządzenie wirtualne użyje nazwy użytkownika i hasła protokołu uwierzytelniania odwrotnego CHAP w celu zidentyfikowania się względem inicjatora podczas tego procesu uwierzytelniania. Należy pamiętać, że ustawienia obiektu docelowego protokołu CHAP są ustawieniami globalnymi. Gdy są stosowane, wszystkie woluminy połączone z urządzeniem wirtualnym magazynu używają uwierzytelniania protokołu CHAP. Aby uzyskać szczegółowy opis kroków, przejdź do tematu [Configure CHAP for your device](storsimple-configure-chap.md#bidirectional-or-mutual-authentication) (Konfigurowanie protokołu CHAP dla urządzenia).

#### <a name="configure-the-storsimple-snapshot-manager-password"></a>Konfigurowanie hasła do programu StorSimple Snapshot Manager
Oprogramowanie StorSimple Snapshot Manager jest zainstalowane na hoście z systemem Windows i umożliwia administratorom zarządzanie kopiami zapasowymi urządzenia StorSimple przez tworzenie migawek lokalnych i w chmurze.

> [!NOTE]
> Dla urządzenia wirtualnego host z systemem Windows jest maszyną wirtualną na platformie Azure.
>
>

Podczas konfigurowania urządzenia w programie StorSimple Snapshot Manager zostanie wyświetlony monit o podanie adresu IP i hasła urządzenia StorSimple w celu uwierzytelnienia urządzenia magazynu. Aby uzyskać szczegółowy opis kroków, przejdź do tematu [Configure StorSimple Snapshot Manager password](storsimple-change-passwords.md#change-the-storsimple-snapshot-manager-password) (Konfigurowanie hasła do programu StorSimple Snapshot Manager).

#### <a name="change-the-device-administrator-password"></a>Zmiana hasła administratora urządzenia
W przypadku uzyskiwania dostępu do urządzenia wirtualnego z użyciem interfejsu programu Windows PowerShell trzeba wprowadzić hasło administratora urządzenia. W celu zachowania bezpieczeństwa danych przed użyciem urządzenia wirtualnego trzeba to hasło zmienić. Aby uzyskać szczegółowy opis kroków, przejdź do tematu [Configure device administrator password](storsimple-change-passwords.md#change-the-device-administrator-password) (Konfigurowanie hasła administratora urządzenia).

## <a name="connect-remotely-to-the-virtual-device"></a>Zdalne nawiązywanie połączenia z urządzeniem wirtualnym
Zdalny dostęp do urządzenia wirtualnego za pośrednictwem interfejsu programu Windows PowerShell nie jest włączony domyślnie. Należy najpierw włączyć zarządzanie zdalne na urządzeniu wirtualnym, a następnie na kliencie, który będzie używany do uzyskiwania dostępu do urządzenia wirtualnego.

Dwuetapowy proces zdalnego połączenia opisano szczegółowo poniżej.

### <a name="step-1-configure-remote-management"></a>Krok 1: Konfigurowanie zdalnego zarządzania
Wykonaj poniższe kroki, aby skonfigurować zarządzanie zdalne dla urządzenia wirtualnego StorSimple.

[!INCLUDE [Configure remote management via HTTP for virtual device](../../includes/storsimple-configure-remote-management-http-device.md)]

### <a name="step-2-remotely-access-the-virtual-device"></a>Krok 2: Zdalne uzyskanie dostępu do urządzenia wirtualnego
Po włączeniu zdalnego zarządzania na stronie konfiguracji urządzenia StorSimple można użyć funkcji komunikacji zdalnej programu Windows PowerShell do nawiązania połączenia z urządzeniem wirtualnym z innej maszyny wirtualnej znajdującej się w tej samej sieci wirtualnej; na przykład można nawiązać połączenie z hosta maszyny wirtualnej, który został skonfigurowany i użyty do nawiązania połączenia z interfejsem iSCSI. W większości wdrożeń będzie już otwarty publiczny punkt końcowy dostępu do hosta maszyny wirtualnej, który służy do uzyskiwania dostępu do urządzenia wirtualnego.

> [!WARNING]
> **Zdecydowanie zalecamy, aby w celu zwiększenia bezpieczeństwa używać protokołu HTTPS podczas nawiązywania połączenia z punktami końcowymi, a następnie usunąć punkty końcowe po zakończeniu sesji zdalnej programu PowerShell.**
>
>

Aby skonfigurować komunikację zdalną dla urządzenia wirtualnego, należy wykonać procedury opisane w temacie [Connecting remotely to your StorSimple device](storsimple-remote-connect.md) (Nawiązywanie połączenia zdalnego z urządzeniem StorSimple).

## <a name="connect-directly-to-the-virtual-device"></a>Nawiązywanie bezpośredniego połączenia z urządzeniem wirtualnym
Można również nawiązać bezpośrednie połączenie z urządzeniem wirtualnym. Jeśli chcesz nawiązać bezpośrednie połączenie z urządzeniem wirtualnym z innego komputera spoza sieci wirtualnej lub znajdującego się poza środowiskiem Microsoft Azure, musisz utworzyć dodatkowe punkty końcowe, zgodnie z opisem w procedurze poniżej.

Wykonaj poniższe kroki, aby utworzyć publiczny punkt końcowy na urządzeniu wirtualnym.

[!INCLUDE [Create public endpoints on a virtual device](../../includes/storsimple-create-public-endpoints-virtual-device.md)]

Firma Microsoft zaleca nawiązywanie połączenia z innej maszyny wirtualnej znajdującej się w tej samej sieci wirtualnej, ponieważ takie rozwiązanie zmniejsza do minimum liczbę publicznych punktów końcowych w sieci wirtualnej. Korzystając z tej metody, wystarczy nawiązać połączenie z maszyną wirtualną za pośrednictwem sesji pulpitu zdalnego, po czym skonfigurować maszynę wirtualną do użytku, jak w przypadku innych klientów z systemem Windows w sieci lokalnej. Nie trzeba dołączać numeru portu publicznego, ponieważ port będzie już znany.

## <a name="work-with-the-storsimple-virtual-device"></a>Praca z urządzeniem wirtualnym StorSimple
Po utworzeniu i skonfigurowaniu urządzenia wirtualnego StorSimple można rozpocząć pracę. Na urządzeniu wirtualnym można pracować z kontenerami woluminów, woluminami i zasadami tworzenia kopii zapasowych tak samo jak na urządzeniu fizycznym StorSimple; jedyna różnica polega na tym, że trzeba koniecznie wybrać urządzenie wirtualne z listy urządzeń. Zapoznaj się z tematem dotyczącym [użycia usługi StorSimple Manager do zarządzania urządzeniem wirtualnym](storsimple-manager-service-administration.md), w którym opisano krok po kroku procedury różnych zadań zarządzania dla urządzenia wirtualnego.

W poniższych sekcjach omówiono niektóre różnice, które wystąpią podczas pracy z urządzeniem wirtualnym.

### <a name="maintain-a-storsimple-virtual-device"></a>Obsługa urządzenia wirtualnego StorSimple
Ponieważ urządzenie wirtualne jest tylko w formie oprogramowania, jego wymagania w zakresie obsługi są ograniczone do minimum w porównaniu z obsługą urządzenia fizycznego. Istnieją następujące opcje:

* **Aktualizacje oprogramowania** — można wyświetlić datę ostatniej aktualizacji oprogramowania oraz wszystkie komunikaty o stanie aktualizacji. Można użyć przycisku **Skanuj aktualizacje** widocznego u dołu strony, aby ręcznie sprawdzić dostępność nowych aktualizacji. Jeśli są dostępne aktualizacje, kliknij przycisk **Zainstaluj aktualizacje**, aby rozpocząć instalację. Ponieważ na urządzeniu wirtualnym zainstalowany jest tylko jeden interfejs, oznacza to, że podczas instalowania aktualizacji może wystąpić niewielkie zakłócenie w działaniu usługi. Urządzenie wirtualne zostanie wyłączone i uruchomione ponownie (w razie konieczności), aby umożliwić zainstalowanie wprowadzonych aktualizacji. Aby zapoznać się z procedurą krok po kroku, przejdź do tematu dotyczącego [aktualizacji urządzenia](storsimple-update-device.md#install-regular-updates-via-the-azure-classic-portal).
* **Pakiet obsługi** — aby ułatwić działowi pomocy technicznej firmy Microsoft rozwiązywanie problemów z urządzeniem wirtualnym, można utworzyć i załadować pakiet obsługi. Aby zapoznać się z procedurą krok po kroku, przejdź do tematu dotyczącego [tworzenia pakietu obsługi i zarządzania nim](storsimple-create-manage-support-package.md).

### <a name="storage-accounts-for-a-virtual-device"></a>Konta magazynu dla urządzeń wirtualnych
Konta magazynu są przeznaczone do użycia przez usługę Menedżer StorSimple, urządzenie wirtualne oraz urządzenie fizyczne. Podczas tworzenia kont magazynu warto użyć identyfikatora regionu w przyjaznej nazwie, aby mieć pewność, że region jest spójny we wszystkich składnikach systemu. W przypadku urządzenia wirtualnego ważne jest, aby wszystkie składniki znajdowały się w tym samym regionie, gdyż zapobiega to problemom z wydajnością.

Aby zapoznać się z procedurą krok po kroku, przejdź do tematu dotyczącego [dodawania konta magazynu](storsimple-manage-storage-accounts.md#add-a-storage-account).

### <a name="deactivate-a-storsimple-virtual-device"></a>Dezaktywacja urządzenia wirtualnego StorSimple
Dezaktywacja urządzenia wirtualnego usuwa maszynę wirtualną i zasoby utworzone podczas inicjowania jej obsługi. Po dezaktywacji urządzenia wirtualnego nie można go przywrócić do poprzedniego stanu. Przed dezaktywacją urządzenia wirtualnego należy koniecznie zatrzymać lub usunąć klientów i hosty, które od niego zależą.

Po dezaktywacji urządzenia wirtualnego:

* Urządzenie wirtualne zostanie usunięte.
* Dysk systemu operacyjnego i dyski danych utworzone dla urządzenia wirtualnego zostaną usunięte.
* Hostowana usługa i sieć wirtualna, które zostały utworzone podczas inicjowania obsługi zostaną zachowywane. Jeśli nie są używane, należy je usunąć ręcznie.
* Migawki w chmurze utworzone dla urządzenia wirtualnego zostaną zachowane.

Aby zapoznać się z procedurą krok po kroku, przejdź do tematu [Deactivate and delete your StorSimple device](storsimple-deactivate-and-delete-device.md) (Dezaktywacja i usuwanie urządzenia StorSimple).

Gdy tylko urządzenie wirtualne zostanie wyświetlone jako dezaktywowane na stronie usługi StorSimple Manager, możesz je usunąć z listy urządzeń na stronie **Urządzenia**.

### <a name="start-stop-and-restart-a-virtual-device"></a>Uruchamianie, zatrzymywanie i ponowne uruchamianie urządzenia wirtualnego
Inaczej niż w przypadku urządzenia fizycznego StorSimple na urządzeniu wirtualnym StorSimple nie ma przycisku włączania zasilania. Mogą jednak wystąpić sytuacje, w których trzeba będzie zatrzymać i ponownie uruchomić urządzenie wirtualne. Na przykład niektóre aktualizacje mogą wymagać ponownego uruchomienia maszyny wirtualnej w celu zakończenia procesu aktualizacji. Najprościej jest uruchomić, zatrzymać i ponownie uruchomić urządzenie wirtualne, używając Konsoli zarządzania maszynami wirtualnymi.

Po wyświetleniu Konsoli zarządzania stanem urządzenia wirtualnego jest **Uruchomiono**, ponieważ po utworzeniu jest ono domyślnie uruchamiane. Można uruchomić, zatrzymać i ponownie uruchomić maszynę wirtualną w dowolnym momencie.

[!INCLUDE [Stop and restart virtual device](../../includes/storsimple-stop-restart-virtual-device.md)]

### <a name="reset-to-factory-defaults"></a>Resetowanie do domyślnych ustawień fabrycznych
Jeśli zdecydujesz się po prostu zacząć od nowa z urządzeniem wirtualnym, dezaktywuj je i usuń, a następnie utwórz nowe. Podobnie jak podczas resetowania urządzenia fizycznego na nowym urządzeniu wirtualnym nie będą zainstalowane żadne aktualizacje; dlatego przed jego użyciem koniecznie sprawdź dostępność aktualizacji.

## <a name="fail-over-to-the-virtual-device"></a>Awaryjne przełączenie się na urządzenie wirtualne
Odzyskiwanie po awarii jest jednym z kluczowych scenariuszy, do którego zostało przeznaczone urządzenie wirtualne StorSimple. W tym scenariuszu urządzenie fizyczne StorSimple lub całe centrum danych mogą nie być dostępne. Na szczęście operacje można przywrócić w innej lokalizacji za pomocą urządzenia wirtualnego. Podczas odzyskiwania po awarii kontenery woluminów z urządzenia źródłowego zmieniają właściciela i zostają przekazane do urządzenia wirtualnego. Zgodnie z wymaganiami wstępnymi dotyczącymi odzyskiwania po awarii urządzenie wirtualne zostało utworzone i skonfigurowane, wszystkie woluminy w kontenerze woluminów zostały przeniesione w tryb offline, a kontener woluminów ma skojarzoną migawkę w chmurze.

> [!NOTE]
> * W przypadku korzystania z urządzenia wirtualnego jako urządzenia pomocniczego do odzyskiwania po awarii należy pamiętać, że urządzenie 8010 ma magazyn o pojemności 30 TB w usłudze Standard Storage, a urządzenie 8020 ma 64 TB w usłudze Premium Storage. Urządzenie wirtualne 8020 o większej pojemności może być bardziej odpowiednie do scenariusza odzyskiwania po awarii.
> * Nie można przejść w tryb failover ani klonować z poziomu urządzenia z aktualizacją Update 2 na urządzenie z oprogramowaniem przed wprowadzeniem aktualizacji Update 1. Można jednak przejść w tryb failover z poziomu urządzenia z aktualizacją Update 2 na urządzenie z aktualizacją Update 1 (1.1 lub 1.2)
>
>

Aby zapoznać się z procedurą krok po kroku, przejdź do tematu dotyczącego [przechodzenia w tryb failover na urządzenie wirtualne](storsimple-device-failover-disaster-recovery.md#fail-over-to-a-storsimple-virtual-device).

## <a name="shut-down-or-delete-the-virtual-device"></a>Wyłączanie lub usuwanie urządzeń wirtualnych
Jeśli urządzenie wirtualne StorSimple zostało wcześniej skonfigurowane i było używane, ale teraz chcemy wyłączyć naliczanie opłat za obliczenia w ramach jego użycia, możemy wyłączyć urządzenie wirtualne. Wyłączenie urządzenia wirtualnego nie powoduje usunięcia jego systemu operacyjnego ani dysków z danymi z magazynu. Powoduje ono zaprzestanie naliczania opłat w ramach subskrypcji, ale opłaty za magazyn dla systemu operacyjnego i dysków z danymi będą nadal pobierane.

W przypadku usunięcia lub wyłączenia urządzenia wirtualnego będzie ono wyświetlane jako **W trybie offline** na stronie Urządzenia w usłudze StorSimple Manager. Jeśli chcesz również usunąć kopie zapasowe utworzone przez urządzenie wirtualne, możesz zdecydować się na dezaktywację lub usunięcie urządzenia. Aby uzyskać więcej informacji, zobacz temat [Deactivate and delete a StorSimple device](storsimple-deactivate-and-delete-device.md) (Dezaktywacja i usuwanie urządzenia StorSimple).

[!INCLUDE [Shut down a virtual device](../../includes/storsimple-shutdown-virtual-device.md)]

[!INCLUDE [Delete a virtual device](../../includes/storsimple-delete-virtual-device.md)]

## <a name="troubleshoot-internet-connectivity-errors"></a>Rozwiązywanie problemów z błędami łączności internetowej
Jeśli podczas tworzenia urządzenia wirtualnego nie ma łączności z Internetem, krok związany z tworzeniem zakończy się niepowodzeniem. Aby rozwiązać problemy w przypadku awarii wynikającej z łączności z Internetem, wykonaj poniższe czynności w klasycznym portalu Azure:

1. Utwórz maszynę wirtualną systemu Windows Server 2012 na platformie Azure. Ta maszyna wirtualna powinna używać tego samego konta magazynu, sieci wirtualnej i podsieci, które są używane przez urządzenie wirtualne. Jeśli masz już istniejący host systemu Windows Server na platformie Azure używający tego samego konta magazynu, sieci wirtualnej i podsieci, możesz również użyć go, aby rozwiązywać problemy z łącznością z Internetem.
2. Zdalnie zaloguj się do maszyny wirtualnej utworzonej w poprzednim kroku.
3. Otwórz okno poleceń w ramach maszyny wirtualnej (naciśnij klawisze Win + R, a następnie wpisz polecenie `cmd`).
4. W wierszu polecenia wpisz następujące polecenie.

    `nslookup windows.net`
5. Jeśli narzędzie `nslookup` ulegnie awarii, oznacza to, że błąd łączności z Internetem uniemożliwia zarejestrowanie urządzenia wirtualnego w usłudze StorSimple Manager.
6. Wprowadź wymagane zmiany w sieci wirtualnej, aby zapewnić, że urządzenie wirtualne może uzyskać dostęp do witryn platformy Azure, na przykład „windows.net”.

## <a name="next-steps"></a>Następne kroki
* Informacje na temat [używania usługi StorSimple Manager do zarządzania urządzeniem wirtualnym](storsimple-manager-service-administration.md).
* Zapoznanie się ze sposobem [przywracania woluminu StorSimple z zestawu kopii zapasowych](storsimple-restore-from-backup-set.md).



<!--HONumber=Feb17_HO2-->


