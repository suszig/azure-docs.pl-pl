---
title: "Konfigurowanie monitora wydajności sieci dla obwody usługi ExpressRoute Azure (wersja zapoznawcza) | Dokumentacja firmy Microsoft"
description: "Skonfiguruj NPM obwody usługi Azure ExpressRoute. (Wersja zapoznawcza)"
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/01/2017
ms.author: cherylmc
ms.openlocfilehash: 35dd3c6be2fb2fa5ec4d14eefce1c16005210364
ms.sourcegitcommit: dcf5f175454a5a6a26965482965ae1f2bf6dca0a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="configure-network-performance-monitor-for-expressroute-preview"></a>Konfigurowanie monitora wydajności sieci dla usługi ExpressRoute (wersja zapoznawcza)

Monitor wydajności sieci (NPM) jest sieci opartej na chmurze rozwiązanie monitorujące, które monitoruje łączność między wdrożenia chmury Azure i lokalizacje lokalnymi (biurach oddziałów, itp.). NPM wchodzi w skład Microsoft Operations Management Suite (OMS). NPM oferuje rozszerzenie dla usługi ExpressRoute, który umożliwia monitorowanie wydajności sieci za pośrednictwem obwody usługi ExpressRoute, które są skonfigurowane do używania prywatnej komunikacji równorzędnej. Po skonfigurowaniu programu NPM dla usługi ExpressRoute, można wykrywać problemy z siecią do identyfikowania i eliminowania.

Możesz:

* Monitorowanie utraty i opóźnienia między różnymi sieciami wirtualnymi i Ustaw alerty

* Monitorowanie wszystkich ścieżek (w tym nadmiarowe ścieżki) w sieci

* Rozwiązywanie problemów przejściowych i w momencie sieci, które są trudne do replikacji

* Pomagają w ustaleniu określonego segmentu sieci, która jest odpowiedzialna za pogorszenie wydajności

* Pobierz przepływności dla sieci wirtualnej (Jeśli masz agentów zainstalowanych w każdej sieci wirtualnej)

* Wyświetlać stan systemu ExpressRoute z poprzedniego punktu w czasie

**Jak to działa?**

Monitorowania agenci są zainstalowani na wielu serwerach, zarówno lokalnie i na platformie Azure. Agenci komunikują się ze sobą, ale nie wysyłaj danych, wysyłają pakiety uzgadnianie protokołu TCP. Komunikacja między agentami umożliwia platformie Azure mapy topologii sieci i ścieżkę, którą może zająć ruchu.

**Przepływ pracy**

1. Utwórz obszar roboczy NPM regionu zachodnie centralnej NAS. Obecnie jest tylko regionu, w którym ta wersja zapoznawcza jest obsługiwana.
2. Instalowanie i konfigurowanie agentów oprogramowania: 
    * Agenci monitorowania należy zainstalować na serwerach lokalnych i maszyn wirtualnych platformy Azure.
    * Skonfiguruj ustawienia na serwerze agenta monitorowania umożliwia agenci monitorowania do komunikacji. (Otwórz porty zapory itp.)
3. Konfigurowanie reguł grupa zabezpieczeń sieci umożliwia agenta monitorowania zainstalowanych na maszynach wirtualnych Azure do komunikowania się z lokalnymi monitorowanie agentów.
4. Żądanie do listy dozwolonych obszaru roboczego programu NPM
5. Konfigurowanie monitorowania: Wykryj automatycznie i zarządzanie sieciami, które są widoczne w NPM.

Jeśli korzystasz już z Monitora wydajności sieci do monitorowania innych obiektów lub usługi, a masz już obszar roboczy w zachodnie środkowe stany, można pominąć krok 1 i 2 i rozpocząć konfigurację z kroku 3.

## <a name="configure"></a>Krok 1: Tworzenie obszaru roboczego

