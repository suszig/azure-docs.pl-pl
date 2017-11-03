---
title: "Usługi Azure Data Lake Tools: Użycia usługi Azure Data Lake Tools dla programu Visual Studio Code | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak Azure Data Lake Tools dla Visual Studio Code umożliwia tworzenie, testowanie i uruchamianie skryptów U-SQL. "
Keywords: "VScode, usługi Azure Data Lake Tools, uruchamiania lokalnego debugowania, debugowania lokalnego pliku magazynu w wersji zapoznawczej, lokalnego przekazać do ścieżki do magazynu, pobierania, przekazywania"
services: data-lake-analytics
documentationcenter: 
author: jejiang
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: dc9b21d8-c5f4-4f77-bcbc-eff458f48de2
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/10/2017
ms.author: jejiang
ms.openlocfilehash: e724a8db4424a1e608ae7ee5625cd4cc16f6078f
ms.sourcegitcommit: 9ae92168678610f97ed466206063ec658261b195
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/17/2017
---
# <a name="use-azure-data-lake-tools-for-visual-studio-code"></a>Użyj usługi Azure Data Lake Tools dla programu Visual Studio Code

Omówienie narzędzi Azure Data Lake Tools dla programu Visual Studio (kod VS) do tworzenia, testowania i uruchamiać skrypty U-SQL. Informacje są także omówione w poniższym klipie wideo:

<a href="https://www.youtube.com/watch?v=J_gWuyFnaGA&feature=youtu.be"><img src="./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-video.png"></a>

## <a name="prerequisites"></a>Wymagania wstępne

Azure Data Lake Tools for VSCode obsługuje Windows, Linux lub MacOS.  

