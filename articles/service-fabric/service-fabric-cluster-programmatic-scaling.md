---
title: "Azure Usługa sieci szkieletowej programowe skalowanie | Dokumentacja firmy Microsoft"
description: "Skalowanie klastra usługi sieć szkieletowa usług Azure lub programistycznie, zgodnie z wyzwalaczy niestandardowych"
services: service-fabric
documentationcenter: .net
author: mjrousos
manager: jonjung
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/17/2017
ms.author: mikerou
ms.openlocfilehash: 1744e3c49ac06abe9e1067d507fd56d694201ffc
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
# <a name="scale-a-service-fabric-cluster-programmatically"></a>Programowo skalowanie klastra sieci szkieletowej usług 

Podstawowe informacje na temat skalowania klastra sieci szkieletowej usług na platformie Azure opisano w dokumentacji na [skalowanie klastra](./service-fabric-cluster-scale-up-down.md). Tym artykule opisano sposób klastrów sieci szkieletowej usług są zbudowane na podstawie zestawy skalowania maszyny wirtualnej i mogą być skalowane ręcznie lub przy użyciu reguł automatycznego skalowania. Ten dokument analizuje metod programistycznych koordynujący Azure operacji skalowania dla bardziej zaawansowanych scenariuszy. 

## <a name="reasons-for-programmatic-scaling"></a>Przyczyny skalowanie programowe
W wielu scenariuszach skalowanie ręcznie lub za pomocą reguł automatycznego skalowania są dobrym rozwiązania. W innych sytuacjach jednak mogą nie być rozwiązanie właściwe. Wady do tych metod uwzględnić:

- Ręcznie skalowanie należy zalogować się i jawne żądanie operacji skalowania. Operacje skalowania wymagane są często lub w czasie nieprzewidywalne, ta metoda nie może być dobrym rozwiązaniem.
- Usunięcie reguły automatycznego skalowania wystąpienia z zestawu skalowania maszyn wirtualnych, ich nie automatycznie usuwaj wiedzę na temat tego węzła z klastra usługi sieć szkieletowa skojarzone chyba, że typ węzła ma poziom trwałości Silver lub Gold. Ponieważ reguły automatycznego skalowania działają na dużą skalę, Ustaw poziom (a nie na poziomie sieci szkieletowej usług), reguły automatycznego skalowania można usunąć węzłów sieci szkieletowej usług bez zamykania bezpiecznie zamknąć. Usunięcie węzła niegrzeczny pozostawi "widma" stan węzła sieci szkieletowej usług po operacji w skali. Osoby (lub usługą) należy okresowo Wyczyść stan usuniętym węźle w klastrze usługi sieć szkieletowa usług.
  - Typ węzła z poziomu trwałości Gold lub Silver automatycznie oczyszcza usuniętych węzłów, więc nie jest wymagane nie dodatkowe oczyszczanie.
- Mimo że istnieją [wiele metryk](../monitoring-and-diagnostics/insights-autoscale-common-metrics.md) obsługiwane przez reguły automatycznego skalowania, nadal jest ograniczony zestaw. Jeśli scenariusz wymaga skalowanie oparte na niektóre metryki nieuwzględnionego w tym zestawie, następnie reguły automatycznego skalowania nie może być dobrym rozwiązaniem.

W oparciu o te ograniczenia, można zaimplementować więcej dostosowane automatycznego skalowania modeli. 

## <a name="scaling-apis"></a>Skalowanie interfejsów API
Interfejsy API Azure istnieje, co umożliwia aplikacjom programowo pracować z maszyną wirtualną skalowanie zestawów i klastrów sieci szkieletowej usług. Jeśli istniejących opcji automatycznego skalowania nie działa dla danego scenariusza, te interfejsy API umożliwiają wdrożenie niestandardowej logiki skalowania. 

Jednym z podejść do wykonania tej "wprowadzone głównej" funkcji skalowania automatycznego jest dodania nowej usługi bezstanowej do aplikacji sieci szkieletowej usług do zarządzania operacjami skalowania. W ramach usługi `RunAsync` metody, zestawu wyzwalaczy można określić, czy skalowanie jest wymagana (w tym sprawdzanie parametry, takie jak maksymalna wielkość klastra i skalowanie cooldowns).   

