---
title: Dodawanie monitorowania i diagnostyki do maszyny wirtualnej platformy Azure | Dokumentacja firmy Microsoft
description: "Szablon usługi Azure resource manager umożliwia utworzenie nowej maszyny wirtualnej systemu Windows z rozszerzeniem diagnostycznych platformy Azure."
services: virtual-machines-windows
documentationcenter: 
author: sbtron
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 8cde8fe7-977b-43d2-be74-ad46dc946058
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/31/2017
ms.author: saurabh
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 6955e3d8c7b032ee898be11e611080905b5069ba
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="use-monitoring-and-diagnostics-with-a-windows-vm-and-azure-resource-manager-templates"></a>Użyj monitorowania i diagnostyki z szablonów maszyny Wirtualnej systemu Windows i usługi Azure Resource Manager
Rozszerzenie diagnostyki Azure udostępnia, monitorowania i diagnostyki możliwości w systemie Windows na podstawie maszyny wirtualnej platformy Azure. Te możliwości w maszynie wirtualnej można włączyć w tym rozszerzenia jako część szablonu usługi azure resource manager. Zobacz [tworzenia szablonów usługi Azure Resource Manager z rozszerzeń maszyny Wirtualnej](template-description.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#extensions) uzyskać więcej informacji o tym wszystkie rozszerzenia w ramach szablonu maszyny wirtualnej. W tym artykule opisano, jak dodać rozszerzenie diagnostyki Azure do szablonu maszyny wirtualnej systemu windows.  

## <a name="add-the-azure-diagnostics-extension-to-the-vm-resource-definition"></a>Dodaj rozszerzenie diagnostyki Azure w definicji zasobu maszyny Wirtualnej
Aby włączyć rozszerzenie diagnostyki na maszynie wirtualnej systemu Windows należy dodać rozszerzenie jako zasób maszyny Wirtualnej w szablonu usługi Resource manager.

Dla prostego Menedżera zasobów konfigurację rozszerzenia, aby dodać oparte na maszynie wirtualnej *zasobów* tablicy dla maszyny wirtualnej: 

    "resources": [
                {
                    "name": "Microsoft.Insights.VMDiagnosticsSettings",
                    "type": "extensions",
                    "location": "[resourceGroup().location]",
                    "apiVersion": "2015-06-15",
                    "dependsOn": [
                        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
                    ],
                    "tags": {
                        "displayName": "AzureDiagnostics"
                    },
                    "properties": {
                        "publisher": "Microsoft.Azure.Diagnostics",
                        "type": "IaaSDiagnostics",
                        "typeHandlerVersion": "1.5",
                        "autoUpgradeMinorVersion": true,
                        "settings": {
                            "xmlCfg": "[base64(concat(variables('wadcfgxstart'), variables('wadmetricsresourceid'), variables('vmName'), variables('wadcfgxend')))]",
                            "storageAccount": "[parameters('existingdiagnosticsStorageAccountName')]"
                        },
                        "protectedSettings": {
                            "storageAccountName": "[parameters('existingdiagnosticsStorageAccountName')]",
                            "storageAccountKey": "[listkeys(variables('accountid'), '2015-05-01-preview').key1]",
                            "storageAccountEndPoint": "https://core.windows.net"
                        }
                    }
                }
            ]


Inny wspólnej Konwencji jest dodać Konfiguracja rozszerzenia w węźle głównym zasobów szablonu zamiast definiować go w węźle zasobów maszyny wirtualnej. Z tej metody należy jawnie określić hierarchiczną relację między rozszerzenia i maszynę wirtualną z *nazwa* i *typu* wartości. Na przykład: 

    "name": "[concat(variables('vmName'),'Microsoft.Insights.VMDiagnosticsSettings')]",
    "type": "Microsoft.Compute/virtualMachines/extensions",

Rozszerzenie zawsze jest skojarzony z maszyną wirtualną, możesz bezpośrednio zdefiniować go bezpośrednio w węźle zasobu maszyny wirtualnej lub zdefiniować ją na poziomie podstawowym i umożliwia hierarchiczne konwencji nazewnictwa skojarzyć go z maszyny wirtualnej.

Zestawy skalowania maszyny wirtualnej konfiguracji rozszerzenia jest przewidziany w *extensionProfile* właściwość *VirtualMachineProfile*.

*Wydawcy* właściwość z wartością **Microsoft.Azure.Diagnostics** i *typu* właściwość z wartością **IaaSDiagnostics** jednoznacznie zidentyfikować rozszerzenia diagnostyki Azure.

Wartość *nazwa* właściwości może służyć do odwoływania się do rozszerzenia w grupie zasobów. Ustawienie jej specjalnie do **Microsoft.Insights.VMDiagnosticsSettings** umożliwi można łatwo zidentyfikować w portalu Azure, zapewniając, że monitorowanie wykresy Pokaż się poprawnie w portalu Azure.

*TypeHandlerVersion* Określa wersję rozszerzenia, o których chcesz użyć. Ustawienie *autoUpgradeMinorVersion* wersja pomocnicza do **true** zapewnia pobierze najnowszą wersję pomocniczą rozszerzenia, które jest dostępne. Zdecydowanie zaleca się, że możesz zawsze ustawić *autoUpgradeMinorVersion* zawsze być **true** tak, aby zawsze uzyskać próbę użycia najnowsze rozszerzenia diagnostyki dostępne nowe funkcje i poprawki błędów. 

*Ustawienia* element zawiera właściwości konfiguracji rozszerzenia, które można ustawić i ponownie odczytywana rozszerzenia (nazywane czasami publicznej konfiguracji). *Xmlcfg* właściwość zawiera konfigurację xml na podstawie dzienników diagnostycznych liczniki wydajności itp, które zostaną zebrane przez agenta diagnostyki. Zobacz [schemat konfiguracji diagnostyki](https://msdn.microsoft.com/library/azure/dn782207.aspx) uzyskać więcej informacji na temat samego schematu xml. Typowym rozwiązaniem jest przechowywanie rzeczywistego pliku xml konfiguracji jako zmienną w szablonie usługi Azure Resource Manager i następnie połącz i base64 kodować je, aby ustawić wartość *xmlcfg*. Zobacz sekcję dotyczącą [zmienne konfiguracji diagnostyki](#diagnostics-configuration-variables) Aby dowiedzieć się więcej o sposobie przechowywania xml w zmiennych. *StorageAccount* właściwość określa nazwę konta magazynu, do których diagnostyki będą przekazywane dane. 

Właściwości w *protectedSettings* (Konfiguracja czasami określone jako prywatne) można ustawić, ale nie można odczytać po ustawiany. Tylko do zapisu rodzaj *protectedSettings* ułatwia przydatne w przypadku przechowywania kluczy tajnych, takie jak klucz konta magazynu której będą zapisywane dane diagnostyczne.    

## <a name="specifying-diagnostics-storage-account-as-parameters"></a>Określanie konta magazynu diagnostyki jako parametrów
Powyżej fragment json rozszerzenia diagnostyki przyjmuje dwa parametry *existingdiagnosticsStorageAccountName* i *existingdiagnosticsStorageResourceGroup* Aby określić konto magazynu diagnostyki, w którym będą przechowywane dane diagnostyczne. Określenie konta magazynu diagnostyki jako parametr ułatwia łatwo zmienić konto magazynu diagnostyki w różnych środowiskach np. może chcesz użyć innego diagnostycznego konta magazynu do testowania i inną wdrożenia produkcyjnego.  

        "existingdiagnosticsStorageAccountName": {
            "type": "string",
            "metadata": {
        "description": "The name of an existing storage account to which diagnostics data will be transfered."
            }        
        },
        "existingdiagnosticsStorageResourceGroup": {
            "type": "string",
            "metadata": {
        "description": "The resource group for the storage account specified in existingdiagnosticsStorageAccountName"
              }
        }

Jest to najlepsze rozwiązanie, aby określić konto magazynu diagnostyki w innej grupie zasobów niż grupa zasobów dla maszyny wirtualnej. Grupa zasobów jest uznawana za jednostkę wdrożenia własnej okres istnienia, maszynę wirtualną można wdrożyć i wdrożone nowe aktualizacje konfiguracji zostały wprowadzone go do niego, ale można kontynuować przechowywanie danych diagnostycznych z tego samego konta magazynu przez te wdrożenia maszyny wirtualnej. Mając konto magazynu w różnych zasobów umożliwia konta magazynu, aby akceptować dane z różnych wdrożenia maszyny wirtualnej, co ułatwia rozwiązywanie problemów w różnych wersjach.

> [!NOTE]
> W przypadku utworzenia szablonu maszyny wirtualnej systemu windows z programu Visual Studio domyślne konto magazynu może być ustawiona do używania tego samego konta magazynu, gdzie jest przekazywany dysk VHD maszyny wirtualnej. To uprościć początkowej konfiguracji maszyny wirtualnej. Należy ponownie współczynnika szablonu, aby użyć innego konta magazynu, który może być przekazany jako parametr. 
> 
> 

## <a name="diagnostics-configuration-variables"></a>Zmienne konfiguracji diagnostyki
Definiuje powyżej fragment json rozszerzenia diagnostyki *accountid* zmiennej, aby uprościć pobieranie klucz konta magazynu dla magazynu diagnostyki:   

    "accountid": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/',parameters('existingdiagnosticsStorageResourceGroup'), '/providers/','Microsoft.Storage/storageAccounts/', parameters('existingdiagnosticsStorageAccountName'))]"


*Xmlcfg* właściwości rozszerzenia diagnostyki jest definiowana za pomocą wielu zmiennych, które są połączone ze sobą. Wartości tych zmiennych są w formacie xml, więc muszą być prawidłowo wpisywany podczas ustawiania zmiennych json.

Poniżej opisano plik xml konfiguracji diagnostyki, który zbiera dane liczników wydajności poziomu standardowych systemowych oraz niektóre dzienniki zdarzeń systemu windows i dzienników infrastruktury Diagnostyka. Ma zostały zmienione znaczenie i prawidłowo sformatowane, dzięki czemu konfiguracji można wkleić bezpośrednio do sekcji zmiennych szablonu. Zobacz [schemat konfiguracji diagnostyki](https://msdn.microsoft.com/library/azure/dn782207.aspx) więcej człowieka przykład do odczytu pliku konfiguracji XML.

        "wadlogs": "<WadCfg> <DiagnosticMonitorConfiguration overallQuotaInMB=\"4096\" xmlns=\"http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration\"> <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter=\"Error\"/> <WindowsEventLog scheduledTransferPeriod=\"PT1M\" > <DataSource name=\"Application!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"Security!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"System!*[System[(Level = 1 or Level = 2)]]\" /></WindowsEventLog>",
        "wadperfcounters1": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\"><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% Processor Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU utilization\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% Privileged Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU privileged time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% User Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU user time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Processor Information(_Total)\\Processor Frequency\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"CPU frequency\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\System\\Processes\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"Processes\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Process(_Total)\\Thread Count\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"Threads\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Process(_Total)\\Handle Count\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"Handles\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\% Committed Bytes In Use\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Memory usage\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\Available Bytes\" sampleRate=\"PT15S\" unit=\"Bytes\"><annotation displayName=\"Memory available\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\Committed Bytes\" sampleRate=\"PT15S\" unit=\"Bytes\"><annotation displayName=\"Memory committed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\Commit Limit\" sampleRate=\"PT15S\" unit=\"Bytes\"><annotation displayName=\"Memory commit limit\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\% Disk Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk active time\" locale=\"en-us\"/></PerformanceCounterConfiguration>",
        "wadperfcounters2": "<PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\% Disk Read Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk active read time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\% Disk Write Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk active write time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Transfers/sec\" sampleRate=\"PT15S\" unit=\"CountPerSecond\"><annotation displayName=\"Disk operations\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Reads/sec\" sampleRate=\"PT15S\" unit=\"CountPerSecond\"><annotation displayName=\"Disk read operations\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Writes/sec\" sampleRate=\"PT15S\" unit=\"CountPerSecond\"><annotation displayName=\"Disk write operations\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Bytes/sec\" sampleRate=\"PT15S\" unit=\"BytesPerSecond\"><annotation displayName=\"Disk speed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Read Bytes/sec\" sampleRate=\"PT15S\" unit=\"BytesPerSecond\"><annotation displayName=\"Disk read speed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Write Bytes/sec\" sampleRate=\"PT15S\" unit=\"BytesPerSecond\"><annotation displayName=\"Disk write speed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\LogicalDisk(_Total)\\% Free Space\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk free space (percentage)\" locale=\"en-us\"/></PerformanceCounterConfiguration></PerformanceCounters>",
        "wadcfgxstart": "[concat(variables('wadlogs'), variables('wadperfcounters1'), variables('wadperfcounters2'), '<Metrics resourceId=\"')]",
        "wadmetricsresourceid": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name , '/providers/', 'Microsoft.Compute/virtualMachines/')]",
        "wadcfgxend": "\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/><MetricAggregation scheduledTransferPeriod=\"PT1M\"/></Metrics></DiagnosticMonitorConfiguration></WadCfg>"

Węzeł xml definicji metryk w powyższej konfiguracji jest element konfiguracji ważne, ponieważ definiuje sposób liczników wydajności zdefiniowanych we wcześniejszej części kodu xml w *PerformanceCounter* węzła będą agregowane i przechowywane. 

> [!IMPORTANT]
> Te metryki dysku wykresy monitorowania i alertów w portalu Azure.  **Metryki** węzła o *resourceID* i **MetricAggregation** muszą być zawarte w konfiguracji diagnostyki dla maszyny Wirtualnej, jeśli chcesz wyświetlić dane w portalu Azure monitorowania maszyny Wirtualnej. 
> 
> 

Poniżej przedstawiono przykładowy kod xml definicji metryk: 

        <Metrics resourceId="/subscriptions/subscription().subscriptionId/resourceGroups/resourceGroup().name/providers/Microsoft.Compute/virtualMachines/vmName">
            <MetricAggregation scheduledTransferPeriod="PT1H"/>
            <MetricAggregation scheduledTransferPeriod="PT1M"/>
        </Metrics>

*ResourceID* atrybutu unikatowo identyfikuje maszyny wirtualnej w ramach subskrypcji. Upewnij się, że należy użyć funkcji subscription() i resourceGroup() tak, aby szablon automatycznie aktualizuje te wartości na podstawie subskrypcji i grupy zasobów, które wdrażasz.

Jeśli tworzysz wiele maszyn wirtualnych w pętli, musisz wypełnić *resourceID* wartości z funkcji copyIndex() poprawnie rozróżnianie każdego poszczególnych maszyn wirtualnych. *XmlCfg* wartość może zostać zaktualizowane do obsługi to w następujący sposób:  

    "xmlCfg": "[base64(concat(variables('wadcfgxstart'), variables('wadmetricsresourceid'), concat(parameters('vmNamePrefix'), copyindex()), variables('wadcfgxend')))]", 

Wartość MetricAggregation *PT1H* i *PT1M* oznaczającego agregacji na minutę i agregacji ponad godzinę.

## <a name="wadmetrics-tables-in-storage"></a>Tabele WADMetrics w magazynie
Konfiguracja metryki powyżej wygeneruje tabel na koncie magazynu diagnostyki z następujących konwencji nazewnictwa:

* **WADMetrics** : standardowego prefiksu dla wszystkich tabel WADMetrics
* **PT1H** lub **PT1M** : oznacza, że tabela zawiera agregowanie danych ponad 1 godzina lub 1 minuta
* **P10D** : oznacza, że tabela będzie zawierać danych dla 10 dni od uruchomienia tabeli na zbieranie danych
* **V2S** : stała znakowa
* **RRRRMMDD** : Data, w którym tabeli rozpoczęcia, zbieranie danych

Przykład: *WADMetricsPT1HP10DV2S20151108* zawierają zagregowane ponad godzinę 10 dni, począwszy od dnia 2015-11-lis dane metryk    

Każda tabela WADMetrics będzie zawierać następujące kolumny:

* **PartitionKey**: partitionkey jest tworzony na podstawie *resourceID* wartość do unikatowej identyfikacji zasobu maszyny Wirtualnej. na przykład: 002Fsubscriptions:<subscriptionID>: 002FresourceGroups:002F<ResourceGroupName>: 002Fproviders:002FMicrosoft:002ECompute:002FvirtualMachines:002F<vmName>  
* **RowKey** : zgodne z formatem <Descending time tick>:<Performance Counter Name>. Malejącej obliczanie osi czasu jest Takty maksymalny czas minus godzina rozpoczęcia okresu agregacji. Na przykład Jeśli okres próbki uruchomione w dniu 2015-10-lis i będzie 00:00Hrs UTC, a następnie obliczenia: DateTime.MaxValue.Ticks - (DateTime(2015,11,10,0,0,0,DateTimeKind.Utc) nowe. Znaczniki). Dla pamięci klucz wiersza danych licznika wydajności liczba dostępnych bajtów będzie wyglądała, takich jak: 2519551871999999999__:005CMemory:005CAvailable:0020 bajtów
* **CounterName** : Nazwa licznika wydajności. Odpowiada *counterSpecifier* zdefiniowane w pliku konfiguracji xml.
* **Maksymalna** : maksymalna wartość licznika wydajności w okresie agregacji.
* **Minimalna** : minimalną wartość licznika wydajności w okresie agregacji.
* **Całkowita liczba** : sumę wszystkich wartości licznika wydajności zgłoszonych w okresie agregacji.
* **Liczba** : zgłosił sumę wartości licznika wydajności.
* **Średnia** : wartość średnia (łączna/liczba) licznika wydajności w okresie agregacji.

## <a name="next-steps"></a>Następne kroki
* Dla kompletnego przykładowego szablonu maszyny wirtualnej systemu Windows z rozszerzeniem diagnostyki [201-rozszerzenia maszyny wirtualnej — monitorowanie diagnostycznych](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-monitoring-diagnostics-extension)   
* Wdrażanie, używając szablonu Menedżera zasobów [programu Azure PowerShell](ps-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) lub [wiersza polecenia platformy Azure](../linux/create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* Dowiedz się więcej o [tworzenia szablonów usługi Azure Resource Manager](../../resource-group-authoring-templates.md)

