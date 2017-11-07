## <a name="prepare-your-intel-nuc"></a>Przygotowanie programu Intel NUC

Aby ukończyć instalację na sprzęt, musisz:

- Twoje NUC Intel nawiązać połączenia z zasilacz zawarte w zestawie.
- Twoje NUC Intel nawiązać połączenia z siecią za pośrednictwem kabla Ethernet.

Ukończono konfigurację sprzętu urządzenie bramy Intel NUC.

### <a name="sign-in-and-access-the-terminal"></a>Zaloguj się i uzyskać dostęp terminala

Dostępne są dwie opcje środowisku terminala dla Twojej firmy Intel NUC dostępu do:

- Jeśli masz klawiatury i monitora podłączone do sieci NUC firmy Intel, możesz uzyskać dostęp powłoki bezpośrednio. Poświadczenia domyślne są username **głównego** i hasło **głównego**.

- Dostęp do powłoki na Twojej NUC Intel przy użyciu protokołu SSH z komputera stacjonarnego.

#### <a name="sign-in-with-ssh"></a>Zaloguj się przy użyciu protokołu SSH

Aby zalogować się przy użyciu protokołu SSH, należy adres IP Twojego NUC Intel. Jeśli masz klawiatury i monitora podłączone do sieci NUC firmy Intel, użyj `ifconfig` polecenie, aby znaleźć adres IP. Można również nawiązać router w taki sposób, aby wyświetlić listę adresów urządzeń w sieci.

Zaloguj się przy użyciu nazwy użytkownika **głównego** i hasło **głównego**.

#### <a name="optional-share-a-folder-on-your-intel-nuc"></a>Opcjonalnie: Udostępnianie folderu w Twojej NUC Intel

Opcjonalnie można udostępnić folder w sieci NUC Intel środowisko pulpitu. Udostępnianie folderu pozwala na użycie w edytorze tekstów pulpitu preferowanych (takich jak [Visual Studio Code](https://code.visualstudio.com/) lub [Sublime tekst](http://www.sublimetext.com/)) do edycji plików w sieci NUC Intel zamiast `nano` lub `vi`.

Aby udostępnić folder z systemem Windows, należy skonfigurować serwer Samba na Intel NUC. Alternatywnie można użyć serwera SFTP na NUC Intel za pomocą protokołu SFTP klienta na komputerze pulpitu.
