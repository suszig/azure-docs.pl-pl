
<properties
   pageTitle="Tworzenie bramy aplikacji przy użyciu szablonów usługi Azure Resource Manager | Microsoft Azure"
   description="Ta strona zawiera instrukcje dotyczące tworzenia bramy aplikacji platformy Azure za pomocą szablonu usługi Azure Resource Manager"
   documentationCenter="na"
   services="application-gateway"
   authors="georgewallace"
   manager="jdial"
   editor="tysonn"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/09/2016"
   ms.author="gwallace"/>


# Tworzenie bramy aplikacji przy użyciu szablonu usługi Azure Resource Manager

Azure Application Gateway to moduł równoważenia obciążenia warstwy 7. Udostępnia tryb failover, oparty na wydajności routing żądań HTTP między różnymi serwerami — w chmurze i lokalnymi. Usługa Application Gateway oferuje następujące funkcje dostarczania aplikacji: równoważenie obciążenia HTTP, koligację sesji opartą na plikach cookie oraz odciążanie protokołu SSL (Secure Sockets Layer).

> [AZURE.SELECTOR]
- [Azure Portal](application-gateway-create-gateway-portal.md)
- [Azure Resource Manager — program PowerShell](application-gateway-create-gateway-arm.md)
- [Klasyczny portal Azure — program PowerShell](application-gateway-create-gateway.md)
- [Szablon usługi Azure Resource Manager](application-gateway-create-gateway-arm-template.md)
- [Interfejs wiersza polecenia platformy Azure](application-gateway-create-gateway-cli.md)

<BR>

Dowiesz się, jak pobrać i zmodyfikować istniejący szablon usługi Azure Resource Manager z usługi GitHub oraz jak wdrożyć szablon z serwisu GitHub, programu PowerShell i interfejsu wiersza polecenia platformy Azure.

Jeśli po prostu wdrażasz szablon usługi Azure Resource Manager bezpośrednio z serwisu GitHub bez wprowadzania żadnych zmian, przejdź do wdrażania szablonu z serwisu GitHub.


## Scenariusz

W ramach tego scenariusza wykonasz następujące czynności:

- Tworzenie bramy aplikacji z dwoma wystąpieniami.
- Tworzenie sieci wirtualnej o nazwie VirtualNetwork1 przy użyciu zarezerwowanego bloku CIDR 10.0.0.0/16.
- Tworzenie podsieci o nazwie Appgatewaysubnet używającej bloku 10.0.0.0/28 jako bloku CIDR.
- Ustawianie dwóch wcześniej skonfigurowanych adresów IP zaplecza dla serwerów sieci Web, w przypadku których chcesz równoważyć obciążenie ruchem. W tym przykładzie szablonu adresy IP zaplecza to 10.0.1.10 i 10.0.1.11.

>[AZURE.NOTE] Te ustawienia to parametry tego szablonu. Aby dostosować szablon, możesz zmienić reguły, odbiornik oraz protokół SSL, który służy do otwierania pliku azuredeploy.json.



![Scenariusz](./media/application-gateway-create-gateway-arm-template/scenario-arm.png)



## Pobieranie szablonu usługi Azure Resource Manager i zapoznawanie się z nim

Z witryny GitHub można pobrać istniejący szablon usługi Azure Resource Manager umożliwiający utworzenie sieci wirtualnej z dwoma podsieciami, wprowadzić wybrane zmiany, a następnie ponownie go użyć. W tym celu wykonaj poniższe kroki:

