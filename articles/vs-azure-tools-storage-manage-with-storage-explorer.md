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
ms.openlocfilehash: d2b93eec9d3ac575e771bceb0ac45823254c142d
ms.sourcegitcommit: e19742f674fcce0fd1b732e70679e444c7dfa729
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/01/2018
---
# <a name="get-started-with-storage-explorer-preview"></a>Wprowadzenie do programu Storage Explorer (wersja zapoznawcza)
## <a name="overview"></a>Przegląd
Microsoft Azure Storage Explorer (wersja zapoznawcza) jest aplikacją autonomiczną, która umożliwia łatwą pracę z danymi w usłudze Azure Storage w systemach Windows, macOS i Linux. W tym artykule dowiesz się kilka sposobów nawiązywania i zarządzanie kontami magazynu Azure.

![Microsoft Azure Storage Explorer (wersja zapoznawcza)][0]

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

1. W Eksploratorze usługi Storage (wersja zapoznawcza), wybierz **Zarządzanie kontami** można przejść do **konto zarządzania panelu**.

    ![Zarządzanie kontami][1]

2. W okienku po lewej stronie zostaną wyświetlone wszystkie konta Azure, który użytkownik został zarejestrowany w usłudze. Aby połączyć innego konta, wybierz **Dodaj konto**

3. Jeśli chcesz zalogować się do chmury national lub stos Azure, kliknij polecenie **środowiska platformy Azure** listy rozwijanej, aby wybrać, które ma być używany w chmurze Azure. Po wybraniu opcji środowiska, kliknij przycisk **Zaloguj...**  przycisku. Jeśli logujesz się do stosu Azure, zobacz [połączenia Eksploratora usługi Storage z subskrypcją platformy Azure stosu](azure-stack/user/azure-stack-storage-connect-se.md) Aby uzyskać więcej informacji.

    ![Zaloguj się w opcji][2]

3. Po pomyślnym zalogowaniu się przy użyciu konta platformy Azure, konta i subskrypcji platformy Azure skojarzonych z tym kontem zostaną dodane do lewego okienka. Wybierz subskrypcje platformy Azure, których chcesz użyć, a następnie wybierz **Zastosuj** (Wybieranie **wszystkie subskrypcje:** powoduje przełączenie między wyświetlaniem wszystkich lub żadna z wymienionych subskrypcji platformy Azure).

    ![Wybieranie subskrypcji platformy Azure][3]

    W okienku po lewej stronie są wyświetlane wszystkie konta magazynu skojarzone z wybranymi subskrypcjami platformy Azure.

    ![Wybrane subskrypcje platformy Azure][4]

## <a name="work-with-local-development-storage"></a>Praca z lokalnym magazynem projektowym
Program Storage Explorer (wersja zapoznawcza) pozwala pracować z magazynem lokalnym przy użyciu emulatora usługi Azure Storage. To rozwiązanie umożliwia symulować pracy z magazynem Azure, bez konieczności posiadania konta magazynu wdrożonego na platformie Azure, ponieważ konto magazynu jest emulowane przez Emulator usługi Azure Storage.

> [!NOTE]
> Emulator usługi Azure Storage jest obecnie obsługiwany tylko dla systemu Windows.
>
>

> [!NOTE]
> Emulatora magazynu Azure nie obsługuje udziałów plików.
>
>

1. W okienku po lewej stronie Eksploratora usługi Storage (wersja zapoznawcza), rozwiń **(lokalny i dołączonego)** > **kont magazynu** > **(Programowanie)**  >  **Kontenerów obiektów blob** węzła.

    ![Węzeł projektowania lokalnego][5]

2. Jeśli Emulator usługi Azure Storage nie został jeszcze zainstalowany, zostanie wyświetlony monit to zrobić za pomocą paska informacyjnego. Po wyświetleniu paska informacyjnego wybierz polecenie **Pobierz najnowszą wersję** i zainstaluj emulator.

    ![Monit o pobranie emulatora usługi Azure Storage][6]

3. Po zainstalowaniu emulatora będziesz mieć możliwość tworzenia lokalnych obiektów blob, kolejek i tabel oraz pracy z nimi. Aby dowiedzieć się, jak pracować z poszczególnymi typami kont, można znaleźć w następujących przewodnikach:

    * [Zarządzanie zasobami usługi Azure Blob Storage](vs-azure-tools-storage-explorer-blobs.md)
    * Zarządzanie zasobami magazynu udziału plików platformy Azure: *dostępne wkrótce*
    * Zarządzanie zasobami usługi Azure Queue Storage: *dostępne wkrótce*
    * Zarządzanie zasobami usługi Azure Table Storage: *dostępne wkrótce*

