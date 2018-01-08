---
title: "Rozwiązywanie problemów ze zmianą w maszynie wirtualnej platformy Azure | Microsoft Docs"
description: "Użyj śledzenia zmian, aby rozwiązać problemy ze zmianami w maszynie wirtualnej platformy Azure."
services: automation
keywords: "zmiana, śledzenie, automatyzacja"
author: jennyhunter-msft
ms.author: jehunte
ms.date: 12/14/2017
ms.topic: hero-article
ms.custom: mvc
manager: carmonm
ms.openlocfilehash: ae9ac6baaaeca418fcd3478145c50d1fa7917d7e
ms.sourcegitcommit: a648f9d7a502bfbab4cd89c9e25aa03d1a0c412b
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/22/2017
---
# <a name="troubleshoot-changes-in-your-environment"></a>Rozwiązywanie problemów ze zmianami we własnym środowisku

W tym samouczku dowiesz się jak rozwiązywać problemy ze zmianami w maszynie wirtualnej platformy Azure. Włączając śledzenie zmian, możesz śledzić zmiany w oprogramowaniu, plikach, demonach systemu Linux, usługach systemu Windows i kluczach rejestru systemu Windows znajdujących się na komputerach.
Identyfikowanie tych zmian konfiguracji może pomóc w określeniu problemów z działaniem w swoim środowisku.

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Dodawanie maszyny wirtualnej do śledzenia zmian i spisu
> * Wyszukiwanie dzienników zmian dla zatrzymanej usługi
> * Konfigurowanie śledzenia zmian
> * Włączanie połączenia dziennika aktywności
> * Wyzwalanie zdarzenia
> * Przeglądanie zmian

## <a name="prerequisites"></a>Wymagania wstępne

Do ukończenia tego samouczka niezbędne są następujące elementy:

