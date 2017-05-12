---
title: "Rozpoczynanie pracy z usługą Data Lake Store przy użyciu witryny Azure Portal | Microsoft Docs"
description: "Użyj witryny Azure Portal, aby utworzyć konto usługi Data Lake Store i wykonywać podstawowe operacje w tej usłudze"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: fea324d0-ad1a-4150-81f0-8682ddb4591c
ms.service: data-lake-store
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/06/2017
ms.author: nitinme
ms.translationtype: Human Translation
ms.sourcegitcommit: 988e7fe2ae9f837b661b0c11cf30a90644085e16
ms.openlocfilehash: efb9e68256765873eb2776637d1a9469af6e46d6
ms.contentlocale: pl-pl
ms.lasthandoff: 04/06/2017


---
# <a name="get-started-with-azure-data-lake-store-using-the-azure-portal"></a>Rozpoczynanie pracy z usługą Azure Data Lake Store za pomocą witryny Azure Portal
> [!div class="op_single_selector"]
> * [Portal](data-lake-store-get-started-portal.md)
> * [PowerShell](data-lake-store-get-started-powershell.md)
> * [Zestaw SDK platformy .NET](data-lake-store-get-started-net-sdk.md)
> * [Zestaw SDK Java](data-lake-store-get-started-java-sdk.md)
> * [Interfejs API REST](data-lake-store-get-started-rest-api.md)
> * [Interfejs wiersza polecenia platformy Azure](data-lake-store-get-started-cli.md)
> * [Interfejs wiersza polecenia platformy Azure 2.0](data-lake-store-get-started-cli-2.0.md)
> * [Node.js](data-lake-store-manage-use-nodejs.md)
> * [Python](data-lake-store-get-started-python.md)
>
> 

Dowiedz się, jak użyć witryny Azure Portal, aby utworzyć konto usługi Azure Data Lake Store i wykonywać podstawowe operacje, takie jak tworzenie folderów, przekazywanie i pobieranie plików danych, usuwanie konta itp. Aby uzyskać więcej informacji o usłudze Data Lake Store, zobacz [Omówienie usługi Azure Data Lake Store](data-lake-store-overview.md).

## <a name="prerequisites"></a>Wymagania wstępne
Przed przystąpieniem do wykonania kroków opisanych w tym samouczku należy dysponować następującymi elementami:

