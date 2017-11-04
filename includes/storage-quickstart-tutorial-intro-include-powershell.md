## <a name="sign-in-to-azure"></a>Logowanie do platformy Azure

Zaloguj się do subskrypcji platformy Azure za pomocą polecenia `Login-AzureRmAccount` i postępuj zgodnie z instrukcjami wyświetlanymi na ekranie.

```powershell
Login-AzureRmAccount
```

Jeśli nie znasz lokalizacji, która ma być używany, można wyświetlić listę dostępnych lokalizacji. Po wyświetleniu listy znaleźć ten, który ma być używany. W tym przykładzie będzie używać **eastus**. Zapisać w zmiennej i użyj zmiennej, dzięki czemu można go zmieniać w jednym miejscu.

```powershell
Get-AzureRmLocation | select Location 
$location = "eastus"
```

## <a name="create-a-resource-group"></a>Tworzenie grupy zasobów

Utwórz grupę zasobów platformy Azure za pomocą polecenia [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Grupa zasobów to logiczny kontener przeznaczony do wdrażania zasobów platformy Azure i zarządzania nimi. 

```powershell
$resourceGroup = "myResourceGroup"
New-AzureRmResourceGroup -Name $resourceGroup -Location $location 
```

## <a name="create-a-storage-account"></a>Tworzenie konta magazynu

Tworzenie konta standardowe magazynu ogólnego przeznaczenia przy użyciu replikacji LRS [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/New-AzureRmStorageAccount), następnie pobrać kontekst konta magazynu, który definiuje konto magazynu do użycia. Działając na konto magazynu, możesz odwoływać się kontekstu zamiast wielokrotnie podawania poświadczeń. W tym przykładzie tworzy konto magazynu o nazwie *mojekontomagazynu* z lokalnie nadmiarowego magazynu i obiektów blob szyfrowanie włączone.

```powershell
$storageAccount = New-AzureRmStorageAccount -ResourceGroupName $resourceGroup `
  -Name "mystorageaccount" `
  -Location $location `
  -SkuName Standard_LRS `
  -Kind Storage `
  -EnableEncryptionService Blob

$ctx = $storageAccount.Context
```
