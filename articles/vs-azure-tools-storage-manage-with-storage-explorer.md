---
title: Wprowadzenie do programu Storage Explorer (wersja zapoznawcza) | Microsoft Docs
description: "Zarządzanie zasobami usługi Azure Storage za pomocą programu Storage Explorer (wersja zapoznawcza)"
services: storage
documentationcenter: na
author: cawa
manager: paulyuk
editor: 
ms.assetid: 1ed0f096-494d-49c4-ab71-f4164ee19ec8
ms.service: storage
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/17/2017
ms.author: cawa
ms.openlocfilehash: 58ab8a9c5864ce0cb505b78fd087df2973a7e0d8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-with-storage-explorer-preview"></a>Wprowadzenie do programu Storage Explorer (wersja zapoznawcza)
## <a name="overview"></a>Omówienie
Microsoft Azure Storage Explorer (wersja zapoznawcza) jest aplikacją autonomiczną, która umożliwia łatwą pracę z danymi w usłudze Azure Storage w systemach Windows, macOS i Linux. Ten artykuł zawiera informacje dotyczące różnych sposobów łączenia się z kontami magazynu Azure i zarządzania nimi.

![Microsoft Azure Storage Explorer (wersja zapoznawcza)][15]

## <a name="prerequisites"></a>Wymagania wstępne
* [Pobieranie i instalowanie programu Storage Explorer (wersja zapoznawcza)](http://www.storageexplorer.com)

## <a name="connect-to-a-storage-account-or-service"></a>Łączenie się z usługą lub kontem magazynu
Program Storage Explorer (wersja zapoznawcza) oferuje kilka sposobów łączenia się z kontem magazynu. Można na przykład:
* Łączyć się z kontami magazynu skojarzonymi z subskrypcjami platformy Azure.
* Łączyć się z usługami i kontami magazynu udostępnianymi z innych subskrypcji platformy Azure.
* Łączyć się z magazynem lokalnym i zarządzać nim przy użyciu emulatora usługi Azure Storage. 

Ponadto można pracować z kontami magazynu na globalnej i krajowej platformie Azure:

* [Łączenie się z subskrypcją platformy Azure](#connect-to-an-azure-subscription): zarządzanie zasobami magazynu należącymi do subskrypcji platformy Azure.
* [Praca z lokalnym magazynem programistycznym](#work-with-local-development-storage): zarządzanie magazynem lokalnym przy użyciu emulatora usługi Azure Storage.
* [Dołączanie do magazynu zewnętrznego](#attach-or-detach-an-external-storage-account): zarządzanie zasobami magazynu należącymi do innej subskrypcji platformy Azure lub w innych chmurach krajowej platformy Azure przy użyciu nazwy, klucza i punktów końcowych konta magazynu.
* [Dołączanie konta magazynu przy użyciu sygnatury dostępu współdzielonego](#attach-storage-account-using-sas): zarządzanie zasobami magazynu należącymi do innej subskrypcji platformy Azure przy użyciu sygnatury dostępu współdzielonego.
* [Dołączanie usługi przy użyciu sygnatury dostępu współdzielonego](#attach-service-using-sas): zarządzanie określoną usługą magazynu (kontenerem obiektów blob, kolejką lub tabelą) należącą do innej subskrypcji platformy Azure przy użyciu sygnatury dostępu współdzielonego.
* [Połącz się z kontem bazy danych rozwiązania Cosmos Azure przy użyciu ciągu połączenia](#connect-to-an-azure-cosmos-db-account-by-using-a-connection-string): Zarządzanie DB rozwiązania Cosmos konta przy użyciu ciągu połączenia.

## <a name="connect-to-an-azure-subscription"></a>Łączenie się z subskrypcją platformy Azure
> [!NOTE]
> Jeśli nie masz konta platformy Azure, możesz [utworzyć konto bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) lub [aktywować korzyści dla subskrybentów programu Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).
>
>

1. W programie Storage Explorer (wersja zapoznawcza) wybierz pozycję **Ustawienia konta platformy Azure**.

    ![Ustawienia konta platformy Azure][0]

2. W lewym okienku są wyświetlane wszystkie konta Microsoft, na których się zalogowano. Aby połączyć się z innym kontem, wybierz pozycję **Dodaj konto** i postępuj zgodnie z instrukcjami w celu zalogowania się przy użyciu konta Microsoft skojarzonego z co najmniej jedną aktywną subskrypcją platformy Azure.

    >[!NOTE]
    >Łączenie z krajową wersją platformy Azure (taką jak niemiecka wersja platformy Azure, chińska wersja platformy Azure i platforma Azure dla instytucji rządowych za pośrednictwem logowania) nie jest obecnie obsługiwane. Zobacz sekcję „Dołączanie lub odłączanie konta magazynu zewnętrznego”, aby zapoznać się z opisem nawiązywania połączenia z kontami magazynu na krajowej platformie Azure.

3. Po pomyślnym zalogowaniu się przy użyciu konta Microsoft w okienku po lewej stronie zostaną wyświetlone wszystkie subskrypcje platformy Azure skojarzone z tym kontem. Wybierz subskrypcje platformy Azure, z którymi chcesz pracować, a następnie wybierz przycisk **Zastosuj**. (Zaznaczenie pola **Wszystkie subskrypcje** powoduje przełączenie między wyświetlaniem wszystkich lub żadnej z wymienionych subskrypcji platformy Azure).

    ![Wybieranie subskrypcji platformy Azure][3]  
    W okienku po lewej stronie są wyświetlane wszystkie konta magazynu skojarzone z wybranymi subskrypcjami platformy Azure.

    ![Wybrane subskrypcje platformy Azure][4]

## <a name="connect-to-an-azure-stack-subscription"></a>Łączenie z subskrypcją usługi Azure Stack

Aby uzyskać informacje dotyczące łączenia z subskrypcją usługi Azure Stack, zobacz [Connect Storage Explorer to an Azure Stack subscription (Łączenie programu Storage Explorer z subskrypcją usługi Azure Stack)](azure-stack/user/azure-stack-storage-connect-se.md).

## <a name="work-with-local-development-storage"></a>Praca z lokalnym magazynem projektowym
Program Storage Explorer (wersja zapoznawcza) pozwala pracować z magazynem lokalnym przy użyciu emulatora usługi Azure Storage. Takie podejście umożliwia pisanie kodu dla magazynu i testowanie go bez konieczności posiadania konta magazynu wdrożonego na platformie Azure, ponieważ konto magazynu jest emulowane przez emulator usługi Azure Storage.

> [!NOTE]
> Emulator usługi Azure Storage jest obecnie obsługiwany tylko dla systemu Windows.
>
>

1. W lewym okienku programu Storage Explorer (wersja zapoznawcza) rozwiń węzeł **(Lokalne i dołączone)** > **Konta usługi Storage** > **(Projektowanie)**.

    ![Węzeł projektowania lokalnego][21]

2. Jeśli emulator usługi Azure Storage nie został jeszcze zainstalowany, na pasku informacyjnym zostanie wyświetlony monit o jego zainstalowanie. Po wyświetleniu paska informacyjnego wybierz polecenie **Pobierz najnowszą wersję** i zainstaluj emulator.

    ![Monit o pobranie emulatora usługi Azure Storage][22]

3. Po zainstalowaniu emulatora będziesz mieć możliwość tworzenia lokalnych obiektów blob, kolejek i tabel oraz pracy z nimi. Aby dowiedzieć się, w jaki sposób pracować z poszczególnymi typami kont magazynu, zobacz jeden z poniższych artykułów:

    * [Zarządzanie zasobami usługi Azure Blob Storage](vs-azure-tools-storage-explorer-blobs.md)
    * Zarządzanie zasobami magazynu udziału plików platformy Azure: *dostępne wkrótce*
    * Zarządzanie zasobami usługi Azure Queue Storage: *dostępne wkrótce*
    * Zarządzanie zasobami usługi Azure Table Storage: *dostępne wkrótce*

## <a name="attach-or-detach-an-external-storage-account"></a>Dołączanie lub odłączanie konta magazynu zewnętrznego
Program Storage Explorer (wersja zapoznawcza) umożliwia dołączanie kont magazynu zewnętrznego, dzięki czemu można łatwo udostępniać konta magazynu. W tej sekcji opisano sposób dołączania (i odłączania) kont magazynu zewnętrznego.

### <a name="get-the-storage-account-credentials"></a>Uzyskiwanie poświadczeń konta magazynu
Aby udostępnić konto magazynu zewnętrznego, właściciel tego konta musi najpierw uzyskać poświadczenia (nazwę konta i klucz) dla konta, a następnie udostępnić te informacje osobie, która chce dołączyć do tego (zewnętrznego) konta. Poświadczenia konta magazynu można uzyskać za pośrednictwem witryny Azure Portal, wykonując następujące czynności:

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).

2. Wybierz pozycję **Przeglądaj**.

3. Wybierz pozycję **Konta usługi Storage**.

4. W bloku **Konta usługi Storage** wybierz odpowiednie konto magazynu.

5. W bloku **Ustawienia** dla wybranego konta magazynu wybierz pozycję **Klucze dostępu**.

    ![Opcja Klucze dostępu][5]

6. W bloku **Klucze dostępu** skopiuj wartość **Nazwa konta usługi Storage** i **klucz 1**, aby użyć ich podczas dołączania do konta magazynu.

    ![Klawisze dostępu][6]

### <a name="attach-to-an-external-storage-account"></a>Dołączanie konta magazynu zewnętrznego
Aby dołączyć konto magazynu zewnętrznego, potrzebna jest nazwa konta i klucz. W sekcji „Uzyskiwanie poświadczeń konta magazynu” wyjaśniono sposób uzyskiwania tych wartości z witryny Azure Portal. Należy pamiętać, że w witrynie Azure Portal klucz konta ma nazwę **klucz 1**. Dlatego w przypadku, gdy program Storage Explorer (wersja zapoznawcza) wymaga podania klucza konta, należy wprowadzić wartość **klucz 1**.

1. W programie Storage Explorer (wersja zapoznawcza) wybierz pozycję **Połącz z usługą Azure Storage**.

    ![Opcja Połącz z usługą Azure Storage][23]

2. W oknie dialogowym **Łączenie z usługą Azure Storage** określ klucz konta (wartość **klucz 1** z witryny Azure Portal), a następnie wybierz przycisk **Dalej**.

    > [!NOTE]
    > Możesz wprowadzić parametry połączenia z usługą Storage z poziomu konta magazynu na krajowej platformie Azure. Na przykład aby połączyć się z kontami magazynu niemieckiej wersji platformy Azure, należy wprowadzić parametry połączenia podobne do następujących: 
    >
    >* DefaultEndpointsProtocol=https
    >* AccountName=cawatest03
    >* AccountKey=<klucz_konta_magazynu>
    >* EndpointSuffix=core.cloudapi.de
    
    >Parametry połączenia można uzyskać z witryny Azure Portal w taki sam sposób, jak opisany w sekcji „Uzyskiwanie poświadczeń konta magazynu”.

    ![Okno dialogowe Łączenie z usługą Azure Storage][24]

3. W oknie dialogowym **Dołączanie zewnętrznej usługi Storage** w polu **Nazwa konta** wprowadź nazwę konta magazynu, określ inne żądane ustawienia, a następnie wybierz przycisk **Dalej**.

    ![Okno dialogowe Dołączanie zewnętrznej usługi Storage][8]

4. Sprawdź informacje wyświetlone w oknie dialogowym **Podsumowanie połączenia**. Aby wprowadzić zmiany, wybierz przycisk **Wstecz** i ponownie wprowadź odpowiednie ustawienia. 

5. Wybierz przycisk **Połącz**.

6. Po pomyślnym nawiązaniu połączenia konto magazynu zewnętrznego będzie wyświetlane z tekstem **(Zewnętrzne)** dodanym na końcu nazwy konta magazynu.

    ![Wynik połączenia z kontem magazynu zewnętrznego][9]

### <a name="detach-from-an-external-storage-account"></a>Odłączanie konta magazynu zewnętrznego
1. Kliknij prawym przyciskiem myszy konto magazynu zewnętrznego, które chcesz odłączyć, a następnie wybierz polecenie **Odłącz**.

    ![Opcja odłączenia magazynu][10]

2. W oknie komunikatu z potwierdzeniem wybierz przycisk **Tak**, aby potwierdzić odłączenie konta magazynu zewnętrznego.

## <a name="attach-a-storage-account-by-using-an-sas"></a>Dołączanie konta magazynu przy użyciu sygnatury dostępu współdzielonego
[Sygnatura dostępu współdzielonego](storage/common/storage-dotnet-shared-access-signature-part-1.md) zapewnia administratorowi subskrypcji platformy Azure możliwość tymczasowego przyznania dostępu do konta magazynu bez konieczności podawania poświadczeń subskrypcji platformy Azure.

Aby zilustrować ten scenariusz, załóżmy, że Użytkownik_A jest administratorem subskrypcji platformy Azure i Użytkownik_A chce zezwolić Użytkownikowi_B na dostęp do konta magazynu przez ograniczony czas z określonymi uprawnieniami:

1. Użytkownik_A generuje sygnaturę dostępu współdzielonego (składającą się z parametrów połączenia dla konta magazynu) dla określonego przedziału czasu i z odpowiednimi uprawnieniami.

2. Użytkownik_A udostępnia sygnaturę dostępu współdzielonego osobie, która chce uzyskać dostęp do konta magazynu — w naszym przykładzie jest to Użytkownik_B.  

3. Użytkownik_B korzysta z programu Storage Explorer (wersja zapoznawcza), aby dołączyć do konta należącego do Użytkownika_A przy użyciu dostarczonej sygnatury dostępu współdzielonego.

### <a name="get-an-sas-for-the-account-you-want-to-share"></a>Uzyskiwanie sygnatury dostępu współdzielonego dla konta, które chcesz udostępnić
1. W programie Storage Explorer (wersja zapoznawcza) kliknij prawym przyciskiem myszy konto magazynu, które chcesz udostępnić, a następnie wybierz polecenie **Uzyskaj sygnaturę dostępu współdzielonego**.

    ![Opcja menu kontekstowego Uzyskaj sygnaturę dostępu współdzielonego][13]

2. W oknie dialogowym **Sygnatura dostępu współdzielonego** określ przedział czasu i uprawnienia dla konta, a następnie wybierz przycisk **Utwórz**.

    ![Okno dialogowe uzyskiwania sygnatury dostępu współdzielonego][14]  
    Zostanie otwarte okno dialogowe **Sygnatura dostępu współdzielonego** z wyświetloną sygnaturą dostępu współdzielonego.

3. Obok pola **Parametry połączenia** wybierz przycisk **Kopiuj**, aby skopiować go do schowka, a następnie wybierz polecenie **Zamknij**.

### <a name="attach-to-the-shared-account-by-using-the-sas"></a>Dołączanie do udostępnionego konta przy użyciu sygnatury dostępu współdzielonego
1. W programie Storage Explorer (wersja zapoznawcza) wybierz pozycję **Połącz z usługą Azure Storage**.

    ![Opcja Połącz z usługą Azure Storage][23]

2. W oknie dialogowym **Łączenie z usługą Azure Storage** określ parametry połączenia, a następnie wybierz przycisk **Dalej**.

    ![Okno dialogowe Łączenie z usługą Azure Storage][24]

3. Sprawdź informacje wyświetlone w oknie dialogowym **Podsumowanie połączenia**. Aby wprowadzić zmiany, wybierz przycisk **Wstecz**, a następnie wprowadź odpowiednie ustawienia. 

4. Wybierz przycisk **Połącz**.

5. Po dołączeniu konto magazynu będzie wyświetlane z tekstem **(SAS)** dodanym na końcu podanej nazwy konta.

    ![Wynik dołączania do konta przy użyciu sygnatury dostępu współdzielonego][17]

## <a name="attach-a-service-by-using-an-sas"></a>Dołączanie usługi przy użyciu sygnatury dostępu współdzielonego
W sekcji „Dołączanie konta magazynu przy użyciu sygnatury dostępu współdzielonego” omówiono sposób udzielania tymczasowego dostępu do konta magazynu przez administratora subskrypcji platformy Azure przez wygenerowanie i udostępnienie sygnatury dostępu współdzielonego dla konta magazynu. Podobnie można wygenerować sygnaturę dostępu współdzielonego dla określonej usługi (kontenera obiektów blob, kolejki lub tabeli) w ramach konta magazynu.  

### <a name="generate-an-sas-for-the-service-that-you-want-to-share"></a>Generowanie sygnatury dostępu współdzielonego dla usługi, którą chcesz udostępnić
W tym kontekście usługa może być kontenerem, kolejką lub tabelą obiektów blob. Aby wygenerować sygnaturę dostępu współdzielonego dla wymienionych usług, zobacz:

* [Uzyskiwanie sygnatury dostępu współdzielonego dla kontenera obiektów blob](vs-azure-tools-storage-explorer-blobs.md#get-the-sas-for-a-blob-container)
* Uzyskiwanie sygnatury dostępu współdzielonego dla udziału plików: *dostępne wkrótce*
* Uzyskiwanie sygnatury dostępu współdzielonego dla kolejki: *dostępne wkrótce*
* Uzyskiwanie sygnatury dostępu współdzielonego dla tabeli: *dostępne wkrótce*

### <a name="attach-to-the-shared-account-service-by-using-the-sas"></a>Dołączanie do usługi udostępnionego konta przy użyciu sygnatury dostępu współdzielonego
1. W programie Storage Explorer (wersja zapoznawcza) wybierz pozycję **Połącz z usługą Azure Storage**.

    ![Opcja Połącz z usługą Azure Storage][23]

2. W oknie dialogowym **Łączenie z usługą Azure Storage** określ identyfikator URI sygnatury dostępu współdzielonego, a następnie wybierz przycisk **Dalej**.

    ![Okno dialogowe Łączenie z usługą Azure Storage][24]

3. Sprawdź informacje wyświetlone w oknie dialogowym **Podsumowanie połączenia**. Aby wprowadzić zmiany, wybierz przycisk **Wstecz**, a następnie wprowadź odpowiednie ustawienia. 

4. Wybierz przycisk **Połącz**.

5. Po dołączeniu nowo dołączona usługa jest wyświetlana w węźle **(Sygnatura dostępu współdzielonego usługi)**.

    ![Wynik dołączenia do usługi udostępnionej przy użyciu sygnatury dostępu współdzielonego][20]

## <a name="connect-to-an-azure-cosmos-db-account-by-using-a-connection-string"></a>Połącz się z kontem bazy danych rozwiązania Cosmos Azure przy użyciu parametrów połączenia
Oprócz zarządzania kontami bazy danych rozwiązania Cosmos Azure przy użyciu subskrypcji platformy Azure, alternatywny sposób łączenia z bazą danych Azure rozwiązania Cosmos jest Użyj parametrów połączenia. Nawiązywanie połączenia przy użyciu parametrów połączenia, wykonaj następujące kroki.

1. Znajdź **lokalnej i dołączonego** w drzewie po lewej stronie kliknij prawym przyciskiem myszy **kont DB rozwiązania Cosmos Azure**, wybierz **Połącz z bazy danych Azure rozwiązania Cosmos...**

    ![Łączenie do bazy danych Azure rozwiązania Cosmos przez ciąg połączenia][33]

2. Wybierz interfejsu API Azure rozwiązania Cosmos DB, Wklej Twojej **ciąg połączenia**, a następnie kliknij przycisk **OK** do łączenia z konta bazy danych Azure rozwiązania Cosmos. Aby uzyskać informacje na podczas pobierania ciągu połączenia, zobacz [pobrać ciągu połączenia](https://docs.microsoft.com/en-us/azure/cosmos-db/manage-account#get-the--connection-string).

    ![connection-string][32]

## <a name="search-for-storage-accounts"></a>Wyszukiwanie kont magazynu
Jeśli masz długą listę kont magazynu, możesz szybko odnaleźć określone konto magazynu przy użyciu pola wyszukiwania w górnej części okienka po lewej stronie.

Podczas pisania w polu wyszukiwania w okienku po lewej stronie są wyświetlane tylko konta magazynu pasujące do podanej wartości wyszukiwania. Na poniższym zrzucie ekranu przedstawiono przykład, gdzie wyszukano wszystkie konta magazynu, których nazwa zawiera tekst **tarcher**:

![Wyszukiwanie kont magazynu][11]

## <a name="next-steps"></a>Następne kroki
* [Zarządzanie zasobami usługi Azure Blob Storage przy użyciu programu Storage Explorer (wersja zapoznawcza)](vs-azure-tools-storage-explorer-blobs.md)
* [Zarządzanie Azure rozwiązania Cosmos bazy danych w Eksploratorze usługi Azure Storage (wersja zapoznawcza)](./cosmos-db/tutorial-documentdb-and-mongodb-in-storage-explorer.md)

[0]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/settings-icon.png
[1]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/add-account-link.png
[3]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/subscriptions-list.png
[4]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/storage-accounts-list.png
[5]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/access-keys.png
[6]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/access-keys-copy.png
[8]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-external-storage-dlg.png
[9]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/external-storage-account.png
[10]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/detach-external-storage.png
[11]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/storage-account-search.png
[12]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/detach-external-storage-confirmation.png
[13]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/get-sas-context-menu.png
[14]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/get-sas-dlg1.png
[15]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/mase.png
[17]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-account-using-sas-finished.png
[20]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-service-using-sas-finished.png
[21]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/local-storage-drop-down.png
[22]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/download-storage-emulator.png
[23]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connect-to-azure-storage-icon.png
[24]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connect-to-azure-storage-next.png
[25]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/add-certificate-azure-stack.png
[26]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/export-root-cert-azure-stack.png
[27]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/import-azure-stack-cert-storage-explorer.png
[28]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/select-target-azure-stack.png
[29]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/add-azure-stack-account.png
[30]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/select-accounts-azure-stack.png
[31]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/azure-stack-storage-account-list.png
[32]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connection-string.PNG
[33]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connect-to-db-by-connection-string.PNG
