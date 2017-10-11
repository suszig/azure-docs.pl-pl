<!--author=SharS last changed: 11/18/16-->

#### <a name="to-install-regular-updates-via-windows-powershell-for-storsimple"></a>Aby zainstalować regularne aktualizacje za pomocą środowiska Windows PowerShell dla urządzenia StorSimple
1. Otwieranie konsoli szeregowej urządzenia i wybierz opcję 1, **Zaloguj się przy użyciu pełnego dostępu**. Wpisz hasło. Domyślne hasło jest *Password1*. 
2. W wierszu polecenia wpisz:
   
     `Get-HcsUpdateAvailability`
   
    Jeśli aktualizacje są dostępne i czy są aktualizacje destrukcyjne lub Brak, otrzymasz powiadomienie.
3. W wierszu polecenia wpisz:
   
     `Start-HcsUpdate`
   
    Rozpocznie się proces aktualizacji.

> [!IMPORTANT]
> * To polecenie dotyczy tylko regularne aktualizacje. Uruchom to polecenie na tylko jeden kontroler, ale zostaną zaktualizowane obu kontrolerów. 
> * Tryb failover kontrolera mogą pojawić się podczas procesu aktualizacji; jednak tryb failover nie wpłynie na dostępność systemu lub operacji.
> 
> 

