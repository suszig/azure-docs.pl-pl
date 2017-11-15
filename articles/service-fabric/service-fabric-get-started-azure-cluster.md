---
title: "Konfigurowanie klastra usługi Azure Service Fabric | Microsoft Docs"
description: "Ten przewodnik Szybki start pomoże Ci w utworzeniu klastra usługi Service Fabric z systemem Windows lub Linux na platformie Azure."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/13/2017
ms.author: ryanwi
ms.openlocfilehash: caf76bb739fa92982c511c8e3e6c6aaf2bf9d6c1
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/03/2017
---
# <a name="create-your-first-service-fabric-cluster-on-azure"></a>Tworzenie pierwszego klastra usługi Service Fabric na platformie Azure
[Klaster usługi Azure Service Fabric](service-fabric-deploy-anywhere.md) jest połączonym z siecią zestawem maszyn wirtualnych lub fizycznych, w którym wdraża się mikrousługi i nimi zarządza. Niniejszy przewodnik Szybki start pomaga w utworzeniu klastra o pięciu węzłach, z systemem Windows lub Linux, za pośrednictwem środowiska [Azure PowerShell](https://msdn.microsoft.com/library/dn135248) lub witryny [Azure Portal](http://portal.azure.com) w ciągu kilku minut. W tym celu można także użyć interfejsu wiersza polecenia platformy Azure.  

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).


## <a name="use-the-azure-portal"></a>Korzystanie z witryny Azure Portal

