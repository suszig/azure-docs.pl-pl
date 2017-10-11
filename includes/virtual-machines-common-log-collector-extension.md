
Diagnozowanie problemów z usługą w chmurze Microsoft Azure wymaga zbieranie plików dziennika usługi na maszynach wirtualnych, ponieważ występują problemy. Można użyć AzureLogCollector rozszerzenia na żądanie do kolekcji jednorazowe perfom dzienników z co najmniej jeden chmury maszyn wirtualnych usługi (od ról sieć web i roli proces roboczy) i przenieść zebranych plików do konta magazynu platformy Azure — wszystko to bez zdalnego logowania się do żadnego z maszyny wirtualne.

> [!NOTE]
> Opisy dla większości zarejestrowane informacje znajdują się w http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.asp.
> 
> 

Istnieją dwa tryby kolekcji zależne od typów plików, które mają być zbierane.

* Gość Azure dzienniki agentów tylko (GA). Ten tryb kolekcji obejmuje wszystkie dzienniki związanych z agentów gości Azure i inne składniki platformy Azure.
* Wszystkie dzienniki (pełną). Ten tryb kolekcji będzie zbierać wszystkie pliki w trybie GA plus:
  
  * dzienniki zdarzeń systemu i aplikacji
  * Dzienniki błędów HTTP
  * Dzienniki programu IIS
  * Dzienniki instalacji
  * inne dzienniki systemu

W obu trybach kolekcji można określić dodatkowe dane folderów kolekcji przy użyciu kolekcję o następującej strukturze:

* **Nazwa**: Nazwa kolekcji, która będzie używana jako nazwa podfolderu wewnątrz pliku zip do zebrania.
* **Lokalizacja**: ścieżka do folderu na maszynie wirtualnej, gdzie będą zbierane pliku.
* **SearchPattern**: wzorzec nazw plików, które mają być zbierane. Domyślna to "*"
* **Cykliczne**: Jeśli pliki mają być zbierane rekursywnie w folderze.

