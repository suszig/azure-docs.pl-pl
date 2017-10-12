---
title: "Wdrażanie urządzenia StorSimple (aktualizacja Update 1) | Microsoft Docs"
description: "W tym artykule opisano kroki i najlepsze rozwiązania dotyczące wdrażania usług i urządzeń StorSimple z aktualizacją Update 1."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: ac631d3c-3c53-4c9e-9e4a-5c61c0cd8167
ms.service: storsimple
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: 4d568fb2eca418ca939f7a76ac24197a0457fe47
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-your-on-premises-storsimple-device-update-1"></a>Wdrażanie lokalnego urządzenia StorSimple z aktualizacją Update 1
> [!div class="op_single_selector"]
> * [Aktualizacja 2](storsimple-deployment-walkthrough-u2.md)
> * [Aktualizacja 1](storsimple-deployment-walkthrough-u1.md)
> * [Wersja ogólnodostępna](storsimple-deployment-walkthrough.md)
> 
> 

## <a name="overview"></a>Omówienie
Witamy w procesie wdrażania urządzenia Microsoft Azure StorSimple. Te samouczki, w których opisano wdrożenie, dotyczą urządzenia z serii StorSimple 8000 z aktualizacją Update 1.0. W tej serii samouczków opisano procedurę konfigurowania urządzenia StorSimple, a także uwzględniono listę kontrolną i wymagania wstępne dotyczące konfiguracji oraz szczegółowe opisy kroków konfiguracji.

Informacje podane w tych samouczkach mają zastosowanie po zapoznaniu się ze środkami ostrożności i dopiero wówczas, gdy urządzenie StorSimple zostało rozpakowane i zamontowane w stojaku oraz podłączono do niego kable. Jeśli nadal trzeba wykonać te zadania, należy rozpocząć od sprawdzenia [środków ostrożności](storsimple-safety.md). W zależności od modelu urządzenia można następnie je rozpakować, zamontować w stojaku i podłączyć kable, wykonując instrukcje podane w następujących tematach:

* [Rozpakowywanie, montowanie w stojaku i podłączanie kabli do urządzenia 8100](storsimple-8100-hardware-installation.md)
* [Rozpakowywanie, montowanie w stojaku i podłączanie kabli do urządzenia 8600](storsimple-8600-hardware-installation.md)

Do ukończenia procesu instalacji i konfiguracji niezbędne są uprawnienia administratora. Przed rozpoczęciem warto przejrzeć listę kontrolną dotyczącą konfiguracji. Proces wdrażania i konfiguracji może potrwać pewien czas.