Zaloguj się w witrynie Azure Portal pod adresem [http://portal.azure.com](http://portal.azure.com).

### <a name="create-the-cluster"></a>Tworzenie klastra

1. W lewym górnym rogu witryny Azure Portal wybierz przycisk **Nowy**.
2. Wyszukaj pozycję **Service Fabric** i wybierz ze zwróconych wyników pozycję **Klaster usługi Service Fabric**. Następnie wybierz przycisk **Utwórz**.
3. Uzupełnij formularz **Podstawy** usługi Service Fabric. W sekcji **System operacyjny** wybierz wersję systemu Windows lub Linux, którego chcesz używać w węzłach klastra. Nazwa użytkownika i hasło wprowadzone w tym miejscu są używane na potrzeby logowania się do maszyny wirtualnej. W obszarze **Grupa zasobów** utwórz nową. Grupa zasobów to logiczny kontener, w którym są tworzone i zbiorczo zarządzane zasoby platformy Azure. Gdy skończysz, kliknij przycisk **OK**.

    ![Zrzut ekranu przedstawiający dane wyjściowe instalacji klastra][cluster-setup-basics]

4. Uzupełnij formularz **Konfiguracja klastra**. Dla ustawienia **Liczba typów węzłów** wprowadź wartość **1**.

5. Wybierz pozycję **Typ węzła 1 (podstawowy)** i wypełnij formularz **Konfiguracja typu węzła**. Wprowadź nazwę typu węzła i dla pozycji [Warstwa trwałości](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster) ustaw wartość **Brązowa**. Następnie wybierz rozmiar maszyny wirtualnej.

    Typy węzłów definiują rozmiar maszyny wirtualnej, liczbę maszyn wirtualnych, niestandardowe punkty końcowe oraz inne ustawienia dla maszyn wirtualnych tego typu. Każdy zdefiniowany typ węzła jest konfigurowany jako oddzielny zestaw skalowania maszyn wirtualnych, który jest używany do wdrażania maszyn wirtualnych i zarządzania nimi jako zestawem. Każdy typ węzła może być niezależnie skalowany w górę lub w dół, może mieć różne zestawy otwartych portów i może mieć różne metryki pojemności. Pierwszy lub główny typ węzła jest miejscem, w którym hostowane są usługi systemowe Service Fabric. Ten typ węzła musi zawierać co najmniej pięć maszyn wirtualnych.

    W przypadku wszystkich wdrożeń produkcyjnych [planowanie pojemności](service-fabric-cluster-capacity.md) jest ważnym krokiem. Niemniej w ramach tego przewodnika Szybki start nie uruchamiasz aplikacji, więc wybierz rozmiar maszyny wirtualnej *Standardowa DS1_v2*. Wybierz wartość **Srebrna** dla [warstwy niezawodności](service-fabric-cluster-capacity.md#the-reliability-characteristics-of-the-cluster) oraz ustaw początkową pojemność zestawu skalowania maszyn wirtualnych na 5.  

    Niestandardowe punkty końcowe otwierają porty w usłudze Azure Load Balancer, dzięki czemu możesz nawiązywać połączenie z aplikacjami uruchomionymi w klastrze.  Wprowadź **80, 8172**, aby otworzyć porty 80 i 8172.

    Nie zaznaczaj pola wyboru **Skonfiguruj ustawienia zaawansowane**. Opcji tej używa się do dostosowywania punktów końcowych zarządzania protokołem TCP/HTTP, zakresem portów aplikacji, [ograniczeniami dotyczącymi umieszczania](service-fabric-cluster-resource-manager-configure-services.md#placement-constraints) oraz [właściwościami pojemności](service-fabric-cluster-resource-manager-metrics.md).    
    
    ![Zrzut ekranu przedstawiający konfigurację typu węzła][node-type-config]

    Kliknij przycisk **OK**.

6. W formularzu **Konfigurowanie klastra** ustaw opcję **Diagnostyka** na wartość **Wł**. W ramach tego przewodnika Szybki start nie musisz wprowadzać żadnych właściwości [ustawień sieci szkieletowej](service-fabric-cluster-fabric-settings.md).  W pozycji **Wersja sieci szkieletowej** wybierz tryb uaktualniania **Automatyczny**, dzięki czemu firma Microsoft będzie automatycznie aktualizować wersję kodu sieci szkieletowej obsługującego klaster.  Ustaw tryb **Ręczny**, jeśli chcesz [wybrać obsługiwaną wersję](service-fabric-cluster-upgrade.md) do uaktualnienia.     

    Kliknij przycisk **OK**.

7. Uzupełnij formularz **Zabezpieczenia**. W ramach tego przewodnika Szybki start wybierz opcję **Niezabezpieczony**. Pamiętaj, że zwykle w przypadku obciążeń produkcyjnych należy tworzyć zabezpieczone klastry. Każdy może anonimowo połączyć się niezabezpieczonym klastrem i wykonywać na nim operacje zarządzania.  

   Usługa Service Fabric używa certyfikatów, aby zapewniać uwierzytelnianie i szyfrowanie w celu zabezpieczania różnych aspektów klastra i jego aplikacji. Aby uzyskać więcej informacji, zobacz temat [Service Fabric cluster security scenarios](service-fabric-cluster-security.md) (Scenariusze zabezpieczeń klastra usługi Service Fabric). Aby włączyć uwierzytelnianie użytkownika przy użyciu usługi Azure Active Directory lub skonfigurować certyfikaty dla zabezpieczeń aplikacji, zobacz temat [Create a cluster from a Resource Manager template](service-fabric-cluster-creation-via-arm.md) (Tworzenie klastra z szablonu usługi Resource Manager).

    Kliknij przycisk **OK**.

8. Przejrzyj podsumowanie. Jeśli chcesz pobrać szablon usługi Azure Resource Manager utworzony na podstawie wprowadzonych ustawień, wybierz opcję **Pobierz szablon i parametry**. Wybierz opcję **Utwórz**, aby utworzyć klaster.

    Możesz zobaczyć postępy tworzenia w powiadomieniach. (Kliknij ikonę „Dzwonka” w pobliżu paska stanu w prawym górnym rogu ekranu). Jeśli wybrano opcję **Przypnij do tablicy startowej** podczas tworzenia klastra, zobaczysz pozycję **Wdrażanie klastra usługi Service Fabric** przypiętą do tablicy **Start**.

### <a name="connect-to-the-cluster-by-using-powershell"></a>Nawiązywanie połączenia z klastrem przy użyciu programu PowerShell
Sprawdź, czy klaster działa, nawiązując połączenie przy użyciu programu PowerShell. Moduł Service Fabric programu PowerShell jest instalowany przy użyciu [Zestawu SDK usługi Service Fabric](service-fabric-get-started.md). Polecenie cmdlet [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) umożliwia ustanowienie połączenia z klastrem.   

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint quickstartcluster.westus2.cloudapp.azure.com:19000
```
Inne przykłady łączenia z klastrem można znaleźć w temacie [Nawiązywanie połączenia z zabezpieczonym klastrem](service-fabric-connect-to-secure-cluster.md). Po nawiązaniu połączenia z klastrem użyj polecenia cmdlet [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps), aby wyświetlić listę węzłów w klastrze oraz informacje o stanie każdego węzła. Element **HealthState** powinien mieć wartość *OK* dla każdego węzła.

```powershell
PS C:\Users\sfuser> Get-ServiceFabricNode |Format-Table

NodeDeactivationInfo NodeName     IpAddressOrFQDN NodeType  CodeVersion  ConfigVersion NodeStatus NodeUpTime NodeDownTime HealthState
-------------------- --------     --------------- --------  -----------  ------------- ---------- ---------- ------------ -----------
                     _nodetype1_2 10.0.0.6        nodetype1 5.7.198.9494 1                     Up 03:00:38   00:00:00              Ok
                     _nodetype1_1 10.0.0.5        nodetype1 5.7.198.9494 1                     Up 03:00:38   00:00:00              Ok
                     _nodetype1_0 10.0.0.4        nodetype1 5.7.198.9494 1                     Up 03:00:38   00:00:00              Ok
                     _nodetype1_4 10.0.0.8        nodetype1 5.7.198.9494 1                     Up 03:00:38   00:00:00              Ok
                     _nodetype1_3 10.0.0.7        nodetype1 5.7.198.9494 1                     Up 03:00:38   00:00:00              Ok
```

### <a name="remove-the-cluster"></a>Usuwanie klastra
Klaster usługi Service Fabric składa się z innych zasobów platformy Azure poza samym zasobem klastra. Aby całkowicie usunąć klaster usługi Service Fabric, musisz również usunąć wszystkie zasoby, z których się składa. Najprostszym sposobem na usunięcie klastra i wszystkich wykorzystywanych przez niego zasobów jest usunięcie grupy zasobów. Aby uzyskać informacje o innych sposobach usunięcia klastra lub usunięcia części (nie wszystkich) zasobów w grupie zasobów, zobacz temat [Usuwanie klastra](service-fabric-cluster-delete.md).

Usuń grupę zasobów w witrynie Azure Portal:
1. Przejdź do klastra usługi Service Fabric, który chcesz usunąć.
2. Na stronie podstawowych elementów klastra wybierz nazwę **Grupy zasobów**.
3. Na stronie **Podstawowe elementy grupy zasobów** wybierz pozycję **Usuń grupę zasobów**. Następnie wykonaj instrukcje na tej stronie, aby zakończyć usuwanie grupy zasobów.
    ![Zrzut ekranu przedstawiający stronę Podstawowe elementy grupy zasobów z wyróżnionym przyciskiem Usuń grupę zasobów][cluster-delete]


## <a name="use-azure-powershell"></a>Korzystanie z programu Azure PowerShell
Innym sposobem na utworzenie klastra jest użycie programu PowerShell. Oto kroki tej procedury:

1. Pobierz na komputer [moduł Azure PowerShell w wersji 4.0 lub nowszej](https://docs.microsoft.com/powershell/azure/install-azurerm-ps).

2. Uruchom polecenie cmdlet [New-AzureRmServiceFabricCluster](/powershell/module/azurerm.servicefabric/new-azurermservicefabriccluster), aby utworzyć klaster usługi Service Fabric z pięciami węzłami zabezpieczony przy użyciu certyfikatu X.509. Polecenie tworzy certyfikat z podpisem własnym i przekazuje go do nowego magazynu kluczy. Certyfikat jest też kopiowany do katalogu lokalnego. Ustaw parametr *-OS*, aby wybrać wersję systemu Windows lub Linux działającego w węzłach klastra. Dostosuj parametry zgodnie z potrzebami. 

    ```powershell
    #Provide the subscription Id
    $subscriptionId = 'yourSubscriptionId'

    # Certificate variables.
    $certpwd="Password#1234" | ConvertTo-SecureString -AsPlainText -Force
    $certfolder="c:\mycertificates\"

    # Variables for VM admin.
    $adminuser="vmadmin"
    $adminpwd="Password#1234" | ConvertTo-SecureString -AsPlainText -Force 

    # Variables for common values
    $clusterloc="SouthCentralUS"
    $clustername = "mysfcluster"
    $groupname="mysfclustergroup"       
    $vmsku = "Standard_D2_v2"
    $vaultname = "mykeyvault"
    $subname="$clustername.$clusterloc.cloudapp.azure.com"

    # Set the number of cluster nodes. Possible values: 1, 3-99
    $clustersize=5 

    # Set the context to the subscription ID where the cluster will be created
    Login-AzureRmAccount
    Get-AzureRmSubscription
    Select-AzureRmSubscription -SubscriptionId $subscriptionId

    # Create the Service Fabric cluster.
    New-AzureRmServiceFabricCluster -Name $clustername -ResourceGroupName $groupname -Location $clusterloc `
    -ClusterSize $clustersize -VmUserName $adminuser -VmPassword $adminpwd -CertificateSubjectName $subname `
    -CertificatePassword $certpwd -CertificateOutputFolder $certfolder `
    -OS WindowsServer2016DatacenterwithContainers -VmSku $vmsku -KeyVaultName $vaultname
    ```

    Ukończenie polecenia może potrwać od 10 do 30 minut. Dane wyjściowe zawierają informacje dotyczące certyfikatu, magazynu kluczy, do którego został przekazany certyfikat, i folderu lokalnego, do którego został skopiowany.     

3. Skopiuj wszystkie dane wyjściowe i zapisz je w pliku tekstowym (będą potrzebne później). Zanotuj następujące informacje z danych wyjściowych: 

    - CertificateSavedLocalPath
    - CertificateThumbprint
    - ManagementEndpoint
    - ClientConnectionEndpointPort

### <a name="install-the-certificate-on-your-local-machine"></a>Instalowanie certyfikatu na komputerze lokalnym
  
Aby połączyć się z klastrem, zainstaluj certyfikat w magazynie osobistym bieżącego użytkownika. 

Uruchom następujące polecenie:

```powershell
$certpwd="Password#1234" | ConvertTo-SecureString -AsPlainText -Force
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My `
        -FilePath C:\mycertificates\<certificatename>.pfx `
        -Password $certpwd
```

Teraz możesz przystąpić do nawiązywania połączenia z zabezpieczonym klastrem.

### <a name="connect-to-a-secure-cluster"></a>Nawiązywanie połączenia z zabezpieczonym klastrem 

Uruchom polecenie cmdlet [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster), aby nawiązać połączenie z bezpiecznym klastrem. Szczegóły certyfikatu muszą być zgodne z certyfikatem, który został użyty do skonfigurowania klastra. 

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <ManagementEndpoint>:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint <CertificateThumbprint> `
          -FindType FindByThumbprint -FindValue <CertificateThumbprint> `
          -StoreLocation CurrentUser -StoreName My
```

Poniżej pokazano przykładowe parametry: 

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint mycluster.southcentralus.cloudapp.azure.com:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -FindType FindByThumbprint -FindValue C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -StoreLocation CurrentUser -StoreName My
```

Uruchom następujące polecenie, aby sprawdzić poprawność połączenia i upewnić się, że klaster jest w dobrej kondycji.

```powershell

Get-ServiceFabricClusterHealth

```
### <a name="remove-the-cluster"></a>Usuwanie klastra
Klaster składa się z innych zasobów platformy Azure poza samym zasobem klastra. Najprostszym sposobem na usunięcie klastra i wszystkich wykorzystywanych przez niego zasobów jest usunięcie grupy zasobów. 

```powershell
$groupname="mysfclustergroup"
Remove-AzureRmResourceGroup -Name $groupname -Force
```
## <a name="use-azure-cli"></a>Interfejs wiersza polecenia platformy Azure
Innym sposobem na utworzenie klastra jest użycie interfejsu wiersza polecenia. Oto kroki tej procedury:

1. Zainstaluj [interfejs wiersza polecenia platformy Azure 2.0](/cli/azure/install-azure-cli?view=azure-cli-latest) na komputerze.
2. Zaloguj się do platformy Azure i wybierz subskrypcję, w ramach której chcesz utworzyć klaster.
   ```azurecli
   az login
   az account set --subscription <GUID>
   ```
3. Uruchom polecenie [az sf cluster create](/cli/azure/sf/cluster?view=azure-cli-latest#az_sf_cluster_create), aby utworzyć klaster usługi Service Fabric z pięcioma węzłami zabezpieczony przy użyciu certyfikatu X.509. Polecenie tworzy certyfikat z podpisem własnym i przekazuje go do nowego magazynu kluczy. Certyfikat jest też kopiowany do katalogu lokalnego. Ustaw parametr *-os*, aby wybrać wersję systemu Windows lub Linux działającego w węzłach klastra. Dostosuj parametry zgodnie z potrzebami.

    ```azurecli
    #!/bin/bash

    # Variables
    ResourceGroupName="aztestclustergroup" 
    ClusterName="aztestcluster" 
    Location="southcentralus" 
    Password="q6D7nN%6ck@6" 
    Subject="aztestcluster.southcentralus.cloudapp.azure.com" 
    VaultName="aztestkeyvault" 
    VaultGroupName="testvaultgroup"
    VmPassword="Mypa$$word!321"
    VmUserName="sfadminuser"

    # Create resource groups
    az group create --name $ResourceGroupName --location $Location 
    az group create --name $VaultGroupName --location $Location

    # Create secure five node Linux cluster. Creates a key vault in a resource group
    # and creates a certficate in the key vault. The certificate's subject name must match 
    # the domain that you use to access the Service Fabric cluster. The certificate is downloaded locally.
    az sf cluster create --resource-group $ResourceGroupName --location $Location --certificate-output-folder . \
        --certificate-password $Password --certificate-subject-name $Subject --cluster-name $ClusterName \
        --cluster-size 5 --os UbuntuServer1604 --vault-name $VaultName --vault-resource-group $VaultGroupName \
        --vm-password $VmPassword --vm-user-name $VmUserName
    ```
    
### <a name="connect-to-the-cluster"></a>Łączenie z klastrem
Uruchom poniższe polecenie interfejsu wiersza polecenia, aby połączyć się z klastrem przy użyciu certyfikatu.  Podczas uwierzytelniania przy użyciu certyfikatu klienta upewnij się, że szczegóły certyfikatu są zgodne z certyfikatem wdrożonym w węzłach klastra. Użyj opcji `--no-verify` w przypadku certyfikatu z podpisem własnym.

```azurecli
az sf cluster select --endpoint https://aztestcluster.southcentralus.cloudapp.azure.com:19080 --pem ./linuxcluster201709161647.pem --no-verify
```

Uruchom następujące polecenie, aby sprawdzić poprawność połączenia i upewnić się, że klaster jest w dobrej kondycji.

```azurecli
az sf cluster health
```

### <a name="connect-to-the-nodes-directly"></a>Bezpośrednie łączenie z węzłami 

Aby połączyć się z węzłami w klastrze systemu Linux, możesz użyć polecenia SSH. Określ numer portu o wartości 3389 lub większej. Na przykład w przypadku pięciu utworzonych wcześniej węzłów klastra polecenia będą następujące:
```bash
ssh sfadminuser@aztestcluster.southcentralus.cloudapp.azure.com -p 3389
ssh sfadminuser@aztestcluster.southcentralus.cloudapp.azure.com -p 3390
ssh sfadminuser@aztestcluster.southcentralus.cloudapp.azure.com -p 3391
ssh sfadminuser@aztestcluster.southcentralus.cloudapp.azure.com -p 3392
ssh sfadminuser@aztestcluster.southcentralus.cloudapp.azure.com -p 3393
```

### <a name="remove-the-cluster"></a>Usuwanie klastra
Klaster składa się z innych zasobów platformy Azure poza samym zasobem klastra. Najprostszym sposobem na usunięcie klastra i wszystkich wykorzystywanych przez niego zasobów jest usunięcie grupy zasobów. 

```azurecli
ResourceGroupName = "aztestclustergroup"
az group delete --name $ResourceGroupName
```

## <a name="next-steps"></a>Następne kroki
Teraz po skonfigurowaniu klastra programowania możesz spróbować wykonać następujące czynności:
* [Wizualizowanie klastra przy użyciu narzędzia Service Fabric Explorer](service-fabric-visualizing-your-cluster.md)
* [Usuwanie klastra](service-fabric-cluster-delete.md) 
* [Wdrażanie aplikacji przy użyciu programu PowerShell](service-fabric-deploy-remove-applications.md)
* [Wdrażanie aplikacji przy użyciu interfejsu wiersza polecenia](service-fabric-application-lifecycle-sfctl.md)


[cluster-setup-basics]: ./media/service-fabric-get-started-azure-cluster/basics.png
[node-type-config]: ./media/service-fabric-get-started-azure-cluster/nodetypeconfig.png
[cluster-status]: ./media/service-fabric-get-started-azure-cluster/clusterstatus.png
[service-fabric-explorer]: ./media/service-fabric-get-started-azure-cluster/sfx.png
[cluster-delete]: ./media/service-fabric-get-started-azure-cluster/delete.png
