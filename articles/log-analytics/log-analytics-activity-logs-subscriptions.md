---
title: "Zbieranie dzienników aktywności platformy Azure do analizy dzienników w subskrypcjach | Dokumentacja firmy Microsoft"
description: "Użyj centra zdarzeń i Logic Apps do zbierania danych z dziennika aktywności platformy Azure i wysyłania go z obszarem roboczym usługi Analiza dzienników Azure w innej dzierżawie."
services: log-analytics, logic-apps, event-hubs
documentationcenter: 
author: richrundmsft
manager: carmonm
editor: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/08/2018
ms.author: richrund; bwren
ms.openlocfilehash: ded0b4cdcbac747d52435023a24b5719f3c58758
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="collect-azure-activity-logs-into-log-analytics-across-subscriptions"></a>Zbieranie dzienników aktywności platformy Azure do analizy dzienników różnych subskrypcji

Ten artykuł czynności za pomocą metody do zbierania dzienników aktywności platformy Azure w obszarze roboczym analizy dzienników dla usługi Logic Apps za pomocą łącznika zbierający dane analizy dziennika Azure. Użyj procesu w tym artykule, gdy chcesz wysłać dzienniki do obszaru roboczego w innej usłudze Azure Active Directory. Na przykład jeśli jesteś dostawcą usługi zarządzanej, można zbierać Dzienniki aktywności z subskrypcją danego klienta i przechowywać je w obszarze roboczym analizy dzienników w ramach własnej subskrypcji.

Jeśli obszar roboczy analizy dzienników jest w tej samej subskrypcji platformy Azure lub w innej subskrypcji, ale w tej samej usługi Azure Active Directory, wykonaj kroki w [rozwiązania Dziennik aktywności platformy Azure](../log-analytics/log-analytics-activity.md) zbierać Dzienniki aktywności platformy Azure.

## <a name="overview"></a>Przegląd

Strategia używana w tym scenariuszu ma wysyłania zdarzeń do dziennika aktywności platformy Azure [Centrum zdarzeń](../event-hubs/event-hubs-what-is-event-hubs.md) gdzie [aplikacji logiki](../logic-apps/logic-apps-overview.md) wysyła je do obszaru roboczego analizy dzienników. 

![Obraz przepływu danych z analizy dziennika do dziennika aktywności](media/log-analytics-activity-logs-subscriptions/data-flow-overview.png)

Zalety tego podejścia obejmują:
- Małe opóźnienia, ponieważ dziennik aktywności platformy Azure jest przesyłana strumieniowo do Centrum zdarzeń.  Aplikacja logiki następnie wyzwoleniu i przesyła dane do analizy dzienników. 
- Minimalny wymagany jest kod i nie bez serwera infrastruktury do wdrożenia.

Ten artykuł zawiera kroki jak:
1. Tworzenie Centrum zdarzeń. 
2. Wyeksportuj Dzienniki aktywności do Centrum zdarzeń za pomocą profilu Eksport dziennika aktywności platformy Azure.
3. Tworzenie aplikacji logiki do odczytywania z Centrum zdarzeń i wysyłania zdarzeń do analizy dzienników.

## <a name="requirements"></a>Wymagania
Poniżej przedstawiono wymagania dotyczące zasobów platformy Azure, w tym scenariuszu.

- Przestrzeń nazw Centrum zdarzeń nie ma w tej samej subskrypcji co emitowanie dzienniki subskrypcji. Użytkownik, który konfiguruje ustawienie musi mieć odpowiednie uprawnienia dostępu do obu subskrypcji. Jeśli masz wiele subskrypcji w tej samej usłudze Azure Active directory, możesz wysłać Dzienniki aktywności dla wszystkich subskrypcji do jednego zdarzenia koncentratora.
- Aplikację logiki może znajdować się w innej subskrypcji z Centrum zdarzeń i musi być w tej samej usługi Azure Active Directory. Odczytuje aplikacji logiki z Centrum zdarzeń za pomocą klucza dostępu współdzielonego Centrum zdarzeń.
- Obszar roboczy analizy dzienników mogą znajdować się w innej subskrypcji i usługi Azure Active Directory z aplikacji logiki, ale dla uproszczenia zaleca się, że są one w tej samej subskrypcji. Aplikację logiki wysyła do analizy dzienników przy użyciu klucza i identyfikator obszaru roboczego analizy dzienników.



