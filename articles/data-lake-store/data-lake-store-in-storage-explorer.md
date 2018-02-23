---
title: "Zarządzanie zasobami usługi Azure Data Lake Store w Eksploratorze usługi Azure Storage"
description: "Umożliwia użytkownikom dostęp do danych i zasobów usługi ADLS oraz zarządzanie nimi w Eksploratorze usługi Azure Storage"
Keywords: Azure Data Lake Store, Azure Storage Explorer
services: Data Lake Store
documentationcenter: 
author: jejiang
manager: DJ
editor: Jenny Jiang
ms.assetid: 
ms.service: Data Lake Store
ms.custom: Azure Data Lake Store
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 02/05/2018
ms.author: jejiang
ms.openlocfilehash: 4c69acebceac6719cf3f12ff0a93879f1b5a8943
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/09/2018
---
# <a name="manage-azure-data-lake-store-resources-with-storage-explorer-preview"></a>Zarządzanie zasobami usługi Azure Data Lake Store w Eksploratorze usługi Storage (wersja zapoznawcza)

[Azure Data Lake Store (ADLS)](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-overview) to usługa umożliwiająca przechowywanie dużych ilości danych bez struktury, na przykład danych tekstowych lub binarnych. Użytkownik może uzyskać dostęp do danych z dowolnego miejsca za pośrednictwem protokołu HTTP lub HTTPS. Usługa ADLS w Eksploratorze usługi Azure Storage umożliwia użytkownikom dostęp do danych i zasobów usługi ADLS oraz zarządzanie nimi wraz z innymi jednostkami platformy Azure, takimi jak obiekty blob i kolejki. Za pomocą jednego narzędzia użytkownicy mogą teraz centralnie zarządzać różnymi jednostkami platformy Azure.

Inną zaletą jest to, że użytkownicy nie muszą mieć uprawnień subskrypcji, aby zarządzać danymi usługi ADLS. W Eksploratorze usługi Storage użytkownicy mogą dołączać ścieżkę ADLS do węzła „Lokalny i dołączony”, jeśli inne osoby przyznają uprawnienia.

## <a name="prerequisites"></a>Wymagania wstępne
Do wykonania kroków opisanych w tym artykule potrzebne jest spełnienie następujących wymagań wstępnych:

