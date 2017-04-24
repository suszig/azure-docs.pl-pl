---
title: "Wprowadzenie do Eksploratora usługi Storage (wersja zapoznawcza) | Microsoft Docs"
description: "Zarządzanie zasobami magazynu platformy Azure za pomocą Eksploratora usługi Storage (wersja zapoznawcza)"
services: storage
documentationcenter: na
author: TomArcher
manager: douge
editor: 
ms.assetid: 1ed0f096-494d-49c4-ab71-f4164ee19ec8
ms.service: storage
ms.devlang: multiple
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/18/2016
ms.author: tarcher
translationtype: Human Translation
ms.sourcegitcommit: 0c4554d6289fb0050998765485d965d1fbc6ab3e
ms.openlocfilehash: 07b62cd6f6deb0cf3ff1c806204ebc26c773a164
ms.lasthandoff: 04/13/2017


---
# <a name="getting-started-with-storage-explorer-preview"></a>Wprowadzenie do Eksploratora usługi Storage (wersja zapoznawcza)
## <a name="overview"></a>Omówienie
Microsoft Azure Storage Explorer (wersja zapoznawcza) jest aplikacją autonomiczną, która umożliwia łatwą obsługę danych w usłudze Azure Storage w systemach Windows, macOS i Linux. Ten artykuł zawiera informacje dotyczące różnych sposobów łączenia się z kontami magazynu Azure i zarządzania nimi.

![Microsoft Azure Storage Explorer (wersja zapoznawcza)][15]

