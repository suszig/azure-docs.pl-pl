<!--author=SharS last changed: 9/17/15-->

#### <a name="to-install-maintenance-mode-updates-via-windows-powershell-for-storsimple"></a>Aby zainstalować aktualizacje tryb konserwacji za pomocą środowiska Windows PowerShell dla urządzenia StorSimple
1. Jeśli jeszcze tego nie zrobiono tego wcześniej, dostęp do konsoli szeregowej urządzenia i wybierz opcję 1, **Zaloguj się przy użyciu pełnego dostępu**. 
2. Wpisz hasło. Domyślne hasło jest **Password1**.
3. W wierszu polecenia wpisz:
   
     `Get-HcsUpdateAvailability` 
4. Jeśli aktualizacje są dostępne i czy są aktualizacje destrukcyjne lub Brak, otrzymasz powiadomienie. Aby zastosować aktualizacje problem, należy umieścić urządzenia w trybie konserwacji. Zobacz [krok 2: tryb konserwacji wprowadź](../articles/storsimple/storsimple-update-device.md#step2) instrukcje.
5. Gdy urządzenie jest w trybie konserwacji, w wierszu polecenia wpisz:`Start-HcsUpdate`
6. Pojawi się monit o potwierdzenie. Po potwierdzeniu aktualizacji będzie można zainstalować na kontrolerze, który obecnie używasz. Po zainstalowaniu aktualizacji kontrolera zostanie uruchomiony ponownie. 
7. Monitorowanie stanu aktualizacji. Wpisz:
   
    `Get-HcsUpdateStatus`
   
    Jeśli `RunInProgress` jest `True`, aktualizacja jest nadal w toku. Jeśli `RunInProgress` jest `False`, oznacza to, że aktualizacja została ukończona.  
8. Gdy aktualizacja jest zainstalowana na kontrolerze bieżącej i została uruchomiona ponownie, połączyć się z innym kontrolerem i wykonaj kroki od 1 do 6.
9. Po aktualizacji obu kontrolerów wyjść z trybu konserwacji. Zobacz [krok 4: tryb konserwacji zakończenia](../articles/storsimple/storsimple-update-device.md#step4) instrukcje.

