<properties
   pageTitle="Wdrażanie lokalnego urządzenia StorSimple | Microsoft Azure"
   description="W tym artykule opisano kroki i najlepsze rozwiązania dotyczące wdrażania usług i urządzeń StorSimple. (Dotyczy urządzeń Microsoft Azure StorSimple w wersji 3 i starszych)."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/17/2016"
   ms.author="alkohli" />

# Wdrażanie lokalnego urządzenia StorSimple

> [AZURE.SELECTOR]
- [Update 2](../articles/storsimple/storsimple-deployment-walkthrough-u2.md)
- [Update 1](../articles/storsimple/storsimple-deployment-walkthrough-u1.md)
- [Wersja ogólnodostępna](../articles/storsimple/storsimple-deployment-walkthrough.md)

## Omówienie

Witamy w procesie wdrażania urządzenia Microsoft Azure StorSimple. Te samouczki wdrażania dotyczą urządzeń z serii StorSimple 8000, StorSimple 8000 z aktualizacją Update 0.1, StorSimple 8000 z aktualizacją Update 0.2 i StorSimple 8000 z aktualizacją Update 0.3. W tej serii samouczków opisano procedurę konfigurowania urządzenia StorSimple, a także uwzględniono listę kontrolną i wymagania wstępne dotyczące konfiguracji oraz szczegółowe opisy kroków konfiguracji.


Informacje podane w tych samouczkach mają zastosowanie po zapoznaniu się ze środkami ostrożności i dopiero wówczas, gdy urządzenie StorSimple zostało rozpakowane i zamontowane w stojaku oraz podłączono do niego kable. Jeśli nadal trzeba wykonać te zadania, należy rozpocząć od sprawdzenia [środków ostrożności](storsimple-safety.md). W zależności od modelu urządzenia można następnie je rozpakować, zamontować w stojaku i podłączyć kable, wykonując instrukcje podane w następujących tematach:

- [Rozpakowywanie, montowanie w stojaku i podłączanie kabli do urządzenia 8100](storsimple-8100-hardware-installation.md)
- [Rozpakowywanie, montowanie w stojaku i podłączanie kabli do urządzenia 8600](storsimple-8600-hardware-installation.md)

Do ukończenia procesu instalacji i konfiguracji niezbędne są uprawnienia administratora. Przed rozpoczęciem warto przejrzeć listę kontrolną dotyczącą konfiguracji. Proces wdrażania i konfiguracji może potrwać pewien czas.

