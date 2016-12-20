
## <a name="start-your-powershell-session"></a>Uruchamianie sesji programu PowerShell
Najpierw należy zainstalować i uruchomić najnowszą wersję programu [Azure PowerShell](https://msdn.microsoft.com/library/mt619274\(v=azure.300\).aspx). Aby uzyskać szczegółowe informacje, zobacz temat [Instalowanie i konfigurowanie programu Azure PowerShell](/powershell/azureps-cmdlets-docs).

> [!NOTE]
> Wiele nowych funkcji bazy danych SQL jest obsługiwanych tylko przy zastosowaniu [modelu wdrażania usługi Azure Resource Manager](../articles/azure-resource-manager/resource-group-overview.md), dlatego w przykładach użyto [poleceń cmdlet programu PowerShell dla usługi Azure SQL Database](https://msdn.microsoft.com/library/azure/mt574084\(v=azure.300\).aspx). [Polecenia cmdlet usługi Azure SQL Database](https://msdn.microsoft.com/library/azure/dn546723\(v=azure.300\).aspx) używane w modelu wdrażania zarządzania usługami (klasycznym) są obsługiwane w celu zapewnienia zgodności z poprzednimi wersjami, ale zalecamy użycie poleceń cmdlet usługi Resource Manager.
> 
> 

Po uruchomieniu polecenia cmdlet [**Add-AzureRmAccount**](https://msdn.microsoft.com/library/azure/mt619267\(v=azure.300\).aspx) zostanie wyświetlony ekran logowania, w którym należy wprowadzić poświadczenia. Użyj tych samych poświadczeń, których używasz do logowania w witrynie Azure Portal.

    Add-AzureRmAccount

Jeśli masz wiele subskrypcji, użyj polecenia cmdlet [**Set-AzureRmContext**](https://msdn.microsoft.com/library/azure/mt619263\(v=azure.300\).aspx) i wybierz subskrypcję, która ma być użyta w sesji programu PowerShell. Aby sprawdzić, która subskrypcja jest używana w bieżącej sesji programu PowerShell, uruchom polecenie [**Get-AzureRmContext**](https://msdn.microsoft.com/library/azure/mt619265\(v=azure.300\).aspx). Aby wyświetlić wszystkie subskrypcje, uruchom polecenie [**Get-AzureRmSubscription**](https://msdn.microsoft.com/library/azure/mt619284\(v=azure.300\).aspx).

    Set-AzureRmContext -SubscriptionId '4cac86b0-1e56-bbbb-aaaa-000000000000'


<!--HONumber=Dec16_HO1-->