Interfejs API, używany do interakcji zestaw skali maszyny wirtualnej, (zarówno do sprawdzenia bieżąca liczba wystąpień maszyn wirtualnych, a następnie zmodyfikować go) jest [fluent biblioteki Azure zarządzania obliczeniowe](https://www.nuget.org/packages/Microsoft.Azure.Management.Compute.Fluent/). Biblioteka fluent obliczeń zapewnia łatwy w użyciu interfejsu API do interakcji z zestawy skalowania maszyny wirtualnej.

Aby pracować interaktywnie z klastra usługi sieć szkieletowa, użyj [System.Fabric.FabricClient](/dotnet/api/system.fabric.fabricclient).

Oczywiście skalowania kodu nie trzeba uruchamiać jako usługi w klastrze, który ma być skalowana w. Zarówno `IAzure` i `FabricClient` mogą łączyć się z zasobami platformy Azure skojarzone zdalnie, więc skalowania usługi można łatwo aplikacji konsoli lub usługa systemu Windows uruchomiony za pośrednictwem poza aplikacją sieci szkieletowej usług. 

## <a name="credential-management"></a>Zarządzanie poświadczeniami
Jednym z wyzwań zapisywania usługi do obsługi skalowania jest, że usługi musi być możliwy dostęp do zasobów zestawu skali maszyny wirtualnej bez logowania interakcyjnego. Uzyskiwanie dostępu do klastra usługi sieć szkieletowa jest proste, jeśli skalowania usługi modyfikuje własnej aplikacji usługi Service Fabric, ale poświadczenia są wymagane do zestawu skalowania. Aby zalogować się, można użyć [nazwy głównej usługi](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli) utworzone za pomocą [Azure CLI 2.0](https://github.com/azure/azure-cli).

Nazwy głównej usługi mogą być tworzone z następujących kroków:

1. Zaloguj się do wiersza polecenia platformy Azure (`az login`) jako użytkownik z dostępem do skalowania maszyny wirtualnej ustawić
2. Tworzenie nazwy głównej z usługi`az ad sp create-for-rbac`
    1. Zanotuj identyfikator aplikacji (nazywane "identyfikator klienta" w innym miejscu), nazwa, hasło i dzierżawy w celu późniejszego użycia.
    2. Należy również identyfikator subskrypcji można wyświetlić w programie`az account list`

Biblioteka fluent obliczeń zalogować się przy użyciu tych poświadczeń w następujący sposób (należy pamiętać, że fluent Azure typów podstawowych, takich jak `IAzure` w [Microsoft.Azure.Management.Fluent](https://www.nuget.org/packages/Microsoft.Azure.Management.Fluent/) pakietu):

```csharp
var credentials = new AzureCredentials(new ServicePrincipalLoginInformation {
                ClientId = AzureClientId,
                ClientSecret = 
                AzureClientKey }, AzureTenantId, AzureEnvironment.AzureGlobalCloud);
IAzure AzureClient = Azure.Authenticate(credentials).WithSubscription(AzureSubscriptionId);

if (AzureClient?.SubscriptionId == AzureSubscriptionId)
{
    ServiceEventSource.Current.ServiceMessage(Context, "Successfully logged into Azure");
}
else
{
    ServiceEventSource.Current.ServiceMessage(Context, "ERROR: Failed to login to Azure");
}
```

Po zalogowaniu, liczba wystąpień zestawu skali można tworzyć zapytania za pomocą `AzureClient.VirtualMachineScaleSets.GetById(ScaleSetId).Capacity`.

## <a name="scaling-out"></a>Skalowanie w poziomie
Przy użyciu fluent Azure obliczeniowe zestawu SDK, wystąpień, mogą być dodawane do zestaw z kilku wywołania - skalowania maszyny wirtualnej

```csharp
var scaleSet = AzureClient.VirtualMachineScaleSets.GetById(ScaleSetId);
var newCapacity = (int)Math.Min(MaximumNodeCount, scaleSet.Capacity + 1);
scaleSet.Update().WithCapacity(newCapacity).Apply(); 
``` 

Alternatywnie rozmiaru zestawu skali maszyny wirtualnej, można też zarządzać za pomocą poleceń cmdlet programu PowerShell. [`Get-AzureRmVmss`](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmss)można pobrać obiektu zestawu skali maszyny wirtualnej. Pojemność bieżąca jest dostępna za pośrednictwem `.sku.capacity` właściwości. Po zmianie pojemność na żądaną wartość, skalowania maszyny wirtualnej w usłudze Azure można zaktualizować za pomocą [ `Update-AzureRmVmss` ](https://docs.microsoft.com/powershell/module/azurerm.compute/update-azurermvmss) polecenia.

Podczas dodawania węzła ręcznie, dodając wystąpienia zestawu skalowania powinny być wszystkie punkty, które ma potrzebne do uruchomienia nowego węzła sieci szkieletowej usług, ponieważ szablon zestawu skalowania obejmują rozszerzenia automatycznie dołączy do klastra usługi sieć szkieletowa nowych wystąpień. 

## <a name="scaling-in"></a>Skalowanie w

Skalowanie w jest podobny do skalowania. Rzeczywiste zestawu skalowania maszyn wirtualnych zmiany praktycznie są takie same. Jednak jak został już wcześniej, usługa sieć szkieletowa tylko automatycznie oczyszcza usuniętych węzłów trwałości Gold lub Silver. Tak, w tym trwałości brązowa skalowania w przypadku należy interakcje z klastrem usługi sieć szkieletowa można zamknąć węzeł ma zostać usunięty, a następnie usunąć jego stanu.

Przygotowanie węzła dla zamknięcia polega na znajdowaniu się, że węzeł, który ma być usunięty (węzeł ostatnio dodane) i dezaktywowanie go. Dla węzłów z systemem innym niż inicjatora nowszej węzły znajdują się na podstawie porównania ilości `NodeInstanceId`. 

```csharp
using (var client = new FabricClient())
{
    var mostRecentLiveNode = (await client.QueryManager.GetNodeListAsync())
        .Where(n => n.NodeType.Equals(NodeTypeToScale, StringComparison.OrdinalIgnoreCase))
        .Where(n => n.NodeStatus == System.Fabric.Query.NodeStatus.Up)
        .OrderByDescending(n => n.NodeInstanceId)
        .FirstOrDefault();
```

Węzłów inicjatora są różne, a nie zawsze wykonaj Konwencji najpierw usunąć większa identyfikatorów wystąpienia.

Po znalezieniu węzeł ma zostać usunięty, można dezaktywować i usunąć korzystającej z tego samego `FabricClient` wystąpienia i `IAzure` wystąpienie z wcześniej.

```csharp
var scaleSet = AzureClient.VirtualMachineScaleSets.GetById(ScaleSetId);

// Remove the node from the Service Fabric cluster
ServiceEventSource.Current.ServiceMessage(Context, $"Disabling node {mostRecentLiveNode.NodeName}");
await client.ClusterManager.DeactivateNodeAsync(mostRecentLiveNode.NodeName, NodeDeactivationIntent.RemoveNode);

// Wait (up to a timeout) for the node to gracefully shutdown
var timeout = TimeSpan.FromMinutes(5);
var waitStart = DateTime.Now;
while ((mostRecentLiveNode.NodeStatus == System.Fabric.Query.NodeStatus.Up || mostRecentLiveNode.NodeStatus == System.Fabric.Query.NodeStatus.Disabling) &&
        DateTime.Now - waitStart < timeout)
{
    mostRecentLiveNode = (await client.QueryManager.GetNodeListAsync()).FirstOrDefault(n => n.NodeName == mostRecentLiveNode.NodeName);
    await Task.Delay(10 * 1000);
}

// Decrement VMSS capacity
var newCapacity = (int)Math.Max(MinimumNodeCount, scaleSet.Capacity - 1); // Check min count 

scaleSet.Update().WithCapacity(newCapacity).Apply(); 
```

Jako ze skalowania poleceń cmdlet programu PowerShell modyfikowania skalowania maszyny wirtualnej zestawu pojemności można także tutaj Jeśli skryptów podejście jest bardziej pożądane. Po usunięciu wystąpienie maszyny wirtualnej, można usunąć stan węzła sieci szkieletowej usług.

```csharp
await client.ClusterManager.RemoveNodeStateAsync(mostRecentLiveNode.NodeName);
```

## <a name="potential-drawbacks"></a>Wady

Jak pokazano w poprzedzających fragmentów kodu, tworzenia własnego skalowania usługi zapewnia najwyższy stopień kontroli i dostosowywalności za pośrednictwem aplikacji na skalowanie. Może to być przydatne w scenariuszach wymagających precyzyjną kontrolę, kiedy i jak aplikacja skaluje przychodzący lub wychodzący. Jednak ten formant pochodzi z zależnościami złożoności kodu. Przy użyciu tej metody oznacza, że konieczne do własnych skalowania kod, który jest nieuproszczony.

Jak powinna wynosić skalowania usługi Service Fabric, zależy od danego scenariusza. W przypadku skalowania rzadko, możliwość dodawania lub usuwania węzłów ręcznie jest prawdopodobnie wystarczający. Dla bardziej złożonymi scenariuszami reguły automatyczne skalowanie i zestawy SDK udostępnia możliwość skalowania programowo oferują zaawansowane alternatyw.

## <a name="next-steps"></a>Kolejne kroki

Aby rozpocząć wdrażanie własną logikę automatyczne skalowanie, należy zapoznać się z przydatne interfejsów API i następujące kwestie:

- [Skalowanie ręcznie lub przy użyciu reguł automatycznego skalowania](./service-fabric-cluster-scale-up-down.md)
- [Fluent biblioteki zarządzania platformy Azure dla platformy .NET](https://github.com/Azure/azure-sdk-for-net/tree/Fluent) (przydatne w przypadku interakcji z podstawowej zestawy skalowania maszyny wirtualnej w klastrze usługi sieć szkieletowa)
- [System.Fabric.FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient) (przydatne w przypadku interakcji z klastra sieci szkieletowej usług i jego węzły)
