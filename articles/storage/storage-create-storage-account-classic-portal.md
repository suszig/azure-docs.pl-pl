<properties
    pageTitle="Sposoby tworzenia, zarządzania i usuwania konta magazynu w klasycznym Portalu Azure | Microsoft Azure"
    description="Utwórz nowe konto magazynu, zarządzaj kluczami dostępu do konta lub usuń konto magazynu w Portalu Azure. Więcej informacji na temat kont magazynu (warstwy Standardowa i Premium)."
    services="storage"
    documentationCenter=""
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="05/09/2016"
    ms.author="robinsh"/>


# Informacje o kontach magazynu Azure

[AZURE.INCLUDE [storage-selector-portal-create-storage-account](../../includes/storage-selector-portal-create-storage-account.md)]

## Omówienie

Konto usługi Azure Storage zapewnia dostęp do usług obiektów Blob, kolejek, tabel i plików platformy Azure w usłudze Azure Storage. Konto magazynu zapewnia unikatową przestrzeń nazw do przechowywania obiektów danych usługi Azure Storage. Domyślnie dane na Twoim koncie są dostępne tylko dla Ciebie, tj. właściciela konta.

Istnieją dwa typy kont magazynu:

- Standardowe konto magazynu obejmuje Magazyn obiektów Blob, tabel, kolejek i plików.
- Konto magazynu w warstwie Premium obecnie obsługuje wyłącznie dyski maszyn wirtualnych Azure. Zobacz temat [Premium Storage: High-Performance Storage for Azure Virtual Machine Workloads](storage-premium-storage.md) (Magazyn Premium: wysoce wydajny magazyn do obciążeń maszyn wirtualnych platformy Azure), aby uzyskać szczegółowe informacje o magazynie Premium.

## Rozliczanie konta usługi Storage

Rozliczenie jest przeprowadzane na podstawie wykorzystania usługi Azure Storage w oparciu o Twoje konto magazynu. Koszty usługi Storage są oparte na czterech czynnikach: pojemności magazynu, schematu replikacji, transakcjach magazynu i wyjścia danych.

- Pojemność usługi Storage dotyczy stopnia wykorzystania przydziału konta magazynu do przechowywania danych. Koszt samego przechowywania danych jest określany na podstawie faktycznej ilości przechowywanych danych oraz sposobu ich replikacji.
- Replikacja określa liczbę kopii danych, które są obsługiwane jednocześnie, i ich lokalizacje.
- Transakcje dotyczą wszystkich operacji odczytu i zapisu w usłudze Azure Storage.
- Wyjście danych dotyczy danych przekazywanych poza region świadczenia usługi Azure. Jeśli do danych na koncie magazynu uzyskuje się dostęp za pośrednictwem aplikacji, która nie działa w tym samym regionie (niezależnie od tego, czy jest to aplikacja w usłudze w chmurze lub inny rodzaj aplikacji), naliczane są opłaty za wyjście danych. (W przypadku usług Azure możesz wykonać czynności związane z grupowaniem danych i usług w tych samych centrach danych, aby zredukować lub całkowicie wyeliminować opłaty za wyjście danych).  

