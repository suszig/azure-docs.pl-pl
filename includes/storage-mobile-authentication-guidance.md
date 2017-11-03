## <a name="configure-your-application-to-access-azure-storage"></a>Konfigurowanie aplikacji dostęp do magazynu Azure
Istnieją dwa sposoby uwierzytelniania aplikacji dostępu do usług magazynu:

* Klucz udostępniony: Użyj udostępnionych klucza tylko do celów testowych
* Udostępnionych podpis dostępu (SAS): Użyj sygnatury dostępu Współdzielonego dla aplikacji produkcyjnych

### <a name="shared-key"></a>Klucz wspólny
Uwierzytelniania klucza współużytkowanego oznacza, że aplikacja będzie używać nazwę konta i klucz konta dostępu do usług magazynu. Na potrzeby szybkiego przedstawiająca sposób używania tej biblioteki firma Microsoft będzie używać uwierzytelniania klucz udostępniony w tym wprowadzenie.

> [!WARNING] 
> **Klucz wstępny uwierzytelniania należy używać tylko do celów testowych!** Nazwa konta i klucz konta, które zapewniają dostęp Pełna odczytu/zapisu do skojarzonego konta magazynu, będą dystrybuowane do wszystkich osób, które pliki do pobrania aplikacji. Jest to **nie** jest dobrą rozwiązaniem, ponieważ istnieje ryzyko, że po klucz szkodzi niezaufani klienci.
> 
> 

Podczas korzystania z uwierzytelniania klucza wspólnego, utworzysz [ciąg połączenia](../articles/storage/common/storage-configure-connection-string.md). Parametry połączenia obejmuje:  

* **DefaultEndpointsProtocol** — możesz wybrać HTTP lub HTTPS. Jednak przy użyciu protokołu HTTPS zaleca się.
* **Nazwa konta** — nazwa konta magazynu
* **Klucz konta** — na [Azure Portal](https://portal.azure.com), przejdź do swojego konta magazynu i kliknij **klucze** ikonę, aby znaleźć te informacje.
* (Opcjonalnie) **EndpointSuffix** — służy do usług magazynu w regionach z innym punktem końcowym sufiksów, na przykład chińskiej wersji platformy Azure lub zarządzania Azure.

Oto przykład parametrów połączenia przy użyciu klucza wspólnego uwierzytelniania:

`"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here"`

### <a name="shared-access-signatures-sas"></a>Sygnatury dostępu współdzielonego (SAS)
Dla aplikacji mobilnej zalecaną metodą uwierzytelniania żądania przez klienta z usługą Azure Storage jest użycie udostępnionych podpis dostępu (SAS). Sygnatury dostępu Współdzielonego umożliwia udzielenie dostępu klienta do zasobu podany okres czasu, z określonym zestawem uprawnień.
Jako właściciel konta magazynu należy wygenerować sygnaturę dostępu Współdzielonego dla klientów mobilnych korzystać. Aby wygenerować sygnatury dostępu Współdzielonego, prawdopodobnie należy do zapisania oddzielne usługa, która generuje SAS zostać rozesłany do klientów. Do celów testowych, można użyć [Eksploratora usługi Microsoft Azure Storage](http://storageexplorer.com) lub [Azure Portal](https://portal.azure.com) wygenerować sygnaturę dostępu Współdzielonego. Podczas tworzenia sygnatury dostępu Współdzielonego, można określić przedział czasu, w którym sygnatury dostępu Współdzielonego jest prawidłowy i uprawnienia, które sygnatury dostępu Współdzielonego nadaje się do klienta.

Poniższy przykład przedstawia sposób używania Eksploratora magazynu Microsoft Azure można wygenerować sygnaturę dostępu Współdzielonego.

1. Jeśli nie jest jeszcze, [Zainstaluj Eksplorator magazynu Microsoft Azure](http://storageexplorer.com)
2. Nawiąż połączenie ze swoją subskrypcją.
3. Kliknij na koncie magazynu, a następnie kliknij kartę "Akcje" w lewym dolnym rogu. Kliknij przycisk "Uzyskaj sygnaturę dostępu współdzielonego" w celu wygenerowania "Parametry połączenia" dla sieci SAS.
4. Oto przykład parametrów połączenia SAS że przyznaje uprawnienia odczytu i zapisu na poziomie usługi, kontenera i obiektu blob usługi konta magazynu.
   
   `"SharedAccessSignature=sv=2015-04-05&ss=b&srt=sco&sp=rw&se=2016-07-21T18%3A00%3A00Z&sig=3ABdLOJZosCp0o491T%2BqZGKIhafF1nlM3MzESDDD3Gg%3D;BlobEndpoint=https://youraccount.blob.core.windows.net"`

Jak widać, korzystając z sygnatury dostępu Współdzielonego, że nie udostępnianie klucz konta w aplikacji. Dowiedz się więcej o sygnatury dostępu Współdzielonego i najlepsze rozwiązania dotyczące przy użyciu sygnatury dostępu Współdzielonego przez wyewidencjonowanie [sygnatury dostępu współdzielonego: opis modelu sygnatur dostępu Współdzielonego](../articles/storage/common/storage-dotnet-shared-access-signature-part-1.md).