*   Subskrypcja platformy Azure. Zobacz temat [Uzyskiwanie bezpłatnej wersji próbnej platformy Azure](https://azure.microsoft.com/pricing/free-trial).
*   Konto usługi Azure Data Lake Store. Aby uzyskać instrukcje dotyczące jego tworzenia, zobacz [Rozpoczynanie pracy z usługą Azure Data Lake Store](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-get-started-portal)

## <a name="installation"></a>Instalacja

Zainstaluj najnowszą wersję [Eksploratora usługi Azure Storage](https://azure.microsoft.com/features/storage-explorer/). Program jest dostępny w wersji dla systemu Windows i Linux oraz dla komputerów Mac.

## <a name="connect-to-an-azure-subscription"></a>Łączenie się z subskrypcją platformy Azure

1. Po zainstalowaniu **Eksploratora usługi Azure Storage** kliknij ikonę **wtyczki** widoczną po lewej stronie, jak pokazano na poniższej ilustracji.
       
   ![Ikona wtyczki](./media/data-lake-store-in-storage-explorer/plug-in-icon.png)
 
2. Wybierz pozycję **Dodaj konto platformy Azure**, a następnie kliknij pozycję **Zaloguj się**.

   ![Łączenie się z subskrypcją platformy Azure](./media/data-lake-store-in-storage-explorer/connect-to-azure-subscription.png)

2. W oknie dialogowym **Logowanie do platformy Azure** wybierz pozycję **Zaloguj**, a następnie wprowadź poświadczenia platformy Azure.

    ![Logowanie](./media/data-lake-store-in-storage-explorer/sign-in.png)

3. Wybierz subskrypcję z listy, a następnie kliknij przycisk **Zastosuj**.

    ![Zastosuj](./media/data-lake-store-in-storage-explorer/apply-subscription.png)

    Zawartość okienka Eksploratora zostanie zaktualizowana i pojawią się konta w wybranej subskrypcji.

    ![Lista kont](./media/data-lake-store-in-storage-explorer/account-list.png)

    Wykonanie powyższych czynności pozwala nawiązać połączenie z usługą **Azure Data Lake Store** i subskrypcją platformy Azure.

## <a name="connect-to-data-lake-store"></a>Nawiązywanie połączenia z usługą Data Lake Store
Jeśli chcesz uzyskać dostęp do zasobów, które nie istnieją w Twojej subskrypcji, ale inne osoby przyznają Ci dostęp do identyfikatora URI zasobów. W takim przypadku możesz nawiązać połączenie z usługą Data Lake Store, używając identyfikatora URI po zalogowaniu się. Przyjrzyj się poniższym krokom.
1. Otwórz Eksploratora usługi Storage (wersja zapoznawcza).
2. W okienku po lewej stronie rozwiń pozycję **Lokalny i dołączony**.
3. Kliknij prawym przyciskiem myszy pozycję **Data Lake Store** i z menu kontekstowego wybierz pozycję **Połącz z usługą Data Lake Store**.

      ![Menu kontekstowe Połącz z usługą Data Lake Store](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-uri-attach.png)

4. Wprowadź identyfikator URI. Narzędzie spowoduje przejście do wprowadzonej lokalizacji adresu URL.

      ![Kontekstowe okno dialogowe Łączenie z usługą Data Lake Store](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-uri-attach-dialog.png)

      ![Wynik działania polecenia Połącz z usługą Data Lake Store](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-attach-finish.png)

## <a name="view-an-azure-data-lake-store-accounts-contents"></a>Wyświetlanie zawartości konta usługi Azure Data Lake Store
Zasoby konta usługi Azure Data Lake Store zawierają foldery i pliki.

Poniższe kroki ilustrują wyświetlanie zawartości konta usługi ADLS w Eksploratorze usługi Storage (wersja zapoznawcza):

1. Otwórz Eksploratora usługi Storage (wersja zapoznawcza).
2. W okienku po lewej stronie rozwiń subskrypcję zawierającą konto usługi Azure Data Lake Store, które chcesz wyświetlić.
3. Rozwiń pozycję **Data Lake Store**.
4. Kliknij prawym przyciskiem myszy węzeł konta usługi Azure Data Lake Store, który chcesz wyświetlić, a następnie z menu kontekstowego wybierz pozycję **Otwórz**. Możesz również kliknąć dwukrotnie konto usługi ADLS, aby je otworzyć. 
5. W okienku głównym zostanie wyświetlona zawartość konta usługi ADLS.

     ![Okienko główne](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-toolbar-mainpane.png) 

## <a name="azure-data-lake-store-resources-management"></a>Zarządzanie zasobami usługi Azure Data Lake Store

Zasobami usługi Azure Data Lake Store można zarządzać, wykonując następujące operacje:
*   Nawigowanie po zasobach ADLS na wielu kontach usługi ADL.  
*   Nawiązywanie połączenia bezpośrednio z usługą ADLS i zarządzanie nią przy użyciu parametrów połączenia. 
*   Wyświetlanie zasobów ADLS udostępnionych przez inne osoby za pomocą listy kontroli dostępu w węźle Lokalny i dołączony.
*   Wykonywanie operacji CRUD na plikach/folderach: obsługa folderu cyklicznego i plików z wyborem wielokrotnym. 
*   Przeciąganie, upuszczanie i dodawanie folderu do paska Szybki dostęp oraz ostatnich lokalizacji, co odzwierciedla środowisko eksploratora plików pulpitu. 
*   Kopiowanie i otwieranie hiperlinku usługi ADL jednym kliknięciem za pomocą Eksploratora usługi Storage. 
*   Wyświetlanie dziennika aktywności w dolnym okienku po prawej stronie w celu wyświetlenia stanu działania.
*   Wyświetlanie statystyk folderów i właściwości plików.

## <a name="manage-resources-in-azure-storage-explorer"></a>Zarządzanie zasobami w Eksploratorze usługi Azure Storage
Po utworzeniu konta usługi Azure Data Lake Store można przekazywać foldery i pliki oraz pobierać i otwierać zasoby na komputerze lokalnym. Można również przypiąć je do paska Szybki dostęp, utworzyć nowy folder, skopiować adres URL i zaznaczyć wszystko. Ponadto można kopiować, wklejać, zmieniać nazwy, usuwać, tworzyć statystyki folderów i odświeżać.

Poniższe elementy pokazują, jak zarządzać zasobami w ramach konta usługi Azure Data Lake Store. Wykonaj następujące kroki w zależności od zadania, które chcesz wykonać:
   * **Przekazywanie plików**

     1. Na pasku narzędzi okienka głównego wybierz pozycję **Przekaż**, a następnie z menu rozwijanego wybierz pozycję **Przekaż pliki**.

        ![Menu Przekaż pliki](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-upload-files-menu.png) 

     2. W oknie dialogowym **Wybieranie plików do przekazania** zaznacz pliki, które chcesz przekazać.

        ![Okno dialogowe Przekazywanie folderu](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-upload-files-dialog.png)

     3. Wybierz pozycję **Otwórz**, aby rozpocząć przekazywanie.
   * **Przekazywanie folderu**

     1. Na pasku narzędzi okienka głównego wybierz pozycję **Przekaż**, a następnie z menu rozwijanego wybierz pozycję **Przekaż folder**.

        ![Menu Przekaż folder](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-upload-folder-menu.png) 
     2. W oknie dialogowym **Wybieranie folderu do przekazania** zaznacz folder, który chcesz przekazać.

        ![Okno dialogowe Przekazywanie folderu](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-upload-folder-dialog.png)      

     3. Wybierz pozycję **Wybierz folder**, aby rozpocząć przekazywanie folderów.

        ![Okno dialogowe Przekazywanie folderu](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-upload-folder-drag.png) 

      > [!NOTE] 
          > W celu zaimplementowania przekazywania możesz bezpośrednio przeciągać foldery i pliki na komputerze lokalnym. 
       
   * **Pobieranie folderów lub plików na komputer lokalny**

     1. Wybierz foldery lub pliki, które chcesz pobrać.
     2. Na pasku narzędzi okienka głównego wybierz pozycję **Pobierz**.
     3. W oknie dialogowym **Wybieranie folderu do zapisania pobranych plików** określ lokalizację, do której mają zostać pobrane foldery lub pliki. Następnie określ nazwę, którą chcesz jej nadać.
     4. Wybierz pozycję **Zapisz**.
   * **Otwieranie folderu lub pliku z komputera lokalnego**

     1. Wybierz folder lub plik, który chcesz otworzyć.
     2. Na pasku narzędzi okienka głównego wybierz pozycję **Otwórz** lub kliknij prawym przyciskiem myszy wybrany folder lub plik, a następnie z menu kontekstowego wybierz pozycję **Otwórz**.
     3. Plik zostanie pobrany i otwarty przy użyciu aplikacji skojarzonej z typem pliku źródłowego. Folder może także zostać otwarty w okienku głównym.

        ![otwieranie pliku](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-open.png) 

   * **Kopiowanie folderów lub plików do schowka**

     1. Wybierz foldery lub pliki, które chcesz skopiować.
     2. Na pasku narzędzi okienka głównego wybierz pozycję **Kopiuj** lub kliknij prawym przyciskiem myszy wybrane foldery lub pliki, a następnie z menu kontekstowego wybierz pozycję **Kopiuj**.
     3. W okienku po lewej stronie przejdź do innego konta usługi ADLS i kliknij je dwukrotnie, aby wyświetlić je w okienku głównym.
     4. Na pasku narzędzi okienka głównego wybierz pozycję **Wklej**, aby utworzyć kopię. Możesz również kliknąć polecenie **Wklej** w menu kontekstowym miejsca docelowego.

        ![Kopiowanie i wklejanie folderu lub pliku](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-copy-paste.png)

      > [!NOTE] 
          > + Kopiowanie i wklejanie między różnymi typami magazynu **nie jest** obsługiwane. Foldery i pliki ADLS można skopiować i wkleić na inne konto usługi ADLS. Jednak **nie można** kopiować folderów lub plików ADLS i wklejać ich do magazynu obiektów blob ani odwrotnie. 
          > + Działanie funkcji kopiowania i wklejania polega na pobraniu folderów lub plików na komputer lokalny, a następnie przekazaniu ich do miejsca docelowego. Narzędzie **nie** wykonuje akcji w zapleczu. Kopiowanie i wklejanie dużych plików trwa długo. Optymalizacja kopiowania/przenoszenia plików o wysokiej wydajności jest w toku.
   * **Usuwanie folderów lub plików**

     1. Wybierz foldery lub pliki, które chcesz usunąć.
     2. Na pasku narzędzi okienka głównego wybierz pozycję **Usuń** lub kliknij prawym przyciskiem myszy wybrane foldery lub pliki, a następnie z menu kontekstowego wybierz polecenie **Usuń**.
     3. W oknie dialogowym potwierdzenia wybierz pozycję **Tak**.

        ![Kopiowanie i wklejanie folderu lub pliku](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-delete.png)

   * **Przypinanie do paska Szybki dostęp**

     1. Wybierz folder, który chcesz przypiąć.
     2. Na pasku narzędzi okienka głównego wybierz pozycję **Przypnij do paska Szybki dostęp**.
     3. W okienku po lewej stronie zobaczysz, że wybrany folder został dodany do węzła **Szybki dostęp**.

        ![Przypnij do paska Szybki dostęp](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-quick-access.png)
     4. Po utworzeniu paska Szybki dostęp można łatwo uzyskiwać dostęp do zasobów.
   * **Linki bezpośrednie**
     1. Jeśli masz adres URL, możesz po prostu wprowadzić go do ścieżki adresu w **Eksploratorze plików** lub przeglądarce.
     2. Następnie automatycznie zostanie uruchomiony plik **Storage Explorer.exe** w celu przejścia do wprowadzonej lokalizacji adresu URL.

        ![Link bezpośredni w Eksploratorze plików](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-deep-link.png)


## <a name="next-steps"></a>Następne kroki
* Zobacz [najnowsze informacje o wersji i filmy wideo dotyczące programu Storage Explorer (wersja zapoznawcza)](http://www.storageexplorer.com).
* Dowiedz się, jak [zarządzać usługą Azure Cosmos DB w Eksploratorze usługi Azure Storage](https://docs.microsoft.com/en-us/azure/cosmos-db/storage-explorer)
* Dowiedz się więcej o Eksploratorze usługi Storage[Wprowadzenie do Eksploratora usługi Storage (wersja zapoznawcza)](https://docs.microsoft.com/en-us/azure/vs-azure-tools-storage-manage-with-storage-explorer)
* Wprowadzenie do usługi Azure Data Lake Store (ADLS)[Omówienie usługi Azure Data Lake Store](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-overview)
* Obejrzyj poniższy klip wideo, aby zobaczyć, [jak używać usługi Azure Cosmos DB w Eksploratorze usługi Azure Storage](https://www.youtube.com/watch?v=iNIbg1DLgWo&feature=youtu.be)
