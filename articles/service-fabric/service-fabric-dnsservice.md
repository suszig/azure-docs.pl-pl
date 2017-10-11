---
title: "Usługa Azure Service Fabric DNS | Dokumentacja firmy Microsoft"
description: "Za pomocą usługi dns platformy Service Fabric wykrywania mikrousług z wewnątrz klastra."
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: vturecek
ms.assetid: 47f5c1c1-8fc8-4b80-a081-bc308f3655d3
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/27/2017
ms.author: msfussell
ms.openlocfilehash: 9871bc5aa4e74ab0faef401d67c4e9558eb5e14b
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/18/2017
---
# <a name="dns-service-in-azure-service-fabric"></a>Usługa DNS w sieci szkieletowej usług Azure
Usługa DNS jest opcjonalny systemu usługi można włączyć w klastrze do odnajdywania innych usług za pomocą protokołu DNS.

Wiele usług, zwłaszcza konteneryzowanych usług, może mieć nazwę istniejącego adresu URL, a możliwość rozwiązać je przy użyciu standardowego protokołu DNS (a nie protokołu Naming Service) jest pożądane, szczególnie w scenariuszach "przyrostu i przesunięcia". Usługa DNS umożliwia mapowanie nazwy DNS na nazwę usługi i dlatego rozpoznać adresów IP punktu końcowego. 

Usługa DNS mapuje nazwy DNS nazwy usługi, które z kolei są rozpoznawane przez usługę nazewnictwa do zwracania punktu końcowego usługi. Nazwy DNS dla usługi znajduje się w momencie tworzenia obiektu. 

![Punkty końcowe usługi][0]

## <a name="enabling-the-dns-service"></a>Włączanie usługi DNS
Najpierw należy włączyć usługę DNS w klastrze. Pobierz szablon dla klastra, który chcesz wdrożyć. Można użyć [przykładowy szablon](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype) lub Utwórz szablon usługi Resource Manager. Można włączyć usługę DNS z następujących kroków:

1. Sprawdź, czy `apiversion` ustawiono `2017-07-01-preview` dla `Microsoft.ServiceFabric/clusters` zasobów i jeśli nie, zaktualizować go jak pokazano w poniższy fragment kodu:

    ```json
    {
        "apiVersion": "2017-07-01-preview",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
    }
    ```

2. Teraz włączyć usługę DNS przez dodanie poniższego `addonFeatures` sekcji po `fabricSettings` sekcji, jak pokazano w poniższy fragment kodu: 

    ```json
        "fabricSettings": [
        ...      
        ],
        "addonFeatures": [
            "DnsService"
        ],
    ```

3. Po aktualizacji szablonu klastra z poprzednim zmiany ich zastosowania i umożliwić Uaktualnianie ukończone. Po wykonaniu tych czynności, usługa systemu DNS uruchamiania w klastrze, który jest nazywany `fabric:/System/DnsService` sekcji usługi systemu w programie Service Fabric explorer. 

Alternatywnie można włączyć usługę DNS za pośrednictwem portalu w czasie tworzenia klastra. Usługa DNS można ją włączyć przez zaznaczenie pola wyboru dla `Include DNS service` w `Cluster configuration` menu, jak pokazano na poniższym zrzucie ekranu:

![Włączanie usługi DNS za pośrednictwem portalu][2]


## <a name="setting-the-dns-name-for-your-service"></a>Ustawienie nazwy DNS dla usługi
Gdy usługa DNS działa w klastrze, można ustawić nazwy DNS dla usługi deklaratywnie dla usług domyślnych w `ApplicationManifest.xml` lub za pomocą poleceń programu Powershell.

### <a name="setting-the-dns-name-for-a-default-service-in-the-applicationmanifestxml"></a>Ustawienie nazwy DNS dla usługi domyślne w pliku ApplicationManifest.xml
Otwórz projekt w Visual Studio lub ulubionego edytora, a `ApplicationManifest.xml` pliku. Przejdź do sekcji domyślnej usługi i dla każdej usługi, dodać `ServiceDnsName` atrybutu. Poniższy przykład pokazuje, jak ustawić nazwę DNS usługi`service1.application1`

```xml
    <Service Name="Stateless1" ServiceDnsName="service1.application1">
    <StatelessService ServiceTypeName="Stateless1Type" InstanceCount="[Stateless1_InstanceCount]">
        <SingletonPartition />
    </StatelessService>
    </Service>
```
Gdy aplikacja jest wdrażana, wystąpienie usługi w programie Service Fabric explorer zawiera nazwę DNS dla tego wystąpienia, jak pokazano na poniższej ilustracji: 

![Punkty końcowe usługi][1]

### <a name="setting-the-dns-name-for-a-service-using-powershell"></a>Ustawienie nazwy DNS dla usługi przy użyciu programu Powershell
Można ustawić nazwy DNS dla usługi, tworząc go za pomocą `New-ServiceFabricService` środowiska Powershell. Poniższy przykład tworzy nową usługę bezstanowych z nazwą DNS`service1.application1`

```powershell
    New-ServiceFabricService `
    -Stateless `
    -PartitionSchemeSingleton `
    -ApplicationName `fabric:/application1 `
    -ServiceName fabric:/application1/service1 `
    -ServiceTypeName Service1Type `
    -InstanceCount 1 `
    -ServiceDnsName service1.application1
```

## <a name="using-dns-in-your-services"></a>Przy użyciu systemu DNS w usługach
Jeśli wdrożono więcej niż jedna usługa, można znaleźć punktów końcowych z innymi usługami w celu komunikowania się z przy użyciu nazwy DNS. Usługa DNS ma zastosowanie tylko do usług bezstanowych, ponieważ protokół DNS nie może komunikować się z usługami stanowych. Dla stanowych usług można użyć wbudowanych zwrotny serwer proxy dla połączenia http do wywołania partycji określonej usługi.

Poniższy kod przedstawia sposób wywołania innej usługi, która to po prostu wywołanie regularne http, w którym podać port i dowolną ścieżkę opcjonalne jako część adresu URL.

```csharp
public class ValuesController : Controller
{
    // GET api
    [HttpGet]
    public async Task<string> Get()
    {
        string result = "";
        try
        {
            Uri uri = new Uri("http://service1.application1:8080/api/values");
            HttpClient client = new HttpClient();
            var response = await client.GetAsync(uri);
            result = await response.Content.ReadAsStringAsync();
            
        }
        catch (Exception e)
        {
            Console.Write(e.Message);
        }

        return result;
    }
}
```

## <a name="next-steps"></a>Następne kroki
Dowiedz się więcej na temat usługi komunikacji w klastrze z [połączenia i łączyć się z usługami](service-fabric-connect-and-communicate-with-services.md)

[0]: ./media/service-fabric-connect-and-communicate-with-services/dns.png
[1]: ./media/service-fabric-dnsservice/servicefabric-explorer-dns.PNG
[2]: ./media/service-fabric-dnsservice/DNSService.PNG
