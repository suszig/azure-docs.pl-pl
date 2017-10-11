


## <a name="using-vm-extensions"></a>Przy użyciu rozszerzeń maszyny Wirtualnej
Implementowanie rozszerzeń maszyny Wirtualnej Azure zachowania lub funkcje, które ułatwiają albo inne programy, które działają na maszynach wirtualnych Azure (na przykład **WebDeployForVSDevTest** rozszerzenia umożliwia programowi Visual Studio Web Deploy rozwiązań na maszynie Wirtualnej platformy Azure) lub podaj zdolność umożliwia interakcję z maszyną Wirtualną do obsługi niektóre inne zachowanie (na przykład umożliwia rozszerzenia dostępu do maszyny Wirtualnej z klientów programu PowerShell, interfejsu wiersza polecenia Azure i REST resetowania lub zmodyfikuj wartości dostępu zdalnego na maszynie Wirtualnej platformy Azure).

> [!IMPORTANT]
> Pełną listę rozszerzeń przez funkcje obsługują zobacz [rozszerzeń maszyny Wirtualnej platformy Azure i funkcje](../articles/virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Ponieważ każde rozszerzenie maszyny Wirtualnej obsługuje określonych funkcji, dokładnie co można, a nie z rozszerzeniem jest zależna od rozszerzenia. W związku z tym przed zmodyfikowaniem maszyny Wirtualnej, sprawdź, czy znasz dokumentacji dla rozszerzenia maszyny Wirtualnej, którego chcesz użyć. Usunięcie niektórych rozszerzeń maszyny Wirtualnej nie jest obsługiwany; inne mają właściwości, które można ustawić które znacząco zmieniają zachowanie maszyny Wirtualnej.
> 
> 

Typowe zadania są:

1. Znajdowanie dostępnych rozszerzeń
2. Aktualizacja rozszerzeń załadowany
3. Dodawanie rozszerzeń
4. Usuwanie rozszerzeń

## <a name="find-available-extensions"></a>Znajdowanie dostępnych rozszerzeń
Można znaleźć rozszerzenia i używanie rozszerzonych informacji:

* PowerShell
* Interfejs wiersza polecenia i Platform Azure (Azure CLI)
* Interfejs API protokołu REST zarządzania usługami

### <a name="azure-powershell"></a>Azure PowerShell
Niektóre rozszerzenia zostały poleceń cmdlet programu PowerShell, które są specyficzne dla nich, które mogą ułatwić ich konfiguracji z programu PowerShell; jednak następujące polecenia cmdlet działać dla wszystkich rozszerzeń maszyny Wirtualnej.

Aby uzyskać informacje o dostępnych rozszerzeń służy następujące polecenia cmdlet:

* Dla wystąpienia ról sieci web lub roli proces roboczy, można użyć [Get-AzureServiceAvailableExtension](https://msdn.microsoft.com/library/azure/dn722498.aspx) polecenia cmdlet.
* Dla wystąpień maszyn wirtualnych, można użyć [Get-AzureVMAvailableExtension](https://msdn.microsoft.com/library/azure/dn722480.aspx) polecenia cmdlet.
  
   Na przykład następujący przykładowy kod przedstawia sposób wyświetlania informacji dotyczących **IaaSDiagnostics** rozszerzenia przy użyciu programu PowerShell.
  
      PS C:\> Get-AzureVMAvailableExtension -ExtensionName IaaSDiagnostics
  
      Publisher                   : Microsoft.Azure.Diagnostics
      ExtensionName               : IaaSDiagnostics
      Version                     : 1.2
      Label                       : Microsoft Monitoring Agent Diagnostics
      Description                 : Microsoft Monitoring Agent Extension
      PublicConfigurationSchema   :
      PrivateConfigurationSchema  :
      IsInternalExtension         : False
      SampleConfig                :
      ReplicationCompleted        : True
      Eula                        :
      PrivacyUri                  :
      HomepageUri                 :
      IsJsonExtension             : True
      DisallowMajorVersionUpgrade : False
      SupportedOS                 :
      PublishedDate               :
      CompanyName                 :

### <a name="azure-command-line-interface-azure-cli"></a>Interfejs wiersza polecenia platformy Azure (Azure CLI)
Niektóre rozszerzenia zostały polecenia interfejsu wiersza polecenia Azure, które są specyficzne dla ich (Docker rozszerzenia maszyny Wirtualnej jest jednym z przykładów), która może ułatwić ich konfiguracji; Jednak poniższe polecenia działać dla wszystkich rozszerzeń maszyny Wirtualnej.

Można użyć **listy rozszerzeń maszyny wirtualnej azure** polecenie, aby uzyskać informacje o dostępnych rozszerzeń, a następnie użyj **—-json** opcję, aby wyświetlić wszystkie dostępne informacje o jedno lub więcej rozszerzeń. Jeśli nie używasz rozszerzenie nazwy, polecenie zwraca opisu JSON wszystkie dostępne rozszerzenia.

Na przykład następujący przykładowy kod przedstawia sposób wyświetlania informacji dotyczących **IaaSDiagnostics** rozszerzenia przy użyciu interfejsu wiersza polecenia Azure **listy rozszerzeń maszyny wirtualnej azure** polecenia i używa **—-json**  opcję, aby zwracać pełne informacje.

    $ azure vm extension list -n IaaSDiagnostics --json
    [
      {
        "publisher": "Microsoft.Azure.Diagnostics",
        "name": "IaaSDiagnostics",
        "version": "1.2",
        "label": "Microsoft Monitoring Agent Diagnostics",
        "description": "Microsoft Monitoring Agent Extension",
        "replicationCompleted": true,
        "isJsonExtension": true
      }
    ]



### <a name="service-management-rest-apis"></a>Interfejsy API REST zarządzania usługami
Aby uzyskać informacje o dostępnych rozszerzeń służy następujących interfejsów API REST:

* Dla wystąpienia ról sieci web lub roli proces roboczy, można użyć [listę dostępnych rozszerzeń](https://msdn.microsoft.com/library/dn169559.aspx) operacji. Aby wyświetlić listę wersji dostępne rozszerzenia, można użyć [wersji rozszerzenia listy](https://msdn.microsoft.com/library/dn495437.aspx).
* Dla wystąpień maszyn wirtualnych, można użyć [listy rozszerzeń zasobu](https://msdn.microsoft.com/library/dn495441.aspx) operacji. Aby wyświetlić listę wersji dostępne rozszerzenia, można użyć [wersji rozszerzenia zasobu listy](https://msdn.microsoft.com/library/dn495440.aspx).

## <a name="add-update-or-disable-extensions"></a>Dodawanie, aktualizowanie lub wyłączyć rozszerzenia
Podczas tworzenia wystąpienia lub mogą być dodawane uruchomione wystąpienie można dodać rozszerzenia. Rozszerzenia może zostać zaktualizowany, wyłączone lub usunięte. Można wykonywać te akcje za pomocą poleceń cmdlet programu Azure PowerShell lub przy użyciu operacji interfejsu API REST zarządzania usługi. Aby zainstalować i skonfigurować niektóre rozszerzenia są wymagane parametry. Parametry publiczne i prywatne są obsługiwane dla rozszerzeń.

### <a name="azure-powershell"></a>Azure PowerShell
Za pomocą poleceń cmdlet programu Azure PowerShell jest najprostszym sposobem na dodawanie i aktualizowanie rozszerzeń. Korzystając z polecenia cmdlet rozszerzenia, większość ustawień konfiguracji rozszerzenia odbywa się automatycznie. W czasie konieczne może być programowane Dodawanie rozszerzenia. Należy to zrobić, należy podać konfiguracji rozszerzenia.

Następujące polecenia cmdlet umożliwia wiedzieć, czy rozszerzenie wymaga konfiguracji publiczne i prywatne parametrów:

* Dla wystąpienia ról sieci web lub roli proces roboczy, można użyć **Get-AzureServiceAvailableExtension** polecenia cmdlet.
* Dla wystąpień maszyn wirtualnych, można użyć **Get-AzureVMAvailableExtension** polecenia cmdlet.

### <a name="service-management-rest-apis"></a>Interfejsy API REST zarządzania usługami
Podczas pobierania listy dostępnych rozszerzeń przy użyciu interfejsów API REST, pojawi się informacji na temat sposobu rozszerzenia ma zostać skonfigurowana. Zwracane informacje mogą być wyświetlane informacje o parametrach reprezentowany przez schemat publicznej i prywatnej schematu. Wartości parametrów publicznego są zwracane w zapytaniach dotyczących wystąpienia. Wartości parametrów prywatne nie są zwracane.

Następujące interfejsy API REST umożliwia wiedzieć, czy rozszerzenie wymaga konfiguracji publiczne i prywatne parametrów:

* Dla wystąpienia ról sieci web lub roli proces roboczy **PublicConfigurationSchema** i **PrivateConfigurationSchema** elementy zawierają informacje w odpowiedzi z [dostępne listy Rozszerzenia](https://msdn.microsoft.com/library/dn169559.aspx) operacji.
* Dla wystąpień maszyn wirtualnych, **PublicConfigurationSchema** i **PrivateConfigurationSchema** elementy zawierają informacje w odpowiedzi z [listy zasobów Rozszerzenia](https://msdn.microsoft.com/library/dn495441.aspx) operacji.

> [!NOTE]
> Rozszerzenia za pomocą konfiguracji, które są zdefiniowane JSON. Gdy te typy rozszerzeń są używane, tylko **SampleConfig** element jest używany.
> 
> 

