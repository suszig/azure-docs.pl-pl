### <a name="prepare-for-a-push-installation-on-a-linux-server"></a>Przygotowanie do instalacji wypychanej na serwerze z systemem Linux

1. Upewnij się, że nawiązano połączenie sieciowe między komputerem z systemem Linux a serwerem przetwarzania.
2. Utwórz konto, za pomocą którego serwer przetwarzania będzie mógł uzyskać dostęp do komputera. Konto powinno być użytkownikiem **root** na źródłowym serwerze z systemem Linux. (Użyj tego konta tylko w związku z instalacją wypychaną i aktualizacjami).
3. Sprawdź, czy plik /etc/hosts na źródłowym serwerze z systemem Linux zawiera wpisy mapujące lokalną nazwę hosta na adres IP skojarzony ze wszystkimi kartami sieciowymi.
4. Zainstaluj najnowsze pakiety openssh, openssh-server i openssl na komputerze, który chcesz replikować.
5. Upewnij się, że protokół Secure Shell (SSH) jest włączony i uruchomiony na porcie 22.
6. Włącz podsystem SFTP i uwierzytelnianie hasłem w pliku sshd_config:
  1.  Zaloguj się jako użytkownik **root**.
  2.  W pliku /etc/ssh/sshd_config znajdź wiersz zaczynający się od ciągu **PasswordAuthentication**.
  3.  Usuń znaczniki komentarza dla wiersza i zmień wartość na **yes**.
  4.  Znajdź wiersz zaczynający się od ciągu **Subsystem** i usuń znaczniki komentarza.

     ![Linux](./media/site-recovery-prepare-push-install-mob-svc-lin/mobility2.png)
  5. Uruchom ponownie usługę **sshd**.

7. Dodaj konto utworzone w narzędziu CSPSConfigtool.
    1.  Zaloguj się do serwera konfiguracji.
    2.  Otwórz plik **cspsconfigtool.exe**. (Jest dostępny jako skrót na pulpicie oraz w folderze %ProgramData%\home\svsystems\bin).
    3.  Na karcie **Zarządzaj kontami** kliknij pozycję **Dodaj konto**.
    4.  Dodaj utworzone konto. 
    5.  Wprowadź używane poświadczenia po włączeniu replikacji dla komputera.
