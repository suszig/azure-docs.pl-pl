---
title: Zbieraj dane o maszynach wirtualnych platformy Azure | Dokumentacja firmy Microsoft
description: "Dowiedz się, jak włączyć rozszerzenia maszyny Wirtualnej agenta pakietu OMS i włączyć zbieranie danych z maszyn wirtualnych Azure z analizy dzienników."
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
ms.date: 09/20/2017
ms.author: magoedte
ms.custom: mvc
ms.openlocfilehash: 2dec744b512a86a30cec1f334e265572fa7acc3e
ms.sourcegitcommit: e6029b2994fa5ba82d0ac72b264879c3484e3dd0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/24/2017
---
# <a name="collect-data-about-azure-virtual-machines"></a>Zbierz dane o maszynach wirtualnych platformy Azure
[Analiza dzienników Azure](log-analytics-overview.md) może zbierać dane bezpośrednio z maszyn wirtualnych platformy Azure i innych zasobów w danym środowisku, w jednym repozytorium szczegółowej analizy i korelacji.  Ta opcja szybkiego startu przedstawia sposób konfigurowania i zbieranie danych z maszyn wirtualnych systemu Windows lub Azure Linux z kilku prostych krokach.  
 
Tego przewodnika Szybki Start założono, że masz już istniejącej maszynie wirtualnej platformy Azure. Jeśli nie możesz [utworzyć Maszynę wirtualną systemu Windows](../virtual-machines/windows/quick-create-portal.md) lub [Utwórz Maszynę wirtualną systemu Linux](../virtual-machines/linux/quick-create-cli.md) po naszej poradniki Szybki Start maszyny Wirtualnej.

