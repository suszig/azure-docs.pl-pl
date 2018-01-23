---
title: "Analiza dzienników Azure dziennika wyszukiwania interfejsu API REST | Dokumentacja firmy Microsoft"
description: "Ten przewodnik zawiera podstawowe samouczek opisujący sposób wyszukiwania Azure Log Analytics interfejsu API REST można używać wraz z przykładami, które opisano, jak używać poleceń."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.assetid: b4e9ebe8-80f0-418e-a855-de7954668df7
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/19/2018
ms.author: bwren
ms.openlocfilehash: 46c88f7cc250d4c35043039a6f0440aaac85b1c2
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/22/2018
---
# <a name="log-analytics-log-search-rest-api"></a>Analiza dzienników dziennika wyszukiwania interfejsu API REST

> [!IMPORTANT]
> Jeśli obszaru roboczego został uaktualniony do [języka zapytań nowe analizy dzienników](log-analytics-log-search-upgrade.md), a następnie należy zapoznać się [dokumentacji nowej wersji wyszukiwania dziennika interfejsu API](https://dev.loganalytics.io/).  Ta starszej wersji interfejsu API może pracować z obszarem roboczym uaktualniony, ale depracated wkrótce będzie.  Należy zmodyfikować istniejących rozwiązaniach za pomocą nowego interfejsu API.

Ten przewodnik zawiera podstawowe — samouczek, wraz z przykładami stosowania interfejsu API REST Search analizy dziennika. 


## <a name="overview-of-the-log-search-rest-api"></a>Omówienie wyszukiwania dziennika interfejsu API REST
Interfejsu API REST Search analizy dziennika jest RESTful i jest możliwy za pomocą interfejsu API Menedżera zasobów Azure. W tym artykule przedstawiono przykłady uzyskiwania dostępu do interfejsu API za pomocą [ARMClient](https://github.com/projectkudu/ARMClient), narzędzie wiersza polecenia typu open source, które upraszcza wywoływanie interfejsu API Azure Resource Manager. Korzystanie z ARMClient jest jedną z wielu opcji, aby uzyskać dostęp do interfejsu API Search analizy dziennika. Innym rozwiązaniem jest moduł Azure PowerShell na potrzeby OperationalInsights, który oferuje polecenia cmdlet do uzyskiwania dostępu do wyszukiwania. Te narzędzia mogą korzystać z interfejsu API Menedżera zasobów Azure do wykonywania wywołań do analizy dzienników obszarów roboczych i wykonywania poleceń wyszukiwania w nich. Interfejs API generuje wyniki wyszukiwania w formacie JSON, co umożliwia używanie wyniki wyszukiwania na wiele różnych sposobów programowo.

Usługi Azure Resource Manager może służyć za pośrednictwem [Library for .NET](https://msdn.microsoft.com/library/azure/dn910477.aspx) i [interfejsu API REST](https://msdn.microsoft.com/library/azure/mt163658.aspx). Aby dowiedzieć się więcej, przejrzyj połączonych stron sieci web.

> [!NOTE]
> Użycie agregacji polecenia takie jak `|measure count()` lub `distinct`, wyszukiwanie każde wywołanie może zwrócić maksymalnie 500 000 rekordów. Wyszukiwania, które nie zawierają polecenia agregacji zwraca maksymalnie 5000 rekordów.
>
>

## <a name="basic-log-analytics-search-rest-api-tutorial"></a>Podstawowy samouczek interfejsu API REST Search analizy dzienników
### <a name="to-use-armclient"></a>Aby użyć ARMClient
1. Zainstaluj [Chocolatey](https://chocolatey.org/), która jest Otwórz Menedżera pakietu źródłowego dla systemu Windows. Otwórz okno Wiersz polecenia jako administrator, a następnie uruchom następujące polecenie:

    ```
    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString('https://chocolatey.org/install.ps1'))" && SET PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin
    ```
2. Zainstaluj ARMClient, uruchamiając następujące polecenie:

    ```
    choco install armclient
    ```

### <a name="to-perform-a-search-using-armclient"></a>Aby wykonać wyszukiwanie przy użyciu ARMClient
1. Zaloguj się za pomocą konta Microsoft lub konta służbowego:

    ```
    armclient login
    ```

    Pomyślne logowanie Wyświetla wszystkie subskrypcje związana z danego konta:

    ```
    PS C:\Users\SampleUserName> armclient login
    Welcome YourEmail@ORG.com (Tenant: zzzzzzzz-zzzz-zzzz-zzzz-zzzzzzzzzzzz)
    User: YourEmail@ORG.com, Tenant: zzzzzzzz-zzzz-zzzz-zzzz-zzzzzzzzzzzz (Example org)
    There are 3 subscriptions
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 1)
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 2)
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 3)
    ```
2. Pobierz obszary robocze Operations Management Suite:

    ```
    armclient get /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces?api-version=2015-03-20
    ```

    Pomyślne wywołanie Get czy dane wyjściowe wszystkich obszarów roboczych, powiązany z subskrypcją:

    ```
    {
    "value": [
    {
      "properties": {
    "source": "External",
    "customerId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "portalUrl": "https://eus.login.mms.microsoft.com/SignIn.aspx?returnUrl=https%3a%2f%2feus.mms.microsoft.com%2fMain.aspx%3fcid%xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
      },
      "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/{WORKSPACE NAME}",
      "name": "{WORKSPACE NAME}",
      "type": "Microsoft.OperationalInsights/workspaces",
      "location": "East US"
       ]
    }
    ```
3. Utwórz zmiennej użytkownika wyszukiwania:

    ```
    $mySearch = "{ 'top':150, 'query':'Error'}";
    ```
4. Wyszukiwanie za pomocą do nowej zmiennej wyszukiwania:

    ```
    armclient post /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{WORKSPACE NAME}/search?api-version=2015-03-20 $mySearch
    ```

## <a name="log-analytics-search-rest-api-reference-examples"></a>Zaloguj się przykłady odwołanie do interfejsu API REST wyszukiwania analityka
W poniższych przykładach pokazano, jak można użyć interfejsu API Search.

### <a name="search---actionread"></a>Wyszukiwanie — Akcja/Odczyt
**Przykładowy adres Url:**

```
    /subscriptions/{SubscriptionId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/search?api-version=2015-03-20
```

**Żądanie:**

```
    $savedSearchParametersJson =
    {
      "top":150,
      "highlight":{
        "pre":"{[hl]}",
        "post":"{[/hl]}"
      },
      "query":"*",
      "start":"2015-02-04T21:03:29.231Z",
      "end":"2015-02-11T21:03:29.231Z"
    }
    armclient post /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/search?api-version=2015-03-20 $searchParametersJson
```
W poniższej tabeli opisano właściwości, które są dostępne.

| **Właściwość** | **Opis** |
| --- | --- |
| top |Maksymalna liczba wyników do zwrócenia. |
| wyróżnienie |Zawiera parametry przed i po używane zwykle do wyróżnianie pasujących pól |
| wstępnie |Prefiksy dany ciąg znaków polom dopasowany. |
| POST |Dołącza podany ciąg polom dopasowany. |
| query |Używany do gromadzenia i zwracają wyniki zapytania wyszukiwania. |
| rozpoczynanie |Początek przedział czasu mają być można znaleźć w wynikach. |
| end |Koniec przedział czasu mają być można znaleźć w wynikach. |

**Odpowiedź:**

```
    {
      "id" : "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "__metadata" : {
        "resultType" : "raw",
        "total" : 1455,
        "top" : 150,
        "StartTime" : "2015-02-11T21:09:07.0345815Z",
        "Status" : "Successful",
        "LastUpdated" : "2015-02-11T21:09:07.331463Z",
        "CoreResponses" : [],
        "sort" : [{
          "name" : "TimeGenerated",
          "order" : "desc"
        }],
        "requestTime" : 450
      },
      "value": [{
        "SourceSystem" : "OpsManager",
        "TimeGenerated" : "2015-02-07T14:07:33Z",
        "Source" : "SideBySide",
        "EventLog" : "Application",
        "Computer" : "BAMBAM",
        "EventCategory" : 0,
        "EventLevel" : 1,
        "EventLevelName" : "Error",
        "UserName" : "N/A",
        "EventID" : 78,
        "MG" : "00000000-0000-0000-0000-000000000001",
        "TimeCollected" : "2015-02-07T14:10:04.69Z",
        "ManagementGroupName" : "AOI-5bf9a37f-e841-462b-80d2-1d19cd97dc40",
        "id" : "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
        "Type" : "Event",
        "__metadata" : {
          "Type" : "Event",
          "TimeGenerated" : "2015-02-07T14:07:33Z",
          "highlighting" : {
          "EventLevelName" : ["{[hl]}Error{[/hl]}"]
        }
      }]
    ],
            "start" : "2015-02-04T21:03:29.231Z",
            "end" : "2015-02-11T21:03:29.231Z"
          }
        }
      }]
    }
```

### <a name="searchid---actionread"></a>Wyszukiwanie i {ID} - odczytu akcji
**Żądanie zapisane wyszukiwanie zawartości:**

```
    armclient post /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/search/{SearchId}?api-version=2015-03-20
```

> [!NOTE]
> Jeśli wyniki wyszukiwania zawierają ze stanem "Oczekujące", następnie sondowania zaktualizowanych wyników jest możliwe za pośrednictwem tego interfejsu API. Po 6 min wynik wyszukiwania zostanie usunięty z pamięci podręcznej i usunięty HTTP zostanie zwrócony. Żądania wyszukiwania początkowej natychmiast zwraca stan "Powiodło się", wyniki nie są dodawane do pamięci podręcznej przyczyną tego interfejsu API do zwrócenia usunięty HTTP, jeśli zapytanie. Zawartość wynik 200 protokołu HTTP jest w tym samym formacie co żądanie początkowe wyszukiwanie tylko z zaktualizowane wartości.
>
>

### <a name="saved-searches"></a>Zapisane wyszukiwania
**Żądania listy zapisanych wyszukiwań:**

```
    armclient post /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/savedSearches?api-version=2015-03-20
```

Obsługiwane metody: GET, PUT usunąć

Obsługiwane metody kolekcji: GET

W poniższej tabeli opisano właściwości, które są dostępne.

| Właściwość | Opis |
| --- | --- |
| Identyfikator |Unikatowy identyfikator. |
| Element etag |**Wymagane poprawki**. Zaktualizowany przez serwer podczas każdego zapisu. Wartość musi być równa wartości przechowywanej bieżącego lub "*" do aktualizacji. 409 zwracane wartości stary lub jest ono nieprawidłowe. |
| properties.query |**Wymagane**. Zapytania wyszukiwania. |
| properties.displayName |**Wymagane**. Nazwa wyświetlana użytkownika zapytania. |
| properties.category |**Wymagane**. Zdefiniowane przez użytkownika kategorii zapytania. |

> [!NOTE]
> Interfejs API wyszukiwania usługi Analiza dzienników zwraca aktualnie utworzonych przez użytkownika zapisane wyszukiwania podczas sondowania zapisane wyszukiwania w obszarze roboczym. Interfejs API nie zwraca zapisanych wyszukiwań pochodzącymi z rozwiązania.
>
>

### <a name="create-saved-searches"></a>Tworzenie zapisanych wyszukiwań
**Żądanie:**

```
    $savedSearchParametersJson = "{'properties': { 'Category': 'myCategory', 'DisplayName':'myDisplayName', 'Query':'* | measure Count() by Source', 'Version':'1'  }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisismyid?api-version=2015-03-20 $savedSearchParametersJson
```

> [!NOTE]
> Nazwa dla wszystkich zapisanych wyszukiwań, harmonogramów i akcje utworzone przy użyciu interfejsu API analizy dziennika musi być pisane małymi literami.

### <a name="delete-saved-searches"></a>Usuń zapisane wyszukiwania
**Żądanie:**

```
    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisIsMyId?api-version=2015-03-20
```

### <a name="update-saved-searches"></a>Aktualizowanie zapisanych wyszukiwań
 **Żądanie:**

```
    $savedSearchParametersJson = "{'etag': 'W/`"datetime\'2015-04-16T23%3A35%3A35.3182423Z\'`"', 'properties': { 'Category': 'myCategory', 'DisplayName':'myDisplayName', 'Query':'* | measure Count() by Source', 'Version':'1'  }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisIsMyId?api-version=2015-03-20 $savedSearchParametersJson
```

### <a name="metadata---json-only"></a>Metadane — tylko w formacie JSON
Poniżej przedstawiono sposób, aby wyświetlać pól dla wszystkich typów dziennika dla danych zebranych w obszarze roboczym. Na przykład jeśli chcesz się, że wiesz, jeśli typ zdarzenia zawiera pole o nazwie komputera to zapytanie jest jednym ze sposobów sprawdzenia.

**Żądanie dla pola:**

```
    armclient get /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/schema?api-version=2015-03-20
```

**Odpowiedź:**

```
    {
      "__metadata" : {
        "schema" : {
          "name" : "Example Name",
          "version" : 2
        },
        "resultType" : "schema",
        "requestTime" : 35
      },
      "value" : [{
          "name" : "MG",
          "displayName" : "MG",
          "type" : "Guid",
          "facetable" : true,
          "display" : false,
          "ownerType" : ["PerfHourly", "ProtectionStatus", "Capacity_SMBUtilizationByHost", "Capacity_ArrayUtilization", "Capacity_SMBShareUtilization", "SQLAssessmentRecommendation", "Event", "ConfigurationChange", "ConfigurationAlert", "ADAssessmentRecommendation", "ConfigurationObject", "ConfigurationObjectProperty"]
        }, {
          "name" : "ManagementGroupName",
          "displayName" : "ManagementGroupName",
          "type" : "String",
          "facetable" : true,
          "display" : true,
          "ownerType" : ["PerfHourly", "ProtectionStatus", "Event", "ConfigurationChange", "ConfigurationAlert", "W3CIISLog", "AlertHistory", "Recommendation", "Alert", "SecurityEvent", "UpdateAgent", "RequiredUpdate", "ConfigurationObject", "ConfigurationObjectProperty"]
        }
      ]
    }
```

W poniższej tabeli opisano właściwości, które są dostępne.

| **Właściwość** | **Opis** |
| --- | --- |
| name |Nazwa pola. |
| Nazwa wyświetlana |Nazwa wyświetlana tego pola. |
| type |Typ wartości pola. |
| Tworzenie aspektów |Kombinacja bieżącego indeksowane, "przechowywane" i właściwości "aspektu". |
| Wyświetl |Bieżący właściwości "display". Wartość true, jeśli pole jest widoczne w obszarze wyszukiwania. |
| ownerType |Zmniejszony tylko do typów należących do IP został załadowany. |

## <a name="optional-parameters"></a>Parametry opcjonalne
Poniżej opisano dostępne następujące parametry opcjonalne.

### <a name="highlighting"></a>Wyróżnianie
Parametr "Highlight" jest parametrem opcjonalnym, których możesz używać do żądania wyszukiwania podsystemu obejmują zestaw znaczników w odpowiedzi.

Te znaczniki wskazują początek i kończyć wyróżnionego tekstu, który dopasowuje podane w kwerendzie wyszukiwania.
Można określić znaczniki rozpoczęcia i zakończenia, które będą używane podczas wyszukiwania, aby zawijać termin wyróżnione.

**Przykładowe zapytanie wyszukiwania**

```
    $savedSearchParametersJson =
    {
      "top":150,
      "highlight":{
        "pre":"{[hl]}",
        "post":"{[/hl]}"
      },
      "query":"*",
      "start":"2015-02-04T21:03:29.231Z",
      "end":"2015-02-11T21:03:29.231Z"
    }
    armclient post /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/search?api-version=2015-03-20 $searchParametersJson
```

**Przykładowe wyniki:**

```
    {
        "TimeGenerated":
        "2015-05-18T23:55:59Z", "Source":
        "EventLog": "Application",
        "Computer": "smokedturkey.net",
        "EventCategory": 0,
        "EventLevel":1,
        "EventLevelName":
        "Error"
        "Manager ", "__metadata":
        {
            "Type": "Event",
            "TimeGenerated": "2015-05-18T23:55:59Z",
            "highlighting": {
                "EventLevelName":
                ["{[hl]}Error{[/hl]}"]
            }
        }
    }
```

Zwróć uwagę, że wyniku poprzedniej zawiera komunikat o błędzie i jest poprzedzony a dołączone.

## <a name="computer-groups"></a>Grupy komputerów
Grupy komputerów są specjalne zapisanych wyszukiwań, które zwracają zestaw komputerów.  Aby ograniczyć wyniki do komputerów w grupie służy grupy komputerów w innych zapytań.  Grupa komputerów jest implementowany jako zapisanego kryterium wyszukiwania z tagiem grupy o wartości komputera.

Poniżej znajduje się przykładowa odpowiedź dla grupy komputerów.

```
    "etag": "W/\"datetime'2016-04-01T13%3A38%3A04.7763203Z'\"",
    "properties": {
        "Category": "My Computer Groups",
        "DisplayName": "My Computer Group",
        "Query": "srv* | Distinct Computer",
        "Tags": [{
            "Name": "Group",
            "Value": "Computer"
          }],
    "Version": 1
    }
```

### <a name="retrieving-computer-groups"></a>Pobieranie grupy komputerów
Aby uzyskać dostęp do grupy komputerów, należy użyć metody Get z identyfikatora grupy.

```
armclient get /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Group ID}`?api-version=2015-03-20
```

### <a name="creating-or-updating-a-computer-group"></a>Tworzenie lub aktualizowanie grupy komputerów
Aby utworzyć grupę komputerów, użyj metody Put identyfikator unikatowy zapisanego kryterium wyszukiwania. Użycie istniejącego Identyfikatora grupy komputerów, co jest modyfikowany. Po utworzeniu grupy komputerów w portalu usługi Analiza dzienników identyfikator jest tworzony z grupy i nazwy.

Zapytanie używane do zdefiniowania grupy musi zwracać zestaw komputerów w grupie, aby działać poprawnie.  Zaleca się kończyć się zapytanie o `| Distinct Computer` aby zapewnić poprawne dane są zwracane.

Definicja zapisanego wyszukiwania musi zawierać tag o nazwie grupy o wartości komputerowi wyszukiwania sklasyfikowany jako grupę komputerów.

```
    $etag=Get-Date -Format yyyy-MM-ddThh:mm:ss.msZ
    $groupName="My Computer Group"
    $groupQuery = "Computer=srv* | Distinct Computer"
    $groupCategory="My Computer Groups"
    $groupID = "My Computer Groups | My Computer Group"

    $groupJson = "{'etag': 'W/`"datetime\'" + $etag + "\'`"', 'properties': { 'Category': '" + $groupCategory + "', 'DisplayName':'"  + $groupName + "', 'Query':'" + $groupQuery + "', 'Tags': [{'Name': 'Group', 'Value': 'Computer'}], 'Version':'1'  }"

    armclient put /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/$groupId`?api-version=2015-03-20 $groupJson
```

### <a name="deleting-computer-groups"></a>Usuwanie grupy komputerów
Aby usunąć grupę komputerów, należy użyć metody Delete z identyfikatora grupy.

```
armclient delete /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/$groupId`?api-version=2015-03-20
```


## <a name="next-steps"></a>Kolejne kroki
* Dowiedz się więcej o [dziennika wyszukiwania](log-analytics-log-searches.md) do kompilowania zapytań przy użyciu pól niestandardowych kryteriów.