## <a name="attach-or-detach-an-external-storage-account"></a>Dołączanie lub odłączanie konta magazynu zewnętrznego
Program Storage Explorer (wersja zapoznawcza) umożliwia dołączanie kont magazynu zewnętrznego, dzięki czemu można łatwo udostępniać konta magazynu. W tej sekcji opisano sposób dołączania (i odłączania) kont magazynu zewnętrznego.

### <a name="get-the-storage-account-credentials"></a>Uzyskiwanie poświadczeń konta magazynu
Aby udostępnić konta magazynu zewnętrznego, właściciel tego konta musi najpierw uzyskać poświadczenia (nazwa konta i klucz) dla konta i udostępnić powiedzieć, że informacje z osobą, która chce dołączyć do konta. Można uzyskać poświadczeń konta magazynu za pośrednictwem portalu Azure, wykonując następujące czynności:

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).

2. Wybierz pozycję **Przeglądaj**.

3. Wybierz pozycję **Konta usługi Storage**.

4. Na liście **kont magazynu**, wybierz odpowiednie konto magazynu.

5. W obszarze **ustawienia**, wybierz pozycję **klucze dostępu**.

    ![Opcja Klucze dostępu][7]

6. Kopiuj **nazwy konta magazynu** i **klucz1**.

    ![Klawisze dostępu][8]

### <a name="attach-to-an-external-storage-account"></a>Dołączanie konta magazynu zewnętrznego
Aby dołączyć konto magazynu zewnętrznego, potrzebna jest nazwa konta i klucz. W sekcji „Uzyskiwanie poświadczeń konta magazynu” wyjaśniono sposób uzyskiwania tych wartości z witryny Azure Portal. Należy pamiętać, że w witrynie Azure Portal klucz konta ma nazwę **klucz 1**. Tak, gdy Eksploratora usługi Storage (wersja zapoznawcza) wprowadza się klucza konta, możesz wprowadzić **klucz1** wartości.

1. W Eksploratorze usługi Storage (wersja zapoznawcza), należy otworzyć **połączyć z okna dialogowego**.

    ![Opcja Połącz z usługą Azure Storage][9]

2. W **połączyć z okna dialogowego**, wybierz **użyć nazwy konta magazynu i klucza**

    ![Dodaj nazwę i opcji klucza][10]

3. Wklej nazwę konta w **nazwa konta** tekst pola i Wklej swój klucz konta usługi ( **klucz1** wartości z portalu Azure) do **klucz konta** pola tekstowego, a następnie wybierz **Dalej**.

    ![Nazwy i klucza][11]

    > [!NOTE]
    > Aby użyć nazwy i klucza z chmury national, użyj **domeny punkty końcowe magazynu:** listy rozwijanej, aby wybrać domenę odpowiednich punktów końcowych: 
    >
    >

4. Sprawdź informacje wyświetlone w oknie dialogowym **Podsumowanie połączenia**. Aby wprowadzić zmiany, wybierz przycisk **Wstecz** i ponownie wprowadź odpowiednie ustawienia. 

5. Wybierz przycisk **Połącz**.

6. Po pomyślnie dołączono konta magazynu, konta magazynu są wyświetlane z **(zewnętrzne)** dołączonym do jego nazwy.

    ![Wynik połączenia z kontem magazynu zewnętrznego][12]

### <a name="detach-from-an-external-storage-account"></a>Odłączanie konta magazynu zewnętrznego
1. Kliknij prawym przyciskiem myszy konto magazynu zewnętrznego, które chcesz odłączyć, a następnie wybierz polecenie **Odłącz**.

    ![Opcja odłączenia magazynu][13]

2. W oknie komunikatu z potwierdzeniem wybierz przycisk **Tak**, aby potwierdzić odłączenie konta magazynu zewnętrznego.

## <a name="attach-a-storage-account-by-using-a-shared-access-signature-sas"></a>Dołączanie konta magazynu przy użyciu udostępnionych podpis dostępu (SAS)
A sygnatura dostępu współdzielonego lub [SAS](storage/common/storage-dotnet-shared-access-signature-part-1.md), umożliwia administratorowi subskrypcji platformy Azure udzielania tymczasowego dostępu do konta magazynu bez konieczności podawania poświadczeń subskrypcji platformy Azure.

Aby zilustrować ten scenariusz, załóżmy, że Użytkownik_A jest administratorem subskrypcji platformy Azure i Użytkownik_A chce zezwolić Użytkownikowi_B na dostęp do konta magazynu przez ograniczony czas z określonymi uprawnieniami:

1. Użytkownik_a generuje ciąg połączenia SAS określony czas i z odpowiednimi uprawnieniami.

2. Użytkownik_a udostępnia sygnaturę dostępu Współdzielonego z osobą, która chce uzyskać dostęp do konta magazynu (w tym przykładzie Użytkownik_b).  