1. W [portalu Azure](https://portal.azure.com), wyszukaj na liście usług w **Marketplace** "Monitor wydajności sieci". Powrotu, kliknij, aby otworzyć **monitora wydajności sieci** strony.

  ![portal](.\media\how-to-npm\3.png)<br><br>
2. W dolnej części głównym **monitora wydajności sieci** kliknij przycisk **Utwórz** otworzyć **sieci monitora wydajności — Utwórz nowe rozwiązanie** strony. Kliknij przycisk **obszarem roboczym pakietu OMS — wybierz obszar roboczy** aby otworzyć stronę obszarów roboczych. Kliknij przycisk **+ Utwórz nowy obszar roboczy** aby otworzyć stronę obszaru roboczego.
3. Na **obszarem roboczym pakietu OMS** wybierz pozycję **Utwórz nowy** i skonfiguruj następujące ustawienia:

  * Obszar roboczy OMS - wpisanie nazwy obszaru roboczego.
  * Subskrypcja — Jeśli masz wiele subskrypcji, wybierz jedną, które chcesz skojarzyć z nowego obszaru roboczego.
  * Grupa zasobów — Utwórz grupę zasobów lub użyć istniejącego.
  * Lokalizacja — należy wybrać zachodnie centralnej nam dla tej wersji zapoznawczej
  * Warstwy cenowej — wybierz bezpłatna

  ![Obszar roboczy](.\media\how-to-npm\4.png)<br><br>
4. Kliknij przycisk **OK** Aby zapisać i wdrożyć ustawienia szablonu. Po weryfikuje szablonu, kliknij przycisk **Utwórz** do wdrożenia w obszarze roboczym.
5. Po wdrożeniu obszaru roboczego, przejdź do **NetworkMonitoring(name)** utworzony zasób. Sprawdź poprawność ustawień, a następnie kliknij przycisk **rozwiązanie wymaga dodatkowej konfiguracji**.

  ![dodatkowa konfiguracja](.\media\how-to-npm\5.png)
6. Na **sieci monitora wydajności — Zapraszamy** wybierz pozycję **TCP używany dla transakcji syntetycznych**, następnie kliknij przycisk **przesyłania**. Transakcje TCP są używane tylko do należy przerwać połączenie. Żadne dane nie są wysyłane za pośrednictwem tych połączeń TCP.

  ![TCP dla transakcji syntetycznych](.\media\how-to-npm\6.png)

## <a name="agents"></a>Krok 2: Instalowanie i konfigurowanie agentów

### <a name="download"></a>2.1: Pobierz plik Instalatora agenta

1. Na **konfiguracji monitora wydajności sieci — strona instalacji TCP** dla zasobu, w **zainstalować agentów OMS** kliknij agenta, który odpowiada procesora i pobierania serwera plik Instalatora.

  >[!NOTE]
  >Agent systemu Linux nie jest obecnie obsługiwany w przypadku połączeń ExpressRoute monitorowania.
  >
  >
2. Następnie skopiuj **identyfikator obszaru roboczego** i **klucz podstawowy** do Notatnika.
3. W **Konfigurowanie agentów** sekcji, Pobierz skrypt programu Powershell. Skrypt programu PowerShell ułatwia otworzyć port zapory odpowiednie dla transakcji protokołu TCP.

  ![Skrypt programu PowerShell](.\media\how-to-npm\7.png)

### <a name="installagent"></a>2.2: Zainstaluj agenta monitorowania na każdym serwerze monitorowania

1. Uruchom **Instalator** do zainstalowania agenta na każdym serwerze, który ma być używany do monitorowania usługi ExpressRoute. Używanego do monitorowania serwera może być maszyny Wirtualnej lub lokalnymi i musi mieć dostęp do Internetu. Należy zainstalować co najmniej jeden lokalny agent i jednego agenta w każdym segmencie sieci, które mają być monitorowane na platformie Azure.
2. Na **powitalnej** kliknij przycisk **dalej**.
3. Na **postanowień licencyjnych** odczytu licencji, a następnie kliknij przycisk **zgadzam się**.
4. Na **Folder docelowy** , zmienić lub zachować domyślny folder instalacji, a następnie kliknij przycisk **dalej**.
5. Na **opcje instalacji agenta** strony, użytkownik może Połącz agenta z Analiza dzienników Azure (OMS) lub programu Operations Manager. Lub puste opcji, jeśli chcesz później skonfigurować agenta. Po wprowadzeniu selection(s) Twojego, kliknij przycisk **dalej**.

  * Jeśli chcesz połączyć się z **Analiza dzienników Azure (OMS)**, Wklej **identyfikator obszaru roboczego** i **klucz obszaru roboczego** (klucz podstawowy) skopiowane do Notatnika w poprzedniej sekcji. Następnie kliknij przycisk **Dalej**.

    ![Identyfikator i klucz](.\media\how-to-npm\8.png)
  * Jeśli chcesz połączyć się z **programu Operations Manager**na **Konfiguracja grupy zarządzania** wpisz **Nazwa grupy zarządzania**, **serwera zarządzania** i **Port serwera zarządzania**. Następnie kliknij przycisk **Dalej**.

    ![Operations Manager](.\media\how-to-npm\9.png)
  * Na **konto akcji agenta** wybierz **systemu lokalnego** konta, lub **domeny lub lokalnego konta komputera**. Następnie kliknij przycisk **Dalej**.

    ![Konto](.\media\how-to-npm\10.png)
6. Na **gotowy do instalacji** , przejrzyj wybrane opcje, a następnie kliknij przycisk **zainstalować**.
7. Na **konfiguracji została ukończona pomyślnie** kliknij przycisk **Zakończ**.
8. Po zakończeniu programu Microsoft Monitoring Agent pojawi się w Panelu sterowania. Można przejrzeć konfigurację istnieje i sprawdź, czy agent jest podłączony do Operational Insights (OMS). Podczas połączenia z usługą OMS, agent wyświetla komunikat z informacją: **programu Microsoft Monitoring Agent pomyślnie połączył się z usługą Microsoft Operations Management Suite**.

### <a name="proxy"></a>2.3: Konfigurowanie ustawień serwera proxy (opcjonalnie)

Jeśli używasz serwera proxy sieci web do uzyskania dostępu do Internetu, następujące kroki umożliwiają konfigurowanie ustawień serwera proxy dla programu Microsoft Monitoring Agent. Wykonaj te kroki dla każdego serwera. Jeśli masz wiele serwerów, które trzeba skonfigurować, łatwiejszym rozwiązaniem może być użycie skryptu automatyzującego ten proces. Jeśli tak, zobacz [skonfigurować ustawienia serwera proxy dla programu Microsoft Monitoring Agent za pomocą skryptu](../log-analytics/log-analytics-windows-agents.md#to-configure-proxy-settings-for-the-microsoft-monitoring-agent-using-a-script).

Aby skonfigurować ustawienia serwera proxy dla programu Microsoft Monitoring Agent w Panelu sterowania:

1. Otwórz **Panel sterowania**.
2. Otwórz program **Microsoft Monitoring Agent**.
3. Kliknij kartę **Ustawienia serwera proxy**.
4. Wybierz **Użyj serwera proxy** i wpisz adres URL i numer portu, jeśli jest ono wymagane. Jeśli Twój serwer proxy wymaga uwierzytelniania, wpisz nazwę użytkownika i hasło, aby uzyskać dostęp do serwera proxy.

  ![Serwer proxy](.\media\how-to-npm\11.png)

### <a name="verifyagent"></a>2.4: Sprawdź łączność agenta

Można łatwo sprawdzić, czy komunikują się agentów.

1. Na serwerze z agenta monitorowania, należy otworzyć **Panelu sterowania**.
2. Otwórz **programu Microsoft Monitoring Agent**.
3. Kliknij przycisk **Analiza dzienników Azure (OMS)** kartę.
4. W **stan** kolumny, powinny być widoczne czy agent pomyślnie połączony z usługą Operations Management Suite.

  ![status](.\media\how-to-npm\12.png)

### <a name="firewall"></a>2.5: otworzyć porty zapory na serwerze agenta monitorowania

Aby korzystać z protokołu TCP, należy otworzyć porty zapory, aby upewnić się, że agenci monitorowania może komunikować się.

Można uruchomić skrypt programu PowerShell, który tworzy kluczy rejestru wymaganych przez Monitor wydajności sieci, a także tworzenie reguł zapory systemu Windows w celu umożliwienia monitorowania agentów do utworzenia połączenia TCP ze sobą. Klucze rejestru utworzony przez skrypt Określ również, czy do rejestrowania dzienników debugowania i ścieżkę do plików dzienników. Definiuje również port TCP agent używany do komunikacji. Wartości te klucze są automatycznie ustawiane za pomocą skryptu, dlatego nie należy ręcznie zmienić te klucze.

Port 8084 jest domyślnie otwierany. Można użyć niestandardowego numeru portu, podając parametr "numer_portu" do skryptu. Jednak zrobić, należy określić ten sam port dla wszystkich serwerów, na których uruchomiono skrypt.

>[!NOTE]
>Skrypt programu PowerShell "EnableRules" umożliwia skonfigurowanie reguł zapory systemu Windows tylko na serwerze, na którym skrypt jest uruchamiany. Jeśli masz zapory sieciowej, należy upewnić się, że umożliwia ruch kierowany do portu TCP używany przez Monitor wydajności sieci.
>
>

Na serwerach agenta Otwórz okno programu PowerShell z uprawnieniami administracyjnymi. Uruchom [EnableRules](https://gallery.technet.microsoft.com/OMS-Network-Performance-04a66634) skrypt programu PowerShell, (który został wcześniej pobrany). Nie należy używać żadnych parametrów.

  ![PowerShell_Script](.\media\how-to-npm\script.png)

## <a name="opennsg"></a>Krok 3: Konfigurowanie zasad grupy zabezpieczeń sieci

Do monitorowania serwerów agenta, które są na platformie Azure, należy skonfigurować reguły grupa zabezpieczeń sieci zezwalająca na ruch TCP na porcie używany przez NPM dla transakcji syntetycznych. Domyślny port to 8084. Dzięki temu agent monitorowania zainstalowany na maszynie Wirtualnej platformy Azure do komunikowania się z lokalnymi agenta monitorowania.

Aby uzyskać więcej informacji na temat grupy NSG, zobacz [grup zabezpieczeń sieci](../virtual-network/virtual-networks-create-nsg-arm-portal.md).

## <a name="whitelist"></a>Krok 4: Żądanie do listy dozwolonych obszaru roboczego

>[!NOTE]
>Upewnij się, czy po zainstalowaniu agentów (agent serwera lokalnego i agent serwera Azure) i zostało uruchomione przed kontynuowaniem skrypt programu PowerShell w ramach tego kroku.
>
>

Przed rozpoczęciem za pomocą funkcji monitorowania ExpressRoute programu NPM, należy zażądać do białej z obszaru roboczego. [Kliknij tutaj, aby przejść do strony, a następnie wypełnij formularz żądania](https://go.microsoft.com/fwlink/?linkid=862263). (Wskazówki: chcesz otworzyć to łącze w nowym oknie lub na karcie). Listę dozwolonych podobnej może potrwać dzień roboczy lub więcej. Po zakończeniu listę dozwolonych podobnej otrzymasz wiadomość e-mail.

## <a name="setupmonitor"></a>Krok 5: Konfigurowanie NPM do monitorowania usługi ExpressRoute

>[!WARNING]
>Nie kontynuować do momentu swojego obszaru roboczego zostało białej i otrzymasz wiadomość e-mail z potwierdzeniem.
>
>

Po ukończeniu poprzedniej sekcji i sprawdź, czy zostały białej, można skonfigurować monitorowanie.

1. Przejdź do monitora wydajności sieci kafelka przeglądu, przechodząc do **wszystkie zasoby** strony, a następnie klikając na białej obszaru roboczego programu NPM.

  ![obszar roboczy programu npm](.\media\how-to-npm\npm.png)
2. Kliknij przycisk **monitora wydajności sieci** kafelka przeglądu, aby wyświetlić pulpit nawigacyjny. Pulpit nawigacyjny zawiera strony ExpressRoute, który pokazuje, że usługi ExpressRoute w "stanu nieskonfigurowanego". Kliknij przycisk **instalacji funkcji** aby otworzyć stronę konfiguracji monitora wydajności sieci.

  ![Ustawienia funkcji](.\media\how-to-npm\npm2.png)
3. Na stronie konfiguracji przejdź do karty "ExpressRoute komunikacji równorzędnych", znajduje się w okienku po lewej stronie. Kliknij przycisk **odnajdywanie teraz**.

  ![Odnajdywanie](.\media\how-to-npm\13.png)
4. Po zakończeniu odnajdowania, zostanie wyświetlony reguły unikatowy obwodu i nazwy sieci wirtualnej. Początkowo te zasady są wyłączone. Włącz reguły, a następnie wybierz agenci monitorowania i wartości progowe.

  ![rules](.\media\how-to-npm\14.png)
5. Po włączenie zasad i wybraniu wartości i agenci mają być monitorowane, jest Poczekaj około 30 – 60 minut dla wartości rozpocząć wypełniania i **ExpressRoute monitorowania** Kafelki, które stają się dostępne. Gdy zobaczysz Kafelki monitorowania obwody usługi ExpressRoute i siecią połączenia są monitorowane przez NPM.

  ![monitorowanie kafelków](.\media\how-to-npm\15.png)

## <a name="explore"></a>Krok 6: Wyświetl Kafelki monitorowania

### <a name="dashboard"></a>Strona wydajności monitorowania sieci

Na stronie NPM zawiera stronę usługi ExpressRoute, który zawiera przegląd kondycji obwody usługi ExpressRoute i komunikacji równorzędnych.

  ![Pulpit nawigacyjny](.\media\how-to-npm\dashboard.png)

### <a name="circuits"></a>Obwody listy

Aby wyświetlić listę wszystkich monitorowanych obwody usługi ExpressRoute, wybierz polecenie **obwody usługi ExpressRoute** kafelka. Możesz wybrać obwód i wyświetlić jej stan kondycji, wykresy trendu do utraty pakietów, użycie przepustowości i opóźnień. Wykresy są interaktywne. Możesz wybrać okno czasu niestandardowych do kreślenia wykresy. Na wykresie powiększanie i zobaczyć punkty szczegółowych danych można przeciągnąć myszy nad obszarem.

  ![circuit_list](.\media\how-to-npm\circuits.png)

#### <a name="trend"></a>Trend utraty, opóźnienia i przepływności

Wykresy przepustowości, opóźnienia i utratę są interaktywne. W dowolnej sekcji wykresy te można powiększyć za pomocą formantów myszy. Przepustowość, czas oczekiwania i utraty danych można wyświetlić dla innych interwałów, klikając **daty/godziny**, który znajduje się poniżej przycisk akcje w lewym górnym rogu.

![Trend](.\media\how-to-npm\16.png)

### <a name="peerings"></a>Lista komunikacji równorzędnych

Kliknięcie **prywatnej komunikacji równorzędnych** kafelka na pulpicie nawigacyjnym powoduje wyświetlenie listy wszystkich połączeń do sieci wirtualnych za pośrednictwem prywatnej komunikacji równorzędnej. W tym miejscu można wybrać wirtualnej połączenie sieciowe i wyświetlić jej stan kondycji, wykresy trendu do utraty pakietów, użycie przepustowości i opóźnień.

  ![Lista obwodu](.\media\how-to-npm\peerings.png)

### <a name="topology"></a>Topologia obwodu

Aby wyświetlić topologii obwód, kliknij **topologii** kafelka. Powoduje to przejście do wybranego widoku topologii obwodu lub komunikacji równorzędnej. Diagram topologii udostępnia czas oczekiwania na każdy z segmentów w sieci i każdego przeskoku warstwy 3 jest reprezentowany przez węzeł diagramu. Kliknięcie przeskoku ujawnia więcej szczegółów na temat przeskoku.
Można zwiększyć poziom widoczności uwzględnienie przeskoków lokalnymi Przesuń suwak poniżej **filtry**. Przenoszenie suwak w lewo lub w prawo, zwiększa/zmniejsza liczbę przeskoków na wykresie topologii. Czas oczekiwania w każdym segmencie jest widoczny, która umożliwia szybsze izolacji segmentów duże opóźnienie w sieci.

![filtry](.\media\how-to-npm\topology.png)

#### <a name="detailed-topology-view-of-a-circuit"></a>Widok szczegółowy topologii obwodu

Ten widok przedstawia połączeń sieci wirtualnej.
![szczegółowe topologii](.\media\how-to-npm\17.png)
