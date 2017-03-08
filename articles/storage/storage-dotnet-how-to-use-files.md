---
title: "Rozpoczynanie pracy z usługą Azure File Storage w systemie Windows | Microsoft Docs"
description: "Usługa Magazyn plików Azure umożliwia przechowywanie plików danych w chmurze oraz instalowanie udziału plików w chmurze z maszyny wirtualnej platformy Azure lub z aplikacji lokalnych działających w systemie Windows."
services: storage
documentationcenter: .net
author: RenaShahMSFT
manager: aungoo
editor: tysonn
ms.assetid: 6a889ee1-1e60-46ec-a592-ae854f9fb8b6
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
/ms.date: 1/18/2017
ms.author: renash
translationtype: Human Translation
ms.sourcegitcommit: 4e81088857c0e9cacaf91342227ae63080fc90c5
ms.openlocfilehash: 780066b1e71d967c64da0a1c1a284ffd5d1b7481
ms.lasthandoff: 02/23/2017


---
# <a name="get-started-with-azure-file-storage-on-windows"></a>Rozpoczynanie pracy z Magazynem plików Azure w systemie Windows
[!INCLUDE [storage-selector-file-include](../../includes/storage-selector-file-include.md)]

[!INCLUDE [storage-check-out-samples-dotnet](../../includes/storage-check-out-samples-dotnet.md)]

[!INCLUDE [storage-file-overview-include](../../includes/storage-file-overview-include.md)]

Aby uzyskać informacje na temat używania usługi File Storage w systemie Linux, zobacz [How to use Azure File Storage with Linux](storage-how-to-use-files-linux.md) (Jak korzystać z usługi Azure File Storage w systemie Linux).

Aby uzyskać szczegółowe informacje o celach dotyczących skalowalności i wydajności usługi File Storage, zobacz [Azure Storage Scalability and Performance Targets](storage-scalability-targets.md#scalability-targets-for-blobs-queues-tables-and-files) (Cele dotyczące skalowalności i wydajności usługi Azure Storage).

[!INCLUDE [storage-dotnet-client-library-version-include](../../includes/storage-dotnet-client-library-version-include.md)]

[!INCLUDE [storage-file-concepts-include](../../includes/storage-file-concepts-include.md)]

## <a name="video-using-azure-file-storage-with-windows"></a>Wideo: Korzystanie z usługi Magazyn plików Azure w systemie Windows
W tym filmie przedstawiamy, jak utworzyć udziały plików platformy Azure i korzystać z nich w systemie Windows.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-File-Storage-with-Windows/player]
> 
> 

## <a name="about-this-tutorial"></a>Informacje o tym samouczku
W tym samouczku wprowadzającym zostały omówione podstawy korzystania z usługi Magazyn plików Microsoft Azure. W tym samouczku zostaną wykonane następujące czynności:

* Utworzenie nowego udziału plików platformy Azure za pomocą Portalu Azure lub programu PowerShell, dodanie katalogu, przekazanie pliku lokalnego do udziału oraz wyświetlenie listy plików w katalogu.
* Instalacja udziału plików tak samo jak instaluje się udziały SMB.
* Uzyskanie dostępu do udziału pliku z poziomu aplikacji lokalnej za pomocą biblioteki klienta usługi Azure Storage dla programu .NET. Utworzenie aplikacji konsolowej i wykonanie następujących akcji dotyczących udziału plików:
  * Zapisanie zawartości pliku w udziale w oknie konsoli.
  * Ustawienie limitu przydziału (maksymalnego rozmiaru) udziału plików.
  * Utworzenie sygnatury dostępu współdzielonego dla pliku, która używa zasad dostępu współdzielonego zdefiniowanych w udziale.
  * Skopiowanie pliku do innego pliku w tym samym koncie magazynu.
  * Skopiowanie pliku do obiektu blob w tym samym koncie magazynu.
* Rozwiązywanie problemów przy użyciu metryk usługi Azure Storage.

