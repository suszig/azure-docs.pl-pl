### <a name="azure-powershell"></a>Azure PowerShell

#### <a name="install-powershell"></a>Instalowanie programu PowerShell
Zainstaluj najnowszy program PowerShell, jeśli nie masz go na swoim komputerze. 

1. W przeglądarce internetowej przejdź do strony [plików do pobrania platformy Azure](https://azure.microsoft.com/downloads/). 
2. Kliknij pozycję **Wersja instalacyjna dla systemu Windows** w sekcji **Narzędzia wiersza polecenia** -> **PowerShell**. 
3. Aby zainstalować program PowerShell, uruchom plik **MSI**. 

Aby uzyskać szczegółowe informacje, zobacz [How to install and configure Azure PowerShell (Jak zainstalować i skonfigurować program Azure PowerShell)](/powershell/azure/install-azurerm-ps). 

#### <a name="log-in-to-powershell"></a>Logowanie do programu PowerShell

1. Uruchom program **PowerShell** na maszynie. Nie zamykaj programu PowerShell aż do końca tego samouczka Szybki start. Jeśli go zamkniesz i otworzysz ponownie, musisz uruchomić te polecenia jeszcze raz.

    ![Uruchamianie programu PowerShell](media/data-factory-quickstart-prerequisites-2/search-powershell.png)
1. Uruchom poniższe polecenie i wprowadź tę samą nazwę użytkownika platformy Azure oraz hasło, których używasz do logowania się w witrynie Azure Portal:
       
    ```powershell
    Login-AzureRmAccount
    ```        
2. Uruchom poniższe polecenie, aby wyświetlić wszystkie subskrypcje dla tego konta:

    ```powershell
    Get-AzureRmSubscription
    ```
3. Jeśli z kontem jest skojarzonych wiele subskrypcji, uruchom poniższe polecenie, aby wybrać subskrypcję, z którą chcesz pracować. Zastąp parametr **SubscriptionId** identyfikatorem Twojej subskrypcji platformy Azure:

    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<SubscriptionId>"       
    ```