## <a name="step-1---create-an-event-hub"></a>Krok 1 — tworzenia Centrum zdarzeń

<!-- Follow the steps in [how to create an Event Hubs namespace and Event Hub](../event-hubs/event-hubs-create.md) to create your event hub. -->

1. W portalu Azure wybierz **nowy**> **Internetu rzeczy** > **usługi Event Hubs**.

   ![Nowe Centrum zdarzeń Marketplace](media/log-analytics-activity-logs-subscriptions/marketplace-new-event-hub.png)

3. W obszarze **tworzenie przestrzeni nazw**, albo wprowadź nowy obszar nazw lub wybierz istniejący. System od razu sprawdza, czy nazwa jest dostępna.

   ![Obraz tworzenia Centrum zdarzeń w oknie dialogowym](media/log-analytics-activity-logs-subscriptions/create-event-hub1.png)

4. Wybierz warstwę cenową (Basic lub Standard), subskrypcji platformy Azure, grupy zasobów i lokalizację dla nowego zasobu.  Kliknij pozycję **Utwórz**, aby utworzyć przestrzeń nazw. Może być konieczne kilkuminutowe do zapewnienia pełnej zasobów systemu.
6. Kliknij przestrzeń nazw utworzoną z listy.
7. Wybierz **zasady dostępu współużytkowanego**, a następnie kliknij przycisk **RootManageSharedAccessKey**.

   ![Obraz programu zasady dostępu udostępnionego Centrum zdarzeń](media/log-analytics-activity-logs-subscriptions/create-event-hub7.png)
   
8. Kliknij przycisk kopiowania, aby skopiować parametry połączenia **RootManageSharedAccessKey** do schowka. 

   ![Obraz klucza dostępu współdzielonego Centrum zdarzeń](media/log-analytics-activity-logs-subscriptions/create-event-hub8.png)

9. W lokalizacji tymczasowej, takiego jak Notatnik przechowywać kopię nazwę Centrum zdarzeń i albo podstawowy lub pomocniczy Centrum zdarzeń parametry połączenia. Aplikacja logiki wymaga tych wartości.  Parametry połączenia Centrum zdarzeń, można użyć **RootManageSharedAccessKey** połączenia string lub Utwórz oddzielne.  Parametry połączenia używane musi rozpoczynać się od `Endpoint=sb://` i zasady, która ma **Zarządzaj** zasady dostępu.


## <a name="step-2---export-activity-logs-to-event-hub"></a>Krok 2 — Dzienniki aktywności eksportu do Centrum zdarzeń

Aby włączyć przesyłanie strumieniowe dziennik aktywności, wybierz Namespace Centrum zdarzeń i zasady dostępu współdzielonego dla tej przestrzeni nazw. Centrum zdarzeń jest tworzony w tej przestrzeni nazw, po wystąpieniu zdarzenia pierwszy nowy dziennik aktywności. 

Można użyć do przestrzeni nazw Centrum zdarzeń, która nie znajduje się w tej samej subskrypcji co subskrypcji emitowanie dzienniki, jednak subskrypcji musi należeć do tej samej usługi Azure Active Directory. Użytkownik, który konfiguruje ustawienie musi mieć odpowiednie RBAC dostępu obie subskrypcje do. 

1. W portalu Azure wybierz **Monitor** > **dziennik aktywności**.
3. Kliknij przycisk **wyeksportować** u góry strony.

   ![Obraz monitora azure w nawigacji](media/log-analytics-activity-logs-subscriptions/activity-log-blade.png)

