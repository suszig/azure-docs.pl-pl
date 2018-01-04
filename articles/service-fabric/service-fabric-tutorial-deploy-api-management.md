---
title: "Integrowanie usługi Azure Service Fabric z interfejsu API zarządzania | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak szybko rozpocząć pracę z usługi Azure API Management i sieci szkieletowej usług."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/10/2017
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: c685e5250943098f43f232b2b09d3ae55c0380d0
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/03/2018
---
# <a name="deploy-api-management-with-service-fabric"></a>Wdrażanie interfejsu API zarządzania za pomocą sieci szkieletowej usług
W tym samouczku jest częścią czterech serii.  Wdrażanie usługi Azure API Management z sieci szkieletowej usług jest zaawansowanym scenariuszu.  Zarządzanie interfejsami API jest przydatne, gdy należy opublikować interfejsy API z bogaty zestaw reguły routingu dla usług sieci szkieletowej usług zaplecza. Aplikacje w chmurze muszą zwykle frontonu bramy do zapewnienia pojedynczy punkt wejściowych użytkowników, urządzeń lub innych aplikacji. W sieci szkieletowej usług Brama może być dowolnej usługi bezstanowej przeznaczone dla ruchu transfer danych przychodzących przykład aplikacji platformy ASP.NET Core, Event Hubs, Centrum IoT lub usługi Azure API Management. 

W tym samouczku przedstawiono sposób konfigurowania [Azure API Management](../api-management/api-management-key-concepts.md) z sieci szkieletowej usług można kierować ruchem do usługi zaplecza w sieci szkieletowej usług.  Po zakończeniu ma wdrożyć zarządzanie interfejsami API w sieci Wirtualnej, skonfigurowane do wysyłania ruchu do zaplecza usługi bezstanowej operacji interfejsu API. Aby dowiedzieć się więcej o scenariuszach usługi Azure API Management z sieci szkieletowej usług, zobacz [omówienie](service-fabric-api-management-overview.md) artykułu.

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Wdrażanie usługi API Management
> * Skonfiguruj zarządzanie interfejsami API
> * Tworzenie operacji interfejsu API
> * Skonfiguruj zasady wewnętrznej bazy danych
> * Dodawanie interfejsu API do produktu

W tym samouczku dowiesz się, jak:
> [!div class="checklist"]
> * Tworzenie bezpiecznej [klastra systemu Windows](service-fabric-tutorial-create-vnet-and-windows-cluster.md) lub [klaster systemu Linux](service-fabric-tutorial-create-vnet-and-linux-cluster.md) na platformie Azure przy użyciu szablonu
> * [Skalowanie klastra przychodzący lub wychodzący](/service-fabric-tutorial-scale-cluster.md)
> * [Uaktualnienie środowiska uruchomieniowego klastra](service-fabric-tutorial-upgrade-cluster.md)
> * Wdrażanie interfejsu API zarządzania za pomocą sieci szkieletowej usług

