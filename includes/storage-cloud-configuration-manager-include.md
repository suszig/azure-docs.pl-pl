[Biblioteka programu Microsoft Azure Configuration Manager dla platformy .NET](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/) zawiera klasę do analizowania parametrów połączenia w pliku konfiguracji. [Klasa CloudConfigurationManager](https://msdn.microsoft.com/library/azure/mt634650.aspx) analizuje ustawienia konfiguracji niezależnie od tego, czy aplikacja kliencka jest uruchomiona na komputerze, urządzeniu przenośnym, na maszynie wirtualnej platformy Azure, czy w usłudze w chmurze platformy Azure.

Aby zapewnić odwołanie do pakietu CloudConfigurationManager, dodaj następującą dyrektywę `using`:

```csharp
using Microsoft.Azure;    //Namespace for CloudConfigurationManager
```

Oto przykład, w którym przedstawiono, jak pobrać parametry połączenia z pliku konfiguracji:

```csharp
// Parse the connection string and return a reference to the storage account.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));
```

Użycie programu Azure Configuration Manager jest opcjonalne. Możesz także użyć interfejsu API, np. klasy [ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager.aspx) programu .NET Framework.



<!--HONumber=Dec16_HO2-->