4. Wybierz **subskrypcji** wyeksportować z, a następnie kliknij przycisk **Zaznacz wszystko** w **regionów** listy rozwijanej wybierz zdarzenia dla zasobów we wszystkich regionach. Kliknij przycisk **wyeksportować do Centrum zdarzeń** pole wyboru.
7. Kliknij przycisk **przestrzeń nazw magistrali usług**, a następnie wybierz **subskrypcji** z Centrum zdarzeń **przestrzeni nazw zdarzenia koncentratora**i **nazwy zasad Centrum zdarzeń**.

    ![Obraz eksportu do strony Centrum zdarzeń](media/log-analytics-activity-logs-subscriptions/export-activity-log2.png)

11. Kliknij przycisk **OK** , a następnie **zapisać** Aby zapisać te ustawienia. Ustawienia są zastosowane natychmiast do subskrypcji.

<!-- Follow the steps in [stream the Azure Activity Log to Event Hubs](../monitoring-and-diagnostics/monitoring-stream-activity-logs-event-hubs.md) to configure a log profile that writes activity logs to an event hub. -->

## <a name="step-3---create-logic-app"></a>Krok 3 — Tworzenie aplikacji logiki

Po Dzienniki aktywności pisania do Centrum zdarzeń, tworzenia aplikacji logiki do zbierania dzienników z Centrum zdarzeń i zapisanie ich do analizy dzienników.

