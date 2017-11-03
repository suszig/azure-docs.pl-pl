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
ms.date: 09/13/2017
ms.author: ryanwi
ms.openlocfilehash: d98d2823c19f24a2d9040f7959bd5189bd6bcc16
ms.sourcegitcommit: ccb84f6b1d445d88b9870041c84cebd64fbdbc72
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/14/2017
---
# <a name="deploy-api-management-with-service-fabric"></a>Wdrażanie interfejsu API zarządzania za pomocą sieci szkieletowej usług
W tym samouczku jest częścią dwóch serii. W tym samouczku przedstawiono sposób konfigurowania [Azure API Management](../api-management/api-management-key-concepts.md) z sieci szkieletowej usług można kierować ruchem do usługi zaplecza w sieci szkieletowej usług.  Po zakończeniu ma wdrożyć zarządzanie interfejsami API w sieci Wirtualnej, skonfigurowane do wysyłania ruchu do zaplecza usługi bezstanowej operacji interfejsu API. Aby dowiedzieć się więcej o scenariuszach usługi Azure API Management z sieci szkieletowej usług, zobacz [omówienie](service-fabric-api-management-overview.md) artykułu.

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
> * Wdrażanie interfejsu API zarządzania za pomocą sieci szkieletowej usług

## <a name="prerequisites"></a>Wymagania wstępne
Przed rozpoczęciem tego samouczka:
- Jeśli nie masz subskrypcji platformy Azure, Utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- Zainstaluj [programu Azure Powershell w wersji modułu 4.1 lub wyższej](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) lub [Azure CLI 2.0](/cli/azure/install-azure-cli).
- Tworzenie bezpiecznej [klastra systemu Windows](service-fabric-tutorial-create-vnet-and-windows-cluster.md) lub [klaster systemu Linux](service-fabric-tutorial-create-vnet-and-linux-cluster.md) na platformie Azure
- W przypadku wdrażania klastra systemu Windows, konfigurowanie środowiska projektowego systemu Windows. Zainstaluj [programu Visual Studio 2017](http://www.visualstudio.com) i **Azure programowanie**, **ASP.NET i sieć web development**, i **aplikacji dla wielu platform .NET Core**obciążeń.  Następnie skonfiguruj [środowiska programowania .NET](service-fabric-get-started.md).
- Jeśli w przypadku wdrażania klastra Linux należy skonfigurować środowisko projektowe Java na [Linux](service-fabric-get-started-linux.md) lub [MacOS](service-fabric-get-started-mac.md).  Zainstaluj [usługi sieci szkieletowej interfejsu wiersza polecenia](service-fabric-cli.md). 

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

## <a name="deploy-api-management"></a>Wdrażanie usługi API Management
Aplikacje w chmurze muszą zwykle frontonu bramy do zapewnienia pojedynczy punkt wejściowych użytkowników, urządzeń lub innych aplikacji. W sieci szkieletowej usług Brama może być dowolnej usługi bezstanowej, takie jak aplikacja platformy ASP.NET Core lub innej usługi, przeznaczony dla ruch przychodzący, takie jak usługi Event Hubs, Centrum IoT lub usługi Azure API Management. W tym samouczku jest wprowadzenie do korzystania z usługi Azure API Management jako bramy dla poszczególnych aplikacji sieci szkieletowej usług. Zarządzanie interfejsami API integruje się bezpośrednio z usługi Service Fabric, co umożliwia publikowanie interfejsów API za pomocą bogaty zestaw reguł routingu do usług sieci szkieletowej usług zaplecza. 

Teraz, gdy masz bezpiecznej [klastra systemu Windows](service-fabric-tutorial-create-vnet-and-windows-cluster.md) lub [klaster systemu Linux](service-fabric-tutorial-create-vnet-and-linux-cluster.md) na platformie Azure, wdrożenia interfejs API zarządzania siecią wirtualną (VNET) w podsieci i NSG przeznaczone dla interfejsu API zarządzania. W tym samouczku jest wstępnie skonfigurowana do używania nazwy sieci Wirtualnej, podsieci i NSG, skonfigurowanym w poprzednim szablonu usługi Resource Manager Management API [samouczek klastra systemu Windows](service-fabric-tutorial-create-vnet-and-windows-cluster.md) lub [samouczka klaster systemu Linux](service-fabric-tutorial-create-vnet-and-linux-cluster.md). 

Pobierz następującego pliku szablonu i parametry Menedżera zasobów:
 
- [APIM.JSON][apim-arm]
- [APIM.parameters.JSON][apim-parameters-arm]

Wypełnij puste parametry w `apim.parameters.json` dla danego wdrożenia.

Użyj następującego skryptu wdrażania Menedżera zasobów plików szablonu i parametr dla interfejsu API zarządzania:

```powershell
$ResourceGroupName = "tutorialgroup"
New-AzureRmResourceGroupDeployment -ResourceGroupName $ResourceGroupName -TemplateFile .\apim.json -TemplateParameterFile .\apim.parameters.json -Verbose
```

```azurecli
ResourceGroupName="tutorialgroup"
az group deployment create --name ApiMgmtDeployment --resource-group $ResourceGroupName --template-file apim.json --parameters @apim.parameters.json 
```

## <a name="configure-api-management"></a>Skonfiguruj zarządzanie interfejsami API

Po są wdrażane w klastrze API Management i sieci szkieletowej usług, można skonfigurować ustawień zabezpieczeń i sieci szkieletowej usług w wewnętrznej bazie danych w usłudze API Management. Dzięki temu można utworzyć zasadę usługi wewnętrznej bazy danych, która wysyła ruchu do klastra usługi sieć szkieletowa usług.

### <a name="configure-api-management-security"></a>Konfigurowanie zabezpieczeń Management API

Aby skonfigurować sieć szkieletowa usług wewnętrznej bazy danych, należy najpierw skonfigurować ustawienia zabezpieczeń interfejsu API zarządzania. Aby skonfigurować ustawienia zabezpieczeń, przejdź do usługi Zarządzanie interfejsami API w portalu Azure.

#### <a name="enable-the-api-management-rest-api"></a>Włącz zarządzanie interfejsami API interfejsu API REST

Interfejs API REST zarządzania interfejsu API jest obecnie jedynym sposobem na skonfigurowanie usługi wewnętrznej bazy danych. Pierwszym krokiem jest Włącz interfejs API REST zarządzania interfejsu API i zabezpieczyć.

 1. W usłudze API Management wybierz **interfejs API zarządzania** w obszarze **zabezpieczeń**.
 2. Sprawdź **Włącz interfejs API zarządzania interfejsu API REST** wyboru.
 3. Uwaga **adres URL interfejsu API zarządzania**, której używamy później skonfigurować wewnętrznej bazy danych usługi Service Fabric.
 4. Generuj **Token dostępu** wybierając datę wygaśnięcia oraz klucz, następnie kliknij przycisk **Generuj** przycisku w kierunku dolnej części strony.
 5. Kopiuj **token dostępu** i zapisz go.  Używamy token dostępu w kolejnych krokach. Należy zauważyć, że ta różni się od klucza podstawowego i pomocniczego klucza.

#### <a name="upload-a-service-fabric-client-certificate"></a>Przekaż certyfikat klienta sieci szkieletowej usług

Zarządzanie interfejsami API musi uwierzytelniać się z klastrem usługi sieć szkieletowa usług dla potrzeb odnajdywania usług przy użyciu certyfikatu klienta, który ma dostęp do klastra. Dla uproszczenia, w tym samouczku używa tego samego certyfikatu, określony wcześniej podczas tworzenia [klastra systemu Windows](service-fabric-tutorial-create-vnet-and-windows-cluster.md#createvaultandcert_anchor) lub [klaster systemu Linux](service-fabric-tutorial-create-vnet-and-linux-cluster.md#createvaultandcert_anchor), które domyślnie mogą służyć do klastra.

 1. W usłudze API Management wybierz **certyfikaty klienta** w obszarze **zabezpieczeń**.
 2. Kliknij przycisk **+ Dodaj** przycisku.
 2. Wybierz plik klucza prywatnego (pfx) certyfikatu klastra, który określone podczas tworzenia klastra usługi sieć szkieletowa, nadaj mu nazwę i podaj hasło klucza prywatnego.

> [!NOTE]
> W tym samouczku używa tego samego certyfikatu uwierzytelniania klienta i zabezpieczenia węzła do węzła klastra. Certyfikat klienta w osobnym może używać, jeśli jest skonfigurowane do korzystania z klastra usługi sieć szkieletowa usług.

### <a name="configure-the-backend"></a>Skonfiguruj wewnętrznej bazy danych

Teraz, gdy zabezpieczeń interfejsu API zarządzania jest skonfigurowany, można skonfigurować sieci szkieletowej usług wewnętrznej bazy danych. W przypadku zapleczy sieci szkieletowej usług klastra usługi sieć szkieletowa jest wewnętrznej bazy danych, a nie dla określonej usługi sieć szkieletowa usług. Dzięki temu jednych zasad można kierować do więcej niż jedną usługę w klastrze.

Ten krok wymaga wcześniej wygenerowania tokenu dostępu i odcisk palca dla certyfikatu klastra wcześniej przekazany do interfejsu API zarządzania.

> [!NOTE]
> Jeśli używasz certyfikatu klienta w osobnym w poprzednim kroku dla interfejsu API zarządzania należy odcisk palca certyfikatu klienta oprócz odcisk palca certyfikatu klastra w tym kroku.

Wyślij następujący HTTP PUT żądania na adres URL interfejsu API Management API zanotowaną wcześniej podczas włączania interfejsu API REST zarządzania interfejsu API do skonfigurowania usługi wewnętrznej bazy danych usługi Service Fabric. Powinny pojawić się `HTTP 201 Created` odpowiedzi, gdy polecenie zakończy się pomyślnie. Aby uzyskać więcej informacji na każde pole, zobacz Zarządzanie interfejsami API [dokumentacji zaplecza interfejsu API](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-contract-reference#a-namebackenda-backend).

Polecenia HTTP i adres URL:
```http
PUT https://your-apim.management.azure-api.net/backends/servicefabric?api-version=2016-10-10
```

Nagłówki żądania:
```http
Authorization: SharedAccessSignature <your access token>
Content-Type: application/json
```

Treść żądania:
```http
{
    "description": "<description>",
    "url": "<fallback service name>",
    "protocol": "http",
    "resourceId": "<cluster HTTP management endpoint>",
    "properties": {
        "serviceFabricCluster": {
            "managementEndpoints": [ "<cluster HTTP management endpoint>" ],
            "clientCertificateThumbprint": "<client cert thumbprint>",
            "serverCertificateThumbprints": [ "<cluster cert thumbprint>" ],
            "maxPartitionResolutionRetries" : 5
        }
    }
}
```

**Adres url** tutaj parametr ma nazwę FQDN usługi usługi w klastrze, że wszystkie żądania są kierowane do domyślnie, jeśli nazwa usługi, nie jest określona w zasadzie wewnętrznej bazy danych. Może używać nazw fałszywych usług, takich jak "fabric: / fałszywych/usługi" Jeśli chcesz, aby usługę rezerwowy.

Odwoływać się do usługi API Management [interfejs API zaplecza dokumentacji](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-contract-reference#a-namebackenda-backend) uzyskać więcej informacji dotyczących każdego pola.

```python
#import requests library for making REST calls
import requests
import json

#specify url
url = 'https://your-apim.management.azure-api.net/backends/servicefabric?api-version=2016-10-10'

token = "SharedAccessSignature integration&201710140514&Lqnbei7n2Sot6doiNtxMFPUi/m9LsNa+1ZK/PdxqFl49JFWjXh1wW5Gd99r/tIOeHp6dU8UV5iZUdOPfcrm5hg=="

payload = {
    "description": "My Service Fabric backend",
    "url": "fabric:/ApiApplication/ApiWebService",
    "protocol": "http",
    "resourceId": "https://tutorialcluster.eastus.cloudapp.azure.com:19080",
    "properties": {
        "serviceFabricCluster": {
            "managementEndpoints": [ "https://tutorialcluster.eastus.cloudapp.azure.com:19080" ],
            "clientCertificateThumbprint": "97EDD7E4979FB072AF3E480A5E5EE34B1B89DD80",
            "serverCertificateThumbprints": [ "97EDD7E4979FB072AF3E480A5E5EE34B1B89DD80" ],
            "maxPartitionResolutionRetries" : 5
        }
    }
}

headers = {'Authorization': token, "Content-Type": "application/json"}

#Call REST API
response = requests.put(url, data=json.dumps(payload), headers=headers)

#Print Response
print(response.status_code)
print(response.text)
```

## <a name="deploy-a-service-fabric-back-end-service"></a>Wdrażanie usługi zaplecza sieci szkieletowej usług

Teraz, gdy masz usługi sieć szkieletowa, skonfigurowany jako zaplecza interfejsu API zarządzania zasady wewnętrznej bazy danych można tworzyć dla interfejsów API, które przesyłają dane do usług sieci szkieletowej usług. Ale najpierw należy w usłudze działającej w sieci szkieletowej usług w celu umożliwienia akceptowania żądań.  Jeśli wcześniej została utworzona [klastra systemu Windows](service-fabric-tutorial-create-vnet-and-windows-cluster.md), wdrożenie usługi sieć szkieletowa usług .NET.  Jeśli wcześniej została utworzona [klaster systemu Linux](service-fabric-tutorial-create-vnet-and-linux-cluster.md), wdrożenie usługi sieć szkieletowa usług Java.

### <a name="deploy-a-net-service-fabric-service"></a>Wdrażanie usługi sieci szkieletowej usług .NET

W tym samouczku utworzymy podstawowe bezstanowej platformy ASP.NET Core niezawodnej usługi przy użyciu domyślnego szablonu projektu interfejsu API sieci Web. Spowoduje to utworzenie punktu końcowego HTTP dla usługi, które należy udostępnić za pośrednictwem usługi Azure API Management:

```
/api/values
```

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

    Dzięki temu usługi sieci szkieletowej określić port dynamicznie z zakresu portów aplikacji, którego możemy otworzyć za pomocą grupy zabezpieczeń sieci w szablonie usługi Resource Manager klastra, dzięki czemu przepływ ruchu do niego z interfejsu API zarządzania.
 
 6. Naciśnij klawisz F5 w programie Visual Studio, aby sprawdzić interfejsu API sieci web jest dostępna lokalnie. 

    Otwórz Eksploratora usługi sieć szkieletowa i przejdź do określonego wystąpienia usługi platformy ASP.NET Core szczegółów, aby wyświetlić adres podstawowy usługi nasłuchuje. Dodaj `/api/values` podstawy adresów, a następnie otwórz go w przeglądarce. To wywołuje metodę Get na ValuesController w szablonie interfejsu API sieci Web. Zwraca domyślny zapewnianej przez szablon, tablicy JSON, który zawiera dwa ciągi:

    ```json
    ["value1", "value2"]`
    ```

    To jest punkt końcowy, który ujawnia za pośrednictwem interfejsu API zarządzania na platformie Azure.

 7. Ponadto można wdrożyć aplikacji do klastra w systemie Azure. [Za pomocą programu Visual Studio](service-fabric-publish-app-remote-cluster.md#to-publish-an-application-using-the-publish-service-fabric-application-dialog-box), kliknij prawym przyciskiem myszy projekt aplikacji i wybierz **publikowania**. Podaj punkt końcowy klastra (na przykład `mycluster.southcentralus.cloudapp.azure.com:19000`) do wdrożenia aplikacji do klastra usługi sieć szkieletowa na platformie Azure.

Usługi bezstanowej platformy ASP.NET Core o nazwie `fabric:/ApiApplication/WebApiService` teraz powinna być uruchomiona w klastrze usługi sieć szkieletowa na platformie Azure.

### <a name="create-a-java-service-fabric-service"></a>Tworzenie usługi sieć szkieletowa usług Java
W tym samouczku możemy wdrożyć serwer podstawowy sieci web, który informujące użytkownika wiadomości. Echo serwer przykładowej aplikacji zawiera punkt końcowy HTTP dla usługi, które należy udostępnić za pośrednictwem usługi Azure API Management.

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


## <a name="create-an-api-operation"></a>Tworzenie operacji interfejsu API

Teraz już wszystko gotowe do tworzenia operacji w usłudze API Management, używanego przez klientów zewnętrznych do komunikowania się z usługi bezstanowej platformy ASP.NET Core uruchamianych w klastrze usługi sieć szkieletowa usług.

1. Zaloguj się do portalu Azure i przejdź do wdrożenia usługi Zarządzanie interfejsami API.
2. W bloku usługi API Management, wybierz **interfejsów API**
3. Dodaj nowy interfejs API, klikając **i interfejs API**, następnie **puste API** pole i wypełniania okno dialogowe:

    - **Adres URL usługi sieci Web**: dla sieci szkieletowej usług zapleczy, ta wartość adresu URL nie jest używany. Tutaj możesz umieścić żadnej wartości. W tym samouczku, użyj: "http://servicefabric".
    - **Nazwa wyświetlana**: Podaj dowolną nazwę interfejsu API. W tym samouczku Użyj "Aplikacja sieci szkieletowej usług".
    - **Nazwa**: wprowadź "Usługa sieci szkieletowej — aplikacji".
    - **Schemat adresu URL**: wybierz opcję **HTTP**, **HTTPS**, lub **zarówno**. W tym samouczku, użyj **zarówno**.
    - **Sufiks adresu URL interfejsu API**: Podaj sufiks na interfejsie API. W tym samouczku Użyj "myapp".
 
4. Wybierz **usługi App Service Fabric** z listy interfejsów API i kliknij przycisk **+ Dodaj operację** powoduje dodanie frontonu operacji interfejsu API. Wprowadź wartości:
    
    - **Adres URL**: Wybierz **UZYSKAĆ** i podaj ścieżkę adresu URL dla interfejsu API. W tym samouczku Użyj "/ api/values".  Domyślnie ścieżkę adresu URL określone w tym miejscu są wysyłane ścieżkę adresu URL do zaplecza usługi sieć szkieletowa usług. Jeśli używasz tego samego adresu URL ścieżkę tutaj korzystającą z usługą, w tym przypadku "/ api/values", a następnie wykonać operację działa bez dalszej modyfikacji. Może również określić ścieżki adresu URL w tym miejscu, który różni się od ścieżki adresu URL z wewnętrzną bazą danych usługi Service Fabric, w którym to przypadku należy również określić Edycja ścieżki w zasadach operację później.
    - **Nazwa wyświetlana**: Podaj dowolną nazwę dla interfejsu API. W tym samouczku Użyj "Wartości".

5. Kliknij pozycję **Zapisz**.

## <a name="configure-a-backend-policy"></a>Skonfiguruj zasady wewnętrznej bazy danych

Zasady zaplecza wiąże co wszystko. Jest to, którym można skonfigurować usługi sieci szkieletowej usług zaplecza, do którego są przesyłane żądania. Te zasady można stosować do żadnej operacji interfejsu API. [Wewnętrznej bazy danych konfiguracji dla usługi Service Fabric](https://docs.microsoft.com/azure/api-management/api-management-transformation-policies#SetBackendService) zawiera następujące żądania routingu kontrolki: 
 - Usługa Wybór wystąpienia, określając nazwę wystąpienia usługi Service Fabric, albo zapisane na stałe (na przykład `"fabric:/myapp/myservice"`) lub generowane na podstawie żądania HTTP (na przykład `"fabric:/myapp/users/" + context.Request.MatchedParameters["name"]`).
 - Rozdzielczość partycji przez generowanie klucza partycji przy użyciu dowolnego schemat partycjonowania sieci szkieletowej usług.
 - Wybór repliki dla stanowych usług.
 - Rozdzielczość ponownych prób warunki, które umożliwiają określenie warunków ponowne rozpoznanie lokalizację usługi i wysłać żądanie.

W tym samouczku Utwórz zasadę kieruje żądania bezpośrednio do usługi bezstanowej platformy ASP.NET Core wdrożonej wcześniej wewnętrznej bazy danych:

 1. Wybierz i edytować **przychodzących zasad** dla operacji wartości, klikając ikonę edycji, a następnie zaznaczając **widoku kodu**.
 2. W edytorze kodu zasad, Dodaj `set-backend-service` zasad w obszarze zasady ruchu przychodzącego, jak pokazano poniżej i kliknij przycisk **zapisać** przycisk:
    
    ```xml
    <policies>
      <inbound>
        <base/>
        <set-backend-service 
           backend-id="servicefabric"
           sf-service-instance-name="fabric:/ApiApplication/WebApiService"
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

### <a name="add-the-api-to-a-product"></a>Dodawanie interfejsu API do produktu 

Przed wywołaniem interfejsu API, można dodać go do produktu, w którym można przyznać dostęp użytkownikom. 

 1. W usłudze API Management wybierz **produkty**.
 2. Domyślnie produkty przeznaczone do interfejsu API zarządzania dwóch dostawców: Starter i bez ograniczeń. Wybierz nieograniczone produktu.
 3. Wybierz **+ Dodaj interfejsów API**.
 4. Wybierz `Service Fabric App` interfejsu API utworzone w poprzednich krokach i kliknij **wybierz** przycisku.

### <a name="test-it"></a>należy przeprowadzić test

Teraz możesz spróbować wysyłania żądania do usługi zaplecza w sieci szkieletowej usług za pośrednictwem interfejsu API zarządzania bezpośrednio z portalu Azure.

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
$ResourceGroupName = "tutorialgroup"
Remove-AzureRmResourceGroup -Name $ResourceGroupName -Force
```

```azurecli
ResourceGroupName="tutorialgroup"
az group delete --name $ResourceGroupName
```

## <a name="next-steps"></a>Następne kroki
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