Strona [Cennik usługi Azure Storage](https://azure.microsoft.com/pricing/details/storage) zawiera szczegółowe informacje o cenach za pojemność magazynu, replikację i transakcje. Strona [Szczegóły cennika transferów danych](https://azure.microsoft.com/pricing/details/data-transfers/) zawiera szczegółowe informacje o cenach za wyjście danych.

Aby uzyskać szczegółowe informacje o celach dotyczących pojemności i wydajności konta magazynu, zobacz [Cele dotyczące skalowalności i wydajności usługi Azure Storage](storage-scalability-targets.md).

> [AZURE.NOTE] Kiedy tworzysz maszynę wirtualną platformy Azure, konto magazynu jest tworzone automatycznie w lokalizacji wdrożenia, jeśli jeszcze nie masz konta magazynu w tej lokalizacji. Dlatego też nie musisz wykonywać poniższych kroków, aby utworzyć konto magazynu dla dysków maszyny wirtualnej. Nazwa konta magazynu będzie opierać się na nazwie maszyny wirtualnej. Zobacz [dokumentację usługi Azure Virtual Machines](https://azure.microsoft.com/documentation/services/virtual-machines/), aby uzyskać więcej szczegółów.

## Tworzenie konta magazynu

1. Zaloguj się do [klasycznego Portalu Azure](https://manage.windowsazure.com).

2. Kliknij przycisk **Nowe** na pasku zadań u dołu strony. Wybierz kolejno pozycje **Usługi danych** | **Usługa Storage**, a następnie kliknij przycisk **Szybkie tworzenie**.

    ![Nowe konto magazynu](./media/storage-create-storage-account-classic-portal/storage_NewStorageAccount.png)

3. W polu **Adres URL** wprowadź nazwę konta magazynu.

    > [AZURE.NOTE] Nazwy kont usługi Magazyn muszą mieć długość od 3 do 24 znaków i mogą zawierać tylko cyfry i małe litery.
    >  
    > Nazwa konta magazynu musi być unikatowa w obrębie platformy Azure. Klasyczny Portal Azure poinformuje Cię, jeśli wybrana nazwa konta magazynu jest już zajęta.

    Zobacz sekcję [Punkty końcowe konta usługi Storage](#storage-account-endpoints) poniżej, aby uzyskać szczegółowe informacje o tym, w jaki sposób nazwa konta magazynu będzie wykorzystywana do adresowania obiektów w usłudze Azure Storage.

4. W pozycji **Grupa lokalizacji/koligacji** wybierz lokalizację dla konta magazynu, która jest blisko Ciebie lub Twoich klientów. Jeśli do danych na Twoim koncie magazynu uzyskuje się dostęp z innej usługi Azure, np. maszyny wirtualnej Azure lub usługi w chmurze, możesz wybrać grupę koligacji z listy, aby zgrupować konto magazynu w tym samym centrum danych z innymi usługami Azure, których używasz, w celu poprawienia wydajności i zredukowania kosztów.

    Pamiętaj, że grupę koligacji musisz wybrać podczas tworzenia konta magazynu. Nie możesz przenieść istniejącego konta do grupy koligacji. Aby uzyskać więcej informacji o grupach koligacji, zobacz sekcję [Wspólna lokalizacja usługi z grupą koligacji](#service-co-location-with-an-affinity-group) poniżej.

    >[AZURE.IMPORTANT] Aby określić lokalizacje dostępne dla Twojej subskrypcji, możesz wywołać operację [List all resource providers](https://msdn.microsoft.com/library/azure/dn790524.aspx) (Utwórz listę wszystkich dostawców zasobów). Aby przywołać listę dostawców w programie PowerShell, wywołaj polecenie [Get-AzureLocation](https://msdn.microsoft.com/library/azure/dn757693.aspx). W programie .NET użyj metody [List](https://msdn.microsoft.com/library/azure/microsoft.azure.management.resources.provideroperationsextensions.list.aspx) klasy ProviderOperationsExtensions.
    >
    >Ponadto zobacz temat [Regiony systemu Azure](https://azure.microsoft.com/regions/#services), aby uzyskać więcej informacji o tym, które usługi są dostępne w danym regionie.


5. Jeśli masz więcej niż jedną subskrypcję platformy Azure, zostanie wyświetlone pole **Subskrypcja**. W polu **Subskrypcja** wprowadź subskrypcję platformy Azure, która ma być używana z kontem magazynu.

6. W pozycji **Replikacja** wybierz żądany poziom replikacji dla konta magazynu. Zalecana opcja replikacji to replikacja geograficznie nadmiarowa, która zapewnia najwyższą trwałość danych. Aby uzyskać więcej szczegółów na temat opcji replikacji usługi Azure Storage, zobacz [Replikacja usługi Azure Storage](storage-redundancy.md).

6. Kliknij opcję **Utwórz konto usługi Storage**.

    Tworzenie konta magazynu może potrwać kilka minut. Aby sprawdzić stan, możesz monitorować powiadomienia wyświetlane w dolnej części klasycznego Portalu Azure. Po utworzeniu konta magazynu nowe konto magazynu ma stan **Online** i jest gotowe do użytku.

![Strona magazynu](./media/storage-create-storage-account-classic-portal/Storage_StoragePage.png)


### Punkty końcowe konta usługi Storage

Każdy obiekt, który jest przechowywany w usłudze Azure Storage, ma unikatowy adres URL. Nazwa konta magazynu tworzy poddomenę tego adresu. Kombinacja nazw poddomeny i domeny, która jest specyficzna dla poszczególnych usług, tworzy *punkt końcowy* konta magazynu.

Jeśli na przykład konto magazynu ma nazwę *mojekontomagazynu*, domyślnymi punktami końcowymi konta magazynu są:

- Usługa Blob: http://*mojekontomagazynu*.blob.core.windows.net

- Usługa tabel: http://*mojekontomagazynu*.table.core.windows.net

- Usługa kolejki: http://*mojekontomagazynu*.queue.core.windows.net

- Usługa plików http://*mojekontomagazynu*.file.core.windows.net

Po utworzeniu konta możesz zobaczyć punkty końcowe konta magazynu w pulpicie nawigacyjnym magazynu w [klasycznym Portalu Azure](https://manage.windowsazure.com).

Adres URL dostępu do obiektu w koncie magazynu jest tworzony przez dodanie lokalizacji obiektu na koncie magazynu do punktu końcowego. Przykładowo adres obiektu Blob może mieć następujący format: http://*mojekontomagazynu*.blob.core.windows.net/*mojkontener*/*mojblob*.

Możesz również skonfigurować niestandardową nazwę domeny do użycia ze swoim kontem magazynu. Zobacz temat [Configure a custom domain name for your blob storage endpoint](storage-custom-domain-name.md) (Konfigurowanie niestandardowej nazwy domeny dla punktu końcowego Magazynu obiektów Blob), aby uzyskać więcej szczegółów.

### Wspólna lokalizacja usługi z grupą koligacji

*Grupa koligacji* to sposób geograficznego grupowania usług i maszyn wirtualnych platformy Azure z kontem magazynu Azure. Grupa koligacji może poprawić wydajność usługi poprzez przydzielenie obciążeń komputerowych w tym samym centrum danych lub w pobliżu docelowych odbiorców użytkownika. Ponadto, jeśli do danych na koncie magazynu uzyskuje się dostęp z innej usługi, która jest częścią tej samej grupy koligacji, nie są naliczane opłaty za wyjście danych.

> [AZURE.NOTE]  Aby utworzyć grupę koligacji, otwórz obszar <b>Ustawienia</b> w [klasycznym Portalu Azure](https://manage.windowsazure.com), kliknij pozycję <b>Grupy koligacji</b>, a następnie kliknij pozycję <b>Dodaj grupę koligacji</b> lub przycisk <b>Dodaj</b>. Możesz też utworzyć grupy koligacji i zarządzać nimi za pomocą interfejsu API zarządzania usługami Azure. Aby uzyskać więcej informacji, zobacz temat <a href="http://msdn.microsoft.com/library/azure/ee460798.aspx">Operations on affinity groups</a> (Operacje na grupach koligacji).

## Wyświetlanie, kopiowanie i ponowne generowanie kluczy dostępu do magazynu

Podczas tworzenia konta magazynu platforma Azure generuje dwa 512-bitowe klucze dostępu do magazynu, które są wykorzystywane do uwierzytelniania podczas uzyskiwania dostępu do konta magazynu. Zapewniając dwa klucze dostępu do magazynu, platforma Azure umożliwia ponowne generowanie kluczy bez zakłóceń w usłudze magazynu lub przerw w dostępie do tej usługi.

> [AZURE.NOTE] Nie zaleca się udostępniania kluczy dostępu do magazynu innym osobom. Aby zezwolić na dostęp do zasobów magazynu bez przekazywania kluczy dostępu, możesz użyć *sygnatury dostępu współdzielonego*. Sygnatura dostępu współdzielonego zapewnia dostęp do zasobów na koncie przez określony czas i z określonymi uprawnieniami. Aby uzyskać więcej informacji, zobacz [Sygnatury dostępu współdzielonego: opis modelu sygnatur dostępu współdzielonego](storage-dotnet-shared-access-signature-part-1.md).

W [klasycznym Portalu Azure](https://manage.windowsazure.com) użyj opcji **Zarządzanie kluczami** na pulpicie nawigacyjnym lub strony **Usługa Storage**, aby wyświetlić, skopiować i ponownie wygenerować klucze dostępu do magazynu, które są wykorzystywane do zapewniania dostępu do usług obiektów Blob, tabel i kolejek.

### Kopiowanie klucza dostępu do magazynu  

Możesz użyć opcji **Zarządzanie kluczami**, aby skopiować klucz dostępu do magazynu w celu wykorzystania go w parametrach połączenia. Parametry połączenia wymagają nazwy konta magazynu oraz klucza do wykorzystania w procesie uwierzytelniania. Aby uzyskać informacje o konfigurowaniu parametrów połączenia w celu uzyskania dostępu do usług magazynu platformy Azure, zobacz temat [Konfiguracja parametrów połączenia usługi Azure Storage](storage-configure-connection-string.md).

1. W [klasycznym Portalu Azure](https://manage.windowsazure.com) kliknij opcję **Usługa Storage**, a następnie kliknij nazwę konta magazynu, aby otworzyć pulpit nawigacyjny.

2. Kliknij przycisk **Zarządzanie kluczami**.

    Otworzy się okno **Zarządzania kluczami dostępu**.

    ![Zarządzanie kluczami](./media/storage-create-storage-account-classic-portal/Storage_ManageKeys.png)


3. Aby skopiować klucz dostępu do magazynu, wybierz tekst klucza. Kliknij go prawym przyciskiem myszy, a następnie kliknij polecenie **Kopiuj**.

### Ponowne generowanie kluczy dostępu do magazynu
Zalecamy okresowe zmienienie kluczy dostępu do konta magazynu, aby zabezpieczyć połączenia z magazynem. Przydzielone są dwa klucze dostępu, więc możesz utrzymać łączność z kontem magazynu za pomocą jednego klucza dostępu, jednocześnie ponownie generując drugi klucz dostępu.

> [AZURE.WARNING] Ponowne generowanie kluczy dostępu może mieć wpływ na usługi na platformie Azure oraz Twoje aplikacje, które są zależne od konta magazynu. Wszyscy klienci, którzy używają klucza dostępu do uzyskiwania dostępu do konta magazynu, muszą zostać poinformowani o potrzebie użycia nowego klucza.

**Usługi multimediów** — jeśli masz usługi multimediów, które są zależne od konta magazynu, musisz ponownie zsynchronizować klucze dostępu z usługą multimediów po ponownym wygenerowaniu kluczy.

**Aplikacje** — jeśli masz aplikacje sieci Web lub usługi w chmurze, które korzystają z konta magazynu, w przypadku ponownego generowania kluczy utracisz połączenia, chyba że wdrożysz klucze. 

**Eksploratory usługi Storage** — jeśli używasz dowolnej [aplikacji eksploratora magazynu](storage-explorers.md), prawdopodobnie zajdzie potrzeba zaktualizowania klucza magazynu używanego przez te aplikacje.

Oto proces związany z rotacją kluczy dostępu do magazynu:

1. Zaktualizuj parametry połączenia w kodzie aplikacji, wprowadzając odwołanie do pomocniczego klucza dostępu do konta magazynu.

2. Ponownie wygeneruj podstawowy klucz dostępu do konta magazynu. W [klasycznym Portalu Azure](https://manage.windowsazure.com) na pulpicie nawigacyjnym lub na stronie **Konfiguracja** kliknij opcję **Zarządzanie kluczami**. Kliknij pozycję **Generuj ponownie** dla podstawowego klucza dostępu, a następnie kliknij przycisk **Tak**, aby potwierdzić wygenerowanie nowego klucza.

3. Zaktualizuj parametry połączenia w kodzie, wprowadzając odwołanie do nowego podstawowego klucza dostępu.

4. Wygeneruj ponownie pomocniczy klucz dostępu.

## Usuwanie konta magazynu

Aby usunąć konto magazynu, którego już nie używasz, użyj polecenia **Usuń** na pulpicie nawigacyjnym lub na stronie **Konfiguracja**. Polecenie **Usuń** usuwa całe konto magazynu, w tym wszystkie obiekty Blob, tabele i kolejki konta.

> [AZURE.WARNING] Nie można przywrócić usuniętego konta magazynu ani odzyskać żadnej zawartości znajdującej się na koncie przed usunięciem. Zanim usuniesz konto, wykonaj kopię zapasową wszystkich danych, które chcesz zapisać. Dotyczy to również wszystkich zasobów na koncie — po usunięciu obiektu Blob, tabeli, kolejki lub pliku elementy te są trwale usuwane.
>
> Jeśli Twoje konto magazynu zawiera pliki VHD dla maszyny wirtualnej Azure, przed usunięciem konta magazynu musisz usunąć wszystkie obrazy i dyski, które korzystają z tych plików VHD. Najpierw zatrzymaj maszynę wirtualną, jeśli jest uruchomiona, a następnie usuń ją. Aby usunąć dyski, przejdź do karty **Dyski** i usuń wszystkie dyski w tym miejscu. Aby usunąć obrazy, przejdź do karty **Obrazy** i usuń wszelkie obrazy przechowywane na koncie.

1. W [klasycznym Portalu Azure](https://manage.windowsazure.com) kliknij pozycję **Usługa Storage**.

2. Kliknij w dowolnym miejscu wpisu konta magazynu (poza nazwą), a następnie kliknij przycisk **Usuń**.

     — Lub —

    Kliknij nazwę konta magazynu, aby otworzyć pulpit nawigacyjny, a następnie kliknij pozycję **Usuń**.

3. Kliknij przycisk **Tak**, aby potwierdzić, że chcesz usunąć konto magazynu.

## Następne kroki

- Aby dowiedzieć się więcej na temat usługi Azure Storage, zobacz [dokumentację usługi Azure Storage](https://azure.microsoft.com/documentation/services/storage/).
- Odwiedź [Blog zespołu odpowiedzialnego za usługę Azure Storage](http://blogs.msdn.com/b/windowsazurestorage/).
- [Transfer danych za pomocą narzędzia wiersza polecenia AzCopy](storage-use-azcopy.md)



<!--HONumber=Jun16_HO2-->


