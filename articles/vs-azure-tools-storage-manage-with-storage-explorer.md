<properties
    pageTitle="Wprowadzenie do Eksploratora usługi Storage (wersja zapoznawcza) | Microsoft Azure"
    description="Zarządzanie zasobami magazynu platformy Azure za pomocą Eksploratora usługi Storage (wersja zapoznawcza)"
    services="visual-studio-online"
    documentationCenter="na"
    authors="TomArcher"
    manager="douge"
    editor="" />

 <tags
    ms.service="visual-studio-online"
    ms.devlang="multiple"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="06/05/2016"
    ms.author="tarcher" />

# Wprowadzenie do Eksploratora usługi Storage (wersja zapoznawcza)

## Omówienie 

Eksplorator usługi Microsoft Azure Storage (wersja zapoznawcza) jest aplikacją autonomiczną, która umożliwia łatwą obsługę danych w usłudze Azure Storage w systemie Windows, OS X i Linux. Ten artykuł zawiera informacje dotyczące różnych sposobów łączenia się z kontami magazynu Azure i zarządzania nimi.

## Wymagania wstępne

- [Pobieranie i instalowanie Eksploratora usługi Storage (wersja zapoznawcza)](http://go.microsoft.com/fwlink/?LinkId=708343)

## Łączenie się z usługą lub kontem magazynu

Eksplorator usługi Storage (wersja zapoznawcza) oferuje wiele sposobów łączenia się z kontem magazynu. Obejmuje to łączenie się z kontami magazynu skojarzonymi z subskrypcjami platformy Azure, łączenie się z usługami i kontami magazynu usługami udostępnianymi z innych subskrypcji platformy Azure, a nawet łączenie się z magazynem lokalnym i zarządzanie nim przy użyciu emulatora usługi Azure Storage:

- [Łączenie się z subskrypcją platformy Azure](#connect-to-an-azure-subscription) — zarządzanie zasobami należącymi do subskrypcji platformy Azure.
- [Łączenie się z magazynem lokalnym](#connect-to-local-storage) — zarządzanie magazynem lokalnym przy użyciu emulatora usługi Azure Storage. 
- [Dołączanie do magazynu zewnętrznego](#attach-or-detach-an-external-storage-account) — zarządzanie zasobami magazynu należącymi do innej subskrypcji platformy Azure przy użyciu nazwy konta i klucza konta magazynu.
- [Dołączanie konta przy użyciu sygnatury dostępu współdzielonego](#attach-account-using-sas) — zarządzanie zasobami magazynu należącymi do innej subskrypcji platformy Azure przy użyciu sygnatury dostępu współdzielonego.
- [Dołączanie usługi przy użyciu sygnatury dostępu współdzielonego](#attach-service-using-sas) — zarządzanie określoną usługą magazynu (kontenerem, kolejką lub tabelą obiektów blob) należącą do innej subskrypcji platformy Azure przy użyciu sygnatury dostępu współdzielonego.

## Łączenie się z subskrypcją platformy Azure

> [AZURE.NOTE] Jeśli nie masz konta platformy Azure, możesz [utworzyć konto bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) lub [aktywować korzyści dla subskrybentów programu Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

1. Uruchom Eksploratora usługi Storage (wersja zapoznawcza). 

1. Jeśli uruchamiasz Eksploratora usługi Storage (wersja zapoznawcza) po raz pierwszy lub Eksplorator uslugi Storage (wersja zapoznawcza) był już wcześniej uruchamiany, ale nie nawiązywano połączenia z kontem platformy Azure, zostanie wyświetlony pasek informacyjny umożliwiający połączenie z kontem platformy Azure.

    ![][0]
    
1. Wybierz pozycję **Połącz z platformą Microsoft Azure** i postępuj zgodnie z instrukcjami wyświetlanymi w oknach dialogowych, aby zalogować się przy użyciu konta Microsoft skojarzonego z co najmniej jedną aktywną subskrypcją platformy Azure.

Po pomyślnym zalogowaniu się przy użyciu konta Microsoft w okienku po lewej stronie Eksploratora usługi Storage (wersja zapoznawcza) zostaną wyświetlone wszystkie konta magazynu skojarzone z wszystkimi subskrypcjami platformy Azure skojarzonymi z kontem Microsoft.

### Filtrowanie subskrypcji platformy Azure

Eksplorator usługi Storage (wersja zapoznawcza) umożliwia filtrowanie subskrypcji platformy Azure skojarzonych z kontami Microsoft, do których się zalogowano, w celu wyświetlenia odpowiednich konta w okienku po lewej stronie.

1. Wybierz ikonę **Ustawienia** (koło zębate).

    ![][1]   

1.  W górnej części okienka po lewej stronie zostanie wyświetlona lista rozwijana zawierająca wszystkie konta Microsoft, do których się zalogowano. 

    ![][3]
     
1.  Wybierz strzałkę w dół obok listy rozwijanej, aby wyświetlić listę wszystkich kont Microsoft, do których się zalogowano, oraz link do dodawania dodatkowych kont Microsoft (logowania się).

    ![][4]

1.  Wybierz odpowiednie konto Microsoft z listy rozwijanej.

1.  W okienku po lewej stronie zostaną wyświetlone wszystkie subskrypcje platformy Azure skojarzone z wybranym kontem Microsoft.
Pole wyboru po lewej stronie każdej subskrypcji platformy Azure pozwala określić, czy w Eksploratorze usługi Storage (wersja zapoznawcza) ma być wyświetlana lista wszystkich kont magazynu skojarzonych z tymi subskrypcjami platformy Azure. Zaznaczenie lub usunięcie zaznaczenia pola **Wszystkie subskrypcje** powoduje przełączenie między wyświetlaniem wszystkich lub żadnej z wymienionych subskrypcji platformy Azure.

    ![][2]  

1.  Po zakończeniu wybierania subskrypcji platformy Azure, którymi chcesz zarządzać, wybierz przycisk **Zastosuj**. W okienku po lewej stronie zostanie zaktualizowana lista wszystkich kont magazynu dla poszczególnych wybranych subskrypcji platformy Azure dla bieżącego konta Microsoft. 

### Dodawanie dodatkowych kont Microsoft

W kolejnych krokach objaśniono sposób podłączania dodatkowych kont Microsoft w celu wyświetlenia subskrypcji platformy Azure dla każdego konta oraz kont magazynu.

1.  Wybierz ikonę **Ustawienia** (koło zębate).

    ![][1]   

1.  W górnej części okienka po lewej stronie zostanie wyświetlona lista rozwijana zawierająca wszystkie obecnie podłączone konta Microsoft.

    ![][3]
     
1.  Wybierz strzałkę w dół obok listy rozwijanej, aby wyświetlić listę wszystkich kont Microsoft, do których się zalogowano, oraz link do dodawania dodatkowych kont Microsoft (logowania się).

    ![][4]

1.  Wybierz pozycję **Dodaj konto** i postępuj zgodnie z instrukcjami wyświetlanymi w oknach dialogowych, aby zalogować się do konta skojarzonego z co najmniej jedną aktywną subskrypcją platformy Azure.

1.  Zaznacz pola wyboru dla subskrypcji platformy Azure, które chcesz przeglądać. 

    ![][2]  

1.  Wybierz przycisk **Zastosuj**.

### Przełączanie między kontami Microsoft

Mimo że można łączyć się z wieloma kontami Microsoft, w okienku po lewej stronie są wyświetlane tylko konta magazynu skojarzone z subskrypcjami dla jednego (bieżącego) konta Microsoft. Jeśli połączono się z wieloma kontami Microsoft, można przełączać się między kontami, wykonując następujące czynności:

1.  Wybierz ikonę **Ustawienia** (koło zębate).

    ![][1]   

1.  W górnej części okienka po lewej stronie zostanie wyświetlona lista rozwijana zawierająca wszystkie obecnie podłączone konta Microsoft.

    ![][3]
     
1.  Wybierz strzałkę w dół obok listy rozwijanej, aby wyświetlić listę wszystkich kont Microsoft, do których się zalogowano, oraz link do dodawania dodatkowych kont Microsoft (logowania się).

    ![][4]

1.  Wybierz odpowiednie konto Microsoft.

1.  Zaznacz pola wyboru dla subskrypcji platformy Azure, które chcesz przeglądać. 

    ![][2]  

1.  Wybierz przycisk **Zastosuj**.
  
## Łączenie się z magazynem lokalnym

Eksplorator usługi Storage (wersja zapoznawcza) umożliwia obsługę magazynu lokalnego przy użyciu emulatora usługi Azure Storage. Dzięki temu można pisać kod dla magazynu i testować go bez konieczności posiadania konta magazynu wdrożonego na platformie Azure, ponieważ konto magazynu jest emulowane przez emulator usługi Azure Storage.

>[AZURE.NOTE] Emulator usługi Azure Storage jest obecnie obsługiwany tylko dla systemu Windows. 

1. Uruchom Eksploratora usługi Storage (wersja zapoznawcza). 

1. W okienku po lewej stronie rozwiń węzeł **(Programowanie)**.

    ![][21]

1. Jeśli emulator usługi Azure Storage nie został jeszcze zainstalowany, na pasku informacyjnym zostanie wyświetlony monit o jego zainstalowanie. Po wyświetleniu paska informacyjnego wybierz polecenie **Pobierz najnowszą wersję** i zainstaluj emulator. 

    ![][22]

1. Po zainstalowaniu emulatora będziesz mieć możliwość tworzenia lokalnych obiektów blob, kolejek i tabel oraz pracy z nimi. Aby dowiedzieć się, w jaki sposób pracować z poszczególnymi typami kont, wybierz odpowiedni link poniżej:

    - [Zarządzanie zasobami Magazynu obiektów blob Azure](./vs-azure-tools-storage-explorer-blobs.md)
    - Zarządzanie zasobami Magazynu kolejek Azure — *wkrótce*
    - Zarządzanie zasobami Magazynu tabel Azure — *wkrótce*

## Dołączanie lub odłączanie konta magazynu zewnętrznego

Eksplorator usługi Storage (wersja zapoznawcza) umożliwia dołączanie kont magazynu zewnętrznego, dzięki czemu można łatwo udostępniać konta magazynu. W tej sekcji opisano sposób dołączania (i odłączania) kont magazynu zewnętrznego.

### Uzyskiwanie poświadczeń konta magazynu

W celu udostępnienia konta magazynu zewnętrznego właściciel tego konta musi najpierw uzyskać poświadczenia — nazwę konta i klucz — dla konta, a następnie udostępnić te informacje osobie, która chce dołączyć do tego (zewnętrznego) konta. Poświadczenia konta magazynu można uzyskać za pośrednictwem Portalu Azure, wykonując następujące czynności: 

1.  Zaloguj się do [Portalu Azure](https://portal.azure.com).
1.  Wybierz pozycję **Przeglądaj**.
1.  Wybierz pozycję **Konta usługi Storage**.
1.  W bloku **Konta usługi Storage** wybierz odpowiednie konto magazynu.
1.  W bloku **Ustawienia** dla wybranego konta magazynu wybierz pozycję **Klucze dostępu**.

    ![][5]
    
1.  W bloku **Klucze dostępu** skopiuj wartość **NAZWA KONTA USŁUGI STORAGE** i **KLUCZ 1**, aby użyć ich podczas dołączania konta magazynu. 

    ![][6]

### Dołączanie konta magazynu zewnętrznego

1.  W Eksploratorze usługi Storage (wersja zapoznawcza) kliknij prawym przyciskiem myszy pozycję **Konta usługi Storage** i z menu kontekstowego wybierz pozycję **Dołącz zewnętrzną usługę Storage**.

    ![][7]
    
1.  W sekcji *Uzyskiwanie poświadczeń konta magazynu* wyjaśniono sposób uzyskiwania wartości nazwy konta magazynu i klucza 1. Te wartości zostaną użyte w kolejnym kroku. W oknie dialogowym **Dołączanie zewnętrznej usługi Storage** wprowadź nazwę konta magazynu w polu **Nazwa konta** i wprowadź wartość Klucz 1 w polu **Klucz konta**. Po zakończeniu wybierz przycisk **OK**. 

    ![][8]

    Po dołączeniu konto magazynu zewnętrznego będzie wyświetlane z tekstem **(Zewnętrzne)** dodanym na końcu nazwy konta magazynu. 

    ![][9]

### Odłączanie konta magazynu zewnętrznego

1.  Kliknij prawym przyciskiem myszy konto magazynu zewnętrznego, które chcesz odłączyć, a następnie z menu kontekstowego wybierz polecenie **Odłącz**.

    ![][10]

1.  Po wyświetleniu okna komunikatu z potwierdzeniem wybierz przycisk **Tak**, aby potwierdzić odłączenie konta magazynu zewnętrznego.

    ![][12]

## Dołączanie konta przy użyciu sygnatury dostępu współdzielonego

Sygnatura dostępu współdzielonego (SAS, Shared Access Signature) zapewnia administratorowi subskrypcji platformy Azure możliwość tymczasowego przyznania dostępu do konta magazynu bez konieczności podawania poświadczeń subskrypcji platformy Azure. 

Na przykład załóżmy, że Użytkownik_A jest administratorem subskrypcji platformy Azure i Użytkownik_A chce zezwolić Użytkownikowi_B na dostęp do konta magazynu przez ograniczony czas z określonymi uprawnieniami:

1. Użytkownik_A generuje sygnaturę dostępu współdzielonego (składającą się z parametrów połączenia dla konta magazynu) dla określonego przedziału czasu i z odpowiednimi uprawnieniami.
1. Użytkownik_A udostępnia sygnaturę dostępu współdzielonego osobie, która chce uzyskać dostęp do konta magazynu — w naszym przykładzie jest to Użytkownik_B.  
1. Użytkownik_B korzysta z Eksploratora usługi Storage (wersja zapoznawcza), aby dołączyć do konta należącego do Użytkownika_A przy użyciu dostarczonej sygnatury dostępu współdzielonego. 

### Uzyskiwanie sygnatury dostępu współdzielonego dla konta, które chcesz udostępnić

1.  Otwórz Eksploratora usługi Storage (wersja zapoznawcza).
1.  W okienku po lewej stronie kliknij prawym przyciskiem myszy konto magazynu, które chcesz udostępnić, i z menu kontekstowego wybierz polecenie **Uzyskaj sygnaturę dostępu współdzielonego**.

    ![][13]

1. W oknie dialogowym **Sygnatura dostępu współdzielonego** określ przedział czasu i uprawnienia dla konta, a następnie wybierz przycisk **Utwórz**.

    ![][14]
 
1. Zostanie wyświetlone drugie okno dialogowe **Sygnatura dostępu współdzielonego**, w którym będzie wyświetlona sygnatura dostępu współdzielonego. Wybierz przycisk **Kopiuj** obok pola **Parametry połączenia**, aby skopiować go do schowka. Wybierz przycisk **Zamknij**, aby zamknąć okno dialogowe.

### Dołączanie do udostępnionego konta przy użyciu sygnatury dostępu współdzielonego

1.  Otwórz Eksploratora usługi Storage (wersja zapoznawcza).
1.  W okienku po lewej stronie kliknij prawym przyciskiem myszy pozycję **Konta usługi Storage** i z menu kontekstowego wybierz pozycję **Dołącz konto przy użyciu sygnatury dostępu współdzielonego**.
    ![][15]

1. W oknie dialogowym **Dołączanie konta przy użyciu sygnatury dostępu współdzielonego**:

    - **Nazwa konta** — wprowadź nazwę, którą chcesz skojarzyć z tym kontem. **UWAGA:** nazwa konta nie musi być zgodna z oryginalną nazwą konta magazynu, dla którego wygenerowano sygnaturę dostępu współdzielonego. 
    - **Parametry połączenia** — wklej skopiowane wcześniej parametry.
    - Po zakończeniu wybierz przycisk **OK**.
    
    ![][16]

Po dołączeniu konto magazynu będzie wyświetlane z tekstem (SAS) dodanym na końcu podanej nazwy konta.

![][17]

## Dołączanie usługi przy użyciu sygnatury dostępu współdzielonego

W sekcji [Dołączanie konta przy użyciu sygnatury dostępu współdzielonego](#attach-account-using-sas) omówiono sposób udzielania tymczasowego dostępu do konta magazynu przez administratora subskrypcji platformy Azure przez wygenerowanie (i udostępnienie) sygnatury dostępu współdzielonego dla konta magazynu. Podobnie można wygenerować sygnatury dostępu współdzielonego dla określonej usługi (kontenera, kolejki lub tabeli obiektów blob) w ramach konta magazynu.  

### Generowanie sygnatury dostępu współdzielonego dla usługi, którą chcesz udostępnić

W tym kontekście usługa może być kontenerem, kolejką lub tabelą obiektów blob. W poniższych sekcjach wyjaśniono sposób generowania sygnatury dostępu współdzielonego dla wymienionych usług:

- [Uzyskiwanie sygnatury dostępu współdzielonego dla kontenera obiektów blob](./vs-azure-tools-storage-explorer-blobs.md#get-the-sas-for-a-blob-container)
- Uzyskiwanie sygnatury dostępu współdzielonego dla kolejki — *wkrótce*
- Uzyskiwanie sygnatury dostępu współdzielonego dla tabeli — *wkrótce*

### Dołączanie do usługi udostępnionego konta przy użyciu sygnatury dostępu współdzielonego

1.  Otwórz Eksploratora usługi Storage (wersja zapoznawcza).
1.  W okienku po lewej stronie kliknij prawym przyciskiem myszy pozycję **Konta usługi Storage** i z menu kontekstowego wybierz pozycję **Dołącz usługę przy użyciu sygnatury dostępu współdzielonego**.
    ![][18]

1. W oknie dialogowym **Dołączanie konta przy użyciu sygnatury dostępu współdzielonego** wklej skopiowany wcześniej identyfikator URI sygnatury dostępu współdzielonego i wybierz przycisk **OK**.

    ![][19]

Po dołączeniu nowo dołączona usługa zostanie wyświetlona w węźle **(Sygnatura dostępu współdzielonego usługi)**. 

![][20]

## Wyszukiwanie kont magazynu

Jeśli masz długą listę kont magazynu, możesz szybko odnaleźć określone konto magazynu przy użyciu pola wyszukiwania w górnej części okienka po lewej stronie. 

Podczas wpisywania w polu wyszukiwania w okienku po lewej stronie będą wyświetlane tylko konta magazynu pasujące do podanej wartości wyszukiwania. Na poniższym ekranie przedstawiono przykład, gdzie wyszukano wszystkie konta magazynu, których nazwa zawiera tekst „tarcher”.

![][11]
    
Aby wyczyścić wyszukiwanie, wybierz przycisk **x** w polu wyszukiwania.

## Następne kroki
- [Zarządzanie zasobami magazynu obiektów blob Azure za pomocą Eksploratora usługi Storage (wersja zapoznawcza)](./vs-azure-tools-storage-explorer-blobs.md)

[0]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connect-to-azure.png
[1]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/settings-gear.png
[2]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/filter-subscriptions.png
[3]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/filter-accounts.png
[4]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/accounts-drop-down.png
[5]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/access-keys.png
[6]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/access-keys-copy.png
[7]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-external-storage.png
[8]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-external-storage-dlg.png
[9]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/external-storage-account.png
[10]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/detach-external-storage.png
[11]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/storage-account-search.png
[12]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/detach-external-storage-confirmation.png
[13]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/get-sas-context-menu.png
[14]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/get-sas-dlg1.png
[15]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-account-using-sas-context-menu.png
[16]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-account-using-sas-dlg.png
[17]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-account-using-sas-finished.png
[18]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-service-using-sas-context-menu.png
[19]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-service-using-sas-dlg.png
[20]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-service-using-sas-finished.png
[21]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/local-storage-drop-down.png
[22]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/download-storage-emulator.png



<!--HONumber=Jun16_HO2-->


