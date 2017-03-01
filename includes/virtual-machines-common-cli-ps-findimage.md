

## <a name="azure-cli-20-preview"></a>Azure CLI 2.0 (wersja zapoznawcza)

Po [zainstalowaniu interfejsu Azure CLI 2.0 (wersja zapoznawcza)](https://docs.microsoft.com/cli/azure/install-az-cli2) wyświetl listę popularnych obrazów maszyn wirtualnych przechowywaną w pamięci podręcznej za pomocą polecenia `az vm image list`. Na przykład polecenie `az vm image list -o table` powoduje wyświetlenie tego:

```
You are viewing an offline list of images, use --all to retrieve an up-to-date list
Offer          Publisher               Sku                 Urn                                                             UrnAlias             Version
-------------  ----------------------  ------------------  --------------------------------------------------------------  -------------------  ---------
WindowsServer  MicrosoftWindowsServer  2012-R2-Datacenter  MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest  Win2012R2Datacenter  latest
WindowsServer  MicrosoftWindowsServer  2008-R2-SP1         MicrosoftWindowsServer:WindowsServer:2008-R2-SP1:latest         Win2008R2SP1         latest
WindowsServer  MicrosoftWindowsServer  2012-Datacenter     MicrosoftWindowsServer:WindowsServer:2012-Datacenter:latest     Win2012Datacenter    latest
UbuntuServer   Canonical               14.04.4-LTS         Canonical:UbuntuServer:14.04.4-LTS:latest                       UbuntuLTS            latest
CentOS         OpenLogic               7.2                 OpenLogic:CentOS:7.2:latest                                     CentOS               latest
openSUSE       SUSE                    13.2                SUSE:openSUSE:13.2:latest                                       openSUSE             latest
RHEL           RedHat                  7.2                 RedHat:RHEL:7.2:latest                                          RHEL                 latest
SLES           SUSE                    12-SP1              SUSE:SLES:12-SP1:latest                                         SLES                 latest
Debian         credativ                8                   credativ:Debian:8:latest                                        Debian               latest
CoreOS         CoreOS                  Stable              CoreOS:CoreOS:Stable:latest                                     CoreOS               latest
```

### <a name="finding-all-current-images"></a>Znajdowanie wszystkich obrazów bieżących

Aby uzyskać bieżącą listę wszystkich obrazów, użyj polecenia `az vm image list` z opcją `--all`. Inaczej niż w przypadku poleceń interfejsu wiersza polecenia platformy Azure w wersji 1.0, polecenie `az vm image list --all` zwraca domyślnie wszystkie obrazy w lokalizacji **westus** (chyba że określi się konkretny argument `--location`), dlatego na wykonanie polecenia `--all` trzeba nieco poczekać. Jeśli zamierzasz zbadać te informacje interakcyjnie, użyj polecenia `az vm image list --all > allImages.json`, które zwraca listę wszystkich obrazów dostępnych na platformie Azure i zapisuje ją w postaci pliku do użytku lokalnego. 

Podając jedną lub kilka opcji, można ograniczyć wyszukiwanie do określonej lokalizacji, oferty, wydawcy lub wersji SKU, jeśli taka informacja jest znana. Jeśli nie określisz lokalizacji, zwracane są wartości dla lokalizacji **westus**.

### <a name="find-specific-images"></a>Znajdowanie określonych obrazów

Aby znaleźć konkretne informacje, użyj polecenia `az vm image list` z [filtrem zapytań JMESPATH](https://docs.microsoft.com/cli/azure/query-az-cli2). Na przykład następujące polecenie powoduje wyświetlenie wersji **SKU**, które są dostępne w ofercie **Debian** (bez przełącznika `--all` polecenie przeszukuje tylko lokalną pamięć podręczną obrazów wspólnych):

```azurecli
az vm image list --query '[?contains(offer,`Debian`)]' -o table --all
```

Dane wyjściowe wyglądają mniej więcej tak: 
```
You are viewing an offline list of images, use --all to retrieve an up-to-date list
  Sku  Publisher    Offer    Urn                       Version    UrnAlias
-----  -----------  -------  ------------------------  ---------  ----------
    8  credativ     Debian   credativ:Debian:8:latest  latest     Debian

<list shortened for the example>
```

Jeśli miejsce wdrażania jest znane, za pomocą ogólnych wyników wyszukiwania obrazów oraz poleceń `az vm image list-skus`, `az vm image list-offers` i `az vm image list-publishers` można znaleźć dokładnie to, co jest potrzebne, i ustalić, gdzie może zostać wdrożone. Na przykład jeśli z poprzedniego przykładu wiadomo, że wydawca `credativ` udostępnia ofertę Debian, można za pomocą przełącznika `--location` i innych opcji znaleźć dokładnie to, co jest potrzebne. Poniższej podano przykład polecenia wyszukującego obraz Debian 8 w lokalizacji **westeurope**:

```azurecli 
az vm image show -l westeurope -f debian -p credativ --skus 8 --version 8.0.201701180
```

Dane wyjściowe wyglądają tak:

```json
{
  "dataDiskImages": [],
  "id": "/Subscriptions/<guid>/Providers/Microsoft.Compute/Locations/westeurope/Publishers/credativ/ArtifactTypes/VMImage/Offers/debian/Skus/8/Versions/8.0.201701180",
  "location": "westeurope",
  "name": "8.0.201701180",
  "osDiskImage": {
    "operatingSystem": "Linux"
  },
  "plan": null,
  "tags": null
}
```

## <a name="azure-cli-10"></a>Interfejs wiersza polecenia platformy Azure CLI w wersji 1.0 

> [!NOTE]
> W tym artykule wyjaśniono, jak nawigować i wybierać obrazy maszyn wirtualnych przy użyciu instalacji interfejsu wiersza polecenia platformy Azure w wersji 1.0 lub programu Azure PowerShell obsługującego model wdrażania przy użyciu usługi Azure Resource Manager. Aby spełnić wymagania wstępne, włącz tryb usługi Resource Manager. W interfejsie Azure CLI włącz ten tryb, wpisując polecenie `azure config mode arm`. 
> 

Najszybszym sposobem odszukania obrazu jest wywołanie polecenia `azure vm image list` i przekazanie do niego lokalizacji, nazwy wydawcy (nie jest rozróżniana wielkość liter) oraz oferty, jeśli oferta jest znana. Na przykład poniższa lista stanowi tylko krótki przykład (wiele list jest dość długich) dla przypadku, w którym wiadomo, że wydawca „Canonical” udostępnia ofertę „UbuntuServer”.

```azurecli
azure vm image list westus canonical ubuntuserver
info:    Executing command vm image list
warn:    The parameter --sku if specified will be ignored
+ Getting virtual machine image skus (Publisher:"canonical" Offer:"ubuntuserver" Location:"westus")
data:    Publisher  Offer         Sku                OS     Version          Location  Urn
data:    ---------  ------------  -----------------  -----  ---------------  --------  --------------------------------------------------------
data:    canonical  ubuntuserver  16.04.0-LTS        Linux  16.04.201604203  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201604203
data:    canonical  ubuntuserver  16.04.0-LTS        Linux  16.04.201605161  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201605161
data:    canonical  ubuntuserver  16.04.0-LTS        Linux  16.04.201606100  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201606100
data:    canonical  ubuntuserver  16.04.0-LTS        Linux  16.04.201606270  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201606270
data:    canonical  ubuntuserver  16.04.0-LTS        Linux  16.04.201607210  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201607210
data:    canonical  ubuntuserver  16.04.0-LTS        Linux  16.04.201608150  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201608150
data:    canonical  ubuntuserver  16.10-DAILY        Linux  16.10.201607220  westus    canonical:ubuntuserver:16.10-DAILY:16.10.201607220
data:    canonical  ubuntuserver  16.10-DAILY        Linux  16.10.201607230  westus    canonical:ubuntuserver:16.10-DAILY:16.10.201607230
data:    canonical  ubuntuserver  16.10-DAILY        Linux  16.10.201607240  westus    canonical:ubuntuserver:16.10-DAILY:16.10.201607240
```

Do polecenia `azure vm quick-create` należy przekazać formę podaną w kolumnie **Urn**.

Często jednak na tym etapie nie wiadomo, co jest dostępne. W takim przypadku można nawigować po obrazach za pomocą polecenia `azure vm image list-publishers` i określać po wyświetleniu monitu lokalizację centrum danych. Na przykład poniższe polecenie tworzy listę wszystkich wydawców obrazów w lokalizacji „West US” (argument lokalizacji przekazuje się, stosując małe litery i usuwając spacje ze standardowej nazwy lokalizacji).

```azurecli
azure vm image list-publishers
info:    Executing command vm image list-publishers
Location: westus
+ Getting virtual machine and/or extension image publishers (Location: "westus")
data:    Publisher                                       Location
data:    ----------------------------------------------  --------
data:    a10networks                                     westus  
data:    aiscaler-cache-control-ddos-and-url-rewriting-  westus  
data:    alertlogic                                      westus  
data:    AlertLogic.Extension                            westus  
```

Te listy mogą być bardzo długie: podana powyżej lista to tylko fragment. Załóżmy, że udało się ustalić, że „Canonical” jest wydawcą obrazów w lokalizacji „West US”. Można teraz znaleźć jego oferty przez wywołanie polecenia `azure vm image list-offers` i przekazanie po wyświetleniu monitu lokalizacji i wydawcy, jak w następującym przykładzie:

```azurecli
azure vm image list-offers
info:    Executing command vm image list-offers
Location: westus
Publisher: canonical
+ Getting virtual machine image offers (Publisher: "canonical" Location:"westus")
data:    Publisher  Offer                      Location
data:    ---------  -------------------------  --------
data:    canonical  Ubuntu15.04Snappy          westus
data:    canonical  Ubuntu15.04SnappyDocker    westus
data:    canonical  UbunturollingSnappy        westus
data:    canonical  UbuntuServer               westus
data:    canonical  Ubuntu_Snappy_Core         westus
data:    canonical  Ubuntu_Snappy_Core_Docker  westus
info:    vm image list-offers command OK
```

Teraz wiemy, że w regionie „West US” wydawca „Canonical” publikuje ofertę **UbuntuServer** na platformie Azure. Ale o jakie wersje SKU chodzi? Aby uzyskać te wartości, należy wywołać polecenie `azure vm image list-skus` i w odpowiedzi na monity podać odnalezione już lokalizację, wydawcę i ofertę.

```azurecli
azure vm image list-skus
info:    Executing command vm image list-skus
Location: westus
Publisher: canonical
Offer: ubuntuserver
+ Getting virtual machine image skus (Publisher:"canonical" Offer:"ubuntuserver" Location:"westus")
data:    Publisher  Offer         sku                Location
data:    ---------  ------------  -----------------  --------
data:    canonical  ubuntuserver  12.04.2-LTS        westus
data:    canonical  ubuntuserver  12.04.3-LTS        westus
data:    canonical  ubuntuserver  12.04.4-LTS        westus
data:    canonical  ubuntuserver  12.04.5-DAILY-LTS  westus
data:    canonical  ubuntuserver  12.04.5-LTS        westus
data:    canonical  ubuntuserver  12.10              westus
data:    canonical  ubuntuserver  14.04-beta         westus
data:    canonical  ubuntuserver  14.04.0-LTS        westus
data:    canonical  ubuntuserver  14.04.1-LTS        westus
data:    canonical  ubuntuserver  14.04.2-LTS        westus
data:    canonical  ubuntuserver  14.04.3-LTS        westus
data:    canonical  ubuntuserver  14.04.4-DAILY-LTS  westus
data:    canonical  ubuntuserver  14.04.4-LTS        westus
data:    canonical  ubuntuserver  14.04.5-DAILY-LTS  westus
data:    canonical  ubuntuserver  14.04.5-LTS        westus
data:    canonical  ubuntuserver  14.10              westus
data:    canonical  ubuntuserver  14.10-beta         westus
data:    canonical  ubuntuserver  14.10-DAILY        westus
data:    canonical  ubuntuserver  15.04              westus
data:    canonical  ubuntuserver  15.04-beta         westus
data:    canonical  ubuntuserver  15.04-DAILY        westus
data:    canonical  ubuntuserver  15.10              westus
data:    canonical  ubuntuserver  15.10-alpha        westus
data:    canonical  ubuntuserver  15.10-beta         westus
data:    canonical  ubuntuserver  15.10-DAILY        westus
data:    canonical  ubuntuserver  16.04-alpha        westus
data:    canonical  ubuntuserver  16.04-beta         westus
data:    canonical  ubuntuserver  16.04.0-DAILY-LTS  westus
data:    canonical  ubuntuserver  16.04.0-LTS        westus
data:    canonical  ubuntuserver  16.10-DAILY        westus
info:    vm image list-skus command OK
```

Dzięki tym informacjom można teraz znaleźć dokładnie odpowiedni obraz, stosując oryginalne wywołanie u góry.

```azurecli
azure vm image list westus canonical ubuntuserver 16.04.0-LTS
info:    Executing command vm image list
+ Getting virtual machine images (Publisher:"canonical" Offer:"ubuntuserver" Sku: "16.04.0-LTS" Location:"westus")
data:    Publisher  Offer         Sku          OS     Version          Location  Urn
data:    ---------  ------------  -----------  -----  ---------------  --------  --------------------------------------------------
data:    canonical  ubuntuserver  16.04.0-LTS  Linux  16.04.201604203  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201604203
data:    canonical  ubuntuserver  16.04.0-LTS  Linux  16.04.201605161  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201605161
data:    canonical  ubuntuserver  16.04.0-LTS  Linux  16.04.201606100  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201606100
data:    canonical  ubuntuserver  16.04.0-LTS  Linux  16.04.201606270  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201606270
data:    canonical  ubuntuserver  16.04.0-LTS  Linux  16.04.201607210  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201607210
data:    canonical  ubuntuserver  16.04.0-LTS  Linux  16.04.201608150  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201608150
info:    vm image list command OK
```

Teraz można dokładnie wybrać obraz, który ma być używany. Aby szybko utworzyć maszynę wirtualną za pomocą właśnie znalezionych informacji URN lub aby użyć szablonu z tymi informacjami URN, zobacz [Używanie interfejsu wiersza polecenia platformy Azure na komputerach Mac i komputerach z systemem Linux oraz Windows z usługą Azure Resource Manager](../articles/xplat-cli-azure-resource-manager.md).

## <a name="powershell"></a>PowerShell
> [!NOTE]
> Zainstaluj i skonfiguruj [najnowszą wersję programu Azure PowerShell](/powershell/azureps-cmdlets-docs). W przypadku korzystania z modułów programu Azure PowerShell w wersji starszej niż 1.0 można używać podanych poniżej poleceń, ale należy najpierw wykonać polecenie `Switch-AzureMode AzureResourceManager`. 
> 
> 

Podczas tworzenia nowej maszyny wirtualnej przy użyciu usługi Azure Resource Manager w niektórych przypadkach należy określić obraz za pomocą kombinacji następujących właściwości obrazu:

* Wydawca
* Oferta
* SKU

Te wartości należy na przykład koniecznie podać w razie używania polecenia cmdlet programu PowerShell `Set-AzureRMVMSourceImage` lub pliku szablonu grupy zasobów, w którym należy określić typ tworzonej maszyny wirtualnej.

Jeśli trzeba ustalić te wartości, można w tym celu nawigować po obrazach:

1. Wyświetl listę wydawców obrazów.
2. Dla danego wydawcy wyświetl listę ofert.
3. Dla danej oferty wyświetl listę wersji SKU.

Najpierw wyświetl listę wydawców za pomocą następujących poleceń:

```powershell
$locName="<Azure location, such as West US>"
Get-AzureRMVMImagePublisher -Location $locName | Select PublisherName
```

Wprowadź nazwę wybranego wydawcy i uruchom następujące polecenia:

```powershell
$pubName="<publisher>"
Get-AzureRMVMImageOffer -Location $locName -Publisher $pubName | Select Offer
```

Wprowadź nazwę wybranej oferty i uruchom następujące polecenia:

```powershell
$offerName="<offer>"
Get-AzureRMVMImageSku -Location $locName -Publisher $pubName -Offer $offerName | Select Skus
```

Dane wyświetlone przez polecenie `Get-AzureRMVMImageSku` zawierają wszystkie informacje potrzebne do określenia obrazu dla nowej maszyny wirtualnej.

Poniżej przedstawiono pełny przykład:

```powershell
PS C:\> $locName="West US"
PS C:\> Get-AzureRMVMImagePublisher -Location $locName | Select PublisherName

PublisherName
-------------
a10networks
aiscaler-cache-control-ddos-and-url-rewriting-
alertlogic
AlertLogic.Extension
Barracuda.Azure.ConnectivityAgent
barracudanetworks
basho
boxless
bssw
Canonical
...
```

Dla wydawcy „MicrosoftWindowsServer”:

```powershell
PS C:\> $pubName="MicrosoftWindowsServer"
PS C:\> Get-AzureRMVMImageOffer -Location $locName -Publisher $pubName | Select Offer

Offer
-----
WindowsServer
```

Dla oferty „WindowsServer”:

```powershell
PS C:\> $offerName="WindowsServer"
PS C:\> Get-AzureRMVMImageSku -Location $locName -Publisher $pubName -Offer $offerName | Select Skus

Skus
----
2008-R2-SP1
2012-Datacenter
2012-R2-Datacenter
2016-Nano-Server-Technical-Previe
2016-Technical-Preview-with-Conta
Windows-Server-Technical-Preview
```

Skopiuj z tej listy nazwę wybranej wersji SKU — w ten sposób uzyskasz wszystkie informacje, jakie należy podać w poleceniu cmdlet programu PowerShell `Set-AzureRMVMSourceImage` lub szablonie grupy zasobów.

<!--Image references-->
[5]: ./media/markdown-template-for-new-articles/octocats.png
[6]: ./media/markdown-template-for-new-articles/pretty49.png
[7]: ./media/markdown-template-for-new-articles/channel-9.png
[8]: ./media/markdown-template-for-new-articles/copytemplate.png

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[gog]: http://google.com/
[yah]: http://search.yahoo.com/  
[msn]: http://search.msn.com/

<!--HONumber=Feb17_HO3-->


