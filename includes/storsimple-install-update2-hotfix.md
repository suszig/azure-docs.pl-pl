<!--author=alkohli last changed: 03/17/16-->

#### <a name="to-download-hotfixes"></a>Aby pobrać poprawki
Wykonaj następujące kroki, aby pobrać aktualizację oprogramowania z Wykazu usługi Microsoft Update.

1. Uruchom program Internet Explorer i przejdź pod adres [http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).
2. Jeśli po raz pierwszy używasz Wykazu usługi Microsoft Update na danym komputerze, po wyświetleniu monitu o zainstalowanie dodatku Wykazu usługi Microsoft Update kliknij pozycję **Zainstaluj**.
    ![Zainstaluj katalogu](./media/storsimple-install-update2-hotfix/HCS_InstallCatalog-include.png)
3. W polu wyszukiwania z wykazu usługi Microsoft Update, wprowadź numer bazy wiedzy Knowledge Base (KB) poprawki do pobrania, na przykład **3121901**, a następnie kliknij przycisk **wyszukiwania**.
   
    Poprawka zostanie wyświetlona na liście, na przykład **zbiorczą oprogramowania pakietu aktualizacji w wersji 2.0 dla z serii StorSimple 8000**.
   
    ![Przeszukiwanie wykazu](./media/storsimple-install-update2-hotfix/HCS_SearchCatalog1-include.png)
4. Kliknij pozycję **Dodaj**. Aktualizacja zostanie dodana do koszyka.
5. Wyszukaj wszystkie dodatkowe poprawki wymienione w powyższej tabeli (**3121900**, **3080728**, **3090322**, i **3121899**) i Dodaj każdy koszyk.
6. Kliknij pozycję **Wyświetl koszyk**.
7. Kliknij pozycję **Pobierz**. Określ lokalizację lokalną, do której mają trafiać pobrane pliki, albo **przejdź** do takiej lokalizacji. Aktualizacje zostaną pobrane do wskazanej lokalizacji i umieszczone w podfolderze o nazwie takiej samej jak aktualizacja. Folder można też skopiować do udziału sieciowego osiągalnego z urządzenia.

> [!NOTE]
> Poprawki musi być dostępny z obu kontrolerów, aby wykryć potencjalne komunikaty o błędach z kontrolera elementu równorzędnego.
> 
> 