> [AZURE.NOTE] Informacje na temat wdrażania urządzenia StorSimple opublikowane w witrynie platformy Microsoft Azure w sieci Web dotyczą tylko urządzeń z serii StorSimple 8000. Aby uzyskać pełne informacje dotyczące urządzeń z serii 5000 i 7000, przejdź do strony [http://onlinehelp.storsimple.com/](http://onlinehelp.storsimple.com). Informacje dotyczące wdrażania urządzeń z serii 5000 i 7000 można znaleźć w [przewodniku Szybki start do systemu StorSimple](http://onlinehelp.storsimple.com/111_Appliance/).

## Kroki wdrażania

Wykonaj wymagane kroki, aby skonfigurować urządzenie StorSimple i połączyć je z usługą Menedżer StorSimple. Oprócz kroków wymaganych istnieją również opcjonalne kroki i procedury, które być może trzeba będzie wykonać podczas wdrażania. W szczegółowych instrukcjach dotyczących wdrażania wskazano, w którym momencie należy wykonać poszczególne kroki opcjonalne.


| Krok                                                                                   | Opis                                                                                                                                                   |
|----------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **WYMAGANIA WSTĘPNE**                                                                      | Te czynności należy wykonać w ramach przygotowań do przyszłego wdrożenia.                                                                                        |
| Lista kontrolna dotycząca konfiguracji wdrożenia.                                                     | Ta lista kontrolna umożliwia zbieranie i rejestrowanie informacji przed wdrożeniem i w jego trakcie.                                                                       |
| Wymagania wstępne dotyczące wdrożenia.                                                               | Służą do sprawdzania, czy środowisko jest gotowe do przeprowadzenia wdrożenia.                                                                                                     |
|                                                                                        |                                                                                                                                                               |
| **WDROŻENIE KROK PO KROKU**                                                                   | Te kroki są wymagane do wdrożenia urządzenia StorSimple w środowisku produkcyjnym.                                                                                      |
| Krok 1. Tworzenie nowej usługi.                                                         | Skonfiguruj magazyn i zarządzanie chmurą dla danego urządzenia StorSimple. Jeśli masz istniejącą usługę dla innych urządzeń StorSimple, pomiń ten krok.                |
| Krok 2. Pobieranie klucza rejestracji usługi.                                               | Użyj tego klucza do zarejestrowania urządzenia StorSimple i połączenia go z usługą zarządzania.                                                                         |
| Krok 3. Konfigurowanie i rejestrowanie urządzenia za pomocą programu Windows PowerShell dla StorSimple.    | Połącz urządzenie z siecią i zarejestruj je przy użyciu platformy Azure, aby ukończyć instalację za pomocą usługi zarządzania.                                            |
| Krok 4. Przeprowadzenie minimalnej konfiguracji urządzenia</br>Opcjonalnie: aktualizacja urządzenia StorSimple.      | Skorzystaj z usługi zarządzania, aby skonfigurować urządzenie i umożliwić przechowywanie w nim danych.                                                                      |
| Krok 5. Tworzenie kontenera woluminów.                                                      | Utwórz kontener, aby umożliwić inicjowanie obsługi woluminów. Kontener woluminów obejmuje ustawienia konta magazynu, przepustowości i szyfrowania wszystkich woluminów, które zawiera.    |
| Krok 6. Tworzenie woluminu.                                                                | Zainicjuj obsługę woluminów magazynu na urządzeniu StorSimple na potrzeby serwerów.                                                                                        |
| Krok 7. Instalowanie, inicjowanie i formatowanie woluminu.</br>Opcjonalnie: konfigurowanie wielościeżkowego wejścia/wyjścia (MPIO)            | Połącz serwery z magazynem iSCSI udostępnianym przez urządzenie. Możesz opcjonalnie skonfigurować wielościeżkowe wejście/wyjście, aby upewnić się, że serwery tolerują błędy linków, sieci i interfejsu.                                                                                                                                                              |
| Krok 8. Tworzenie kopii zapasowej.                                                                  | Skonfiguruj zasady tworzenia kopii zapasowej, aby chronić dane.                                                                                                                 |
|                                                                                        |                                                                                                                                                               |
| **INNE PROCEDURY**                                                                   | Te procedury mogą okazać się potrzebne podczas wdrażania rozwiązania.                                                                                        |
| Konfigurowanie nowego konta magazynu dla usługi.                                      |                                                                                                                                                               |
| Łączenie z konsolą szeregową urządzenia przy użyciu programu PuTTY.                                    |                                                                                                                                                               |
| Pobieranie nazwy IQN hosta z systemem Windows Server.                                                   |                                                                                                                                                               |
| Ręczne tworzenie kopii zapasowej.                                                                 |


## Lista kontrolna dotycząca konfiguracji wdrożenia

Na poniższej liście kontrolnej dotyczącej konfiguracji wdrażania określono, jakie informacje należy zebrać przed rozpoczęciem konfigurowania oprogramowania na urządzeniu StorSimple oraz w jego trakcie. Wcześniejsze przygotowanie niektórych z tych informacji usprawni proces wdrażania urządzenia StorSimple w środowisku użytkownika. Ta lista kontrolna umożliwia również notowanie niezbędnych szczegółów konfiguracji podczas wdrażania urządzenia.

| Etap                                  | Parametr                                         | Szczegóły                                                                                                                                                                | Wartości |
|----------------------------------------|---------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------|
| **Podłączanie kabli do urządzenia**                      | Dostęp szeregowy                                     | Początkowa konfiguracja urządzenia                                                                  | Tak/Nie |
|   |   |  |  |
| **Konfigurowanie i rejestrowanie urządzenia**          | Ustawienia sieci interfejsu Dane 0                           | Adres IP interfejsu Dane 0:</br>Maska podsieci:</br>Brama:</br>Podstawowy serwer DNS:</br>Podstawowy serwer NTP:</br>Adres IP/nazwa FQDN serwera proxy sieci Web (opcjonalnie):</br>Port serwera proxy sieci Web:|        |
|                                        | Hasło administratora urządzenia                     | Hasło musi zawierać od 8 do 15 znaków, w tym małe litery, wielkie litery, cyfry i znaki specjalne. |        |
|                                        | Hasło programu StorSimple Snapshot Manager              | Hasło musi zawierać od 14 do 15 znaków, w tym małe litery, wielkie litery, cyfry i znaki specjalne.|        |
|                                        | Klucz rejestracji usługi                          | Ten klucz jest generowany przy użyciu klasycznego portalu Azure.    |        |
|                                        | Klucz szyfrowania danych usługi                       | Ten klucz jest tworzony podczas rejestrowania urządzenia przy użyciu usługi zarządzania w programie Windows PowerShell dla urządzenia StorSimple. Skopiuj ten klucz i zapisz go w bezpiecznym miejscu.|  |
|   |   |  |  |
| **Przeprowadzenie minimalnej konfiguracji urządzenia**          | Przyjazna nazwa urządzenia                     | Jest to opisowa nazwa urządzenia. |        |
|                                        | Strefa czasowa                                          | Wszystkie zaplanowane operacje urządzenia będą wykonywane w ramach tej strefy czasowej.  |        |
|                                        | Pomocniczy serwer DNS                              | To jest wymagana konfiguracja.                                  |        |
|                                        | Interfejs sieciowy: stałe adresy IP kontrolera Dane 0                                     | Te adresy IP powinny obsługiwać routing do Internetu.</br>Stały adres IP kontrolera 0:</br>Stały adres IP kontrolera 1:|
|   |   |  |  |
| **Dodatkowe ustawienia interfejsu sieciowego**  | Interfejs sieciowy: Dane 1</br>Jeśli włączono interfejs iSCSI, nie konfiguruj bramy.      | Cel: chmura/iSCSI/nie jest używany</br>Adres IP:</br>Maska podsieci:</br>Brama:|
|                                        | Interfejs sieciowy: Dane 2</br>Jeśli włączono interfejs iSCSI, nie konfiguruj bramy.      | Cel: chmura/iSCSI/nie jest używany</br>Adres IP:</br>Maska podsieci:</br>Brama:|
|                                        | Interfejs sieciowy: Dane 3</br>Jeśli włączono interfejs iSCSI, nie konfiguruj bramy.      | Cel: chmura/iSCSI/nie jest używany</br>Adres IP:</br>Maska podsieci:</br>Brama:|
|                                        | Interfejs sieciowy: Dane 4</br>Jeśli włączono interfejs iSCSI, nie konfiguruj bramy.      | Cel: chmura/iSCSI/nie jest używany</br>Adres IP:</br>Maska podsieci:</br>Brama:|
|                                        | Interfejs sieciowy: Dane 5</br>Jeśli włączono interfejs iSCSI, nie konfiguruj bramy.      | Cel: chmura/iSCSI/nie jest używany</br>Adres IP:</br>Maska podsieci:</br>Brama:|
|   |   |  |  |
| **Tworzenie kontenera woluminów**                      | Nazwa kontenera woluminów:                            | Nazwa kontenera                                                                                                                                                 |        |
|                                        | Konto magazynu Azure:                            | Klucz dostępu i nazwa konta magazynu do skojarzenia z tym kontenerem woluminu                                                                                              |        |
|                                        | Klucz szyfrowania magazynu w chmurze:                     | Klucz szyfrowania magazynu w poszczególnych kontenerach                                                                                                                           |        |
|   |   |  |  |
| **Tworzenie woluminu**                        | Szczegóły poszczególnych woluminów                           | Nazwa woluminu:                                                                                                                                                           |        |
|                                        |                                                   | Rozmiar:                                                                                                                                                                  |        |
|                                        |                                                   | Typ użycia:                                                                                                                                                            |        |
|                                        |                                                   | Nazwa ACR:                                                                                                                                                              |        |
|                                        |                                                   | Domyślne zasady tworzenia kopii zapasowej:                                                                                                                                                 |        |
|   |   |  |  |
| **Instalowanie, inicjowanie i formatowanie woluminu** | Szczegóły poszczególnych serwerów hosta łączących się z magazynem | Nazwa serwera Windows Server:                                                                                                                                                   |        |
|                                        |                                                   | Nazwa IQN serwera Windows Server:                                                                                                                                                    |        |
|                                        |                                                   | Nazwa woluminu z systemem Windows Server:                                                                                                                                                   |        |
|                                        |                                                   | Litera dysku/punkt instalacji systemu plików NTFS:                                                                                                                                      |        |

## Wymagania wstępne dotyczące wdrożenia

Poniższe sekcje zawierają opis wymagań wstępnych dotyczących konfiguracji usługi Menedżer StorSimple, urządzenia StorSimple i sieci w centrum danych.

### Usługa Menedżer StorSimple

Przed rozpoczęciem upewnij się, że:

- Masz konto Microsoft z poświadczeniami dostępu.

- Masz konto magazynu platformy Microsoft Azure z poświadczeniami dostępu.

- Subskrypcja platformy Microsoft Azure dla usługi Menedżer StorSimple została włączona. Subskrypcję należy zakupić w ramach umowy [Enterprise Agreement](https://azure.microsoft.com/pricing/enterprise-agreement/).

- Masz dostęp do oprogramowania służącego do emulacji terminala, takiego jak PuTTY.

### Urządzenie w centrum danych

Przed skonfigurowaniem urządzenia upewnij się, że:

- Urządzenie zostało całkowicie rozpakowane, zamontowane w stojaku i podłączono wszystkie kable umożliwiające dostęp do zasilania, sieci oraz dostęp szeregowy, zgodnie z opisem w części:

    -  [Rozpakowywanie, montowanie w stojaku i podłączanie kabli do urządzenia 8100](storsimple-8100-hardware-installation.md)
    -  [Rozpakowywanie, montowanie w stojaku i podłączanie kabli do urządzenia 8600](storsimple-8600-hardware-installation.md)


### Sieć w centrum danych

Przed rozpoczęciem upewnij się, że:

- Porty w zaporze centrum danych zostały otwarte w celu zezwolenia na ruch związany z interfejsem iSCSI i chmurą, zgodnie z opisem w temacie [Networking requirements for your StorSimple device](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device) (Wymagania dotyczące pracy w sieci dla urządzenia StorSimple).
- Urządzenie w centrum danych może nawiązać połączenie z siecią zewnętrzną. Uruchom następujące polecenia cmdlet programu [Windows PowerShell 4.0](http://www.microsoft.com/download/details.aspx?id=40855) (przedstawione w poniższej tabeli), aby zweryfikować połączenie z siecią zewnętrzną. Tę weryfikację należy przeprowadzić na komputerze (w sieci centrum danych), który ma łączność z platformą Azure i na którym zostanie wdrożone urządzenie StorSimple.  

| W przypadku tego parametru...       | Aby sprawdzić poprawność...                                                                                                                                                                                | Uruchom te polecenia/polecenia cmdlet                                                                                                                                                             |
|---------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Adres IP**</br>**Podsieć**</br>**Brama** | Czy to jest prawidłowy adres IPv4 lub IPv6?</br>Czy to jest prawidłowa podsieć?</br>Czy to jest prawidłowa brama?</br>Czy to jest powielony adres IP w sieci?                                                                          | `ping ip`</br>`arp -a`</br>Powinien wystąpić błąd poleceń `ping` i `arp` wskazujący, że w sieci centrum danych nie ma urządzeń używających tego adresu IP.
|                           |                                                                                                                                                                    |                                                                                                                                                                                         |
| **DNS**                       | Czy to jest prawidłowy serwer DNS i czy może rozpoznawać adresy URL platformy Azure?                                                                                                                    | `Resolve-DnsName -Name www.bing.com -Server <DNS server IP address>` </br>Można też użyć następującego polecenia:</br>`nslookup --dns-ip=<DNS server IP address> www.bing.com`      |
|                           | Sprawdź, czy port 53 został otwarty. Ma to zastosowanie tylko wtedy, gdy używasz zewnętrznego serwera DNS dla tego urządzenia. Wewnętrzny serwer DNS powinien automatycznie rozpoznawać zewnętrzne adresy URL.  | `Test-Port -comp dc1 -port 53 -udp -UDPtimeout 10000`  </br>[Więcej informacji na temat tego polecenia cmdlet](http://learn-powershell.net/2011/02/21/querying-udp-ports-with-powershell/)|
|                           |                                                                                                                                                                    |                                                                                                                                                                                         |
| **NTP**                       | Synchronizacja czasu jest wyzwalana po wprowadzeniu danych serwera NTP. Sprawdź, czy port 123 protokołu UDP został otwarty podczas wprowadzania danych `time.windows.com` lub publicznych serwerów czasu. | [Pobierz ten skrypt i korzystaj z niego](https://gallery.technet.microsoft.com/scriptcenter/Get-Network-NTP-Time-with-07b216ca).                                                                                                                                                           |
|                           |                                                                                                                                                                    |                                                                                                                                                                                         |
| **Serwer proxy (opcjonalnie)**          | Czy to jest prawidłowy port i identyfikator URI serwera proxy? </br> Czy tryb uwierzytelniania jest prawidłowy?                                                                                                                                | <code>wget http://bing.com &#124; % {$_.StatusCode}</code></br>To polecenie należy uruchomić natychmiast po skonfigurowaniu serwera proxy sieci Web. Zwrócenie kodu stanu 200 wskazuje, że połączenie zostało nawiązane.                                                                                                                                 |
|                           | Czy można przesyłać ruch przez serwer proxy?                                                                                                                                 | Po skonfigurowaniu serwera proxy na urządzeniu uruchom raz weryfikację systemu DNS, sprawdzenie protokołu NTP lub sprawdzenie protokołu HTTP. Dzięki temu zyskasz jasny obraz ewentualnych blokad ruchu na serwerze proxy lub w innym miejscu.                                                                                                                      |
|                           |                                                                                                                                                                    |                                                                                                                                                                                         |
| **Rejestracja**              | Sprawdź, czy porty TCP ruchu wychodzącego 443, 80 i 9354 zostały otwarte.                                                                                                                |  `Test-NetConnection -Port   443 -InformationLevel Detailed`</br>[Więcej informacji na temat polecenia cmdlet Test-NetConnection](https://technet.microsoft.com/library/dn372891.aspx)                                                                           |

## Wdrożenie krok po kroku

Poniższe instrukcje krok po kroku dotyczą wdrażania urządzenia StorSimple w centrum danych.

## Krok 1. Tworzenie nowej usługi

Usługa Menedżer StorSimple umożliwia zarządzanie wieloma urządzeniami StorSimple. W przypadku wdrażania pierwszego urządzenia StorSimple musisz utworzyć nową usługę Menedżer StorSimple.

> [AZURE.IMPORTANT] Pomiń ten krok, jeśli masz już usługę Menedżer StorSimple i zamierzasz wdrożyć urządzenie StorSimple przy użyciu tej usługi.

Wykonaj poniższe kroki, aby utworzyć nowe wystąpienie usługi Menedżer StorSimple.

[AZURE.INCLUDE [storsimple-create-new-service](../../includes/storsimple-create-new-service.md)]

> [AZURE.IMPORTANT] Jeśli nie włączono automatycznego tworzenia konta magazynu przy użyciu usługi, po pomyślnym utworzeniu usługi musisz utworzyć co najmniej jedno konto magazynu. To konto magazynu będzie używane podczas tworzenia kontenera woluminu.
>
> Jeśli nie utworzono automatycznie konta magazynu, przejdź do kroku [Konfigurowanie nowego konta magazynu dla usługi](#configure-a-new-storage-account-for-the-service) w celu uzyskania szczegółowych informacji.
> Jeśli włączono automatyczne tworzenie konta magazynu, przejdź do części [Krok 2. Pobieranie klucza rejestracji usługi](#step-2:-get-the-service-registration-key).

## Krok 2. Pobieranie klucza rejestracji usługi

Po skonfigurowaniu i uruchomieniu usługi Menedżer StorSimple musisz pobrać klucz rejestracji usługi. Ten klucz służy do rejestrowania urządzenia StorSimple i łączenia go z usługą.

Wykonaj poniższe czynności w klasycznym portalu Azure.

[AZURE.INCLUDE [storsimple-get-service-registration-key](../../includes/storsimple-get-service-registration-key.md)]


## Krok 3. Konfigurowanie i rejestrowanie urządzenia za pomocą programu Windows PowerShell dla urządzenia StorSimple

> [AZURE.IMPORTANT] Przed wykonaniem tej konfiguracji odłącz wszystkie interfejsy sieciowe inne niż Dane 0 na kontrolerach aktywnych i pasywnych.

Użyj programu Windows PowerShell dla urządzenia StorSimple do przeprowadzenia konfiguracji początkowej urządzenia StorSimple, jak opisano w poniższej procedurze. Do wykonania tego kroku niezbędne jest użycie oprogramowania do emulacji terminala. Aby uzyskać więcej informacji, zobacz część [Łączenie z konsolą szeregową urządzenia przy użyciu programu PuTTY](#use-putty-to-connect-to-the-device-serial-console).

[AZURE.INCLUDE [storsimple-configure-and-register-device](../../includes/storsimple-configure-and-register-device.md)]

## Krok 4. Przeprowadzenie minimalnej konfiguracji urządzenia

Do przeprowadzenia minimalnej konfiguracji urządzenia StorSimple konieczne jest:

- Skonfigurowanie pomocniczego serwera DNS.
- Włączenie usługi iSCSI na co najmniej jednym interfejsie sieciowym.
- Przypisanie stałych adresów IP do obu kontrolerów.

Aby przeprowadzić minimalną konfigurację urządzenia, wykonaj poniższe kroki w klasycznym portalu Azure.

[AZURE.INCLUDE [storsimple-complete-minimum-device-setup](../../includes/storsimple-complete-minimum-device-setup.md)]

Po zakończeniu konfigurowania urządzenia wyszukaj aktualizacje i jeśli są dostępne, zainstaluj je. Zastosowanie aktualizacji może potrwać kilka godzin. Postępuj zgodnie z instrukcjami w części [Wyszukiwanie i stosowanie aktualizacji](#scan-for-and-apply-updates).


## Krok 5. Tworzenie kontenera woluminów

Kontener woluminów obejmuje ustawienia konta magazynu, przepustowości i szyfrowania wszystkich woluminów, które zawiera. Kontener woluminów należy utworzyć przed zainicjowaniem woluminów na urządzeniu StorSimple.

Wykonaj poniższe czynności w klasycznym portalu Azure, aby utworzyć kontener woluminów.

[AZURE.INCLUDE [storsimple-create-volume-container](../../includes/storsimple-create-volume-container.md)]

## Krok 6. Tworzenie woluminu

Po utworzeniu kontenera woluminów możesz zainicjować obsługę woluminu magazynu w urządzeniu StorSimple dla serwerów. Wykonaj poniższe czynności w klasycznym portalu Azure, aby utworzyć wolumin.

> [AZURE.IMPORTANT] Menedżer StorSimple może tworzyć tylko woluminy alokowane elastycznie.  Nie można tworzyć woluminów alokowanych w pełni ani częściowo.

[AZURE.INCLUDE [storsimple-create-volume](../../includes/storsimple-create-volume.md)]

## Krok 7. Instalowanie, inicjowanie i formatowanie woluminu

> [AZURE.IMPORTANT]

> - Aby korzystać z wysokiej dostępności rozwiązania StorSimple, warto skonfigurować wielościeżkowe wejście/wyjście (MPIO) na hoście systemu Windows Server (opcjonalnie) przed skonfigurowaniem usługi iSCSI na hoście systemu Windows Server. Dzięki skonfigurowaniu wielościeżkowego wejścia/wyjścia (MPIO) na serwerach hosta można mieć pewność, że serwery tolerują błędy linków, sieci lub interfejsu.

> - Instrukcje dotyczące instalowania i konfigurowania usługi iSCSI oraz wielościeżkowego wejścia/wyjścia można znaleźć w części [Konfigurowanie wielościeżkowego wejścia/wyjścia dla urządzenia StorSimple](storsimple-configure-mpio-windows-server.md). Informacje te dotyczą również kroków opisujących instalowanie, inicjowanie i formatowanie woluminów StorSimple.

Jeśli nie chcesz konfigurować wielościeżkowego wejścia/wyjścia, wykonaj poniższe kroki, aby zainstalować, zainicjować i sformatować woluminy StorSimple.

[AZURE.INCLUDE [storsimple-mount-initialize-format-volume](../../includes/storsimple-mount-initialize-format-volume.md)]

## Krok 8. Tworzenie kopii zapasowej

Kopie zapasowe oferują ochronę woluminów w określonym momencie i udoskonalają możliwości odzyskiwania przy równoczesnym zminimalizowaniu czasów przywracania. W urządzeniu StorSimple można wykonywać dwa typy kopii zapasowych: migawki lokalne i migawki w chmurze. Kopie obu typów można tworzyć jako **zaplanowane** lub **ręczne**.

Wykonaj poniższe czynności w klasycznym portalu Azure, aby utworzyć zaplanowaną kopię zapasową.

[AZURE.INCLUDE [storsimple-take-backup](../../includes/storsimple-take-backup.md)]

Kopię zapasową można utworzyć ręcznie w dowolnym momencie. Informacje na temat procedur można znaleźć w części [Ręczne tworzenie kopii zapasowej](#Create-a-manual-backup).

## Konfigurowanie nowego konta magazynu dla usługi

Jest to krok opcjonalny, który należy wykonać tylko, jeśli nie włączono automatycznego tworzenia konta magazynu przy użyciu usługi. Do utworzenia kontenera woluminów StorSimple wymagane jest konto magazynu platformy Microsoft Azure.

Instrukcje krok po kroku dotyczące tworzenia konta usługi Azure Storage w innym regionie można znaleźć w temacie [About Azure Storage Accounts](../storage/storage-create-storage-account.md) (Informacje o kontach usługi Azure Storage).

Wykonaj poniższe kroki w klasycznej witrynie Azure Portal na stronie **usługi StorSimple Manager**.

[AZURE.INCLUDE [storsimple-configure-new-storage-account](../../includes/storsimple-configure-new-storage-account.md)]


## Łączenie z konsolą szeregową urządzenia przy użyciu programu PuTTY

Aby nawiązać połączenie z programem Windows PowerShell dla urządzenia StorSimple, należy użyć oprogramowania do emulacji terminala, takiego jak PuTTY. Programu PuTTY można używać w przypadku uzyskiwania dostępu do urządzenia bezpośrednio za pośrednictwem konsoli szeregowej lub przez otwarcie sesji telnet z komputera zdalnego.

[AZURE.INCLUDE [Use PuTTY to connect to the device serial console](../../includes/storsimple-use-putty.md)]

## Wyszukiwanie aktualizacji i ich stosowanie

Aktualizowanie urządzenia może potrwać od 1 do 4 godzin. Wykonaj poniższe kroki w celu wyszukania aktualizacji i zastosowania ich na urządzeniu.

> [AZURE.NOTE] Jeśli na interfejsie sieciowym innym niż Dane 0 skonfigurowano bramę, przed zainstalowaniem aktualizacji musisz wyłączyć interfejsy sieciowe Dane 2 i Dane 3. Przejdź do pozycji **Urządzenia > Konfiguruj** i wyłącz interfejsy Dane 2 i Dane 3. Te interfejsy powinno się ponownie włączyć po zaktualizowaniu urządzenia.

#### Aby zaktualizować urządzenie
1.  Na stronie **Szybki start** urządzenia kliknij pozycję **Urządzenia**. Wybierz urządzenie fizyczne, kliknij pozycję **Obsługa**, a następnie kliknij pozycję **Wyszukaj aktualizacje**.  
2.  Zostanie utworzone zadanie wyszukiwania dostępnych aktualizacji. Jeśli aktualizacje są dostępne, pozycja **Wyszukaj aktualizacje** zmieni się na **Zainstaluj aktualizacje**. Kliknij pozycję **Zainstaluj aktualizacje**. Przed zainstalowaniem aktualizacji może zostać wyświetlony monit o wyłączenie interfejsów Dane 2 i Dane 3. Należy wyłączyć te interfejsy sieciowe. W przeciwnym razie aktualizacje mogą się nie powieść.
3.  Zostanie utworzone zadanie aktualizacji. Aby monitorować stan aktualizacji, przejdź do pozycji **Zadania**.

    > [AZURE.NOTE] Po uruchomieniu zadania aktualizacji stan zostanie od razu wyświetlony na poziomie 50 procent. Następnie stan zmieni się na 100 procent dopiero po zakończeniu zadania aktualizacji. Zmiana stanu procesu aktualizacji w czasie rzeczywistym nie jest wyświetlana.

4.  Po pomyślnym zaktualizowaniu urządzenia włącz interfejsy sieciowe Dane 2 i Dane 3, jeśli zostały wcześniej wyłączone.



## Pobieranie nazwy IQN hosta z systemem Windows Server

Wykonaj poniższe kroki, aby pobrać kwalifikowaną nazwę iSCSI (IQN) hosta z systemem Windows, na którym uruchomiono system Windows Server 2012.

[AZURE.INCLUDE [Create a manual backup](../../includes/storsimple-get-iqn.md)]


## Ręczne tworzenie kopii zapasowej

Wykonaj poniższe kroki w klasycznym portalu Azure, aby na żądanie ręcznie utworzyć kopię zapasową pojedynczego woluminu w urządzeniu StorSimple.

[AZURE.INCLUDE [Create a manual backup](../../includes/storsimple-create-manual-backup.md)]


## Następne kroki

- Skonfiguruj [urządzenie wirtualne](storsimple-virtual-device-u2.md).

- Użyj [usługi StorSimple Manager](https://msdn.microsoft.com/library/azure/dn772396.aspx) do zarządzania urządzeniem StorSimple.



<!--HONumber=sep16_HO1-->


