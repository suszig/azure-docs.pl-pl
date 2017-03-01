## <a name="delete-a-recovery-services-vault-powershell"></a>Usuwanie magazynu usług Recovery Services (PowerShell)

1. Pobierz magazyn usług Recovery Services.

        $vault = Get-AzureRmRecoveryServicesVault -Name "ContosoVault"

2. Usuń magazyn.

        Remove-AzureRmRecoveryServicesVault -Vault $vault

>[!WARNING]
>
> Powyższego polecenia należy używać z zachowaniem szczególnej ostrożności, ponieważ omyłkowe usunięcie magazynu spowoduje utratę wszystkich danych. Skutki tej akcji są trwałe i nie można jej cofnąć.  




<!--HONumber=Feb17_HO3-->


