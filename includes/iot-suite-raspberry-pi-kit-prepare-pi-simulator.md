## <a name="prepare-your-raspberry-pi"></a>Przygotowanie Twojego malinowe Pi

### <a name="install-raspbian"></a>Zainstaluj Raspbian

Jeśli jest to w przypadku korzystania z Pi malina po raz pierwszy, musisz zainstalować Raspbian systemu operacyjnego za pomocą NOOBS na kartę SD. zawarte w zestawie. [Malina Pi oprogramowania przewodnik] [ lnk-install-raspbian] opisuje sposób instalowania systemu operacyjnego na Twoje malina Pi. W tym samouczku założono, że na Twoje Pi malina zainstalowano Raspbian systemu operacyjnego.

> [!NOTE]
> Karta SD objęte [Microsoft Azure IoT Starter Kit malina Pi 3] [ lnk-starter-kits] ma już zainstalowany NOOBS. Możesz rozruch Pi malina z tej karty i wybrać opcję instalacji systemu operacyjnego Raspbian.

Aby ukończyć instalację na sprzęt, musisz:

- Twoje Pi malina nawiązać połączenia z zasilacz zawarte w zestawie.
- Twoje Pi malina nawiązanie połączenia z siecią za pomocą kabla Ethernet zawarte w zestawie. Alternatywnie możesz skonfigurować [łączności bezprzewodowej] [ lnk-pi-wireless] Twojego malina pi.

Ustawienia sprzętu z Pi malina zostało zakończone.

### <a name="sign-in-and-access-the-terminal"></a>Zaloguj się i uzyskać dostęp terminala

Dostępne są dwie opcje środowisku końcowych dla sieci Pi malina dostępu do:

- Jeśli masz klawiatury i monitora podłączone do sieci Pi malina umożliwia Raspbian graficzny interfejs użytkownika dostępu okno terminala.

- Dostęp do wiersza polecenia na Twoje Pi malina przy użyciu protokołu SSH z komputera stacjonarnego.

#### <a name="use-a-terminal-window-in-the-gui"></a>Okno terminalu w graficznym interfejsie użytkownika

Nazwa użytkownika są domyślne poświadczenia dla Raspbian **pi** i hasło **malina**. Na pasku zadań w interfejsie GUI, uruchom **Terminal** narzędzie przy użyciu ikony, która wygląda jak monitora.

#### <a name="sign-in-with-ssh"></a>Zaloguj się przy użyciu protokołu SSH

Umożliwia SSH dla wiersza polecenia dostępu użytkownika Pi malina. Artykuł [SSH (Secure Shell)] [ lnk-pi-ssh] w tym artykule opisano sposób konfigurowania SSH na Twoje Pi malina oraz sposób nawiązywania połączenia z [Windows] [ lnk-ssh-windows] lub [ System operacyjny Linux & Mac][lnk-ssh-linux].

Zaloguj się przy użyciu nazwy użytkownika **pi** i hasło **malina**.

#### <a name="optional-share-a-folder-on-your-raspberry-pi"></a>Opcjonalnie: Udostępnianie folderu w Twojej Pi malina

Opcjonalnie można udostępnić folder w sieci Pi malina środowisko pulpitu. Udostępnianie folderu pozwala na użycie w edytorze tekstów pulpitu preferowanych (takich jak [Visual Studio Code](https://code.visualstudio.com/) lub [Sublime tekst](http://www.sublimetext.com/)) do edycji plików w sieci Pi malina zamiast `nano` lub `vi`.

Aby udostępnić folder z systemem Windows, należy skonfigurować serwer Samba na malina Pi. Alternatywnie można użyć wbudowanej [SFTP](https://www.raspberrypi.org/documentation/remote-access/) serwera za pomocą klienta protokołu SFTP na pulpicie.

[lnk-install-raspbian]: https://www.raspberrypi.org/learning/software-guide/quickstart/
[lnk-pi-wireless]: https://www.raspberrypi.org/documentation/configuration/wireless/README.md
[lnk-pi-ssh]: https://www.raspberrypi.org/documentation/remote-access/ssh/README.md
[lnk-ssh-windows]: https://www.raspberrypi.org/documentation/remote-access/ssh/windows.md
[lnk-ssh-linux]: https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md
[lnk-starter-kits]: https://azure.microsoft.com/develop/iot/starter-kits/