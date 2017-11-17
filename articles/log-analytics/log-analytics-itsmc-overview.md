---
title: "IT usługi łącznika zarządzania w programie Azure Log Analytics | Dokumentacja firmy Microsoft"
description: "Ten artykuł zawiera omówienie IT usługi zarządzania łącznika (ITSMC) i informacje o sposobie używania tego rozwiązania, aby centralnie monitorować i zarządzać nimi Zarządzanie usługami IT — elementów roboczych w analizy dzienników OMS i szybkie rozwiązywanie problemów."
services: log-analytics
documentationcenter: 
author: JYOTHIRMAISURI
manager: riyazp
editor: 
ms.assetid: 0b1414d9-b0a7-4e4e-a652-d3a6ff1118c4
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2017
ms.author: v-jysur
ms.openlocfilehash: bd384255b3c46b3ae88b1269ab26e0ddaa6f6e77
ms.sourcegitcommit: 7d107bb9768b7f32ec5d93ae6ede40899cbaa894
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/16/2017
---
# <a name="centrally-manage-itsm-work-items-using-it-service-management-connector-preview"></a>Centralne zarządzanie Zarządzanie usługami IT — elementów roboczych za pomocą łącznika zarządzania usługi IT (wersja zapoznawcza)

![Symbol łącznika usługi zarządzania IT](./media/log-analytics-itsmc/itsmc-symbol.png)

Łącznik zarządzania usługi IT (ITSMC) zapewnia dwukierunkową integrację między obsługiwane usługi produktów zarządzania usługi IT (zarządzanie, usługami IT —) i analizy dzienników.  Za pośrednictwem tego połączenia można utworzyć zdarzenia i alerty lub zdarzenia w produkcie Zarządzanie usługami IT — na podstawie analizy dzienników alerty, rekordów dziennika lub alerty platformy Azure. Łącznik również importuje dane takie jak zdarzenia i żądania zmiany z produktu Zarządzanie usługami IT — do analizy dzienników OMS.

ITSMC można:

  - Integracja operacyjnej alerty z swoje praktyki zarządzania zdarzeniami w narzędziu Zarządzanie usługami IT — wybranych przez użytkownika.
    - Tworzenie elementów roboczych (na przykład alert, zdarzenie, zdarzenia) w zarządzanie usługami IT —, OMS alertów i za pomocą wyszukiwania dziennika.
    - Tworzenie elementów roboczych na podstawie alertów programu dziennika aktywności platformy Azure za pomocą akcji Zarządzanie usługami IT — w grupach akcji.

  - Ujednolicenie monitorowania, dzienników i danych zarządzania usługi używane w organizacji.
    - Skorelować zdarzenia i zmień dane żądania z Twojej Zarządzanie usługami IT — narzędzi z właściwymi danymi dziennika w obszarze roboczym analizy dzienników.   
    - Wyświetlanie najwyższego poziomu pulpity nawigacyjne omówienie na incydenty, żądania zmiany i wpływ na systemy.
    - Pisanie zapytań analizy dzienników, aby uzyskać wgląd w dane usługi zarządzania.

## <a name="adding-the-it-service-management-connector-solution"></a>Dodawanie IT usługi rozwiązania do zarządzania łącznika

Dodaj rozwiązanie IT usługi zarządzania łącznika do obszaru roboczego analizy dzienników przy użyciu procesu opisanego w [rozwiązań dodać analizy dzienników z galerii rozwiązań](log-analytics-add-solutions.md).

Oto fragment ITSMC pojawi się w galerii rozwiązań:

![Łącznik kafelka](./media/log-analytics-itsmc/itsmc-solutions-tile.png)

Po pomyślnym dodaniu zobaczysz łącznika zarządzania usługi IT w obszarze **OMS** > **ustawienia** > **połączonych źródeł.**

![ITSMC połączone](./media/log-analytics-itsmc/itsmc-overview-solution-in-connected-sources.png)

> [!NOTE]

> Domyślnie ITSMC odświeża dane połączenia raz w co 24 godziny. Aby odświeżyć tego połączenia danych natychmiast dla wszystkich edycji lub szablonu aktualizacje, które zostaną wprowadzone, kliknij przycisk "Odśwież", wyświetlane obok połączenia.

 ![Odśwież ITSMC](./media/log-analytics-itsmc/itsmc-connection-refresh.png)


