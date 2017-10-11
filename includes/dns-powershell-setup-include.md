## <a name="set-up-azure-powershell-for-azure-dns"></a>Konfigurowanie programu Azure PowerShell dla usługi Azure DNS

### <a name="before-you-begin"></a>Przed rozpoczęciem

Przed rozpoczęciem konfiguracji sprawdź, czy dysponujesz następującymi elementami:

* Subskrypcja platformy Azure. Jeśli nie masz jeszcze subskrypcji platformy Azure, możesz aktywować [korzyści dla subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) lub utworzyć [bezpłatne konto](https://azure.microsoft.com/pricing/free-trial/).
* Musisz zainstalować najnowszą wersję poleceń cmdlet programu PowerShell Menedżera zasobów Azure. Aby uzyskać więcej informacji, zobacz [Instalowanie i konfigurowanie programu Azure PowerShell](/powershell/azureps-cmdlets-docs).

### <a name="sign-in-to-your-azure-account"></a>Zaloguj się do swojego konta platformy Azure

Otwórz konsolę programu PowerShell i połącz się ze swoim kontem. Aby uzyskać więcej informacji, zobacz [przy użyciu programu PowerShell z usługą Resource Manager](../articles/azure-resource-manager/powershell-azure-resource-manager.md).

```powershell
Login-AzureRmAccount
```

### <a name="select-the-subscription"></a>Wybierz subskrypcję
 
Sprawdź subskrypcje dostępne na koncie.

```powershell
Get-AzureRmSubscription
```

Wybierz subskrypcję platformy Azure do użycia.

```powershell
Select-AzureRmSubscription -SubscriptionName "your_subscription_name"
```

### <a name="create-a-resource-group"></a>Tworzenie grupy zasobów

Usługa Azure Resource Manager wymaga, aby wszystkie grupy zasobów określały lokalizację. Ta lokalizacja będzie używana jako domyślna lokalizacja dla zasobów w danej grupie zasobów. Ponieważ jednak wszystkie zasoby DNS są globalne, a nie regionalne, wybór lokalizacji grupy zasobów nie ma wpływu na usługę Azure DNS.

Ten krok można pominąć, jeśli używasz istniejącej grupy zasobów.

```powershell
New-AzureRmResourceGroup -Name MyAzureResourceGroup -location "West US"
```

### <a name="register-resource-provider"></a>Rejestrowanie dostawcy zasobów

Usługa Azure DNS jest zarządzana przez dostawcę zasobów Microsoft.Network. Aby można było korzystać z usługi Azure DNS, Twoja subskrypcja platformy Azure musi być zarejestrowana do używania tego dostawcy zasobów. Jest to jednorazowa operacja dla każdej subskrypcji.

```powershell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
```