* **Subskrypcja platformy Azure**. Zobacz temat [Uzyskiwanie bezpłatnej wersji próbnej platformy Azure](https://azure.microsoft.com/pricing/free-trial/).

## <a name="do-you-learn-faster-with-videos"></a>Czy uczysz się szybciej dzięki filmom?
Obejrzyj następujące filmy wideo, aby rozpocząć pracę z usługą Data Lake Store.

* [Tworzenie konta usługi Data Lake Store](https://mix.office.com/watch/1k1cycy4l4gen)
* [Manage data in Data Lake Store using the Data Explorer](https://mix.office.com/watch/icletrxrh6pc) (Zarządzanie danymi w usłudze Data Lake Store za pomocą eksploratora danych)

## <a name="create-an-azure-data-lake-store-account"></a>Tworzenie konta usługi Azure Data Lake Store
1. Zaloguj się w nowej witrynie [Azure Portal](https://portal.azure.com).
2. Kliknij pozycję **NOWY**, pozycję **Dane i magazyn**, a następnie pozycję **Azure Data Lake Store**. Przeczytaj informacje w bloku **Azure Data Lake Store**, a następnie kliknij pozycję **Utwórz** w lewym dolnym rogu bloku.
3. W bloku **Nowa usługa Data Lake Store** podaj wartości, jak pokazano na poniższym zrzucie ekranu:
   
    ![Tworzenie nowego konta usługi Azure Data Lake Store](./media/data-lake-store-get-started-portal/ADL.Create.New.Account.png "Tworzenie nowego konta usługi Azure Data Lake")
   
   * **Nazwa**. Wprowadź unikatową nazwę konta usługi Data Lake Store.
   * **Subskrypcja**. Wybierz subskrypcję, w ramach której chcesz utworzyć nowe konto usługi Data Lake Store.
   * **Grupa zasobów**. Wybierz istniejącą grupę zasobów lub użyj opcji **Utwórz nową**, aby utworzyć taką grupę. Grupa zasobów to kontener, który zawiera powiązane zasoby dla aplikacji. Aby uzyskać więcej informacji, zobacz [Grupy zasobów na platformie Azure](../azure-resource-manager/resource-group-overview.md#resource-groups).
   * **Lokalizacja**. Wybierz lokalizację, w której chcesz utworzyć konto usługi Data Lake Store.
   * **Ustawienia szyfrowania**. Możesz wybrać, czy chcesz szyfrować konto usługi Data Lake Store. Jeśli zdecydujesz się na szyfrowanie, możesz również określić sposób zarządzania głównym kluczem szyfrowania, którego chcesz używać do szyfrowania danych na swoim koncie.
     
     * (Opcjonalnie) Wybierz pozycję **Nie włączaj szyfrowania** z listy rozwijanej, aby zrezygnować z szyfrowania.
     * (Domyślnie) Wybierz pozycję **Użyj kluczy zarządzanych przez usługę Azure Data Lake**, jeśli chcesz zarządzać kluczami szyfrowania za pomocą usługi Azure Data Lake Store.
       
         ![Szyfrowanie usługi Data Lake Store](./media/data-lake-store-get-started-portal/adls-encryption-1.png "Szyfrowanie usługi Data Lake Store")
     * (Opcjonalnie) Wybierz pozycję **Wybierz klucze z usługi Azure Key Vault**, jeśli chcesz używać własnych kluczy znajdujących się w usłudze Azure Key Vault. Po wybraniu tej opcji możesz również utworzyć konto i klucze usługi Key Vault, jeśli ich jeszcze nie masz.
       
         ![Szyfrowanie usługi Data Lake Store](./media/data-lake-store-get-started-portal/adls-encryption-2.png "Szyfrowanie usługi Data Lake Store")
       
       Kliknij przycisk **OK** w bloku **Ustawienia szyfrowania**.
       
       > [!NOTE]
       > Jeśli używasz kluczy z usługi Azure Key Vault do skonfigurowania szyfrowania konta Data Lake Store, musisz przypisać do konta usługi Azure Data Lake uprawnienia dostępu do usługi Azure Key Vault. Informacje o tym, jak to zrobić, znajdziesz w sekcji [Przypisywanie uprawnień do usługi Azure Key Vault](#assign-permissions-to-the-azure-key-vault)
       > 
       > 
4. Kliknij przycisk **Utwórz**. Jeśli wybrano opcję przypięcia konta do pulpitu nawigacyjnego, nastąpi powrót do pulpitu nawigacyjnego i będzie widoczny postęp aprowizowania konta usługi Data Lake Store. Po aprowizowaniu konta usługi Data Lake Store pojawi się blok konta.

## <a name="assign-permissions-to-the-azure-key-vault"></a>Przypisywanie uprawnień do usługi Azure Key Vault
Jeśli używasz kluczy z usługi Azure Key Vault do skonfigurowania szyfrowania konta Data Lake Store, musisz skonfigurować dostęp między kontami usług Azure Data Lake a Azure Key Vault. W tym celu wykonaj poniższe kroki.

1. Jeśli użyto kluczy z usługi Azure Key Vault, na górze bloku konta usługi Data Lake Store zostanie wyświetlone ostrzeżenie. Kliknij je, aby otworzyć blok **Konfigurowanie uprawnień usługi Key Vault**.
   
    ![Szyfrowanie usługi Data Lake Store](./media/data-lake-store-get-started-portal/adls-encryption-3.png "Szyfrowanie usługi Data Lake Store")
2. Blok zawiera dwie opcje umożliwiające skonfigurowanie dostępu.
   
   * W pierwszej opcji kliknij przycisk **Udziel uprawnienia** w celu skonfigurowania dostępu. Pierwsza opcja jest włączona tylko wtedy, gdy użytkownik, który utworzył konto usługi Data Lake Store jest również administratorem usługi Azure Key Vault.
   * Druga opcja polega na uruchomieniu polecenia cmdlet programu PowerShell wyświetlonego w bloku. Musisz być właścicielem usługi Azure Key Vault lub mieć możliwość udzielania uprawnień w usłudze Azure Key Vault. Po uruchomieniu polecenia cmdlet wróć do bloku i kliknij przycisk **Włącz**, aby skonfigurować dostęp.

## <a name="createfolder"></a>Tworzenie folderów w ramach konta usługi Azure Data Lake Store
W ramach konta usługi Data Lake Store możesz tworzyć foldery w celu zarządzania danymi i przechowywania ich.

1. Otwórz właśnie utworzone konto usługi Data Lake Store. W okienku po lewej stronie kliknij pozycję **Przeglądaj**, kliknij pozycję **Data Lake Store**, a następnie w bloku Data Lake Store kliknij nazwę konta, w ramach którego chcesz utworzyć foldery. Jeśli konto jest przypięte do tablicy startowej, kliknij kafelek konta.
2. W bloku konta usługi Data Lake Store kliknij pozycję **Eksplorator danych**.
   
    ![Tworzenie folderów na koncie usługi Data Lake Store](./media/data-lake-store-get-started-portal/ADL.Create.Folder.png "Tworzenie folderów na koncie usługi Data Lake Store")
3. W bloku konta usługi Data Lake Store kliknij pozycję **Nowy folder**, wprowadź nazwę nowego folderu, a następnie kliknij przycisk **OK**.
   
    ![Tworzenie folderów na koncie usługi Data Lake Store](./media/data-lake-store-get-started-portal/ADL.Folder.Name.png "Tworzenie folderów na koncie usługi Data Lake Store")
   
    Nowo utworzony folder będzie widoczny na liście w bloku **Eksplorator danych**. Foldery możesz zagnieżdżać do dowolnego poziomu.
   
    ![Tworzenie folderów na koncie usługi Data Lake](./media/data-lake-store-get-started-portal/ADL.New.Directory.png "Tworzenie folderów na koncie usługi Data Lake")

## <a name="uploaddata"></a>Przekazywanie danych do konta usługi Azure Data Lake Store
Swoje dane możesz przekazać do konta usługi Azure Data Lake Store bezpośrednio na poziom główny albo do folderu utworzonego w ramach konta. Wykonaj kroki przekazywania pliku do podfolderu z bloku **Eksplorator danych** przedstawione na poniższym zrzucie ekranu. Na tym zrzucie ekranu plik jest przekazywany do podfolderu widocznego w obszarze nawigacji (zaznaczony czerwonym prostokątem).

Jeśli szukasz przykładowych danych do przekazania, możesz pobrać folder **Ambulance Data** z [repozytorium Git usługi Azure Data Lake](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData).

![Przekazywanie danych](./media/data-lake-store-get-started-portal/ADL.New.Upload.File.png "Przekazywanie danych")

## <a name="properties"></a>Właściwości i akcje dostępne dla przechowywanych danych
Kliknij nowo dodany plik, aby otworzyć blok **Właściwości**. W tym bloku dostępne są właściwości powiązane z plikiem i akcje, które można wykonać względem pliku. Możesz również skopiować pełną ścieżkę do pliku w ramach swojego konta usługi Azure Data Lake Store. Ścieżka jest zaznaczona czerwonym prostokątem na poniższym zrzucie ekranu.

![Właściwości danych](./media/data-lake-store-get-started-portal/ADL.File.Properties.png "Właściwości danych")

* Kliknij pozycję **Podgląd**, aby wyświetlić podgląd pliku bezpośrednio w przeglądarce. Możesz również określić format podglądu. Kliknij pozycję **Podgląd**, kliknij pozycję **Format** w bloku **Podgląd pliku**, a następnie w bloku **Format podglądu pliku** określ opcje, takie jak liczba wierszy do wyświetlenia, używane kodowanie, używany ogranicznik itp.
  
  ![Format podglądu pliku](./media/data-lake-store-get-started-portal/ADL.File.Preview.png "Format podglądu pliku")
* Kliknij pozycję **Pobierz**, aby pobrać plik na komputer.
* Aby zmienić nazwę pliku, kliknij pozycję **Zmień nazwę pliku**.
* Aby usunąć plik, kliknij pozycję **Usuń plik**.

## <a name="secure-your-data"></a>Zabezpieczanie danych
Dane przechowywane w ramach konta usługi Azure Data Lake Store możesz zabezpieczyć, korzystając z usługi Azure Active Directory i kontroli dostępu (listy ACL). Aby uzyskać instrukcje, jak to zrobić, zobacz [Zabezpieczanie danych w usłudze Azure Data Lake Store](data-lake-store-secure-data.md).

## <a name="delete-azure-data-lake-store-account"></a>Usuwanie konta usługi Azure Data Lake Store
Aby usunąć konto usługi Azure Data Lake Store z bloku Data Lake Store, kliknij pozycję **Usuń**. W celu potwierdzenia akcji zostanie wyświetlony monit o wprowadzenie nazwy konta, które chcesz usunąć. Wprowadź nazwę konta, a następnie kliknij pozycję **Usuń**.

![Usuwanie konta usługi Data Lake](./media/data-lake-store-get-started-portal/ADL.Delete.Account.png "Usuwanie konta usługi Data Lake")

## <a name="next-steps"></a>Następne kroki
* [Zabezpieczanie danych w usłudze Data Lake Store](data-lake-store-secure-data.md)
* [Korzystanie z usługi Azure Data Lake Analytics z usługą Data Lake Store](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Korzystanie z usługi Azure HDInsight z usługą Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)
* [Dostęp do dzienników diagnostycznych usługi Data Lake Store](data-lake-store-diagnostic-logs.md)


