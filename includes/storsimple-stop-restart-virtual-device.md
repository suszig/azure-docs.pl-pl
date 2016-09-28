#### Aby zatrzymać i ponownie uruchomić urządzenie wirtualne
Aby zatrzymać urządzenie wirtualne, kliknij jego nazwę, a następnie kliknij pozycję **Zamknij**. Stan zamykanego urządzenia wirtualnego to **Zatrzymywanie**. Stan zamkniętego urządzenia wirtualnego to **Zatrzymane**.

Użyj poniższych poleceń cmdlet do zatrzymywania i uruchamiania urządzenia wirtualnego.

`Stop-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`


`Start-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`
    
#### Aby ponownie uruchomić urządzenia wirtualne

Jeśli urządzenie wirtualne działa i chcesz uruchomić je ponownie, kliknij nazwę urządzenia, a następnie kliknij pozycję **Uruchom ponownie**. Stan ponownie uruchamianego urządzenia wirtualnego to **Ponowne uruchamianie**. Gdy urządzenie wirtualne jest gotowe do użycia, jego stan to **Uruchomione**.

Użyj poniższego polecenia cmdlet, aby ponownie uruchomić urządzenie wirtualne.

`Restart-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`






<!--HONumber=Sep16_HO3-->


