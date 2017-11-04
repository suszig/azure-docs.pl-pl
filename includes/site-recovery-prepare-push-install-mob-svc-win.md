### <a name="prepare-for-a-push-installation-on-a-windows-computer"></a>Przygotowanie do instalacji wypychanej na komputerze z systemem Windows

1. Upewnij się, że istnieje połączenie sieciowe między komputerem z systemem Windows i serwera przetwarzania.
2. Utwórz konto, za pomocą którego serwer przetwarzania będzie mógł uzyskać dostęp do komputera. Konto musi mieć prawa administratora (lokalnego lub domeny). (Używają tego konta, tylko dla instalacji wypychanej i dla aktualizacji agenta).

   > [!NOTE]
   > Jeśli nie używasz konta domeny, należy wyłączyć kontroli dostępu użytkownika zdalnego na komputerze lokalnym. Aby wyłączyć kontroli dostępu użytkownika zdalnego, w kluczu rejestru HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System dodać nową wartość typu DWORD: **LocalAccountTokenFilterPolicy**. Ustaw wartość na **1**. W tym celu w wierszu polecenia Uruchom następujące polecenie:  
   `REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`
   >
   >
2. W Zaporze systemu Windows na komputerze, który chcesz chronić, wybierz **Zezwalaj aplikacji lub funkcji przez zaporę**. Włącz **udostępnianie plików i drukarek** i **Instrumentacji zarządzania Windows (WMI)**. Dla komputerów, które należą do domeny można skonfigurować ustawienia zapory, za pomocą obiektu zasad grupy (GPO).

   ![Ustawienia zapory](./media/site-recovery-prepare-push-install-mob-svc-win/mobility1.png)

3. Dodaj konto utworzone w narzędziu CSPSConfigtool.
    1.  Zaloguj się do serwera konfiguracji.
    2.  Otwórz plik **cspsconfigtool.exe**. (Jest dostępny jako skrót na pulpicie oraz w folderze %ProgramData%\home\svsystems\bin).
    3.  Na **Zarządzanie kontami** wybierz opcję **Dodaj konto**.
    4.  Dodaj utworzone konto.
    5.  Wprowadź używane poświadczenia po włączeniu replikacji dla komputera.
