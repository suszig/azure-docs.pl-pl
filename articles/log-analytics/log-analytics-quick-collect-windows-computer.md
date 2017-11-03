---
title: "Zbierz dane z lokalnych komputerów z systemem Windows z Azure Log Analytics | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak wdrożyć agenta analizy dzienników dla systemu Windows na komputerach poza platformą Azure i Włącz zbieranie danych za pomocą analizy dzienników."
services: log-analytics
documentationcenter: log-analytics
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 10/16/2017
ms.author: magoedte
ms.custom: mvc
ms.openlocfilehash: 6da36184baee921c828b037e1337df2d14c79462
ms.sourcegitcommit: e6029b2994fa5ba82d0ac72b264879c3484e3dd0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/24/2017
---
# <a name="collect-data-from-windows-computers-hosted-in-your-environment"></a>Zbieranie danych z komputerów z systemem Windows w środowisku
[Analiza dzienników Azure](log-analytics-overview.md) może zbierać dane bezpośrednio z komputerów fizycznych lub wirtualnych Windows i innych zasobów w danym środowisku, w jednym repozytorium szczegółowej analizy i korelacji.  Ta opcja szybkiego startu przedstawia sposób konfigurowania i zbieranie danych z komputera z systemem Windows z kilku prostych krokach.  Dla maszyn wirtualnych systemu Windows Azure, zobacz następujący temat [zbierać dane o maszynach wirtualnych platformy Azure](log-analytics-quick-collect-azurevm.md).  
 
Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="log-in-to-azure-portal"></a>Zaloguj się do portalu Azure
Zaloguj się do portalu Azure pod adresem [https://portal.azure.com](https://portal.azure.com). 

## <a name="create-a-workspace"></a>Tworzenie obszaru roboczego
1. W portalu Azure kliknij **więcej usług** znaleziono w lewym dolnym rogu. Na liście zasobów wpisz **Log Analytics**. Po rozpoczęciu pisania zawartość listy jest filtrowana w oparciu o wpisywane dane. Wybierz **dziennika analizy**.<br><br> ![Witryna Azure Portal](media/log-analytics-quick-collect-azurevm/azure-portal-01.png)<br><br>  
2. Kliknij przycisk **Utwórz**, a następnie wybierz opcje dla następujących elementów:

  * Podaj nazwę dla nowego **obszarem roboczym pakietu OMS**, takich jak *DefaultLAWorkspace*. 
  * Wybierz **Subskrypcję** do połączenia poprzez wybór subskrypcji z listy rozwijanej, jeśli domyślnie wybrana subskrypcja jest niewłaściwa.
  * Aby uzyskać **grupy zasobów**, wybierz istniejącą grupę zasobów, która zawiera co najmniej jednej maszyny wirtualnej Azure.  
  * Wybierz **lokalizacji** maszyny wirtualne są wdrażane.  Aby uzyskać dodatkowe informacje, sprawdź, która [Log Analytics jest dostępna w regionach](https://azure.microsoft.com/regions/services/).
  * Możesz wybrać spośród trzech różnych **warstw cenowych** w analizy dzienników, ale dla tego przewodnika Szybki Start zamierzasz wybierz **wolnego** warstwy.  Aby uzyskać dodatkowe informacje dotyczące konkretnego warstw, zobacz [szczegóły cennika usługi Analiza dzienników](https://azure.microsoft.com/pricing/details/log-analytics/).

        ![Create Log Analytics resource blade](media/log-analytics-quick-collect-azurevm/create-loganalytics-workspace-01.png)<br>  
3. Po podaniu wymaganych informacji w **obszarem roboczym pakietu OMS** okienku, kliknij przycisk **OK**.  

Podczas weryfikowania informacji i tworzenia obszaru roboczego możesz śledzić postęp w sekcji **Powiadomienia** z poziomu menu. 

## <a name="obtain-workspace-id-and-key"></a>Uzyskaj identyfikator i klucz
Przed zainstalowaniem programu Microsoft Monitoring Agent dla systemu Windows, należy identyfikator i klucz obszaru roboczego analizy dzienników.  Ta informacja jest wymagana przez Kreatora instalacji, aby poprawnie skonfigurować agenta i upewnij się, że mogą się komunikować z analizy dzienników.  

1. W portalu Azure kliknij **więcej usług** znaleziono w lewym dolnym rogu. Na liście zasobów wpisz **Log Analytics**. Po rozpoczęciu pisania zawartość listy jest filtrowana w oparciu o wpisywane dane. Wybierz **dziennika analizy**.
2. Na liście obszarów roboczych usługi Analiza dzienników, wybierz *DefaultLAWorkspace* utworzony wcześniej.
3. Wybierz **Zaawansowane ustawienia**.<br><br> ![Ustawienia zaawansowane analizy dzienników](media/log-analytics-quick-collect-azurevm/log-analytics-advanced-settings-01.png)<br><br>  
4. Wybierz **połączonych źródeł**, a następnie wybierz **serwerów z systemem Windows**.   
5. Wartość z prawej strony **identyfikator obszaru roboczego** i **klucz podstawowy**. Skopiuj i Wklej zarówno do Twojego ulubionego edytora.   

## <a name="install-the-agent-for-windows"></a>Zainstaluj agenta dla systemu Windows
Poniższe kroki zainstalować i skonfigurować agenta dla analizy dzienników w chmurze Azure oraz Azure dla instytucji rządowych za pomocą Instalatora programu Microsoft Monitoring Agent na tym komputerze.  

1. Na **serwerów z systemem Windows** wybierz odpowiedni **Pobierz agenta systemu Windows** wersji do pobrania w zależności od architektury procesora systemu operacyjnego Windows.
2. Uruchom Instalatora, aby zainstalować agenta na komputerze.
2. Na **powitalnej** kliknij przycisk **dalej**.
3. Na **postanowień licencyjnych** , zapoznaj się z licencyjnymi a następnie kliknij przycisk **zgadzam się**.
4. Na **Folder docelowy** strony, zmienić lub zachować domyślny folder instalacji, a następnie kliknij przycisk **dalej**.
5. Na **opcje instalacji agenta** wybierz Połącz agenta z do Analiza dzienników Azure (OMS), a następnie kliknij pozycję **dalej**.   
6. Na **Azure Log Analytics** strony, wykonaj następujące czynności:
   1. Wklej **identyfikator obszaru roboczego** i **klucz obszaru roboczego (klucz podstawowy)** skopiowanego wcześniej.  Jeśli komputer należy zgłosić obszaru roboczego analizy dzienników w chmurze Azure dla instytucji rządowych, wybierz **Azure instytucji rządowych Stanów Zjednoczonych** z **chmury Azure** listy rozwijanej.  
   2. Jeśli komputer musi komunikować się za pośrednictwem serwera proxy z usługą analizy dzienników, kliknij przycisk **zaawansowane** i podaj adres URL i numer portu serwera proxy.  Jeśli serwer proxy wymaga uwierzytelniania, wpisz nazwę użytkownika i hasło do uwierzytelniania za pomocą serwera proxy, a następnie kliknij przycisk **dalej**.  
7. Kliknij przycisk **dalej** po zakończeniu, podając wymagane ustawienia konfiguracyjne.<br><br> ![Wklej klucz podstawowy i identyfikator obszaru roboczego](media/log-analytics-quick-collect-windows-computer/log-analytics-mma-setup-laworkspace.png)<br><br>
8. Na **gotowy do instalacji** strony, przejrzyj wybrane opcje, a następnie kliknij przycisk **zainstalować**.
9. Na **konfiguracji została ukończona pomyślnie** kliknij przycisk **Zakończ**.

Po zakończeniu **programu Microsoft Monitoring Agent** pojawia się w **Panelu sterowania**. Możesz przejrzeć konfigurację i sprawdź, czy agent jest połączony z analizą dzienników. Po podłączeniu na **Analiza dzienników Azure (OMS)** kartę, agent wyświetla komunikat z informacją: **Microsoft Monitoring Agent pomyślnie połączył się z usługą Microsoft Operations Management Suite.**<br><br> ![MMA stanu połączenia analizy dzienników](media/log-analytics-quick-collect-windows-computer/log-analytics-mma-laworkspace-status.png)

## <a name="collect-event-and-performance-data"></a>Zbieranie danych zdarzeń i wydajności
Analizy dzienników można zbierać zdarzenia z dziennika zdarzeń systemu Windows i liczniki wydajności, które określisz dłuższy okres analizy i raportowania i podejmij akcję po wykryciu określonego warunku.  Wykonaj następujące kroki, aby rozpocząć konfigurowanie zbierania zdarzeń z dziennika zdarzeń systemu Windows i kilka typowych liczniki wydajności.  

1. W portalu Azure kliknij **więcej usług** znaleziono w lewym dolnym rogu. Na liście zasobów wpisz **Log Analytics**. Po rozpoczęciu pisania zawartość listy jest filtrowana w oparciu o wpisywane dane. Wybierz **dziennika analizy**.
2. Wybierz **Zaawansowane ustawienia**.<br><br> ![Ustawienia zaawansowane analizy dzienników](media/log-analytics-quick-collect-azurevm/log-analytics-advanced-settings-01.png)<br><br> 
3. Wybierz **danych**, a następnie wybierz **dzienniki zdarzeń systemu Windows**.  
4. Możesz dodać dziennika zdarzeń, wpisując nazwę dziennika.  Typ **systemu** , a następnie kliknij znak plus  **+** .  
5. W tabeli, sprawdź wag **błąd** i **ostrzeżenie**.   
6. Kliknij przycisk **zapisać** w górnej części strony, aby zapisać konfigurację.
7. Wybierz **danych wydajności systemu Windows** można włączyć kolekcji liczników wydajności na komputerze z systemem Windows. 
8. Podczas pierwszej konfiguracji liczników wydajności systemu Windows dla nowego obszaru roboczego analizy dzienników, podano opcję, aby szybko utworzyć kilka typowych liczników. Są one wyświetlane pole wyboru obok każdego.<br> ![Liczniki wydajności systemu Windows domyślne wybrane](media/log-analytics-quick-collect-azurevm/windows-perfcounters-default.png).<br> Kliknij przycisk **Dodaj wybrane liczniki wydajności**.  Są one dodawane i ustawienia wstępnego z interwału próbkowania sekundę dziesięć kolekcji.  
9. Kliknij przycisk **zapisać** w górnej części strony, aby zapisać konfigurację.

## <a name="view-data-collected"></a>Wyświetlanie danych zebranych
Teraz, gdy jest włączone zbieranie danych, pozwala uruchomić przykład wyszukiwania prostego dziennika, aby wyświetlić dane z komputera docelowego.  

1. W portalu Azure w ramach wybranego obszaru roboczego kliknij **wyszukiwania dziennika** kafelka.  
2. W okienku wyszukiwania dziennika, w polu wpisz kwerendę `Perf` , a następnie naciśnij klawisz wprowadź lub kliknij przycisk wyszukiwania z prawej strony pola zapytania.<br><br> ![Przykład zapytania wyszukiwania dziennika analizy dzienników](media/log-analytics-quick-collect-linux-computer/log-analytics-portal-queryexample.png)<br><br> Na przykład 735 rekordów wydajności zwrócić zapytania na poniższej ilustracji.<br><br> ![Wynik wyszukiwania dziennika analizy dzienników](media/log-analytics-quick-collect-windows-computer/log-analytics-search-perf.png)

## <a name="clean-up-resources"></a>Oczyszczanie zasobów
Gdy nie są już potrzebne, można usunąć agenta z komputera z systemem Windows i usuwanie obszaru roboczego analizy dzienników.  

Aby usunąć agenta, wykonaj następujące kroki.

1. Otwórz **Panel sterowania**.
2. Otwórz **programy i funkcje**.
3. W **programy i funkcje**, wybierz pozycję **programu Microsoft Monitoring Agent** i kliknij przycisk **Odinstaluj**.

Aby usunąć obszar roboczy, wybierz obszar roboczy analizy dzienników utworzono wcześniej i kliknij stronę zasobów **usunąć**.<br><br> ![Usuń zasób analizy dzienników](media/log-analytics-quick-collect-azurevm/log-analytics-portal-delete-resource.png)

## <a name="next-steps"></a>Następne kroki
Teraz, kiedy są zbierane operacyjne i dane dotyczące wydajności z komputera lokalnego systemu Linux, można łatwo rozpocząć eksplorowanie, analizowanie i podjęcie działań na danych zbieranych dla *wolnego*.  

Aby dowiedzieć się, jak przeglądać i analizować dane, nadal samouczka.   

> [!div class="nextstepaction"]
> [Wyświetlanie i analizowanie danych analizy dzienników](log-analytics-tutorial-viewdata.md)
