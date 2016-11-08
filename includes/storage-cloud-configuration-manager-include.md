[Biblioteka programu Microsoft Azure Configuration Manager dla platformy .NET](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/) zawiera klasę do analizowania parametrów połączenia w pliku konfiguracji. [Klasa CloudConfigurationManager](https://msdn.microsoft.com/library/azure/mt634650.aspx) analizuje ustawienia konfiguracji niezależnie od tego, czy aplikacja kliencka jest uruchomiona na komputerze, urządzeniu mobilnym, na maszynie wirtualnej Azure, czy w usłudze w chmurze platformy Azure.

Aby zapewnić odwołanie do pakietu CloudConfigurationManager, dodaj następującą instrukcję `using` do swojej klasy:

    using Microsoft.Azure;  //Namespace for CloudConfigurationManager

Oto przykład, w którym przedstawiono, jak pobrać parametry połączenia z pliku konfiguracji:

    // Parse the connection string and return a reference to the storage account.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

Użycie programu Azure Configuration Manager jest opcjonalne. Można także użyć interfejsu API, np. [klasy ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager.aspx) programu .NET Framework.

<!--HONumber=Sep16_HO3-->