Usługa Magazyn plików jest teraz obsługiwana dla wszystkich kont magazynu, można więc użyć istniejącego konta magazynu lub utworzyć nowe konto. Aby uzyskać informacje dotyczące tworzenia nowego konta magazynu, zobacz [How to create a storage account](storage-create-storage-account.md#create-a-storage-account) (Jak utworzyć konto magazynu).

## <a name="use-the-azure-portal-to-manage-a-file-share"></a>Zarządzanie udziałem plików za pomocą Portalu Azure
Udziałami plików można zarządzać z poziomu interfejsu użytkownika w witrynie [Azure Portal](https://portal.azure.com). Portal umożliwia wykonywanie następujących czynności:

* Tworzenie udziału plików
* Przekazywanie i pobieranie plików do i z udziału plików
* Monitorowanie rzeczywistego użycia każdego udziału plików
* Dostosowywanie limitu przydziału rozmiaru udziału
* Instalowanie udziału plików z poziomu klienta systemu Windows za pomocą polecenia `net use`

### <a name="create-file-share"></a>Tworzenie udziału plików
1. Zaloguj się do Portalu Azure.
2. W menu nawigacyjnym kliknij pozycję **Konta usługi Storage** lub **Konta usługi Storage (klasyczne)**.
   
    ![Zrzut ekranu pokazujący sposób tworzenia udziału plików w portalu](./media/storage-dotnet-how-to-use-files/files-create-share-0.png)
3. Wybierz konto magazynu.
   
    ![Zrzut ekranu pokazujący sposób tworzenia udziału plików w portalu](./media/storage-dotnet-how-to-use-files/files-create-share-1.png)
4. Wybierz usługę „Pliki”.
   
    ![Zrzut ekranu pokazujący sposób tworzenia udziału plików w portalu](./media/storage-dotnet-how-to-use-files/files-create-share-2.png)
5. Kliknij pozycję „Udziały plików”, a następnie kliknij link w celu utworzenia pierwszego udziału plików.
   
    ![Zrzut ekranu pokazujący sposób tworzenia udziału plików w portalu](./media/storage-dotnet-how-to-use-files/files-create-share-3.png)
6. Wypełnij pola nazwy i rozmiaru udziału plików (do 5120 GB) w celu utworzenia pierwszego udziału plików. Po utworzeniu udziału plików możesz go zainstalować z dowolnego systemu plików, który obsługuje protokół SMB 2.1 lub SMB 3.0.
   
    ![Zrzut ekranu pokazujący sposób tworzenia udziału plików w portalu](./media/storage-dotnet-how-to-use-files/files-create-share-4.png)

### <a name="upload-and-download-files"></a>Przekazywanie i pobieranie plików
1. Wybierz utworzony udział plików.
   
    ![Zrzut ekranu pokazujący sposób przekazywania i pobierania plików z portalu](./media/storage-dotnet-how-to-use-files/files-upload-download-1.png)
2. Kliknij przycisk **Przekaż**, aby otworzyć interfejs użytkownika funkcji przekazywania plików.
   
    ![Zrzut ekranu pokazujący sposób przekazywania plików z portalu](./media/storage-dotnet-how-to-use-files/files-upload-download-2.png)
3. Kliknij prawym przyciskiem myszy plik i wybierz polecenie **Pobierz**, aby pobrać go do udziału lokalnego.
   
    ![Zrzut ekranu pokazujący sposób pobierania plików z portalu](./media/storage-dotnet-how-to-use-files/files-upload-download-3.png)

### <a name="manage-file-share"></a>Zarządzanie udziałem plików
1. Kliknij przycisk **Limit przydziału**, aby zmienić rozmiar udziału plików (do 5120 GB).
   
    ![Zrzut ekranu pokazujący sposób konfigurowania limitu przydziału udziału plików](./media/storage-dotnet-how-to-use-files/files-manage-1.png)
2. Kliknij przycisk **Połącz**, aby wyświetlić wiersz polecenia umożliwiający zainstalowanie udziału plików z systemu Windows.
   
    ![Zrzut ekranu pokazujący sposób instalacji udziału plików](./media/storage-dotnet-how-to-use-files/files-manage-2.png)
   
    ![Zrzut ekranu pokazujący sposób instalacji udziału plików](./media/storage-dotnet-how-to-use-files/files-manage-3.png)
   
   > [!TIP]
   > Aby znaleźć klucz dostępu do konta magazynu na potrzeby instalacji, kliknij przycisk **Ustawienia** w sekcji konta magazynu, a następnie kliknij pozycję **Klucze dostępu**.
   > 
   > 
   
    ![Zrzut ekranu pokazujący, gdzie znaleźć klucz dostępu do konta magazynu](./media/storage-dotnet-how-to-use-files/files-manage-4.png)
   
    ![Zrzut ekranu pokazujący, gdzie znaleźć klucz dostępu do konta magazynu](./media/storage-dotnet-how-to-use-files/files-manage-5.png)

## <a name="use-powershell-to-manage-a-file-share"></a>Zarządzanie udziałem plików za pomocą programu PowerShell
Tworzenie udziałów plików i zarządzanie nimi jest także możliwe przy użyciu programu Azure PowerShell.

### <a name="install-the-powershell-cmdlets-for-azure-storage"></a>Instalowanie poleceń cmdlet programu PowerShell dla usługi Azure Storage
Aby przygotować się do użycia programu Azure PowerShell, pobierz i zainstaluj polecenia cmdlet tego programu. Aby uzyskać informacje o punkcie instalacji oraz instrukcje dotyczące instalacji, zobacz [How to install and configure Azure PowerShell](/powershell/azureps-cmdlets-docs) (Jak zainstalować i skonfigurować program Azure PowerShell).

> [!NOTE]
> Zalecamy pobranie i zainstalowanie najnowszej wersji modułu Azure PowerShell (lub uaktualnienie do tej wersji).
> 
> 

Otwórz okno programu Azure PowerShell, klikając przycisk **Start** i wpisując polecenie **Windows PowerShell**. W oknie programu PowerShell zostanie załadowany moduł Azure PowerShell.

### <a name="create-a-context-for-your-storage-account-and-key"></a>Tworzenie kontekstu konta magazynu i klucza
Teraz utworzymy kontekst konta magazynu. W kontekście zawarta jest nazwa konta magazynu oraz klucz konta. Aby uzyskać instrukcje dotyczące kopiowania klucza konta z witryny [Azure Portal](https://portal.azure.com), zobacz [Wyświetlanie i kopiowanie kluczy dostępu do magazynu](storage-create-storage-account.md#view-and-copy-storage-access-keys).

W poniższym przykładzie zastąp zmienne `storage-account-name` i `storage-account-key` nazwą konta magazynu i kluczem.

```powershell
# create a context for account and key
$ctx=New-AzureStorageContext storage-account-name storage-account-key
```

### <a name="create-a-new-file-share"></a>Tworzenie nowego udziału plików
Następnym krokiem jest utworzenie nowego udziału o nazwie `logs`.

```powershell
# create a new share
$s = New-AzureStorageShare logs -Context $ctx
```

Masz już udział plików w usłudze Magazyn plików. Teraz dodamy katalog i plik.

> [!IMPORTANT]
> Nazwa udziału plików musi się składać z samych małych liter. Szczegółowe informacje o nazwach plików i udziałów plików można znaleźć w temacie [Naming and Referencing Shares, Directories, Files, and Metadata](https://msdn.microsoft.com/library/azure/dn167011.aspx) (Nazywanie i odwoływanie się do udziałów, katalogów, plików i metadanych).
> 
> 

### <a name="create-a-directory-in-the-file-share"></a>Tworzenie katalogu w udziale plików
Następnym krokiem będzie utworzenie katalogu w udziale. W poniższym przykładzie katalog nosi nazwę `CustomLogs`.

```powershell
# create a directory in the share
New-AzureStorageDirectory -Share $s -Path CustomLogs
```

### <a name="upload-a-local-file-to-the-directory"></a>Przekazywanie pliku lokalnego do katalogu
Teraz przekażemy plik lokalny do katalogu. W poniższym przykładzie plik zostanie przekazany z lokalizacji `C:\temp\Log1.txt`. Zmień ścieżkę pliku tak, aby wskazywała prawidłowy plik na komputerze lokalnym.

```powershell
# upload a local file to the new directory
Set-AzureStorageFileContent -Share $s -Source C:\temp\Log1.txt -Path CustomLogs
```

### <a name="list-the-files-in-the-directory"></a>Wyświetlanie listy plików w katalogu
Aby zobaczyć plik w katalogu, możesz wyświetlić listę wszystkich plików w tym katalogu. To polecenie zwraca pliki i podkatalogi (jeśli istnieją) w katalogu CustomLogs.

```powershell
# list files in the new directory
Get-AzureStorageFile -Share $s -Path CustomLogs | Get-AzureStorageFile
```

Polecenie Get-AzureStorageFile zwraca listę plików i katalogów dla dowolnego przekazanego obiektu katalogu. Polecenie „Get-AzureStorageFile -Share $s” zwraca listę plików i katalogów w katalogu głównym. Aby uzyskać listę plików w podkatalogu, trzeba przekazać nazwę podkatalogu do polecenia Get-AzureStorageFile. Tak właśnie działa powyższy zapis — pierwsza część polecenia do kreski pionowej zwraca wystąpienie katalogu dla podkatalogu CustomLogs. Następnie jest ono przekazywane do polecenia Get-AzureStorageFile, które zwraca pliki i katalogi w podkatalogu CustomLogs.

### <a name="copy-files"></a>Kopiowanie plików
Począwszy od wersji 0.9.7 programu Azure PowerShell, można kopiować pliki do innych plików, pliki do obiektów blob oraz obiekty blob do plików. Poniżej przedstawiamy sposób wykonywania tych operacji kopiowania za pomocą poleceń cmdlet programu PowerShell.

```powershell
# copy a file to the new directory
Start-AzureStorageFileCopy -SrcShareName srcshare -SrcFilePath srcdir/hello.txt -DestShareName destshare -DestFilePath destdir/hellocopy.txt -Context $srcCtx -DestContext $destCtx

# copy a blob to a file directory
Start-AzureStorageFileCopy -SrcContainerName srcctn -SrcBlobName hello2.txt -DestShareName hello -DestFilePath hellodir/hello2copy.txt -DestContext $ctx -Context $ctx
```

## <a name="mount-the-file-share"></a>Instalowanie udziału plików
Po dodaniu obsługi protokołu SMB 3.0 usługa Magazyn plików obsługuje teraz szyfrowanie i dojścia stałe od klientów protokołu SMB 3.0. Obsługa szyfrowania oznacza, że klienci protokołu SMB 3.0 mogą zainstalować udział plików z dowolnego miejsca, takiego jak:

* Maszyna wirtualna platformy Azure w tym samym regionie (również obsługiwana przez protokół SMB 2.1)
* Maszyna wirtualna platformy Azure w innym regionie (tylko protokół SMB 3.0)
* Lokalna aplikacja kliencka (tylko protokół SMB 3.0)

Gdy klient uzyskuje dostęp do usługi Magazyn plików, używana wersja protokołu SMB zależy od wersji protokołu SMB obsługiwanej przez system operacyjny. Poniższa tabela zawiera podsumowanie obsługi protokołu SMB dla klientów systemu Windows. Więcej szczegółów na temat [wersji protokołu SMB](http://blogs.technet.com/b/josebda/archive/2013/10/02/windows-server-2012-r2-which-version-of-the-smb-protocol-smb-1-0-smb-2-0-smb-2-1-smb-3-0-or-smb-3-02-you-are-using.aspx) zawiera ten blog.

| Klient systemu Windows | Obsługiwana wersja protokołu SMB |
|:--- |:--- |
| Windows 7 |SMB 2.1 |
| Windows Server 2008 R2 |SMB 2.1 |
| Windows 8 |SMB 3.0 |
| Windows Server 2012 |SMB 3.0 |
| Windows Server 2012 R2 |SMB 3.0 |
| Windows 10 |SMB 3.0 |

### <a name="mount-the-file-share-from-an-azure-virtual-machine-running-windows"></a>Instalowanie udziału plików z maszyny wirtualnej platformy Azure z systemem Windows
Aby zademonstrować sposób instalacji udziału plików na platformie Azure, utworzymy teraz maszynę wirtualną platformy Azure z systemem Windows i połączymy się z nią zdalnie w celu zainstalowania udziału

1. Najpierw utwórz nową maszynę wirtualną platformy Azure, postępując zgodnie z instrukcjami w artykule [Tworzenie pierwszej maszyny wirtualnej z systemem Windows w witrynie Azure Portal](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
2. Następnie nawiąż połączenie zdalne z maszyną wirtualną, postępując zgodnie z instrukcjami w artykule [Logowanie na maszynie wirtualnej z systemem Windows za pomocą witryny Azure Portal](../virtual-machines/virtual-machines-windows-connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
3. Otwórz okno programu PowerShell na maszynie wirtualnej.

### <a name="persist-your-storage-account-credentials-for-the-virtual-machine"></a>Utrwalanie poświadczeń konta magazynu dla maszyny wirtualnej
Przed zainstalowaniem w udziale plików najpierw musisz utrwalić poświadczenia konta magazynu na maszynie wirtualnej. Ten krok umożliwia systemowi Windows automatyczne ponowne połączenie z udziałem plików po ponownym rozruchu maszyny wirtualnej. Aby utrwalić poświadczenia konta, uruchom polecenie `cmdkey` w oknie programu PowerShell na maszynie wirtualnej. Zastąp ciąg `<storage-account-name>` nazwą konta magazynu, a ciąg `<storage-account-key>` kluczem konta magazynu. Musisz jawnie wskazać domenę „AZURE”, tak jak w poniższym przykładzie. 

```
cmdkey /add:<storage-account-name>.file.core.windows.net /user:AZURE\<storage-account-name> /pass:<storage-account-key>
```

System Windows będzie teraz ponownie łączyć się z udziałem plików po ponownym rozruchu maszyny wirtualnej. Możesz sprawdzić, czy udział został ponownie podłączony, uruchamiając polecenie `net use` z okna programu PowerShell.

Pamiętaj, że poświadczenia są trwałe tylko w kontekście, w którym uruchomiono polecenie `cmdkey`. Jeśli tworzysz aplikację, która działa jako usługa, musisz utrwalić poświadczenia także w tym kontekście.

### <a name="mount-the-file-share-using-the-persisted-credentials"></a>Instalowanie udziału plików za pomocą utrwalonych poświadczeń
Po nawiązaniu połączenia zdalnego z maszyną wirtualną można uruchomić polecenie `net use`, aby zainstalować udział plików, używając następującej składni. Zastąp ciąg `<storage-account-name>` nazwą konta magazynu, a ciąg `<share-name>` nazwą udziału usługi File Storage.

```
net use <drive-letter>: \\<storage-account-name>.file.core.windows.net\<share-name>

example :
net use z: \\samples.file.core.windows.net\logs
```

Ponieważ poświadczenia konta magazynu zostały utrwalone w poprzednim kroku, nie trzeba podawać ich w poleceniu `net use`. Jeśli poświadczenia nie zostały jeszcze utrwalone, dołącz je jako parametr przekazywany do polecenia `net use`, jak pokazano w poniższym przykładzie.

```
net use <drive-letter>: \\<storage-account-name>.file.core.windows.net\<share-name> /u:AZURE\<storage-account-name> <storage-account-key>

example :
net use z: \\samples.file.core.windows.net\logs /u:AZURE\samples <storage-account-key>
```

Teraz możesz pracować z udziałem usługi File Storage z poziomu maszyny wirtualnej tak samo jak z każdym innym dyskiem. Możesz wydawać standardowe polecenia dotyczące plików z wiersza polecenia lub wyświetlić zainstalowany udział i jego zawartość w Eksploratorze plików. Ponadto możesz uruchomić na maszynie wirtualnej kod, który będzie uzyskiwać dostęp do udziału plików przy użyciu standardowych interfejsów API we/wy systemu Windows dotyczących plików, np. udostępnianych przez [przestrzenie nazw System.IO](http://msdn.microsoft.com/library/gg145019.aspx) w programie .NET Framework.

Udział plików można również zainstalować z poziomu roli uruchomionej w usłudze w chmurze na platformie Azure, łącząc się z tą rolą zdalnie.

### <a name="mount-the-file-share-from-an-on-premises-client-running-windows"></a>Instalowanie udziału plików z klienta lokalnego z systemem Windows
Aby zainstalować udział plików z klienta lokalnego, musisz najpierw wykonać następujące czynności:

* Zainstaluj wersję systemu Windows, która obsługuje protokół SMB 3.0. System Windows będzie korzystać z szyfrowania w protokole SMB 3.0 do bezpiecznego transferu danych między klientem lokalnym a udziałem plików na platformie Azure w chmurze.
* Otwórz dostęp do Internetu przez port 445 (dla ruchu wychodzącego TCP) w sieci lokalnej zgodnie z wymaganiami protokołu SMB.

> [!NOTE]
> Niektórzy usługodawcy internetowi mogą blokować port 445 — może więc być konieczny kontakt z usługodawcą.
> 
> 

## <a name="develop-with-file-storage"></a>Pisanie kodu z użyciem usługi Magazyn plików
Aby napisać kod, który wywołuje usługę Magazyn plików, można użyć bibliotek klienckich magazynu dla platform .NET i Java lub interfejsu API REST usługi Magazyn Azure. W przykładzie w tej sekcji pokazano sposób pracy z udziałem plików przy użyciu [biblioteki klienta usługi Storage Azure dla programu .NET](https://msdn.microsoft.com/library/mt347887.aspx) z prostej aplikacji konsolowej działającej na komputerze.

### <a name="create-the-console-application-and-obtain-the-assembly"></a>Tworzenie aplikacji konsolowej i uzyskiwanie zestawu
Aby utworzyć nową aplikację konsolową w programie Visual Studio i zainstalować pakiet NuGet zawierający bibliotekę klienta usługi Azure Storage:

1. W programie Visual Studio wybierz pozycje **Plik > Nowy projekt**, a następnie wybierz pozycje **Windows > Aplikacja konsolowa** z listy szablonów Visual C#.
2. Podaj nazwę aplikacji konsolowej, a następnie kliknij przycisk **OK**.
3. Po utworzeniu projektu kliknij go prawym przyciskiem myszy w Eksploratorze rozwiązań i wybierz polecenie **Zarządzaj pakietami NuGet**. Wyszukaj w trybie online ciąg „WindowsAzure.Storage”, a następnie kliknij przycisk **Zainstaluj**, aby zainstalować pakiet i zależności biblioteki klienta usługi Azure Storage dla programu .NET.

W przykładach kodu w tym artykule jest także używana [biblioteka programu Microsoft Azure Configuration Manager](https://msdn.microsoft.com/library/azure/mt634646.aspx), za pomocą której z pliku app.config aplikacji konsolowej pobierane są parametry połączenia magazynu. Program Azure Configuration Manager umożliwia pobranie parametrów połączenia w środowisku uruchomieniowym niezależnie od tego, czy aplikacja działa na platformie Microsoft Azure, na komputerze, urządzeniu przenośnym czy w sieci Web.

Aby zainstalować pakiet programu Azure Configuration Manager, kliknij prawym przyciskiem myszy projekt w Eksploratorze rozwiązań i wybierz polecenie **Zarządzaj pakietami NuGet**. Wyszukaj w trybie online ciąg „ConfigurationManager” i kliknij przycisk **Zainstaluj**, aby zainstalować pakiet.

Użycie programu Azure Configuration Manager jest opcjonalne. Można także użyć interfejsu API, np. [klasy ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager.aspx) programu .NET Framework.

### <a name="save-your-storage-account-credentials-to-the-appconfig-file"></a>Zapisywanie poświadczeń konta magazynu w pliku app.config
Teraz zapiszemy poświadczenia w pliku app.config projektu. Zmodyfikuj plik app.config, jak pokazano w poniższym przykładzie, zastępując ciąg `myaccount` nazwą konta magazynu, a ciąg `mykey` kluczem konta magazynu.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
    <startup>
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5" />
    </startup>
    <appSettings>
        <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=StorageAccountKeyEndingIn==" />
    </appSettings>
</configuration>
```

> [!NOTE]
> Najnowsza wersja emulatora magazynu Azure nie obsługuje usługi Magazyn plików. Aby pracować z usługą Magazyn plików, parametry połączenia muszą wskazywać konto magazynu platformy Azure w chmurze.
> 
> 

### <a name="add-namespace-declarations"></a>Dodawanie deklaracji przestrzeni nazw
Otwórz plik `program.cs` w Eksploratorze rozwiązań i dodaj następujące deklaracje przestrzeni nazw na początku pliku.

```csharp
using Microsoft.Azure; // Namespace for Azure Configuration Manager
using Microsoft.WindowsAzure.Storage; // Namespace for Storage Client Library
using Microsoft.WindowsAzure.Storage.Blob; // Namespace for Blob storage
using Microsoft.WindowsAzure.Storage.File; // Namespace for File storage
```

[!INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]

### <a name="access-the-file-share-programmatically"></a>Programowy dostęp do udziału plików
Teraz dodamy poniższy kod do metody `Main()` (po kodzie pokazanym powyżej) w celu pobrania parametrów połączenia. Ten kod pobiera odwołanie do pliku utworzonego wcześniej i wyświetla jego zawartość w oknie konsoli.

```csharp
// Create a CloudFileClient object for credentialed access to File storage.
CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

// Get a reference to the file share we created previously.
CloudFileShare share = fileClient.GetShareReference("logs");

// Ensure that the share exists.
if (share.Exists())
{
    // Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.GetRootDirectoryReference();

    // Get a reference to the directory we created previously.
    CloudFileDirectory sampleDir = rootDir.GetDirectoryReference("CustomLogs");

    // Ensure that the directory exists.
    if (sampleDir.Exists())
    {
        // Get a reference to the file we created previously.
        CloudFile file = sampleDir.GetFileReference("Log1.txt");

        // Ensure that the file exists.
        if (file.Exists())
        {
            // Write the contents of the file to the console window.
            Console.WriteLine(file.DownloadTextAsync().Result);
        }
    }
}
```

Aby zobaczyć dane wyjściowe, uruchom aplikację konsolową.

### <a name="set-the-maximum-size-for-a-file-share"></a>Ustawianie maksymalnego rozmiaru udziału plików
Począwszy od wersji 5.x biblioteki klienta usługi Azure Storage, można ustawić limit przydziału (lub maksymalny rozmiar) udziału plików w gigabajtach. Można również sprawdzić, ile danych jest obecnie przechowywanych w udziale.

Ustawiając limit przydziału dla udziału, można ograniczyć całkowity rozmiar plików przechowywanych w udziale. Jeśli całkowity rozmiar plików w udziale przekroczy ustawiony limit przydziału, klienci nie będą mogli zwiększyć rozmiaru istniejących plików ani tworzyć nowych plików (chyba że pliki będą puste).

W poniższym przykładzie pokazano, jak sprawdzić bieżące użycie udziału oraz jak ustawić limit przydziału w udziale.

```csharp
// Parse the connection string for the storage account.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create a CloudFileClient object for credentialed access to File storage.
CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

// Get a reference to the file share we created previously.
CloudFileShare share = fileClient.GetShareReference("logs");

// Ensure that the share exists.
if (share.Exists())
{
    // Check current usage stats for the share.
    // Note that the ShareStats object is part of the protocol layer for the File service.
    Microsoft.WindowsAzure.Storage.File.Protocol.ShareStats stats = share.GetStats();
    Console.WriteLine("Current share usage: {0} GB", stats.Usage.ToString());

    // Specify the maximum size of the share, in GB.
    // This line sets the quota to be 10 GB greater than the current usage of the share.
    share.Properties.Quota = 10 + stats.Usage;
    share.SetProperties();

    // Now check the quota for the share. Call FetchAttributes() to populate the share's properties.
    share.FetchAttributes();
    Console.WriteLine("Current share quota: {0} GB", share.Properties.Quota);
}
```

### <a name="generate-a-shared-access-signature-for-a-file-or-file-share"></a>Generowanie sygnatury dostępu współdzielonego dla pliku lub udziału plików
Począwszy od wersji 5.x biblioteki klienta usługi Azure Storage, można wygenerować sygnaturę dostępu współdzielonego dla udziału plików lub dla pojedynczego pliku. Można też utworzyć zasady dostępu współdzielonego w udziale plików na potrzeby zarządzania sygnaturami dostępu współdzielonego. Utworzenie zasad dostępu współdzielonego jest zalecane, ponieważ umożliwia cofnięcie sygnatur w przypadku zagrożenia bezpieczeństwa.

W poniższym przykładzie tworzone są zasady dostępu współdzielonego w udziale. Następnie za pomocą tych zasad nakładane są ograniczenia na sygnatury dostępu współdzielonego w pliku w udziale.

```csharp
// Parse the connection string for the storage account.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create a CloudFileClient object for credentialed access to File storage.
CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

// Get a reference to the file share we created previously.
CloudFileShare share = fileClient.GetShareReference("logs");

// Ensure that the share exists.
if (share.Exists())
{
    string policyName = "sampleSharePolicy" + DateTime.UtcNow.Ticks;

    // Create a new shared access policy and define its constraints.
    SharedAccessFilePolicy sharedPolicy = new SharedAccessFilePolicy()
        {
            SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24),
            Permissions = SharedAccessFilePermissions.Read | SharedAccessFilePermissions.Write
        };

    // Get existing permissions for the share.
    FileSharePermissions permissions = share.GetPermissions();

    // Add the shared access policy to the share's policies. Note that each policy must have a unique name.
    permissions.SharedAccessPolicies.Add(policyName, sharedPolicy);
    share.SetPermissions(permissions);

    // Generate a SAS for a file in the share and associate this access policy with it.
    CloudFileDirectory rootDir = share.GetRootDirectoryReference();
    CloudFileDirectory sampleDir = rootDir.GetDirectoryReference("CustomLogs");
    CloudFile file = sampleDir.GetFileReference("Log1.txt");
    string sasToken = file.GetSharedAccessSignature(null, policyName);
    Uri fileSasUri = new Uri(file.StorageUri.PrimaryUri.ToString() + sasToken);

    // Create a new CloudFile object from the SAS, and write some text to the file.
    CloudFile fileSas = new CloudFile(fileSasUri);
    fileSas.UploadText("This write operation is authenticated via SAS.");
    Console.WriteLine(fileSas.DownloadText());
}
```

Aby uzyskać więcej informacji na temat tworzenia i używania sygnatur dostępu współdzielonego, zobacz [Using Shared Access Signatures (SAS)](storage-dotnet-shared-access-signature-part-1.md) (Używanie sygnatur dostępu współdzielonego (SAS)) oraz [Create and use a SAS with Blob storage](storage-dotnet-shared-access-signature-part-2.md) (Tworzenie i używanie sygnatury dostępu współdzielonego w Magazynie obiektów Blob).

### <a name="copy-files"></a>Kopiowanie plików
Począwszy od wersji 5.x biblioteki klienta usługi Azure Storage, można kopiować pliki do innych plików, pliki do obiektów blob oraz obiekty blob do plików. W kolejnych sekcjach pokażemy, jak programowo wykonywać te operacje kopiowania.

Do kopiowania plików do innych plików oraz obiektów blob do plików i odwrotnie można także użyć narzędzia AzCopy. Zobacz: [Transfer danych za pomocą narzędzia wiersza polecenia AzCopy](storage-use-azcopy.md).

> [!NOTE]
> W przypadku kopiowania obiektu blob do pliku lub pliku do obiektu blob konieczne jest uwierzytelnienie obiektu źródłowego za pomocą sygnatury dostępu współdzielonego, nawet jeśli kopiowanie odbywa się w ramach tego samego konta magazynu.
> 
> 

**Kopiowanie pliku do innego pliku**

Poniższy przykładowy kod kopiuje plik do innego pliku w tym samym udziale. Ponieważ ta operacja kopiowania jest wykonywana w ramach tego samego konta magazynu, można użyć uwierzytelniania przy użyciu klucza wspólnego.

```csharp
// Parse the connection string for the storage account.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create a CloudFileClient object for credentialed access to File storage.
CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

// Get a reference to the file share we created previously.
CloudFileShare share = fileClient.GetShareReference("logs");

// Ensure that the share exists.
if (share.Exists())
{
    // Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.GetRootDirectoryReference();

    // Get a reference to the directory we created previously.
    CloudFileDirectory sampleDir = rootDir.GetDirectoryReference("CustomLogs");

    // Ensure that the directory exists.
    if (sampleDir.Exists())
    {
        // Get a reference to the file we created previously.
        CloudFile sourceFile = sampleDir.GetFileReference("Log1.txt");

        // Ensure that the source file exists.
        if (sourceFile.Exists())
        {
            // Get a reference to the destination file.
            CloudFile destFile = sampleDir.GetFileReference("Log1Copy.txt");

            // Start the copy operation.
            destFile.StartCopy(sourceFile);

            // Write the contents of the destination file to the console window.
            Console.WriteLine(destFile.DownloadText());
        }
    }
}
```

**Kopiowanie pliku do obiektu blob**

Poniższy przykładowy kod tworzy plik i kopiuje go do obiektu blob w ramach tego samego konta magazynu. Dla pliku źródłowego tworzona jest sygnatura dostępu współdzielonego, za pomocą której usługa uwierzytelnia dostęp do tego pliku podczas operacji kopiowania.

```csharp
// Parse the connection string for the storage account.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create a CloudFileClient object for credentialed access to File storage.
CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

// Create a new file share, if it does not already exist.
CloudFileShare share = fileClient.GetShareReference("sample-share");
share.CreateIfNotExists();

// Create a new file in the root directory.
CloudFile sourceFile = share.GetRootDirectoryReference().GetFileReference("sample-file.txt");
sourceFile.UploadText("A sample file in the root directory.");

// Get a reference to the blob to which the file will be copied.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
CloudBlobContainer container = blobClient.GetContainerReference("sample-container");
container.CreateIfNotExists();
CloudBlockBlob destBlob = container.GetBlockBlobReference("sample-blob.txt");

// Create a SAS for the file that's valid for 24 hours.
// Note that when you are copying a file to a blob, or a blob to a file, you must use a SAS
// to authenticate access to the source object, even if you are copying within the same
// storage account.
string fileSas = sourceFile.GetSharedAccessSignature(new SharedAccessFilePolicy()
{
    // Only read permissions are required for the source file.
    Permissions = SharedAccessFilePermissions.Read,
    SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24)
});

// Construct the URI to the source file, including the SAS token.
Uri fileSasUri = new Uri(sourceFile.StorageUri.PrimaryUri.ToString() + fileSas);

// Copy the file to the blob.
destBlob.StartCopy(fileSasUri);

// Write the contents of the file to the console window.
Console.WriteLine("Source file contents: {0}", sourceFile.DownloadText());
Console.WriteLine("Destination blob contents: {0}", destBlob.DownloadText());
```

W ten sam sposób można skopiować obiekt blob do pliku. Jeśli obiekt źródłowy jest obiektem blob, utwórz sygnaturę dostępu współdzielonego w celu uwierzytelniania dostępu do tego obiektu blob podczas operacji kopiowania.

## <a name="troubleshooting-file-storage-using-metrics"></a>Rozwiązywanie problemów z usługą Magazyn plików przy użyciu metryk
Funkcja analizy usługi Azure Storage obsługuje teraz metryki na potrzeby usługi File Storage. Dane metryk umożliwiają śledzenie żądań i diagnozowanie problemów.

Metryki dla usługi File Storage można włączyć w witrynie [Azure Portal](https://portal.azure.com). Można też włączyć je programowo przez wywołanie operacji ustawiania właściwości usługi plików za pomocą interfejsu API REST lub przy użyciu jednej z analogicznych operacji w bibliotece klienta usługi Storage.

Poniższy przykładowy kod pokazuje, jak włączyć metryki dla usługi File Storage za pomocą biblioteki klienta usługi Storage programu .NET.

Najpierw dodaj następujące instrukcje `using` do pliku program.cs (oprócz tych dodanych powyżej):

```csharp
using Microsoft.WindowsAzure.Storage.File.Protocol;
using Microsoft.WindowsAzure.Storage.Shared.Protocol;
```

Zwróć uwagę, że usługi Blob Storage, Table Storage i Queue Storage używają wspólnie typu `ServiceProperties` w przestrzeni nazw `Microsoft.WindowsAzure.Storage.Shared.Protocol`, natomiast usługa File Storage ma swój własny typ `FileServiceProperties` w przestrzeni nazw `Microsoft.WindowsAzure.Storage.File.Protocol`. Jednak aby można było skompilować poniższy kod, musi on zawierać odwołania do obu tych przestrzeni nazw.

```csharp
// Parse your storage connection string from your application's configuration file.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));
// Create the File service client.
CloudFileClient fileClient = storageAccount.CreateCloudFileClient();

// Set metrics properties for File service.
// Note that the File service currently uses its own service properties type,
// available in the Microsoft.WindowsAzure.Storage.File.Protocol namespace.
fileClient.SetServiceProperties(new FileServiceProperties()
{
    // Set hour metrics
    HourMetrics = new MetricsProperties()
    {
        MetricsLevel = MetricsLevel.ServiceAndApi,
        RetentionDays = 14,
        Version = "1.0"
    },
    // Set minute metrics
    MinuteMetrics = new MetricsProperties()
    {
        MetricsLevel = MetricsLevel.ServiceAndApi,
        RetentionDays = 7,
        Version = "1.0"
    }
});

// Read the metrics properties we just set.
FileServiceProperties serviceProperties = fileClient.GetServiceProperties();
Console.WriteLine("Hour metrics:");
Console.WriteLine(serviceProperties.HourMetrics.MetricsLevel);
Console.WriteLine(serviceProperties.HourMetrics.RetentionDays);
Console.WriteLine(serviceProperties.HourMetrics.Version);
Console.WriteLine();
Console.WriteLine("Minute metrics:");
Console.WriteLine(serviceProperties.MinuteMetrics.MetricsLevel);
Console.WriteLine(serviceProperties.MinuteMetrics.RetentionDays);
Console.WriteLine(serviceProperties.MinuteMetrics.Version);
```

Aby uzyskać kompleksowe wskazówki dotyczące rozwiązywania problemów, można zajrzeć do [artykułu na temat rozwiązywania problemów z usługą Azure Files](storage-troubleshoot-file-connection-problems.md). 

## <a name="file-storage-faq"></a>Często zadawane pytania dotyczące usługi Magazyn plików
1. **Czy usługa File Storage obsługuje uwierzytelnianie w usłudze Active Directory?**
   
    Obecnie nie zapewniamy obsługi uwierzytelniania w usłudze AD dla list kontroli dostępu, ale planujemy dodanie tej funkcji w przyszłości. Tymczasem do uwierzytelniania w udziale plików używane są klucze kont usługi Azure Storage. Ponadto dostępne jest obejście w formie sygnatur dostępu współdzielonego, których można używać za pośrednictwem interfejsu API REST lub bibliotek klienckich. Za pomocą tych sygnatur można wygenerować tokeny z określonymi uprawnieniami, które będą ważne przez ustalony czas. Można na przykład wygenerować token zapewniający dostęp tylko do odczytu do danego pliku. Każda osoba z tym tokenem (w trakcie jego okresu ważności) może uzyskiwać dostęp tylko do odczytu do tego pliku.
   
    Sygnatury dostępu współdzielonego są obsługiwane wyłącznie za pośrednictwem interfejsu API REST lub bibliotek klienckich. W przypadku zainstalowania udziału plików przy użyciu protokołu SMB nie można delegować dostępu do jego zawartości za pomocą sygnatur dostępu współdzielonego. 

2. **Jak zapewnić dostęp do konkretnego pliku przez przeglądarkę sieci Web?**
   Za pomocą tych sygnatur można wygenerować tokeny z określonymi uprawnieniami, które będą ważne przez ustalony czas. Na przykład można wygenerować token z dostępem tylko do odczytu do konkretnego pliku przez określony czas. Każdy posiadacz tego adresu URL będzie mógł przeprowadzić pobieranie bezpośrednio z dowolnej przeglądarki sieci Web (dopóki adres ten będzie ważny). Klucze sygnatur dostępu współdzielonego można łatwo generować z interfejsu użytkownika, na przykład Eksploratora magazynu.

3.   **Jakie są różne sposoby uzyskiwania dostępu do plików w usłudze Azure File Storage?**
    Udział plików można zainstalować na maszynie lokalnej, używając protokołu SMB 3.0, lub użyć narzędzi takich jak [Eksplorator magazynu](http://storageexplorer.com/) lub Cloudberry do uzyskania dostępu do plików w Twoim udziale plików. Z poziomu aplikacji dostęp do plików w udziale plików na platformie Azure możesz uzyskiwać, używając bibliotek klienckich, interfejsu API REST lub programu PowerShell.
    
4.   **Jak zainstalować udział plików platformy Azure na maszynie lokalnej?** Udział plików może zostać zainstalowany za pomocą protokołu SMB, dopóki port 445 (wychodzący ruch TCP) jest otwarty, a Twój klient obsługuje protokół SMB 3.0 (*np.* system Windows 8 lub Windows Server 2012). Poproś swojego usługodawcę internetowego o odblokowanie portu. W międzyczasie możesz wyświetlać swoje pliki przy użyciu programu Storage Explorer lub dowolnego programu innej firmy, np. Cloudberry.

5. **Czy ruch sieciowy między maszyną wirtualną na platformie Azure a udziałem plików jest liczony jako zewnętrzne użycie przepustowości i rozliczany w ten sposób w subskrypcji?**
   
    Jeśli udział plików i maszyna wirtualna znajdują się w różnych regionach, ruch między nimi będzie rozliczany jako zewnętrzne użycie przepustowości.
6. **Czy ruch sieciowy między maszyną wirtualną a udziałem plików w tym samym regionie jest bezpłatny?**
   
    Tak. Ruch sieciowy w tym samym regionie jest bezpłatny.
7. **Czy do nawiązania połączenia między lokalnymi maszynami wirtualnymi a usługą Azure File Storage jest wymagana usługa Azure ExpressRoute?**
   
    Nie. Nawet jeśli nie masz usługi ExpressRoute, możesz uzyskiwać dostęp do udziału plików z zasobów lokalnych, o ile masz otwarty port 445 (ruch wychodzący TCP) dla połączeń internetowych. Jeśli chcesz, możesz używać usługi ExpressRoute z usługą Magazyn plików.
8. **Czy usługa Azure File Storage może pełnić funkcję „monitora udziału plików” dla klastra trybu failover?**
   
    Obecnie takie rozwiązanie nie jest obsługiwane.
9. **Czy usługa File Storage jest obecnie replikowana tylko w modelu LRS lub GRS?**  
   
    Planujemy dodanie obsługi modelu RA-GRS, ale jeszcze nie wiemy, kiedy to nastąpi.
10. **Kiedy będzie można używać istniejących kont magazynu z usługą Azure File Storage?**
   
    Usługa Azure File Storage działa obecnie ze wszystkimi kontami magazynu.
11. **Czy do interfejsu API REST zostanie także dodana operacja zmiany nazwy?**
   
    Zmiana nazwy nie jest jeszcze obsługiwana w interfejsie API REST.
12. **Czy można zagnieżdżać udziały (czyli tworzyć udział w udziale)?**
    
    Nie. Udział plików to sterownik wirtualny z możliwością instalacji, dlatego zagnieżdżanie nie jest obsługiwane.
13. **Czy można określić uprawnienia tylko do odczytu lub tylko do zapisu w folderach w ramach udziału?**
    
    W przypadku udziałów plików instalowanych za pomocą protokołu SMB nie masz takiej kontroli nad uprawnieniami. Jednak możesz to zrobić, tworząc sygnaturę dostępu współdzielonego za pośrednictwem interfejsu API REST lub bibliotek klienckich.  
14. **Rozpakowywanie plików w usłudze File Storage działa wolno. Co zrobić?**
    
    Jeśli chcesz przetransferować dużą liczbę plików do usługi Magazyn plików, zalecamy skorzystanie z narzędzia AzCopy, programu Azure Powershell (Windows) lub interfejsu wiersza polecenia platformy Azure (Linux/Unix). Te narzędzia są zoptymalizowane pod kątem transferu sieciowego.
15. **Wydano poprawkę rozwiązującą problem powolnego działania usługi Pliki Azure **
    
    Zespół systemu Windows wydał ostatnio poprawkę rozwiązującą problem z wydajnością, gdy klienci próbują uzyskać dostęp do usługi Azure File Storage z systemu Windows 8.1 lub Windows Server 2012 R2. Aby uzyskać więcej informacji, zapoznaj się z odpowiednim artykułem KB: [Slow performance when you access Azure Files Storage from Windows 8.1 or Server 2012 R2](https://support.microsoft.com/en-us/kb/3114025) (Niska wydajność podczas dostępu do usługi Azure File Storage z systemu Windows 8.1 lub Windows Server 2012 R2).
16. **Korzystanie z usługi Azure File Storage z programem IBM MQ**
    
    Firma IBM wydała dokument pomagający posiadaczom oprogramowania IBM MQ w skonfigurowaniu usługi Azure File Storage. Aby uzyskać więcej informacji, zobacz: [How to setup IBM MQ Multi instance queue manager with Microsoft Azure File Service](https://github.com/ibm-messaging/mq-azure/wiki/How-to-setup-IBM-MQ-Multi-instance-queue-manager-with-Microsoft-Azure-File-Service) (Jak skonfigurować menedżera kolejki z wieloma wystąpieniami programu IBM MQ do działania z usługą Magazyn plików Azure).
17. **Jak usuwać błędy usługi Azure File Storage?**
    
    Aby uzyskać kompleksowe wskazówki dotyczące rozwiązywania problemów, można zajrzeć do [artykułu na temat rozwiązywania problemów z usługą Azure Files](storage-troubleshoot-file-connection-problems.md).               

18. **Jak mogę włączyć szyfrowanie po stronie serwera dla usługi Azure Files?**

    [Szyfrowanie po stronie serwera](https://docs.microsoft.com/en-us/azure/storage/storage-service-encryption) jest obecnie dostępne w wersji zapoznawczej. W okresie zapoznawczym tę funkcję można włączyć tylko dla nowo utworzonych kont magazynu usługi Azure Resource Manager (ARM).
    Tę funkcję można włączyć z poziomu konta magazynu usługi Azure Resource Manager przy użyciu witryny Azure Portal. Planujemy do końca lutego umożliwić włączanie szyfrowania dla magazynu plików za pomocą programu [Azure PowerShell](https://msdn.microsoft.com/en-us/library/azure/mt607151.aspx), [interfejsu wiersza polecenia platformy Azure](https://docs.microsoft.com/en-us/azure/storage/storage-azure-cli-nodejs) lub [interfejsu API dostawcy zasobów usługi Microsoft Azure Storage](https://docs.microsoft.com/en-us/rest/api/storagerp/storageaccounts). Włączenie tej funkcji nie wiąże się z żadną dodatkową opłatą. Po włączeniu szyfrowania usługi Storage dla usługi Azure File Storage dane są automatycznie szyfrowane. 
    Dowiedz się więcej o szyfrowaniu usługi Storage. Jeśli masz dodatkowe pytania dotyczące wersji zapoznawczej, wyślij wiadomość e-mail na adres ssediscussions@microsoft.com.

## <a name="next-steps"></a>Następne kroki
Poniższe linki umożliwiają uzyskanie dodatkowych informacji na temat usługi Magazyn plików Azure.

### <a name="conceptual-articles-and-videos"></a>Artykuły koncepcyjne i filmy
* [Azure Files Storage: a frictionless cloud SMB file system for Windows and Linux](https://azure.microsoft.com/documentation/videos/azurecon-2015-azure-files-storage-a-frictionless-cloud-smb-file-system-for-windows-and-linux/) (Azure File Storage: płynnie działający system plików SMB w chmurze dla systemów Windows i Linux)
* [How to use Azure File Storage with Linux](storage-how-to-use-files-linux.md) (Jak używać usługi Azure File Storage z systemem Linux)

### <a name="tooling-support-for-file-storage"></a>Narzędzia dostępne dla usługi Magazyn plików
* [Używanie programu Azure PowerShell z usługą Azure Storage](storage-powershell-guide-full.md)
* [How to use AzCopy with Microsoft Azure Storage](storage-use-azcopy.md) (Jak używać narzędzia AzCopy z usługą Microsoft Azure Storage)
* [Używanie interfejsu wiersza polecenia platformy Azure z usługą Azure Storage](storage-azure-cli.md#create-and-manage-file-shares)
* [Rozwiązywanie problemów z usługą Azure File Storage](https://docs.microsoft.com/en-us/azure/storage/storage-troubleshoot-file-connection-problems)

### <a name="reference"></a>Dokumentacja
* [Dokumentacja biblioteki klienta usługi Storage dla programu .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx)
* [Dokumentacja interfejsu API REST usługi plików](http://msdn.microsoft.com/library/azure/dn167006.aspx)

### <a name="blog-posts"></a>Wpisy na blogach
* [Azure File storage is now generally available](https://azure.microsoft.com/blog/azure-file-storage-now-generally-available/) (Usługa Azure File Storage została udostępniona publicznie)
* [Inside Azure File Storage](https://azure.microsoft.com/blog/inside-azure-file-storage/) (Za kulisami usługi Azure File Storage)
* [Introducing Microsoft Azure File Service](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx) (Wprowadzenie do usługi plików platformy Microsoft Azure)
* [Persisting connections to Microsoft Azure Files](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-connections-to-microsoft-azure-files.aspx) (Utrwalanie połączeń z plikami platformy Microsoft Azure)

