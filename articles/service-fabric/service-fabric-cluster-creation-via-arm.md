---
title: "Tworzenie klastra usługi sieć szkieletowa usług Azure na podstawie szablonu | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano, jak skonfigurować bezpieczny klastra sieci szkieletowej usług na platformie Azure przy użyciu usługi Azure Resource Manager, magazyn kluczy Azure i usługi Azure Active Directory (Azure AD) do uwierzytelniania klientów."
services: service-fabric
documentationcenter: .net
author: chackdan
manager: timlt
editor: chackdan
ms.assetid: 15d0ab67-fc66-4108-8038-3584eeebabaa
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/22/2017
ms.author: chackdan
ms.openlocfilehash: 47152d05eb7e31e7fe1f35e33a10fe8e903e21e2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-service-fabric-cluster-by-using-azure-resource-manager"></a>Tworzenie klastra sieci szkieletowej usług za pomocą usługi Azure Resource Manager
> [!div class="op_single_selector"]
> * [Azure Resource Manager](service-fabric-cluster-creation-via-arm.md)
> * [Witryna Azure Portal](service-fabric-cluster-creation-via-portal.md)
>
>

Ten przewodnik krok po kroku przeprowadzi Cię przez proces konfigurowania bezpiecznej klastra usługi sieć szkieletowa usług Azure na platformie Azure przy użyciu usługi Azure Resource Manager. Potwierdzam, że artykuł jest długa. Niemniej jednak jeśli nie masz już dokładnie zapoznać się z zawartości, należy dokładnie wykonaj wszystkie kroki.

Przewodnik obejmuje następujące procedury:

* Konfigurowanie usługi Azure key vault Przekaż certyfikaty zabezpieczeń klastra i aplikacji
* Tworzenie klastra zabezpieczonych na platformie Azure przy użyciu usługi Azure Resource Manager
* Uwierzytelnianie użytkowników przy użyciu usługi Azure Active Directory (Azure AD) do zarządzania klastrem

Bezpieczne klastra jest klastra, który uniemożliwia nieautoryzowany dostęp do operacji zarządzania. W tym wdrażanie, uaktualnianie i usuwanie aplikacji, usług i danych, które zawierają. Niezabezpieczone klastra oznacza, że każdy użytkownik w dowolnym momencie połączyć się i wykonywać operacje zarządzania. Chociaż można utworzyć klaster niezabezpieczone, zdecydowanie zaleca się utworzyć klaster bezpieczne od samego początku. Ponieważ niezabezpieczone klastra nie można zabezpieczyć później, należy utworzyć nowy klaster.