#### <a name="to-install-and-verify-regular-mode-hotfixes"></a>Aby zainstalować i zweryfikować poprawki przeznaczone do trybu normalnego
Wykonaj następujące kroki, aby zainstalować i zweryfikować poprawki przeznaczone do trybu normalnego. Jeśli została już zainstalowana ich przy użyciu portalu Azure, przejdź do [zainstalowany i sprawdź poprawki trybu konserwacji](#to-install-and-verify-maintenance-mode-hotfixes).

1. Aby zainstalować poprawki, przejdź do interfejsu programu Windows PowerShell na konsoli szeregowej Twojego urządzenia StorSimple. Postępuj zgodnie ze szczegółowymi instrukcjami w części [Nawiązywanie połączenia z konsolą szeregową urządzenia przy użyciu programu PuTTY](../articles/storsimple/storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console). W wierszy polecenia naciśnij klawisz **Enter**.
2. Wybierz **opcję 1**, aby zalogować się do urządzenia z pełnym dostępem.
3. Aby zainstalować poprawkę, w wierszu polecenia wpisz:
   
    `Start-HcsHotfix -Path <path to update file> -Credential <credentials in domain\username format>`
   
    W powyższym poleceniu użyj adresu IP zamiast nazwy DNS w ścieżce udziału. Parametr Credential jest używany tylko wtedy, gdy uzyskuje się dostęp do uwierzytelnionego udziału.
   
    Użycie parametru Credential jest zalecane przy uzyskiwaniu dostępu do udziałów. Nawet udziały otwarte dla wszystkich nie są zwykle otwarte dla użytkowników nieuwierzytelnionych.
   
    Poniżej pokazano przykładowe dane wyjściowe.
   
    ```
    Controller0>Start-HcsHotfix -Path \\10.100.100.100\share
    \hcsmdssoftwareupdate.exe -Credential contoso\John

    Confirm

    This operation starts the hotfix installation and could reboot one or
    both of the controllers. If the device is serving I/Os, these will not
    be disrupted. Are you sure you want to continue?
    [Y] Yes [N] No [?] Help (default is "Y"): Y
    ```
4. Wpisz **Y**, gdy zostanie wyświetlony monit o potwierdzenie instalacji poprawki.
5. Monitoruj aktualizację za pomocą polecenia cmdlet `Get-HcsUpdateStatus`.
   
    Następujące przykładowe dane wyjściowe pokazują aktualizację w toku. Gdy aktualizacja będzie w toku, parametr `RunInprogress` będzie mieć wartość `True`.
   
    ```
    Controller0>Get-HcsUpdateStatus
    RunInprogress       : True
    LastHotfixTimestamp : 12/21/2015 10:36:13 PM
    LastUpdateTimestamp : 12/21/2015 10:35:25 PM
    Controller0Events   :
    Controller1Events   :
    ```
   
     Następujące przykładowe dane wyjściowe wskazują, że aktualizacja została zakończona. Gdy aktualizacja zostanie zakończona, parametr `RunInProgress` będzie mieć wartość `False`.
   
    ```
    Controller1>Get-HcsUpdateStatus

    RunInprogress       : False
    LastHotfixTimestamp : 12/21/2015 10:59:13 PM
    LastUpdateTimestamp : 12/21/2015 10:35:25 PM
    Controller0Events   :
    Controller1Events   :
    ```
   
   > [!NOTE]
   > Czasem polecenie cmdlet zgłasza wartość `False`, gdy aktualizacja jest ciągle w toku. Aby upewnić się, że instalacja poprawki została zakończona, zaczekaj kilka minut, uruchom ponownie to polecenie i sprawdź, czy parametr `RunInProgress` ma wartość `False`. Jeśli tak jest, instalowanie poprawki zostało zakończone.

6. Po ukończeniu aktualizacji oprogramowania, powtórz kroki 3 – 5, aby zainstalować i monitorować agenta SaaS i agenta usług MDS. Upewnij się, że `all-hcsmdssoftwareupdate_0b438ddf0d5b686aada2378b754fac8c7f2160e9.exe` została zainstalowana przed `all-cismdsagentupdatebundle_f98e62f4d56c79e2a6644d027af7a2393a93827a.exe`.
7. Sprawdź system wersje oprogramowania. Wpisz:
   
    `Get-HcsSystem`
   
    Powinny zostać wyświetlone następujące wersje:
   
   * HcsSoftwareVersion: 6.3.9600.17673
   * CisAgentVersion: 1.0.9150.0
   * MdsAgentVersion: 30.0.4698.13
     
     Po zastosowaniu tej aktualizacji nie należy zmieniać numerów wersji, wskazuje, że poprawka nie zastosować. Jeśli widzisz coś takiego, skontaktuj się z [pomocą techniczną firmy Microsoft](../articles/storsimple/storsimple-contact-microsoft-support.md) w celu uzyskania dalszej pomocy.
8. Powtórz kroki 3 – 5, aby zainstalować pozostałe poprawki tryb zwykły.
   
   * Sterownik LSI - KB3121900
   * Aktualizacja Storport - KB3080728
   * Aktualizacja Spaceport - KB3090322

#### <a name="to-install-and-verify-maintenance-mode-hotfixes"></a>Aby zainstalować i zweryfikować poprawki przeznaczone do trybu konserwacji
Użyj KB3121899 do instalowania aktualizacji oprogramowania układowego dysku. Te aktualizacje wymagają zatrzymania pracy i zajmują około 30 minut. Można zdecydować się na zainstalowanie ich w zaplanowanym oknie obsługi, łącząc się z konsolą szeregową urządzenia.

Pamiętaj, że jeśli oprogramowanie układowe dysku jest już aktualne, tych aktualizacji nie trzeba instalować. Uruchom polecenie cmdlet `Get-HcsUpdateAvailability` z konsoli szeregowej urządzenia, aby sprawdzić, czy aktualizacje są dostępne i czy wymagają przerwania pracy (są przeznaczone do trybu konserwacji), czy nie (są przeznaczone do trybu normalnego).

Aby zainstalować aktualizacje oprogramowania układowego dysku, postępuj zgodnie z instrukcjami poniżej.

1. Ustaw urządzenia w trybie konserwacji. Należy pamiętać, że nie należy używać komunikacji zdalnej programu Windows PowerShell podczas nawiązywania połączenia z urządzeniem w trybie konserwacji. Zamiast tego Uruchom to polecenie cmdlet na kontrolerze urządzenia podczas połączenia za pośrednictwem konsoli szeregowej urządzenia. Wpisz:
   
    `Enter-HcsMaintenanceMode`
   
    Poniżej pokazano przykładowe dane wyjściowe.
   
        Controller0>Enter-HcsMaintenanceMode
        Checking device state...
   
        In maintenance mode, your device will not service IOs and will be disconnected from the Microsoft Azure StorSimple Manager service. Entering maintenance mode will end the current session and reboot both controllers, which takes a few minutes to complete. Are you sure you want to enter maintenance mode?
        [Y] Yes [N] No (Default is "Y"): Y
   
        -----------------------MAINTENANCE MODE------------------------
        Microsoft Azure StorSimple Appliance Model 8100
        Name: Update2-8100-SHG0997879L76YD
        Software Version: 6.3.9600.17664
        Copyright (C) 2014 Microsoft Corporation. All rights reserved.
        You are connected to Controller0 - Passive
        ---------------------------------------------------------------
   
        Serial Console Menu
        [1] Log in with full access
        [2] Log into peer controller with full access
        [3] Connect with limited access
        [4] Change language
        Please enter your choice>
   
    Zarówno kontrolery następnie uruchom ponownie w trybie konserwacji.
2. Aby zainstalować aktualizację oprogramowania układowego dysku, wpisz:
   
    `Start-HcsHotfix -Path <path to update file> -Credential <credentials in domain\username format>`
   
    Poniżej pokazano przykładowe dane wyjściowe.
   
        Controller1>Start-HcsHotfix -Path \\10.100.100.100\share\DiskFirmwarePackage.exe -Credential contoso\john
        Enter Password:
        WARNING: In maintenance mode, hotfixes should be installed on each controller sequentially. After the hotfix is installed on this controller, install it on the peer controller.
        Confirm
        This operation starts a hotfix installation and could reboot one or both of the controllers. By installing new updates you agree to, and accept any additional terms associated with, the new functionality listed in the release notes (https://go.microsoft.com/fwLink/?LinkID=613790). Are you sure you want to continue?
        [Y] Yes [N] No (Default is "Y"): Y
        WARNING: Installation is currently in progress. This operation can take several minutes to complete.
3. Monitoruj postęp instalacji za pomocą polecenia `Get-HcsUpdateStatus`. Aktualizacja będzie zakończona, gdy parametr `RunInProgress` zmieni wartość na `False`.
4. Po zakończeniu instalacji kontroler, na którym została zainstalowana poprawka przeznaczona do trybu konserwacji, zostanie uruchomiony ponownie. Zaloguj się za pomocą opcji 1 (z pełnym dostępem) i sprawdź wersję oprogramowania układowego dysku. Wpisz:
   
   `Get-HcsFirmwareVersion`
   
   Oczekiwane wersje oprogramowania układowego dysku to:
   
   `XMGG, XGEG, KZ50, F6C2, VR08`
   
   Poniżej pokazano przykładowe dane wyjściowe.
   
       -----------------------MAINTENANCE MODE------------------------
       Microsoft Azure StorSimple Appliance Model 8100
       Name: Update2-8100-SHG0997879L76YD
       Software Version: 6.3.9600.17664
       Copyright (C) 2014 Microsoft Corporation. All rights reserved.
       You are connected to Controller1
       ---------------------------------------------------------------
   
       Controller1>Get-HcsFirmwareVersion
   
       Controller0 : TalladegaFirmware
         ActiveBIOS:0.45.0006
         BackupBIOS:0.45.0008
         MainCPLD:17.0.0005
         ActiveBMCRoot:2.0.000E
         BackupBMCRoot:2.0.000E
         BMCBoot:2.0.0001
         LsiFirmware:19.00.00.00
         LsiBios:07.37.00.00
         Battery1Firmware:06.29
         Battery2Firmware:06.29
         DomFirmware:X231600
         CanisterFirmware:3.5.0.32
         CanisterBootloader:5.03
         CanisterConfigCRC:0xD1B030A4
         CanisterVPDStructure:0x06
         CanisterGEMCPLD:0x17
         CanisterVPDCRC:0xEE3504B4
         MidplaneVPDStructure:0x0C
         MidplaneVPDCRC:0xA6BD4F64
         MidplaneCPLD:0x10
         PCM1Firmware:1.00|1.05
         PCM1VPDStructure:0x05
         PCM1VPDCRC:0x41BEF99C
         PCM2Firmware:1.00|1.05
         PCM2VPDStructure:0x05
         PCM2VPDCRC:0x41BEF99C
   
         DisksFirmware
         SEAGATE:ST400FM0073:XGEG
         SEAGATE:ST400FM0073:XGEG
         SEAGATE:ST400FM0073:XGEG
         SEAGATE:ST400FM0073:XGEG
         SEAGATE:ST4000NM0023:XMGG
         SEAGATE:ST4000NM0023:XMGG
         SEAGATE:ST4000NM0023:XMGG
         SEAGATE:ST4000NM0023:XMGG
         SEAGATE:ST4000NM0023:XMGG
         SEAGATE:ST4000NM0023:XMGG
         SEAGATE:ST4000NM0023:XMGG
         SEAGATE:ST4000NM0023:XMGG
   
    Uruchom polecenie `Get-HcsFirmwareVersion` na drugim kontrolerze, aby sprawdzić, czy wersja oprogramowania została zaktualizowana. Następnie możesz wyjść z trybu konserwacji. W tym celu wpisz następujące polecenie dla każdego kontrolera urządzenia:
   
   `Exit-HcsMaintenanceMode`
5. Kontrolery Uruchom ponownie po wyjściu z trybu konserwacji. Gdy aktualizacje oprogramowania układowego dysku zostaną pomyślnie zastosowane i urządzenie wyjdzie z trybu konserwacji, wróć do klasycznej witryny Azure Portal. Należy pamiętać, że portalu nie mogą być wyświetlane, czy zainstalowane aktualizacje trybu konserwacji na 24 godziny.