3. Użytkownik_B korzysta z programu Storage Explorer (wersja zapoznawcza), aby dołączyć do konta należącego do Użytkownika_A przy użyciu dostarczonej sygnatury dostępu współdzielonego.

### <a name="generate-a-sas-connection-string-for-the-account-you-want-to-share"></a>Generowanie sygnatury dostępu Współdzielonego parametry połączenia dla konta, które chcesz udostępnić
1. W Eksploratorze usługi Storage (wersja zapoznawcza), kliknij prawym przyciskiem myszy konto magazynu, które chcesz udostępnić, a następnie wybierz **Uzyskaj sygnaturę dostępu współdzielonego...** .

    ![Opcja menu kontekstowego Uzyskaj sygnaturę dostępu współdzielonego][14]

2. W **Generowanie sygnatura dostępu współdzielonego** oknie dialogowym Określ przedział czasu i uprawnienia dla konta, a następnie kliknij przycisk **Utwórz** przycisku.

    ![Okno dialogowe SAS][15]  

3. Obok pozycji **ciąg połączenia** pole tekstowe, wybierz opcję **kopiowania** skopiuj go do Schowka, a następnie kliknij przycisk **Zamknij**.

### <a name="attach-to-a-storage-account-by-using-a-sas-connection-string"></a>Aby dołączyć do konta magazynu przy użyciu parametrów połączenia SAS
1. W Eksploratorze usługi Storage (wersja zapoznawcza), należy otworzyć **połączyć z okna dialogowego**.

    ![Opcja Połącz z usługą Azure Storage][9]

2. W **połączyć z okna dialogowego** okno dialogowe, wybierz **za pomocą parametrów połączenia lub sygnatury dostępu współdzielonego URI** , a następnie kliknij przycisk **dalej**.

    ![Okno dialogowe Łączenie z usługą Azure Storage][16]

3. Wybierz **Użyj parametrów połączenia** i Wklej parametry połączenia do **ciąg połączenia:** pola. Kliknij przycisk **dalej** przycisku.

    ![Okno dialogowe Łączenie z usługą Azure Storage][17]

4. Sprawdź informacje wyświetlone w oknie dialogowym **Podsumowanie połączenia**. Aby wprowadzić zmiany, wybierz przycisk **Wstecz**, a następnie wprowadź odpowiednie ustawienia. 

5. Wybierz przycisk **Połącz**.

6. Po pomyślnie dołączono konta magazynu, konta magazynu są wyświetlane z **(SAS)** dołączonym do jego nazwy.

    ![Wynik dołączania do konta przy użyciu sygnatury dostępu współdzielonego][18]

## <a name="attach-a-service-by-using-a-shared-access-signature-sas"></a>Dołączanie usługi przy użyciu udostępnionych podpis dostępu (SAS)
W sekcji "Dołącz konto magazynu przy użyciu sygnatury dostępu Współdzielonego" objaśniono sposób udzielania tymczasowego dostępu do konta magazynu przez udostępnianie sygnatury dostępu Współdzielonego dla konta magazynu i generowania administratora subskrypcji platformy Azure. Podobnie można wygenerować sygnaturę dostępu Współdzielonego dla określonej usługi (kontenera obiektów blob, kolejki, tabeli lub udział plików) w ramach konta magazynu.  

### <a name="generate-an-sas-for-the-service-that-you-want-to-share"></a>Generowanie sygnatury dostępu współdzielonego dla usługi, którą chcesz udostępnić
W tym kontekście usługi mogą być kontenera obiektów blob, kolejek, tabel lub udziału plików. Aby wygenerować sygnaturę dostępu współdzielonego dla wymienionych usług, zobacz:

* [Uzyskiwanie sygnatury dostępu współdzielonego dla kontenera obiektów blob](vs-azure-tools-storage-explorer-blobs.md#get-the-sas-for-a-blob-container)
* Uzyskiwanie sygnatury dostępu współdzielonego dla udziału plików: *dostępne wkrótce*
* Uzyskiwanie sygnatury dostępu współdzielonego dla kolejki: *dostępne wkrótce*
* Uzyskiwanie sygnatury dostępu współdzielonego dla tabeli: *dostępne wkrótce*

### <a name="attach-to-the-shared-account-service-by-using-a-sas-uri"></a>Dołączanie do usługi udostępnionego konta przy użyciu identyfikatora URI sygnatury dostępu Współdzielonego
1. W Eksploratorze usługi Storage (wersja zapoznawcza), należy otworzyć **połączyć z okna dialogowego**.

    ![Opcja Połącz z usługą Azure Storage][9]

2. W **połączyć z okna dialogowego** okno dialogowe Wybierz **za pomocą parametrów połączenia lub sygnatury dostępu współdzielonego URI** , a następnie kliknij przycisk **dalej**.

    ![Okno dialogowe Łączenie z usługą Azure Storage][16]

3. Wybierz **korzystania z identyfikatora URI sygnatury dostępu Współdzielonego** i Wklej z identyfikatora URI do **identyfikatora URI:** pola. Kliknij przycisk **dalej** przycisku.

    ![Okno dialogowe Łączenie z usługą Azure Storage][19]

3. Sprawdź informacje wyświetlone w oknie dialogowym **Podsumowanie połączenia**. Aby wprowadzić zmiany, wybierz przycisk **Wstecz**, a następnie wprowadź odpowiednie ustawienia. 

4. Wybierz przycisk **Połącz**.

5. Po pomyślnym dołączeniu usługi usługa jest wyświetlana w obszarze **(usługi SAS-Attached)** węzła.

    ![Wynik dołączenia do usługi udostępnionej przy użyciu sygnatury dostępu współdzielonego][20]

## <a name="connect-to-an-azure-cosmos-db-account-by-using-a-connection-string"></a>Połącz się z kontem bazy danych rozwiązania Cosmos Azure przy użyciu parametrów połączenia
Oprócz zarządzania kontami bazy danych rozwiązania Cosmos Azure przy użyciu subskrypcji platformy Azure, alternatywny sposób łączenia z bazą danych Azure rozwiązania Cosmos jest Użyj parametrów połączenia. Aby nawiązać połączenie przy użyciu parametrów połączenia, wykonaj następujące kroki.

1. Znajdź pozycję **Lokalne i dołączone** w drzewie po lewej stronie, kliknij prawym przyciskiem myszy pozycję **Konta usługi Azure Cosmos DB** i wybierz pozycję **Połącz z usługą Azure Cosmos DB**.

    ![Łączenie do bazy danych Azure rozwiązania Cosmos przez ciąg połączenia][21]

2. Wybierz interfejsu API Azure rozwiązania Cosmos DB, Wklej Twojej **ciąg połączenia**, a następnie kliknij przycisk **OK** do łączenia z konta bazy danych Azure rozwiązania Cosmos. Aby uzyskać informacje dotyczące pobierania parametrów połączenia, zobacz [Get the connection string (Pobieranie parametrów połączenia)](https://docs.microsoft.com/azure/cosmos-db/manage-account#get-the--connection-string).

    ![connection-string][22]

## <a name="search-for-storage-accounts"></a>Wyszukiwanie kont magazynu
Jeśli chcesz znaleźć zasobu magazynu i nie wiadomo, gdzie jest, można użyć pola wyszukiwania w górnej części okienka po lewej stronie do wyszukiwania dla zasobu.

Podczas wpisywania tekstu w polu wyszukiwania, w lewym okienku wyświetlane są wszystkie zasoby, które pasują do wartości wyszukiwania, wprowadzone do tego punktu. Na przykład wyszukiwanie **punkty końcowe** przedstawiono na poniższym zrzucie ekranu:

![Wyszukiwanie kont magazynu][23]

> [!NOTE]
> Użyj **konto zarządzania panelu** zaznaczenie żadnych subskrypcji, które nie zawierają elementu szukasz aby skrócić czas wykonywania wyszukiwania. Można również kliknąć prawym przyciskiem myszy w węźle i wybierz **wyszukiwania z tutaj** się rozpocząć wyszukiwanie z określonego węzła.
>
>

## <a name="next-steps"></a>Kolejne kroki
* [Zarządzanie zasobami usługi Azure Blob Storage przy użyciu programu Storage Explorer (wersja zapoznawcza)](vs-azure-tools-storage-explorer-blobs.md)
* [Zarządzanie Azure rozwiązania Cosmos bazy danych w Eksploratorze usługi Azure Storage (wersja zapoznawcza)](./cosmos-db/storage-explorer.md)

[0]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/Overview.png
[1]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ManageAccounts.png
[2]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ConnectDialog-SignInSelected.png
[3]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/AccountPanel.png
[4]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/SubscriptionNode.png
[5]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/DevelopmentNode.png
[6]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/EmulatorNotInstalled.png
[7]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/PortalAccessKeys.png
[8]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/AccessKeys.png
[9]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ConnectDialog.png
[10]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ConnectDialog-AddWithKeySelected.png
[11]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ConnectDialog-NameAndKeyPage.png
[12]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/AttachedWithKeyAccount.png
[13]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/AttachedWithKeyAccount-Detach.png
[14]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/GetSharedAccessSignature.png
[15]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/SharedAccessSignatureDialog.png
[16]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ConnectDialog-WithConnStringOrSASSelected.png
[17]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ConnectDialog-ConnStringOrSASPage-1.png
[18]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/AttachedWithSASAccount.png
[19]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ConnectDialog-ConnStringOrSASPage-2.png
[20]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ServiceAttachedWithSAS.png
[21]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connect-to-db-by-connection-string.png
[22]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connection-string.png
[23]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/Search.png
