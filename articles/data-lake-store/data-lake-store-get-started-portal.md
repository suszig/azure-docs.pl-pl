<properties 
   pageTitle="Rozpoczynanie pracy z usługą Data Lake Store | Azure" 
   description="Użyj portalu, aby utworzyć konto usługi Data Lake Store i wykonywać podstawowe operacje w usłudze Data Lake Store" 
   services="data-lake-store" 
   documentationCenter="" 
   authors="nitinme" 
   manager="paulettm" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="08/02/2016"
   ms.author="nitinme"/>

# Rozpoczynanie pracy z usługą Azure Data Lake Store za pomocą witryny Azure Portal

> [AZURE.SELECTOR]
- [Portal](data-lake-store-get-started-portal.md)
- [PowerShell](data-lake-store-get-started-powershell.md)
- [Zestaw SDK .NET](data-lake-store-get-started-net-sdk.md)
- [Zestaw SDK Java](data-lake-store-get-started-java-sdk.md)
- [Interfejs API REST](data-lake-store-get-started-rest-api.md)
- [Interfejs wiersza polecenia platformy Azure](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

Dowiedz się, jak użyć witryny Azure Portal, aby utworzyć konto usługi Azure Data Lake Store i wykonywać podstawowe operacje, takie jak tworzenie folderów, przekazywanie i pobieranie plików danych, usuwanie konta itp. Aby uzyskać więcej informacji o usłudze Data Lake Store, zobacz [Omówienie usługi Azure Data Lake Store](data-lake-store-overview.md).

## Wymagania wstępne

Przed przystąpieniem do wykonania kroków opisanych w tym samouczku należy dysponować następującymi elementami:

- **Subskrypcja platformy Azure**. Zobacz [Uzyskiwanie bezpłatnej wersji próbnej platformy Azure](https://azure.microsoft.com/pricing/free-trial/).

## <a name="signup"></a>Włączanie możliwości korzystania z publicznej wersji zapoznawczej usługi Data Lake Store w ramach subskrypcji platformy Azure

Najpierw należy zażądać włączenia możliwości korzystania z publicznej wersji zapoznawczej usługi Data Lake Store w ramach subskrypcji platformy Azure. Wykonaj poniższe kroki.

1. Zaloguj się w nowej witrynie [Azure Portal](https://portal.azure.com).
2. Kliknij pozycję **NOWY**, pozycję **Dane i magazyn**, a następnie pozycję **Azure Data Lake Store**.
3. W bloku **Nowa usługa Data Lake Store** kliknij pozycję **Zarejestruj się, aby skorzystać z wersji zapoznawczej**. Przeczytaj informacje, a następnie kliknij przycisk **OK**. Po włączeniu możliwości korzystania z publicznej wersji zapoznawczej w ramach subskrypcji otrzymasz wiadomość e-mail.

    ![Rejestracja w celu skorzystania z publicznej wersji zapoznawczej](./media/data-lake-store-get-started-portal/preview-signup.png "Create a new Azure Data Lake account")

## Czy dzięki filmom szybko się uczysz?

Obejrzyj następujące filmy wideo, aby rozpocząć pracę z usługą Data Lake Store.

* [Tworzenie konta usługi Data Lake Store](https://mix.office.com/watch/1k1cycy4l4gen)
* [Manage data in Data Lake Store using the Data Explorer (Zarządzanie danymi w usłudze Data Lake Store za pomocą eksploratora danych)](https://mix.office.com/watch/icletrxrh6pc)

## Tworzenie konta usługi Azure Data Lake Store

1. Zaloguj się w nowej witrynie [Azure Portal](https://portal.azure.com).

2. Kliknij pozycję **NOWY**, pozycję **Dane i magazyn**, a następnie pozycję **Azure Data Lake Store**. Przeczytaj informacje w bloku **Azure Data Lake Store**, a następnie kliknij pozycję **Utwórz** w lewym dolnym rogu bloku.

3. W bloku **Nowa usługa Data Lake Store** podaj wartości, jak pokazano na poniższym zrzucie ekranu:

    ![Tworzenie nowego konta usługi Azure Data Lake Store](./media/data-lake-store-get-started-portal/ADL.Create.New.Account.png "Create a new Azure Data Lake account")

    - **Subskrypcja**. Wybierz subskrypcję, w ramach której chcesz utworzyć nowe konto usługi Data Lake Store.
    - **Grupa zasobów**. Wybierz istniejącą grupę zasobów lub kliknij pozycję **Utwórz grupę zasobów**, aby utworzyć taką grupę. Grupa zasobów to kontener, który zawiera powiązane zasoby dla aplikacji. Aby uzyskać więcej informacji, zobacz [Grupy zasobów na platformie Azure](resource-group-overview.md#resource-groups).
    - **Lokalizacja**. Wybierz lokalizację, w której chcesz utworzyć konto usługi Data Lake Store.

4. Jeśli chcesz, aby konto usługi Data Lake Store było dostępne z tablicy startowej, wybierz pozycję **Przypnij do tablicy startowej**.

5. Kliknij przycisk **Utwórz**. Jeśli wybierzesz opcję przypięcia konta do tablicy startowej, nastąpi powrót do tablicy startowej i widoczny będzie postęp aprowizowania konta usługi Data Lake Store. Po aprowizowaniu konta usługi Data Lake Store pojawi się blok konta.

6. Rozwiń listę rozwijaną **Podstawowe elementy**, aby wyświetlić informacje o Twoim koncie usługi Data Lake Store, na przykład informacje o grupie zasobów, do której należy, lokalizacji itp. Kliknij ikonę **Szybki start**, aby zobaczyć linki do innych zasobów powiązanych z usługą Data Lake Store.

    ![Twoje konto usługi Azure Data Lake Store](./media/data-lake-store-get-started-portal/ADL.Account.QuickStart.png "Your Azure Data Lake account")

## <a name="createfolder"></a>Tworzenie folderów w ramach konta usługi Azure Data Lake Store

W ramach konta usługi Data Lake Store możesz tworzyć foldery w celu zarządzania danymi i przechowywania ich.

1. Otwórz właśnie utworzone konto usługi Data Lake Store. W okienku po lewej stronie kliknij pozycję **Przeglądaj**, kliknij pozycję **Data Lake Store**, a następnie w bloku Data Lake Store kliknij nazwę konta, w ramach którego chcesz utworzyć foldery. Jeśli konto jest przypięte do tablicy startowej, kliknij kafelek konta.

2. W bloku konta usługi Data Lake Store kliknij pozycję **Eksplorator danych**.

    ![Tworzenie folderów w ramach konta usługi Data Lake Store](./media/data-lake-store-get-started-portal/ADL.Create.Folder.png "Create folders in Data Lake Store account")

3. W bloku konta usługi Data Lake Store kliknij pozycję **Nowy folder**, wprowadź nazwę nowego folderu, a następnie kliknij przycisk **OK**.
    
    ![Tworzenie folderów w ramach konta usługi Data Lake Store](./media/data-lake-store-get-started-portal/ADL.Folder.Name.png "Create folders in Data Lake Store account")
    
    Nowo utworzony folder będzie widoczny na liście w bloku **Eksplorator danych**. Foldery możesz zagnieżdżać do dowolnego poziomu.

    ![Tworzenie folderów w ramach konta usługi Data Lake](./media/data-lake-store-get-started-portal/ADL.New.Directory.png "Create folders in Data Lake account")


## <a name="uploaddata"></a>Przekazywanie danych do konta usługi Azure Data Lake Store

Swoje dane możesz przekazać do konta usługi Azure Data Lake Store bezpośrednio na poziom główny albo do folderu utworzonego w ramach konta. Wykonaj kroki przekazywania pliku do podfolderu z bloku **Eksplorator danych** przedstawione na poniższym zrzucie ekranu. Na tym zrzucie ekranu plik jest przekazywany do podfolderu widocznego w obszarze nawigacji (zaznaczony czerwonym prostokątem).

Jeśli szukasz przykładowych danych do przekazania, możesz pobrać folder **Ambulance Data** z [repozytorium Git usługi Azure Data Lake](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData).

![Przekazywanie danych](./media/data-lake-store-get-started-portal/ADL.New.Upload.File.png "Upload data")


## <a name="properties"></a>Właściwości i akcje dostępne dla przechowywanych danych

Kliknij nowo dodany plik, aby otworzyć blok **Właściwości**. W tym bloku dostępne są właściwości powiązane z plikiem i akcje, które można wykonać względem pliku. Możesz również skopiować pełną ścieżkę do pliku w ramach swojego konta usługi Azure Data Lake Store. Ścieżka jest zaznaczona czerwonym prostokątem na poniższym zrzucie ekranu.

![Właściwości danych](./media/data-lake-store-get-started-portal/ADL.File.Properties.png "Properties on the data")

* Kliknij pozycję **Podgląd**, aby wyświetlić podgląd pliku bezpośrednio w przeglądarce. Możesz również określić format podglądu. Kliknij pozycję **Podgląd**, kliknij pozycję **Format** w bloku **Podgląd pliku**, a następnie w bloku **Format podglądu pliku** określ opcje, takie jak liczba wierszy do wyświetlenia, używane kodowanie, używany ogranicznik itp.

  ![Format podglądu pliku](./media/data-lake-store-get-started-portal/ADL.File.Preview.png "File preview format")

* Kliknij pozycję **Pobierz**, aby pobrać plik na komputer.

* Aby zmienić nazwę pliku, kliknij pozycję **Zmień nazwę pliku**.

* Aby usunąć plik, kliknij pozycję **Usuń plik**.


## Zabezpieczanie danych

Dane przechowywane w ramach konta usługi Azure Data Lake Store możesz zabezpieczyć, korzystając z usługi Azure Active Directory i kontroli dostępu (listy ACL). Aby uzyskać instrukcje, jak to zrobić, zobacz [Zabezpieczanie danych w usłudze Azure Data Lake Store](data-lake-store-secure-data.md).


## Usuwanie konta usługi Azure Data Lake Store

Aby usunąć konto usługi Azure Data Lake Store z bloku Data Lake Store, kliknij pozycję **Usuń**. W celu potwierdzenia akcji zostanie wyświetlony monit o wprowadzenie nazwy konta, które chcesz usunąć. Wprowadź nazwę konta, a następnie kliknij pozycję **Usuń**.

![Usuwanie konta usługi Data Lake](./media/data-lake-store-get-started-portal/ADL.Delete.Account.png "Delete Data Lake account")


## Następne kroki

- [Zabezpieczanie danych w usłudze Data Lake Store](data-lake-store-secure-data.md)
- [Korzystanie z usługi Azure Data Lake Analytics z usługą Data Lake Store](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Korzystanie z usługi Azure HDInsight z usługą Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)
- [Dostęp do dzienników diagnostycznych usługi Data Lake Store](data-lake-store-diagnostic-logs.md)



<!--HONumber=sep16_HO1-->


