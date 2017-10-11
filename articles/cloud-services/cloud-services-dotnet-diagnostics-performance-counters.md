---
title: "Użyj liczników wydajności w diagnostyce Azure | Dokumentacja firmy Microsoft"
description: "Użyj liczników wydajności usługi w chmurze platformy Azure lub maszyny wirtualnej, aby znaleźć wąskich gardeł i dostrajania wydajności."
services: cloud-services
documentationcenter: .net
author: rboucher
manager: jwhit
editor: tysonn
ms.assetid: fc4c61e9-d49d-4ed9-a32c-b91cdf851909
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/29/2016
ms.author: robb
ms.openlocfilehash: 2cf765cb034725199127c547a9b8b997a4a6089c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/29/2017
---
# <a name="create-and-use-performance-counters-in-an-azure-application"></a>Tworzenie i używanie liczników wydajności w aplikacji Azure
W tym artykule opisano zalet i jak poddane liczniki wydajności aplikacji platformy Azure. Można używać ich do zbierania danych, Znajdź wąskich gardeł i dostrajania wydajności systemu i aplikacji.

Można również zebranych liczników wydajności dostępnych dla systemu Windows Server, IIS i platformy ASP.NET i używany w celu określenia kondycji role sieci web platformy Azure, roli proces roboczy i maszyny wirtualne. Można również utworzyć i używać niestandardowe liczniki wydajności.  

Należy zbadać dane licznika wydajności

