
## Uruchamianie sesji programu PowerShell

Najpierw należy zainstalować i uruchomić najnowszą wersję programu [Azure PowerShell](https://msdn.microsoft.com/library/mt619274.aspx). Aby uzyskać szczegółowe informacje, zobacz temat [Instalowanie i konfigurowanie programu Azure PowerShell](../articles/powershell-install-configure.md).


>[AZURE.NOTE] Wiele nowych funkcji bazy danych SQL jest obsługiwanych tylko przy zastosowaniu [modelu wdrażania usługi Azure Resource Manager](../articles/resource-group-overview.md), dlatego w przykładach użyto [poleceń cmdlet programu PowerShell dla usługi Azure SQL Database](https://msdn.microsoft.com/library/azure/mt574084.aspx). Istniejące [(klasyczne) polecenia cmdlet bazy danych SQL Azure ](https://msdn.microsoft.com/library/azure/dn546723.aspx) używane w klasycznym modelu wdrażania są obsługiwane w celu zapewnienia zgodności z poprzednimi wersjami, ale zalecamy użycie poleceń cmdlet usługi Resource Manager.


Po uruchomieniu polecenia cmdlet [**Add-AzureRmAccount**](https://msdn.microsoft.com/library/mt619267.aspx) zostanie wyświetlony ekran logowania, w którym należy wprowadzić poświadczenia. Użyj tych samych poświadczeń, których używasz do logowania w witrynie Azure Portal.

    Add-AzureRmAccount

Jeśli masz wiele subskrypcji, użyj polecenia cmdlet [**Set-AzureRmContext**](https://msdn.microsoft.com/library/mt619263.aspx) i wybierz subskrypcję, która ma być użyta w sesji programu PowerShell. Aby sprawdzić, która subskrypcja jest używana w bieżącej sesji programu PowerShell, uruchom polecenie [**Get-AzureRmContext**](https://msdn.microsoft.com/library/mt619265.aspx). Aby wyświetlić wszystkie subskrypcje, uruchom polecenie [**Get-AzureRmSubscription**](https://msdn.microsoft.com/library/mt619284.aspx).

    Set-AzureRmContext -SubscriptionId '4cac86b0-1e56-bbbb-aaaa-000000000000'


<!--HONumber=Sep16_HO3-->