## <a name="prerequisites"></a>Wymagania wstępne
Przed rozpoczęciem tego samouczka:
- Jeśli nie masz subskrypcji platformy Azure, Utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- Zainstaluj [programu Azure Powershell w wersji modułu 4.1 lub wyższej](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) lub [Azure CLI 2.0](/cli/azure/install-azure-cli).
- Tworzenie bezpiecznej [klastra systemu Windows](service-fabric-tutorial-create-vnet-and-windows-cluster.md) lub [klaster systemu Linux](service-fabric-tutorial-create-vnet-and-linux-cluster.md) na platformie Azure
- W przypadku wdrażania klastra systemu Windows, konfigurowanie środowiska projektowego systemu Windows. Zainstaluj [programu Visual Studio 2017](http://www.visualstudio.com) i **Azure programowanie**, **ASP.NET i sieć web development**, i **aplikacji dla wielu platform .NET Core**obciążeń.  Następnie skonfiguruj [środowiska programowania .NET](service-fabric-get-started.md).
- Jeśli w przypadku wdrażania klastra Linux należy skonfigurować środowisko projektowe Java na [Linux](service-fabric-get-started-linux.md) lub [MacOS](service-fabric-get-started-mac.md).  Zainstaluj [usługi sieci szkieletowej interfejsu wiersza polecenia](service-fabric-cli.md). 

## <a name="network-topology"></a>Topologia sieci
Teraz, gdy masz bezpiecznej [klastra systemu Windows](service-fabric-tutorial-create-vnet-and-windows-cluster.md) lub [klaster systemu Linux](service-fabric-tutorial-create-vnet-and-linux-cluster.md) na platformie Azure, wdrożenia interfejs API zarządzania siecią wirtualną (VNET) w podsieci i NSG przeznaczone dla interfejsu API zarządzania. W tym samouczku jest wstępnie skonfigurowana do używania nazwy sieci Wirtualnej, podsieci i NSG, skonfigurowanym w poprzednim szablonu usługi Resource Manager Management API [samouczek klastra systemu Windows](service-fabric-tutorial-create-vnet-and-windows-cluster.md) lub [samouczka klaster systemu Linux](service-fabric-tutorial-create-vnet-and-linux-cluster.md). W tym samouczku wdraża następujących topologii na platformie Azure, w którym interfejs API zarządzania i sieci szkieletowej usług znajdują się w podsieci w tej samej sieci wirtualnej:

 ![Podpis obrazu][sf-apim-topology-overview]

## <a name="sign-in-to-azure-and-select-your-subscription"></a>Logowanie do platformy Azure i wyboru subskrypcji
Zaloguj się na konto Azure Wybierz subskrypcję, przed wykonaniem polecenia platformy Azure.

```powershell
Login-AzureRmAccount
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>
```

```azurecli
az login
az account set --subscription <guid>
```

## <a name="deploy-a-service-fabric-back-end-service"></a>Wdrażanie usługi zaplecza sieci szkieletowej usług

Przed skonfigurowaniem API Management do kierowania ruchem do usługi zaplecza usługi Service Fabric, należy najpierw uruchomionej usługi do akceptowania żądań.  Jeśli wcześniej została utworzona [klastra systemu Windows](service-fabric-tutorial-create-vnet-and-windows-cluster.md), wdrożenie usługi sieć szkieletowa usług .NET.  Jeśli wcześniej została utworzona [klaster systemu Linux](service-fabric-tutorial-create-vnet-and-linux-cluster.md), wdrożenie usługi sieć szkieletowa usług Java.

### <a name="deploy-a-net-service-fabric-service"></a>Wdrażanie usługi sieci szkieletowej usług .NET

W tym samouczku Utwórz podstawowe bezstanowej platformy ASP.NET Core niezawodnej usługi przy użyciu domyślnego szablonu projektu interfejsu API sieci Web. Spowoduje to utworzenie punktu końcowego HTTP dla usługi, które należy udostępnić za pośrednictwem usługi Azure API Management.

Uruchom program Visual Studio jako Administrator i Utwórz usługę platformy ASP.NET Core:

 1. W programie Visual Studio wybierz Plik -> Nowy projekt.
 2. Wybierz szablon aplikacji sieci szkieletowej usług w chmurze i nadaj mu nazwę **"ApiApplication"**.
 3. Wybierz szablon usługi podstawowej platformy ASP.NET i nazwij projekt **"WebApiService"**.
 4. Wybierz szablon projektu sieci Web API platformy ASP.NET Core 1.1.
 5. Po utworzeniu projektu otwórz `PackageRoot\ServiceManifest.xml` i Usuń `Port` atrybut z konfiguracji zasobu punktu końcowego:
 
    ```xml
    <Resources>
      <Endpoints>
        <Endpoint Protocol="http" Name="ServiceEndpoint" Type="Input" />
      </Endpoints>
    </Resources>
    ```

    Usuwanie portu umożliwia sieci szkieletowej usług w celu wskazania portu dynamicznie z zakresu portów aplikacji, otworzyć za pomocą grupy zabezpieczeń sieci w szablonie usługi Resource Manager klastra, dzięki czemu przepływ ruchu do niego z interfejsu API zarządzania.
 
 6. Naciśnij klawisz F5 w programie Visual Studio, aby sprawdzić interfejsu API sieci web jest dostępna lokalnie. 

    Otwórz Eksploratora usługi sieć szkieletowa i przejdź do określonego wystąpienia usługi platformy ASP.NET Core szczegółów, aby wyświetlić adres podstawowy usługi nasłuchuje. Dodaj `/api/values` podstawy adresów, a następnie otwórz go w przeglądarce, która wywołuje metodę Get na ValuesController w szablonie interfejsu API sieci Web. Zwraca domyślny zapewnianej przez szablon, tablicy JSON, który zawiera dwa ciągi:

    ```json
    ["value1", "value2"]`
    ```

    To jest punkt końcowy, który ujawnia za pośrednictwem interfejsu API zarządzania na platformie Azure.

 7. Ponadto można wdrożyć aplikacji do klastra w systemie Azure. W programie Visual Studio, kliknij prawym przyciskiem myszy projekt aplikacji, a następnie wybierz **publikowania**. Podaj punkt końcowy klastra (na przykład `mycluster.southcentralus.cloudapp.azure.com:19000`) do wdrożenia aplikacji do klastra usługi sieć szkieletowa na platformie Azure.

Usługi bezstanowej platformy ASP.NET Core o nazwie `fabric:/ApiApplication/WebApiService` teraz powinna być uruchomiona w klastrze usługi sieć szkieletowa na platformie Azure.

### <a name="create-a-java-service-fabric-service"></a>Tworzenie usługi sieć szkieletowa usług Java
Dla tego samouczka wdrożyć serwer sieci web podstawowa, która zwraca komunikaty z powrotem do użytkownika. Echo serwer przykładowej aplikacji zawiera punkt końcowy HTTP dla usługi, które należy udostępnić za pośrednictwem usługi Azure API Management.

1. Klonowanie pobierania próbek uruchomiono Java.

   ```bash
   git clone https://github.com/Azure-Samples/service-fabric-java-getting-started.git
   cd service-fabric-java-getting-started
   ```

2. Edytuj *Services/EchoServer/EchoServer1.0/EchoServerApplication/EchoServerPkg/ServiceManifest.xml*. Dzięki usłudze nasłuchuje na porcie 8081, należy zaktualizować punktu końcowego.

   ```xml
   <Endpoint Name="WebEndpoint" Protocol="http" Port="8081" />
   ```

3. Zapisz *ServiceManifest.xml*, późniejszego kompilowania aplikacji EchoServer1.0.

   ```bash
   cd Services/EchoServer/EchoServer1.0/
   gradle
   ```

4. Wdrażanie aplikacji do klastra.

   ```bash
   cd Scripts
   sfctl cluster select --endpoint http://mycluster.southcentralus.cloudapp.azure.com:19080
   ./install.sh
   ```

   Usługi bezstanowej Java o nazwie `fabric:/EchoServerApplication/EchoServerService` teraz powinna być uruchomiona w klastrze usługi sieć szkieletowa na platformie Azure.

5. Otwórz przeglądarkę i typ w http://mycluster.southcentralus.cloudapp.azure.com:8081/getMessage powinna zostać wyświetlona "[wersja 1.0] Hello World!" wyświetlane.

## <a name="download-and-understand-the-resource-manager-template"></a>Pobierz i zrozumieć szablonu usługi Resource Manager
Pobierz i Zapisz następujące Menedżera zasobów plików szablonu i parametry:
 
- [APIM.JSON][apim-arm]
- [APIM.parameters.JSON][apim-parameters-arm]

W poniższych sekcjach opisano zasobów definiowanego przez *apim.json* szablonu. Aby uzyskać więcej informacji skorzystaj z łączy się z dokumentacją odwołanie szablonu w każdej sekcji. Można skonfigurować parametrów zdefiniowanych w *apim.parameters.json* pliku parametrów są ustawione w dalszej części tego artykułu.

### <a name="microsoftapimanagementservice"></a>Microsoft.ApiManagement/service
[Microsoft.ApiManagement/service](/azure/templates/microsoft.apimanagement/service) opisuje wystąpienie usługi Zarządzanie interfejsami API: nazwa, jednostki SKU lub warstwy lokalizacja grupy zasobów, informacje o wydawcy i sieci wirtualnej.

### <a name="microsoftapimanagementservicecertificates"></a>Microsoft.ApiManagement/service/certificates
[Microsoft.ApiManagement/service/certificates](/azure/templates/microsoft.apimanagement/service/certificates) konfiguruje zabezpieczeń interfejsu API zarządzania. Zarządzanie interfejsami API musi uwierzytelniać się z klastrem usługi sieć szkieletowa usług dla potrzeb odnajdywania usług przy użyciu certyfikatu klienta, który ma dostęp do klastra. Ten samouczek używa tego samego certyfikatu, określony wcześniej podczas tworzenia [klastra systemu Windows](service-fabric-tutorial-create-vnet-and-windows-cluster.md#createvaultandcert_anchor) lub [klaster systemu Linux](service-fabric-tutorial-create-vnet-and-linux-cluster.md#createvaultandcert_anchor), które domyślnie mogą służyć do klastra. 

W tym samouczku używa tego samego certyfikatu uwierzytelniania klienta i zabezpieczenia węzła do węzła klastra. Certyfikat klienta w osobnym może używać, jeśli jest skonfigurowane do korzystania z klastra usługi sieć szkieletowa usług. Podaj **nazwa**, **hasło**, i **danych** (ciąg algorytmem base-64) z pliku klucza prywatnego (pfx) certyfikatu klastra, który można określić podczas tworzenia użytkownika Klaster sieci szkieletowej usług.

### <a name="microsoftapimanagementservicebackends"></a>Microsoft.ApiManagement/service/backends
[Microsoft.ApiManagement/service/backends](/azure/templates/microsoft.apimanagement/service/backends) zawiera opis usługi wewnętrznej bazy danych, który ruch jest kierowany do. 

W przypadku zapleczy sieci szkieletowej usług klastra usługi sieć szkieletowa jest wewnętrznej bazy danych zamiast określonej usługi sieć szkieletowa usług. Dzięki temu jednych zasad można kierować do więcej niż jedną usługę w klastrze. **Adres url** pola w tym miejscu jest nazwą FQDN usługi usługi w klastrze, że wszystkie żądania są kierowane do domyślnie, jeśli nazwa usługi, nie jest określona w zasadzie wewnętrznej bazy danych. Może używać nazw fałszywych usług, takich jak "fabric: / fałszywych/usługi" Jeśli chcesz, aby usługę rezerwowy. **resourceId** określa punkt końcowy zarządzania klastrem.  **clientCertificateThumbprint** i **serverCertificateThumbprints** zidentyfikować certyfikaty używane do uwierzytelniania w klastrze.

### <a name="microsoftapimanagementserviceproducts"></a>Microsoft.ApiManagement/service/products
[Microsoft.ApiManagement/service/products](/azure/templates/microsoft.apimanagement/service/products) tworzy produktu. W systemie Azure API Management produkt zawiera jeden lub więcej interfejsów API, a także przydział użycia i warunki użytkowania. Po opublikowaniu produktu deweloperzy mogą subskrybować produktu i rozpocząć przy użyciu interfejsów API produktu. 

Wpisz opisową nazwę **displayName** i **opis** dla produktu. W tym samouczku wymagana jest subskrypcja, ale subskrypcji zatwierdzenia przez administratora nie jest.  Ten produkt **stanu** "opublikowaniu" która jest widoczna dla subskrybentów. 

### <a name="microsoftapimanagementserviceapis"></a>Microsoft.ApiManagement/service/apis
[Microsoft.ApiManagement/service/apis](/azure/templates/microsoft.apimanagement/service/apis) tworzy interfejs API. Interfejs API w usłudze API Management reprezentuje zestaw operacji, które może być wywoływany przez aplikacje klienckie. Po dodaniu działania, interfejsu API jest dodawany do produktu i mogą być publikowane. Po opublikowaniu interfejsu API można zasubskrybować i używane przez programistów.

- **Nazwa wyświetlana** może być dowolną nazwą dla interfejsu API. W tym samouczku Użyj "Aplikacja sieci szkieletowej usług".
- **Nazwa** to unikatowa i opisowa nazwa interfejsu API, takich jak "Usługa sieci szkieletowej — aplikacji". Jest on wyświetlany w portalach deweloperów i wydawcy. 
- **serviceUrl** odwołuje się do usługi HTTP implementacja interfejsu API. Zarządzanie interfejsami API przekazuje żądania do tego adresu. Ta wartość adresu URL nie jest używana do zapleczy sieci szkieletowej usług. Tutaj możesz umieścić żadnej wartości. W tym samouczku, na przykład "http://servicefabric". 
- **ścieżka** jest dołączany do podstawowego adresu URL dla interfejsu API usługi zarządzania. Element podstawowy adres URL jest wspólne dla wszystkich interfejsów API hostowanych przez wystąpienie usługi Zarządzanie interfejsami API. Zarządzanie interfejsami API odróżnia interfejsy API według ich sufiks i w związku z tym sufiks muszą być unikatowe dla każdego interfejsu API dla danego wydawcy. 
- **protokoły** ustalić, które protokoły umożliwia dostęp do interfejsu API. W tym samouczku listy **http** i **https**.
- **ścieżka** jest sufiksem dla interfejsu API. W tym samouczku Użyj "myapp".

### <a name="microsoftapimanagementserviceapisoperations"></a>Microsoft.ApiManagement/service/apis/operations
[Microsoft.ApiManagement/service/apis/operations](/azure/templates/microsoft.apimanagement/service/apis/operations) przed interfejsu API w usłudze API Management może być używany, operacji musi zostać dodany do interfejsu API.  Za pomocą operacji klientów zewnętrznych komunikować się z usługą bezstanowych platformy ASP.NET Core uruchamianych w klastrze usługi sieć szkieletowa usług.

Aby dodać frontonu operacji interfejsu API, wprowadź wartości:

- **Nazwa wyświetlana** i **opis** opisano wykonać operację. W tym samouczku Użyj "Wartości".
- **Metoda** określa zlecenie HTTP.  W tym samouczku, określ **UZYSKAĆ**.
- **urlTemplate** jest dołączona do podstawowego adresu URL interfejsu API i identyfikuje jednej operacji HTTP.  W tym samouczku, użyj `/api/values` Jeśli został dodany do usługi zaplecza .NET lub `getMessage` Jeśli został dodany do usługi zaplecza Java.  Domyślnie ścieżkę adresu URL określone w tym miejscu są wysyłane ścieżkę adresu URL do zaplecza usługi sieć szkieletowa usług. Jeśli używasz tego samego adresu URL ścieżkę tutaj używanego z usługą, takie jak "/ api/values", operacja działa bez dalszej modyfikacji. Może również określić ścieżki adresu URL w tym miejscu, który różni się od ścieżki adresu URL z wewnętrzną bazą danych usługi Service Fabric, w którym to przypadku należy również określić Edycja ścieżki w zasadach operację później.

### <a name="microsoftapimanagementserviceapispolicies"></a>Microsoft.ApiManagement/service/apis/policies
[Microsoft.ApiManagement/service/apis/policies](/azure/templates/microsoft.apimanagement/service/apis/policies) tworzy zasady wewnętrznej bazy danych, które wszystko, co wiąże ze sobą. Jest to, którym można skonfigurować usługi sieci szkieletowej usług zaplecza, do którego są przesyłane żądania. Te zasady można stosować do żadnej operacji interfejsu API.  Aby uzyskać więcej informacji, zobacz [Omówienie zasad](/azure/api-management/api-management-howto-policies). 

[Wewnętrznej bazy danych konfiguracji dla usługi Service Fabric](/azure/api-management/api-management-transformation-policies#SetBackendService) zawiera następujące żądania routingu kontrolki: 
 - Usługa Wybór wystąpienia, określając nazwę wystąpienia usługi Service Fabric, albo zapisane na stałe (na przykład `"fabric:/myapp/myservice"`) lub generowane na podstawie żądania HTTP (na przykład `"fabric:/myapp/users/" + context.Request.MatchedParameters["name"]`).
 - Rozdzielczość partycji przez generowanie klucza partycji przy użyciu dowolnego schemat partycjonowania sieci szkieletowej usług.
 - Wybór repliki dla stanowych usług.
 - Rozdzielczość ponownych prób warunki, które umożliwiają określenie warunków ponowne rozpoznanie lokalizację usługi i wysłać żądanie.

**policyContent** jest Json wpisywany zawartość XML zasady.  W tym samouczku Utwórz zasady żądań trasę bezpośrednio do usługi bezstanowej .NET lub Java wdrożonej wcześniej wewnętrznej bazy danych. Dodaj `set-backend-service` zasad w obszarze zasady ruchu przychodzącego.  Zamień "Nazwa usługi" `fabric:/ApiApplication/WebApiService` Jeśli wdrożono usługę zaplecza .NET lub `fabric:/EchoServerApplication/EchoServerService` Jeśli wdrożono usługę Java.
    
```xml
<policies>
  <inbound>
    <base/>
    <set-backend-service 
        backend-id="servicefabric"
        sf-service-instance-name="service-name"
        sf-resolve-condition="@((int)context.Response.StatusCode != 200)" />
  </inbound>
  <backend>
    <base/>
  </backend>
  <outbound>
    <base/>
  </outbound>
</policies>
```

Pełny zestaw atrybutów wewnętrznych zasad sieci szkieletowej usług można znaleźć w temacie [dokumentacji zaplecza interfejsu API zarządzania](https://docs.microsoft.com/azure/api-management/api-management-transformation-policies#SetBackendService)

## <a name="set-parameters-and-deploy-api-management"></a>Ustaw parametry i wdrażać zarządzanie interfejsami API
Wypełnij następujące parametry pusta w *apim.parameters.json* dla danego wdrożenia. 

|Parametr|Wartość|
|---|---|
|apimInstanceName|apim rz|
|apimPublisherEmail|myemail@contosos.com|
|apimSku|Developer|
|serviceFabricCertificateName|sfclustertutorialgroup320171031144217|
|CertificatePassword|q6D7nN %6ck@6| 
|serviceFabricCertificateThumbprint|C4C1E541AD512B8065280292A8BA6079C3F26F10 |
|serviceFabricCertificate|&lt;ciąg kodowany w formacie Base-64&gt;|
|url_path|/ api/wartości|
|clusterHttpManagementEndpoint|https://mysfcluster.southcentralus.cloudapp.Azure.com:19080|
|inbound_policy|&lt;Ciąg XML&gt;|

*certificatePassword* i *serviceFabricCertificateThumbprint* musi być zgodna z certyfikatu klastra używana do konfigurowania klastra.  

*serviceFabricCertificate* jest certyfikat jako ciąg zakodowany base-64, który można wygenerować za pomocą następującego skryptu:

```powershell
$bytes = [System.IO.File]::ReadAllBytes("C:\mycertificates\sfclustertutorialgroup220171109113527.pfx");
$b64 = [System.Convert]::ToBase64String($bytes);
[System.Io.File]::WriteAllText("C:\mycertificates\sfclustertutorialgroup220171109113527.txt", $b64);
```

W *inbound_policy*, zastąp "Nazwa usługi" `fabric:/ApiApplication/WebApiService` Jeśli wdrożono usługę zaplecza .NET lub `fabric:/EchoServerApplication/EchoServerService` Jeśli wdrożono usługę Java.

```xml
<policies>
  <inbound>
    <base/>
    <set-backend-service 
        backend-id="servicefabric"
        sf-service-instance-name="service-name"
        sf-resolve-condition="@((int)context.Response.StatusCode != 200)" />
  </inbound>
  <backend>
    <base/>
  </backend>
  <outbound>
    <base/>
  </outbound>
</policies>
```

Użyj następującego skryptu wdrażania Menedżera zasobów plików szablonu i parametr dla interfejsu API zarządzania:

```powershell
$ResourceGroupName = "sfclustertutorialgroup"
New-AzureRmResourceGroupDeployment -ResourceGroupName $ResourceGroupName -TemplateFile .\apim.json -TemplateParameterFile .\apim.parameters.json -Verbose
```

```azurecli
ResourceGroupName="sfclustertutorialgroup"
az group deployment create --name ApiMgmtDeployment --resource-group $ResourceGroupName --template-file apim.json --parameters @apim.parameters.json 
```

## <a name="test-it"></a>należy przeprowadzić test

Teraz możesz wysyłania żądania do usługi zaplecza w sieci szkieletowej usług za pośrednictwem interfejsu API zarządzania bezpośrednio z [portalu Azure](https://portal.azure.com).

 1. W usłudze API Management wybierz **interfejsu API**.
 2. W **usługi App Service Fabric** interfejsu API utworzone w poprzednich krokach, wybierz opcję **testu** kartę, a następnie **wartości** operacji.
 3. Kliknij przycisk **wysyłania** przycisk, aby wysłać żądania testu do usługi zaplecza.  Powinna zostać wyświetlona odpowiedź HTTP podobny do:

    ```http
    HTTP/1.1 200 OK

    Transfer-Encoding: chunked

    Content-Type: application/json; charset=utf-8

    Vary: Origin

    Access-Control-Allow-Origin: https://apimanagement.hosting.portal.azure.net

    Access-Control-Allow-Credentials: true

    Access-Control-Expose-Headers: Transfer-Encoding,Date,Server,Vary,Ocp-Apim-Trace-Location

    Ocp-Apim-Trace-Location: https://apimgmtstuvyx3wa3oqhdbwy.blob.core.windows.net/apiinspectorcontainer/RaVVuJBQ9yxtdyH55BAsjQ2-1?sv=2015-07-08&sr=b&sig=Ab6dPyLpTGAU6TdmlEVu32DMfdCXTiKAASUlwSq3jcY%3D&se=2017-09-15T05%3A49%3A53Z&sp=r&traceId=ed9f1f4332e34883a774c34aa899b832

    Date: Thu, 14 Sep 2017 05:49:56 GMT


    [
    "value1",
    "value2"
    ]
    ```

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Klaster składa się z innych zasobów platformy Azure poza samym zasobem klastra. Najprostszym sposobem na usunięcie klastra i wszystkich wykorzystywanych przez niego zasobów jest usunięcie grupy zasobów.

Logowanie do platformy Azure i wybierz identyfikator subskrypcji, z którą chcesz usunąć klaster.  Identyfikator subskrypcji można znaleźć po zalogowaniu się do [portalu Azure](http://portal.azure.com). Usuń grupę zasobów i zasobów klastra przy użyciu [polecenia cmdlet Remove-AzureRMResourceGroup](/en-us/powershell/module/azurerm.resources/remove-azurermresourcegroup).

```powershell
$ResourceGroupName = "sfclustertutorialgroup"
Remove-AzureRmResourceGroup -Name $ResourceGroupName -Force
```

```azurecli
ResourceGroupName="sfclustertutorialgroup"
az group delete --name $ResourceGroupName
```

## <a name="next-steps"></a>Kolejne kroki
W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Wdrażanie usługi API Management
> * Skonfiguruj zarządzanie interfejsami API
> * Tworzenie operacji interfejsu API
> * Skonfiguruj zasady wewnętrznej bazy danych
> * Dodawanie interfejsu API do produktu

[azure-powershell]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/

[apim-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/apim.json
[apim-parameters-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/apim.parameters.json

[network-arm]: https://github.com/Azure-Samples/service-fabric-api-management/blob/master/network.json
[network-parameters-arm]: https://github.com/Azure-Samples/service-fabric-api-management/blob/master/network.parameters.json

[cluster-arm]: https://github.com/Azure-Samples/service-fabric-api-management/blob/master/cluster.json
[cluster-parameters-arm]: https://github.com/Azure-Samples/service-fabric-api-management/blob/master/cluster.parameters.json

<!-- pics -->
[sf-apim-topology-overview]: ./media/service-fabric-tutorial-deploy-api-management/sf-apim-topology-overview.png
