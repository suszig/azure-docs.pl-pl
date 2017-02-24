UnifiedSetup.exe [/ServerMode <CS/PS>] [/InstallDrive <DriveLetter>] [/MySQLCredsFilePath <MySQL credentials file path>] [/VaultCredsFilePath <Vault credentials file path>] [/EnvType <VMWare/NonVMWare>] [/PSIP <adres IP do użycia do transferu danych] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>]

Parametry:

* /ServerMode: obowiązkowy. Określa, czy mają zostać zainstalowane oba serwery, konfiguracji i przetwarzania, czy tylko serwer przetwarzania. Wartości wejściowe: CS, PS.
* InstallLocation: obowiązkowy. Folder, w którym są instalowane składniki.
* /MySQLCredsFilePath. Obowiązkowy. Ścieżka pliku, w której są przechowywane poświadczenia serwera MySQL. Plik powinien mieć następujący format:
* [MySQLCredentials]
* MySQLRootPassword = "<Password>"
* MySQLUserPassword = "<Password>"
* /VaultCredsFilePath. Obowiązkowy. Lokalizacja pliku poświadczeń magazynu.
* /EnvType. Obowiązkowy. Typ instalacji. Wartości: VMware, NonVMware
* / PSIP i /CSIP. Obowiązkowy. Adres IP serwera przetwarzania i serwera konfiguracji.
* /PassphraseFilePath. Obowiązkowy. Lokalizacja pliku hasła.
* /BypassProxy. Opcjonalny. Określa, że serwer konfiguracji łączy się z platformą Azure bez serwera proxy.
* /ProxySettingsFilePath. Opcjonalny. Ustawienia serwera proxy (domyślny serwer proxy wymaga uwierzytelniania lub niestandardowy serwer proxy). Plik powinien mieć następujący format:
* [ProxySettings]
* ProxyAuthentication = "Yes/No"
* Proxy IP = "IP Address>"
* ProxyPort = "<Port>"
* ProxyUserName="<User Name>"
* ProxyPassword="<Password>"
* DataTransferSecurePort. Opcjonalny. Numer portu dla danych replikacji.
* SkipSpaceCheck. Opcjonalny. Pomiń sprawdzanie miejsca dla pamięci podręcznej.
* AcceptThirdpartyEULA. Obowiązkowy. Akceptuje umowę licencyjną innej firmy.
* ShowThirdpartyEULA. Obowiązkowy. Wyświetla umowę licencyjną innej firmy. Jeśli zostanie podany w danych wejściowych, wszystkie inne parametry są ignorowane.


<!--HONumber=Feb17_HO2-->


