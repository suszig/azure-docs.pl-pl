



W zależności od środowiska i dostępnych wyborów skryptu można utworzyć wszystkich infrastruktury klastra, łącznie z sieci wirtualnej platformy Azure, konta magazynu, usługi w chmurze, kontroler domeny, baz danych lokalnych lub zdalnych, węzła głównego i dodatkowe węzły klastra. Alternatywnie skryptu można użyć istniejącej infrastruktury platformy Azure i utworzyć węzłów klastra HPC.

Aby uzyskać informacje dotyczące planowania klastra HPC Pack, zobacz [wersja ewaluacyjna produktu i planowanie](https://technet.microsoft.com/library/jj899596.aspx) i [wprowadzenie](https://technet.microsoft.com/library/jj899590.aspx) zawartości w bibliotece TechNet HPC Pack 2012 R2.

## <a name="prerequisites"></a>Wymagania wstępne
* **Subskrypcja platformy Azure**: subskrypcji można użyć w usłudze Azure globalne lub chińskiej wersji platformy Azure. Swoje limity subskrypcji wpłynąć na liczbę i rodzaj węzłach klastra, którą można wdrożyć. Aby uzyskać informacje, zobacz [subskrypcji platformy Azure i usługi limity, przydziały i ograniczenia](../articles/azure-subscription-service-limits.md).
* **Komputer kliencki z systemem Windows przy użyciu programu Azure PowerShell 0.8.10 lub nowszy zainstalowany i skonfigurowany**: zobacz [wprowadzenie do programu Azure PowerShell](/powershell/azureps-cmdlets-docs) instrukcje dotyczące instalacji i kroki nawiązać połączenia z subskrypcją platformy Azure.
* **Skrypt wdrożenia HPC Pack IaaS**: Pobierz i Rozpakuj najnowszej wersji skryptu z [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949). Sprawdź wersję skryptu, uruchamiając `New-HPCIaaSCluster.ps1 –Version`. W tym artykule jest oparty na wersji 4.5.2 skryptu.
* **Konfiguracja pliku**: Utwórz plik XML, który używa skryptu do konfiguracji klastra HPC. Informacje i przykłady zobacz sekcje w dalszej części tego artykułu i plik Manual.rtf, który towarzyszy skrypt wdrożenia.

## <a name="syntax"></a>Składnia
```PowerShell
New-HPCIaaSCluster.ps1 [-ConfigFile] <String> [-AdminUserName]<String> [[-AdminPassword] <String>] [[-HPCImageName] <String>] [[-LogFile] <String>] [-Force] [-NoCleanOnFailure] [-PSSessionSkipCACheck] [<CommonParameters>]
```
> [!NOTE]
> Uruchom skrypt jako administrator.
> 
> 

### <a name="parameters"></a>Parametry
* **ConfigFile**: Określa ścieżkę pliku konfiguracji do opisywania klastra HPC. Zobacz więcej informacji na temat pliku konfiguracji, w tym temacie lub w pliku Manual.rtf w folderze zawierających skrypt.
* **AdminUserName**: Określa nazwę użytkownika. Jeśli w lesie domeny został utworzony przez skrypt, staje się on nazwą użytkownika administratora lokalnego dla wszystkich maszyn wirtualnych i nazwę administratora domeny. Jeśli w lesie domeny już istnieje, określa użytkownika domeny jako nazwa użytkownika administratora lokalnego do zainstalowania pakietu HPC.
* **AdminPassword**: Określa hasło administratora. Jeśli nie zostanie określony w wierszu polecenia, skrypt wyświetli monit o wprowadzania hasła.
* **HPCImageName** (opcjonalnie): Określa nazwę obrazu maszyny Wirtualnej programu HPC Pack umożliwia wdrożenie klastra HPC. Musi być obrazem udostępnionych przez firmę Microsoft HPC Pack z portalu Azure Marketplace. Jeśli nie zostanie określony (zwykle zalecane), skrypt wybierze najnowszych opublikowanych [obrazu HPC Pack 2012 R2](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/). Najnowsze obraz jest oparty na Windows Server 2012 R2 Datacenter z HPC Pack 2012 R2 Update 3.
  
  > [!NOTE]
  > Wdrożenie zakończy się niepowodzeniem, jeśli nie określisz prawidłowym obrazem HPC Pack.
  > 
  > 
* **Plik dziennika** (opcjonalnie): Określa ścieżkę pliku dziennika wdrażania. Jeśli nie zostanie określony, skrypt tworzy plik dziennika w katalogu tymczasowym komputera z uruchomionym skryptu.
* **Wymuś** (opcjonalnie): pomija wszystkie o potwierdzenie.
* **NoCleanOnFailure** (opcjonalnie): Określa, czy maszynach wirtualnych platformy Azure, które nie zostały pomyślnie wdrożone nie zostaną usunięte. Ręcznie usuń te maszyny wirtualne przed uruchomienie skryptu, aby kontynuować wdrażanie lub wdrożenie może się nie powieść.
* **PSSessionSkipCACheck** (opcjonalnie): dla każdej usługi w chmurze z maszyn wirtualnych wdrożonych przez ten skrypt certyfikatu z podpisem własnym jest generowany automatycznie przez usługę Azure i wszystkich maszyn wirtualnych w usłudze w chmurze ten certyfikat będzie używany jako domyślny zdalnego systemu Windows Certyfikat zarządzania (WinRM). Aby wdrożenie funkcji HPC na tych maszynach wirtualnych Azure, skrypt domyślnie po tymczasowo zainstaluje tych certyfikatów na komputerze lokalnym\\magazynu Zaufane główne urzędy certyfikacji na komputerze klienckim, aby pominąć błąd zabezpieczeń "zaufany urząd certyfikacji" Podczas wykonywania skryptu. Certyfikaty zostaną usunięte po zakończeniu działania skryptu. Jeśli ten parametr jest określony, certyfikaty nie są zainstalowane na komputerze klienckim i ostrzeżenie o zabezpieczeniach jest pomijane.
  
  > [!IMPORTANT]
  > Ten parametr nie jest zalecane w przypadku wdrożeń produkcyjnych.
  > 
  > 

### <a name="example"></a>Przykład
Poniższy przykład tworzy klastra HPC Pack przy użyciu pliku konfiguracji *MyConfigFile.xml*i określa poświadczenia administratora dla instalowania klastra.

```PowerShell
.\New-HPCIaaSCluster.ps1 –ConfigFile MyConfigFile.xml -AdminUserName <username> –AdminPassword <password>
```

### <a name="additional-considerations"></a>Dodatkowe zagadnienia
* Skrypt opcjonalnie umożliwiają przesyłanie zadań za pomocą portalu internetowego HPC Pack lub interfejsu API REST HPC Pack.
* Skrypt Opcjonalnie można uruchamianie niestandardowych skryptów przed i po konfiguracji w węźle głównym, jeśli chcesz zainstalować dodatkowe oprogramowanie lub skonfigurować inne ustawienia.

## <a name="configuration-file"></a>Plik konfiguracji
Plik konfiguracji do skryptu wdrażania jest plik XML. Plik schematu HPCIaaSClusterConfig.xsd znajduje się w folderze skryptów wdrażania HPC Pack IaaS. **IaaSClusterConfig** jest elementem głównym pliku konfiguracji, który zawiera elementy podrzędne, które opisano szczegółowo w pliku Manual.rtf w folderze skryptów wdrażania.