## <a name="log-in-to-azure-portal"></a>Zaloguj się do portalu Azure
Zaloguj się do portalu Azure pod adresem [https://portal.azure.com](https://portal.azure.com). 

## <a name="create-a-workspace"></a>Tworzenie obszaru roboczego
1. W portalu Azure kliknij **więcej usług** znaleziono w lewym dolnym rogu. Na liście zasobów wpisz **Log Analytics**. Po rozpoczęciu pisania zawartość listy jest filtrowana w oparciu o wpisywane dane. Wybierz **dziennika analizy**.<br> ![Witryna Azure Portal](media/log-analytics-quick-collect-azurevm/azure-portal-01.png)<br>  
2. Kliknij przycisk **Utwórz**, a następnie wybierz opcje dla następujących elementów:

  * Podaj nazwę dla nowego **obszarem roboczym pakietu OMS**, takich jak *DefaultLAWorkspace*. 
  * Wybierz **Subskrypcję** do połączenia poprzez wybór subskrypcji z listy rozwijanej, jeśli domyślnie wybrana subskrypcja jest niewłaściwa.
  * Aby uzyskać **grupy zasobów**, wybierz istniejącą grupę zasobów, która zawiera co najmniej jednej maszyny wirtualnej Azure.  
  * Wybierz **lokalizacji** maszyny wirtualne są wdrażane.  Aby uzyskać dodatkowe informacje, sprawdź, która [Log Analytics jest dostępna w regionach](https://azure.microsoft.com/regions/services/).
  * Możesz wybrać spośród trzech różnych **warstw cenowych** w analizy dzienników, ale dla tego przewodnika Szybki Start zamierzasz wybierz **wolnego** warstwy.  Aby uzyskać dodatkowe informacje dotyczące konkretnego warstw, zobacz [szczegóły cennika usługi Analiza dzienników](https://azure.microsoft.com/pricing/details/log-analytics/).

        ![Create Log Analytics resource blade](./media/log-analytics-quick-collect-azurevm/create-loganalytics-workspace-01.png)<br>  
3. Po podaniu wymaganych informacji w **obszarem roboczym pakietu OMS** okienku, kliknij przycisk **OK**.  

Podczas weryfikowania informacji i tworzenia obszaru roboczego możesz śledzić postęp w sekcji **Powiadomienia** z poziomu menu. 

## <a name="enable-the-log-analytics-vm-extension"></a>Włączanie rozszerzenia maszyny Wirtualnej analizy dziennika
Dla systemu Windows i Linux wdrożone maszyny wirtualne na platformie Azure należy zainstalować agenta analizy dzienników rozszerzenie maszyny Wirtualnej analizy dziennika.  Przy użyciu rozszerzenia upraszcza proces instalacji i automatycznie konfiguruje agenta do wysyłania danych do obszaru roboczego analizy dzienników, który określisz. Agent jest również uaktualniana automatycznie, sprawdzeniu, czy masz najnowsze funkcje i poprawki.

Można zauważyć banerze w górnej części strony zasobów Log Analytics w portalu zaproszenie do uaktualnienia.  Uaktualnienie nie jest wymagane na potrzeby tego przewodnika Szybki Start.<br>

![Analiza dzienników uaktualniania powiadomienia w portalu Azure](media/log-analytics-quick-collect-azurevm/log-analytics-portal-upgradebanner.png).    
1. W portalu Azure kliknij **więcej usług** znaleziono w lewym dolnym rogu. Na liście zasobów wpisz **Log Analytics**. Po rozpoczęciu pisania zawartość listy jest filtrowana w oparciu o wpisywane dane. Wybierz **dziennika analizy**.
2. Na liście obszarów roboczych usługi Analiza dzienników, wybierz *DefaultLAWorkspace* utworzony wcześniej.
3. W menu po lewej stronie, w obszarze roboczym źródła danych, kliknij polecenie **maszyn wirtualnych**.  
4. Na liście **maszyn wirtualnych**, wybierz maszynę wirtualną, którą chcesz zainstalować agenta na. Zwróć uwagę, że **stan połączenia OMS** dla maszyny Wirtualnej wskazuje, że jest **niepołączone**.
5. Informacje dotyczące maszyny wirtualnej, wybierz **Connect**. Agent jest automatycznie instalowany i konfigurowany dla obszaru roboczego analizy dzienników. Ten proces trwa kilka minut, w tym czasie **stan** jest **łączenie**.
6. Po zainstalowaniu i połącz agenta z **stan połączenia OMS** zostaną zaktualizowane **ten obszar roboczy**.

## <a name="collect-event-and-performance-data"></a>Zbieranie danych zdarzeń i wydajności
Analizy dzienników można zbierać zdarzenia z dzienników zdarzeń systemu Windows lub Linux Syslog i liczniki wydajności, które określisz dłuższy okres analizy i raportowania i podejmij akcję po wykryciu określonego warunku.  Wykonaj następujące kroki, aby rozpocząć konfigurowanie zbierania zdarzeń w dzienniku systemu Windows i systemu Linux i kilka typowych liczniki wydajności.  

### <a name="data-collection-from-windows-vm"></a>Zbieranie danych z maszyny Wirtualnej systemu Windows
1. Wybierz **Zaawansowane ustawienia**.<br> ![Ustawienia zaawansowane analizy dzienników](media/log-analytics-quick-collect-azurevm/log-analytics-advanced-settings-01.png)<br> 
3. Wybierz **danych**, a następnie wybierz **dzienniki zdarzeń systemu Windows**.  
4. Możesz dodać dziennika zdarzeń, wpisując nazwę dziennika.  Typ **systemu** , a następnie kliknij znak plus  **+** .  
5. W tabeli, sprawdź wag **błąd** i **ostrzeżenie**.   
6. Kliknij przycisk **zapisać** w górnej części strony, aby zapisać konfigurację.
7. Wybierz **danych wydajności systemu Windows** można włączyć kolekcji liczników wydajności na komputerze z systemem Windows. 
8. Podczas pierwszej konfiguracji liczników wydajności systemu Windows dla nowego obszaru roboczego analizy dzienników, podano opcję, aby szybko utworzyć kilka typowych liczników. Są one wyświetlane pole wyboru obok każdego.<br> ![Liczniki wydajności systemu Windows domyślne wybrane](media/log-analytics-quick-collect-azurevm/windows-perfcounters-default.png).<br> Kliknij przycisk **Dodaj wybrane liczniki wydajności**.  Są one dodawane i ustawienia wstępnego z interwału próbkowania sekundę dziesięć kolekcji.  
9. Kliknij przycisk **zapisać** w górnej części strony, aby zapisać konfigurację.

### <a name="data-collection-from-linux-vm"></a>Zbieranie danych z maszyny Wirtualnej systemu Linux

1. Wybierz **Syslog**.  
2. Możesz dodać dziennika zdarzeń, wpisując nazwę dziennika.  Typ **Syslog** , a następnie kliknij znak plus  **+** .  
3. W tabeli, usuń zaznaczenie pola wyboru wag **informacji**, **powiadomienia** i **debugowania**. 
4. Kliknij przycisk **zapisać** w górnej części strony, aby zapisać konfigurację.
5. Wybierz **dane wydajności systemu Linux** można włączyć kolekcji liczników wydajności na komputerze z systemem Windows. 
6. Podczas pierwszej konfiguracji liczników wydajności systemu Linux dla nowego obszaru roboczego analizy dzienników, podano opcję, aby szybko utworzyć kilka typowych liczników. Są one wyświetlane pole wyboru obok każdego.<br> ![Liczniki wydajności systemu Windows domyślne wybrane](media/log-analytics-quick-collect-azurevm/linux-perfcounters-default.png).<br> Kliknij przycisk **Dodaj wybrane liczniki wydajności**.  Są one dodawane i ustawienia wstępnego z interwału próbkowania sekundę dziesięć kolekcji.  
7. Kliknij przycisk **zapisać** w górnej części strony, aby zapisać konfigurację.

## <a name="view-data-collected"></a>Wyświetlanie danych zebranych
Teraz, gdy jest włączone zbieranie danych, pozwala uruchomić przykład wyszukiwania prostego dziennika, aby wyświetlić dane z docelowych maszyn wirtualnych.  

1. W portalu Azure przejdź do analizy dzienników i wybierz obszar roboczy utworzony wcześniej.
2. Kliknij przycisk **wyszukiwania dziennika** Kafelek i w okienku wyszukiwania dziennika, w polu wpisz kwerendę `Type=Perf` , a następnie naciśnij klawisz wprowadź lub kliknij przycisk wyszukiwania z prawej strony pola zapytania.<br> ![Przykład zapytania wyszukiwania dziennika analizy dzienników](./media/log-analytics-quick-collect-azurevm/log-analytics-portal-queryexample.png)<br> Na przykład 78,000 rekordów wydajności zwrócić zapytania na poniższej ilustracji.  Wyniki wyszukiwania będzie znacznie mniej.<br> ![Wynik wyszukiwania dziennika analizy dzienników](media/log-analytics-quick-collect-azurevm/log-analytics-search-perf.png)

## <a name="clean-up-resources"></a>Oczyszczanie zasobów
Gdy nie jest już potrzebny, Usuń obszar roboczy analizy dzienników. Aby to zrobić, wybierz obszar roboczy analizy dzienników utworzono wcześniej i kliknij stronę zasobów **usunąć**.<br> ![Usuń zasób analizy dzienników](media/log-analytics-quick-collect-azurevm/log-analytics-portal-delete-resource.png)

## <a name="next-steps"></a>Następne kroki
Teraz, kiedy są zbierane operacyjne i danych dotyczących wydajności z maszyn wirtualnych systemu Windows lub Linux, możesz z łatwością rozpocząć eksplorowanie, analizowanie i podjęcie działań na danych zbieranych dla *wolnego*.  

Aby dowiedzieć się, jak przeglądać i analizować dane, nadal samouczka.   

> [!div class="nextstepaction"]
> [Wyświetlanie i analizowanie danych analizy dzienników](log-analytics-tutorial-viewdata.md)
