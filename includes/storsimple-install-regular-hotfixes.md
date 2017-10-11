<!--author=SharS last changed: 9/17/15-->

#### <a name="to-install-regular-hotfixes-via-windows-powershell-for-storsimple"></a>Aby zainstalować regularne poprawek za pomocą środowiska Windows PowerShell dla urządzenia StorSimple
1. Łączenie z konsolą szeregową urządzenia. Aby uzyskać więcej informacji, zobacz [krok 1: łączenie z konsolą szeregową](../articles/storsimple/storsimple-update-device.md#step1).
2. W menu konsoli szeregowej wybierz opcję 1, **Zaloguj się przy użyciu pełnego dostępu**. Wpisz hasło. Domyślne hasło jest **Password1**.
3. W wierszu polecenia wpisz:
   
    ```
    Start-HcsHotfix
    ```
   
    > [!IMPORTANT]
    >
    > To polecenie dotyczy tylko regularne poprawki. Uruchom to polecenie na tylko jeden kontroler, ale zostaną zaktualizowane obu kontrolerów.
    > Tryb failover kontrolera mogą pojawić się podczas procesu aktualizacji; jednak tryb failover nie wpłynie na dostępność systemu lub operacji.

4. Po wyświetleniu monitu podaj ścieżkę do udostępnionego folderu sieciowego, który zawiera pliki poprawek.
5. Pojawi się monit o potwierdzenie. Typ **Y** kontynuować instalacji poprawki.

