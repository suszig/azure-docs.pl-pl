# <a name="how-to-use-the-azure-command-line-tools-for-mac-and-linux"></a>Jak używać narzędzi wiersza polecenia platformy Azure dla systemów Mac i Linux
Ten przewodnik opisuje sposób korzystania z narzędzi wiersza polecenia platformy Azure dla systemów Mac i Linux do tworzenia usług na platformie Azure i zarządzania nimi. Uwzględnione scenariusze obejmują **instalowanie narzędzi**, **importowanie ustawień publikowania**, **tworzenie witryn Azure Websites i zarządzanie nimi** oraz **tworzenie maszyn wirtualnych Azure Virtual Machines i zarządzanie nimi**. Aby zapoznać się z kompleksową dokumentacją referencyjną, zobacz [Azure command-line tool for Mac and Linux Documentation][reference-docs] (Dokumentacja narzędzia wiersza polecenia platformy Azure dla systemów Mac i Linux). 

## <a name="table-of-contents"></a>Spis treści
* [Jakie narzędzia wiersza polecenia platformy Azure są dostępne dla systemów Mac i Linux](#Overview)
* [Jak zainstalować narzędzia wiersza polecenia platformy Azure dla systemów Mac i Linux](#Download)
* [Jak utworzyć konto platformy Azure](#CreateAccount)
* [Jak pobrać i zaimportować ustawienia publikowania](#Account)
* [Jak utworzyć witrynę sieci Web platformy Azure i zarządzać nią](#WebSites)
* [Jak utworzyć maszynę wirtualną platformy Azure i zarządzać nią](#VMs)

<h2><a id="Overview"></a>Jakie narzędzia wiersza polecenia platformy Azure są dostępne dla systemów Mac i Linux</h2>

Narzędzia wiersza polecenia platformy Azure dla systemów Mac i Linux to zestaw narzędzi pozwalających na wdrażanie usług Azure i zarządzanie nimi.

Obsługiwane zadania to m.in.:

* Importowanie ustawień publikowania.
* Tworzenie witryn Azure Websites i zarządzanie nimi.
* Tworzenie maszyn wirtualnych Azure Virtual Machines i zarządzanie nimi.

Aby zapoznać się z pełną listą obsługiwanych poleceń, wpisz polecenie `azure -help` w wierszu polecenia po zainstalowaniu narzędzi lub zobacz [dokumentację referencyjną][reference-docs].

<h2><a id="Download">Jak zainstalować narzędzia wiersza polecenia platformy Azure dla systemów Mac i Linux</a></h2>

Poniższa lista zawiera informacje dotyczące instalowania narzędzi wiersza polecenia, w zależności od systemu operacyjnego:

* **Mac**: Pobierz [instalator zestawu Azure SDK][mac-installer]. Otwórz pobrany plik pkg i wykonaj kroki instalacji zgodnie z wyświetlanymi monitami.
* **Linux**: Zainstaluj najnowszą wersję środowiska [Node.js][nodejs-org], zobacz [Install Node.js via Package Manager][install-node-linux] (Instalowanie środowiska Node.js przy użyciu menedżera pakietów), i uruchom następujące polecenie:
  
        npm install azure-cli -g
  
    **Uwaga**: może być konieczne uruchomienie tego polecenia z podwyższonym poziomem uprawnień:
  
        sudo npm install azure-cli -g
* **Windows**: uruchom instalator systemu Windows (plik msi), który jest dostępny tutaj: [narzędzia wiersza polecenia platformy Azure][windows-installer].

Aby przetestować instalację, wpisz polecenie `azure` w wierszu polecenia. Jeśli instalacja się powiodła, zostanie wyświetlona lista wszystkich dostępnych poleceń `azure`.

<h2><a id="CreateAccount"></a>Jak utworzyć konto platformy Azure</h2>

Aby używać narzędzi wiersza polecenia platformy Azure dla systemów Mac i Linux, potrzebne jest konto platformy Azure.

Otwórz przeglądarkę sieci Web, przejdź do witryny [http://www.windowsazure.com][windowsazuredotcom] i kliknij pozycję **Bezpłatne konto** w prawym górnym rogu.

![Witryna sieci Web platformy Azure][Azure Web Site]

Postępuj zgodnie z instrukcjami dotyczącymi tworzenia konta.

<h2><a id="Account"></a>Jak pobrać i zaimportować ustawienia publikowania</h2>

Aby rozpocząć pracę, należy najpierw pobrać i zaimportować swoje ustawienia publikowania. Dzięki temu będzie można korzystać z narzędzi do tworzenia usług Azure i zarządzania nimi. Aby pobrać swoje ustawienia publikowania, użyj polecenia `account download`:

    azure account download

Spowoduje to otworzenie domyślnej przeglądarki i wyświetlenie monitu o zalogowanie się do portalu zarządzania. Po zalogowaniu się Twój plik `.publishsettings` zostanie pobrany. Zanotuj miejsce, w którym plik został zapisany.

Następnie zaimportuj plik `.publishsettings`, uruchamiając następujące polecenie, przy czym zmienną `{path to .publishsettings file}` zastąp ścieżką do Twojego pliku `.publishsettings`:

    azure account import {path to .publishsettings file}

Wszystkie informacje zapisane przez polecenie <code>import</code> możesz usunąć za pomocą polecenia <code>account clear</code>:

    azure account clear

Aby zobaczyć listę opcji poleceń `account`, skorzystaj z opcji `-help`:

    azure account -help

Po zaimportowaniu ustawień publikowania ze względów bezpieczeństwa usuń plik `.publishsettings`.

> [!NOTE]
> Podczas importowania ustawień publikowania poświadczenia umożliwiające uzyskanie dostępu do Twojej subskrypcji platformy Azure są zapisywane w folderze `user`. Folder `user` jest chroniony przez system operacyjny. Zaleca się jednak wykonanie dodatkowych kroków w celu zaszyfrowania folderu `user`. Można to zrobić na następujące sposoby:    
> 
> * W systemie Windows zmodyfikuj właściwości folderu lub użyj funkcji BitLocker.
> * W systemie Mac włącz funkcję FileVault dla folderu.
> * W systemie Ubuntu skorzystaj z funkcji zaszyfrowanego katalogu głównego. Inne dystrybucje systemu Linux oferują równoważne funkcje.
> 
> 

Teraz możesz przystąpić do tworzenia witryn Azure Websites i maszyn wirtualnych Azure Virtual Machines oraz zarządzania nimi.  

<h2><a id="WebSites"></a>Jak utworzyć witrynę sieci Web platformy Azure i zarządzać nią</h2>

### <a name="create-a-website"></a>Tworzenie witryny sieci Web
Aby utworzyć witrynę sieci Web platformy Azure, najpierw utwórz pusty katalog o nazwie `MySite` i przejdź do tego katalogu.

Następnie uruchom poniższe polecenie:

    azure site create MySite --git

Dane wyjściowe tego polecenia będą zawierać domyślny adres URL dla nowo utworzonej witryny sieci Web. Opcja `--git` pozwala na używanie sytemu Git do publikowania w witrynie sieci Web dzięki utworzeniu repozytoriów Git zarówno w lokalnym katalogu aplikacji, jak i w centrum danych witryny sieci Web. Zauważ, że jeśli folder lokalny już jest repozytorium Git, polecenie spowoduje dodanie do istniejącego repozytorium nowego repozytorium zdalnego wskazującego repozytorium w centrum danych witryny sieci Web.

Polecenie `azure site create` można wykonywać z dowolną z następujących opcji:

* `--location [location name]`. Ta opcja umożliwia określenie lokalizacji centrum danych, w którym zostanie utworzona witryna sieci Web (np. „Zachodnie stany USA”). W przypadku pominięcia tej opcji zostanie wyświetlony monit, aby wybrać lokalizację.
* `--hostname [custom host name]`. Ta opcja pozwala określić niestandardową nazwę hosta dla witryny sieci Web.

Następnie możesz dodać zawartość do katalogu witryny sieci Web. Użyj zwykłego przepływu Git (`git add`, `git commit`) do zatwierdzenia zawartości. Za pomocą następującego polecenia Git wypchnij zawartość witryny sieci Web na platformę Azure: 

    git push azure master

### <a name="set-up-publishing-from-github"></a>Konfigurowanie publikowania z usługi GitHub
Aby skonfigurować publikowanie ciągłe z repozytorium GitHub, użyj opcji `--GitHub` podczas tworzenia witryny:

    auzre site create MySite --github --githubusername username --githubpassword password --githubrepository githubuser/reponame

Jeśli masz lokalnego klona repozytorium GitHub lub repozytorium z jednym odwołaniem zdalnym do repozytorium GitHub, to polecenie automatycznie opublikuje kod w repozytorium GitHub w Twojej witrynie. Od tego momentu wszystkie zmiany wypychane do repozytorium GitHub będą automatycznie publikowane w witrynie.

Po skonfigurowaniu publikowania z usługi GitHub domyślnie używana gałąź to gałąź główna. Aby określić inną gałąź, wykonaj następujące polecenie w repozytorium lokalnym:

    azure site repository <branch name>

### <a name="configure-app-settings"></a>Konfigurowanie ustawień aplikacji
Ustawienia aplikacji to pary klucz-wartość, które są dostępne dla aplikacji w czasie wykonywania. Określenie wartości ustawień aplikacji dla witryny sieci Web platformy Azure spowoduje zastąpienie ustawień zdefiniowanych w pliku Web.config witryny, które mają ten sam klucz. W przypadku aplikacji Node.js i PHP ustawienia aplikacji są dostępne jako zmienne środowiskowe. W poniższym przykładzie przedstawiono sposób ustawiania pary klucz-wartość:

    azure site config add <key>=<value> 

Aby wyświetlić listę wszystkich par klucz-wartość, użyj następującego polecenia:

    azure site config list 

Jeśli znasz klucz i chcesz wyświetlić wartość, możesz także użyć polecenia:

    azure site config get <key> 

Aby zmienić wartość istniejącego klucza, musisz najpierw wyczyścić istniejący klucz, a następnie ponownie go dodać. Polecenie czyszczenia jest następujące:

    azure site config clear <key> 

### <a name="list-and-show-sites"></a>Wyświetlanie listy witryn i informacji o witrynach
Aby wyświetlić listę witryn sieci Web, użyj następującego polecenia:

    azure site list

Aby uzyskać szczegółowe informacje dotyczące witryny, użyj polecenia `site show`. W poniższym przykładzie przedstawiono szczegółowe informacje dotyczące witryny `MySite`:

    azure site show MySite

### <a name="stop-start-or-restart-a-site"></a>Zatrzymywanie, uruchamianie lub ponowne uruchamianie witryny
Witrynę możesz zatrzymać, uruchomić lub uruchomić ponownie przy użyciu polecenia `site stop`, `site start` lub `site restart`:

    azure site stop MySite
    azure site start MySite
    azure site restart MySite

### <a name="delete-a-site"></a>Usuwanie witryny
Dodatkowo możesz też usunąć witrynę za pomocą polecenia `site delete`:

    azure site delete MySite

Pamiętaj, że jeśli uruchamiasz dowolne z powyższych poleceń z poziomu folderu, w którym uruchomiono polecenie `site create`, nie musisz określać nazwy witryny `MySite` jako ostatniego parametru.

Aby wyświetlić pełną listę poleceń `site`, podaj opcję `-help`:

    azure site -help 

<h2><a id="VMs"></a>Jak utworzyć maszynę wirtualną platformy Azure i zarządzać nią</h2>

Maszyna wirtualna platformy Azure jest tworzona z obrazu maszyny wirtualnej (pliku vhd) udostępnionego przez Ciebie lub dostępnego w galerii obrazów. Aby wyświetlić dostępne obrazy, użyj polecenia `vm image list`:

    azure vm image list

Aby aprowizować i uruchomić maszynę wirtualną na podstawie jednego z dostępnych obrazów, skorzystaj z polecenia `vm create`. Poniższy przykład przedstawia sposób tworzenia maszyny wirtualnej z systemem Linux (o nazwie `myVM`) na podstawie obrazu pochodzącego z galerii obrazów (CentOS 6.2). Nazwa i hasło użytkownika głównego maszyny wirtualnej to odpowiednio `myusername` i `Mypassw0rd`. (Pamiętaj, że parametr `--location` określa centrum danych, w którym maszyna wirtualna zostanie utworzona. W przypadku pominięcia parametru `--location` zostanie wyświetlony monit, aby wybrać lokalizację).

    azure vm create myVM OpenLogic__OpenLogic-CentOS-62-20120509-en-us-30GB.vhd myusername --location "West US"

Opcjonalnie możesz przekazać flagę `--ssh` (Linux) lub `--rdp` (Windows) do polecenia `vm create`, aby włączyć połączenia zdalne z nowo utworzoną maszyną wirtualną.

Jeśli wolisz aprowizować maszynę wirtualną z obrazu niestandardowego, możesz utworzyć obraz z pliku vhd za pomocą polecenia `vm image create`, a następnie skorzystać z polecenia `vm create`, aby aprowizować maszynę wirtualną. Poniższy przykład przedstawia sposób tworzenia obrazu systemu Linux (o nazwie `myImage`) z lokalnego pliku vhd. (Parametr `--location` określa centrum danych, w którym obraz zostanie zapisany).

    azure vm image create myImage /path/to/myImage.vhd --os linux --location "West US"

Zamiast tworzyć obraz z lokalnego pliku vhd, możesz utworzyć obraz z pliku vhd przechowywanego w usłudze Azure Blob Storage. W tym celu możesz użyć parametru `blob-url`:

    azure vm image create myImage --blob-url <url to .vhd in Blob Storage> --os linux

Po utworzeniu obrazu możesz aprowizować maszynę wirtualną na podstawie obrazu za pomocą polecenia `vm create`. Poniższe polecenie tworzy maszynę wirtualną o nazwie `myVM` z obrazu utworzonego powyżej (`myImage`).

    azure vm create myVM myImage myusername --location "West US"

Po aprowizowaniu maszyny wirtualnej możesz utworzyć punkty końcowe, aby (na przykład) umożliwić uzyskiwanie dostępu zdalnego do maszyny wirtualnej. W poniższym przykładzie użyto polecenia `vm create endpoint`, aby otworzyć zewnętrzny port 22 i lokalny port 22 w maszynie wirtualnej `myVM`:

    azure vm endpoint create myVM 22 22

Za pomocą polecenia `vm show` możesz uzyskać szczegółowe informacje dotyczące maszyny wirtualnej (w tym adres IP, nazwę DNS i informacje o punkcie końcowym):

    azure vm show myVM

Aby zamknąć, uruchomić lub ponownie uruchomić maszynę wirtualną, użyj jednego z następujących poleceń:

    azure vm shutdown myVM
    azure vm start myVM
    azure vm restart myVM

Aby usunąć maszynę wirtualną, użyj polecenia `vm delete`:

    azure vm delete myVM

Aby uzyskać pełną listę poleceń służących do tworzenia maszyn wirtualnych i zarządzania nimi, użyj opcji `-h`:

    azure vm -h

<!-- IMAGES -->
[Azure Web Site]: ./media/crossplat-cmd-tools/freetrial.png

<!-- LINKS -->
[nodejs-org]: http://nodejs.org/
[install-node-linux]: https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager
[mac-installer]: http://go.microsoft.com/fwlink/?LinkId=252249
[windows-installer]: http://go.microsoft.com/fwlink/?LinkID=275464
[reference-docs]: http://go.microsoft.com/fwlink/?LinkId=252246
[windowsazuredotcom]: http://www.windowsazure.com



<!--HONumber=Jan17_HO5-->