Aplikację logiki obejmuje następujące funkcje:
- [Centrum zdarzeń łącznika](https://docs.microsoft.com/connectors/eventhubs/) trigger, aby odczytać z Centrum zdarzeń.
- A [akcji przeanalizować JSON](../logic-apps/logic-apps-content-type.md) wyodrębnić zdarzenia JSON.
- A [tworzą akcji](../logic-apps/logic-apps-workflow-actions-triggers.md#compose-action) do konwersji kodu JSON do obiektu.
- A [analizy dzienników wysyłania danych łącznika](https://docs.microsoft.com/connectors/azureloganalyticsdatacollector/) do wysłania danych do analizy dzienników.

   ![Obraz Dodawanie wyzwalacz Centrum zdarzeń w aplikacjach logiki](media/log-analytics-activity-logs-subscriptions/log-analytics-logic-apps-activity-log-overview.png)

### <a name="logic-app-requirements"></a>Wymagania dotyczące aplikacji logiki
Przed utworzeniem aplikacji logiki, upewnij się, że masz następujące informacje z poprzednich kroków:
- Nazwa centrum zdarzeń
- Zdarzenia koncentratora parametry połączenia (podstawowej lub pomocniczej) dla przestrzeni nazw Centrum zdarzeń.
- Identyfikator obszaru roboczego analizy dzienników
- Analiza dzienników wspólny klucz

Aby uzyskać ciąg połączenia i nazwę Centrum zdarzeń, postępuj zgodnie z instrukcjami [uprawnień przestrzeni nazw Sprawdź centra zdarzeń i Znajdź parametry połączenia](../connectors/connectors-create-api-azure-event-hubs.md#check-event-hubs-namespace-permissions-and-find-the-connection-string).


### <a name="create-a-new-blank-logic-app"></a>Utwórz nową aplikację logiki puste

1. W portalu Azure wybierz **Utwórz zasób** > **integracji przedsiębiorstwa** > **aplikacji logiki**.

    ![Marketplace nowej aplikacji logiki](media/log-analytics-activity-logs-subscriptions/marketplace-new-logic-app.png)

2. Wprowadź ustawienia w poniższej tabeli.

    ![Tworzenie aplikacji logiki](media/log-analytics-activity-logs-subscriptions/create-logic-app.png)

   |Ustawienie | Opis  |
   |:---|:---|
   | Name (Nazwa)           | Unikatowa nazwa aplikacji logiki. |
   | Subskrypcja   | Wybierz subskrypcję platformy Azure, która będzie zawierać aplikacji logiki. |
   | Grupa zasobów | Wybierz istniejącą grupę zasobów platformy Azure lub Utwórz nową aplikację logiki. |
   | Lokalizacja       | Wybierz region centrum danych do wdrażania swojej aplikacji logiki. |
   | Log Analytics  | Wybierz, jeśli chcesz rejestrować stan każdym uruchomieniu aplikacji logiki w analizy dzienników.  |

    
3. Wybierz pozycję **Utwórz**. Gdy **Zakończono pomyślnie wdrażanie** wyświetla powiadomienie, kliknij na **przejdź do zasobu** otworzyć aplikację logiki.

4. W obszarze **Szablony** wybierz pozycję **Pusta aplikacja logiki**. 

Projektant aplikacji logiki teraz przedstawiono dostępne łączniki i ich wyzwalaczy, których używasz do uruchamiania przepływu pracy aplikacji logiki.

<!-- Learn [how to create a logic app](../logic-apps/quickstart-create-first-logic-app-workflow.md). -->

### <a name="add-event-hub-trigger"></a>Dodaj wyzwalacza Centrum zdarzeń

1. W polu wyszukiwania dla projektanta aplikacji logiki, wpisz *usługi event hubs* filtru. Wybierz wyzwalacz **usługi Event Hubs — gdy zdarzenia są dostępne w Centrum zdarzeń**.

   ![Obraz Dodawanie wyzwalacz Centrum zdarzeń w aplikacjach logiki](media/log-analytics-activity-logs-subscriptions/logic-apps-event-hub-add-trigger.png)

2. Jeśli zostanie wyświetlony monit o poświadczenia, połączenie z przestrzeni nazw usługi Event Hubs. Wprowadź nazwę dla połączenia, a następnie parametry połączenia, które zostały skopiowane.  Wybierz pozycję **Utwórz**.

   ![Dodawanie połączenia Centrum zdarzeń w aplikacjach logiki obraz](media/log-analytics-activity-logs-subscriptions/logic-apps-event-hub-add-connection.png)

3. Po utworzeniu połączenia, należy edytować ustawienia wyzwalacza. Najpierw wybrać **insights operacyjne — dzienniki** z **nazwę Centrum zdarzeń** listy rozwijanej.

   ![Jeśli zdarzenia są dostępne w oknie dialogowym](media/log-analytics-activity-logs-subscriptions/logic-apps-event-hub-read-events.png)

5. Rozwiń węzeł **Pokaż zaawansowane opcje** i zmienić **typ zawartości** do *application/json*

   ![Okno dialogowe konfiguracji Centrum zdarzeń](media/log-analytics-activity-logs-subscriptions/logic-apps-event-hub-configuration.png)

### <a name="add-parse-json-action"></a>Dodaj akcję przeanalizować JSON

Dane wyjściowe z Centrum zdarzeń zawiera ładunek JSON z tabeli rekordów. [Przeanalizować JSON](../logic-apps/logic-apps-content-type.md) akcji jest używany do wyodrębniania właśnie tablicy rekordów do wysyłania do analizy dzienników.

1. Kliknij przycisk **nowy krok** > **Dodaj akcję**
2. W polu wyszukiwania wpisz *przeanalizować składni json* filtru. Wybierz akcję **operacji danych - przeanalizować JSON**.

   ![Dodanie parse json akcji w aplikacjach logiki](media/log-analytics-activity-logs-subscriptions/logic-apps-add-parse-json-action.png)

3. Kliknij **zawartości** a następnie wybierz opcję *treści*.

4. Skopiuj i wklej następujący schematu do **schematu** pola.  Ten schemat reprezentuje wyjście z akcji Centrum zdarzeń.  

   ![Przeanalizować json okna dialogowego](media/log-analytics-activity-logs-subscriptions/logic-apps-parse-json-configuration.png)

``` json-schema
{
    "properties": {
        "body": {
            "properties": {
                "ContentData": {
                    "type": "string"
                },
                "Properties": {
                    "properties": {
                        "ProfileName": {
                            "type": "string"
                        },
                        "x-opt-enqueued-time": {
                            "type": "string"
                        },
                        "x-opt-offset": {
                            "type": "string"
                        },
                        "x-opt-sequence-number": {
                            "type": "number"
                        }
                    },
                    "type": "object"
                },
                "SystemProperties": {
                    "properties": {
                        "EnqueuedTimeUtc": {
                            "type": "string"
                        },
                        "Offset": {
                            "type": "string"
                        },
                        "PartitionKey": {},
                        "SequenceNumber": {
                            "type": "number"
                        }
                    },
                    "type": "object"
                }
            },
            "type": "object"
        },
        "headers": {
            "properties": {
                "Cache-Control": {
                    "type": "string"
                },
                "Content-Length": {
                    "type": "string"
                },
                "Content-Type": {
                    "type": "string"
                },
                "Date": {
                    "type": "string"
                },
                "Expires": {
                    "type": "string"
                },
                "Location": {
                    "type": "string"
                },
                "Pragma": {
                    "type": "string"
                },
                "Retry-After": {
                    "type": "string"
                },
                "Timing-Allow-Origin": {
                    "type": "string"
                },
                "Transfer-Encoding": {
                    "type": "string"
                },
                "Vary": {
                    "type": "string"
                },
                "X-AspNet-Version": {
                    "type": "string"
                },
                "X-Powered-By": {
                    "type": "string"
                },
                "x-ms-request-id": {
                    "type": "string"
                }
            },
            "type": "object"
        }
    },
    "type": "object"
}
```

>[!TIP]
> Ładunek próbki można uzyskać, klikając **Uruchom** i patrzeć **nieprzetworzone dane wyjściowe** z Centrum zdarzeń.  Następnie za pomocą tego raportu można z **ładunku próbki używany do generowania schematu** w **przeanalizować JSON** działania do generowania schematu.

### <a name="add-compose-action"></a>Dodaj akcję tworzenia
[Redaguj](../logic-apps/logic-apps-workflow-actions-triggers.md#compose-action) Akcja pobiera dane wyjściowe JSON i tworzy obiekt, który mogą być używane przez akcję analizy dzienników.

1. Kliknij przycisk **nowy krok** > **Dodaj akcję**
2. Typ *tworzą* dla filtru, a następnie wybierz akcję **tworzą operacje na danych -**.

    ![Dodaj akcję tworzenia](media/log-analytics-activity-logs-subscriptions/logic-apps-add-compose-action.png)

3. Kliknij przycisk **dane wejściowe** i wybierz pozycję **treści** w obszarze **przeanalizować JSON** działania.


### <a name="add-log-analytics-send-data-action"></a>Dodaj akcję Wyślij dane analizy dziennika
[Zbierający dane analizy dziennika Azure](https://docs.microsoft.com/connectors/azureloganalyticsdatacollector/) akcji obiekt z operacji tworzenia i wysyłania go do analizy dzienników.

1. Kliknij przycisk **nowy krok** > **Dodaj akcję**
2. Typ *dziennika analizy* dla filtru, a następnie wybierz akcję **zbierający dane analizy dziennika Azure — Wyślij dane**.

   ![Dodawanie analizy dzienników wysłanie danych w aplikacjach logiki](media/log-analytics-activity-logs-subscriptions/logic-apps-send-data-to-log-analytics-connector.png)

3. Wprowadź nazwę połączenia i Wklej w **identyfikator obszaru roboczego** i **klucz obszaru roboczego** obszaru roboczego analizy dzienników.  Kliknij przycisk **Utwórz**.

   ![Dodawanie połączenia analizy dziennika w aplikacjach logiki](media/log-analytics-activity-logs-subscriptions/logic-apps-log-analytics-add-connection.png)

4. Po utworzeniu połączenia, Edytuj ustawienia w poniższej tabeli. 

    ![Skonfiguruj wysyłanie danych akcji](media/log-analytics-activity-logs-subscriptions/logic-apps-send-data-to-log-analytics-configuration.png)

   |Ustawienie        | Wartość           | Opis  |
   |---------------|---------------------------|--------------|
   |Treść żądania JSON  | **Dane wyjściowe** z **Redaguj** akcji | Pobiera rekordy z treści akcji tworzenia. |
   | Nazwa dziennika niestandardowego | AzureActivity | Nazwa tabeli dziennika niestandardowego można utworzyć w analizy dzienników do przechowywania importowanych danych. |
   | Time-generated-field | time | Nie zaznaczaj pola JSON dla **czas** — po prostu wpisz wyrazy czas. Po zaznaczeniu pola JSON projektanta umieszcza **Wyślij dane** akcję *dla każdego* pętli, której nie chcesz. |




10. Kliknij przycisk **zapisać** Aby zapisać zmiany wprowadzone do aplikacji logiki.

## <a name="step-4---test-and-troubleshoot-the-logic-app"></a>Krok 4. badanie i rozwiązywanie problemów aplikacji logiki
Z ukończenia przepływu pracy można sprawdzić w projektancie, aby sprawdzić, czy działa bez błędów.

W Projektancie aplikacji logiki, kliknij przycisk **Uruchom** do testowania aplikacji logiki. Każdy krok w aplikacji logiki pokazuje ikona stanu z białym znacznikiem wyboru w zielone koło informujący o powodzeniu.

   ![Przetestuj aplikację logiki](media/log-analytics-activity-logs-subscriptions/test-logic-app.png)

Aby wyświetlić szczegółowe informacje o każdym kroku, kliknij nazwę krok, aby go rozwinąć. Polecenie **Pokaż nieprzetworzone dane wejściowe** i **Pokaż nieprzetworzone dane wyjściowe** Aby uzyskać więcej informacji o danych odebranych i wysłanych w każdym kroku.

## <a name="step-5---view-azure-activity-log-in-log-analytics"></a>Krok 5 — Przeglądanie dziennika aktywności platformy Azure Log Analytics
Ostatnim krokiem jest do sprawdzenia obszaru roboczego analizy dzienników, aby upewnić się, że dane są zbierane zgodnie z oczekiwaniami.

1. W portalu Azure wybierz **analizy dzienników**.
2. Wybierz obszar roboczy, a następnie **wyszukiwania dziennika** kafelka.
3. Na pasku zapytania wyszukiwania wpisz `AzureActivity_CL` i kliknij przycisk wyszukiwania. Jeśli nie nazwa dziennika niestandardowego *AzureActivity*, wpisz wybraną nazwę i Dołącz `_CL`.

>[!NOTE]
> Nowy dziennik niestandardowy jest wysyłane do analizy dzienników po raz pierwszy może potrwać do godziny dziennik niestandardowy wyszukiwania.

>[!NOTE]
> Dzienniki aktywności są zapisywane w niestandardowych tabeli i nie wyświetlaj w [rozwiązania Dziennik aktywności](./log-analytics-activity.md).


![Przetestuj aplikację logiki](media/log-analytics-activity-logs-subscriptions/log-analytics-results.png)

## <a name="next-steps"></a>Kolejne kroki

W tym artykule zostanie utworzona aplikacja logiki, aby odczytać Dzienniki aktywności platformy Azure z Centrum zdarzeń i wysyłać je do analizy dzienników dla analizy. Aby dowiedzieć się więcej o wizualizacji danych analizy dzienników, w tym tworzenie pulpitów nawigacyjnych, przejrzyj samouczek dotyczący Wizualizacja danych.

> [!div class="nextstepaction"]
> [Wizualizuj samouczek danych wyszukiwania dziennika](./log-analytics-tutorial-dashboards.md)