## <a name="prerequisites"></a>Wymagania wstępne
* Musisz mieć konto magazynu dla rozszerzenia do zapisywania plików zip wygenerowany.
* Należy upewnić się, że używasz V0.8.0 poleceń cmdlet programu PowerShell Azure lub nowszej. Aby uzyskać więcej informacji, zobacz [Azure pobiera](https://azure.microsoft.com/downloads/).

## <a name="add-the-extension"></a>Dodawanie rozszerzeń
Można użyć [Microsoft Azure PowerShell](https://msdn.microsoft.com/library/dn495240.aspx) poleceń cmdlet lub [API REST zarządzania usługami](https://msdn.microsoft.com/library/ee460799.aspx) można dodać rozszerzenia AzureLogCollector.

Dla usług w chmurze, istniejącego polecenia cmdlet programu Azure Powershell **AzureServiceExtension zestaw**, można włączyć rozszerzenia dla wystąpień roli usługi w chmurze. Za każdym razem, gdy to rozszerzenie jest włączona za pomocą tego polecenia cmdlet, zbierania dzienników jest wyzwalane w wystąpieniach wybranej roli wybranych ról.

W przypadku maszyn wirtualnych istniejącego polecenia cmdlet programu Azure Powershell **AzureVMExtension zestaw**, można włączyć rozszerzenia na maszynach wirtualnych. Za każdym razem, gdy to rozszerzenie jest włączona za pomocą polecenia cmdlet, zbierania dzienników jest wyzwalane w każdym wystąpieniu.

Wewnętrznie to rozszerzenie używa PublicConfiguration opartych na formacie JSON i PrivateConfiguration. Poniżej znajduje się układ próbkę JSON konfiguracji publiczne i prywatne.

### <a name="publicconfiguration"></a>PublicConfiguration
    {
        "Instances":  "*",
        "Mode":  "Full",
        "SasUri":  "SasUri to your storage account with sp=wl",
        "AdditionalData":
        [
          {
                  "Name":  "StorageData",
                  "Location":  "%roleroot%storage",
                  "SearchPattern":  "*.*",
                  "Recursive":  "true"
          },
          {
                "Name":  "CustomDataFolder2",
                "Location":  "c:\customFolder",
                "SearchPattern":  "*.log",
                "Recursive":  "false"
          },
        ]
    }

### <a name="privateconfiguration"></a>PrivateConfiguration
    {

    }

> [!NOTE]
> To rozszerzenie nie wymaga **privateConfiguration**. Można podać tylko pustą strukturą dla **— PrivateConfiguration** argumentu.
> 
> 

Możesz wykonać jedną dwa poniższe kroki, aby dodać AzureLogCollector do co najmniej jedno wystąpienie usługi w chmurze lub wybranych ról maszyny wirtualnej, która wyzwala kolekcje na każdej maszynie Wirtualnej Uruchom i wysyłanie zebranych plików do określone konto platformy Azure.

## <a name="adding-as-a-service-extension"></a>Dodawanie jako rozszerzenie usługi
1. Postępuj zgodnie z instrukcjami, aby połączyć program Azure PowerShell do subskrypcji.
2. Określić do których chcesz dodać i włączyć rozszerzenie AzureLogCollector wystąpień nazwę, miejsca, ról i rolę usługi.
   
        #Specify your cloud service name
        $ServiceName = 'extensiontest2'
   
        #Specify the slot. 'Production' or 'Staging'
        $slot = 'Production'
   
        #Specified the roles on which the extension will be installed and enabled
        $roles = @("WorkerRole1","WebRole1")
   
        #Specify the instances on which extension will be installed and enabled.  Use wildcard * for all instances
        $instances = @("*")
   
        #Specify the collection mode, "Full" or "GA"
        $mode = "GA"
3. Określ folder dodatkowych danych, dla której będą zbierane pliki (ten krok jest opcjonalny).
   
        #add one location
        $a1 = New-Object PSObject
   
        $a1 | Add-Member -MemberType NoteProperty -Name "Name" -Value "StorageData"
        $a1 | Add-Member -MemberType NoteProperty -Name "SearchPattern" -Value "*"
        $a1 | Add-Member -MemberType NoteProperty -Name "Location" -Value "%roleroot%storage"  #%roleroot% is normally E: or F: drive
        $a1 | Add-Member -MemberType NoteProperty -Name "Recursive" -Value "true"
   
        $AdditionalDataList+= $a1
              #more locations can be added....
   
   > [!NOTE]
   > Można użyć tokenu `%roleroot%` do określenia dysku głównym roli, ponieważ nie używa dysk stały.
   > 
   > 
4. Podaj nazwę konta magazynu platformy Azure i klucz, do którego zostanie przekazany zebranych plików.
   
        $StorageAccountName = 'YourStorageAccountName'
        $StorageAccountKey  = ‘YouStorageAccountKey'
5. Wywołanie SetAzureServiceLogCollector.ps1 (dołączony na końcu artykułu) w następujący sposób włączyć rozszerzenie AzureLogCollector dla usługi w chmurze. Po zakończeniu wykonywania można znaleźć przekazanego pliku w obszarze`https://YouareStorageAccountName.blob.core.windows.net/vmlogs`
   
        .\SetAzureServiceLogCollector.ps1 -ServiceName YourCloudServiceName  -Roles $roles  -Instances $instances –Mode $mode -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey -AdditionDataLocationList $AdditionalDataList

Poniżej znajduje się definicja parametry przekazywane do skryptu. (To jest kopiowana poniżej również.)

    [CmdletBinding(SupportsShouldProcess = $true)]

    param (
      [Parameter(Mandatory=$true)]
    [string]   $ServiceName,

    [Parameter(Mandatory=$false)]
    [string[]] $Roles ,

    [Parameter(Mandatory=$false)]
    [string[]] $Instances,

    [Parameter(Mandatory=$false)]
    [string]   $Slot = 'Production',

    [Parameter(Mandatory=$false)]
    [string]   $Mode = 'Full',

    [Parameter(Mandatory=$false)]
    [string]   $StorageAccountName,

    [Parameter(Mandatory=$false)]
    [string]   $StorageAccountKey,

    [Parameter(Mandatory=$false)]
    [PSObject[]] $AdditionDataLocationList = $null
    )

* *ServiceName*: Nazwa usługi w chmurze.
* *Role*: lista ról, takich jak "WebRole1" lub "WorkerRole1".
* *Wystąpienia*: Lista nazw wystąpień roli oddzielonych przecinkami — Użyj ciągu symbol wieloznaczny ("*") dla wszystkich wystąpień roli.
* *Gniazdo*: nazwa miejsca. "Production" lub "Tymczasowości".
* *Tryb*: tryb kolekcji. "Pełnej" lub "GA".
* *StorageAccountName*: Nazwa Azure konta magazynu do przechowywania zbieranych danych.
* *StorageAccountKey*: klucz konta magazynu Name Azure.
* *AdditionalDataLocationList*: Lista następującej strukturze:
  
      {
      String Name,
      String Location,
      String SearchPattern,
      Bool   Recursive
      }

## <a name="adding-as-a-vm-extension"></a>Dodawanie jako rozszerzenie maszyny Wirtualnej
Postępuj zgodnie z instrukcjami, aby połączyć program Azure PowerShell do subskrypcji.

1. Określ nazwę usługi, maszyny Wirtualnej i tryb kolekcji.
   
        #Specify your cloud service name
        $ServiceName = 'YourCloudServiceName'
   
        #Specify the VM name
        $VMName = "'YourVMName'"
   
        #Specify the collection mode, "Full" or "GA"
        $mode = "GA"
   
        Specify the additional data folder for which files will be collected (this step is optional).
   
        #add one location
        $a1 = New-Object PSObject
   
        $a1 | Add-Member -MemberType NoteProperty -Name "Name" -Value "StorageData"
        $a1 | Add-Member -MemberType NoteProperty -Name "SearchPattern" -Value "*"
        $a1 | Add-Member -MemberType NoteProperty -Name "Location" -Value "%roleroot%storage"  #%roleroot% is normally E: or F: drive
        $a1 | Add-Member -MemberType NoteProperty -Name "Recursive" -Value "true"
   
        $AdditionalDataList+= $a1
              #more locations can be added....
2. Podaj nazwę konta magazynu platformy Azure i klucz, do którego zostanie przekazany zebranych plików.
   
        $StorageAccountName = 'YourStorageAccountName'
        $StorageAccountKey  = ‘YouStorageAccountKey'
3. Wywołanie SetAzureVMLogCollector.ps1 (dołączony na końcu artykułu) w następujący sposób włączyć rozszerzenie AzureLogCollector dla usługi w chmurze. Po zakończeniu wykonywania można znaleźć przekazanego pliku w obszarze https://YouareStorageAccountName.blob.core.windows.net/vmlogs

Poniżej znajduje się definicja parametry przekazywane do skryptu. (To jest kopiowana poniżej również.)

    [CmdletBinding(SupportsShouldProcess = $true)]

    param (
        [Parameter(Mandatory=$true)]
      [string]   $ServiceName,

      [Parameter(Mandatory=$false)]
      [string] $VMName ,

        [Parameter(Mandatory=$false)]
      [string]   $Mode = 'Full',

      [Parameter(Mandatory=$false)]
      [string]   $StorageAccountName,

      [Parameter(Mandatory=$false)]
      [string]   $StorageAccountKey,

      [Parameter(Mandatory=$false)]
      [PSObject[]] $AdditionDataLocationList = $null
      )

* ServiceName: Twoje nazwa usługi w chmurze.
* VMName Nazwa maszyny Wirtualnej.
* Tryb: Tryb kolekcji. "Pełnej" lub "GA".
* StorageAccountName: Nazwa konta magazynu Azure do przechowywania zebranych danych.
* StorageAccountKey: Nazwa klucz konta magazynu platformy Azure.
* AdditionalDataLocationList: Lista następującej strukturze:

```
      {
        String Name,
        String Location,
        String SearchPattern,
        Bool   Recursive
      }
```

## <a name="extention-powershell-script-files"></a>Rozszerzenie plików skryptów środowiska PowerShell
SetAzureServiceLogCollector.ps1

    [CmdletBinding(SupportsShouldProcess = $true)]

    param (
                  [Parameter(Mandatory=$true)]
                  [string]   $ServiceName,

                  [Parameter(Mandatory=$false)]
                  [string[]] $Roles ,

                  [Parameter(Mandatory=$false)]
                  [string[]] $Instances = '*',

                  [Parameter(Mandatory=$false)]
                  [string]   $Slot = 'Production',

                  [Parameter(Mandatory=$false)]
                  [string]   $Mode = 'Full',

                  [Parameter(Mandatory=$false)]
                  [string]   $StorageAccountName,

                  [Parameter(Mandatory=$false)]
                  [string]   $StorageAccountKey,

                  [Parameter(Mandatory=$false)]
                  [PSObject[]] $AdditionDataLocationList = $null
            )

    $publicConfig = New-Object PSObject

    if ($Instances -ne $null -and $Instances.Count -gt 0)  #Instances should be seperated by ,
    {
        $instanceText = $Instances[0]
        for ($i = 1;$i -lt $Instances.Count;$i++)
        {
              $instanceText = $instanceText+ "," + $Instances[$i]
          }
        $publicConfig | Add-Member -MemberType NoteProperty -Name "Instances" -Value $instanceText
    }
    else  #For all instances if not specified.  The value should be a space or *
    {
        $publicConfig | Add-Member -MemberType NoteProperty -Name "Instances" -Value " "
    }

    if ($Mode -ne $null )
    {
        $publicConfig | Add-Member -MemberType NoteProperty -Name "Mode" -Value $Mode
    }
    else
    {
        $publicConfig | Add-Member -MemberType NoteProperty -Name "Mode" -Value "Full"
    }

    #
    #we need to get the Sasuri from StorageAccount and containers
    #
    $context = New-AzureStorageContext -Protocol https -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

    $ContainerName = "azurelogcollectordata"
    $existingContainer = Get-AzureStorageContainer -Context $context |  Where-Object { $_.Name -like $ContainerName}
    if ($existingContainer -eq $null)
    {
        "Container ($ContainerName) doesn't exist. Creating it now.."
        New-AzureStorageContainer -Context $context -Name $ContainerName -Permission off
    }

    $ExpiryTime =  [DateTime]::Now.AddMinutes(120).ToString("o")
    $SasUri = New-AzureStorageContainerSASToken -ExpiryTime $ExpiryTime -FullUri -Name $ContainerName -Permission rwl -Context $context
    $publicConfig | Add-Member -MemberType NoteProperty -Name "SasUri" -Value $SasUri

    #
    #Add AdditionalData to collect data from additional folders
    #
    if ($AdditionDataLocationList -ne $null )
    {
      $publicConfig | Add-Member -MemberType NoteProperty -Name "AdditionalData" -Value $AdditionDataLocationList
    }

    #
    # Convert it to JSON format
    #
    $publicConfigJSON = $publicConfig | ConvertTo-Json
    "publicConfig is:  $publicConfigJSON"

    #we just provide a empty privateConfig object
    $privateconfig = "{
    }"

    if ($Roles -ne $null)
    {
          Set-AzureServiceExtension -Service $ServiceName -Slot $Slot -Role $Roles -ExtensionName 'AzureLogCollector' -ProviderNamespace Microsoft.WindowsAzure.Compute -PublicConfiguration $publicConfigJSON -PrivateConfiguration $privateconfig -Version 1.0 -Verbose
    }
    else
    {
          Set-AzureServiceExtension -Service $ServiceName -Slot $Slot  -ExtensionName 'AzureLogCollector' -ProviderNamespace Microsoft.WindowsAzure.Compute -PublicConfiguration $publicConfigJSON -PrivateConfiguration $privateconfig -Version 1.0 -Verbose
    }

    #
    #This is an optional step: generate a sasUri to the container so it can be shared with other people if nened
    #
    $SasExpireTime = [DateTime]::Now.AddMinutes(120).ToString("o")
    $SasUri = New-AzureStorageContainerSASToken -ExpiryTime $ExpiryTime -FullUri -Name $ContainerName -Permission rl -Context $context
    $SasUri = $SasUri + "&restype=container&comp=list"
    Write-Output "The container for uploaded file can be accessed using this link:`r`n$sasuri"


SetAzureVMLogCollector.ps1

    [CmdletBinding(SupportsShouldProcess = $true)]

    param (
                  [Parameter(Mandatory=$true)]
                  [string]   $ServiceName,

                  [Parameter(Mandatory=$false)]
                  [string] $VMName ,

                  [Parameter(Mandatory=$false)]
                  [string]   $Mode = 'Full',

                  [Parameter(Mandatory=$false)]
                  [string]   $StorageAccountName,

                  [Parameter(Mandatory=$false)]
                  [string]   $StorageAccountKey,

                  [Parameter(Mandatory=$false)]
                  [PSObject[]] $AdditionDataLocationList = $null
            )

    $publicConfig = New-Object PSObject
    $publicConfig | Add-Member -MemberType NoteProperty -Name "Instances" -Value "*"

    if ($Mode -ne $null )
    {
        $publicConfig | Add-Member -MemberType NoteProperty -Name "Mode" -Value $Mode
    }
    else
    {
        $publicConfig | Add-Member -MemberType NoteProperty -Name "Mode" -Value "Full"
    }

    #
    #we need to get the Sasuri from StorageAccount and containers
    #
    $context = New-AzureStorageContext -Protocol https -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

    $ContainerName = "azurelogcollectordata"
    $existingContainer = Get-AzureStorageContainer -Context $context |  Where-Object { $_.Name -like $ContainerName}
    if ($existingContainer -eq $null)
    {
        "Container ($ContainerName) doesn't exist. Creating it now.."
        New-AzureStorageContainer -Context $context -Name $ContainerName -Permission off
    }

    $ExpiryTime =  [DateTime]::Now.AddMinutes(90).ToString("o")
    $SasUri = New-AzureStorageContainerSASToken -ExpiryTime $ExpiryTime -FullUri -Name $ContainerName -Permission rwl -Context $context
    $publicConfig | Add-Member -MemberType NoteProperty -Name "SasUri" -Value $SasUri

    #
    #Add AdditionalData to collect data from additional folders
    #
    if ($AdditionDataLocationList -ne $null )
    {
      $publicConfig | Add-Member -MemberType NoteProperty -Name "AdditionalData" -Value $AdditionDataLocationList
    }

    #
    # Convert it to JSON format
    #
    $publicConfigJSON = $publicConfig | ConvertTo-Json

    Write-Output "PublicConfigurtion is: \r\n$publicConfigJSON"

    #
    #we just provide a empty privateConfig object
    #
    $privateconfig = "{
    }"

    if ($VMName -ne $null )
    {
          $VM = Get-AzureVM -ServiceName $ServiceName -Name $VMName
          $VM.VM.OSVirtualHardDisk.OS

          if ($VM.VM.OSVirtualHardDisk.OS -like '*Windows*')
          {
                Set-AzureVMExtension -VM $VM -ExtensionName "AzureLogCollector" -Publisher Microsoft.WindowsAzure.Compute -PublicConfiguration $publicConfigJSON -PrivateConfiguration $privateconfig -Version 1.* | Update-AzureVM -Verbose

                #
                #We will check the VM status to find if operation by extension has been completed or not. The completion of the operation,either succeed or fail, can be indicated by
                #the presence of SubstatusList field.
                #
                $Completed = $false
                while ($Completed -ne $true)
                {
                        $VM = Get-AzureVM -ServiceName $ServiceName -Name $VMName
                        $status = $VM.ResourceExtensionStatusList | Where-Object {$_.HandlerName -eq "Microsoft.WindowsAzure.Compute.AzureLogCollector"}

                        if ( ($status.Code -ne 0) -and ($status.Status -like '*error*'))
                        {
                            Write-Output "Error status is returned: $($Status.ExtensionSettingStatus.FormattedMessage.Message)."
                              $Completed = $true
                        }
                        elseif (($status.ExtensionSettingStatus.SubstatusList -eq $null -or $status.ExtensionSettingStatus.SubstatusList.Count -lt 1))
                        {
                              $Completed = $false
                              Write-Output "Waiting for operation to complete..."
                        }
                        else
                        {
                              $Completed = $true
                              Write-Output "Operation completed."

                        $UploadedFileUri = $Status.ExtensionSettingStatus.SubStatusList[0].FormattedMessage.Message
                              $blob = New-Object Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob($UploadedFileUri)

                      #
                            # This is an optional step:  For easier access to the file, we can generate a read-only SasUri directly to the file
                              #
                              $ExpiryTimeRead =  [DateTime]::Now.AddMinutes(120).ToString("o")
                              $ReadSasUri = New-AzureStorageBlobSASToken -ExpiryTime $ExpiryTimeRead  -FullUri  -Blob  $blob.name -Container $blob.Container.Name -Permission r -Context $context

                            Write-Output "The uploaded file can be accessed using this link: $ReadSasUri"

                              #
                              #This is an optional step:  Remove the extension after we are done
                              #
                              Get-AzureVM -ServiceName $ServiceName -Name $VMName | Set-AzureVMExtension -Publisher Microsoft.WindowsAzure.Compute -ExtensionName "AzureLogCollector" -Version 1.* -Uninstall | Update-AzureVM -Verbose

                        }
                        Start-Sleep -s 5
                }
          }
          else
          {
              Write-Output "VM OS Type is not Windows, the extension cannot be enabled"
          }

    }
    else
    {
      Write-Output "VM name is not specified, the extension cannot be enabled"
    }

## <a name="next-steps"></a>Następne kroki
Teraz możesz zbadać lub skopiuj dzienniki z jednej lokalizacji bardzo proste.

