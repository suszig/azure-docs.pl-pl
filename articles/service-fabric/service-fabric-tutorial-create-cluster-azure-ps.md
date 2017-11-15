---
title: "Tworzenie klastra sieci szkieletowej usług na platformie Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak utworzyć klaster systemu Windows lub Linux Service Fabric na platformie Azure przy użyciu programu PowerShell"
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
ms.date: 10/03/2017
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 217b9f2f0dfed5b095e1bac1c8146abf4753fadc
ms.sourcegitcommit: 659cc0ace5d3b996e7e8608cfa4991dcac3ea129
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/13/2017
---
# <a name="create-a-secure-cluster-in-azure-by-using-powershell"></a>Tworzenie bezpiecznej klastra na platformie Azure przy użyciu programu PowerShell
W tym artykule jest pierwszy z serii samouczków, przedstawiających sposób przenoszenia aplikacji .NET do chmury przy użyciu klastrów sieci szkieletowej usług Azure i kontenerów. W poniższych krokach można Dowiedz się, jak utworzyć klaster sieci szkieletowej usług (Windows lub Linux) działającą w usłudze Azure. Po zakończeniu, masz bezpiecznego klastra, który działa w chmurze, do którego można wdrożyć aplikacji.

## <a name="prerequisites"></a>Wymagania wstępne
Przed rozpoczęciem tego samouczka:
- Jeśli nie masz subskrypcji platformy Azure, Utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Zainstaluj [usługi SDK sieci szkieletowej](service-fabric-get-started.md).
- Zainstaluj [programu Azure Powershell w wersji modułu 4.1 lub wyższej](https://docs.microsoft.com/powershell/azure/install-azurerm-ps). (W razie potrzeby [zainstalować program Azure PowerShell](/powershell/azure/overview) lub [zaktualizować do nowszej wersji](https://docs.microsoft.com/en-us/powershell/azure/install-azurerm-ps?view=azurermps-4.4.0#update-azps).)


## <a name="create-a-service-fabric-cluster"></a>Tworzenie klastra sieci szkieletowej usług

Ten skrypt tworzy jednowęzłowy, Service Fabric w wersji preview klastra. Certyfikatu z podpisem własnym zabezpiecza klastra. Skrypt tworzy certyfikat wraz z klastra, a następnie umieszcza certyfikat w magazynie kluczy. Nie można skalować jednowęzłowych poza jednej maszyny wirtualnej, a nie można uaktualnić klastrów w wersji zapoznawczej do nowszej wersji.

Aby obliczyć kosztów ponoszonych przez uruchomienie klastra sieci szkieletowej usług platformy Azure, użyj [Azure Kalkulator cen](https://azure.microsoft.com/pricing/calculator/).
Aby uzyskać więcej informacji na temat tworzenia klastrów sieci szkieletowej usług, zobacz [tworzenia klastra usługi sieć szkieletowa usług za pomocą usługi Azure Resource Manager](service-fabric-cluster-creation-via-arm.md).

## <a name="log-in-to-azure"></a>Zaloguj się do platformy Azure.
Otwórz konsolę programu PowerShell, logowanie do platformy Azure i wybierz subskrypcję, którą chcesz wdrożyć w klastrze:

   ```PowerShell
   Login-AzureRmAccount
   Select-AzureRmSubscription -SubscriptionId <subscription-id>
   ```

## <a name="cluster-parameters"></a>Parametry klastra

   Ten skrypt korzysta z następujących parametrów i pojęcia. Dostosuj parametry zgodnie z własnymi wymaganiami.

   | Parametr       | Opis | Sugerowana wartość |
   | --------------- | ----------- | --------------- |
   | Lokalizacja | Region platformy Azure, gdzie wdrażania klastra. | Na przykład *westeurope*, *eastasia*, lub *eastus* |
   | Nazwa     | Nazwa klastra, który chcesz utworzyć. Nazwa musi mieć 4-23 znaków i może mieć tylko małe litery, cyfry i łączniki. | Na przykład *sfpreviewcluster Komputer1* |
   | Grupy zasobów o nazwie   | Nazwa grupy zasobów, w której chcesz utworzyć klaster. | Na przykład *myresourcegroup* |
   | VmSku  | Maszyna wirtualna jednostki SKU dla węzłów. | Wszelkie prawidłową maszynę wirtualną jednostki SKU |
   | System operacyjny  | Maszyna wirtualna systemu operacyjnego do użycia dla węzłów. | Wszelkie prawidłową maszynę wirtualną systemu operacyjnego |
   | Nazwa | Nazwa nowego magazynu kluczy do skojarzenia z klastrem. | Na przykład *mykeyvault* |
   | ClusterSize | Liczba maszyn wirtualnych w klastrze (może być *1* lub *3-99*).| Określ tylko jedną maszynę wirtualną klastra w wersji zapoznawczej |
   | CertificateSubjectName | Nazwa podmiotu certyfikatu, który ma zostać utworzony. | Zgodne z formatem:  *<name>* . *<location>* . cloudapp.azure.com |

### <a name="default-parameter-values"></a>Domyślne wartości parametrów
**Maszyna wirtualna**: ustawienia opcjonalne. Jeśli nie zostały określone, nazwa użytkownika domyślnie *vmadmin* i programu PowerShell wyświetli monit o podanie hasła maszyny wirtualnej przed utworzeniem klastra.

**Porty**: Domyślnie porty 80 i 8081. Możesz określić dodatkowe porty wykonując wskazówki dotyczące [portów w klastrach usługi sieć szkieletowa](https://docs.microsoft.com/en-us/azure/service-fabric/create-load-balancer-rule).

**Diagnostyka**: domyślnie włączone.

**Usługa DNS**: nie jest domyślnie włączone.

**Odwrotny serwer proxy**: nie jest domyślnie włączone.

## <a name="create-the-cluster-with-your-parameters"></a>Tworzenie klastra z parametrami

Po wybraniu spełniających wymagania dotyczące parametrów, uruchom następujące polecenie do generowania bezpiecznego klastra sieci szkieletowej usług i jego certyfikatu.

Możesz zmodyfikować ten skrypt, aby uwzględnić dodatkowe parametry. Aby uzyskać więcej informacji na parametry w celu tworzenia klastra, zobacz [AzureRmServiceFabricCluster nowy](/powershell/module/azurerm.servicefabric/new-azurermservicefabriccluster.md) polecenia cmdlet.

>[!NOTE]
>Przed uruchomieniem tego polecenia należy utworzyć folder, w której możesz przechowywać certyfikatu klastra.

```PowerShell

# Set the certificate variables. This creates and encrypts a password that Service Fabric will use.
$certpwd="Password#1234" | ConvertTo-SecureString -AsPlainText -Force

# You must create the folder where you want to store the certificate on your machine before you start this step.
$certfolder="c:\mycertificates\"

# Set the variables for common values. Change the values to fit your needs.
$clusterloc="WestUS"
$clustername = "mysfcluster"
$groupname="mysfclustergroup"       
$vmsku = "Standard_D2_v2"
$vaultname = "mykeyvault"
$subname="$clustername.$clusterloc.cloudapp.azure.com"

# Set the number of cluster nodes. The possible values are 1 and 3-99.
$clustersize=1

# Create the Service Fabric cluster and its self-signed certificate. The OS you specify here lets you use this cluster with any applications that are also using containers.
New-AzureRmServiceFabricCluster -Name $clustername -ResourceGroupName $groupname -Location $clusterloc `
-ClusterSize $clustersize -CertificateSubjectName $subname `
-CertificatePassword $certpwd -CertificateOutputFolder $certfolder `
-OS WindowsServer2016DatacenterwithContainers -VmSku $vmsku -KeyVaultName $vaultname
```

Proces tworzenia może potrwać kilka minut. Po zakończeniu konfiguracji generuje informacje o klastrze utworzona na platformie Azure. Kopiuje również certyfikat klastra katalogu - CertificateOutputFolder na ścieżkę, którą określono dla tego parametru.

Zwróć uwagę na **ManagementEndpoint** adres URL dla klastra, która może być na przykład następujący adres URL: https://mycluster.westeurope.cloudapp.azure.com:19080.

## <a name="import-the-certificate"></a>Zaimportuj certyfikat

Po pomyślnym utworzeniu klastra, uruchom następujące polecenie, aby upewnić się, że można użyć certyfikatu z podpisem własnym:

```PowerShell

# To connect to the cluster, install the certificate into the Personal (My) store of the current user on your computer.
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My `
-FilePath C:\mycertificates\mysfclustergroup20170531104310.pfx `
-Password $certpwd
```

To polecenie instaluje certyfikat na bieżącego użytkownika na komputerze. Należy na dostęp do Eksploratora usługi sieć szkieletowa i wyświetlić informacje o kondycji klastra tego certyfikatu.


## <a name="view-your-cluster-optional"></a>Wyświetl klastra (opcjonalnie)

Po umieszczeniu w i zaimportowany certyfikat, możesz połączyć się z klastrem i wyświetlić jej kondycji. Istnieje wiele sposobów łączenia, za pomocą Eksploratora usługi sieć szkieletowa lub programu PowerShell.

### <a name="service-fabric-explorer"></a>Service Fabric Explorer
Można wyświetlić kondycję klastra za pomocą Eksploratora usługi sieć szkieletowa. Aby to zrobić, przejdź do **ManagementEndpoint** adres URL dla klastra, a następnie wybierz certyfikat zapisany na komputerze.

>[!NOTE]
>Po otwarciu Eksploratora usługi sieć szkieletowa, zobacz błąd certyfikatu, ponieważ używasz certyfikatu z podpisem własnym. W programie Edge, trzeba kliknąć **szczegóły**, a następnie kliknij przycisk **przejdź do strony sieci Web** łącza. W przeglądarce Chrome, trzeba kliknąć **zaawansowane**, a następnie kliknij przycisk **kontynuować** łącza.

### <a name="powershell"></a>PowerShell

Moduł programu PowerShell usługi Service Fabric zawiera wiele poleceń cmdlet do zarządzania klastrami, aplikacji i usług sieci szkieletowej usług. Użyj [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster) polecenia cmdlet, aby połączyć się z klastrem bezpieczne. Szczegóły punktu końcowego połączenia i odcisk palca certyfikatu można znaleźć w danych wyjściowych z poprzedniego kroku.

```PowerShell
Connect-ServiceFabricCluster -ConnectionEndpoint mysfcluster.southcentralus.cloudapp.azure.com:19000 `
-KeepAliveIntervalInSec 10 `
-X509Credential -ServerCertThumbprint C4C1E541AD512B8065280292A8BA6079C3F26F10 `
-FindType FindByThumbprint -FindValue C4C1E541AD512B8065280292A8BA6079C3F26F10 `
-StoreLocation CurrentUser -StoreName My
```

Możesz również sprawdzić czy masz połączenie, i że klaster jest w dobrej kondycji za pomocą [Get-ServiceFabricClusterHealth](/powershell/module/servicefabric/get-servicefabricclusterhealth) polecenia cmdlet.

```PowerShell
Get-ServiceFabricClusterHealth
```

## <a name="next-steps"></a>Następne kroki
W tym samouczku przedstawiono sposób tworzenia bezpiecznego klastra sieci szkieletowej usług na platformie Azure przy użyciu programu PowerShell.

Następnie przejdź do następujących samouczkiem, aby dowiedzieć się, jak wdrożyć istniejącą aplikację:
> [!div class="nextstepaction"]
> [Wdrażanie istniejącą aplikację .NET z rozwiązania Docker Compose](service-fabric-host-app-in-a-container.md)

 
