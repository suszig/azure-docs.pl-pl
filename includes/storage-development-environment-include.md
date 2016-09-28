## Konfigurowanie środowiska projektowego

Następnie skonfiguruj środowisko projektowe w programie Visual Studio, aby przygotować się do wypróbowania przykładowych kodów podanych w tym przewodniku.

### Utwórz projekt aplikacji konsolowej dla systemu Windows

W programie Visual Studio utwórz nową aplikację konsolową dla systemu Windows:

![Tworzenie aplikacji konsolowej dla systemu Windows](./media/storage-development-environment-include/storage-development-environment-include-1.png)

Wszystkie przykładowe kody w tym samouczku można dodać do metody **Main()** w `program.cs` w aplikacji konsolowej.

Pamiętaj, że Biblioteki klienta usługi Storage platformy Azure można używać z dowolnego typu aplikacji w środowisku .NET, m.in. usługi w chmurze Azure, aplikacji sieci Web platformy Azure, aplikacji pulpitu lub aplikacji mobilnej. W tym przewodniku dla uproszczenia przedstawiono aplikację konsolową.

### Użycie pakietu NuGet w celu zainstalowania wymaganych pakietów

Istnieją dwa pakiety, które trzeba zainstalować w projekcie, aby wykonać instrukcje z tego samouczka:

- [Biblioteka klienta usługi Storage platformy Microsoft Azure dla środowiska .NET](https://www.nuget.org/packages/WindowsAzure.Storage/): ten pakiet zapewnia dostęp programowy do zasobów danych na koncie magazynu.
- [Biblioteka programu Microsoft Azure Configuration Manager dla środowiska .NET](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/): ten pakiet zawiera klasę do analizowania parametrów połączenia w pliku konfiguracji, niezależnie od tego, gdzie została uruchomiona aplikacja.

Pakiet NuGet służy do pobrania obu pakietów. Wykonaj następujące kroki:

1. Kliknij projekt prawym przyciskiem myszy w **Eksploratorze rozwiązań** i wybierz polecenie **Zarządzaj pakietami NuGet**.
2. Wyszukaj w trybie online ciąg „WindowsAzure.Storage”, a następnie kliknij przycisk **Zainstaluj**, aby zainstalować Bibliotekę klienta usługi Storage oraz jej zależności.
3. Wyszukaj w trybie online ciąg „ConfigurationManager” i kliknij przycisk **Zainstaluj**, aby zainstalować program Azure Configuration Manager.

>[AZURE.NOTE] Pakiet Biblioteki klienta usługi Storage znajduje się również w [zestawie SDK Azure dla środowiska .NET](https://azure.microsoft.com/downloads/). Firma Microsoft zaleca jednak również zainstalowanie Biblioteki klienta usługi Storage z pakietu NuGet, aby zagwarantować posiadanie najnowszej wersji biblioteki klienta.
>
>Zależności ODataLib w Bibliotece klienta usługi Storage dla środowiska .NET są rozwiązywane za pośrednictwem pakietów ODataLib (w wersji 5.0.2 i nowszych) dostępnych przez pakiet NuGet, a nie za pośrednictwem usług Data Services WCF. Biblioteki ODataLib można pobrać bezpośrednio lub użyć odwołań w projekcie kodu za pośrednictwem pakietu NuGet. Określone pakiety ODataLib używane przez Bibliotekę klienta usługi Storage to [OData](http://nuget.org/packages/Microsoft.Data.OData/5.0.2), [Edm](http://nuget.org/packages/Microsoft.Data.Edm/5.0.2) i [Spatial](http://nuget.org/packages/System.Spatial/5.0.2). Ze względu na to, że biblioteki te są używane przez klasy usługi Azure Table Storage, są one wymaganymi zależnościami do programowania za pomocą Biblioteki klienta usługi Storage.


### Określanie środowiska docelowego

W przypadku uruchamiania przykładów w tym przewodniku istnieją dwie opcje środowiska:

- Można uruchomić kod dla konta usługi Azure Storage w chmurze. 
- Można uruchomić kod dla emulatora usługi Azure Storage. Emulator magazynu jest lokalnym środowiskiem, które emuluje konto usługi Azure Storage w chmurze. Emulator jest bezpłatną opcją do testowania i debugowania kodu, gdy aplikacja jest w fazie projektowania. Emulator używa dobrze znanego konta i klucza. Więcej szczegółowych informacji można znaleźć w temacie [Use the Azure Storage Emulator for Development and Testing](../articles/storage/storage-use-emulator.md) (Używanie emulatora usługi Azure Storage do programowania i testowania).

Jeśli obiektem docelowym jest konto magazynu w chmurze, skopiuj podstawowy klucz dostępu dla konta magazynu z witryny Azure Portal. Aby uzyskać więcej informacji, zobacz temat [View and copy storage access keys](../articles/storage/storage-create-storage-account.md#view-and-copy-storage-access-keys) (Wyświetlanie i kopiowanie kluczy dostępu kopiowania).

> [AZURE.NOTE] Na obiekt docelowy można wybrać emulator magazynu, aby uniknąć ponoszenia kosztów związanych z usługą Azure Storage. Jednak jeśli wybranym obiektem docelowym będzie konto usługi Azure Storage w chmurze, koszty związane z wykonaniem instrukcji w tym samouczku będą niewielkie.

### Konfigurowanie parametrów połączenia magazynu

Biblioteka klienta usługi Storage platformy Azure dla środowiska .NET obsługuje korzystanie z parametrów połączenia magazynu w celu konfiguracji punktów końcowych i poświadczeń w celu uzyskania dostępu do usług Storage. Najlepiej przechowywać parametry połączenia magazynu w pliku konfiguracji. 

Aby uzyskać więcej informacji dotyczących parametrów połączenia, zobacz temat [Configure a Connection String to Azure Storage](../articles/storage/storage-configure-connection-string.md) (Konfigurowania parametrów połączenia z usługą Azure Storage).

> [AZURE.NOTE] Klucz konta magazynu jest podobny do hasła głównego konta magazynu. Zawsze chroń klucz konta magazynu. Nie udostępniaj go innym użytkownikom, nie koduj go trwale ani nie zapisuj w zwykłym pliku tekstowym, do którego mają dostęp inne osoby. Wygeneruj ponownie klucz za pośrednictwem witryny Azure Portal, jeśli uważasz, że jego zabezpieczenia mogły zostać naruszone.

Aby skonfigurować parametry połączenia, otwórz plik `app.config` z Eksploratora rozwiązań w programie Visual Studio. Dodaj zawartość elementu `<appSettings>` widocznego poniżej. Zastąp ciąg `account-name` nazwą konta magazynu, a ciąg `account-key` kluczem dostępu do konta:

    <configuration>
        <startup> 
            <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5.2" />
        </startup>
        <appSettings>
            <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key" />
        </appSettings>
    </configuration>

Na przykład ustawienie konfiguracji powinno wyglądać mniej więcej tak:

    <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=storagesample;AccountKey=nYV0gln6fT7mvY+rxu2iWAEyzPKITGkhM88J8HUoyofvK7C6fHcZc2kRZp6cKgYRUM74lHI84L50Iau1+9hPjB==" />

Jeśli obiektem docelowym ma być emulator magazynu, możesz użyć skrótu klawiaturowego, który mapuje dobrze znaną nazwę konta i klucz. W takim przypadku ustawienie parametrów połączenia będzie wyglądać następująco:

    <add key="StorageConnectionString" value="UseDevelopmentStorage=true;" />



<!--HONumber=Sep16_HO3-->


