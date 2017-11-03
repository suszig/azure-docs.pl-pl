---
title: "Sieć szkieletowa usług Azure z interfejsu API zarządzania szybki start | Dokumentacja firmy Microsoft"
description: "W tym przewodniku przedstawiono sposób szybkie wprowadzenie do usługi Azure API Management i sieci szkieletowej usług."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: 96176149-69bb-4b06-a72e-ebbfea84454b
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/01/2017
ms.author: vturecek
ms.openlocfilehash: 2969834713fc7c2f1a2e281a6c988158d803dc45
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="service-fabric-with-azure-api-management-quick-start"></a>Sieć szkieletowa usług z usługi Azure API Management Szybki Start

W tym przewodniku przedstawiono sposób instalowania usługi Azure API Management z sieci szkieletowej usług i konfigurowania pierwszej operacji interfejsu API do wysyłania ruchu do usług zaplecza w sieci szkieletowej usług. Aby dowiedzieć się więcej o scenariuszach usługi Azure API Management z sieci szkieletowej usług, zobacz [omówienie](service-fabric-api-management-overview.md) artykułu. 

## <a name="deploy-api-management-and-service-fabric-to-azure"></a>Wdrażanie na platformie Azure API Management i sieci szkieletowej usług

Pierwszym krokiem jest wdrażania interfejsu API zarządzania i klaster sieci szkieletowej usług Azure w sieci wirtualnej udostępnionego. Dzięki temu zarządzanie interfejsami API do bezpośredniego komunikowania się z sieci szkieletowej usług, który może wykonywać odnajdywania usługi, rozdzielczość partycji usługi i przekazywać ruch bezpośrednio do dowolnej usługi wewnętrznej bazy danych w sieci szkieletowej usług.

### <a name="topology"></a>Topologia

Ten przewodnik wdrażania następujących topologii na platformie Azure, w którym interfejs API zarządzania i sieci szkieletowej usług znajdują się w podsieci w tej samej sieci wirtualnej:

 ![Podpis obrazu][sf-apim-topology-overview]

Aby szybko rozpocząć pracę, szablony usługi Resource Manager są dostępne dla każdego kroku wdrożenia:

 - Topologia sieci:
    - [Network.JSON][network-arm]
    - [Network.parameters.JSON][network-parameters-arm]
 - Klaster sieci szkieletowej usług:
    - [cluster.JSON][cluster-arm]
    - [cluster.parameters.JSON][cluster-parameters-arm]
 - Zarządzanie interfejsami API:
    - [APIM.JSON][apim-arm]
    - [APIM.parameters.JSON][apim-parameters-arm]

### <a name="sign-in-to-azure-and-select-your-subscription"></a>Logowanie do platformy Azure i wyboru subskrypcji

Ten przewodnik po używa [programu Azure PowerShell][azure-powershell]. Po ponownym uruchomieniu nowej sesji programu PowerShell, zaloguj się do konta platformy Azure i wybierz subskrypcję, przed wykonaniem polecenia platformy Azure.
 
Zaloguj się do konta platformy Azure:

```powershell
Login-AzureRmAccount
```

Wybierz subskrypcję:

```powershell
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>
```

### <a name="create-a-resource-group"></a>Tworzenie grupy zasobów

Utwórz nową grupę zasobów dla danego wdrożenia. Nadaj nazwę i lokalizację.

```powershell
New-AzureRmResourceGroup -Name <my-resource-group> -Location westus
```

### <a name="deploy-the-network-topology"></a>Wdrażanie topologii sieci

Pierwszym krokiem jest ustanowienie topologii sieci, do którego będzie można wdrożyć API Management i klaster sieci szkieletowej usług. [Network.json] [ network-arm] szablonu usługi Resource Manager skonfigurowano do tworzenia sieci wirtualnych (VNET) z dwoma podsieciami i dwie grupy zabezpieczeń sieci (NSG). 