> [!NOTE]
> Informacje na temat wdrażania urządzenia StorSimple opublikowane w witrynie platformy Microsoft Azure w sieci Web dotyczą tylko urządzeń z serii StorSimple 8000. Aby uzyskać pełne informacje dotyczące urządzeń z serii 5000 i 7000, przejdź do strony [http://onlinehelp.storsimple.com/](http://onlinehelp.storsimple.com). Informacje dotyczące wdrażania urządzeń z serii 5000 i 7000 można znaleźć w [przewodniku Szybki start do systemu StorSimple](http://onlinehelp.storsimple.com/111_Appliance/).
> 
> 

## <a name="deployment-steps"></a>Kroki wdrażania
Wykonaj wymagane kroki, aby skonfigurować urządzenie StorSimple i połączyć je z usługą Menedżer StorSimple. Oprócz kroków wymaganych istnieją również opcjonalne kroki i procedury, które być może trzeba będzie wykonać podczas wdrażania. W szczegółowych instrukcjach dotyczących wdrażania wskazano, w którym momencie należy wykonać poszczególne kroki opcjonalne.

| Krok | Opis |
| --- | --- |
| **WYMAGANIA WSTĘPNE** |Te czynności należy wykonać w ramach przygotowań do przyszłego wdrożenia. |
| Lista kontrolna dotycząca konfiguracji wdrożenia. |Ta lista kontrolna umożliwia zbieranie i rejestrowanie informacji przed wdrożeniem i w jego trakcie. |
| Wymagania wstępne dotyczące wdrożenia. |Służą do sprawdzania, czy środowisko jest gotowe do przeprowadzenia wdrożenia. |
|  | |
| **WDROŻENIE KROK PO KROKU** |Te kroki są wymagane do wdrożenia urządzenia StorSimple w środowisku produkcyjnym. |
| Krok 1. Tworzenie nowej usługi. |Skonfiguruj magazyn i zarządzanie chmurą dla danego urządzenia StorSimple. Jeśli masz istniejącą usługę dla innych urządzeń StorSimple, pomiń ten krok. |
| Krok 2. Pobieranie klucza rejestracji usługi. |Użyj tego klucza do zarejestrowania urządzenia StorSimple i połączenia go z usługą zarządzania. |
| Krok 3. Konfigurowanie i rejestrowanie urządzenia za pomocą programu Windows PowerShell dla StorSimple. |Połącz urządzenie z siecią i zarejestruj je przy użyciu platformy Azure, aby ukończyć instalację za pomocą usługi zarządzania. |
| Krok 4. Przeprowadzenie minimalnej konfiguracji urządzenia</br>Opcjonalnie: aktualizacja urządzenia StorSimple. |Skorzystaj z usługi zarządzania, aby skonfigurować urządzenie i umożliwić przechowywanie w nim danych. |
| Krok 5. Tworzenie kontenera woluminów. |Utwórz kontener, aby umożliwić inicjowanie obsługi woluminów. Kontener woluminów obejmuje ustawienia konta magazynu, przepustowości i szyfrowania wszystkich woluminów, które zawiera. |
| Krok 6. Tworzenie woluminu. |Zainicjuj obsługę woluminów magazynu na urządzeniu StorSimple na potrzeby serwerów. |
| Krok 7. Instalowanie, inicjowanie i formatowanie woluminu.</br>Opcjonalnie: konfigurowanie wielościeżkowego wejścia/wyjścia (MPIO) |Połącz serwery z magazynem iSCSI udostępnianym przez urządzenie. Możesz opcjonalnie skonfigurować wielościeżkowe wejście/wyjście, aby upewnić się, że serwery tolerują błędy linków, sieci i interfejsu. |
| Krok 8. Tworzenie kopii zapasowej. |Skonfiguruj zasady tworzenia kopii zapasowej, aby chronić dane. |
|  | |
| **INNE PROCEDURY** |Te procedury mogą okazać się potrzebne podczas wdrażania rozwiązania. |
| Konfigurowanie nowego konta magazynu dla usługi. | |
| Łączenie z konsolą szeregową urządzenia przy użyciu programu PuTTY. | |
| Pobieranie nazwy IQN hosta z systemem Windows Server. | |
| Ręczne tworzenie kopii zapasowej. | |

## <a name="deployment-configuration-checklist"></a>Lista kontrolna dotycząca konfiguracji wdrożenia
Na poniższej liście kontrolnej dotyczącej konfiguracji wdrażania określono, jakie informacje należy zebrać przed rozpoczęciem konfigurowania oprogramowania na urządzeniu StorSimple oraz w jego trakcie. Wcześniejsze przygotowanie niektórych z tych informacji usprawni proces wdrażania urządzenia StorSimple w środowisku użytkownika. Ta lista kontrolna umożliwia również notowanie niezbędnych szczegółów konfiguracji podczas wdrażania urządzenia.

| Etap | Parametr | Szczegóły | Wartości |
| --- | --- | --- | --- |
| **Podłączanie kabli do urządzenia** |Dostęp szeregowy |Początkowa konfiguracja urządzenia |Tak/Nie |
|  | | | |
| **Konfigurowanie i rejestrowanie urządzenia** |Ustawienia sieci interfejsu Dane 0 |Adres IP interfejsu Dane 0:</br>Maska podsieci:</br>Brama:</br>Podstawowy serwer DNS:</br>Podstawowy serwer NTP:</br>Adres IP/nazwa FQDN serwera proxy sieci Web (opcjonalnie):</br>Port serwera proxy sieci Web: | |
| &nbsp; |Hasło administratora urządzenia |Hasło musi zawierać od 8 do 15 znaków, w tym małe litery, wielkie litery, cyfry i znaki specjalne. | |
| &nbsp; |Hasło programu StorSimple Snapshot Manager |Hasło musi zawierać od 14 do 15 znaków, w tym małe litery, wielkie litery, cyfry i znaki specjalne. | |
| &nbsp; |Klucz rejestracji usługi |Ten klucz jest generowany przy użyciu klasycznego portalu Azure. | |
| &nbsp; |Klucz szyfrowania danych usługi |Ten klucz jest tworzony podczas rejestrowania urządzenia przy użyciu usługi zarządzania w programie Windows PowerShell dla urządzenia StorSimple. Skopiuj ten klucz i zapisz go w bezpiecznym miejscu. | |
|  | | | |
| **Przeprowadzenie minimalnej konfiguracji urządzenia** |Przyjazna nazwa urządzenia |Jest to opisowa nazwa urządzenia. | |
| &nbsp; |Strefa czasowa |Wszystkie zaplanowane operacje urządzenia będą wykonywane w ramach tej strefy czasowej. | |
| &nbsp; |Pomocniczy serwer DNS |To jest wymagana konfiguracja. | |
| &nbsp; |Interfejs sieciowy: stałe adresy IP kontrolera Dane 0 |Te adresy IP powinny obsługiwać routing do Internetu.</br>Stały adres IP kontrolera 0:</br>Stały adres IP kontrolera 1: | |
|  | | | |
| **Dodatkowe ustawienia interfejsu sieciowego** |Interfejs sieciowy: Dane 1</br>Jeśli włączono interfejs iSCSI, nie konfiguruj bramy. |Cel: chmura/iSCSI/nie jest używany</br>Adres IP:</br>Maska podsieci:</br>Brama: | |
| &nbsp; |Interfejs sieciowy: Dane 2</br>Jeśli włączono interfejs iSCSI, nie konfiguruj bramy. |Cel: chmura/iSCSI/nie jest używany</br>Adres IP:</br>Maska podsieci:</br>Brama: | |
| &nbsp; |Interfejs sieciowy: Dane 3</br>Jeśli włączono interfejs iSCSI, nie konfiguruj bramy. |Cel: chmura/iSCSI/nie jest używany</br>Adres IP:</br>Maska podsieci:</br>Brama: | |
| &nbsp; |Interfejs sieciowy: Dane 4</br>Jeśli włączono interfejs iSCSI, nie konfiguruj bramy. |Cel: chmura/iSCSI/nie jest używany</br>Adres IP:</br>Maska podsieci:</br>Brama: | |
| &nbsp; |Interfejs sieciowy: Dane 5</br>Jeśli włączono interfejs iSCSI, nie konfiguruj bramy. |Cel: chmura/iSCSI/nie jest używany</br>Adres IP:</br>Maska podsieci:</br>Brama: | |
|  | | | |
| **Tworzenie kontenera woluminów** |Nazwa kontenera woluminów: |Nazwa kontenera | |
| &nbsp; |Konto magazynu Azure: |Klucz dostępu i nazwa konta magazynu do skojarzenia z tym kontenerem woluminu | |
| &nbsp; |Klucz szyfrowania magazynu w chmurze: |Klucz szyfrowania magazynu w poszczególnych kontenerach | |
|  | | | |
| **Tworzenie woluminu** |Szczegóły poszczególnych woluminów |Nazwa woluminu: | |
| &nbsp; |&nbsp; |Rozmiar: | |
| &nbsp; |&nbsp; |Typ użycia: | |
| &nbsp; |&nbsp; |Nazwa ACR: | |
| &nbsp; |&nbsp; |Domyślne zasady tworzenia kopii zapasowej: | |
|  | | | |
| **Instalowanie, inicjowanie i formatowanie woluminu** |Szczegóły poszczególnych serwerów hosta łączących się z magazynem |Nazwa serwera Windows Server: | |
| &nbsp; |&nbsp; |Nazwa IQN serwera Windows Server: | |
| &nbsp; |&nbsp; |Nazwa woluminu z systemem Windows Server: | |
| &nbsp; |&nbsp; |Litera dysku/punkt instalacji systemu plików NTFS: | |

## <a name="deployment-prerequisites"></a>Wymagania wstępne dotyczące wdrożenia
Poniższe sekcje zawierają opis wymagań wstępnych dotyczących konfiguracji usługi Menedżer StorSimple oraz urządzenia StorSimple.

### <a name="for-the-storsimple-manager-service"></a>Usługa Menedżer StorSimple
Przed rozpoczęciem upewnij się, że:

* Masz konto Microsoft z poświadczeniami dostępu.
* Masz konto magazynu platformy Microsoft Azure z poświadczeniami dostępu.
* Subskrypcja platformy Microsoft Azure dla usługi Menedżer StorSimple została włączona. Subskrypcję należy zakupić w ramach umowy [Enterprise Agreement](https://azure.microsoft.com/pricing/enterprise-agreement/).
* Masz dostęp do oprogramowania służącego do emulacji terminala, takiego jak PuTTY.

### <a name="for-the-device-in-the-datacenter"></a>Urządzenie w centrum danych
Przed skonfigurowaniem urządzenia upewnij się, że:

* Urządzenie zostało całkowicie rozpakowane, zamontowane w stojaku i podłączono wszystkie kable umożliwiające dostęp do zasilania, sieci oraz dostęp szeregowy, zgodnie z opisem w części:
  
  * [Rozpakowywanie, montowanie w stojaku i podłączanie kabli do urządzenia 8100](storsimple-8100-hardware-installation.md)
  * [Rozpakowywanie, montowanie w stojaku i podłączanie kabli do urządzenia 8600](storsimple-8600-hardware-installation.md)

### <a name="for-the-network-in-the-datacenter"></a>Sieć w centrum danych
Przed rozpoczęciem upewnij się, że:

* Porty w zaporze centrum danych zostały otwarte w celu zezwolenia na ruch związany z interfejsem iSCSI i chmurą, zgodnie z opisem w temacie [Networking requirements for your StorSimple device](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device) (Wymagania dotyczące pracy w sieci dla urządzenia StorSimple).

## <a name="step-by-step-deployment"></a>Wdrożenie krok po kroku
Poniższe instrukcje krok po kroku dotyczą wdrażania urządzenia StorSimple w centrum danych.

## <a name="step-1-create-a-new-service"></a>Krok 1. Tworzenie nowej usługi
Usługa Menedżer StorSimple umożliwia zarządzanie wieloma urządzeniami StorSimple. Wykonaj poniższe kroki, aby utworzyć nowe wystąpienie usługi Menedżer StorSimple.

[!INCLUDE [storsimple-create-new-service](../../includes/storsimple-create-new-service.md)]

> [!IMPORTANT]
> Jeśli nie włączono automatycznego tworzenia konta magazynu przy użyciu usługi, po pomyślnym utworzeniu usługi musisz utworzyć co najmniej jedno konto magazynu. To konto magazynu będzie używane podczas tworzenia kontenera woluminu.
> 
> * Jeśli nie utworzono automatycznie konta magazynu, przejdź do kroku [Konfigurowanie nowego konta magazynu dla usługi](#configure-a-new-storage-account-for-the-service) w celu uzyskania szczegółowych informacji.
> * Jeśli włączono automatyczne tworzenie konta magazynu, przejdź do części [Krok 2. Pobieranie klucza rejestracji usługi](#step-2-get-the-service-registration-key).
> 
> 

## <a name="step-2-get-the-service-registration-key"></a>Krok 2. Pobieranie klucza rejestracji usługi
Po skonfigurowaniu i uruchomieniu usługi Menedżer StorSimple musisz pobrać klucz rejestracji usługi. Ten klucz służy do rejestrowania urządzenia StorSimple i łączenia go z usługą.

Wykonaj poniższe czynności w klasycznym portalu Azure.

[!INCLUDE [storsimple-get-service-registration-key](../../includes/storsimple-get-service-registration-key.md)]

## <a name="step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple"></a>Krok 3. Konfigurowanie i rejestrowanie urządzenia za pomocą programu Windows PowerShell dla urządzenia StorSimple
Użyj programu Windows PowerShell dla urządzenia StorSimple do przeprowadzenia konfiguracji początkowej urządzenia StorSimple, jak opisano w poniższej procedurze. Do wykonania tego kroku niezbędne jest użycie oprogramowania do emulacji terminala. Aby uzyskać więcej informacji, zobacz część [Łączenie z konsolą szeregową urządzenia przy użyciu programu PuTTY](#use-putty-to-connect-to-the-device-serial-console).

[!INCLUDE [storsimple-configure-and-register-device-u1](../../includes/storsimple-configure-and-register-device-u1.md)]

## <a name="step-4-complete-minimum-device-setup"></a>Krok 4. Przeprowadzenie minimalnej konfiguracji urządzenia
Do przeprowadzenia minimalnej konfiguracji urządzenia StorSimple konieczne jest:

* Skonfigurowanie pomocniczego serwera DNS.
* Włączenie usługi iSCSI na co najmniej jednym interfejsie sieciowym.
* Przypisanie stałych adresów IP do obu kontrolerów.

Aby przeprowadzić minimalną konfigurację urządzenia, wykonaj poniższe kroki w klasycznym portalu Azure.

[!INCLUDE [storsimple-complete-minimum-device-setup](../../includes/storsimple-complete-minimum-device-setup-u1.md)]

## <a name="step-5-create-a-volume-container"></a>Krok 5. Tworzenie kontenera woluminów
Kontener woluminów obejmuje ustawienia konta magazynu, przepustowości i szyfrowania wszystkich woluminów, które zawiera. Kontener woluminów należy utworzyć przed zainicjowaniem woluminów na urządzeniu StorSimple.

Wykonaj poniższe czynności w klasycznym portalu Azure, aby utworzyć kontener woluminów.

[!INCLUDE [storsimple-create-volume-container](../../includes/storsimple-create-volume-container.md)]

## <a name="step-6-create-a-volume"></a>Krok 6. Tworzenie woluminu
Po utworzeniu kontenera woluminów możesz zainicjować obsługę woluminu magazynu w urządzeniu StorSimple dla serwerów. Wykonaj poniższe czynności w klasycznym portalu Azure, aby utworzyć wolumin.

> [!IMPORTANT]
> Menedżer StorSimple może tworzyć tylko woluminy alokowane elastycznie. Nie można tworzyć w pełni lub częściowo inicjowanych woluminów.
> 
> 

[!INCLUDE [storsimple-create-volume](../../includes/storsimple-create-volume.md)]

## <a name="step-7-mount-initialize-and-format-a-volume"></a>Krok 7. Instalowanie, inicjowanie i formatowanie woluminu
Poniższe kroki wykonuje się na hoście systemu Windows Server.

> [!IMPORTANT]
> * W celu zapewnienia dużej dostępności rozwiązania StorSimple warto przed skonfigurowaniem usługi iSCSI skonfigurować wielościeżkowe wejście/wyjście (MPIO) na serwerach hosta (opcjonalnie). Dzięki skonfigurowaniu wielościeżkowego wejścia/wyjścia (MPIO) na serwerach hosta można mieć pewność, że serwery tolerują błędy linków, sieci lub interfejsu.
> * Instrukcje dotyczące instalowania i konfigurowania usługi iSCSI i wielościeżkowego wejścia/wyjścia (MPIO) na hoście systemu Windows Server można znaleźć w temacie [Configure MPIO for your StorSimple device](storsimple-configure-mpio-windows-server.md) (Konfigurowanie wielościeżkowego wejścia/wyjścia dla urządzenia StorSimple). Informacje te dotyczą również kroków opisujących instalowanie, inicjowanie i formatowanie woluminów StorSimple.
> * Instrukcje dotyczące instalowania i konfigurowania usługi iSCSI i wielościeżkowego wejścia/wyjścia (MPIO) na hoście systemu Linux można znaleźć w temacie [Configure MPIO for your StorSimple Linux host](storsimple-configure-mpio-on-linux.md) (Konfigurowanie wielościeżkowego wejścia/wyjścia dla hosta z systemem Linux urządzenia StorSimple).
> 
> 

Jeśli nie chcesz konfigurować wielościeżkowego wejścia/wyjścia (MPIO), wykonaj poniższe kroki, aby zainstalować, zainicjować i sformatować woluminy StorSimple na hoście systemu Windows Server.

[!INCLUDE [storsimple-mount-initialize-format-volume](../../includes/storsimple-mount-initialize-format-volume.md)]

## <a name="step-8-take-a-backup"></a>Krok 8. Tworzenie kopii zapasowej
Kopie zapasowe oferują ochronę woluminów w określonym momencie i udoskonalają możliwości odzyskiwania przy równoczesnym zminimalizowaniu czasów przywracania. W urządzeniu StorSimple można wykonywać dwa typy kopii zapasowych: migawki lokalne i migawki w chmurze. Kopie obu typów można tworzyć jako **zaplanowane** lub **ręczne**.

Wykonaj poniższe czynności w klasycznym portalu Azure, aby utworzyć zaplanowaną kopię zapasową.

[!INCLUDE [storsimple-take-backup](../../includes/storsimple-take-backup.md)]

Kopię zapasową można utworzyć ręcznie w dowolnym momencie. Informacje na temat procedur można znaleźć w części [Ręczne tworzenie kopii zapasowej](#create-a-manual-backup).

## <a name="configure-a-new-storage-account-for-the-service"></a>Konfigurowanie nowego konta magazynu dla usługi
Jest to krok opcjonalny, który należy wykonać tylko, jeśli nie włączono automatycznego tworzenia konta magazynu przy użyciu usługi. Do utworzenia kontenera woluminów StorSimple wymagane jest konto magazynu platformy Microsoft Azure.

Instrukcje krok po kroku dotyczące tworzenia konta usługi Azure Storage w innym regionie można znaleźć w temacie [About Azure Storage Accounts](../storage/common/storage-create-storage-account.md) (Informacje o kontach usługi Azure Storage).

Wykonaj poniższe kroki w klasycznej witrynie Azure Portal na stronie **usługi StorSimple Manager**.

[!INCLUDE [storsimple-configure-new-storage-account-u1](../../includes/storsimple-configure-new-storage-account-u1.md)]

## <a name="use-putty-to-connect-to-the-device-serial-console"></a>Łączenie z konsolą szeregową urządzenia przy użyciu programu PuTTY
Aby nawiązać połączenie z programem Windows PowerShell dla urządzenia StorSimple, należy użyć oprogramowania do emulacji terminala, takiego jak PuTTY. Programu PuTTY można używać w przypadku uzyskiwania dostępu do urządzenia bezpośrednio za pośrednictwem konsoli szeregowej lub przez otwarcie sesji telnet z komputera zdalnego.

[!INCLUDE [Use PuTTY to connect to the device serial console](../../includes/storsimple-use-putty.md)]

## <a name="scan-for-and-apply-updates"></a>Wyszukiwanie aktualizacji i ich stosowanie
Aktualizowanie urządzenia może potrwać kilka godzin. Wykonaj poniższe kroki w celu wyszukania aktualizacji i zastosowania ich na urządzeniu.
<!--can take 1-4 hours-->

<!--If you have a gateway configured on a network interface other than Data 0, you will need to disable Data 2 and Data 3 network interfaces before installing the update. Go to **Devices > Configure** and disable Data 2 and Data 3 interfaces. You should re-enable these interfaces after the device is updated.-->

#### <a name="to-update-your-device"></a>Aby zaktualizować urządzenie
1. Na stronie **Szybki start** urządzenia kliknij pozycję **Urządzenia**. Wybierz urządzenie fizyczne, kliknij pozycję **Obsługa**, a następnie kliknij pozycję **Wyszukaj aktualizacje**.  
2. Zostanie utworzone zadanie wyszukiwania dostępnych aktualizacji. Jeśli aktualizacje są dostępne, pozycja **Wyszukaj aktualizacje** zmieni się na **Zainstaluj aktualizacje**. Kliknij pozycję **Zainstaluj aktualizacje**.
3. Zostanie utworzone zadanie aktualizacji. Aby monitorować stan aktualizacji, przejdź do pozycji **Zadania**.
   
   > [!NOTE]
   > Po uruchomieniu zadania aktualizacji stan zostanie od razu wyświetlony na poziomie 50 procent. Stan zmieni się na 100% dopiero po zakończeniu zadania aktualizacji. Zmiana stanu procesu aktualizacji w czasie rzeczywistym nie jest wyświetlana.
   > 
   > 
4. Po pomyślnym zaktualizowaniu urządzenia włącz interfejsy sieciowe Dane 2 i Dane 3, jeśli zostały wcześniej wyłączone.

<!-- In step 2, you may be requested to disable Data 2 and Data 3 prior to installing the updates. You must disable these network interfaces or the updates may fail.-->

## <a name="get-the-iqn-of-a-windows-server-host"></a>Pobieranie nazwy IQN hosta z systemem Windows Server
Wykonaj poniższe kroki, aby pobrać kwalifikowaną nazwę iSCSI (IQN) hosta z systemem Windows, na którym uruchomiono system Windows Server® 2012.

[!INCLUDE [Create a manual backup](../../includes/storsimple-get-iqn.md)]

## <a name="create-a-manual-backup"></a>Ręczne tworzenie kopii zapasowej
Wykonaj poniższe kroki w klasycznym portalu Azure, aby na żądanie ręcznie utworzyć kopię zapasową pojedynczego woluminu w urządzeniu StorSimple.

[!INCLUDE [Create a manual backup](../../includes/storsimple-create-manual-backup.md)]

## <a name="configure-mpio"></a>Konfigurowanie wielościeżkowego wejścia/wyjścia (MPIO)
Wielościeżkowe wejście/wyjście (MPIO) jest funkcją opcjonalną i nie jest instalowane domyślnie w systemie Windows Server. Powinno być instalowane jako funkcja za pomocą Menedżera serwera. Instrukcje dotyczące instalowania wielościeżkowego wejścia/wyjścia można znaleźć w temacie [Konfigurowanie wielościeżkowego wejścia/wyjścia dla urządzenia StorSimple](storsimple-configure-mpio-windows-server.md).

Instrukcje dotyczące instalowania wielościeżkowego wejścia/wyjścia (MPIO) dotyczące urządzenia StorSimple podłączonego do hosta z systemem Linux można znaleźć w temacie [Konfigurowanie wielościeżkowego wejścia/wyjścia dla hosta z systemem Linux](storsimple-configure-mpio-on-linux.md).

> [!NOTE]
> Wielościeżkowe wejście/wyjście (MPIO) nie jest obsługiwane na urządzeniu wirtualnym StorSimple.
> 
> 

## <a name="next-steps"></a>Następne kroki
* Skonfiguruj [urządzenie wirtualne](storsimple-virtual-device-u2.md).
* Użyj [usługi StorSimple Manager](storsimple-manager-service-administration.md) do zarządzania urządzeniem StorSimple.

