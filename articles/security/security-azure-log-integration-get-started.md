---
title: "Wprowadzenie do integracji dzienników Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak zainstalować usługi integracji dzienników Azure oraz integrowanie dzienników z magazynu Azure, dzienników inspekcji platformy Azure i alerty Centrum zabezpieczeń Azure."
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomShinder
ms.assetid: 53f67a7c-7e17-4c19-ac5c-a43fabff70e1
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ums.workload: na
ms.date: 07/26/2017
ms.author: TomSh
ms.custom: azlog
ms.openlocfilehash: 9d39ecd513386b75b4b640721f80991caaf9ade8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-log-integration-with-azure-diagnostics-logging-and-windows-event-forwarding"></a>Integracja dzienników Azure z rejestrowania diagnostyki Azure i funkcji przekazywania zdarzeń systemu Windows
Integracja dziennika Azure (AzLog) pozwala na integrowanie nowych dzienników z zasobów platformy Azure lokalnych systemów Security Information and Event Management (SIEM). Integracja ta sprawia, że możliwe ujednoliconego zabezpieczeń pulpitu nawigacyjnego dla wszystkich zasobów, lokalnie lub w chmurze, dzięki czemu może agregować, skorelowania, analizowanie i alerty dla zdarzeń zabezpieczeń skojarzonych z aplikacjami.
>[!NOTE]
Aby uzyskać więcej informacji o integracji dziennika Azure, możesz przejrzeć [Omówienie integracji dziennika Azure](https://docs.microsoft.com/azure/security/security-azure-log-integration-overview).

Ten artykuł pomoże Ci rozpoczęcie pracy z integracji dziennika Azure koncentrujących się na instalację usługi Azlog i Integrowanie usługi z diagnostyki Azure. Usługa Azure dziennika integracji będą w stanie do zbierania informacji dziennika zdarzeń systemu Windows z kanału zdarzeń zabezpieczeń systemu Windows z maszyn wirtualnych wdrożonych w IaaS platformy Azure. To jest bardzo podobny do "Funkcji przekazywania zdarzeń" czy użyto lokalnymi.

>[!NOTE]
>Możliwość dostosowania danych wyjściowych dzienników Azure integracji Siem są udostępniane przez sam SIEM. Zobacz artykuł [integracji Azure dziennika integracji z lokalnymi system SIEM](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/) Aby uzyskać więcej informacji.

Się oczywiste — Usługa integracji dziennika Azure działa na komputerze fizycznym lub wirtualnym, korzystających z systemu Windows Server 2008 R2 lub nowszym systemu operacyjnego (Windows Server 2012 R2 lub Windows Server 2016 są preferowane).

Komputer fizyczny można uruchomić lokalnie (lub w witrynie dostawcy usług hostingowych). Jeśli chcesz uruchomić usługę Azure dziennika integracji na maszynie wirtualnej, czy maszyna wirtualna może być znajdujących się lokalnie lub w chmurze publicznej, takich jak Microsoft Azure.

Maszyny fizycznej lub wirtualnej uruchomiona usługa integracji dziennika Azure wymaga połączenia sieciowego chmurze publicznej Azure. Kroki opisane w tym artykule szczegółowo konfiguracji.

## <a name="prerequisites"></a>Wymagania wstępne
Co najmniej instalacji AzLog wymaga następujących elementów:
* **Subskrypcji platformy Azure**. Jeśli jej nie masz, możesz zarejestrować się w celu [utworzenia bezpłatnego konta](https://azure.microsoft.com/free/).
* A **konta magazynu** które mogą być używane do rejestrowania diagnostyki Windows Azure (możesz użyć konta magazynu wstępnie skonfigurowane lub Utwórz nową — zostanie przedstawiony sposób konfigurowania konta magazynu w dalszej części tego artykułu)
  >[!NOTE]
  W zależności od danego scenariusza konta magazynu nie może być wymagane. Azure Diagnostics scenariusz omówione w tym artykule, co jest potrzebne.
* **Dwa systemy**: komputerze, na którym działa usługa Azure dziennika integracji i na komputerze, który będzie monitorowany i jego informacje rejestrowania wysyłane do usługi Azlog maszyny.
   * Maszyny, który chcesz monitorować — jest to maszyny Wirtualnej z systemem jako [maszyny wirtualnej platformy Azure](../virtual-machines/virtual-machines-windows-overview.md)
   * Na komputerze, który uruchomi dzienników Azure Usługa integracji; Ten komputer będzie zbierać wszystkie informacje dziennika, które później zostaną zaimportowane do rozwiązania SIEM.
    * Ten system może być lokalnie lub na platformie Microsoft Azure.  
    * Musi być uruchomiona x64 wersji systemu Windows server 2008 R2 z dodatkiem SP1 lub nowszej i mają .NET 4.5.1 zainstalowane. Można określić wersji programu .NET zainstalowane wykonując artykuł [porady: ustalić, które .NET Framework są zainstalowane wersje](https://msdn.microsoft.com/library/hh925568)  
    Musi on mieć łączność z kontem magazynu platformy Azure, używane na potrzeby rejestrowania diagnostyki Azure. Firma Microsoft udostępni instrukcje w dalszej części tego artykułu w sposób możesz potwierdzić połączeń

Do szybkiego pokaz proces tworzenia maszyny wirtualnej przy użyciu portalu Azure Przyjrzyjmy się wideo poniżej.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure-Security-Videos/Azure-Log-Integration-Videos-Create-a-Virtual-Machine/player]



## <a name="deployment-considerations"></a>Zagadnienia dotyczące wdrażania
Podczas testowania integracji dziennika Azure, można użyć dowolnego systemu, który spełnia minimalne wymagania systemu operacyjnego. Jednak w środowisku produkcyjnym obciążenia może wymagać planowania skalowania w górę lub w poziomie.

Można uruchomić wiele wystąpień usługi integracji dziennika Azure (jedno wystąpienie na maszynie fizycznej lub wirtualnej), jeśli wolumin zdarzeń jest duża. Ponadto użytkownik może równoważyć obciążenie konta magazynu diagnostyki Azure dla systemu Windows (WAD) i liczbę subskrypcji zapewnić wystąpienia powinna być oparta na wydajność.
>[!NOTE]
W tej chwili nie mamy zalecenia dotyczące Kiedy skalować w poziomie wystąpień maszyn integracji dzienników Azure (np. maszyn, które są uruchomione usługi integracji dzienników Azure) lub dla konta magazynu lub subskrypcji. Skalowanie decyzje powinny być oparte na Twojej uwagi wydajności w każdym z tych obszarów.

Masz również opcję, aby skalować usługę Azure dziennika integracji w celu zwiększenia wydajności. Następujące metryki wydajności mogą pomóc w części ustalanie rozmiarów maszyn, które chcesz uruchamiać Usługa integracji dzienników Azure:
* Na maszynie procesor 8 (podstawowe) jedno wystąpienie Azlog Integrator może przetworzyć około 24 miliony zdarzeń na dzień (~1M/hour).

* Na komputerze 4-procesora (podstawowe) jedno wystąpienie Azlog Integrator może przetworzyć około 1,5 miliona zdarzeń na dzień (~62.5K/hour).

## <a name="install-azure-log-integration"></a>Zainstaluj integracji dzienników Azure
Aby zainstalować Azure integracji dziennika, należy pobrać [integracji dzienników Azure](https://www.microsoft.com/download/details.aspx?id=53324) pliku instalacyjnego. Wykonaj czynności procedury instalacji i zdecyduj, jeśli chcesz podać informacje o telemetrii do firmy Microsoft.  

![Ekran instalacji z zaznaczonym polem telemetrii](./media/security-azure-log-integration-get-started/telemetry.png)

*
> [!NOTE]
> Firma Microsoft zaleca, aby zezwolić firmie Microsoft na zbieranie danych telemetrycznych. Zbieranie danych telemetrycznych można wyłączyć, usuwając zaznaczenie pola wyboru tej opcji.
>


Usługa Azure dziennika integracji zbiera dane telemetryczne z komputera, na którym jest zainstalowany.  

Zbierane dane z telemetrii jest:

* Wyjątków występujących podczas wykonywania integracji dzienników Azure
* Metryki dotyczące liczby zapytań i przetwarzania zdarzeń
* Statystyki, o które Azlog.exe są używane opcje wiersza polecenia

Poniższy klip wideo omówiono proces instalacji.
> [!VIDEO https://channel9.msdn.com/Blogs/Azure-Security-Videos/Azure-Log-Integration-Videos-Install-Azure-Log-Integration/player]



## <a name="post-installation-and-validation-steps"></a>Opublikuj kroków instalacji i sprawdzania poprawności
Po zakończeniu procedury podstawowa konfiguracja, wszystko jest gotowe kroki do wykonania po instalacji i kroki weryfikacji:
1. Otwórz okno programu PowerShell z podwyższonym poziomem uprawnień i przejdź do **c:\Program Files\Microsoft Azure dziennika integracji**
2. Pierwszym krokiem, który należy wykonać jest uzyskanie poleceń cmdlet AzLog zaimportowane. Możesz to zrobić, uruchamiając skrypt **LoadAzlogModule.ps1** (Zwróć uwagę ". \" w poniższym poleceniu). Typ **.\LoadAzlogModule.ps1** i naciśnij klawisz **ENTER**.  
Powinien zostać wyświetlony ekran podobny do wyświetlaną na poniższej ilustracji. </br></br>
![Ekran instalacji z zaznaczonym polem telemetrii](./media/security-azure-log-integration-get-started/loaded-modules.png) </br></br>
3. Teraz należy skonfigurować AzLog do użycia w określonym środowisku platformy Azure. "Środowiska platformy Azure" jest "type" w centrum danych chmury Azure, który chcesz współpracować. Gdy istnieje wiele środowisk Azure w tym momencie, obecnie odpowiednie opcje są **AzureCloud** lub **AzureUSGovernment**.   W środowisku PowerShell z podwyższonym poziomem uprawnień, upewnij się, że jesteś w **c:\program files\Microsoft Azure dziennika integracji\** </br></br>
    Gdy istnieje, uruchom polecenie: </br>
    ``Set-AzlogAzureEnvironment -Name AzureCloud``(w przypadku komercyjnej Azure)

      >[!NOTE]
      Jeśli polecenie powiedzie się, nie otrzymasz żadnych opinii.  Jeśli chcesz użyć w chmurze Azure dla instytucji rządowych nam, użyj **AzureUSGovernment** (dla zmiennej Name) dla chmury dla instytucji rządowych USA. Inne chmury Azure nie są obsługiwane w tej chwili.  
4. Zanim będzie możliwe monitorowanie systemu konieczne będzie nazwa konta magazynu w użyciu diagnostyki Azure.  W portalu Azure przejdź do **maszyn wirtualnych** i wyszukaj maszynę wirtualną, która będzie monitorował. W **właściwości** wybierz **ustawień diagnostycznych**.  Polecenie **agenta** i zanotuj nazwę konta magazynu określony. Konieczne będzie nazwa tego konta w późniejszym kroku.
![Ustawienia diagnostyki Azure](./media/security-azure-log-integration-get-started/storage-account-large.png) </br></br>

      ![Ustawienia diagnostyki Azure](./media/security-azure-log-integration-get-started/azure-monitoring-not-enabled-large.png)
      >[!NOTE]
      Jeśli podczas tworzenia maszyny wirtualnej nie włączono monitorowania będzie mieć opcję, aby ją włączyć, jak pokazano powyżej.
5. Teraz możemy przełączać wymagające uwagi do maszyny integracji dzienników Azure. Musimy zweryfikować, czy istnieje połączenie z kontem magazynu z systemu, w którym zainstalowano integracji dziennika Azure. Komputer fizyczny lub maszyna wirtualna uruchomiona usługa integracji dziennika Azure wymaga dostępu do konta magazynu można pobrać informacji o zarejestrowane przez diagnostyki Azure, zgodnie z konfiguracją w każdym monitorowanym systemie.  
  1. Możesz pobrać Eksploratora usługi Storage Azure [tutaj](http://storageexplorer.com/).
  2. Wykonaj czynności procedury instalacji
  3. Po zakończeniu instalacji kliknij przycisk **dalej** i zaznacz pole wyboru **Uruchom Eksploratora usługi Microsoft Azure Storage** zaznaczone.  
  4. Logowanie do platformy Azure.
  5. Sprawdź, czy widzisz konta magazynu, które skonfigurowano diagnostyki Azure.  
![Konta magazynu](./media/security-azure-log-integration-get-started/storage-account.jpg) </br></br>
   6. Zwróć uwagę, że istnieje kilka opcji w obszarze kont magazynu. Jeden z nich jest **tabel**. W obszarze **tabel** powinny pojawić się jeden o nazwie **WADWindowsEventLogsTable**. </br></br>
   ![Konta magazynu](./media/security-azure-log-integration-get-started/storage-explorer.png) </br>

## <a name="integrate-azure-diagnostic-logging"></a>Integracja rejestrowania diagnostycznego Azure
W tym kroku skonfigurujesz komputerze, na którym działa usługa Azure dziennika integracji do nawiązania połączenia konta magazynu, który zawiera pliki dziennika.
Aby ukończyć ten krok potrzebujemy góry kilka rzeczy.  
* **FriendlyNameForSource:** jest przyjazna nazwa, którą można zastosować do konta magazynu, że skonfigurowano maszynę wirtualną do przechowywania informacji z diagnostyki Azure
* **StorageAccountName:** jest to nazwa konta magazynu określony podczas konfigurowania diagnotics platformy Azure.  
* **Atrybutu StorageKey:** jest klucz magazynu dla konta magazynu przechowywania informacji diagnostyki Azure dla tej maszyny wirtualnej.  

Wykonaj poniższe kroki, aby uzyskać klucz magazynu:
 1. Przejdź do [portalu Azure](http://portal.azure.com).
 2. W okienku nawigacji konsoli platformy Azure, przewiń w dół i kliknij przycisk **więcej usług.**

 ![Więcej usług](./media/security-azure-log-integration-get-started/more-services.png)
 3. Wprowadź **magazynu** w **filtru** pola tekstowego. Kliknij przycisk **kont magazynu** (Ta wartość będzie wyświetlana po wprowadzeniu **magazynu**)

   ![pole filtru](./media/security-azure-log-integration-get-started/filter.png)
 4. Zostanie wyświetlona lista kont magazynu, kliknij dwukrotnie konto przypisane do przechowywania dziennika.

   ![Lista kont magazynu](./media/security-azure-log-integration-get-started/storage-accounts.png)
 5. Polecenie **klucze dostępu** w **ustawienia** sekcji.

  ![Klawisze dostępu](./media/security-azure-log-integration-get-started/storage-account-access-keys.png)
 6. Kopiuj **klucz1** i umieszczenie go w bezpiecznej lokalizacji, do której będziesz mieć dostęp do kolejnego kroku.

   ![dwa klucze dostępu](./media/security-azure-log-integration-get-started/storage-account-access-keys.png)
 7. Na serwerze zainstalowania integracji dziennika Azure Otwórz wiersz polecenia z podwyższonym poziomem uprawnień (należy pamiętać, że firma Microsoft korzysta z podwyższonym poziomem uprawnień okno wiersza polecenia w tym miejscu, nie z podwyższonym poziomem uprawnień konsoli programu PowerShell).
 8. Przejdź do **c:\Program Files\Microsoft Azure dziennika integracji**
 9. Uruchom polecenie ``Azlog source add <FriendlyNameForTheSource> WAD <StorageAccountName> <StorageKey> `` </br> Na przykład ``Azlog source add Azlogtest WAD Azlog9414 fxxxFxxxxxxxxywoEJK2xxxxxxxxxixxxJ+xVJx6m/X5SQDYc4Wpjpli9S9Mm+vXS2RVYtp1mes0t9H5cuqXEw==`` czy identyfikator subskrypcji wyświetlani w zdarzeniu XML, Dołącz identyfikator subskrypcji do przyjazną nazwę: ``Azlog source add <FriendlyNameForTheSource>.<SubscriptionID> WAD <StorageAccountName> <StorageKey>`` lub na przykład``Azlog source add Azlogtest.YourSubscriptionID WAD Azlog9414 fxxxFxxxxxxxxywoEJK2xxxxxxxxxixxxJ+xVJx6m/X5SQDYc4Wpjpli9S9Mm+vXS2RVYtp1mes0t9H5cuqXEw==``

>[!NOTE]  
Poczekaj do 60 minut, a następnie wyświetlać zdarzenia, które są pobierane z konta magazynu. Aby wyświetlić, otwórz **Podgląd zdarzeń > Dzienniki systemu Windows > zdarzenia przekazane** na Azlog Integrator.

Tutaj można zobaczyć wideo przez kroki wymienionego powyżej.
> [!VIDEO https://channel9.msdn.com/Blogs/Azure-Security-Videos/Azure-Log-Integration-Videos-Enable-Diagnostics-and-Storage/player]


## <a name="what-if-data-is-not-showing-up-in-the-forwarded-events-folder"></a>Co zrobić, jeśli dane nie pojawia się w folderze zdarzenia przekazane?
Jeśli po upływie godziny danych nie pojawia się w **zdarzenia przekazane** folder, a następnie:

1. Sprawdź komputerze, na którym działa usługa Azure dziennika integracji i upewnij się, że można uzyskać dostępu do usługi Azure. Test łączności, spróbuj otworzyć [portalu Azure](http://portal.azure.com) z przeglądarki.
2. Upewnij się, że konto użytkownika **Azlog** ma uprawnienia do zapisu w folderze **users\Azlog**.
  <ol type="a">
   <li>Otwórz **Eksploratora Windows** </li>
  <li> Przejdź do **c:\users** </li>
  <li> Kliknij prawym przyciskiem myszy **c:\users\Azlog** </li>
  <li> Polecenie **zabezpieczeń**  </li>
  <li> Polecenie **NT Service\Azlog** i sprawdź uprawnienia dla konta. Jeśli konto nie ma na tej karcie lub odpowiednie uprawnienia, które nie są obecnie pokazujące, można przyznać praw konta na tej karcie.</li>
  </ol>
3.Upewnij się, konto magazynu, dodane w poleceniu **dodać źródła Azlog** znajduje się po uruchomieniu polecenia **listy źródeł Azlog**.
4. Przejdź do **Podgląd zdarzeń > Dzienniki systemu Windows > Aplikacja** aby zobaczyć, czy wystąpiły żadne błędy zgłoszone integracji dzienników Azure.


Jeśli wystąpiły problemy podczas instalacji i konfiguracji, otwórz [żądania obsługi](../azure-supportability/how-to-create-azure-support-request.md), wybierz pozycję **integracji dziennika** jako usługa żądania pomocy technicznej.

Inną opcją pomocy technicznej jest [Forum MSDN integracji dziennika Azure](https://social.msdn.microsoft.com/Forums/home?forum=AzureLogIntegration). W tym miejscu Wspólnoty może obsługiwać wzajemnie pytania, odpowiedzi, porady i wskazówki dotyczące sposobu maksymalne wykorzystanie integracji dziennika Azure. Ponadto zespół Azure dziennika integracji monitoruje tym forum i zawierają informacje pomocne przy każdym możemy.

## <a name="next-steps"></a>Następne kroki
Aby dowiedzieć się więcej na temat integracji dziennika Azure, można znaleźć w następujących dokumentach:

* [Microsoft Azure dziennika integracji Azure dzienników](https://www.microsoft.com/download/details.aspx?id=53324) — Centrum pobierania, aby uzyskać szczegółowe informacje, wymagania systemowe i zainstalować instrukcje dotyczące integracji dzienników Azure.
* [Wprowadzenie do integracji dzienników Azure](security-azure-log-integration-overview.md) — ten dokument stanowi wprowadzenie do integracji dzienników Azure, jego kluczowych możliwości i jak działa.
* [Czynności konfiguracyjnych partnera](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/) — ten wpis w blogu przedstawiono sposób konfigurowania integracji dzienników Azure do pracy z rozwiązań partnerskich Splunk HP ArcSight i IBM QRadar. Jest to nasze Bieżące wskazówki dotyczące sposobu konfigurowania składników SIEM. Skontaktuj się z dostawcą SIEM najpierw uzyskać dodatkowe szczegóły.
* [Dzienników Azure — często zadawane pytania (FAQ) integracji](security-azure-log-integration-faq.md) — to często zadawane pytania dotyczące odpowiedzi na pytania dotyczące integracji dzienników Azure.
* [Integrowanie Centrum zabezpieczeń alertów z usługi Azure dziennika integracji](../security-center/security-center-integrating-alerts-with-log-integration.md) — tym dokumencie przedstawiono sposób synchronizować alerty Centrum zabezpieczeń, wraz z zebrane przez diagnostyki Azure i Dzienniki aktywności platformy Azure, z rozwiązania SIEM lub analizy dzienników zdarzeń zabezpieczeń maszyny wirtualnej.
* [Nowe funkcje diagnostyki Azure i dzienników inspekcji platformy Azure](https://azure.microsoft.com/blog/new-features-for-azure-diagnostics-and-azure-audit-logs/) — ten wpis w blogu stanowi wprowadzenie do dzienników inspekcji platformy Azure i inne funkcje, które ułatwiają uzyskać wgląd w funkcjonowanie zasobów platformy Azure.
