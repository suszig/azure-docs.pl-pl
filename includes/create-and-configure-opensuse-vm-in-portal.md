1. Zaloguj się w witrynie [Azure Portal](http://portal.azure.com).  
2. Kliknij przycisk **nowy** > **obliczeniowe** > **Zobacz wszystkie** , a następnie wpisz **SUSE**.
   
3. Wybieranie obrazu maszyny wirtualnej OpenSUSE, a następnie kliknij strzałkę, aby kontynuować.
5. W pierwszym **konfiguracji maszyny wirtualnej** strony:
   
   * Wpisz **nazwę maszyny wirtualnej**, takie jak "testlinuxvm". Nazwa musi zawierać od 3 do 15 znaków, może zawierać tylko litery, cyfry i łączniki, musi zaczynać się literą i kończyć literą lub cyfrą.
   * Sprawdź **warstwy** i wybierz **rozmiar**. Warstwy Określa rozmiary, do których są dostępne. Rozmiar wpływa kosztów, a także opcje konfiguracji, takich jak dane jak wiele dysków, które można dołączyć. Aby uzyskać więcej informacji, zobacz [rozmiary maszyn wirtualnych](../articles/virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
   * Wpisz **nową nazwę użytkownika**, lub zaakceptuj wartość domyślną, **azureuser**. Ta nazwa jest dodawana do pliku listy Sudoers.
   * Zdecyduj, którego typ **uwierzytelniania** do użycia. Hasło ogólne wskazówki, zobacz [silne hasła](http://msdn.microsoft.com/library/ms161962.aspx).
6. Przy następnym **konfiguracji maszyny wirtualnej** strony:
   
   * Użyj domyślnej **Utwórz nową usługę w chmurze**.
   * W **nazwy DNS** wpisz unikatową nazwę DNS ma być używana jako część adresu, na przykład "testlinuxvm".
   * W **Region/koligacji grupy/wirtualnej sieci** wybierz region, w którym będzie hostowana ten obraz wirtualnej.
   * W obszarze **punkty końcowe**, zachować punkt końcowy SSH. Można dodać inne teraz, lub Dodaj, Zmień lub usuń je, po utworzeniu maszyny wirtualnej.
     
     > [!NOTE]
     > Jeśli chcesz użyć sieci wirtualnej maszyny wirtualnej można **musi** sieci wirtualnej można określić podczas tworzenia maszyny wirtualnej. Nie można dodać maszyny wirtualnej do sieci wirtualnej, po utworzeniu maszyny wirtualnej. Aby uzyskać więcej informacji, zobacz [omówienie sieci wirtualnych](../articles/virtual-network/virtual-networks-overview.md).
     > 
     > 
7. W ciągu ostatnich **konfiguracji maszyny wirtualnej** strony, zachować ustawienia domyślne, a następnie kliknij znacznik wyboru, aby zakończyć.

Portal Wyświetla nowej maszyny wirtualnej w ramach **maszyn wirtualnych**. Gdy stan został zgłoszony jako **(Inicjowanie obsługi administracyjnej)**, trwa konfigurowanie maszyny wirtualnej. Jeśli stan jest zgłaszany jako **systemem**, możesz przejść do następnego kroku.

## <a name="connect-to-the-virtual-machine"></a>Połącz z maszyną wirtualną
Do nawiązania połączenia dla maszyny wirtualnej, w zależności od systemu operacyjnego na komputerze, na którą będzie łączyć z użyjesz SSH lub PuTTY:

* Z komputera z systemem Linux używanie protokołu SSH. W wierszu polecenia wpisz:
  
    `$ ssh newuser@testlinuxvm.cloudapp.net -o ServerAliveInterval=180`
  
    Wpisz hasło użytkownika.
* Z komputera z systemem Windows przy użyciu programu PuTTY. Jeśli użytkownik nie jest zainstalowany, pobierz go z [strony pobierania programu PuTTY][PuTTYDownload].
  
    Zapisz **putty.exe** do katalogu na komputerze. Otwórz wiersz polecenia, przejdź do folderu, a następnie uruchom **putty.exe**.
  
    Wpisz nazwę hosta, taką jak "testlinuxvm.cloudapp.net" i wpisz "22" **portu**.
  
    ![Ekran programu puTTY][Image6]  

## <a name="update-the-virtual-machine-optional"></a>Zaktualizuj maszynę wirtualną (opcjonalnie)
1. Po połączeniu się z maszyną wirtualną, można opcjonalnie zainstalować poprawki i aktualizacje systemu. Aby uruchomić aktualizację, wpisz:
   
    `$ sudo zypper update`
2. Wybierz **oprogramowania**, następnie **aktualizacji w trybie Online** Aby wyświetlić listę dostępnych aktualizacji. Wybierz **Akceptuj** aby rozpocząć instalację i zastosować nowe wszystkie dostępne poprawki (z wyjątkiem pól opcjonalnych).
3. Po zakończeniu instalacji wybierz **Zakończ**.  System jest już aktualny.

[PuTTYDownload]: http://www.puttyssh.org/download.html

[Image6]: ./media/create-and-configure-opensuse-vm-in-portal/putty.png
