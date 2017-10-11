>[!NOTE]
> Możesz pozostawić komentarze na tej stronie opinii lub za pomocą [Azure opinii](https://feedback.azure.com/forums/216843-virtual-machines) #azerrormessage znacznika.

## <a name="error-response-format"></a>Format odpowiedzi błędu 
Maszyny wirtualne platformy Azure, użyj następującego formatu JSON dla odpowiedzi komunikat o błędzie:

```json
{
  "status": "status code",
  "error": {
    "code":"Top level error code",
    "message":"Top level error message",
    "details":[
     {
      "code":"Inner evel error code",
      "message":"Inner level error message"
     }
    ]
   }
}
```

Odpowiedzi na błąd zawsze zawiera kod stanu i obiektu błędu. Każdy obiekt błąd zawsze zawiera kod błędu i komunikatu. Jeśli maszyna wirtualna została utworzona przy użyciu szablonu, obiekt błędu zawiera także sekcji Szczegóły, która zawiera wewnętrzny poziom nawiasów kody błędów i komunikatów. Najbardziej wewnętrzny poziom komunikat o błędzie jest zazwyczaj awarii głównego.


## <a name="common-virtual-machine-management-errors"></a>Typowe błędy zarządzania maszyny wirtualnej

Ta sekcja zawiera typowe komunikaty o błędach, które można napotkać podczas zarządzania maszyn wirtualnych:

|  Kod błędu:  |  Komunikat o błędzie  |  
|  :------| :-------------|  
|  AcquireDiskLeaseFailed  |  Nie można ustanowić dzierżawy podczas tworzenia dysku "{0}" za pomocą obiektu blob z identyfikatorem URI {1}. Obiekt blob jest już używana.  |  
|  AllocationFailed  |  Alokacja nie powiodła się. Zmniejsz rozmiar maszyny Wirtualnej lub liczbę maszyn wirtualnych, spróbuj ponownie później lub spróbuj przeprowadzić wdrożenie w innym zestawie dostępności lub innej lokalizacji platformy Azure.  |  
|  AllocationFailed  |  Alokacja maszyny Wirtualnej nie powiodła się z powodu błędu wewnętrznego. Spróbuj ponownie później lub spróbuj wdrożenia w innej lokalizacji.  |
|  ArtifactNotFound  |  Nie można odnaleźć rozszerzenia maszyny Wirtualnej o wydawcy "{0}" i typie "{1}" w lokalizacji "{2}".  |
|  ArtifactNotFound  |  Rozszerzenie o wydawcy "{0}", wpisz "{1}" i nie można odnaleźć wersji programu obsługi typu "{2}" w repozytorium rozszerzeń.  |
|  ArtifactVersionNotFound  |  W repozytorium artefaktów nie znaleziono wersji odpowiadającej żądanej wersji „{0}”.  |
|  ArtifactVersionNotFound  |  Wersja nie znaleziono w repozytorium artefaktów, która spełnia żądanej wersji "{0}" dla rozszerzenia maszyny Wirtualnej o wydawcy "{1}" i wpisz "{2}".  |
|  AttachDiskWhileBeingDetached  |  Nie można dołączyć dysku danych "{0}" do maszyny Wirtualnej "{1}", ponieważ dysk jest aktualnie odłączona. Poczekaj, aż dysk zostanie całkowicie odłączony, a następnie spróbuj ponownie.  |
|  Element BadRequest  |  Wyrównane "zestawów dostępności nie są jeszcze obsługiwane w tym regionie.  |
|  Element BadRequest  |  Dodawanie maszyny wirtualnej z dyskami zarządzanych do niezarządzanych zestawu dostępności lub dodanie z dyskami na podstawie obiektu blob do zarządzanych zestawu dostępności maszyny wirtualnej nie jest obsługiwane. Utwórz zbiór dostępności z właściwością "zarządzany" ustawiona, aby dodać maszyny Wirtualnej z dyskami zarządzanych do niego.  |
|  Element BadRequest  |  Dyski zarządzane nie są obsługiwane w tym regionie.  |
|  Element BadRequest  |  Wiele Vmextension na program obsługi nie jest obsługiwane w systemie operacyjnym typu "{0}". VMExtension "{1}" z programem obsługi "{2}" już dodany lub określony w danych wejściowych.  |
|  Element BadRequest  |  Operacja "{0}" nie jest obsługiwana dla zasobu "{1}" w przypadku dysków zarządzanych.  |
|  CertificateImproperlyFormatted  |  Reprezentacja JSON klucza tajnego uzyskana z {0} ma pole danych, które nie jest poprawnie sformatowanym plikiem PFX, albo podane hasło nie dekoduje prawidłowo pliku PFX.  |
|  CertificateImproperlyFormatted  |  Danych uzyskanych z {0} nie da się rozszeregować na format JSON.  |
|  Konflikt  |  Zmiana rozmiaru dysku jest dozwolona tylko podczas tworzenia maszyny wirtualnej lub po cofnięciu jej przydziału.  |
|  ConflictingUserInput  |  Nie można dołączyć dysku "{0}", ponieważ dysk jest już przypisany do maszyny Wirtualnej "{1}".  |
|  ConflictingUserInput  |  Źródłowa grupa zasobów jest taka sama jak docelowa.  |
|  ConflictingUserInput  |  Konta magazynu źródłowego i docelowego dla dysku {0} są różne.  |
|  ContainerAlreadyOnLease  |  Istnieje już dzierżawa na kontenerze magazynu przechowującym obiekt blob z identyfikatorem URI {0}.  |
|  CrossSubscriptionMoveWithKeyVaultResources  |  Żądanie przeniesienia zasobów zawiera zasoby KeyVault przywoływane przez co najmniej jeden {0} s w żądaniu. Nie jest to obsługiwane obecnie między subskrypcjami przenoszenia. Sprawdź, czy szczegóły błędu dla identyfikatorów zasobów KeyVault.  |
|  DiagnosticsOperationInternalError  |  Wystąpił błąd wewnętrzny podczas przetwarzania profilu diagnostyki maszyny wirtualnej {0}.  |
|  DiskBlobAlreadyInUseByAnotherDisk  |  {0} obiektu blob jest już używany przez inny dysk należący do maszyny Wirtualnej "{1}". Możesz zbadać metadane obiektu blob, aby uzyskać informacje na dysku.  |
|  DiskBlobNotFound  |  Nie można znaleźć obiektu blob dysku VHD z identyfikatora URI {0} dla dysku "{1}".  |
|  DiskBlobNotFound  |  Nie można znaleźć obiektu blob dysku VHD z identyfikatorem URI {0}.  |
|  DiskEncryptionKeySecretMissingTags  |  klucz tajny {0} nie ma tagów {1}. Zaktualizuj wersję klucza tajnego, Dodaj wymagane tagi i spróbuj ponownie.  |
|  DiskEncryptionKeySecretUnwrapFailed  |  Dekodowanie wartości tajny {0} przy użyciu klucza {1} nie powiodło się.  |
|  DiskImageNotReady  |  {0} obrazu dysku jest w stanie {1}. Spróbuj ponownie, gdy obraz będzie gotowy.  |
|  DiskPreparationError  |  Wystąpił co najmniej jeden błąd podczas przygotowywania dysków maszyny Wirtualnej. Zobacz widok wystąpienia dysku, aby uzyskać szczegółowe informacje.  |
|  DiskProcessingError  |  Dysk przetwarzanie zostało zatrzymane, jako maszyna wirtualna ma inne dyski w przypadku dysków nie powiodło się.  |
|  ImageBlobNotFound  |  Nie można znaleźć obiektu blob dysku VHD z identyfikatora URI {0} dla dysku "{1}".  |
|  ImageBlobNotFound  |  Nie można znaleźć obiektu blob dysku VHD z identyfikatorem URI {0}.  |
|  IncorrectDiskBlobType  |  Obiekty BLOB dysków może być tylko typu stronicowych obiektów blob. {0} obiektu blob dla dysku "{1}" ma typ blokowy obiekt blob.  |
|  IncorrectDiskBlobType  |  Obiekty BLOB dysków może być tylko typu stronicowych obiektów blob. {0} obiektu blob jest typu "{1}".  |
|  IncorrectImageBlobType  |  Obiekty BLOB dysków może być tylko typu stronicowych obiektów blob. {0} obiektu blob dla dysku "{1}" ma typ blokowy obiekt blob.  |
|  IncorrectImageBlobType  |  Obiekty BLOB dysków może być tylko typu stronicowych obiektów blob. {0} obiektu blob jest typu "{1}".  |
|  InternalOperationError  |  Nie można rozpoznać konto magazynu {0}. Upewnij się, że został on utworzony za pośrednictwem dostawcy zasobów magazynu w tej samej lokalizacji co zasób obliczeniowy.  |
|  InternalOperationError  |  zadania poszukiwania celu {0} nie powiodło się.  |
|  InternalOperationError  |  Wystąpił błąd podczas sprawdzania poprawności profilu sieciowego maszyny wirtualnej „{0}”.  |
|  InvalidAccountType  |  Dla konta {0} jest nieprawidłowy.  |
|  InvalidParameter  |  Wartość parametru {0} jest nieprawidłowa.  |
|  InvalidParameter  |  Określone hasło administratora jest niedozwolone.  |
|  InvalidParameter  |  "Podane hasło musi należeć do zakresu od {0}-{1\} \ znaków i musi spełniać co najmniej {2} wymagania dotyczące złożoności hasła z następujących czynności: <ol><li> Zawiera wielką literę</li><li>Zawiera małą literę</li><li>Zawiera cyfrę</li><li>Zawiera znaki specjalne.</li></ol>  |
|  InvalidParameter  |  Określona nazwa użytkownika administratora jest niedozwolona.  |
|  InvalidParameter  |  Nie można podłączyć istniejącego dysku z systemem operacyjnym, jeśli maszyna wirtualna jest utworzona z obrazu użytkownika lub platformy.  |
|  InvalidParameter  |  Nazwa kontenera {0} jest nieprawidłowy. Nazwy kontenerów muszą być 3 do 63 znaków długości i mogą zawierać tylko małe znaki alfanumeryczne i łączniki. Łącznik musi być poprzedzone i następuje znak alfanumeryczny.  |
|  InvalidParameter  |  Nazwa kontenera {0} w {1} adres URL jest nieprawidłowy. Nazwy kontenerów muszą być 3 do 63 znaków długości i mogą zawierać tylko małe znaki alfanumeryczne i łączniki. Łącznik musi być poprzedzone i następuje znak alfanumeryczny.  |
|  InvalidParameter  |  Nazwa obiektu blob w {0} adres URL zawiera ukośnik. Jest to obecnie nieobsługiwane w przypadku dysków.  |
|  InvalidParameter  |  Identyfikator URI {0} nie wygląda na prawidłowy identyfikator URI obiektu blob.  |
|  InvalidParameter  |  Dysk o nazwie "{0}" już używa tego samego numeru LUN: {1}.  |
|  InvalidParameter  |  Dysk o nazwie "{0}" już istnieje.  |
|  InvalidParameter  |  Nie można określić elementów przesłaniających obraz użytkownika dla dysku, który zdefiniowano w określonym odwołaniu do obrazu.  |
|  InvalidParameter  |  Dysk o nazwie "{0}" już używa tej samej {1} adres URL dysku VHD.  |
|  InvalidParameter  |  Liczba błędów określonej domeny {0} musi należeć {1} zakres do {2}.  |
|  InvalidParameter  |  Typ licencji {0} jest nieprawidłowy. Prawidłowy typ licencji są: Windows_Client lub Windows_Server, z uwzględnieniem wielkości liter.  |
|  InvalidParameter  |  Nazwa hosta systemu Linux nie może przekraczać {0} znaków ani zawierać następujących znaków: {1}.  |
|  InvalidParameter  |  Ścieżka docelowa kluczy publicznych SSH jest obecnie ograniczona do wartości domyślnej {0} ze względu na znany problem z agentem inicjowania obsługi administracyjnej systemu Linux.  |
|  InvalidParameter  |  Dysk pod numerem LUN {0} już istnieje.  |
|  InvalidParameter  |  {0} subskrypcji żądania musi odpowiadać {1} subskrypcji zawarte w identyfikatorze dysku zarządzanego.  |
|  InvalidParameter  |  Niestandardowe dane w profilu OSProfile muszą korzystać z szyfrowania Base64, a liczba ich znaków nie może przekraczać {0}.  |
|  InvalidParameter  |  Nazwa obiektu blob w {0} adres URL musi mieć rozszerzenie "{1}".  |
|  InvalidParameter  |  {0} "nie jest prawidłowy prefiks nazwy przechwyconego obiektu blob dysku VHD. Prawidłowy prefiks zgodny z wyrażeniem regularnym "{1}".  |
|  InvalidParameter  |  Jeśli obsługa administracyjna agenta maszyny wirtualnej nie została zainicjowana, nie można dodać certyfikatów do maszyny wirtualnej.  |
|  InvalidParameter  |  Dysk pod numerem LUN {0} już istnieje.  |
|  InvalidParameter  |  Nie można utworzyć maszyny Wirtualnej, ponieważ żądany rozmiar {0} nie jest dostępne w klastrze, w którym zestawu dostępności jest aktualnie przydzielone. Są dostępne rozmiary: {1}. Przeczytaj więcej VM na zmianę rozmiaru strategii w https://aka.ms/azure-resizevm.  |
|  InvalidParameter  |  Żądana {0} rozmiar maszyny Wirtualnej nie jest dostępny w bieżącym obszarze. Dostępne rozmiary bieżącego obszaru są: {1}. Dowiedz się więcej informacji na temat dostępnych rozmiarów maszyn wirtualnych w każdym regionie na https://aka.ms/azure-regions.  |
|  InvalidParameter  |  Żądana {0} rozmiar maszyny Wirtualnej nie jest dostępny w bieżącym obszarze. Dowiedz się więcej informacji na temat dostępnych rozmiarów maszyn wirtualnych w każdym regionie na https://aka.ms/azure-regions.  |
|  InvalidParameter  |  Nazwa użytkownika administratora systemu Windows nie może być większa niż {0} znaków długie, kończyć się kropką (.) ani zawierać następujących znaków: {1}.  |
|  InvalidParameter  |  Nazwa komputera systemu Windows nie może być większa niż liczba znaków {0} długie, być całkowicie numeryczna ani zawierać następujących znaków: {1}.  |
|  MissingMoveDependentResources  |  Żądanie przeniesienia zasobów zawiera zasoby zależne. Sprawdź szczegóły błędów dotyczących brakujących identyfikatorów zasobów.  |
|  MoveResourcesHaveInvalidState  |  Żądanie przeniesienia zasobów zawiera maszyny wirtualne, które są skojarzone z kontami magazynu nieprawidłowy. Sprawdź, czy szczegółowe informacje dotyczące tych identyfikatorów zasobów i nazwy konta magazynu do którego istnieje odwołanie.  |
|  MoveResourcesHavePendingOperations  |  Żądanie przeniesienia zasobów zawiera zasoby, dla których operacja oczekuje. Sprawdź szczegóły identyfikatorów tych zasobów. Ponów operację po zakończeniu operacji oczekujących.  |
|  MoveResourcesNotFound  |  Żądanie przeniesienia zasobów zawiera zasoby, których nie można znaleźć. Sprawdź szczegóły identyfikatorów tych zasobów.  |
|  NetworkingInternalOperationError  |  Nieznany błąd alokacji sieci.  |
|  NetworkingInternalOperationError  |  Nieznany błąd alokacji sieci  |
|  NetworkingInternalOperationError  |  Wystąpił błąd podczas przetwarzania profilu sieciowego maszyny wirtualnej.  |
|  notFound  |  Nie można znaleźć zestawu danych o dostępności {0}.  |
|  notFound  |  Źródłowej maszyny wirtualnej "{0}" określony w żądaniu nie istnieje w tej lokalizacji platformy Azure.  |
|  notFound  |  Nie znaleziono dzierżawy o identyfikatorze {0}.  |
|  notFound  |  Nie można odnaleźć obrazu {0}.  |
|  NotSupported  |  Typ licencji to {0}, ale {1} obiektu blob obrazu nie jest obiektem lokalnym.  |
|  OperationNotAllowed  |  Nie można usunąć zestawu dostępności {0}. Przed usunięciem zestawu danych o dostępności upewnij się, że nie zawiera żadnej maszyny Wirtualnej.  |
|  OperationNotAllowed  |  Zmiana zestawu dostępności jednostki SKU z 'Wyrównany' do 'Klasycznym' jest niedozwolona.  |
|  OperationNotAllowed  |  Nie można modyfikować rozszerzeń w maszynie wirtualnej, kiedy maszyna wirtualna nie działa.  |
|  OperationNotAllowed  |  Akcji przechwytywania jest obsługiwana tylko na maszynie wirtualnej z dyskami na podstawie obiektu blob. Użyj zasobu "Image" interfejsy API do utworzenia obrazu z zarządzanego maszyny wirtualnej.  |
|  OperationNotAllowed  |  Nie można utworzyć zasobu {0} z obrazu {1}, dopóki obraz został utworzony pomyślnie.  |
|  OperationNotAllowed  |  Nie można aktualizować elementu encryptionSettings po przydzieleniu maszyny wirtualnej. Spróbuj ponownie po cofnięciu przydziału maszyny wirtualnej.  |
|  OperationNotAllowed  |  Dodanie zarządzanego dysku do maszyny wirtualnej z dyskami opartymi na obiektach blob nie jest obsługiwane.  |
|  OperationNotAllowed  |  Maksymalna liczba dysków danych może zostać dołączony do tego rozmiaru maszyny Wirtualnej to {0}.  |
|  OperationNotAllowed  |  Dodanie dysku opartego na obiektach blob do maszyny wirtualnej z zarządzanymi dyskami nie jest obsługiwane.  |
|  OperationNotAllowed  |  Operacja "{0}" nie jest dozwolona w obrazie "{1}", ponieważ obraz jest oznaczony do usunięcia. Można tylko spróbuj ponownie wykonać operację usuwania (lub poczekaj, aż trwającej co do ukończenia).  |
|  OperationNotAllowed  |  Operacja "{0}" nie jest dozwolona na maszynie Wirtualnej "{1}", ponieważ uogólniona maszyna wirtualna.  |
|  OperationNotAllowed  |  Operacja "{0}" nie jest dozwolona jako kolekcja punktu przywracania "{1}" jest oznaczona do usunięcia.  |
|  OperationNotAllowed  |  Operacja "{0}" nie jest dozwolona na rozszerzenia maszyny Wirtualnej "{1}", ponieważ jest ona oznaczona do usunięcia. Można tylko spróbuj ponownie wykonać operację usuwania (lub poczekaj, aż trwającej co do ukończenia).  |
|  OperationNotAllowed  |  Operacja "{0}" nie jest dozwolona, ponieważ trwa przydzielania maszyny wirtualnej "{1}" przy użyciu obrazu "{2}".  |
|  OperationNotAllowed  |  Operacja "{0}" nie jest dozwolona, ponieważ aktualnie używa obrazu "{2}" ScaleSet maszyny wirtualnej "{1}".  |
|  OperationNotAllowed  |  Operacja "{0}" nie jest dozwolona na maszynie Wirtualnej "{1}", ponieważ maszyna wirtualna została oznaczona do usunięcia. Można tylko spróbuj ponownie wykonać operację usuwania (lub poczekaj, aż trwającej co do ukończenia).  |
|  OperationNotAllowed  |  Operacja "{0}" nie jest dozwolona na maszynie Wirtualnej "{1}", ponieważ maszyna wirtualna jest alokację lub oznaczone do cofnięcia alokacji.  |
|  OperationNotAllowed  |  Operacja "{0}" nie jest dozwolona na maszynie Wirtualnej "{1}", ponieważ maszyna wirtualna jest uruchomiona. Określ zasilania poza jawnie w przypadku zamykania maszyny Wirtualnej z wewnątrz systemu operacyjnego gościa.  |
|  OperationNotAllowed  |  Operacja "{0}" nie jest dozwolona na maszynie Wirtualnej "{1}", ponieważ nie cofnięciu przydziału maszyny Wirtualnej.  |
|  OperationNotAllowed  |  Operacja "{0}" nie jest dozwolona na maszynie Wirtualnej "{1}", ponieważ maszyna wirtualna ma rozszerzenie "{2}" w stanie niepowodzenia.  |
|  OperationNotAllowed  |  Operacja "{0}" nie jest dozwolona na maszynie Wirtualnej "{1}", ponieważ inna operacja jest w toku.  |
|  OperationNotAllowed  |  Operacja "{0}" wymaga maszynę wirtualną "{1}", aby być uogólniony.  |
|  OperationNotAllowed  |  Operacja wymaga uruchomienia maszyny wirtualnej (lub ustawienia jej w celu uruchomienia).  |
|  OperationNotAllowed  |  Dysk o rozmiarze {0} GB, który jest mniejszy niż rozmiar {1}GB odpowiedniego dysku w obrazie, jest niedozwolone.  |
|  OperationNotAllowed  |  Rozszerzenia zestawu skali maszyny wirtualnej programu obsługi „{0}” można dodać tylko w momencie tworzenia zestawu skali maszyny wirtualnej.  |
|  OperationNotAllowed  |  Rozszerzenia zestawu skali maszyny wirtualnej programu obsługi „{0}” można usunąć tylko w momencie usuwania zestawu skali maszyny wirtualnej.  |
|  OperationNotAllowed  |  Maszyny Wirtualnej "{0}" jest już używa dysków zarządzanych.  |
|  OperationNotAllowed  |  Maszyny Wirtualnej "{0}" należy do zestawu dostępności "Klasycznym" "{1}". Zaktualizuj wartość "wyrównany' SKU, a następnie ponów próbę wykonania konwersji dostępności.  |
|  OperationNotAllowed  |  Utworzony na podstawie obrazu maszyny Wirtualnej nie może mieć obiektu blob na podstawie dysków. Wszystkie dyski muszą być dysków zarządzanych.  |
|  OperationNotAllowed  |  Nie można ukończyć operacji przechwytywania, ponieważ to nie jest uogólniona maszyna wirtualna.  |
|  OperationNotAllowed  |  Operacje zarządzania na maszynie Wirtualnej "{0}" są niedozwolone, ponieważ konwersji dysków maszyny Wirtualnej do zarządzanych dysków.  |
|  OperationNotAllowed  |  Wykonywana operacja zmienia stan zasilania maszyny wirtualnej {0} do {1}. Przeprowadź {2} operację po pewnym czasie.  |
|  OperationNotAllowed  |  Nie można dodać lub zaktualizować maszyny Wirtualnej. Żądana {0} rozmiar maszyny Wirtualnej nie może być dostępna w istniejącej jednostki alokacji. Przeczytaj więcej VM na zmianę rozmiaru strategii w https://aka.ms/azure-resizevm.  |
|  OperationNotAllowed  |  Można zmienić rozmiaru maszyny Wirtualnej, ponieważ żądany rozmiar {0} nie jest dostępne w klastrze, w którym zestawu dostępności aktualnego przydziału. Są dostępne rozmiary: {1}. Przeczytaj więcej VM na zmianę rozmiaru strategii w https://aka.ms/azure-resizevm.  |
|  OperationNotAllowed  |  Nie można zmienić rozmiar maszyny Wirtualnej, ponieważ żądany rozmiar {0} nie jest dostępna w klastrze, w którym aktualnego przydziału maszyny Wirtualnej. Aby zmienić rozmiar należy Cofnij Przydział maszyny Wirtualnej do {1} (jest to operacja zatrzymania w portalu Azure), a następnie spróbuj ponownie wykonać operację zmiany rozmiaru. Przeczytaj więcej VM na zmianę rozmiaru strategii w https://aka.ms/azure-resizevm.  |
|  OSProvisioningClientError  |  Wystąpił błąd aprowizowania systemu operacyjnego dla maszyny wirtualnej „{0}”, ponieważ system operacyjny gościa jest aktualnie aprowizowany.  |
|  OSProvisioningClientError  |  Inicjowania obsługi systemu operacyjnego dla maszyny Wirtualnej "{0}" nie powiodło się. Szczegóły błędu: {1} upewnij się, że obraz został poprawnie przygotowany (uogólniony). <ul><li>Instrukcje dla systemu Windows: https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/  </li></ul> |
|  OSProvisioningClientError  |  Generowanie klucza SSH hosta nie powiodło się. Szczegóły błędu: {0}. Aby rozwiązać ten problem Sprawdź, czy agenta systemu Linux jest poprawnie skonfigurowana. <ul><li>Możesz sprawdzić instrukcjami znajdującymi się na: https://azure.microsoft.com/documentation/articles/virtual-machines-linux-agent-user-guide/ </li></ul> |
|  OSProvisioningClientError  |  Nazwa użytkownika dla maszyny Wirtualnej jest nieprawidłowy dla tej dystrybucji systemu Linux. Szczegóły błędu: {0}.  |
|  OSProvisioningInternalError  |  Niepowodzenie inicjowania obsługi systemu operacyjnego na maszynie wirtualnej „{0}” z powodu błędu wewnętrznego.  |
|  OSProvisioningTimedOut  |  Inicjowania obsługi systemu operacyjnego dla maszyny Wirtualnej "{0}" nie została ukończona w wyznaczonym czasie. Maszyna wirtualna może nadal pomyślnie zakończyć inicjowanie obsługi. Sprawdź później stan inicjowania obsługi administracyjnej.  |
|  OSProvisioningTimedOut  |  Inicjowania obsługi systemu operacyjnego dla maszyny Wirtualnej "{0}" nie została ukończona w wyznaczonym czasie. Maszyna wirtualna może nadal pomyślnie zakończyć inicjowanie obsługi. Sprawdź później stan inicjowania obsługi administracyjnej. Sprawdź także, czy obraz został poprawnie przygotowany (uogólniony).   <ul><li>Instrukcje dla systemu Windows: https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/ </li><li> Instrukcje dla systemu Linux: https://azure.microsoft.com/documentation/articles/virtual-machines-linux-capture-image/</li></ul>  |
|  OSProvisioningTimedOut  |  Inicjowania obsługi systemu operacyjnego dla maszyny Wirtualnej "{0}" nie została ukończona w wyznaczonym czasie. Agent gościa maszyny Wirtualnej Wykryto jednak uruchomiona. Sugeruje to system operacyjny gościa nie został poprawnie przygotowany do użycia jako obraz maszyny Wirtualnej (CreateOption = FromImage). Aby rozwiązać ten problem, użyj wirtualnego dysku twardego, ponieważ jest z CreateOption = Attach lub prawidłowo przygotowania go do użycia jako obraz:   <ul><li>Instrukcje dla systemu Windows: https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/ </li><li> Instrukcje dla systemu Linux: https://azure.microsoft.com/documentation/articles/virtual-machines-linux-capture-image/</li></ul>  |
|  OverConstrainedAllocationRequest  |  Wymagany rozmiar maszyny wirtualnej nie jest obecnie dostępny w wybranej lokalizacji.  |
|  ResourceUpdateBlockedOnPlatformUpdate  |  Nie można zaktualizować zasobu w tej chwili z powodu trwającej platformy aktualizacji. Spróbuj ponownie później.  |
|  StorageAccountLimitation  |  Konto magazynu „{0}” nie obsługuje stronicowych obiektów blob, które są wymagane do tworzenia dysków.  |
|  StorageAccountLimitation  |  Konto magazynu „{0}” przekroczyło limit przydziału.  |
|  StorageAccountLocationMismatch  |  Nie można rozpoznać konto magazynu {0}. Upewnij się, że został on utworzony za pośrednictwem dostawcy zasobów magazynu w tej samej lokalizacji co zasób obliczeniowy.  |
|  StorageAccountNotFound  |  Nie można odnaleźć konta magazynu {0}. Upewnij się, konto magazynu nie zostanie usunięta i należy do tej samej lokalizacji platformy Azure co maszyna wirtualna.  |
|  StorageAccountNotRecognized  |  Użyj konta magazynu zarządzanego przez dostawcę zasobów magazynu. Użycie {0} nie jest obsługiwane.  |
|  StorageAccountOperationInternalError  |  Podczas dostępu do konta magazynu {0} wystąpił błąd wewnętrzny.  |
|  StorageAccountSubscriptionMismatch  |  Konto magazynu {0} nie należy do subskrypcji {1}.  |
|  StorageAccountTooBusy  |  Konto magazynu "{0}" jest obecnie zbyt zajęty. Należy wziąć pod uwagę przy użyciu innego konta.  |
|  StorageAccountTypeNotSupported  |  {0} dysku używa {1}, który jest konto magazynu obiektów Blob. Spróbuj ponownie z konta magazynu ogólnego przeznaczenia.  |
|  StorageAccountTypeNotSupported  |  Konto magazynu {0} jest typu {1}. Funkcja diagnostyki rozruchu obsługuje typy kont magazynu {2}.  |
|  SubscriptionNotAuthorizedForImage  |  Subskrypcja nie została autoryzowana.  |
|  TargetDiskBlobAlreadyExists  |  Obiekt blob {0} już istnieje. Podaj inny identyfikator URI do utworzenia nowego dysku danych puste "{1}" obiektu blob.  |
|  TargetDiskBlobAlreadyExists  |  Przechwytywanie nie można kontynuować operacji, ponieważ docelowy obrazu obiektu blob {0} już istnieje i nie ustawiono flagę, aby zastępować obiekty BLOB dysków VHD. Usuń obiekt blob, albo Ustaw flagę, aby zastępować obiekty BLOB dysków VHD, a następnie spróbuj ponownie.  |
|  TargetDiskBlobAlreadyExists  |  Nie można kontynuować operacji przechwytywania, ponieważ obiekt blob obrazu docelowego {0} ma aktywną dzierżawę.   |
|  TargetDiskBlobAlreadyExists  |  Obiekt blob {0} już istnieje. Podaj inny identyfikator URI obiektu blob jako cel dysku "{1}".  |
|  TooManyVMRedeploymentRequests  |  Zbyt dużą liczbę żądań odbieranych na maszynie Wirtualnej "{0}" lub maszyn wirtualnych w tym samym zestawie dostępności o tej maszyny Wirtualnej. Spróbuj ponownie później.  |
|  VHDSizeInvalid  |  Wartość rozmiaru dysku {0} dla dysku "{1}" z {2} obiektu blob jest nieprawidłowy. Rozmiar dysku musi należeć do zakresu od {3} i {4}.  |
|  VMAgentStatusCommunicationError  |  Maszyny Wirtualnej "{0}" nie zgłosiła stanu agenta maszyny Wirtualnej lub rozszerzenia. Sprawdź, czy maszyna wirtualna ma działającego agenta maszyny Wirtualnej i może nawiązywać wychodzące połączenia z magazynem platformy Azure.  |
|  VMArtifactRepositoryInternalError  |  Podczas komunikacji z repozytorium artefaktów w celu pobrania szczegółów artefaktu maszyny wirtualnej wystąpił błąd.  |
|  VMArtifactRepositoryInternalError  |  Wystąpił błąd wewnętrzny podczas pobierania danych artefaktu maszyny wirtualnej z repozytorium artefaktów.  |
|  VMExtensionHandlerNonTransientError  |  Program obsługi "{0}" zgłosiła błąd dla rozszerzenia maszyny Wirtualnej "{1}" z kodem błędu terminala "{2}" i komunikatem o błędzie: "{3}"  |
|  VMExtensionManagementInternalError  |  Podczas przetwarzania rozszerzenia maszyny wirtualnej „{0}” wystąpił błąd wewnętrzny.  |
|  VMExtensionManagementInternalError  |  Wystąpiło wiele błędów podczas przygotowywania rozszerzeń maszyny Wirtualnej. Zobacz widok wystąpienia rozszerzenia maszyny Wirtualnej, aby uzyskać szczegółowe informacje.  |
|  VMExtensionProvisioningError  |  Maszyna wirtualna zgłosiła błąd podczas przetwarzania rozszerzenia "{0}". Komunikat o błędzie: "{1}".  |
|  VMExtensionProvisioningError  |  Wiele rozszerzeń maszyny Wirtualnej nie może być inicjowana na Maszynie wirtualnej. Zobacz widok wystąpienia rozszerzenia maszyny Wirtualnej, aby uzyskać szczegółowe informacje.  |
|  VMExtensionProvisioningTimeout  |  Inicjowanie obsługi rozszerzenia maszyny Wirtualnej "{0}" został przekroczony. Instalacja rozszerzenia może trwać zbyt długo lub nie można uzyskać stanu rozszerzenia.  |
|  VMMarketplaceInvalidInput  |  Tworzenie maszyny wirtualnej z obrazu niepochodzącego z witryny Marketplace nie wymaga informacji o planie, Usuń informacji o planie w żądaniu. Nazwa dysku systemu operacyjnego to {0}.  |
|  VMMarketplaceInvalidInput  |  Informacje o zakupie nie jest zgodna. Nie można wdrożyć z obrazu witryny Marketplace. Nazwa dysku systemu operacyjnego to {0}.  |
|  VMMarketplaceInvalidInput  |  Tworzenie maszyny wirtualnej z obrazu witryny Marketplace wymaga informacji o planie w żądaniu. Nazwa dysku systemu operacyjnego to {0}.  |
|  VMNotFound  |  Nie można odnaleźć maszyny Wirtualnej "{0}".  |
|  VMRedeploymentFailed  |  Ponowne wdrożenie maszyny Wirtualnej "{0}" nie powiodło się z powodu błędu wewnętrznego. Spróbuj ponownie później.  |
|  VMRedeploymentTimedOut  |  Ponowne wdrożenie maszyny Wirtualnej "{0}" nie zostało zakończone w przydzielonym czasie. Go może zakończyła się pomyślnie w pewnym czasie. W przeciwnym wypadku można ponowić żądanie.  |
|  VMStartTimedOut  |  Nie można uruchomić maszyny Wirtualnej "{0}" w przydzielonym czasie. Maszyna wirtualna nadal może uruchomić się pomyślnie. Sprawdź później stan zasilania.  |


## <a name="next-steps"></a>Następne kroki
Jeśli potrzebujesz więcej pomocy, skontaktuj się ekspertów platformy Azure na [fora MSDN Azure i przepełnienie stosu](https://azure.microsoft.com/support/forums/). Alternatywnie można pliku zdarzenia pomocy technicznej platformy Azure. Przejdź do [witrynę pomocy technicznej platformy Azure](https://azure.microsoft.com/support/options/) i wybierz **uzyskać obsługuje**.