## <a name="configuring-the-itsmc-connection-with-your-itsm-productsservices"></a>Konfigurowanie połączenia ITSMC z Zarządzanie usługami IT — produktów/usług

ITSMC obsługuje połączenia z **System Center Service Manager**, **ServiceNow**, **Provance**, i **Cherwell**.

Użyj następujących procedur jako odpowiednie dla Ciebie:

- [Program System Center Service Manager (SCSM)](log-analytics-itsmc-connections.md#connect-system-center-service-manager-to-it-service-management-connector-in-oms)

- [Usługi ServiceNow](log-analytics-itsmc-connections.md#connect-servicenow-to-it-service-management-connector-in-oms)

- [Provance](log-analytics-itsmc-connections.md#connect-provance-to-it-service-management-connector-in-oms)  

- [Cherwell](log-analytics-itsmc-connections.md#connect-cherwell-to-it-service-management-connector-in-oms)

## <a name="using-the-solution"></a>Użycie rozwiązania

Po skonfigurowaniu łącznika rozpoczyna zbieranie danych z połączonych Zarządzanie usługami IT — produktu lub usługi. W zależności od liczby zdarzenia i żądania zmiany w zarządzanie usługami IT — produktu lub usługi początkowej synchronizacji należy wykonać w kilka minut.

> [!NOTE]
> - Dane importowane z produktu Zarządzanie usługami IT — przez rozwiązanie ITSMC pojawia się w analizy dzienników jako rekord dziennika typu **ServiceDesk_CL**.
> - Rekord dziennika zawiera pole o nazwie **ServiceDeskWorkItemType_s**, która jest albo zdarzeniem lub żądaniem zmiany, dwa rodzaje danych zaimportowane z produktu Zarządzanie usługami IT —.

## <a name="data-synced-from-itsm-product"></a>Synchronizowane z produktu Zarządzanie usługami IT — danych
Zdarzenia i żądania zmiany są synchronizowane z produktu Zarządzanie usługami IT — do obszaru roboczego analizy dzienników.
Poniższe informacje przedstawiono przykłady danych zbieranych przez ITSMC:

> [!NOTE]

> W zależności od typu elementu roboczego zaimportowane do analizy dzienników **ServiceDesk_CL** zawiera następujące pola:

**Element pracy:** **zdarzenia**  
ServiceDeskWorkItemType_s = "Zdarzenie"

**Pola**

- ServiceDeskConnectionName
- Identyfikator technicznej usługi
- Stan
- Pilności
- Wpływ
- Priorytet
- Eskalacja
- Utworzone przez
- Rozwiązany przez
- Zamknięte przez
- Źródło
- Przypisano do
- Kategoria
- Stanowisko
- Opis
- Data utworzenia
- Data zamknięcia
- Data rozwiązania
- Data ostatniej modyfikacji
- Komputer


**Element pracy:** **żądania zmiany**

ServiceDeskWorkItemType_s = "Żądanie zmiany"

**Pola**
- ServiceDeskConnectionName
- Identyfikator technicznej usługi
- Utworzone przez
- Zamknięte przez
- Źródło
- Przypisano do
- Stanowisko
- Typ
- Kategoria
- Stan
- Eskalacja
- Konflikt stanu
- Pilności
- Priorytet
- Ryzyko
- Wpływ
- Przypisano do
- Data utworzenia
- Data zamknięcia
- Data ostatniej modyfikacji
- Żądana data
- Planowana data rozpoczęcia
- Planowana data zakończenia
- Data rozpoczęcia pracy
- Data zakończenia pracy
- Opis
- Komputer

## <a name="output-data-for-a-servicenow-incident"></a>Dane wyjściowe dla zdarzenia usługi ServiceNow

| Pole OMS | Zarządzanie usługami IT — pola |
|:--- |:--- |
| ServiceDeskId_s| Liczba |
| IncidentState_s | Stan |
| Urgency_s |Pilności |
| Impact_s |Wpływ|
| Priority_s | Priorytet |
| CreatedBy_s | Otwarty przez |
| ResolvedBy_s | Rozwiązany przez|
| ClosedBy_s  | Zamknięte przez |
| Source_s| Skontaktuj się z typu |
| AssignedTo_s | Przypisane do  |
| Category_s | Kategoria |
| Title_s|  Krótki opis |
| Description_s|  Uwagi |
| CreatedDate_t|  Otwierano |
| ClosedDate_t| zamknięte|
| ResolvedDate_t|Rozwiązane|
| Komputer  | element konfiguracji |

## <a name="output-data-for-a-servicenow-change-request"></a>Dane wyjściowe dla usługi ServiceNow żądania zmiany

| Pole OMS | Zarządzanie usługami IT — pola |
|:--- |:--- |
| ServiceDeskId_s| Liczba |
| CreatedBy_s | Żądane przez |
| ClosedBy_s | Zamknięte przez |
| AssignedTo_s | Przypisane do  |
| Title_s|  Krótki opis |
| Type_s|  Typ |
| Category_s|  Kategoria |
| CRState_s|  Stan|
| Urgency_s|  Pilności |
| Priority_s| Priorytet|
| Risk_s| Ryzyko|
| Impact_s| Wpływ|
| RequestedDate_t  | Żądana według daty |
| ClosedDate_t | Data zamknięcia |
| PlannedStartDate_t  |     Planowana data rozpoczęcia |
| PlannedEndDate_t  |   Planowana data zakończenia |
| WorkStartDate_t  | Rzeczywista data rozpoczęcia |
| WorkEndDate_t | Rzeczywista data zakończenia|
| Description_s | Opis |
| Komputer  | Element konfiguracji |

**Przykładowy ekran analizy dzienników dla danych Zarządzanie usługami IT —:**

![Ekran analiza dziennika](./media/log-analytics-itsmc/itsmc-overview-sample-log-analytics.png)

## <a name="itsmc-integration-with-other-oms-solutions"></a>Integracja ITSMC z innych rozwiązań pakietu OMS

Zarządzanie usługami IT — łącznik obecnie obsługuje integrację z rozwiązaniem mapy usługi.

Mapa usług automatycznie wykrywa składniki aplikacji w systemach Windows i Linux i mapuje komunikacji między usługami. Umożliwia ona przeglądać serwery jako traktować ich — jako połączonych systemy, które dostarczają usług krytycznych. Mapy usługi pokazuje połączeń między serwerami, procesów, i portów w dowolnej architekturze połączenia TCP z konfiguracji wymagane inne niż instalacji agenta.

Więcej informacji: [mapy usługi](../operations-management-suite/operations-management-suite-service-map.md).

Jeśli używasz również rozwiązania mapy usług, można wyświetlić elementy technicznej usług utworzone w rozwiązaniach Zarządzanie usługami IT —, jak pokazano w poniższym przykładzie:

![Integracja ServiceMap](./media/log-analytics-itsmc/itsmc-overview-integrated-solutions.png)
## <a name="create-itsm-work-items-for-oms-alerts"></a>Tworzenie elementów roboczych Zarządzanie usługami IT — OMS alertów

Rozwiązanie ITSMC w miejscu można skonfigurować alerty OMS, aby wyzwolić tworzenie elementów roboczych w połączonych narzędzie Zarządzanie usługami IT —. Postępuj zgodnie z następującą procedurą:

1. Z **wyszukiwania dziennika** okna, uruchom zapytania wyszukiwania dziennika, aby wyświetlić dane. Wyniki zapytania są źródłem dla elementów roboczych.
2. W **wyszukiwania dziennika**, kliknij przycisk **alertu** otworzyć **Dodaj regułę alertu** strony.

    ![Ekran analiza dziennika](./media/log-analytics-itsmc/itsmc-work-items-for-oms-alerts.png)

3. Na **Dodaj regułę alertu** okna, podaj wymagane szczegóły dotyczące **nazwa**, **ważność**, **zapytania wyszukiwania**, i **alertu kryteria** (pomiar Metryka okno czasu).
4. Wybierz **tak** dla **akcje Zarządzanie usługami IT —**.
5. Wybierz połączenie Zarządzanie usługami IT — od **połączenia wybierz** listy.
6. Podaj szczegóły zgodnie z potrzebami.
7. Aby utworzyć element roboczy osobne dla każdego wpisu dziennika tego alertu, wybierz **utworzyć poszczególnych elementach roboczych dla każdego wpisu dziennika** wyboru.

    Lub

    Pozostaw to pole wyboru niezaznaczone można utworzyć tylko jeden element roboczy dla dowolnej liczby wpisów dziennika w ramach tego alertu.

7. Kliknij pozycję **Zapisz**.

Utworzono alert OMS są widoczne w obszarze **ustawienia**>**alerty**. Zarządzanie usługami IT — połączenie odpowiednich elementów roboczych są tworzone po spełnieniu warunku określony alert.

## <a name="create-itsm-work-items-from-oms-logs"></a>Tworzenie elementów roboczych Zarządzanie usługami IT — z dzienników OMS

Można też utworzyć elementów roboczych w połączonych źródeł Zarządzanie usługami IT — bezpośrednio z rekordu dziennika. Postępuj zgodnie z następującą procedurą:

1. Z **wyszukiwania dziennika**, wyszukaj wymagane dane, wybierz szczegóły, a następnie kliknij przycisk **Utwórz element roboczy**.

    **Tworzenie elementu roboczego Zarządzanie usługami IT —** zostanie wyświetlone okno:

    ![Ekran analiza dziennika](media/log-analytics-itsmc/itsmc-work-items-from-oms-logs.png)

2.   Dodaj następujące informacje:

  - **Tytuł elementu pracy**: tytuł dla elementu roboczego.
  - **Opis elementu roboczego**: opis nowego elementu roboczego.
  - **Wpływ na komputerze**: Nazwa komputera, na którym zostało znalezione te dane dziennika.
  - **Wybierz połączenie**: Zarządzanie usługami IT — połączenie, w którym chcesz utworzyć ten element roboczy.
  - **Element roboczy**: typ elementu roboczego.

3. Aby korzystać z istniejących szablonów elementów pracy zdarzenia, kliknij przycisk **tak** w obszarze **element roboczy Generowanie na podstawie szablonu** opcji, a następnie kliknij przycisk **Utwórz**.

    Lub:

    Kliknij przycisk **nr** Jeśli chcesz podać dostosowanych wartości.

4. Podaj odpowiednie wartości w **typ**, **wpływ**, **pilność**, **kategorii**, i **podkategorii** pola tekstowe, a następnie kliknij przycisk **Utwórz**.

## <a name="create-itsm-work-items-from-azure-alerts"></a>Tworzenie elementów roboczych Zarządzanie usługami IT — Azure alertów

ITSMC jest zintegrowany z grupy akcji.

[Grupy akcji](../monitoring-and-diagnostics/monitoring-action-groups.md) umożliwiają moduły i wielokrotnego użytku z wyzwalają akcje dla alerty Azure. Przy użyciu akcji Zarządzanie usługami IT — Akcja grup, można utworzyć elementów roboczych w produkcie z istniejącego połączenia z rozwiązaniem łącznika Zarządzanie usługami IT — zarządzanie usługami IT —.

Postępuj zgodnie z następującą procedurą:

1. W portalu Azure kliknij **Monitor**.
2. W okienku po lewej stronie kliknij **grupy akcji**. **Dodaj grupę akcji** zostanie wyświetlone okno.

    ![Grupy akcji](media/log-analytics-itsmc/action-groups.png)

3. Podaj **nazwa** i **nazwa_skrócona** grupy działań. Wybierz **grupy zasobów** i **subskrypcji** której chcesz utworzyć grupy działań.

    ![Szczegóły grupy akcji](media/log-analytics-itsmc/action-groups-details.png)

4. Na liście akcji wybierz **Zarządzanie usługami IT —** z menu rozwijanego dla **typ akcji**. Podaj **nazwa** dla akcji, a następnie kliknij przycisk **Edytuj szczegóły**.
5. Wybierz **subskrypcji** którym znajduje się obszar roboczy analizy dzienników. Wybierz **połączenia** name (nazwa łącznika Zarządzanie usługami IT —) następuje nazwa obszaru roboczego. Na przykład "MyITSMMConnector(MyWorkspace)".

    ![Akcja Zarządzanie usługami IT — szczegóły](./media/log-analytics-itsmc/itsm-action-details.png)

6. Wybierz **elementu roboczego** typu z menu rozwijanego.
   Wybierz korzystać z istniejących szablonów lub wypełnienie pól wymaganych przez produkt Zarządzanie usługami IT —.
7. Kliknij przycisk **OK**.

Podczas tworzenia/edytowania Azure reguły alertu, należy użyć grupy akcji, która ma akcję Zarządzanie usługami IT —. Gdy alert jest wyzwalane, element roboczy jest tworzony w narzędziu Zarządzanie usługami IT —.

>[!NOTE]

> Obecnie tylko alerty dziennika aktywności obsługuje zarządzanie usługami IT — akcja, Azure inne alerty nie obsługują to.


## <a name="troubleshoot-itsm-connections-in-oms"></a>Rozwiązywanie problemów z połączeniami Zarządzanie usługami IT — w OMS
1.  Jeśli połączenie nie powiedzie się z poziomu interfejsu użytkownika połączenia źródła z **błąd podczas zapisywania połączenia** wiadomości, wykonaj następujące czynności:
 - W przypadku połączeń usługi ServiceNow, Cherwell i Provance — upewnij się, poprawnie wprowadzono nazwę użytkownika, hasło, identyfikator klienta i klucz tajny klienta dla każdego połączenia.
        -Sprawdź, czy masz wystarczające uprawnienia w produktu Zarządzanie usługami IT — do nawiązania połączenia.
 - W przypadku połączeń programu Service Manager-sprawdź, czy aplikacja sieci Web zostanie pomyślnie wdrożona i utworzeniu połączenia hybrydowego. Aby sprawdzić pomyślnym nawiązaniu połączenia z komputera lokalnego programu Service Manager, odwiedź adres URL aplikacji sieci Web zgodnie z opisem w dokumentacji dotyczącej wprowadzania [połączenia hybrydowego](log-analytics-itsmc-connections.md#configure-the-hybrid-connection).

2.  Jeśli dane z usługi ServiceNow jest wprowadzenie nie zostały zsynchronizowane z analizy dzienników, upewnij się, że usługi ServiceNow, wystąpienie nie jest uśpiony. Wystąpień deweloperów usługi ServiceNow czasami Przejdź w stan uśpienia podczas bezczynności przez dłuższy okres. W przeciwnym wypadku zgłosić problem.
3.  Jeśli alerty OMS wyzwalać, ale działa elementy nie są tworzone w produkcie Zarządzanie usługami IT — lub elementy konfiguracji nie są utworzone/połączone elementy robocze lub inne informacje ogólne, można znaleźć w następujących miejscach:
 -  ITSMC: Rozwiązanie zawiera podsumowanie połączeń/pracy elementów/komputerów itp. Kliknij przycisk przedstawiający Kafelek **stan łącznika**, która powoduje przejście do **wyszukiwania dziennika** z odpowiednimi zapytania. Przyjrzyj się rekordy dziennika z LogType_S jako błąd Aby uzyskać więcej informacji.
 - **Zaloguj się wyszukiwania** strony: umożliwia wyświetlanie informacji powiązanych z/błędy bezpośrednio za pomocą zapytania *typu = ServiceDeskLog_CL*.

## <a name="troubleshoot-service-manager-web-app-deployment"></a>Rozwiązywanie problemów z wdrażaniem aplikacji sieci Web programu Service Manager
1.  W przypadku wystąpienia jakichkolwiek problemów z wdrażania aplikacji sieci web upewnij się, że masz wystarczające uprawnienia w wymienionych do tworzenia/wdrożenie zasobów subskrypcji.
2.  Jeśli otrzymasz **"Obiekt odwołanie nie jest ustawione na wystąpienie obiektu"** błąd podczas uruchamiania [skryptu](log-analytics-itsmc-service-manager-script.md), upewnij się, że wprowadzono prawidłowe wartości w obszarze **Konfiguracja użytkownika** sekcji .
3.  Nie można utworzyć przestrzeń nazw przekaźnik magistrali usług, upewnij się, że dostawca wymagany zasób jest zarejestrowany w subskrypcji. Jeśli nie jest zarejestrowany, należy ręcznie utworzyć przestrzeń nazw przekaźnik magistrali usług przy użyciu portalu Azure. Można również utworzyć go podczas [Tworzenie połączenia hybrydowego](log-analytics-itsmc-connections.md#configure-the-hybrid-connection) z portalu Azure.


## <a name="contact-us"></a>Skontaktuj się z nami

Dla zapytania lub opinii na temat łącznika zarządzania usługi IT, skontaktuj się z nami pod adresem [ omsitsmfeedback@microsoft.com ](mailto:omsitsmfeedback@microsoft.com).

## <a name="next-steps"></a>Następne kroki
[Dodaj do łącznika zarządzania usługi IT zarządzanie usługami IT — produktów/usług](log-analytics-itsmc-connections.md).