1. Przejdź do szablonu [Tworzenie bramy aplikacji](https://github.com/Azure/azure-quickstart-templates/tree/master/101-application-gateway-create).
2. Kliknij plik **azuredeploy.json**, a następnie kliknij opcję **RAW**.
3. Zapisz plik w folderze lokalnym na komputerze.
4. Jeśli znasz szablony usługi Azure Resource Manager, przejdź do kroku 7.
5. Otwórz zapisany plik i przyjrzyj się zawartości sekcji **parameters** w wierszu 5. Parametry szablonu usługi Azure Resource Manager zawierają symbole zastępcze wartości, które można wypełnić podczas wdrażania.

  	| Parametr | Opis |
  	|---|---|
  	| **location** | Region świadczenia usługi Azure, w którym zostanie utworzona brama aplikacji |
  	| **VirtualNetwork1** | Nazwa nowej sieci wirtualnej |
  	| **addressPrefix** | Przestrzeń adresowa sieci wirtualnej w formacie CIDR |
  	| **ApplicationGatewaysubnet** | Nazwa podsieci bramy aplikacji |
  	| **subnetPrefix** | Blok CIDR podsieci bramy aplikacji |
  	| **skuname** | Rozmiar wystąpienia jednostki SKU |
  	| **capacity** | Liczba wystąpień |
  	| **backendaddress1** | Adres IP pierwszego serwera sieci Web |
  	| **backendaddress2** | Adres IP drugiego serwera sieci Web |


    >[AZURE.IMPORTANT] Szablony usługi Azure Resource Manger utrzymywane w usłudze GitHub mogą z upływem czasu ulec zmianie. Przed użyciem szablonu należy go sprawdzić.

6. Sprawdź zawartość w obszarze **resources**, w szczególności następujące pozycje:

    - **type**. Typ zasobu tworzonego przez szablon. W tym przypadku typ to **Microsoft.Network/applicationGateways**. Reprezentuje on bramę aplikacji.
    - **name**. Nazwa zasobu. Zwróć uwagę na sposób użycia elementu **[parameters('applicationGatewayName')]**. Oznacza on, że nazwa zostanie podana przez użytkownika jako dane wejściowe lub w pliku parametrów podczas wdrażania.
    - **properties**. Lista właściwości zasobu. Ten szablon korzysta z sieci wirtualnej i publicznego adresu IP podczas tworzenia aplikacji bramy.

7. Przejdź z powrotem na stronę [https://github.com/Azure/azure-quickstart-templates/blob/master/101-application-gateway-create/](https://github.com/Azure/azure-quickstart-templates/blob/master/101-application-gateway-create).
8. Kliknij plik **azuredeploy-paremeters.json**, a następnie kliknij opcję **RAW**.
9. Zapisz plik w folderze lokalnym na komputerze.
10. Otwórz zapisany plik i edytuj wartości parametrów. Do wdrożenia opisanej w scenariuszu bramy aplikacji użyj poniższych wartości.

        {
          "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
        {
        "location" : {
        "value" : "West US"
        },
        "addressPrefix": {
        "value": "10.0.0.0/16"
        },
        "subnetPrefix": {
        "value": "10.0.0.0/24"
        },
        "skuName": {
        "value": "Standard_Small"
        },
        "capacity": {
        "value": 2
        },
        "backendIpAddress1": {
        "value": "10.0.1.10"
        },
        "backendIpAddress2": {
        "value": "10.0.1.11"
        }
        }

11. Zapisz plik. Szablon JSON i szablon parametrów można przetestować za pomocą narzędzi weryfikacji danych JSON w trybie online, takich jak [JSlint.com](http://www.jslint.com/).

## Wdrażanie szablonu usługi Azure Resource Manager przy użyciu programu PowerShell

Jeśli nie znasz programu Azure PowerShell, zapoznaj się z artykułem [Instalowanie i konfigurowanie programu Azure PowerShell](../powershell-install-configure.md) i postępuj zgodnie z instrukcjami, aby zalogować się w programie Azure i wybrać subskrypcję.

### Krok 1

    Login-AzureRmAccount



### Krok 2

Sprawdź subskrypcje dostępne na koncie.

    Get-AzureRmSubscription

Zostanie wyświetlony monit o uwierzytelnienie przy użyciu własnych poświadczeń.<BR>

### Krok 3

Wybierz subskrypcję platformy Azure do użycia. <BR>


    Select-AzureRmSubscription -Subscriptionid "GUID of subscription"


### Krok 4


W razie potrzeby utwórz grupę zasobów za pomocą polecenia cmdlet **New-AzureResourceGroup**. W poniższym przykładzie utworzona zostanie nowa grupa zasobów o nazwie AppgatewayRG w lokalizacji Wschodnie stany USA.

    New-AzureRmResourceGroup -Name AppgatewayRG -Location "East US"

        ResourceGroupName : AppgatewayRG
        Location          : eastus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
                     Actions  NotActions
                     =======  ==========
                      *

        ResourceId        : /subscriptions/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/resourceGroups/AppgatewayRG

Uruchom polecenie cmdlet **New-AzureRmResourceGroupDeployment**, aby wdrożyć nową sieć wirtualną przy użyciu uprzednio pobranych i zmodyfikowanych plików szablonu oraz parametrów.

    New-AzureRmResourceGroupDeployment -Name TestAppgatewayDeployment -ResourceGroupName AppgatewayRG `
        -TemplateFile C:\ARM\azuredeploy.json -TemplateParameterFile C:\ARM\azuredeploy-parameters.json

Dane wyjściowe generowane przy użyciu wiersza polecenia są następujące:

    DeploymentName    : testappgatewaydeployment
    ResourceGroupName : appgatewayRG
    ProvisioningState : Succeeded
    Timestamp         : 9/19/2015 1:49:41 AM
    Mode              : Incremental
    TemplateLink      :
    Parameters        :
                Name             Type                       Value
                ===============  =========================  ==========
                location         String                     East US
                addressPrefix    String                     10.0.0.0/16
                subnetPrefix     String                     10.0.0.0/24
                skuName          String                     Standard_Small
                capacity         Int                        2
                backendIpAddress1  String                     10.0.1.10
                backendIpAddress2  String                     10.0.1.11

    Outputs           :


## Wdrażanie szablonu usługi Azure Resource Manager przy użyciu interfejsu wiersza polecenia platformy Azure

Aby wdrożyć pobrany szablon usługi Azure Resource Manager przy użyciu interfejsu wiersza polecenia platformy Azure, wykonaj poniższe kroki:

### Krok 1

Jeśli po raz pierwszy używasz interfejsu wiersza polecenia platformy Azure, zobacz artykuł [Install and configure the Azure CLI](../xplat-cli-install.md) (Instalowanie i konfigurowanie interfejsu wiersza polecenia Azure) i postępuj zgodnie z instrukcjami aż do momentu wybrania konta i subskrypcji platformy Azure.
### Krok 2

Uruchom polecenie **azure config mode**, aby włączyć tryb usługi Resource Manager, jak pokazano poniżej.

    azure config mode arm

Oto oczekiwane dane wyjściowe po wprowadzeniu powyższego polecenia:

    info:   New mode is arm

### Krok 3

W razie potrzeby uruchom polecenie **azure group create**, aby utworzyć nową grupę zasobów, jak pokazano poniżej. Zwróć uwagę na dane wyjściowe polecenia. Lista wyświetlana po danych wyjściowych zawiera opis używanych parametrów. Aby uzyskać więcej informacji na temat grup zasobów, zobacz temat [Omówienie usługi Azure Resource Manager](../resource-group-overview.md).

    azure group create -n appgatewayRG -l eastus

**-n (lub --name)**. Nazwa nowej grupy zasobów. W naszym scenariuszu jest to *appgatewayRG*.

**-l (lub --location)**. Region świadczenia usługi Azure, w którym zostanie utworzona nowa grupa zasobów. W naszym scenariuszu jest to *eastus*.

### Krok 4

Uruchom polecenie cmdlet **azure group deployment create**, aby wdrożyć nową sieć wirtualną przy użyciu uprzednio pobranych i zmodyfikowanych plików szablonu oraz parametrów. Lista wyświetlana po danych wyjściowych zawiera opis używanych parametrów.

    azure group deployment create -g appgatewayRG -n TestAppgatewayDeployment -f C:\ARM\azuredeploy.json -e C:\ARM\azuredeploy-parameters.json

Oto oczekiwane dane wyjściowe po wprowadzeniu powyższego polecenia:

    azure group deployment create -g appgatewayRG -n TestAppgatewayDeployment -f C:\ARM\azuredeploy.json -e C:\ARM\azuredeploy-parameters.json
    info:    Executing command group deployment create
    + Initializing template configurations and parameters
    + Creating a deployment
    info:    Created template deployment "TestAppgatewayDeployment"
    + Waiting for deployment to complete
    data:    DeploymentName     : TestAppgatewayDeployment
    data:    ResourceGroupName  : appgatewayRG
    data:    ProvisioningState  : Succeeded
    data:    Timestamp          : 2015-09-21T20:50:27.5129912Z
    data:    Mode               : Incremental
    data:    Name               Type    Value
    data:    -----------------  ------  --------------
    data:    location           String  East US
    data:    addressPrefix      String  10.0.0.0/16
    data:    subnetPrefix       String  10.0.0.0/24
    data:    skuName            String  Standard_Small
    data:    capacity           Int     2
    data:    backendIpAddress1  String  10.0.1.10
    data:    backendIpAddress2  String  10.0.1.11
    info:    group deployment create command OK

**-g (lub --resource-group)**. Nazwa grupy zasobów, w której zostanie utworzona nowa sieć wirtualna.

**-f (lub --template-file)**. Ścieżka do pliku szablonu usługi Azure Resource Manager.

**-e (lub --parameters-file)**. Ścieżka do pliku parametrów usługi Azure Resource Manager.

## Wdrażanie szablonu usługi Azure Resource Manager przy użyciu funkcji szybkiego wdrażania

Użycie funkcji szybkiego wdrażania to kolejny sposób korzystania z szablonów usługi Azure Resource Manager. Jest to prosty sposób używania szablonów w portalu Azure.


### Krok 1
Przejdź do artykułu [Create an application gateway with public IP](https://azure.microsoft.com/documentation/templates/101-application-gateway-public-ip/) (Tworzenie bramy aplikacji przy użyciu publicznego adresu IP).


### Krok 2

Kliknij pozycję **Wdrażanie na platformie Azure**.

![Wdrażanie na platformie Azure](./media/application-gateway-create-gateway-arm-template/deploytoazure.png)

### Krok 3

Wprowadź parametry szablonu wdrożenia w portalu i kliknij przycisk **OK**.

![Parametry](./media/application-gateway-create-gateway-arm-template/ibiza1.png)

### Krok 4

Wybierz pozycję **Postanowienia prawne** i kliknij pozycję **Kup**.

### Krok 5

W bloku wdrożenia niestandardowego kliknij pozycję **Utwórz**.



## Następne kroki

Jeśli chcesz skonfigurować odciążanie protokołu SSL, zobacz artykuł [Configure an application gateway for SSL offload](application-gateway-ssl.md) (Konfigurowanie bramy aplikacji na potrzeby odciążania protokołu SSL).

Jeśli chcesz skonfigurować bramę aplikacji do użycia z wewnętrznym modułem równoważenia obciążenia, zobacz artykuł [Create an application gateway with an internal load balancer (ILB)](application-gateway-ilb.md) (Tworzenie bramy aplikacji przy użyciu wewnętrznego modułu równoważenia obciążenia).

Więcej ogólnych informacji na temat opcji równoważenia obciążenia możesz znaleźć w następujących artykułach:

- [Azure Load Balancer](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)



<!--HONumber=sep16_HO1-->