- [Visual Studio Code](https://www.visualstudio.com/products/code-vs.aspx).

System MacOS i Linux:
- [Oprogramowanie .NET core SDK 2.0](https://www.microsoft.com/net/download/core). 
- [Mono 5.2.x](http://www.mono-project.com/download/).

## <a name="install-data-lake-tools"></a>Instalowania narzędzi Data Lake Tools

Po zainstalowaniu wymagań wstępnych, można zainstalować narzędzi Data Lake Tools dla programu VS kodu.

**Aby zainstalować narzędzia Data Lake Tools**

1. Otwórz kod programu Visual Studio.
2. Kliknij przycisk **rozszerzenia** w okienku po lewej stronie. Wprowadź **usługi Azure Data Lake** w polu wyszukiwania.
3. Kliknij przycisk **zainstalować** obok **usługi Azure Data Lake tools**. Po kilku sekundach **zainstalować** przycisk zostanie zmieniony na **Załaduj ponownie**.
4. Kliknij przycisk **Załaduj ponownie** aktywować **usługi Azure Data Lake tools** rozszerzenia.
5. Kliknij przycisk **ponownego załadowania okna** o potwierdzenie. Widać **usługi Azure Data Lake tools** w okienku rozszerzenia.

    ![Narzędzia Data Lake Tools dla okienka rozszerzenia kodu programu Visual Studio](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-extensions.png)

 
## <a name="activate-azure-data-lake-tools"></a>Aktywacja usługi Azure Data Lake Tools
Utwórz nowy plik .usql lub Otwórz istniejący plik .usql aktywować rozszerzenia. 

## <a name="open-the-sample-script"></a>Otwórz przykładowy skrypt
Otwórz palety polecenia (Ctrl + Shift + P), a następnie wprowadź **ADL: Otwórz przykładowy skrypt**. Otwiera inne wystąpienie tego przykładu. Można również edytować, skonfigurować i przesłać skrypt w tym wystąpieniu.

## <a name="work-with-u-sql"></a>Praca z języka U-SQL

Należy otworzyć plik U-SQL lub folder do pracy w języku U-SQL.

**Aby otworzyć folder projektu U-SQL**

1. W programie Visual Studio Code, wybierz **pliku** menu, a następnie wybierz **Otwórz Folder**.
2. Określ folder, a następnie wybierz **wybierz Folder**.
3. Wybierz **pliku** menu, a następnie wybierz **nowy**. Plik 1 bez tytułu zostanie dodany do projektu.
4. Wprowadź następujący kod do pliku bez tytułu 1:

        @departments  = 
            SELECT * FROM 
                (VALUES
                    (31,    "Sales"),
                    (33,    "Engineering"), 
                    (34,    "Clerical"),
                    (35,    "Marketing")
                ) AS 
                      D( DepID, DepName );
         
        OUTPUT @departments
            TO "/Output/departments.csv"
        USING Outputters.Csv();

    Skrypt tworzy plik departments.csv z niektóre dane zawarte w folderze/Output.

5. Zapisz plik jako **myUSQL.usql** w otwartym folderze. Plik konfiguracji adltools_settings.json jest także dodawane do projektu.
4. Otwórz i skonfiguruj adltools_settings.json z następującymi właściwościami:

    - Konto: Konta Data Lake Analytics w ramach Twojej subskrypcji platformy Azure.
    - Baza danych: Baza danych przy użyciu tego konta. Wartość domyślna to **wzorca**.
    - Schemat: Schemat w bazie danych. Wartość domyślna to **dbo**.
    - Ustawienia opcjonalne:
        - Priorytet: Zakres priorytetu jest z zakresu od 1 do 1000 z 1 oznacza najwyższy priorytet. Wartość domyślna to **1000**.
        - Równoległość: Zakres równoległości jest z zakresu od 1 do 150. Wartość domyślna to równoległości maksymalny dozwolony na koncie usługi Azure Data Lake Analytics. 
        
        > [!NOTE] 
        > Jeśli te ustawienia są nieprawidłowe, zostaną użyte wartości domyślne.

        ![Narzędzia Data Lake Tools dla Visual Studio Code pliku konfiguracji](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-configuration-file.png)

        Obliczeniowe konta usługi Data Lake Analytics jest potrzebny do kompilowanie i uruchamianie zadań U-SQL. Należy skonfigurować konto komputera, przed możesz skompilować i uruchomić zadań U-SQL.
    
        Po zapisaniu konfiguracji informacje konta, bazy danych i schematu są wyświetlane na pasku stanu, w lewym dolnym rogu odpowiedni plik .usql. 
 
 
W porównaniu do otwierania pliku, po otwarciu folderu, które można wykonywać następujące czynności:

- Użyj pliku CodeBehind. W trybie pojedynczego pliku CodeBehind nie jest obsługiwane.
- Przy użyciu pliku konfiguracji. Po otwarciu folderu skryptów w folderze roboczym udostępniać pojedynczym pliku konfiguracji.


Skrypt U-SQL kompiluje zdalnie za pomocą usługi Data Lake Analytics. Podczas wystawiania **skompilować** polecenia skryptu U-SQL jest wysyłany do swojego konta usługi Data Lake Analytics. Później Visual Studio Code odbiera wynik kompilacji. Z powodu zdalnej kompilacji programu Visual Studio Code wymaga, wyświetlić listę informacji do łączenia się z kontem usługi Data Lake Analytics w pliku konfiguracji.

**Do kompilowania skryptu U-SQL**

1. Wybierz kombinację klawiszy Ctrl + Shift + P, aby otworzyć paletę polecenia. 
2. Wprowadź **ADL: kompilowania skryptu**. Zostaną wyświetlone wyniki kompilacji w **dane wyjściowe** okna. Można również kliknąć prawym przyciskiem myszy plik skryptu, a następnie wybierz **ADL: kompilowania skryptu** skompilować zadań U-SQL. Wynik kompilacji jest wyświetlana w **dane wyjściowe** okienka.
 

**Aby przesłać skrypt U-SQL**

1. Wybierz kombinację klawiszy Ctrl + Shift + P, aby otworzyć paletę polecenia. 
2. Wprowadź **ADL: przesłać zadania**.  Można również kliknąć prawym przyciskiem myszy plik skryptu, a następnie wybierz **ADL: Prześlij zadanie**. 

Po przesłaniu zadania skryptu U-SQL w dziennikach przesyłania są wyświetlane w **dane wyjściowe** okna w kodzie VS. Jeśli przesyłanie zakończy się pomyślnie, zadanie adres URL pojawia się również. Adres URL zadania można otworzyć w przeglądarce sieci web, aby śledzić stan zadania w czasie rzeczywistym.

Aby włączyć danych wyjściowych szczegóły zadania, ustaw **jobInformationOutputPath** w **vs kod u sql_settings.json** pliku.
 
## <a name="use-a-code-behind-file"></a>Przy użyciu pliku CodeBehind

Plik CodeBehind jest pliku C# skojarzonego z jednego skryptu U-SQL. Skrypt dedykowanych można zdefiniować UDO UDA, UDT i funkcji zdefiniowanej przez użytkownika w pliku CodeBehind. UDO, UDA, UDT i UDF można bezpośrednio w skrypcie bez rejestrowania najpierw zestaw. Plik CodeBehind jest umieszczany w tym samym folderze co jego komunikacji równorzędnej pliku skryptu U-SQL. Jeśli skrypt ma nazwę xxx.usql, jako xxx.usql.cs nosi nazwę CodeBehind. Jeśli ręcznie usuń plik CodeBehind, funkcja CodeBehind jest wyłączona dla jego skojarzony skrypt U-SQL. Aby uzyskać więcej informacji na temat pisania kodu klienta dla skryptu U-SQL, zobacz [pisania i przy użyciu niestandardowego kodu w języku U-SQL: funkcje zdefiniowane przez użytkownika]( https://blogs.msdn.microsoft.com/visualstudio/2015/10/28/writing-and-using-custom-code-in-u-sql-user-defined-functions/).

Aby obsługiwać związane z kodem, możesz otworzyć folderu roboczego. 

**Aby wygenerować plik CodeBehind**

1. Otwórz plik źródłowy. 
2. Wybierz kombinację klawiszy Ctrl + Shift + P, aby otworzyć paletę polecenia.
3. Wprowadź **ADL: generowanie kodu**. Plik CodeBehind jest tworzony w tym samym folderze. 

Można również kliknąć prawym przyciskiem myszy plik skryptu, a następnie wybierz **ADL: generowanie kodu za**. 

Aby skompilować i przesłać skrypt U-SQL przy użyciu pliku CodeBehind jest taka sama jak z autonomicznego pliku skryptu U-SQL.

Dwa poniższe zrzuty ekranu pokazują plik CodeBehind i jego skojarzony plik skryptu U-SQL:
 
![Narzędzia Data Lake Tools dla Visual Studio Code kodem](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-code-behind.png)

![Plik skryptu narzędzi Data Lake Tools dla Visual Studio Code kodem](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-code-behind-call.png) 

Obsługujemy uruchamiania lokalnego i debugowania lokalnego, zobacz instrukcje [uruchamiania lokalnego skryptu U-SQL i debugowania lokalnego z kodem Visual Studio](data-lake-tools-for-vscode-local-run-and-debug.md).

## <a name="use-assemblies"></a>Korzystanie z zestawów

Aby uzyskać informacje na temat tworzenia zespołów, zobacz [opracowanie U-SQL zestawy dla usługi Azure Data Lake Analytics zadań](data-lake-analytics-u-sql-develop-assemblies.md).

Data Lake Tools służy do rejestrowania zestawów niestandardowych kodów w katalogu Data Lake Analytics.

**Można zarejestrować zestawu**

Można zarejestrować zestawu za pomocą **ADL: Zarejestruj zestawu** lub **ADL: Zarejestruj zestawu za pomocą konfiguracji** poleceń.

**Aby zarejestrować za pomocą ADL: polecenie zarejestrować zestawu**
1.  Wybierz kombinację klawiszy Ctrl + Shift + P, aby otworzyć paletę polecenia.
2.  Wprowadź **ADL: zarejestrować zestawu**. 
3.  Określ ścieżkę zestawu lokalnego. 
4.  Wybierz konto usługi Data Lake Analytics.
5.  Wybierz bazę danych.

Wyniki: Portal jest otwarty w przeglądarce i przedstawia proces rejestracji zestawu.  

Inny wygodny sposób, aby wyzwolić **ADL: Zarejestruj zestawu** polecenie jest kliknij prawym przyciskiem myszy plik .dll w Eksploratorze plików. 

**Aby zarejestrować jednak ADL: Zarejestruj zestawu za pomocą polecenia konfiguracji**
1.  Wybierz kombinację klawiszy Ctrl + Shift + P, aby otworzyć paletę polecenia.
2.  Wprowadź **ADL: zarejestrować zestawu za pomocą konfiguracji**. 
3.  Określ ścieżkę zestawu lokalnego. 
4.  Plik JSON jest wyświetlany. Przejrzyj i Edytuj zależności zestawu i parametry zasobu, jeśli to konieczne. Instrukcje są wyświetlane w **dane wyjściowe** okna. Aby przejść do rejestrowania zestawów, Zapisz (Ctrl + S) w pliku JSON.

![Narzędzia Data Lake Tools dla Visual Studio Code kodem](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-register-assembly-advance.png)
>[!NOTE]
>- Zależności zestawu: automatycznie wykrywa Azure Data Lake Tools Określa, czy biblioteka DLL nie ma żadnych zależności. Zależności są wyświetlane w pliku JSON, po ich wykryciu. 
>- Zasoby: Można przekazać biblioteki DLL zasobów (na przykład txt, .png i .csv) jako część zestawu rejestracji. 

Innym sposobem wyzwolenia **ADL: Zarejestruj zestawu za pomocą konfiguracji** polecenie jest kliknij prawym przyciskiem myszy plik .dll w Eksploratorze plików. 

Poniższy kod języka U-SQL przedstawia sposób wywołania zestawu. W przykładzie nazwa zestawu jest *testu*.

```
REFERENCE ASSEMBLY [test];

@a = 
    EXTRACT 
        Iid int,
    Starts DateTime,
    Region string,
    Query string,
    DwellTime int,
    Results string,
    ClickedUrls string 
    FROM @"Sample/SearchLog.txt" 
    USING Extractors.Tsv();

@d =
    SELECT DISTINCT Region 
    FROM @a;

@d1 = 
    PROCESS @d
    PRODUCE 
        Region string,
    Mkt string
    USING new USQLApplication_codebehind.MyProcessor();

OUTPUT @d1 
    TO @"Sample/SearchLogtest.txt" 
    USING Outputters.Tsv();
```

## <a name="connect-to-azure"></a>Nawiązywanie połączenia z usługą Azure

Można było skompilować i uruchomić skrypty U-SQL w usługi Data Lake Analytics, należy połączyć z kontem platformy Azure.

**Do nawiązania połączenia platformy Azure**

1.  Wybierz kombinację klawiszy Ctrl + Shift + P, aby otworzyć paletę polecenia. 
2.  Wprowadź **ADL: logowania**. Informacje logowania są wyświetlane w **dane wyjściowe** okienka.

    ![Data Lake Tools dla Visual Studio Code polecenia palety](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-extension-login.png)
    ![narzędzi Data Lake Tools dla Visual Studio Code informacji o logowaniu urządzenia](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-login-info.png)
3. Wybierz kombinację klawiszy Ctrl + kliknięcie na adres URL logowania: https://aka.ms/devicelogin aby otworzyć stronę logowania w sieci Web. Wprowadź kod **G567LX42V** w polu tekstowym, a następnie wybierz **Kontynuuj**.

   ![Narzędzia Data Lake Tools dla Visual Studio Code logowania Wklej kod](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-extension-login-paste-code.png )   
4.  Postępuj zgodnie z instrukcjami, aby zalogować się w stronę sieci Web. Po nawiązaniu połączenia nazwa konta platformy Azure jest wyświetlana na pasku stanu, w lewym dolnym rogu **kodzie VS** okna. 

    > [!NOTE] 
    > Jeśli konto ma dwa składniki włączone, zalecane jest użycie z telefonu uwierzytelniania, a nie za pomocą numeru PIN.

Aby się wylogować, wprowadź polecenie **ADL: wylogowania**.

## <a name="list-your-data-lake-analytics-accounts"></a>Lista kont usługi Data Lake Analytics

Aby przetestować połączenie, wyświetlenie listy kont usługi Data Lake Analytics.

**Aby wyświetlić listę kont usługi Data Lake Analytics w ramach Twojej subskrypcji platformy Azure**

1. Wybierz kombinację klawiszy Ctrl + Shift + P, aby otworzyć paletę polecenia.
2. Wprowadź **ADL: listy kont**. Konta są wyświetlane w **dane wyjściowe** okienka.


## <a name="access-the-data-lake-analytics-catalog"></a>Dostęp do katalogu Data Lake Analytics

Po podłączeniu do platformy Azure, służy następujące kroki, aby uzyskać dostęp do katalogu U-SQL.

**Aby uzyskać dostępu do metadanych usługi Azure Data Lake Analytics**

1.  Wybierz kombinację klawiszy Ctrl + Shift + P, a następnie wprowadź **ADL: listy tabel**.
2.  Wybierz jedno z kont usługi Data Lake Analytics.
3.  Wybierz jedną z bazy danych usługi Data Lake Analytics.
4.  Wybierz jeden ze schematów. Można wyświetlić listę tabel.

## <a name="view-data-lake-analytics-jobs"></a>Widok Data Lake Analytics zadania

**Aby wyświetlić zadania usługi Data Lake Analytics**
1.  Otwórz palety polecenia (Ctrl + Shift + P) i wybierz **ADL: Pokaż zadania**. 
2.  Wybierz usługi Data Lake Analytics lub konta lokalnego. 
3.  Poczekaj na liście zadania konta są wyświetlane.
4.  Wybierz zadanie z listy zadań, narzędzi Data Lake Tools Otwiera szczegóły zadania w portalu Azure i plik informacji o zadaniu jest wyświetlany w kodzie VS.

    ![Typy obiektów narzędzi Data Lake Tools dla programu Visual Studio kodu IntelliSense](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-show-job.png)

## <a name="azure-data-lake-storage-integration"></a>Integracja magazynu usługi Azure Data Lake

Można użyć poleceń usługi Azure Data Lake Storage, aby:
 - Przeglądaj zasoby usługi Azure Data Lake Storage. [Lista ścieżki do magazynu](#list-the-storage-path). 
 - Wyświetl podgląd pliku usługi Azure Data Lake Storage. [Podgląd pliku magazynu](#preview-the-storage-file). 
 - Przekaż plik bezpośrednio do usługi Azure Data Lake Storage w kodzie VS. [Przekaż plik](#upload-file).
 - Pobierz plik bezpośrednio z usługi Azure Data Lake Storage w kodzie VS. [Pobierz plik](#download-file).

## <a name="list-the-storage-path"></a>Lista ścieżki do magazynu 

**Aby wyświetlić listę ścieżki magazynu za pomocą polecenia palety**

Kliknij prawym przyciskiem myszy edytora skryptów, a następnie wybierz **ADL: ścieżka magazynu listy**.

Wybierz folder z listy lub kliknij przycisk **wprowadź ścieżkę** lub **przejść z głównego** (używa wprowadź ścieżkę jako przykład). -> Wybierz Twoje **konta ADLA**. -> Navigate lub wprowadź ścieżkę do folderu magazynu (na przykład: / wyjście /). -> Informacje o ścieżce na podstawie wpisów list poleceń palety.

![Narzędzia Data Lake Tools dla Visual Studio Code lista wyników ścieżki magazynu](./media/data-lake-analytics-data-lake-tools-for-vscode/list-storage-path.png)

Bardziej wygodne ścieżka względna jest za pośrednictwem menu kontekstowym kliknij prawym przyciskiem myszy.

**Aby wyświetlić listę ścieżki magazynu za pośrednictwem kliknij prawym przyciskiem myszy**

Kliknij prawym przyciskiem myszy ciąg ścieżki, aby wybrać **ścieżki do magazynu listy** aby kontynuować.

![Narzędzia Data Lake Tools dla Visual Studio Code kliknij prawym przyciskiem myszy menu kontekstowe](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-right-click-path.png)


## <a name="preview-the-storage-file"></a>Podgląd pliku magazynu

Kliknij prawym przyciskiem myszy edytora skryptów, a następnie wybierz **ADL: Podgląd pliku magazynu**.

Wybierz użytkownika **konta ADLA**. -> Wprowadź ścieżkę pliku magazynu Azure (na przykład /output/SearchLog.txt). -> Wynik: plik zostanie otwarty w VSCode.

   ![Narzędzia Data Lake Tools dla Visual Studio Code Podgląd pliku wyników](./media/data-lake-analytics-data-lake-tools-for-vscode/preview-storage-file.png)

Innym sposobem Podgląd pliku magazynu jest za pomocą menu kliknij prawym przyciskiem myszy na pełną ścieżkę pliku lub ścieżki względnej pliku w Edytorze skryptów. 

## <a name="upload-file"></a>Przekazywanie pliku 

Możesz przekazać pliki, wprowadzając polecenia **ADL: Przekaż plik** lub **ADL: Przekaż plik przy użyciu konfiguracji**.

**Aby przekazać pliki jednak ADL: Przekaż plik za pomocą polecenia konfiguracji**
1.  Kliknij prawym przyciskiem myszy edytora skryptów, a następnie wybierz **Przekaż plik przy użyciu konfiguracji**.
2.  VS kod przedstawia plik JSON. Można wprowadzić ścieżki do plików i przekazywania wielu plików jednocześnie. Instrukcje są wyświetlane w **dane wyjściowe** okna. Aby kontynuować, można przekazać pliku, Zapisz (Ctrl + S) w pliku JSON.

       ![Ścieżka pliku narzędzia Data Lake Tools dla Visual Studio Code](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-upload-file.png)

3.  Wyniki: **dane wyjściowe** okno wyświetla stan przekazywania plików.

       ![Narzędzia Data Lake Tools dla Visual Studio Code przekazać stanu](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-upload-status.png)     

Jednocześnie, można monitorować [stan przekazywania](#check-storage-tasks-status).

**Aby przekazać pliki jednak ADL: Przekaż plik — polecenie**

Kliknij prawym przyciskiem myszy edytora skryptów, a następnie wybierz **Przekaż plik**.

Dane wejściowe użytkownika **ścieżkę do pliku lokalnego**. -> Wybierz folder na liście, lub kliknij przycisk **wprowadź ścieżkę** lub **przejść z głównego** (używa wprowadź ścieżkę jako przykład). -> Wybierz Twoje **konta ADLA**. -> Navigate lub wprowadź ścieżkę do folderu magazynu (na przykład: / wyjście /). -> kliknij **Wybierz bieżący Folder** do określenia folderu docelowego przekazywania.

![Narzędzia Data Lake Tools dla Visual Studio Code przekazać stanu](./media/data-lake-analytics-data-lake-tools-for-vscode/upload-file.png)    


Innym sposobem przekazywania plików do magazynu jest za pomocą menu kliknij prawym przyciskiem myszy na pełną ścieżkę pliku lub ścieżki względnej pliku w Edytorze skryptów.

Jednocześnie, można monitorować [stan przekazywania](#check-storage-tasks-status).

## <a name="download-file"></a>Pobierz plik 
Pliki można pobrać przez wprowadzenie polecenia **ADL: Pobierz plik magazynu** lub **ADL: Pobierz plik magazynu za pomocą konfiguracji**.

**Aby pobrać jednak plików ADL: Pobierz plik za pomocą polecenia konfiguracji**
1. Kliknij prawym przyciskiem myszy edytora skryptów, a następnie wybierz **Pobierz plik magazynu za pomocą konfiguracji**.
2. VS kod przedstawia plik JSON. Można wprowadzić ścieżki do pliku i pobrać wiele plików w tym samym czasie. Instrukcje są wyświetlane w **dane wyjściowe** okna. Aby kontynuować, aby pobrać plik, Zapisz (Ctrl + S) w pliku JSON.

    ![Narzędzia Data Lake Tools dla programu Visual Studio Code Pobierz pliki z konfiguracji](./media/data-lake-analytics-data-lake-tools-for-vscode/download-multi-files.png)

3.  Wyniki: **dane wyjściowe** okno wyświetla stan przekazywania plików.

    ![Data Lake Tools dla programu Visual Studio Code Pobierz wiele plików wyników](./media/data-lake-analytics-data-lake-tools-for-vscode/download-multi-file-result.png)     

Jednocześnie, można monitorować [pobieranie stanu](#check-storage-tasks-status).

**Jednak pobierania plików ADL: Pobierz plik magazynu — polecenie**

Kliknij prawym przyciskiem myszy edytora skryptów, a następnie wybierz **Pobierz plik magazynu**.

Wybierz folder z listy lub kliknij przycisk **wprowadź ścieżkę** lub **przejść z głównego** (używa wprowadź ścieżkę jako przykład). -> Wybierz Twoje **konta ADLA**. -> Navigate lub wprowadź ścieżkę do folderu magazynu (na przykład: / wyjście /) -> Wybierz plik do pobrania.

   ![Stan pobierania narzędzia Data Lake Tools dla Visual Studio Code](./media/data-lake-analytics-data-lake-tools-for-vscode/download-file.png) 

   W wyniku obraz plik jest zapisywany jako folderu tymczasowego. Możesz ustawić domyślny pobierania ścieżki dla parametru **usql.defaultLocalFolderForDownload** samodzielnie, za pomocą VSCode menu **pliku** -> **preferencje**  ->  **Ustawienie**.

Innym sposobem pobierania magazyn plików jest za pomocą menu kliknij prawym przyciskiem myszy na pełną ścieżkę pliku lub ścieżki względnej pliku w Edytorze skryptów.

Jednocześnie, można monitorować [pobieranie stanu](#check-storage-tasks-status).

## <a name="check-storage-tasks-status"></a>Sprawdź stan zadania magazynu
Stan jest wyświetlany w dolnej części paska stanu, po zakończeniu pobierania i przekazywania.
1. Kliknij przycisk poniżej paska stanu, a następnie wyświetlać pobieranie i stan przekazywania w **dane wyjściowe** panelu.

   ![Narzędzia Data Lake Tools dla programu Visual Studio Code Sprawdź magazynu stanu](./media/data-lake-analytics-data-lake-tools-for-vscode/storage-status.png)


## <a name="open-azure-storage-explorer"></a>Eksplorator usługi storage Azure Otwórz
Możesz otworzyć **Eksploratora usługi Storage Azure** przez wprowadzenie polecenia **ADL: Otwórz Eksploratora usługi sieci Web Azure Storage** lub wybierając z menu kontekstowego kliknij prawym przyciskiem myszy.

**Aby otworzyć Eksploratora usługi Storage platformy Azure**

1. Wybierz kombinację klawiszy Ctrl + Shift + P, aby otworzyć paletę polecenia.
2. Wprowadź **Otwórz Eksploratora usługi Storage Azure Web** lub kliknij prawym przyciskiem myszy na ścieżkę względną lub pełną ścieżkę w Edytorze skryptów, a następnie wybierz **Otwórz Eksploratora usługi Storage Azure Web**.
3. Wybierz konto usługi Data Lake Analytics.

Narzędzia Data Lake Tools otwiera ścieżki do magazynu Azure w portalu Azure. Można znaleźć ścieżki i wyświetlić podgląd pliku z sieci web.

## <a name="local-run-and-local-debug-for-windows-users"></a>Przebieg lokalny i lokalny debugowania dla systemu Windows użytkownicy
Uruchamiania lokalnego skryptu U-SQL sprawdza danych lokalnych i sprawdza poprawność skrypt lokalnie, przed opublikowaniem kodu do usługi Data Lake Analytics. Funkcja debugowania lokalnego umożliwia wykonywanie następujących zadań przed przesłaniem do usługi Data Lake Analytics kodu: 
- Debugowanie programu C# związane z kodem. 
- Kroków opisanych w kodzie. 
- Sprawdź poprawność skrypt lokalnie.

Aby uzyskać instrukcje dotyczące przebiegu lokalnego i debugowania lokalnego, zobacz [uruchamiania lokalnego skryptu U-SQL i debugowania lokalnego z kodem Visual Studio](data-lake-tools-for-vscode-local-run-and-debug.md).

## <a name="additional-features"></a>Dodatkowe funkcje

Narzędzia Data Lake Tools dla programu VS kodu obsługuje następujące funkcje:

-   Funkcja automatycznego uzupełniania IntelliSense: sugestie są wyświetlane w wyskakujące wokół elementów, takich jak słowa kluczowe, metod i zmiennych. Inne ikony reprezentują różnych typów obiektów:

    - Scala — typ danych
    - Typ danych złożonych
    - Wbudowanych typów
    - Klasy i kolekcji .NET
    - Wyrażenia języka C#
    - Funkcje UDF wbudowane C#, obiektów udo i UDAAGs 
    - Funkcje języka U-SQL
    - Funkcją okienkową U-SQL
 
    ![Typy obiektów narzędzi Data Lake Tools dla programu Visual Studio kodu IntelliSense](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-auto-complete-objects.png)
 
-   Funkcja automatycznego uzupełniania w metadanych usługi Data Lake Analytics IntelliSense: narzędzi Data Lake Tools pobierze lokalnie informacje metadanych usługi Data Lake Analytics. Funkcja IntelliSense automatycznie wypełni obiektów, łącznie z bazy danych, schematu, tabeli, widoku, funkcji zwracającej tabelę, procedury i zestawów języka C# z metadanych usługi Data Lake Analytics.
 
    ![Narzędzia Data Lake Tools dla programu Visual Studio kodu IntelliSense metadanych](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-auto-complete-metastore.png)

-   Znacznik błąd IntelliSense: narzędzi Data Lake Tools podkreśla błędy edytowania U-SQL i C#. 
-   Najważniejsze funkcje Składnia: narzędzi Data Lake Tools używa różne kolory do odróżnienia elementów, takich jak zmienne, słowa kluczowe, typ danych i funkcji. 

    ![Zawiera opis narzędzi Data Lake Tools dla Visual Studio Code składni](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-syntax-highlights.png)

## <a name="next-steps"></a>Następne kroki

- Dla przebiegu lokalnego skryptu U-SQL i debugowania lokalnego z kodem Visual Studio, zobacz [uruchamiania lokalnego skryptu U-SQL i debugowania lokalnego z kodem Visual Studio](data-lake-tools-for-vscode-local-run-and-debug.md).
- Aby — wprowadzenie informacji na temat usługi Data Lake Analytics, zobacz [samouczek: rozpoczynanie pracy z usługą Azure Data Lake Analytics](data-lake-analytics-get-started-portal.md).
- Aby informacji na temat narzędzia Data Lake Tools dla programu Visual Studio, zobacz [samouczek: skryptów U-SQL opracowanie przy użyciu narzędzi Data Lake Tools dla programu Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
- Aby uzyskać informacje na temat tworzenia zespołów, zobacz [opracowanie U-SQL zestawy dla usługi Azure Data Lake Analytics zadań](data-lake-analytics-u-sql-develop-assemblies.md).