* Subskrypcja platformy Azure. Jeśli nie masz subskrypcji, możesz [aktywować korzyści dla subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) lub utworzyć [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* [Konto automatyzacji](automation-offering-get-started.md) do przechowywania obserwatora i elementów Runbook akcji oraz zadania obserwatora.
* [Maszyna wirtualna](../virtual-machines/windows/quick-create-portal.md) do dołączenia.

## <a name="log-in-to-azure"></a>Zaloguj się do platformy Azure.

Zaloguj się w witrynie Azure Portal pod adresem http://portal.azure.com.

## <a name="enable-change-tracking-and-inventory"></a>Włączanie śledzenia zmian i spisu

Najpierw musisz włączyć śledzenie zmian i spisu dla maszyny wirtualnej w tym samouczku. Jeśli inne rozwiązanie automatyzacji zostało wcześniej włączone dla maszyny wirtualnej, ten krok nie jest konieczny.

1. W menu po lewej stronie wybierz pozycję **Maszyny wirtualne** i wybierz z listy maszynę wirtualną
1. W menu po lewej stronie w obszarze **Operacje** kliknij pozycję **Spis**. Zostanie otwarta strona **Włączanie śledzenia zmian i spisu**.

Jest przeprowadzana walidacja w celu ustalenia, czy śledzenie zmian i spis są włączone dla tej maszyny wirtualnej.
Walidacja obejmuje kontrole obszaru roboczego usługi Log Analytics i powiązanego konta usługi Automation i tego, czy rozwiązanie znajduje się w obszarze roboczym.

Obszar roboczy usługi [Log Analytics](../log-analytics/log-analytics-overview.md?toc=%2fazure%2fautomation%2ftoc.json) służy do zbierania danych generowanych przez funkcje i usługi, takie jak spis.
Obszar roboczy zawiera pojedynczą lokalizację do przeglądania i analizowania danych z wielu źródeł.

Proces walidacji sprawdza również, czy maszyna wirtualna jest aprowizowana za pomocą programu Microsoft Monitoring Agent (MMA) i hybrydowego procesu roboczego.
Ten agent jest używany do komunikacji z maszyną wirtualną i uzyskiwania informacji dotyczących zainstalowanego oprogramowania.
Proces walidacji sprawdza również, czy maszyna wirtualna jest aprowizowana za pomocą programu Microsoft Monitoring Agent (MMA) i hybrydowego procesu roboczego elementu Runbook usługi Automation.

Jeśli te wymagania wstępne nie są spełnione, zostanie wyświetlony transparent zawierający opcję włączenia rozwiązania.

![Transparent konfiguracji dołączony do śledzenia zmian i spisu](./media/automation-tutorial-troubleshoot-changes/enableinventory.png)

Kliknij transparent, aby włączyć rozwiązanie.
Jeśli którekolwiek z następujących wymagań wstępnych nie będzie występować po walidacji, zostanie ono automatycznie dodane:

* Obszar roboczy usługi [Log Analytics](../log-analytics/log-analytics-overview.md?toc=%2fazure%2fautomation%2ftoc.json)
* [Automatyzacja](./automation-offering-get-started.md)
* [Hybrydowy proces roboczy elementu Runbook](./automation-hybrid-runbook-worker.md) jest włączony na maszynie wirtualnej

Został otwarty ekran **Śledzenie zmian i spis**. Skonfiguruj lokalizację, obszar roboczy usługi Log Analytics i konto usługi Automation, a następnie kliknij pozycję **Włącz**. Jeśli pola są wygaszone, oznacza to, że inne rozwiązanie automatyzacji jest włączone dla maszyny wirtualnej, a tym samym należy użyć tego samego obszaru roboczego i konta automatyzacji.

![Okno włączania rozwiązania do śledzenia zmian](./media/automation-tutorial-troubleshoot-changes/installed-software-enable.png)

Włączanie rozwiązania może potrwać do 15 minut. W tym czasie nie należy zamykać okna przeglądarki.
Po włączeniu rozwiązania informacje dotyczące zainstalowanego oprogramowania i zmian w maszynie wirtualnej są przekazywane do usługi Log Analytics.
Udostępnienie danych do analizy może potrwać od 30 minut do 6 godzin.


## <a name="using-change-tracking-in-log-analytics"></a>Używanie śledzenia zmian w usłudze Log Analytics

Śledzenie zmian generuje dane dziennika, które są wysyłane do usługi Log Analytics. Aby wyszukiwać w dziennikach za pomocą uruchamiania zapytań, wybierz pozycję **Log Analytics** w górnej części okna **Śledzenie zmian**.
Dane śledzenia zmian są przechowywane w obszarze typu **ConfigurationChange**. Następujące przykładowe zapytanie usługi Log Analytics zwraca wszystkie usługi systemu Windows, które zostały zatrzymane.

```
ConfigurationChange
| where ConfigChangeType == "WindowsServices" and SvcState == "Stopped"
```

Aby dowiedzieć się więcej na temat uruchamiania i wyszukiwania plików dziennika w usłudze Log Analytics, zobacz [Azure Log Analytics](https://docs.loganalytics.io/index).

## <a name="configure-change-tracking"></a>Konfigurowanie śledzenia zmian

Śledzenie zmian daje możliwość śledzenia zmian konfiguracji na własnej maszynie wirtualnej. Poniższe kroki pokazują, jak skonfigurować śledzenie kluczy rejestru i plików.
 
Aby wybrać, które pliki i klucze rejestru mają być zbierane i śledzone, wybierz pozycję **Edytuj ustawienia** w górnej części strony **Śledzenie zmian**.

> [!NOTE]
> Spis i śledzenie zmian używają tych samych ustawień gromadzenia, a ustawienia zostaną skonfigurowane na poziomie obszaru roboczego.

W oknie **Konfiguracja obszaru roboczego** dodaj klucze rejestru systemu Windows, pliki systemu Windows lub pliki systemu Linux, które mają być śledzone, zgodnie z opisem w trzech kolejnych sekcjach.

### <a name="add-a-windows-registry-key"></a>Dodawanie klucza rejestru systemu Windows

1. Na karcie **Rejestr systemu Windows** wybierz opcję **Dodaj**.
    Zostanie otwarte okno **Dodawanie rejestru systemu Windows dla śledzenia zmian**.

   ![Śledzenie zmian dodawania do rejestru](./media/automation-vm-change-tracking/change-add-registry.png)

2. W obszarze **Włączone** wybierz pozycję **Prawda**.
3. W polu **Nazwa elementu** wprowadź przyjazną nazwę.
4. (Opcjonalnie) W polu **Grupa** wprowadź nazwę grupy.
5. W polu **Klucz rejestru systemu Windows** wpisz nazwę klucza rejestru, który ma być śledzony.
6. Wybierz pozycję **Zapisz**.

### <a name="add-a-windows-file"></a>Dodawanie pliku systemu Windows

1. Na karcie **Pliki systemu Windows** wybierz opcję **Dodaj**. Zostanie otwarte okno **Dodawanie pliku systemu Windows dla śledzenia zmian**.

   ![Śledzenie zmian dodawania pliku systemu Windows](./media/automation-vm-change-tracking/change-add-win-file.png)

2. W obszarze **Włączone** wybierz pozycję **Prawda**.
3. W polu **Nazwa elementu** wprowadź przyjazną nazwę.
4. (Opcjonalnie) W polu **Grupa** wprowadź nazwę grupy.
5. W polu **Wprowadź ścieżkę** wprowadź pełną ścieżkę i nazwę pliku, który ma być śledzony.
6. Wybierz pozycję **Zapisz**.

### <a name="add-a-linux-file"></a>Dodawanie pliku systemu Linux

1. Na karcie **Pliki systemu Linux** wybierz opcję **Dodaj**. Zostanie otwarte okno **Dodawanie pliku systemu Linux dla śledzenia zmian**.

   ![Śledzenie zmian dodawania pliku systemu Linux](./media/automation-vm-change-tracking/change-add-linux-file.png)

2. W obszarze **Włączone** wybierz pozycję **Prawda**.
3. W polu **Nazwa elementu** wprowadź przyjazną nazwę.
4. (Opcjonalnie) W polu **Grupa** wprowadź nazwę grupy.
5. W polu **Wprowadź ścieżkę** wprowadź pełną ścieżkę i nazwę pliku, który ma być śledzony.
6. W polu **Typ ścieżki** wybierz pozycję **Plik** albo **Katalog**.
7. W obszarze **Rekursja**, aby śledzić zmiany na określonej ścieżce oraz wszystkich plikach i ścieżkach poniżej niej, wybierz pozycję **Wł.**. Aby śledzić tylko wybraną ścieżkę lub plik, wybierz pozycję **Wył.**.
8. W obszarze **Użyj Sudo**, aby śledzić pliki, które wymagają wydania polecenia `sudo` do uzyskania dostępu, wybierz polecenie **Wł.**. W przeciwnym razie wybierz **Wył.**.
9. Wybierz pozycję **Zapisz**.

## <a name="enable-activity-log-connection"></a>Włączanie połączenia dziennika aktywności

Ze strony **Śledzenie zmian** na swojej maszynie wirtualnej wybierz pozycję **Zarządzanie połączeniem dziennika aktywności**. To zadanie powoduje otwarcie strony **Dziennik aktywności platformy Azure**. Wybierz pozycję **Połącz**, aby połączyć śledzenie zmian z dziennikiem aktywności platformy Azure dla Twojej maszyny wirtualnej.

Po włączeniu tego ustawienia przejdź do strony **Omówienie** dla maszyny wirtualnej i wybierz pozycję **Zatrzymaj**, aby zatrzymać swoją maszynę wirtualną. Po wyświetleniu monitu wybierz pozycję **Tak**, aby zatrzymać maszynę wirtualną. Po cofnięciu jej przydziału wybierz pozycję **Start**, aby ponownie uruchomić maszynę wirtualną.

Zatrzymanie i uruchomienie maszyny wirtualnej rejestruje zdarzenie w jego dzienniku aktywności. Przejdź z powrotem do strony **Śledzenie zmian**. Wybierz kartę **Zdarzenia** u dołu strony. Po chwili zdarzenia są wyświetlane na wykresie i w tabeli. Podobnie jak w poprzednim kroku, każde zdarzenie można wybrać, aby wyświetlić szczegółowe informacje o zdarzeniu.

![Wyświetlanie szczegółów zmiany w portalu](./media/automation-tutorial-troubleshoot-changes/viewevents.png)

## <a name="view-changes"></a>Przeglądanie zmian

Po włączeniu rozwiązania śledzenia zmian i spisu możesz wyświetlić wyniki na stronie **Śledzenie zmian**.

W ramach maszyny wirtualnej zaznacz pozycję **Śledzenie zmian** w obszarze **OPERACJE**.

![Tworzenie alertu w klasycznym portalu OMS](./media/automation-tutorial-troubleshoot-changes/change-tracking-list.png)

Wykres pokazuje zmiany, które wystąpiły w czasie.
Po dodaniu połączenia dziennika aktywności wykres liniowy u góry wyświetla zdarzenia dziennika aktywności platformy Azure.
Każdy wiersz wykresów słupkowych reprezentuje innego typu zmiany umożliwiające śledzenie.
Do tych typów należą demony systemu Linux, pliki, klucze rejestru systemu Windows, oprogramowanie i usługi systemu Windows.
Karta zmiany przedstawia szczegółowe informacje dla zmian pokazanych w wizualizacji w kolejności malejącej według czasu, kiedy wystąpiła zmiana (najnowsze na początku).
Na karcie **Zdarzenia** są wyświetlane w tabeli zdarzenia połączonego dziennika aktywności wraz z ich odpowiednimi szczegółami, przy czym najnowsze są na początku.

W wynikach możesz zobaczyć, że w systemie było wiele zmian łącznie ze zmianami usług i oprogramowania. Możesz użyć filtrów u góry strony, aby przefiltrować wyniki według **Typu zmiany** lub zakresu czasu.

Wybierz zmianę **WindowsServices**, a spowoduje to otwarcie okna **Szczegóły zmiany**. Okno Szczegóły zmiany zawiera szczegółowe informacje o zmianie oraz wartości przed i po zmianie. W tym wystąpieniu zatrzymano usługę ochrony oprogramowania.

![Wyświetlanie szczegółów zmiany w portalu](./media/automation-tutorial-troubleshoot-changes/change-details.png)

## <a name="next-steps"></a>Następne kroki

W tym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Dodawanie maszyny wirtualnej do śledzenia zmian i spisu
> * Wyszukiwanie dzienników zmian dla zatrzymanej usługi
> * Konfigurowanie śledzenia zmian
> * Włączanie połączenia dziennika aktywności
> * Wyzwalanie zdarzenia
> * Przeglądanie zmian

Przejdź do omówienia rozwiązania śledzenia zmian i spisu, aby uzyskać więcej informacji.

> [!div class="nextstepaction"]
> [Change management and Inventory solution](../log-analytics/log-analytics-change-tracking.md?toc=%2fazure%2fautomation%2ftoc.json) (Rozwiązanie zarządzania zmianami i spisem)