Pojęcie tworzenia bezpiecznych klastrów jest taki sam, czy są one Linux lub klastry z systemem Windows. Aby uzyskać więcej informacji i pomocnika skryptów tworzenia bezpiecznych klastrów systemu Linux, zobacz [tworzenia bezpiecznych klastrów w systemie Linux](#secure-linux-clusters).

## <a name="sign-in-to-your-azure-account"></a>Zaloguj się do swojego konta platformy Azure
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

## <a name="set-up-a-key-vault"></a>Konfigurowanie magazynu kluczy
W tej sekcji omówiono tworzenie magazynu kluczy dla klastra sieci szkieletowej usług platformy Azure i dla aplikacji sieci szkieletowej usług. Aby uzyskać pełne informacje do usługi Azure Key Vault, zapoznaj się [Key Vault Wprowadzenie — przewodnik][key-vault-get-started].

Sieć szkieletowa usług używa certyfikatów X.509 do zabezpieczania klastra oraz udostępnia funkcje zabezpieczeń aplikacji. Magazyn kluczy umożliwia zarządzanie certyfikatami dla klastrów sieci szkieletowej usług platformy Azure. Po wdrożeniu klastra w systemie Azure dostawcy zasobów platformy Azure, który jest odpowiedzialny za tworzenie klastrów usługi sieć szkieletowa ściąga certyfikaty z magazynu kluczy i instaluje je w klastrze maszyn wirtualnych.

Na poniższym diagramie przedstawiono relację między usługą Azure Key Vault, klaster sieci szkieletowej usług i dostawcy zasobów platformy Azure, który wykorzystuje certyfikaty przechowywane w magazynie kluczy, podczas tworzenia klastra:

![Diagram instalacji certyfikatu][cluster-security-cert-installation]

### <a name="create-a-resource-group"></a>Tworzenie grupy zasobów
Pierwszym krokiem jest aby utworzyć grupę zasobów, w szczególności dla magazynu kluczy. Firma Microsoft zaleca umieścić magazynu kluczy w jego własnej grupy zasobów. Ta akcja umożliwia usunięcie grupy zasobów obliczeniowych i przestrzeni dyskowej, wraz z grupy zasobów zawierającej klaster sieci szkieletowej usług, bez utraty z kluczy i kluczy tajnych. Grupy zasobów, która zawiera magazynu kluczy _musi znajdować się w tym samym regionie_ co klaster, który go używa.

Jeśli planujesz wdrożyć klastry w wielu regionach, zalecamy o nazwie grupy zasobów i klucz magazynu w sposób, który wskazuje regionu, do którego on należy.  

```powershell

    New-AzureRmResourceGroup -Name westus-mykeyvault -Location 'West US'
```
Dane wyjściowe powinny wyglądać następująco:

```powershell

    WARNING: The output object type of this cmdlet is going to be modified in a future release.

    ResourceGroupName : westus-mykeyvault
    Location          : westus
    ProvisioningState : Succeeded
    Tags              :
    ResourceId        : /subscriptions/<guid>/resourceGroups/westus-mykeyvault

```
<a id="new-key-vault"></a>

### <a name="create-a-key-vault-in-the-new-resource-group"></a>Tworzenie magazynu kluczy w nowej grupy zasobów
Magazyn kluczy _musi być włączona dla wdrożenia_ umożliwia dostawcy zasobów obliczeniowych uzyskanie certyfikatów i zainstalować go na wystąpieniu maszyny wirtualnej:

```powershell

    New-AzureRmKeyVault -VaultName 'mywestusvault' -ResourceGroupName 'westus-mykeyvault' -Location 'West US' -EnabledForDeployment

```

Dane wyjściowe powinny wyglądać następująco:

```powershell

    Vault Name                       : mywestusvault
    Resource Group Name              : westus-mykeyvault
    Location                         : West US
    Resource ID                      : /subscriptions/<guid>/resourceGroups/westus-mykeyvault/providers/Microsoft.KeyVault/vaults/mywestusvault
    Vault URI                        : https://mywestusvault.vault.azure.net
    Tenant ID                        : <guid>
    SKU                              : Standard
    Enabled For Deployment?          : False
    Enabled For Template Deployment? : False
    Enabled For Disk Encryption?     : False
    Access Policies                  :
                                       Tenant ID                :    <guid>
                                       Object ID                :    <guid>
                                       Application ID           :
                                       Display Name             :    
                                       Permissions to Keys      :    get, create, delete, list, update, import, backup, restore
                                       Permissions to Secrets   :    all


    Tags                             :
```
<a id="existing-key-vault"></a>

## <a name="use-an-existing-key-vault"></a>Użyj istniejącego magazynu kluczy

Aby użyć istniejącego magazynu kluczy, możesz _należy włączyć dla wdrożenia_ umożliwia dostawcy zasobów obliczeniowych uzyskanie certyfikatów i zainstalować go na węzłach klastra:

```powershell

Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -EnabledForDeployment

```

<a id="add-certificate-to-key-vault"></a>

## <a name="add-certificates-to-your-key-vault"></a>Dodaj certyfikaty do magazynu kluczy

W usłudze Service Fabric używa się certyfikatów, aby zapewniać uwierzytelnianie i szyfrowanie w celu zabezpieczania różnych aspektów klastra i jego aplikacji. Aby uzyskać więcej informacji dotyczących używania certyfikatów w sieci szkieletowej usług, zobacz [scenariusze zabezpieczeń klastra sieci szkieletowej usług][service-fabric-cluster-security].

### <a name="cluster-and-server-certificate-required"></a>Klaster a serwera certyfikatów (wymagane)
Ten certyfikat jest wymagany do zabezpieczania klastra i uniemożliwić nieautoryzowany dostęp do niego. Udostępnia ona zabezpieczeń klastra na dwa sposoby:

* Uwierzytelnianie klastra: uwierzytelnia komunikacji między węzłami w Federacji klastra. Tylko węzły, które można potwierdzić swoją tożsamość, z tym certyfikatem można dołączyć do klastra.
* Uwierzytelnianie serwera: służy do uwierzytelniania punktów końcowych klastra zarządzania do klienta zarządzania, aby klient zarządzania znała rozmawia rzeczywistych klastra. Ten certyfikat zapewnia również SSL interfejsu API zarządzania HTTPS oraz Service Fabric Explorer, za pośrednictwem protokołu HTTPS.

Do obsługi tych celów, certyfikat musi spełniać następujące wymagania:

* Certyfikat musi zawierać klucz prywatny.
* Certyfikat musi zostać utworzony dla wymiany kluczy, które można wyeksportować do pliku wymiany informacji osobistych (pfx).
* Nazwa podmiotu certyfikatu musi odpowiadać domeny, który umożliwia dostęp do klastra usługi sieć szkieletowa usług. To dopasowanie jest wymagane jest podanie protokołu SSL dla punktów końcowych HTTPS zarządzania i Service Fabric Explorer klastra. Nie można uzyskać certyfikat od urzędu certyfikacji (CA) dla. cloudapp.azure.com domeny. Należy uzyskać niestandardowej nazwy domeny dla klastra. Podczas żądania certyfikatu z urzędu certyfikacji, nazwa podmiotu certyfikatu musi odpowiadać nazwie domeny niestandardowej, używanego przez klaster.

### <a name="application-certificates-optional"></a>Certyfikaty aplikacji (opcjonalnie)
Dowolna liczba dodatkowych certyfikatów można zainstalować w klastrze dla aplikacji ze względów bezpieczeństwa. Przed utworzeniem klastra, należy wziąć pod uwagę scenariusze zabezpieczeń aplikacji, które wymagają certyfikatów do zainstalowania w węzłach, takich jak:

* Szyfrowanie i odszyfrowywanie wartości konfiguracji aplikacji.
* Szyfrowanie danych w węzłach podczas replikacji.

### <a name="formatting-certificates-for-azure-resource-provider-use"></a>Formatowanie certyfikatów do użytku dostawcy zasobów platformy Azure
Można dodać i używać plików kluczy prywatnych (pfx) bezpośrednio za pomocą magazynu kluczy. Dostawcy zasobów obliczeniowych Azure wymaga jednak przechowywać w formacie JavaScript Object Notation (JSON) specjalnych kluczy. Format zawiera plik PFX jako ciąg zakodowany w formacie 64 bazy i hasło klucza prywatnego. Aby uwzględnić te wymagania, klucze należy umieścić w ciągu JSON i następnie przechowywane jako "kluczy tajnych" w magazynie kluczy.

Aby ułatwić ten proces, [moduł programu PowerShell jest dostępny w witrynie GitHub][service-fabric-rp-helpers]. Aby użyć modułu programu, wykonaj następujące czynności:

1. Pobierz całą zawartość repozytorium do katalogu lokalnego.
2. Przejdź do katalogu lokalnego.
2. Zaimportuj moduł ServiceFabricRPHelpers w oknie programu PowerShell:

```powershell

 Import-Module "C:\..\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1"

```

`Invoke-AddCertToKeyVault` Polecenia w tym module środowiska PowerShell automatycznie formatuje klucz prywatny certyfikatu do ciągu JSON i przekazuje go do magazynu kluczy. Użyj polecenia, aby dodać certyfikat klastra i wszystkich certyfikatów dodatkową aplikację do magazynu kluczy. Powtórz ten krok dla dodatkowych certyfikatów, które chcesz zainstalować w klastrze.

#### <a name="uploading-an-existing-certificate"></a>Przekazywanie istniejącego certyfikatu

```powershell

 Invoke-AddCertToKeyVault -SubscriptionId <guid> -ResourceGroupName westus-mykeyvault -Location "West US" -VaultName mywestusvault -CertificateName mycert -Password "<password>" -UseExistingCertificate -ExistingPfxFilePath "C:\path\to\mycertkey.pfx"

```

Jeśli wystąpi błąd, takie jak pokazano poniżej, zwykle oznacza to, że występuje konflikt adresu URL zasobu. Aby rozwiązać konflikt, Zmień nazwę magazynu kluczy.

```
Set-AzureKeyVaultSecret : The remote name could not be resolved: 'westuskv.vault.azure.net'
At C:\Users\chackdan\Documents\GitHub\Service-Fabric\Scripts\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1:440 char:11
+ $secret = Set-AzureKeyVaultSecret -VaultName $VaultName -Name $Certif ...
+           ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : CloseError: (:) [Set-AzureKeyVaultSecret], WebException
    + FullyQualifiedErrorId : Microsoft.Azure.Commands.KeyVault.SetAzureKeyVaultSecret

```

Po konflikt został rozwiązany, dane wyjściowe powinny wyglądać następująco:

```

    Switching context to SubscriptionId <guid>
    Ensuring ResourceGroup westus-mykeyvault in West US
    WARNING: The output object type of this cmdlet is going to be modified in a future release.
    Using existing value mywestusvault in West US
    Reading pfx file from C:\path\to\key.pfx
    Writing secret to mywestusvault in vault mywestusvault


Name  : CertificateThumbprint
Value : E21DBC64B183B5BF355C34C46E03409FEEAEF58D

Name  : SourceVault
Value : /subscriptions/<guid>/resourceGroups/westus-mykeyvault/providers/Microsoft.KeyVault/vaults/mywestusvault

Name  : CertificateURL
Value : https://mywestusvault.vault.azure.net:443/secrets/mycert/4d087088df974e869f1c0978cb100e47

```

>[!NOTE]
>Należy trzech powyższych ciągów, CertificateThumbprint SourceVault i CertificateURL, Konfigurowanie bezpiecznego klastra sieci szkieletowej usług i uzyskanie wszystkich certyfikatów aplikacji, które mogą używać zabezpieczeń aplikacji. Jeśli nie zapiszesz ciągi, może być trudne pobierać je później badając magazynu kluczy.

<a id="add-self-signed-certificate-to-key-vault"></a>

#### <a name="creating-a-self-signed-certificate-and-uploading-it-to-the-key-vault"></a>Tworzenie certyfikatu z podpisem własnym i przekazać go do magazynu kluczy

Jeśli certyfikaty zostały już przekazane do magazynu kluczy, Pomiń ten krok. Ten krok jest generowania nowego certyfikatu z podpisem własnym i przekazać go do magazynu kluczy. Po zmiany parametrów w poniższym skrypcie, a następnie uruchom go, należy się monit o hasło certyfikatu.  

```powershell

$ResourceGroup = "chackowestuskv"
$VName = "chackokv2"
$SubID = "6c653126-e4ba-42cd-a1dd-f7bf96ae7a47"
$locationRegion = "westus"
$newCertName = "chackotestcertificate1"
$dnsName = "www.mycluster.westus.mydomain.com" #The certificate's subject name must match the domain used to access the Service Fabric cluster.
$localCertPath = "C:\MyCertificates" # location where you want the .PFX to be stored

 Invoke-AddCertToKeyVault -SubscriptionId $SubID -ResourceGroupName $ResourceGroup -Location $locationRegion -VaultName $VName -CertificateName $newCertName -CreateSelfSignedCertificate -DnsName $dnsName -OutputPath $localCertPath

```

Jeśli wystąpi błąd, takie jak pokazano poniżej, zwykle oznacza to, że występuje konflikt adresu URL zasobu. Aby rozwiązać konflikt, Zmień nazwę magazynu kluczy, nazwa grupy zasobów i tak dalej.

```
Set-AzureKeyVaultSecret : The remote name could not be resolved: 'westuskv.vault.azure.net'
At C:\Users\chackdan\Documents\GitHub\Service-Fabric\Scripts\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1:440 char:11
+ $secret = Set-AzureKeyVaultSecret -VaultName $VaultName -Name $Certif ...
+           ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : CloseError: (:) [Set-AzureKeyVaultSecret], WebException
    + FullyQualifiedErrorId : Microsoft.Azure.Commands.KeyVault.SetAzureKeyVaultSecret

```

Po konflikt został rozwiązany, dane wyjściowe powinny wyglądać następująco:

```
PS C:\Users\chackdan\Documents\GitHub\Service-Fabric\Scripts\ServiceFabricRPHelpers> Invoke-AddCertToKeyVault -SubscriptionId $SubID -ResourceGroupName $ResouceGroup -Location $locationRegion -VaultName $VName -CertificateName $newCertName -Password $certPassword -CreateSelfSignedCertificate -DnsName $dnsName -OutputPath $localCertPath
Switching context to SubscriptionId 6c343126-e4ba-52cd-a1dd-f8bf96ae7a47
Ensuring ResourceGroup chackowestuskv in westus
WARNING: The output object type of this cmdlet will be modified in a future release.
Creating new vault westuskv1 in westus
Creating new self signed certificate at C:\MyCertificates\chackonewcertificate1.pfx
Reading pfx file from C:\MyCertificates\chackonewcertificate1.pfx
Writing secret to chackonewcertificate1 in vault westuskv1


Name  : CertificateThumbprint
Value : 96BB3CC234F9D43C25D4B547sd8DE7B569F413EE

Name  : SourceVault
Value : /subscriptions/6c653126-e4ba-52cd-a1dd-f8bf96ae7a47/resourceGroups/chackowestuskv/providers/Microsoft.KeyVault/vaults/westuskv1

Name  : CertificateURL
Value : https://westuskv1.vault.azure.net:443/secrets/chackonewcertificate1/ee247291e45d405b8c8bbf81782d12bd

```

>[!NOTE]
>Należy trzech powyższych ciągów, CertificateThumbprint SourceVault i CertificateURL, Konfigurowanie bezpiecznego klastra sieci szkieletowej usług i uzyskanie wszystkich certyfikatów aplikacji, które mogą używać zabezpieczeń aplikacji. Jeśli nie zapiszesz ciągi, może być trudne pobierać je później badając magazynu kluczy.

 W tym momencie w miejscu musisz mieć następujące elementy:

* Grupa zasobów magazynu kluczy.
* Magazyn kluczy oraz adres URL (o nazwie SourceVault w poprzednim dane wyjściowe programu PowerShell).
* Certyfikat uwierzytelniania serwera klastra i jego adres URL w magazynie kluczy.
* Certyfikaty aplikacji i ich adresy URL w magazynie kluczy.


<a id="add-AAD-for-client"></a>

## <a name="set-up-azure-active-directory-for-client-authentication"></a>Konfigurowanie usługi Azure Active Directory do uwierzytelniania klientów

Usługi Azure AD umożliwia organizacjom (nazywane dzierżawcy), aby zarządzać dostępem użytkowników do aplikacji. Aplikacje są podzielone na tych z opartą na sieci web interfejsu użytkownika logowania i te przy użyciu środowiska macierzystego klienta. W tym artykule przyjęto założenie, że utworzono już dzierżawcy. Jeśli nie masz, Rozpocznij od przeczytania [jak uzyskać dzierżawę usługi Azure Active Directory][active-directory-howto-tenant].

Klaster sieci szkieletowej usług oferuje kilka punktów wejścia do jego funkcjonalność zarządzania, w tym oparta na sieci web [Service Fabric Explorer] [ service-fabric-visualizing-your-cluster] i [programu Visual Studio] [ service-fabric-manage-application-in-visual-studio]. W związku z tym tworzone są dwie aplikacje usługi Azure AD do kontrolowania dostępu do klastra, jednej aplikacji sieci web i jednej aplikacji natywnej.

Aby uprościć niektóre etapy konfigurowania usługi Azure AD z klastrem usługi sieć szkieletowa, został utworzony zestaw skryptów programu Windows PowerShell.

> [!NOTE]
> Przed utworzeniem klastra, wykonaj następujące kroki. Ponieważ skrypty oczekuje nazwy klastra i punktów końcowych, wartości powinny zostać zaplanowane, a nie wartości, które zostało już utworzone.

1. [Pobierz skrypty] [ sf-aad-ps-script-download] do komputera.
2. Kliknij prawym przyciskiem myszy plik zip, wybierz **właściwości**, wybierz pozycję **Odblokuj** pole wyboru, a następnie kliknij przycisk **Zastosuj**.
3. Wyodrębnij plik ZIP.
4. Uruchom `SetupApplications.ps1`i podaj TenantId ClusterName i WebApplicationReplyUrl jako parametry. Na przykład:

    ```powershell
    .\SetupApplications.ps1 -TenantId '690ec069-8200-4068-9d01-5aaf188e557a' -ClusterName 'mycluster' -WebApplicationReplyUrl 'https://mycluster.westus.cloudapp.azure.com:19080/Explorer/index.html'
    ```

    Wykonując polecenie programu PowerShell można znaleźć Twojego identyfikatora dzierżawcy `Get-AzureSubscription`. Wykonywanie to polecenie wyświetla TenantId dla każdej subskrypcji.

    ClusterName jest używana do prefiksu aplikacji usługi Azure AD, które są tworzone przez skrypt. Nie musi dokładnie odpowiadać nazwie rzeczywiste klastra. Ma ona tylko ułatwiają mapy usługi Azure AD artefakty do klastra usługi sieć szkieletowa, który jest używany z.

    WebApplicationReplyUrl jest domyślny punkt końcowy zwracające usługi Azure AD dla użytkowników po kończyły się zalogować. Ustaw ten punkt końcowy jako punkt końcowy Service Fabric Explorer dla klastra, która domyślnie jest:

    https://&lt;cluster_domain&gt;: 19080/Explorer

    Zostanie wyświetlony monit Zaloguj się do konta z uprawnieniami administratora dla dzierżawy usługi Azure AD. Po zalogowaniu, skrypt tworzy sieć web i natywnych aplikacji do reprezentowania klastra sieci szkieletowej usług. Jeśli przyjrzymy się aplikacji dzierżawcy w [klasycznego portalu Azure][azure-classic-portal], powinny pojawić się dwa nowe wpisy:

   * *ClusterName*\_klastra
   * *ClusterName*\_klienta

   Skrypt drukuje JSON wymagane przez szablon Menedżera zasobów Azure, podczas tworzenia klastra w następnej sekcji, dlatego warto nie zamykaj okna programu PowerShell.

```json
"azureActiveDirectory": {
  "tenantId":"<guid>",
  "clusterApplication":"<guid>",
  "clientApplication":"<guid>"
},
```

## <a name="create-a-service-fabric-cluster-resource-manager-template"></a>Utwórz szablon Menedżera zasobów klastra sieci szkieletowej usług
W tej sekcji dane wyjściowe poprzedniego polecenia programu PowerShell są używane w szablonie usługi Resource Manager klastra sieci szkieletowej usług.

Menedżer zasobów przykładowe szablony są dostępne w [galerii szablonów szybki start Azure w serwisie GitHub][azure-quickstart-templates]. Te szablony może służyć jako punkt początkowy dla szablonu klastra.

### <a name="create-the-resource-manager-template"></a>Tworzenie szablonu usługi Resource Manager
W tym przewodniku używa [5 węzłów klastra bezpiecznego] [ service-fabric-secure-cluster-5-node-1-nodetype] przykładowy szablon i parametrów szablonu. Pobierz `azuredeploy.json` i `azuredeploy.parameters.json` do komputera, a następnie otwórz oba pliki w edytorze tekstu.

### <a name="add-certificates"></a>Dodaj certyfikaty
Dodaj certyfikaty do szablonu usługi Resource Manager klastra odbywa się za pomocą odwołań do magazynu kluczy, która zawiera klucze certyfikatu. Zaleca się umieszczenie wartości magazynu kluczy w pliku parametrów szablonu usługi Resource Manager. W ten sposób przechowuje Menedżera zasobów pliku szablonu może być ponownie używane i wolne od wartości określonych do wdrożenia.

#### <a name="add-all-certificates-to-the-virtual-machine-scale-set-osprofile"></a>Dodaj wszystkie certyfikaty do osProfile zestaw skali maszyny wirtualnej
Każdy certyfikat, który jest zainstalowany w klastrze muszą być skonfigurowane w sekcji osProfile zasób zestawu skali (Microsoft.Compute/virtualMachineScaleSets). Ta akcja powoduje, że dostawca zasobów, aby zainstalować certyfikat na maszynach wirtualnych. Ta instalacja obejmuje zarówno certyfikat klastra, jak i wszelkich certyfikatów zabezpieczeń aplikacji, które będą używane dla aplikacji:

```json
{
  "apiVersion": "2016-03-30",
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  ...
  "properties": {
    ...
    "osProfile": {
      ...
      "secrets": [
        {
          "sourceVault": {
            "id": "[parameters('sourceVaultValue')]"
          },
          "vaultCertificates": [
            {
              "certificateStore": "[parameters('clusterCertificateStorevalue')]",
              "certificateUrl": "[parameters('clusterCertificateUrlValue')]"
            },
            {
              "certificateStore": "[parameters('applicationCertificateStorevalue')",
              "certificateUrl": "[parameters('applicationCertificateUrlValue')]"
            },
            ...
          ]
        }
      ]
    }
  }
}
```

#### <a name="configure-the-service-fabric-cluster-certificate"></a>Konfigurowanie certyfikatu klastra sieci szkieletowej usług
Certyfikat uwierzytelniania klastra muszą być skonfigurowane w obu sieci szkieletowej usług zasobu klastra (Microsoft.ServiceFabric/clusters) i rozszerzenia skalowania maszyny wirtualnej dla sieci szkieletowej usług ustawia w zasobie zestaw skali maszyny wirtualnej. To rozmieszczenie umożliwia jest skonfigurowana do użycia uwierzytelniania klastra i uwierzytelniania serwera dla punktów końcowych zarządzania dostawcy zasobów sieci szkieletowej usług.

##### <a name="virtual-machine-scale-set-resource"></a>Zasób zestawu skalowania maszyn wirtualnych:
```json
{
  "apiVersion": "2016-03-30",
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  ...
  "properties": {
    ...
    "virtualMachineProfile": {
      "extensionProfile": {
        "extensions": [
          {
            "name": "[concat('ServiceFabricNodeVmExt','_vmNodeType0Name')]",
            "properties": {
              ...
              "settings": {
                ...
                "certificate": {
                  "thumbprint": "[parameters('clusterCertificateThumbprint')]",
                  "x509StoreName": "[parameters('clusterCertificateStoreValue')]"
                },
                ...
              }
            }
          }
        ]
      }
    }
  }
}
```

##### <a name="service-fabric-resource"></a>Zasobów sieci szkieletowej usług:
```json
{
  "apiVersion": "2016-03-01",
  "type": "Microsoft.ServiceFabric/clusters",
  "name": "[parameters('clusterName')]",
  "location": "[parameters('clusterLocation')]",
  "dependsOn": [
    "[concat('Microsoft.Storage/storageAccounts/', variables('supportLogStorageAccountName'))]"
  ],
  "properties": {
    "certificate": {
      "thumbprint": "[parameters('clusterCertificateThumbprint')]",
      "x509StoreName": "[parameters('clusterCertificateStoreValue')]"
    },
    ...
  }
}
```

### <a name="insert-azure-ad-configuration"></a>Wstaw konfiguracji usługi Azure AD
Konfiguracja usługi Azure AD, utworzony wcześniej mogą być wstawiane bezpośrednio do szablonu usługi Resource Manager. Jednak zaleca się najpierw wyodrębnić wartości w pliku parametrów, aby zachować Menedżera zasobów szablonu do ponownego użycia i wolne od wartości określonej dla wdrożenia.

```json
{
  "apiVersion": "2016-03-01",
  "type": "Microsoft.ServiceFabric/clusters",
  "name": "[parameters('clusterName')]",
  ...
  "properties": {
    "certificate": {
      "thumbprint": "[parameters('clusterCertificateThumbprint')]",
      "x509StoreName": "[parameters('clusterCertificateStorevalue')]"
    },
    ...
    "azureActiveDirectory": {
      "tenantId": "[parameters('aadTenantId')]",
      "clusterApplication": "[parameters('aadClusterApplicationId')]",
      "clientApplication": "[parameters('aadClientApplicationId')]"
    },
    ...
  }
}
```

### < "Konfigurowanie ramię" ></a>parametry szablonu Konfigurowanie Menedżera zasobów
<!--- Loc Comment: It seems that <a "configure-arm" > must be replaced with <a name="configure-arm"></a> since the link seems not to be redirecting correctly --->
Na koniec użyj wartości danych wyjściowych z magazynu kluczy i poleceń programu PowerShell usługi Azure AD do wypełnienia pliku parametrów:

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        ...
        "clusterCertificateStoreValue": {
            "value": "My"
        },
        "clusterCertificateThumbprint": {
            "value": "<thumbprint>"
        },
        "clusterCertificateUrlValue": {
            "value": "https://myvault.vault.azure.net:443/secrets/myclustercert/4d087088df974e869f1c0978cb100e47"
        },
        "applicationCertificateStorevalue": {
            "value": "My"
        },
        "applicationCertificateUrlValue": {
            "value": "https://myvault.vault.azure.net:443/secrets/myapplicationcert/2e035058ae274f869c4d0348ca100f08"
        },
        "sourceVaultvalue": {
            "value": "/subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault"
        },
        "aadTenantId": {
            "value": "<guid>"
        },
        "aadClusterApplicationId": {
            "value": "<guid>"
        },
        "aadClientApplicationId": {
            "value": "<guid>"
        },
        ...
    }
}
```
W tym momencie w miejscu musisz mieć następujące elementy:

* Grupa zasobów magazynu kluczy
  * Magazyn kluczy
  * Certyfikat uwierzytelniania serwera klastra
  * Certyfikat Szyfrowanie danych
* Dzierżawy usługi Azure Active Directory
  * Aplikacji usługi Azure AD dla zarządzania opartego na sieci web i Service Fabric Explorer
  * Aplikacji do zarządzania klientami z usługi Azure AD
  * Użytkownicy i przypisane im role
* Szablon usługi Resource Manager klastra sieci szkieletowej usług
  * Skonfigurowanych za pomocą magazynu kluczy certyfikatów
  * Usługa Azure Active Directory skonfigurowane

Na poniższym diagramie przedstawiono, w którym magazyn kluczy i konfiguracji usługi Azure AD mieści się w szablonie usługi Resource Manager.

![Menedżer zasobów zależności mapy][cluster-security-arm-dependency-map]

## <a name="create-the-cluster"></a>Tworzenie klastra
Teraz można przystąpić do tworzenia klastra przy użyciu [wdrażania szablonu zasobów platformy Azure][resource-group-template-deploy].

#### <a name="test-it"></a>należy przeprowadzić test
Do przetestowania pliku parametrów szablonu usługi Resource Manager, należy użyć następującego polecenia programu PowerShell:

```powershell
Test-AzureRmResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json
```

#### <a name="deploy-it"></a>Wdróż go
Jeśli test szablonu usługi Resource Manager zakończy się pomyślnie, użyj następującego polecenia programu PowerShell do wdrażania szablonu usługi Resource Manager przy użyciu pliku parametrów:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json
```