## <a name="prerequisites"></a>Wymagania wstępne
* [Pobieranie i instalowanie Eksploratora usługi Storage (wersja zapoznawcza)](http://www.storageexplorer.com)

## <a name="connect-to-a-storage-account-or-service"></a>Łączenie się z usługą lub kontem magazynu
Program Storage Explorer (wersja zapoznawcza) oferuje wiele sposobów łączenia się z kontami magazynu. Obejmuje to łączenie się z kontami magazynu skojarzonymi z subskrypcjami platformy Azure, łączenie się z usługami i kontami magazynu usługami udostępnianymi z innych subskrypcji platformy Azure, a nawet łączenie się z magazynem lokalnym i zarządzanie nim przy użyciu emulatora usługi Azure Storage. Ponadto można pracować z kontami magazynu na globalnej i krajowej platformie Azure:

* [Łączenie się z subskrypcją platformy Azure](#connect-to-an-azure-subscription) — zarządzanie zasobami należącymi do subskrypcji platformy Azure.
* [Praca z lokalnym magazynem projektowym](#work-with-local-development-storage) — zarządzanie magazynem lokalnym przy użyciu emulatora usługi Azure Storage.
* [Dołączanie do magazynu zewnętrznego](#attach-or-detach-an-external-storage-account) — zarządzanie zasobami magazynu należącymi do innej subskrypcji platformy Azure lub w innych chmurach krajowej platformy Azure przy użyciu nazwy, klucza i punktów końcowych konta magazynu.
* [Dołączanie konta magazynu przy użyciu sygnatury dostępu współdzielonego](#attach-storage-account-using-sas) — zarządzanie zasobami magazynu należącymi do innej subskrypcji platformy Azure przy użyciu sygnatury dostępu współdzielonego.
* [Dołączanie usługi przy użyciu sygnatury dostępu współdzielonego](#attach-service-using-sas) — zarządzanie określoną usługą Storage (kontenerem, kolejką lub tabelą obiektów blob) należącą do innej subskrypcji platformy Azure przy użyciu sygnatury dostępu współdzielonego.

## <a name="connect-to-an-azure-subscription"></a>Łączenie się z subskrypcją platformy Azure
> [!NOTE]
> Jeśli nie masz konta platformy Azure, możesz [utworzyć konto bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) lub [aktywować korzyści dla subskrybentów programu Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).
>
>

1. W programie Storage Explorer (wersja zapoznawcza) wybierz pozycję **Ustawienia konta platformy Azure**.

    ![Ustawienia konta platformy Azure][0]
2. W lewym okienku są wyświetlane wszystkie konta Microsoft, na których się zalogowano. Aby połączyć się z innym kontem, wybierz pozycję **Dodaj konto** i postępuj zgodnie z instrukcjami wyświetlanymi w oknach dialogowych, aby zalogować się przy użyciu konta Microsoft skojarzonego z co najmniej jedną aktywną subskrypcją platformy Azure.
> [!NOTE]
>Łączenie z krajową wersją platformy Azure, taką jak Black Forest Azure, Fairfax Azure i Mooncake Azure, za pośrednictwem logowania nie jest obecnie obsługiwane. Zobacz sekcję **Dołączanie lub odłączanie konta magazynu zewnętrznego**, aby zapoznać się z opisem nawiązywania połączenia z kontami magazynu na krajowej platformie Azure.

3. Po pomyślnym zalogowaniu się przy użyciu konta Microsoft w okienku po lewej stronie zostaną wyświetlone wszystkie subskrypcje platformy Azure skojarzone z tym kontem. Wybierz subskrypcję platformy Azure, przy użyciu której chcesz pracować, a następnie wybierz przycisk **Zastosuj**. (Zaznaczenie pola **Wszystkie subskrypcje** powoduje przełączenie między wyświetlaniem wszystkich lub żadnej z wymienionych subskrypcji platformy Azure).

    ![Wybieranie subskrypcji platformy Azure][3]
4. W okienku po lewej stronie są wyświetlane wszystkie konta magazynu skojarzone z wybranymi subskrypcjami platformy Azure.

    ![Wybrane subskrypcje platformy Azure][4]

## <a name="connect-to-an-azure-stack-subscription"></a>Łączenie z subskrypcją usługi Azure Stack

1. Eksplorator usługi Storage wymaga połączenia VPN do zdalnego uzyskiwania dostępu do subskrypcji usługi Azure Stack. Aby dowiedzieć się, jak skonfigurować połączenie VPN do usługi Azure Stack, zapoznaj się z tematem [Connect to Azure Stack with VPN](azure-stack/azure-stack-connect-azure-stack.md#connect-with-vpn) (Łączenie z usługą Azure Stack przy użyciu sieci VPN)

2. W celu przeprowadzenia weryfikacji koncepcji usługi Azure Stack musisz wyeksportować certyfikat główny urzędu usługi Azure Stack. Otwórz plik `mmc.exe` na maszynie CON01 MAS, czyli maszynie hosta usługi Azure Stack lub lokalnej maszynie z połączeniem VPN z usługą Azure Stack. W menu **Plik** wybierz pozycję **Dodaj/Usuń przystawkę** i dodaj pozycję **Certyfikaty**, aby zarządzać pozycją **Konto komputera** dla **komputera lokalnego**.

   ![Ładowanie certyfikatu głównego usługi Azure Stack za pośrednictwem pliku mmc.exe][25]   

   Znajdź element **AzureStackCertificationAuthority** w obszarze **Katalog główny konsoli\Certyfikowane (komputer lokalny)\Zaufane główne urzędy certyfikacji\Certyfikaty**. Kliknij prawym przyciskiem myszy ten element i wybierz pozycję **Wszystkie zadania -> Eksportuj**. Następnie postępuj zgodnie z instrukcjami w oknach dialogowych, aby wyeksportować **certyfikat X.509 szyfrowany algorytmem Base-64 (CER)**. Wyeksportowany certyfikat będzie używany w następnym kroku.   

   ![Eksportowanie certyfikatu głównego urzędu usługi Azure Stack][26]   

3. W Eksploratorze usługi Storage (wersja zapoznawcza) wybierz menu **Edytuj**, a następnie wybierz kolejno pozycje **Certyfikaty SSL** i **Importuj certyfikaty**. Użyj okna dialogowego selektora plików, aby wyszukać i otworzyć certyfikat eksplorowany w poprzednim kroku. Po zaimportowaniu zobaczysz monit o ponowne uruchomienie Eksploratora usługi Storage.

   ![Importowanie certyfikatu do Eksploratora usługi Storage (wersja zapoznawcza)][27]

4. Po ponownym uruchomieniu Eksploratora usługi Storage (wersja zapoznawcza) wybierz menu **Edytuj** i upewnij się, że pozycja **Oznacz usługę Azure Stack jako cel** została zaznaczona. W przeciwnym razie zaznacz ją i ponownie uruchom Eksploratora usługi Storage, aby zastosować zmiany. Ta konfiguracja jest wymagana w celu zachowania zgodności ze środowiskiem usługi Azure Stack.

   ![Sprawdzanie, czy pozycja Oznacz usługę Azure Stack jako cel została zaznaczona][28]

5. Na pasku po lewej stronie wybierz pozycję **Zarządzaj kontami**. W lewym okienku są wyświetlane wszystkie konta Microsoft, na których się zalogowano. Aby połączyć się z kontem usługi Azure Stack, wybierz pozycję **Dodaj konto**.

   ![Dodawanie konta usługi Azure Stack][29]

6. Wybierz pozycję **Utwórz środowisko niestandardowe** w obszarze **Środowisko platformy Azure** w oknie dialogowym **Dodawanie nowego konta**, a następnie kliknij przycisk **Dalej**.

7. Wprowadź wszystkie wymagane informacje dotyczące środowiska niestandardowego usługi Azure Stack, a następnie kliknij pozycję **Zaloguj się**.  Wypełnij pola okna dialogowego **Logowanie do środowiska niestandardowego chmury**, aby zalogować się za pomocą konta usługi Storage, które zostało skojarzone z co najmniej jedną aktywną subskrypcją usługi Azure Stack. Szczegółowe informacje dotyczące każdego pola w oknie dialogowym:

    * **Nazwa środowiska** — pole z możliwością dostosowywania przez użytkownika.
    * **Urząd** — wymagana wartość to https://login.windows.net. W przypadku chińskiej wersji platformy Azure China (Mooncake) użyj adresu https://login.chinacloudapi.cn.
    * **Identyfikator zasobu logowania** — wartość należy pobrać, wykonując następujące polecenia programu PowerShell:

    Jeśli jesteś administratorem chmury:

    ```powershell
    PowerShell (Invoke-RestMethod -Uri https://adminmanagement.local.azurestack.external/metadata/endpoints?api-version=1.0 -Method Get).authentication.audiences[0]
    ```

    Jeśli pracujesz w dzierżawie:

    ```powershell
    PowerShell (Invoke-RestMethod -Uri https://management.local.azurestack.external/metadata/endpoints?api-version=1.0 -Method Get).authentication.audiences[0]
    ```

    * **Punkt końcowy programu Graph** — wymagana wartość to https://graph.windows.net. W przypadku chińskiej wersji platformy Azure China (Mooncake) użyj adresu https://graph.chinacloudapi.cn.
    * **Identyfikator zasobu usługi ARM** — należy użyć wartości pola Identyfikator zasobu logowania.
    * **Punkt końcowy zasobu usługi ARM** — przykłady punktu końcowego zasobu usługi ARM:

    W przypadku administratora chmury: https://adminmanagement.local.azurestack.external   
    W przypadku dzierżawy: https://management.local.azurestack.external
 
    * **Identyfikatory dzierżawy** — pole opcjonalne. Zawiera ono wartość tylko wtedy, gdy należy określić katalog.

8. Po pomyślnym zalogowaniu się przy użyciu konta usługi Azure Stack w okienku po lewej stronie zostaną wyświetlone wszystkie subskrypcje usługi Azure Stack skojarzone z tym kontem. Wybierz subskrypcję usługi Azure Stack, przy użyciu której chcesz pracować, a następnie wybierz przycisk **Zastosuj**. (Zaznaczenie pola **Wszystkie subskrypcje** powoduje przełączenie między wyświetlaniem wszystkich lub żadnej z wymienionych subskrypcji usługi Azure Stack).

   ![Wybieranie subskrypcji usługi Azure Stack po wypełnieniu pól okna dialogowego Środowisko niestandardowe chmury][30]

9. W okienku po lewej stronie są wyświetlane wszystkie konta magazynu skojarzone z wybranymi subskrypcjami usługi Azure Stack.

   ![Lista kont magazynu, w tym kont subskrypcji usługi Azure Stack][31]

## <a name="work-with-local-development-storage"></a>Praca z lokalnym magazynem projektowym
Eksplorator usługi Storage (wersja zapoznawcza) umożliwia obsługę magazynu lokalnego przy użyciu emulatora usługi Azure Storage. Dzięki temu można pisać kod dla magazynu i testować go bez konieczności posiadania konta magazynu wdrożonego na platformie Azure, ponieważ konto magazynu jest emulowane przez emulator usługi Azure Storage.

> [!NOTE]
> Emulator usługi Azure Storage jest obecnie obsługiwany tylko dla systemu Windows.
>
>

1. W lewym okienku programu Storage Explorer (wersja zapoznawcza) rozwiń węzeł **(Lokalne i dołączone** > **Konta usługi Storage** > **(Projektowanie)**.

    ![Węzeł projektowania lokalnego][21]
2. Jeśli emulator usługi Azure Storage nie został jeszcze zainstalowany, na pasku informacyjnym zostanie wyświetlony monit o jego zainstalowanie. Po wyświetleniu paska informacyjnego wybierz polecenie **Pobierz najnowszą wersję** i zainstaluj emulator.

    ![Monit o pobranie emulatora usługi Azure Storage][22]
3. Po zainstalowaniu emulatora będziesz mieć możliwość tworzenia lokalnych obiektów blob, kolejek i tabel oraz pracy z nimi. Aby dowiedzieć się, w jaki sposób pracować z poszczególnymi typami kont magazynu, wybierz jeden z poniższych linków:

   * [Zarządzanie zasobami usługi Azure Blob Storage](vs-azure-tools-storage-explorer-blobs.md)
   * Zarządzanie zasobami magazynu udziału plików platformy Azure — *wkrótce*
   * Zarządzanie zasobami usługi Azure Queue Storage — *wkrótce*
   * Zarządzanie zasobami usługi Azure Table Storage — *wkrótce*

## <a name="attach-or-detach-an-external-storage-account"></a>Dołączanie lub odłączanie konta magazynu zewnętrznego
Eksplorator usługi Storage (wersja zapoznawcza) umożliwia dołączanie kont magazynu zewnętrznego, dzięki czemu można łatwo udostępniać konta magazynu. W tej sekcji opisano sposób dołączania (i odłączania) kont magazynu zewnętrznego.

### <a name="get-the-storage-account-credentials"></a>Uzyskiwanie poświadczeń konta magazynu
Aby udostępnić konto magazynu zewnętrznego, właściciel tego konta musi najpierw uzyskać poświadczenia — nazwę konta i klucz — dla konta, a następnie udostępnić te informacje osobie, która chce dołączyć do tego (zewnętrznego) konta. Poświadczenia konta magazynu można uzyskać za pośrednictwem Portalu Azure, wykonując następujące czynności:

1. Zaloguj się do [Portalu Azure](https://portal.azure.com).
2. Wybierz pozycję **Przeglądaj**.
3. Wybierz pozycję **Konta usługi Storage**.
4. W bloku **Konta usługi Storage** wybierz odpowiednie konto magazynu.
5. W bloku **Ustawienia** dla wybranego konta magazynu wybierz pozycję **Klucze dostępu**.

   ![Opcja Klucze dostępu][5]
6. W bloku **Klucze dostępu** skopiuj wartość **NAZWA KONTA USŁUGI STORAGE** i **KLUCZ 1**, aby użyć ich podczas dołączania konta magazynu.

   ![Klawisze dostępu][6]

### <a name="attach-to-an-external-storage-account"></a>Dołączanie konta magazynu zewnętrznego
Aby dołączyć konto magazynu zewnętrznego, potrzebna jest nazwa konta i klucz. W sekcji *Uzyskiwanie poświadczeń konta magazynu* wyjaśniono sposób uzyskiwania tych wartości z witryny Azure Portal. Pamiętaj jednak, że w portalu klucz konta ma nazwę „klucz 1”, zatem gdy program Storage Explorer (wersja zapoznawcza) poprosi o podanie klucza konta, wprowadź (lub wklej) wartość „klucz 1”.

1. W programie Storage Explorer (wersja zapoznawcza) wybierz pozycję **Połącz z usługą Azure Storage**.

   ![Opcja Połącz z usługą Azure Storage][23]
2. W oknie dialogowym **Łączenie z usługą Azure Storage** określ klucz konta (wartość „klucz 1” z witryny Azure Portal), a następnie wybierz przycisk **Dalej**.
> [!NOTE]
> Możesz wprowadzić parametry połączenia z usługą Storage z poziomu konta magazynu na krajowej platformie Azure. Na przykład wprowadź parametry połączenia podobne do następujących, aby połączyć się z kontami magazynu usługi Azure Black Forest: DefaultEndpointsProtocol=https;AccountName=cawatest03;AccountKey=<klucz_konta_magazynu>;EndpointSuffix=core.cloudapi.de;. Parametry połączenia możesz pobrać z witryny Azure Portal w sposób opisany w sekcji **Get the storage account credentials** (Uzyskiwanie poświadczeń konta magazynu)

   ![Okno dialogowe Łączenie z usługą Azure Storage][24]

3. W oknie dialogowym **Dołączanie zewnętrznej usługi Storage** wprowadź nazwę konta magazynu w polu **Nazwa konta**, określ inne odpowiednie ustawienia, a następnie wybierz przycisk **Dalej**.

   ![Okno dialogowe Dołączanie zewnętrznej usługi Storage][8]
4. Sprawdź informacje wyświetlone w oknie dialogowym **Podsumowanie połączenia**. Aby wprowadzić zmiany, wybierz przycisk **Wstecz** i ponownie wprowadź odpowiednie ustawienia. Po zakończeniu wybierz przycisk **Połącz**.
5. Po nawiązaniu połączenia konto magazynu zewnętrznego będzie wyświetlane z tekstem **(Zewnętrzne)** dodanym na końcu nazwy konta magazynu.

   ![Wynik połączenia z kontem magazynu zewnętrznego][9]

### <a name="detach-from-an-external-storage-account"></a>Odłączanie konta magazynu zewnętrznego
1. Kliknij prawym przyciskiem myszy konto magazynu zewnętrznego, które chcesz odłączyć, a następnie z menu kontekstowego wybierz polecenie **Odłącz**.

   ![Opcja odłączenia magazynu][10]
2. Po wyświetleniu okna komunikatu z potwierdzeniem wybierz przycisk **Tak**, aby potwierdzić odłączenie konta magazynu zewnętrznego.

## <a name="attach-storage-account-using-sas"></a>Dołączanie konta magazynu przy użyciu sygnatury dostępu współdzielonego
[Sygnatura dostępu współdzielonego (SAS, Shared Access Signature)](storage/storage-dotnet-shared-access-signature-part-1.md) zapewnia administratorowi subskrypcji platformy Azure możliwość tymczasowego przyznania dostępu do konta magazynu bez konieczności podawania poświadczeń subskrypcji platformy Azure.

Na przykład załóżmy, że Użytkownik_A jest administratorem subskrypcji platformy Azure i Użytkownik_A chce zezwolić Użytkownikowi_B na dostęp do konta magazynu przez ograniczony czas z określonymi uprawnieniami:

1. Użytkownik_A generuje sygnaturę dostępu współdzielonego (składającą się z parametrów połączenia dla konta magazynu) dla określonego przedziału czasu i z odpowiednimi uprawnieniami.
2. Użytkownik_A udostępnia sygnaturę dostępu współdzielonego osobie, która chce uzyskać dostęp do konta magazynu — w naszym przykładzie jest to Użytkownik_B.  
3. Użytkownik_B korzysta z Eksploratora usługi Storage (wersja zapoznawcza), aby dołączyć do konta należącego do Użytkownika_A przy użyciu dostarczonej sygnatury dostępu współdzielonego.

### <a name="get-a-sas-for-the-account-you-want-to-share"></a>Uzyskiwanie sygnatury dostępu współdzielonego dla konta, które chcesz udostępnić
1. W programie Storage Explorer (wersja zapoznawcza) kliknij prawym przyciskiem myszy konto magazynu, które chcesz udostępnić, i z menu kontekstowego wybierz polecenie **Uzyskaj sygnaturę dostępu współdzielonego**.

   ![Opcja menu kontekstowego Uzyskaj sygnaturę dostępu współdzielonego][13]
2. W oknie dialogowym **Sygnatura dostępu współdzielonego** określ przedział czasu i uprawnienia dla konta, a następnie wybierz przycisk **Utwórz**.

    ![Okno dialogowe uzyskiwania sygnatury dostępu współdzielonego][14]
3. W drugim oknie dialogowym **Sygnatura dostępu współdzielonego** wyświetlana jest sygnatura dostępu współdzielonego. Wybierz przycisk **Kopiuj** obok pola **Parametry połączenia**, aby skopiować go do schowka. Wybierz przycisk **Zamknij**, aby zamknąć okno dialogowe.

### <a name="attach-to-the-shared-account-using-the-sas"></a>Dołączanie do udostępnionego konta przy użyciu sygnatury dostępu współdzielonego
1. W programie Storage Explorer (wersja zapoznawcza) wybierz pozycję **Połącz z usługą Azure Storage**.

   ![Opcja Połącz z usługą Azure Storage][23]
2. W oknie dialogowym **Łączenie z usługą Azure Storage** określ parametry połączenia, a następnie wybierz przycisk **Dalej**.

   ![Okno dialogowe Łączenie z usługą Azure Storage][24]
3. Sprawdź informacje wyświetlone w oknie dialogowym **Podsumowanie połączenia**. Aby wprowadzić zmiany, wybierz przycisk **Wstecz** i ponownie wprowadź odpowiednie ustawienia. Po zakończeniu wybierz przycisk **Połącz**.
4. Po dołączeniu konto magazynu będzie wyświetlane z tekstem (SAS) dodanym na końcu podanej nazwy konta.

   ![Wynik dołączania konta przy użyciu sygnatury dostępu współdzielonego][17]

## <a name="attach-service-using-sas"></a>Dołączanie usługi przy użyciu sygnatury dostępu współdzielonego
W sekcji [Dołączanie konta magazynu przy użyciu sygnatury dostępu współdzielonego](#attach-storage-account-using-sas) omówiono sposób udzielania tymczasowego dostępu do konta magazynu przez administratora subskrypcji platformy Azure przez wygenerowanie (i udostępnienie) sygnatury dostępu współdzielonego dla konta magazynu. Podobnie można wygenerować sygnatury dostępu współdzielonego dla określonej usługi (kontenera, kolejki lub tabeli obiektów blob) w ramach konta magazynu.  

### <a name="generate-a-sas-for-the-service-you-want-to-share"></a>Generowanie sygnatury dostępu współdzielonego dla usługi, którą chcesz udostępnić
W tym kontekście usługa może być kontenerem, kolejką lub tabelą obiektów blob. W poniższych sekcjach wyjaśniono sposób generowania sygnatury dostępu współdzielonego dla wymienionych usług:

* [Uzyskiwanie sygnatury dostępu współdzielonego dla kontenera obiektów blob](vs-azure-tools-storage-explorer-blobs.md#get-the-sas-for-a-blob-container)
* Uzyskiwanie sygnatury dostępu współdzielonego dla udziału plików — *wkrótce*
* Uzyskiwanie sygnatury dostępu współdzielonego dla kolejki — *wkrótce*
* Uzyskiwanie sygnatury dostępu współdzielonego dla tabeli — *wkrótce*

### <a name="attach-to-the-shared-account-service-using-the-sas"></a>Dołączanie do usługi udostępnionego konta przy użyciu sygnatury dostępu współdzielonego
1. W programie Storage Explorer (wersja zapoznawcza) wybierz pozycję **Połącz z usługą Azure Storage**.

   ![Opcja Połącz z usługą Azure Storage][23]
2. W oknie dialogowym **Łączenie z usługą Azure Storage** określ identyfikator URI sygnatury dostępu współdzielonego, a następnie wybierz przycisk **Dalej**.

   ![Okno dialogowe Łączenie z usługą Azure Storage][24]
3. Sprawdź informacje wyświetlone w oknie dialogowym **Podsumowanie połączenia**. Aby wprowadzić zmiany, wybierz przycisk **Wstecz** i ponownie wprowadź odpowiednie ustawienia. Po zakończeniu wybierz przycisk **Połącz**.
4. Po dołączeniu nowo dołączona usługa będzie wyświetlana w węźle **(Sygnatura dostępu współdzielonego usługi)**.

   ![Wynik dołączenia do usługi udostępnionej przy użyciu sygnatury dostępu współdzielonego][20]

## <a name="search-for-storage-accounts"></a>Wyszukiwanie kont magazynu
Jeśli masz długą listę kont magazynu, możesz szybko odnaleźć określone konto magazynu przy użyciu pola wyszukiwania w górnej części okienka po lewej stronie.

Podczas wpisywania w polu wyszukiwania w okienku po lewej stronie są wyświetlane tylko konta magazynu pasujące do podanej wartości wyszukiwania. Na poniższym ekranie przedstawiono przykład, gdzie wyszukano wszystkie konta magazynu, których nazwa zawiera tekst „tarcher”.

![Wyszukiwanie kont magazynu][11]

Aby wyczyścić wyszukiwanie, wybierz przycisk **x** w polu wyszukiwania.

## <a name="next-steps"></a>Następne kroki
* [Zarządzanie zasobami usługi Azure Blob Storage za pomocą Eksploratora usługi Storage (wersja zapoznawcza)](vs-azure-tools-storage-explorer-blobs.md)

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