[Network.parameters.json] [ network-parameters-arm] plik parametrów zawiera nazwy podsieci i grup NSG, które interfejs API zarządzania i sieci szkieletowej usług zostaną wdrożone do. Ten przewodnik dla wartości parametrów, które nie muszą zostać zmienione. Zarządzanie interfejsami API i Menedżer zasobów sieci szkieletowej usług szablony użycie tych wartości, więc jeśli zostaną zmodyfikowane w tym miejscu, należy je zmodyfikować w innych szablonów usługi Resource Manager w związku z tym. 

 1. Pobierz następującego pliku szablonu i parametry Menedżera zasobów:

    - [Network.JSON][network-arm]
    - [Network.parameters.JSON][network-parameters-arm]

 2. Do wdrażania Menedżera zasobów plików szablonu i parametru konfiguracji sieci, należy użyć następującego polecenia programu PowerShell:

    ```powershell
    New-AzureRmResourceGroupDeployment -ResourceGroupName <my-resource-group> -TemplateFile .\network.json -TemplateParameterFile .\network.parameters.json -Verbose
    ```

### <a name="deploy-the-service-fabric-cluster"></a>Wdrażanie klastra sieci szkieletowej usług

Zasoby sieciowe zostały wykonane wdrażanie następnym krokiem jest wdrożenie klastra sieci szkieletowej usług w podsieci sieci Wirtualnej i NSG przeznaczone dla klastra sieci szkieletowej usług. W tym samouczku szablon Menedżera zasobów sieci szkieletowej usług jest wstępnie skonfigurowana do używania nazwy sieci Wirtualnej, podsieci i NSG, skonfigurowanym w poprzednim kroku. 

Szablonu usługi Resource Manager klastra usługi sieć szkieletowa jest skonfigurowany do tworzenia bezpiecznego klastra z certyfikatu zabezpieczeń. Certyfikat służy do zabezpieczania komunikacji między węzłami klastra oraz zarządzanie dostępem użytkowników do klastra usługi sieć szkieletowa usług. Zarządzanie interfejsami API używa tego certyfikatu do dostępu do usługi nazewnictwa sieci szkieletowej usług dla potrzeb odnajdywania usług.

Ten krok wymaga mające certyfikat w magazynie kluczy dla zabezpieczeń klastra. Aby uzyskać więcej informacji na temat konfigurowania bezpiecznej klastra z Key Vault, zobacz [ten przewodnik na temat tworzenia klastra na platformie Azure przy użyciu usługi Resource Manager](service-fabric-cluster-creation-via-arm.md)