<a name="assign-roles"></a>

## <a name="assign-users-to-roles"></a>Przypisywanie użytkowników do ról
Po utworzeniu aplikacji do reprezentowania klastra przypisać użytkowników do ról obsługiwane przez usługi Service Fabric: tylko do odczytu i administratora. Role można przypisać za pomocą [klasycznego portalu Azure][azure-classic-portal].

1. W portalu Azure, przejdź do swojej dzierżawy, a następnie wybierz **aplikacji**.
2. Wybierz aplikację sieci web, która zawiera nazwę jak `myTestCluster_Cluster`.
3. Kliknij przycisk **użytkowników** kartę.
4. Wybierz użytkownika do przypisywania, a następnie kliknij przycisk **przypisać** u dołu ekranu.

    ![Przypisywanie użytkowników do ról przycisku][assign-users-to-roles-button]
5. Wybierz rolę do przypisania do użytkownika.

    ![Okno dialogowe "Przypisywanie użytkowników"][assign-users-to-roles-dialog]

> [!NOTE]
> Aby uzyskać więcej informacji na temat ról w sieci szkieletowej usług, zobacz [kontroli dostępu opartej na rolach dla klientów usługi sieć szkieletowa](service-fabric-cluster-security-roles.md).
>
>

 <a name="secure-linux-clusters"></a>
 <!--- Loc Comment: It seems that letter S in cluster was missing, which caused the wrong redirection of the link --->