1. Bezpośrednio na hoście aplikacji za pomocą narzędzia Monitor wydajności dostęp za pomocą pulpitu zdalnego
2. Za pomocą programu System Center Operations Manager przy użyciu pakietu administracyjnego platformy Azure
3. Przy użyciu innych narzędzi monitorowania, które uzyskują dostęp do danych diagnostycznych przekazywane do magazynu Azure. Zobacz [magazynu i widoku danych diagnostycznych w usłudze Azure Storage](https://msdn.microsoft.com/library/azure/hh411534.aspx) Aby uzyskać więcej informacji.  

Aby uzyskać więcej informacji na temat monitorowania wydajności aplikacji w [portalu Azure](http://portal.azure.com/), zobacz [jak usługi w chmurze Monitor](https://www.azure.com/manage/services/cloud-services/how-to-monitor-a-cloud-service/).

Aby uzyskać dodatkowe szczegółowe wskazówki dotyczące tworzenia rejestrowania i śledzenia strategii i przy użyciu diagnostyki i innych technik rozwiązywania problemów i optymalizowanie aplikacji Azure, zobacz [Rozwiązywanie problemów z najlepszych rozwiązań dotyczących tworzenia Azure Aplikacje](https://msdn.microsoft.com/library/azure/hh771389.aspx).

## <a name="enable-performance-counter-monitoring"></a>Aby włączyć monitorowanie licznika wydajności
Liczniki wydajności nie są domyślnie włączone. Aplikacja lub zadanie uruchamiania należy zmodyfikować domyślną konfigurację agenta diagnostyki uwzględnienie liczniki wydajności zależnych, które chcesz monitorować dla każdego wystąpienia roli.

### <a name="performance-counters-available-for-microsoft-azure"></a>Liczniki wydajności dostępne platformy Microsoft Azure
Platforma Azure udostępnia podzbioru dostępnych liczników wydajności dla systemu Windows Server, IIS i stosu ASP.NET. W poniższej tabeli wymieniono niektóre liczniki wydajności szczególne znaczenie dla aplikacji platformy Azure.

| Kategoria licznika: Obiektu (wystąpienia) | Nazwa licznika | Dokumentacja |
| --- | --- | --- |
| Wyjątki środowiska CLR platformy .NET (*globalne*) |# Clr\\liczba wyjątków / s |Liczniki wydajności wyjątku |
| .NET CLR pamięci (*globalne*) |% Czasu odzyskiwania pamięci |Liczniki wydajności pamięci |
| ASP.NET |Aplikacja zostanie uruchomiona ponownie |Liczniki wydajności dla programu ASP.NET |
| ASP.NET |Czas wykonania żądania |Liczniki wydajności dla programu ASP.NET |
| ASP.NET |Liczba żądań rozłączonych |Liczniki wydajności dla programu ASP.NET |
| ASP.NET |Ponowne uruchomienie procesu roboczego |Liczniki wydajności dla programu ASP.NET |
| Aplikacji programu ASP.NET (**całkowita**) |Całkowita liczba żądań |Liczniki wydajności dla programu ASP.NET |
| Aplikacji programu ASP.NET (**całkowita**) |Żądania na sekundę |Liczniki wydajności dla programu ASP.NET |
| ASP.NET 4.0.30319 |Czas wykonania żądania |Liczniki wydajności dla programu ASP.NET |
| ASP.NET 4.0.30319 |Czas oczekiwania na żądanie |Liczniki wydajności dla programu ASP.NET |
| ASP.NET 4.0.30319 |Bieżące żądania |Liczniki wydajności dla programu ASP.NET |
| ASP.NET 4.0.30319 |Liczba żądań w kolejce |Liczniki wydajności dla programu ASP.NET |
| ASP.NET 4.0.30319 |Liczba żądań odrzuconych |Liczniki wydajności dla programu ASP.NET |
| Memory (Pamięć) |Dostępna pamięć (MB) |Liczniki wydajności pamięci |
| Memory (Pamięć) |Zadeklarowane bajty |Liczniki wydajności pamięci |
| Procesor(_Total) |Czas procesora (%) |Liczniki wydajności dla programu ASP.NET |
| TCPv4 |Liczba błędów połączenia |Obiekt TCP |
| TCPv4 |Połączeń |Obiekt TCP |
| TCPv4 |Resetowanie połączenia |Obiekt TCP |
| TCPv4 |Segmenty wysłane/s |Obiekt TCP |
| Interface(*) sieci |Bajty odebrane/s |Obiekt interfejsu sieciowego |
| Interface(*) sieci |Bajty wysłane/s |Obiekt interfejsu sieciowego |
| Interfejs sieciowy (_2 karty magistrali maszyny wirtualnej Microsoft sieci) |Bajty odebrane/s |Obiekt interfejsu sieciowego |
| Interfejs sieciowy (_2 karty magistrali maszyny wirtualnej Microsoft sieci) |Bajty wysłane/s |Obiekt interfejsu sieciowego |
| Interfejs sieciowy (_2 karty magistrali maszyny wirtualnej Microsoft sieci) |Całkowita liczba bajtów/s |Obiekt interfejsu sieciowego |

## <a name="create-and-add-custom-performance-counters-to-your-application"></a>Utwórz i Dodaj niestandardowe liczniki wydajności do aplikacji
Platforma Azure ma techniczną w celu tworzenia i modyfikowania niestandardowe liczniki wydajności dla ról sieci web i proces roboczy. Liczniki mogą służyć do śledzenie i monitorowanie zachowania specyficzne dla aplikacji. Można tworzyć i usuwanie kategorii licznika wydajności niestandardowych i specyfikatory zadanie uruchamiania, rola sieci web lub roli proces roboczy z podwyższonym poziomem uprawnień.

> [!NOTE]
> Kod, który zmienia niestandardowe liczniki wydajności mają podwyższony poziom uprawnień do uruchomienia. Jeśli kod jest w roli sieci web lub roli proces roboczy, rola musi zawierać znacznik <Runtime executionContext="elevated" /> w pliku ServiceDefinition.csdef dla roli, aby został poprawnie zainicjowany.
>
>

Możesz wysłać dane licznika wydajności niestandardowych do magazynu platformy Azure przy użyciu agenta diagnostyki.

Dane licznika wydajności standardowe jest generowany przez procesy Azure. Dane licznika wydajności niestandardowych muszą być utworzone przez aplikację sieci web roli lub procesu roboczego roli. Zobacz [typy licznika wydajności](https://msdn.microsoft.com/library/z573042h.aspx) informacje o typach danych, które mogą być przechowywane w niestandardowe liczniki wydajności. Zobacz [próbki liczniki wydajności](http://code.msdn.microsoft.com/azure/) na przykład, które tworzy i ustawia dane licznika wydajności niestandardowych w roli sieci web.

## <a name="store-and-view-performance-counter-data"></a>Magazyn i widoku danych licznika wydajności
Azure przechowuje dane licznika wydajności z innych informacji diagnostycznych. Te dane są dostępne dla monitorowania zdalnego uruchomionej dla wystąpienia roli przy użyciu zdalnego dostępu do pulpitu. Aby wyświetlić narzędzi, takich jak monitora wydajności. Do utrwalenia danych poza wystąpienia roli, agenta diagnostyki należy przenieść dane do magazynu Azure. Limit rozmiaru danych licznika wydajności pamięci podręcznej można skonfigurować w agencie diagnostyki lub może być skonfigurowany jako część udostępnionego limit dla wszystkich danych diagnostycznych. Aby uzyskać więcej informacji na temat ustawiania rozmiaru buforu, zobacz [OverallQuotaInMB](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitorconfiguration.overallquotainmb.aspx) i [DirectoriesBufferConfiguration](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.directoriesbufferconfiguration.aspx). Zobacz [magazynu i widoku danych diagnostycznych w usłudze Azure Storage](https://msdn.microsoft.com/library/azure/hh411534.aspx) omówienie konfigurowania agenta diagnostyki na przesyłanie danych do konta magazynu.

Każde wystąpienie licznika wydajności skonfigurowany jest rejestrowana w określonym próbkowania, a próbki danych jest przekazywana do konta magazynu przez żądanie transferu zaplanowanego lub żądanie transferu na żądanie. Przeniesienia automatyczne może zostać zaplanowane tak często, jak jeden raz na minutę. Przekazywane przez agenta diagnostyki danych licznika wydajności są przechowywane w tabeli, WADPerformanceCountersTable, w ramach konta magazynu. Ta tabela może uzyskać dostępu do i wykonać zapytania z metody interfejsu API magazynu Azure w warstwie standardowa. Zobacz [przykład liczniki wydajności programu Microsoft Azure](http://code.msdn.microsoft.com/Windows-Azure-PerformanceCo-7d80ebf9) przykład kwerend i wyświetlanie danych licznika wydajności z tabeli WADPerformanceCountersTable.

> [!NOTE]
> W zależności od Diagnostyka agenta transferu częstotliwość i czas oczekiwania w kolejce najnowsze dane licznika wydajności w ramach konta magazynu może potrwać kilka minut nieaktualny.
>
>

## <a name="enable-performance-counters-using-diagnostics-configuration-file"></a>Włącz użycie pliku konfiguracji diagnostyki liczniki wydajności
Aby włączyć liczniki wydajności w aplikacji platformy Azure, użyj poniższej procedury.

## <a name="prerequisites"></a>Wymagania wstępne
W tej sekcji założono, że zaimportowane monitor diagnostyki aplikacji i plik konfiguracji diagnostyki dodany do rozwiązania Visual Studio (diagnostics.wadcfg w 2.4 zestawu SDK i poniżej lub diagnostics.wadcfgx w zestawie SDK, 2.5 lub nowszej). Zobacz kroki 1 i 2 w [Włączanie diagnostyki w usług Azure Cloud Services i maszyn wirtualnych](cloud-services-dotnet-diagnostics.md)) Aby uzyskać więcej informacji.

## <a name="step-1-collect-and-store-data-from-performance-counters"></a>Krok 1: Zbieranie i przechowywanie danych z liczników wydajności
Po dodaniu pliku diagnostyki do rozwiązania Visual Studio, można skonfigurować zbieranie i przechowywanie danych licznika wydajności w aplikacji Azure. Jest to realizowane przez dodanie liczników wydajności do pliku diagnostyki. Dane diagnostyczne, łącznie z liczników wydajności, najpierw są zbierane w wystąpieniu. Dane następnie jest trwały do tabeli WADPerformanceCountersTable w usłudze tabel Azure, więc należy również określić konto magazynu w aplikacji. W przypadku testowania lokalnie aplikację w emulatorze obliczeniowe, można również przechowywać dane diagnostyczne lokalnie w emulatorze magazynu. Przed zapisaniem danych diagnostycznych, należy najpierw przejść do [portalu Azure](http://portal.azure.com/) i Utwórz konto magazynu klasycznego. Najlepszym rozwiązaniem jest można znaleźć konta magazynu w tej samej lokalizacji geograficznej jako aplikacji platformy Azure. Przez zachowanie aplikacji Azure i konto magazynu znajdują się w tej samej lokalizacji geograficznej, uniknąć kosztów przepustowości zewnętrznych płatności i zmniejszenia opóźnień.

### <a name="add-performance-counters-to-the-diagnostics-file"></a>Dodaj liczniki wydajności do pliku diagnostyki
Istnieje wiele liczników, których można użyć. W poniższym przykładzie przedstawiono kilka liczniki wydajności, które są zalecane sieci web i monitorowania roli procesu roboczego.

Otwórz plik diagnostyki (diagnostics.wadcfg w 2.4 zestawu SDK i poniżej lub diagnostics.wadcfgx w zestawie SDK, 2.5 lub nowszym) i dodaj następującą wartość do elementu DiagnosticMonitorConfiguration:

```xml
<PerformanceCounters bufferQuotaInMB="0" scheduledTransferPeriod="PT30M">
    <PerformanceCounterConfiguration counterSpecifier="\Memory\Available Bytes" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT30S" />

    <!-- Use the Process(w3wp) category counters in a web role -->

    <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\% Processor Time" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\Private Bytes" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\Thread Count" sampleRate="PT30S" />

    <!-- Use the Process(WaWorkerHost) category counters in a worker role.
        <PerformanceCounterConfiguration counterSpecifier="\Process(WaWorkerHost)\% Processor Time" sampleRate="PT30S" />
        <PerformanceCounterConfiguration counterSpecifier="\Process(WaWorkerHost)\Private Bytes" sampleRate="PT30S" />
        <PerformanceCounterConfiguration counterSpecifier="\Process(WaWorkerHost)\Thread Count" sampleRate="PT30S" />
    -->

    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Interop(_Global_)\# of marshalling" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Loading(_Global_)\% Time Loading" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR LocksAndThreads(_Global_)\Contention Rate / sec" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Memory(_Global_)\# Bytes in all Heaps" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Networking(_Global_)\Connections Established" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Remoting(_Global_)\Remote Calls/sec" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Jit(_Global_)\% Time in Jit" sampleRate="PT30S" />
</PerformanceCounters>
```

Atrybut bufferQuotaInMB, która określa maksymalną ilość pamięci systemu plików, która jest dostępna dla tego typu kolekcji danych (Azure dzienniki, dzienniki programu IIS, itp.). Wartość domyślna to 0. Po osiągnięciu limitu przydziału najstarsze dane zostaną usunięte po dodaniu nowych danych. Suma wszystkich właściwości bufferQuotaInMB musi być większa niż wartość atrybutu OverallQuotaInMB. Bardziej szczegółowe omówienie określania, ile miejsca do magazynowania jest wymagana w przypadku zbierania danych diagnostycznych, zobacz sekcję konfiguracji WAD [Rozwiązywanie problemów z najlepszych rozwiązań dotyczących tworzenia aplikacji Azure](https://msdn.microsoft.com/library/windowsazure/hh771389.aspx).

Atrybut scheduledTransferPeriod, który określa interwał między zaplanowane transferów danych, zaokrąglona w górę do najbliższej minutę. W poniższych przykładach jest ustawiona na PT30M (30 minut). Ustawienie okresu transfer mała wartość, na przykład 1 minutę, obniży wydajności aplikacji w środowisku produkcyjnym, ale może być przydatne w przypadku diagnostyki oglądanie Postaramy się szybko podczas testowania. Wartość okresu zaplanowanego transferu powinna być duże, aby upewnić się, że danych diagnostycznych nie jest zastępowana wystąpienia, ale wystarczająco duży, że nie ma wpływu na wydajność aplikacji.

Atrybut counterSpecifier Określa licznik wydajności do zbierania. Atrybut sampleRate określa szybkość, w którym licznik wydajności powinny być pobrane, w tym przypadku 30 sekund.

Po dodaniu liczniki wydajności, które chcesz zebrać, Zapisz zmiany w pliku diagnostyki. Następnie należy określić konto magazynu, które zostaną utrwalone danych diagnostycznych.

### <a name="specify-the-storage-account"></a>Określ konto magazynu
Aby utrwalić danych diagnostycznych do konta magazynu Azure, należy określić parametry połączenia w pliku konfiguracyjnym (pliku ServiceConfiguration.cscfg) usługi.

2.5 zestawu SDK platformy Azure konta magazynu można określić w pliku diagnostics.wadcfgx.

> [!NOTE]
> Te instrukcje dotyczą tylko, 2.4 zestawu SDK platformy Azure i poniżej. 2.5 zestawu SDK platformy Azure konta magazynu można określić w pliku diagnostics.wadcfgx.
>
>

Aby ustawić parametry połączenia:

1. Otwórz plik ServiceConfiguration.Cloud.cscfg przy użyciu w ulubionym edytorze tekstów i ustaw parametry połączenia magazynu. *AccountName* i *AccountKey* wartości znajdują się w portalu Azure na pulpicie nawigacyjnym konta magazynu, w obszarze klucze dostępu.

    ```xml
    <ConfigurationSettings>
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="DefaultEndpointsProtocol=https;AccountName=<name>;AccountKey=<key>"/>
    </ConfigurationSettings>
    ```
2. Zapisz plik ServiceConfiguration.Cloud.cscfg.
3. Otwórz plik ServiceConfiguration.Local.cscfg i sprawdź, czy UseDevelopmentStorage jest ustawiony na wartość true.

    ```xml
    <ConfigurationSettings>
      <Settingname="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true"/>
    </ConfigurationSettings>
    ```
   Teraz, gdy parametry połączenia są ustawione, aplikacji zachować dane diagnostyczne do konta magazynu, po wdrożeniu aplikacji.
4. Zapisz i skompilowanie projektu, a następnie wdrożyć aplikację.

## <a name="step-2-optional-create-custom-performance-counters"></a>Krok 2: (Opcjonalnie) Utwórz niestandardowe liczniki wydajności
Oprócz liczniki wydajności wstępnie zdefiniowane można dodać własne niestandardowe liczniki wydajności do monitorowania role sieci web lub procesu roboczego. Niestandardowe liczniki wydajności mogą służyć do śledzenia i monitorowanie zachowania specyficzne dla aplikacji i może być utworzone lub usunięte zadanie uruchamiania, rola sieci web lub roli proces roboczy z podwyższonym poziomem uprawnień.

Agent Azure diagnostics odświeża konfigurację liczników wydajności z pliku .wadcfg minutę po uruchomieniu.  Utwórz niestandardowe liczniki wydajności w przypadku metody OnStart, uruchamiania zadań trwać dłużej niż minutę do wykonania Twoje niestandardowe liczniki wydajności zostaną nie utworzono podczas diagnostyki Azure agent próbuje załadować je.  W tym scenariuszu zobaczysz, że diagnostyki Azure poprawnie przechwytuje wszystkie dane diagnostyczne, z wyjątkiem Twojego niestandardowe liczniki wydajności.  Aby rozwiązać ten problem, należy utworzyć liczniki wydajności w zadanie uruchamiania lub Przenieś niektóre zadania uruchamiania działają metody OnStart po utworzeniu liczników wydajności.

Wykonaj poniższe kroki, aby utworzyć prosty wydajności niestandardowych licznik o nazwie "\MyCustomCounterCategory\MyButton1Counter":

1. Otwórz plik definicji usługi (CSDEF) dla aplikacji.
2. Dodaj element środowiska uruchomieniowego do sieć Web lub proces roboczy elementu umożliwia wykonanie z podwyższonym poziomem uprawnień:

    ```xml
    <runtime executioncontext="elevated"/>
    ```
3. Zapisz plik.
4. Otwórz plik diagnostyki (diagnostics.wadcfg w 2.4 zestawu SDK i poniżej lub diagnostics.wadcfgx w zestawie SDK, 2.5 lub nowszym) i dodaj następującą wartość do DiagnosticMonitorConfiguration

    ```xml
    <PerformanceCounters bufferQuotaInMB="0" scheduledTransferPeriod="PT30M">
      <PerformanceCounterConfiguration counterSpecifier="\MyCustomCounterCategory\MyButton1Counter" sampleRate="PT30S"/>
    </PerformanceCounters>
    ```
5. Zapisz plik.
6. Tworzenie kategorii licznika wydajności niestandardowe metody OnStart roli użytkownika przed wywołaniem bazy. OnStart. W poniższym przykładzie C# tworzy kategorię niestandardową, jeśli jeszcze nie istnieje:

    ```csharp
    public override bool OnStart()
    {
      if (!PerformanceCounterCategory.Exists("MyCustomCounterCategory"))
      {
         CounterCreationDataCollection counterCollection = new CounterCreationDataCollection();

         // add a counter tracking user button1 clicks
         CounterCreationData operationTotal1 = new CounterCreationData();
         operationTotal1.CounterName = "MyButton1Counter";
         operationTotal1.CounterHelp = "My Custom Counter for Button1";
         operationTotal1.CounterType = PerformanceCounterType.NumberOfItems32;
         counterCollection.Add(operationTotal1);

         PerformanceCounterCategory.Create(
           "MyCustomCounterCategory",
           "My Custom Counter Category",
           PerformanceCounterCategoryType.SingleInstance, counterCollection);

         Trace.WriteLine("Custom counter category created.");
      }
      else {
        Trace.WriteLine("Custom counter category already exists.");
      }

    return base.OnStart();
    }
    ```
7. Aktualizuj liczniki w aplikacji. Poniższy przykład aktualizacje licznika wydajności niestandardowych zdarzeń Button1_Click:

    ```csharp
    protected void Button1_Click(object sender, EventArgs e)
    {
      PerformanceCounter button1Counter = new PerformanceCounter(
        "MyCustomCounterCategory",
        "MyButton1Counter",
        string.Empty,
        false);
      button1Counter.Increment();
      this.Button1.Text = "Button 1 count: " +
        button1Counter.RawValue.ToString();
    }
    ```
8. Zapisz plik.  

Dane licznika wydajności niestandardowych teraz zostaną zebrane przez monitor diagnostycznych platformy Azure.

## <a name="step-3-query-performance-counter-data"></a>Krok 3: Wykonywanie zapytań danych licznika wydajności
Po wdrożeniu aplikacji i uruchomiona, monitor diagnostyki rozpocznie się zbierania liczników wydajności i przechowywanie danych do magazynu Azure. Użyj narzędzi takich jak Eksplorator serwera w programie Visual Studio, [Eksploratora usługi Storage Azure](http://azurestorageexplorer.codeplex.com/), lub [Menedżera diagnostyki Azure](http://www.cerebrata.com/Products/AzureDiagnosticsManager/Default.aspx) przez Cerebrata, aby wyświetlić dane w WADPerformanceCountersTable liczniki wydajności Tabela. Można również programowo zapytania przy użyciu usługi tabeli [C#](../cosmos-db/table-storage-how-to-use-dotnet.md), [Java](../cosmos-db/table-storage-how-to-use-java.md), [Node.js](../cosmos-db/table-storage-how-to-use-nodejs.md), [Python](../cosmos-db/table-storage-how-to-use-python.md), [Ruby](../cosmos-db/table-storage-how-to-use-ruby.md), lub [PHP](../cosmos-db/table-storage-how-to-use-php.md).

W poniższym przykładzie C# zawiera zapytania podstawowego w tabeli WADPerformanceCountersTable i zapisuje dane diagnostyczne do pliku CSV. Liczniki wydajności są zapisane do pliku CSV, można użyć graficznych możliwości programu Microsoft Excel lub inne narzędzie do wizualizacji danych. Pamiętaj dodać odwołania do Microsoft.WindowsAzure.Storage.dll, która jest uwzględniona w zestawie Azure SDK dla platformy .NET października 2012 lub nowszym. Zestaw jest instalowany w katalogu % Program Files%\Microsoft SDKs\Microsoft Azure.NET SDK\version-num\ref\.

```csharp
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Auth;
using Microsoft.WindowsAzure.Storage.Table;
...

// Get the connection string. When using Microsoft Azure Cloud Services, it is recommended
// you store your connection string using the Microsoft Azure service configuration
// system (*.csdef and *.cscfg files). You can you use the CloudConfigurationManager type
// to retrieve your storage connection string.  If you're not using Cloud Services, it's
// recommended that you store the connection string in your web.config or app.config file.
// Use the ConfigurationManager type to retrieve your storage connection string.

string connectionString = Microsoft.WindowsAzure.CloudConfigurationManager.GetSetting("StorageConnectionString");
//string connectionString = System.Configuration.ConfigurationManager.ConnectionStrings["StorageConnectionString"].ConnectionString;

// Get a reference to the storage account using the connection string.  You can also use the development
// storage account (Storage Emulator) for local debugging.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString);
//CloudStorageAccount storageAccount = CloudStorageAccount.DevelopmentStorageAccount;

// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create the CloudTable object that represents the "WADPerformanceCountersTable" table.
CloudTable table = tableClient.GetTableReference("WADPerformanceCountersTable");

// Create the table query, filter on a specific CounterName, DeploymentId and RoleInstance.
TableQuery<PerformanceCountersEntity> query = new TableQuery<PerformanceCountersEntity>()
  .Where(
    TableQuery.CombineFilters(
      TableQuery.GenerateFilterCondition("CounterName", QueryComparisons.Equal, @"\Processor(_Total)\% Processor Time"),
      TableOperators.And,
      TableQuery.CombineFilters(
      TableQuery.GenerateFilterCondition("DeploymentId", QueryComparisons.Equal, "ec26b7a1720447e1bcdeefc41c4892a3"),
      TableOperators.And,
      TableQuery.GenerateFilterCondition("RoleInstance", QueryComparisons.Equal, "WebRole1_IN_0")
    )
  )
);

// Execute the table query.
IEnumerable<PerformanceCountersEntity> result = table.ExecuteQuery(query);

// Process the query results and build a CSV file.
StringBuilder sb = new StringBuilder("TimeStamp,EventTickCount,DeploymentId,Role,RoleInstance,CounterName,CounterValue\n");

foreach (PerformanceCountersEntity entity in result)
{
  sb.Append(entity.Timestamp + "," + entity.EventTickCount + "," + entity.DeploymentId + ","
    + entity.Role + "," + entity.RoleInstance + "," + entity.CounterName + "," + entity.CounterValue+"\n");
}

StreamWriter sw = File.CreateText(@"C:\temp\PerfCounters.csv");
sw.Write(sb.ToString());
sw.Close();
```

Jednostki są mapowane do obiektów C# za pomocą niestandardowej klasy pochodzącej od **TableEntity**. Poniższy kod definiuje klasę jednostki, która reprezentuje licznika wydajności w **WADPerformanceCountersTable** tabeli.

```csharp
public class PerformanceCountersEntity : TableEntity
{
  public long EventTickCount { get; set; }
  public string DeploymentId { get; set; }
  public string Role { get; set; }
  public string RoleInstance { get; set; }
  public string CounterName { get; set; }
  public double CounterValue { get; set; }
}
```

## <a name="next-steps"></a>Następne kroki
[Przeglądanie dodatkowych artykułów na diagnostyki Azure](../azure-diagnostics.md)