> [!NOTE]
> Można dodać uwierzytelniania usługi Azure Active Directory oprócz certyfikatu używanego na potrzeby dostępu do klastra. Azure Active Directory jest zalecanym sposobem zarządzanie dostępem użytkowników do klastra usługi sieć szkieletowa, ale nie jest konieczne do ukończenia tego samouczka. Certyfikat jest wymagany niezależnie od sposobu zabezpieczenia węzła do węzła klastra, a do uwierzytelniania usługi Azure API Management, który obecnie nie obsługuje uwierzytelniania w usłudze Azure Active Directory dla usługi Service Fabric w wewnętrznej bazie danych.

 1. Pobierz następującego pliku szablonu i parametry Menedżera zasobów:
 
    - [cluster.JSON][cluster-arm]
    - [cluster.parameters.JSON][cluster-parameters-arm]

 2. Wypełnij puste parametry w `cluster.parameters.json` plików dla danego wdrożenia, łącznie z [informacje magazynie klucza](service-fabric-cluster-creation-via-arm.md#set-up-a-key-vault) dla certyfikatu klastra.

 3. Użyj następującego polecenia programu PowerShell do wdrażania Menedżera zasobów plików szablonu i parametru do utworzenia klastra sieci szkieletowej usług:

    ```powershell
    New-AzureRmResourceGroupDeployment -ResourceGroupName <my-resource-group> -TemplateFile .\cluster.json -TemplateParameterFile .\cluster.parameters.json -Verbose
    ```

### <a name="deploy-api-management"></a>Wdrażanie usługi API Management

Ponadto należy wdrożyć zarządzanie interfejsami API w podsieci sieci Wirtualnej i NSG przeznaczone dla interfejsu API zarządzania. Nie trzeba czekać na zakończenie przed wdrożeniem usługi API Management wdrożenia klastra sieci szkieletowej usług. 

W tym samouczku szablon Menedżera zasobów interfejsu API zarządzania jest wstępnie skonfigurowana do używania nazwy sieci Wirtualnej, podsieci i NSG, skonfigurowanym w poprzednim kroku. 

 1. Pobierz następującego pliku szablonu i parametry Menedżera zasobów:
 
    - [APIM.JSON][apim-arm]
    - [APIM.parameters.JSON][apim-parameters-arm]

 2. Wypełnij puste parametry w `apim.parameters.json` dla danego wdrożenia.

 3. Wdrażać pliki szablonu i parametr Menedżera zasobów dla interfejsu API zarządzania za pomocą następującego polecenia programu PowerShell:

    ```powershell
    New-AzureRmResourceGroupDeployment -ResourceGroupName <my-resource-group> -TemplateFile .\apim.json -TemplateParameterFile .\apim.parameters.json -Verbose
    ```

## <a name="configure-api-management"></a>Skonfiguruj zarządzanie interfejsami API

Po są wdrażane w klastrze API Management i sieci szkieletowej usług, można skonfigurować sieci szkieletowej usług w wewnętrznej bazie danych w usłudze API Management. Dzięki temu można utworzyć zasadę usługi wewnętrznej bazy danych, która wysyła ruchu do klastra usługi sieć szkieletowa usług.

### <a name="api-management-security"></a>Zabezpieczenia Management API

Aby skonfigurować sieć szkieletowa usług wewnętrznej bazy danych, należy najpierw skonfigurować ustawienia zabezpieczeń interfejsu API zarządzania. Aby skonfigurować ustawienia zabezpieczeń, przejdź do usługi Zarządzanie interfejsami API w portalu Azure.

#### <a name="enable-the-api-management-rest-api"></a>Włącz zarządzanie interfejsami API interfejsu API REST

Interfejs API REST zarządzania interfejsu API jest obecnie jedynym sposobem na skonfigurowanie usługi wewnętrznej bazy danych. Pierwszym krokiem jest Włącz interfejs API REST zarządzania interfejsu API i zabezpieczyć.

 1. W usłudze API Management wybierz **API Management — wersja ZAPOZNAWCZA** w obszarze **zabezpieczeń**.
 2. Sprawdź **Włącz interfejs API zarządzania interfejsu API REST** wyboru.
 3. Zanotuj adres URL interfejsu API zarządzania — jest to adres URL, które będą później używane do konfigurowania sieci szkieletowej usług wewnętrznej bazy danych
 4. Generuj **tokenu dostępu** wybierając datę wygaśnięcia oraz klucz, następnie kliknij przycisk **Generuj** przycisku w kierunku dolnej części strony.
 5. Kopiuj **token dostępu** i zapisać go — użyjemy w kolejnych krokach. Należy zauważyć, że ta różni się od klucza podstawowego i pomocniczego klucza.

#### <a name="upload-a-service-fabric-client-certificate"></a>Przekaż certyfikat klienta sieci szkieletowej usług

Zarządzanie interfejsami API musi uwierzytelniać się z klastrem usługi sieć szkieletowa usług dla potrzeb odnajdywania usług przy użyciu certyfikatu klienta, który ma dostęp do klastra. Dla uproszczenia samouczku ten sam certyfikat określony podczas tworzenia klastra usługi sieć szkieletowa usług, które domyślnie mogą być używane do dostępu klastra.

 1. W usłudze API Management wybierz **certyfikatów klienta — w wersji ZAPOZNAWCZEJ** w obszarze **zabezpieczeń**.
 2. Kliknij przycisk **+ Dodaj** przycisku
 2. Wybierz plik klucza prywatnego (pfx) certyfikatu klastra, który określone podczas tworzenia klastra usługi sieć szkieletowa, nadaj mu nazwę i podaj hasło klucza prywatnego.

> [!NOTE]
> W tym samouczku używa tego samego certyfikatu uwierzytelniania klienta i zabezpieczenia węzła do węzła klastra. Certyfikat klienta w osobnym może używać, jeśli jest skonfigurowane do korzystania z klastra usługi sieć szkieletowa usług.

### <a name="configure-the-backend"></a>Skonfiguruj wewnętrznej bazy danych

Teraz, gdy zabezpieczeń interfejsu API zarządzania jest skonfigurowany, można skonfigurować sieci szkieletowej usług wewnętrznej bazy danych. W przypadku zapleczy sieci szkieletowej usług klastra usługi sieć szkieletowa jest wewnętrznej bazy danych, a nie dla określonej usługi sieć szkieletowa usług. Dzięki temu jednych zasad można kierować do więcej niż jedną usługę w klastrze.

Ten krok wymaga wcześniej wygenerowania tokenu dostępu i odcisk palca dla certyfikatu klastra, który został przekazany do interfejsu API zarządzania w poprzednim kroku.

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

**Adres url** tutaj parametr ma nazwę usługi w pełni kwalifikowaną usługi w klastrze, że wszystkie żądania są kierowane do domyślnie, jeśli nazwa usługi, nie jest określona w zasadzie wewnętrznej bazy danych. Może używać nazw fałszywych usług, takich jak "fabric: / fałszywych/usługi" Jeśli chcesz, aby usługę rezerwowy. Należy pamiętać, **adres url** musi być w formacie "fabric: / app/usługi" nawet jeśli jest fałszywy usługę rezerwowy.

Odwoływać się do usługi API Management [interfejs API zaplecza dokumentacji](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-contract-reference#a-namebackenda-backend) uzyskać więcej informacji dotyczących każdego pola.

#### <a name="example"></a>Przykład

```http
PUT https://your-apim.management.azure-api.net/backends/servicefabric?api-version=2016-10-10
Authorization: SharedAccessSignature 230948023984&Ld93cRGcNU6KZ4uVz7JlfTec4eX43Q9Nu8ndatOgBzs6+f559Pkf3iHX2cSge+r42pn35qGY3TitjrIl13hwcQ==
Content-Type: application/json

{
    "description": "My Service Fabric backend",
    "url": "fabric:/myapp/myservice",
    "protocol": "http",
    "resourceId": "https://your-cluster.westus.cloudapp.azure.com:19080",
    "properties": {
        "serviceFabricCluster": {
            "managementEndpoints": ["https://your-cluster.westus.cloudapp.azure.com:19080"],
            "clientCertificateThumbprint": "57bc463aba3aea3a12a18f36f44154f819f0fe32",
            "serverCertificateThumbprints": ["57bc463aba3aea3a12a18f36f44154f819f0fe32"],
            "maxPartitionResolutionRetries" : 5
        }
    }
}
```

## <a name="deploy-a-service-fabric-back-end-service"></a>Wdrażanie usługi zaplecza sieci szkieletowej usług

Teraz, gdy masz usługi sieć szkieletowa, skonfigurowany jako zaplecza interfejsu API zarządzania zasady wewnętrznej bazy danych można tworzyć dla interfejsów API, które przesyłają dane do usług sieci szkieletowej usług. Ale najpierw należy w usłudze działającej w sieci szkieletowej usług w celu umożliwienia akceptowania żądań.

### <a name="create-a-service-fabric-service-with-an-http-endpoint"></a>Tworzenie usługi sieć szkieletowa usług za pomocą punktu końcowego HTTP

W tym samouczku utworzymy podstawowe bezstanowej platformy ASP.NET Core niezawodnej usługi przy użyciu domyślnego szablonu projektu interfejsu API sieci Web. Spowoduje to utworzenie punktu końcowego HTTP dla usługi, która będzie udostępnić za pośrednictwem usługi Azure API Management:

```
/api/values
```

Rozpocznij od [Konfigurowanie środowiska programowania do tworzenia aplikacji platformy ASP.NET Core](service-fabric-add-a-web-frontend.md#set-up-your-environment-for-aspnet-core).

Po skonfigurowaniu środowiska projektowego, uruchom program Visual Studio jako Administrator i Utwórz usługę platformy ASP.NET Core:

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

    To jest punkt końcowy, który będzie udostępnić za pośrednictwem interfejsu API zarządzania na platformie Azure.

 7. Ponadto można wdrożyć aplikacji do klastra w systemie Azure. [Za pomocą programu Visual Studio](service-fabric-publish-app-remote-cluster.md#to-publish-an-application-using-the-publish-service-fabric-application-dialog-box), kliknij prawym przyciskiem myszy projekt aplikacji i wybierz **publikowania**. Podaj punkt końcowy klastra (na przykład `mycluster.westus.cloudapp.azure.com:19000`) do wdrożenia aplikacji do klastra usługi sieć szkieletowa na platformie Azure.

Usługi bezstanowej platformy ASP.NET Core o nazwie `fabric:/ApiApplication/WebApiService` teraz powinna być uruchomiona w klastrze usługi sieć szkieletowa na platformie Azure.

## <a name="create-an-api-operation"></a>Tworzenie operacji interfejsu API

Teraz już wszystko gotowe do tworzenia operacji w usłudze API Management, używanego przez klientów zewnętrznych do komunikowania się z usługi bezstanowej platformy ASP.NET Core uruchamianych w klastrze usługi sieć szkieletowa usług.

 1. Zaloguj się do portalu Azure i przejdź do wdrożenia usługi Zarządzanie interfejsami API.
 2. W bloku usługi API Management, wybierz **interfejsów API — wersja zapoznawcza**
 3. Dodaj nowy interfejs API, klikając **puste API** pole i wypełniania okno dialogowe:

     - **Adres URL usługi sieci Web**: dla sieci szkieletowej usług zapleczy, ta wartość adresu URL nie jest używany. Tutaj możesz umieścić żadnej wartości. W tym samouczku, użyj: `http://servicefabric`.
     - **Nazwa**: Podaj dowolną nazwę interfejsu API. W tym samouczku, użyj `Service Fabric App`.
     - **Schemat adresu URL**: Wybierz HTTP, HTTPS lub obu. W tym samouczku, użyj `both`.
     - **Sufiks adresu URL interfejsu API**: Podaj sufiks na interfejsie API. W tym samouczku, użyj `myapp`.
 
 4. Po utworzeniu interfejsu API, kliknij przycisk **+ Dodaj operację** powoduje dodanie frontonu operacji interfejsu API. Wprowadź wartości:
    
     - **Adres URL**: Wybierz `GET` i podaj ścieżkę adresu URL dla interfejsu API. W tym samouczku, użyj `/api/values`.
     
       Domyślnie ścieżkę adresu URL określone w tym miejscu są wysyłane ścieżkę adresu URL do zaplecza usługi sieć szkieletowa usług. Jeśli używasz tego samego adresu URL ścieżkę tutaj używanego z usługą, w tym przypadku `/api/values`, następnie operacji działa bez dalszej modyfikacji. Można również określić ścieżki adresu URL w tym miejscu, który różni się od ścieżki adresu URL z wewnętrzną bazą danych usługi Service Fabric, w którym to przypadku należy również określić Edycja ścieżki w zasadach operację później.
     - **Nazwa wyświetlana**: Podaj dowolną nazwę dla interfejsu API. W tym samouczku, użyj `Values`.

## <a name="configure-a-backend-policy"></a>Skonfiguruj zasady wewnętrznej bazy danych

Zasady zaplecza wiąże co wszystko. Jest to, którym można skonfigurować usługi sieci szkieletowej usług zaplecza, do którego są przesyłane żądania. Te zasady można stosować do żadnej operacji interfejsu API. [Wewnętrznej bazy danych konfiguracji dla usługi Service Fabric](https://docs.microsoft.com/azure/api-management/api-management-transformation-policies#SetBackendService) zawiera następujące żądania routingu kontrolki: 
 - Usługa Wybór wystąpienia, określając nazwę wystąpienia usługi Service Fabric, albo zapisane na stałe (na przykład `"fabric:/myapp/myservice"`) lub generowane na podstawie żądania HTTP (na przykład `"fabric:/myapp/users/" + context.Request.MatchedParameters["name"]`).
 - Rozdzielczość partycji przez generowanie klucza partycji przy użyciu dowolnego schemat partycjonowania sieci szkieletowej usług.
 - Wybór repliki dla stanowych usług.
 - Rozdzielczość ponownych prób warunki, które umożliwiają określenie warunków ponowne rozpoznanie lokalizację usługi i wysłać żądanie.

W tym samouczku Utwórz zasadę kieruje żądania bezpośrednio do usługi bezstanowej platformy ASP.NET Core wdrożonej wcześniej wewnętrznej bazy danych:

 1. Wybierz i edytować **przychodzących zasad** dla `Values` operacji, klikając ikonę edycji, a następnie zaznaczając **widoku kodu**.
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

### <a name="add-the-api-to-a-product"></a>Dodawanie interfejsu API z produktem. 

Przed wywołaniem interfejsu API, można dodać go do produktu, w którym można przyznać dostęp użytkownikom. 

 1. W usłudze API Management wybierz **produktów - PREVIEW**.
 2. Domyślnie produkty przeznaczone do interfejsu API zarządzania dwóch dostawców: Starter i bez ograniczeń. Wybierz nieograniczone produktu.
 3. Wybierz interfejsy API.
 4. Kliknij przycisk **+ Dodaj** przycisku.
 5. Wybierz `Service Fabric App` interfejsu API utworzone w poprzednich krokach i kliknij **wybierz** przycisku.

### <a name="test-it"></a>należy przeprowadzić test

Teraz możesz spróbować wysyłania żądania do usługi zaplecza w sieci szkieletowej usług za pośrednictwem interfejsu API zarządzania bezpośrednio z portalu Azure.

 1. W usłudze API Management wybierz **interfejsu API — wersja ZAPOZNAWCZA**.
 2. W `Service Fabric App` interfejsu API utworzone w poprzednich krokach, wybierz opcję **testu** kartę.
 3. Kliknij przycisk **wysyłania** przycisk, aby wysłać żądania testu do usługi zaplecza.

## <a name="next-steps"></a>Następne kroki

W tym momencie powinny mieć podstawowe ustawienia z interfejsu API zarządzania i sieci szkieletowej usług.

W tym samouczku korzysta z uwierzytelniania opartego na certyfikatach użytkownika podstawowego dla klastra usługi sieć szkieletowa można uzyskać, możesz szybko skonfigurować. Bardziej zaawansowanego uwierzytelniania użytkownika dla klastra usługi sieć szkieletowa jest preferowana w [uwierzytelniania usługi Azure Active Directory](service-fabric-cluster-creation-via-arm.md#set-up-azure-active-directory-for-client-authentication). 

Następnie [utworzyć i skonfigurować ustawienia zaawansowane produktu w usłudze Azure API Management](https://docs.microsoft.com/azure/api-management/api-management-howto-product-with-rules) przygotowanie aplikacji dla ruchu w świecie rzeczywistym.

<!-- links -->
[azure-powershell]:https://azure.microsoft.com/documentation/articles/powershell-install-configure/

[network-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/network.json
[network-parameters-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/network.parameters.json

[apim-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/apim.json
[apim-parameters-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/apim.parameters.json

[cluster-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/cluster.json
[cluster-parameters-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/cluster.parameters.json


<!-- pics -->
[sf-apim-topology-overview]: ./media/service-fabric-api-management-quickstart/sf-apim-topology-overview.png