## <a name="create-secure-clusters-on-linux"></a>Tworzenie bezpiecznych klastrów w systemie Linux
Aby ułatwić proces, firma Microsoft umieściła [skryptu Pomocnika](http://github.com/ChackDan/Service-Fabric/tree/master/Scripts/CertUpload4Linux). Przed użyciem tego skryptu pomocnika, upewnij się, że masz już Azure interfejsu wiersza polecenia (CLI) zainstalowany i znajduje się w ścieżce. Upewnij się, że skrypt ma uprawnienia do wykonania, uruchamiając `chmod +x cert_helper.py` po ją pobrać. Pierwszym krokiem jest logowania do konta platformy Azure przy użyciu interfejsu wiersza polecenia z `azure login` polecenia. Po zalogowaniu się do konta platformy Azure, użyj skryptu pomocnika przy użyciu podpisanego certyfikatu urzędu certyfikacji, jak przedstawiono na poniższym polecenie:

```sh
./cert_helper.py [-h] CERT_TYPE [-ifile INPUT_CERT_FILE] [-sub SUBSCRIPTION_ID] [-rgname RESOURCE_GROUP_NAME] [-kv KEY_VAULT_NAME] [-sname CERTIFICATE_NAME] [-l LOCATION] [-p PASSWORD]
```

Parametr - ifile może zająć plik pfx lub plik PEM jako dane wejściowe typu certyfikatu (pfx lub pem lub ss, jeśli certyfikat z podpisem własnym).
Parametr -h drukowania tekst pomocy.


To polecenie zwraca następujące trzy ciągi jako dane wyjściowe:

* SourceVaultID, który jest wyświetlany identyfikator nowego grupa zasobów KeyVault on utworzony
* CertificateUrl do uzyskiwania dostępu do certyfikatu
* CertificateThumbprint, który jest używany do uwierzytelniania

Poniższy przykład przedstawia sposób użycia polecenia:

```sh
./cert_helper.py pfx -sub "fffffff-ffff-ffff-ffff-ffffffffffff"  -rgname "mykvrg" -kv "mykevname" -ifile "/home/test/cert.pfx" -sname "mycert" -l "East US" -p "pfxtest"
```
Poprzednie polecenie wykonywania oferuje trzy ciągi w następujący sposób:

```sh
SourceVault: /subscriptions/fffffff-ffff-ffff-ffff-ffffffffffff/resourceGroups/mykvrg/providers/Microsoft.KeyVault/vaults/mykvname
CertificateUrl: https://myvault.vault.azure.net/secrets/mycert/00000000000000000000000000000000
CertificateThumbprint: 0xfffffffffffffffffffffffffffffffffffffffff
```

Nazwa podmiotu certyfikatu musi odpowiadać domeny, który umożliwia dostęp do klastra usługi sieć szkieletowa usług. To dopasowanie jest wymagane jest podanie protokołu SSL dla punktów końcowych HTTPS zarządzania i Service Fabric Explorer klastra. Nie można uzyskać certyfikatu SSL z urzędu certyfikacji dla `.cloudapp.azure.com` domeny. Należy uzyskać niestandardowej nazwy domeny dla klastra. Podczas żądania certyfikatu z urzędu certyfikacji, nazwa podmiotu certyfikatu musi odpowiadać nazwie domeny niestandardowej, używanego przez klaster.

Te nazwy podmiotu są wpisów, należy utworzyć bezpiecznego klastra sieci szkieletowej usług (bez usługi Azure AD), zgodnie z opisem w [parametrów szablonu usługi Resource Manager skonfigurować](#configure-arm). Możesz nawiązać bezpiecznego klastra zgodnie z instrukcjami dla [uwierzytelniania dostępu klienta do klastra](service-fabric-connect-to-secure-cluster.md). Klastry z systemem Linux nie obsługują uwierzytelniania usługi Azure AD. Można przypisać role administratora, jak i klienta, zgodnie z opisem w [przypisać role do użytkowników](#assign-roles) sekcji. Po określeniu ról administratora, jak i klienta przez klaster systemu Linux, musisz podać odciski palców certyfikatów do uwierzytelniania. Nie zostanie określona nazwa podmiotu, ponieważ brak weryfikacji łańcucha lub odwołania jest wykonywana.

Jeśli chcesz użyć certyfikatu z podpisem własnym do testowania, służy sam skrypt go wygenerować. Certyfikat można następnie przekazać do magazynu kluczy, zapewniając flagę `ss` zamiast certyfikatu ścieżka i nazwa certyfikatu. Na przykład zobacz następujące polecenie dla tworzenie i przekazywanie certyfikatu z podpisem własnym:

```sh
./cert_helper.py ss -rgname "mykvrg" -sub "fffffff-ffff-ffff-ffff-ffffffffffff" -kv "mykevname"   -sname "mycert" -l "East US" -p "selftest" -subj "mytest.eastus.cloudapp.net"
```
To polecenie zwraca samej trzy ciągi: SourceVault, CertificateUrl i CertificateThumbprint. Ciągi można następnie użyć do tworzenia bezpiecznego klastra systemu Linux i z lokalizacji, w którym znajduje się certyfikat z podpisem własnym. Należy certyfikat z podpisem własnym można połączyć się z klastrem. Możesz nawiązać bezpiecznego klastra zgodnie z instrukcjami dla [uwierzytelniania dostępu klienta do klastra](service-fabric-connect-to-secure-cluster.md).

Nazwa podmiotu certyfikatu musi odpowiadać domeny, który umożliwia dostęp do klastra usługi sieć szkieletowa usług. To dopasowanie jest wymagane jest podanie protokołu SSL dla punktów końcowych HTTPS zarządzania i Service Fabric Explorer klastra. Nie można uzyskać certyfikatu SSL z urzędu certyfikacji dla `.cloudapp.azure.com` domeny. Należy uzyskać niestandardowej nazwy domeny dla klastra. Podczas żądania certyfikatu z urzędu certyfikacji, nazwa podmiotu certyfikatu musi odpowiadać nazwie domeny niestandardowej, używanego przez klaster.

Możesz wpisać parametry skryptu pomocnika w portalu Azure, zgodnie z opisem w [tworzenia klastra w portalu Azure](service-fabric-cluster-creation-via-portal.md#create-cluster-in-the-azure-portal) sekcji.

## <a name="next-steps"></a>Następne kroki
W tym momencie masz bezpiecznego klaster o udostępnienie uwierzytelniania zarządzania usługą Azure Active Directory. Następnie [połączyć się z klastrem](service-fabric-connect-to-secure-cluster.md) i Dowiedz się, jak [Zarządzanie klucze tajne aplikacji](service-fabric-application-secret-management.md).

## <a name="troubleshoot-setting-up-azure-active-directory-for-client-authentication"></a>Rozwiązywanie problemów z Konfigurowanie usługi Azure Active Directory do uwierzytelniania klientów
Jeśli napotkasz problem podczas podczas konfigurowania usługi Azure AD do uwierzytelniania klientów, przejrzyj potencjalne rozwiązania w tej sekcji.

### <a name="service-fabric-explorer-prompts-you-to-select-a-certificate"></a>Service Fabric Explorer wyświetla monit o wybranie certyfikatu
#### <a name="problem"></a>Problem
Po zalogowaniu pomyślnie do usługi Azure AD w narzędziu Service Fabric Explorer, zwraca przeglądarkę na stronę główną, ale monit o wybranie certyfikatu.

![Wybierz certyfikat SFX w oknie dialogowym][sfx-select-certificate-dialog]

#### <a name="reason"></a>Przyczyna
Użytkownik nie jest przypisana rola w aplikacji klastra usługi Azure AD. W związku z tym niepowodzenia uwierzytelniania usługi Azure AD w klastrze usługi sieć szkieletowa usług. Service Fabric Explorer powraca do uwierzytelniania certyfikatów.

#### <a name="solution"></a>Rozwiązanie
Instrukcje dotyczące konfigurowania usługi Azure AD i przypisz role użytkownika. Ponadto zalecamy włączenie "Przypisanie użytkownika wymagane na dostęp do aplikacji," jako `SetupApplications.ps1` jest.

### <a name="connection-with-powershell-fails-with-an-error-the-specified-credentials-are-invalid"></a>Połączenie przy użyciu programu PowerShell zakończy się niepowodzeniem z powodu błędu: "podane poświadczenia są nieprawidłowe"
#### <a name="problem"></a>Problem
Połącz się z klastrem przy użyciu trybu zabezpieczeń "Usługi AzureActiveDirectory", po zalogowaniu pomyślnie do usługi Azure AD przy użyciu programu PowerShell, połączenie nie powiedzie się z powodu błędu: "podane poświadczenia są nieprawidłowe."

#### <a name="solution"></a>Rozwiązanie
To rozwiązanie jest taka sama jak mieszanego.

### <a name="service-fabric-explorer-returns-a-failure-when-you-sign-in-aadsts50011"></a>Service Fabric Explorer zwróci błąd podczas logowania: "AADSTS50011"
#### <a name="problem"></a>Problem
Podczas próby Zaloguj się do usługi Azure AD w narzędziu Service Fabric Explorer strony zwraca błąd: "AADSTS50011: adres zwrotny &lt;adres url&gt; pasują do adresów odpowiedzi skonfigurowanych dla aplikacji: &lt;guid&gt;."

![Adres zwrotny SFX jest niezgodny.][sfx-reply-address-not-match]

#### <a name="reason"></a>Przyczyna
Aplikacja klastra (sieć web), która reprezentuje Service Fabric Explorer podejmuje próbę uwierzytelniania usługi Azure AD, a jako część żądania zapewnia zwrotny adres URL przekierowania. Adres URL nie jest wyświetlany w aplikacji usługi Azure AD, ale **adres URL odpowiedzi** listy.

#### <a name="solution"></a>Rozwiązanie
Na **Konfiguruj** kartę aplikacji klastra (sieć web), Dodaj adres URL z Eksploratora usługi sieć szkieletowa do **adres URL odpowiedzi** listy lub Zastąp jeden z elementów na liście. Po zakończeniu zapisz zmianę.

![Adres url odpowiedzi aplikacji sieci Web][web-application-reply-url]

### <a name="connect-the-cluster-by-using-azure-ad-authentication-via-powershell"></a>Połącz klaster przy użyciu uwierzytelniania usługi Azure AD za pomocą programu PowerShell
Połącz z klastrem usługi sieć szkieletowa, użyj następującego polecenia programu PowerShell:

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <endpoint> -KeepAliveIntervalInSec 10 -AzureActiveDirectory -ServerCertThumbprint <thumbprint>
```

Aby dowiedzieć się więcej na temat polecenia cmdlet Connect-ServiceFabricCluster, zobacz [Connect-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx).

### <a name="can-i-reuse-the-same-azure-ad-tenant-in-multiple-clusters"></a>Czy można ponownie użyć tej samej dzierżawy usługi Azure AD w wielu klastrach?
Tak. Należy jednak pamiętać dodać adres URL Eksploratora sieci szkieletowej usług do aplikacji klastra (sieć web). W przeciwnym razie Eksploratora usługi sieć szkieletowa nie działa.

### <a name="why-do-i-still-need-a-server-certificate-while-azure-ad-is-enabled"></a>Dlaczego nadal należy certyfikat serwera po włączeniu usługi Azure AD?
Klienta fabricclient z rolą i FabricGateway wykonać uwierzytelnianie wzajemne. Podczas uwierzytelniania usługi Azure AD integracji z usługą Azure AD zapewnia tożsamością klienta do serwera, a certyfikat serwera jest używany do weryfikacji tożsamości serwera. Aby uzyskać więcej informacji na temat sieci szkieletowej usług certyfikatów, zobacz [certyfikatów X.509 i sieci szkieletowej usług][x509-certificates-and-service-fabric].

<!-- Links -->
[azure-powershell]:https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[key-vault-get-started]:../key-vault/key-vault-get-started.md
[aad-graph-api-docs]:https://msdn.microsoft.com/library/azure/ad/graph/api/api-catalog
[azure-classic-portal]: https://manage.windowsazure.com
[service-fabric-rp-helpers]: https://github.com/ChackDan/Service-Fabric/tree/master/Scripts/ServiceFabricRPHelpers
[service-fabric-cluster-security]: service-fabric-cluster-security.md
[active-directory-howto-tenant]: ../active-directory/active-directory-howto-tenant.md
[service-fabric-visualizing-your-cluster]: service-fabric-visualizing-your-cluster.md
[service-fabric-manage-application-in-visual-studio]: service-fabric-manage-application-in-visual-studio.md
[sf-aad-ps-script-download]:http://servicefabricsdkstorage.blob.core.windows.net/publicrelease/MicrosoftAzureServiceFabric-AADHelpers.zip
[azure-quickstart-templates]: https://github.com/Azure/azure-quickstart-templates
[service-fabric-secure-cluster-5-node-1-nodetype]: https://github.com/Azure/azure-quickstart-templates/blob/master/service-fabric-secure-cluster-5-node-1-nodetype/
[resource-group-template-deploy]: https://azure.microsoft.com/documentation/articles/resource-group-template-deploy/
[x509-certificates-and-service-fabric]: service-fabric-cluster-security.md#x509-certificates-and-service-fabric

<!-- Images -->
[cluster-security-arm-dependency-map]: ./media/service-fabric-cluster-creation-via-arm/cluster-security-arm-dependency-map.png
[cluster-security-cert-installation]: ./media/service-fabric-cluster-creation-via-arm/cluster-security-cert-installation.png
[assign-users-to-roles-button]: ./media/service-fabric-cluster-creation-via-arm/assign-users-to-roles-button.png
[assign-users-to-roles-dialog]: ./media/service-fabric-cluster-creation-via-arm/assign-users-to-roles.png
[sfx-select-certificate-dialog]: ./media/service-fabric-cluster-creation-via-arm/sfx-select-certificate-dialog.png
[sfx-reply-address-not-match]: ./media/service-fabric-cluster-creation-via-arm/sfx-reply-address-not-match.png
[web-application-reply-url]: ./media/service-fabric-cluster-creation-via-arm/web-application-reply-url.png

