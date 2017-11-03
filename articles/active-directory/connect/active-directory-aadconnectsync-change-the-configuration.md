---
title: 'Synchronizacja programu Azure AD Connect: Konfiguracja synchronizacji Azure AD Connect | Dokumentacja firmy Microsoft'
description: "Przedstawiono sposób zmiany w konfiguracji synchronizacji Azure AD Connect."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 7b9df836-e8a5-4228-97da-2faec9238b31
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 63a7ae9d39e1a74294637172efd607ee41b2d69b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-ad-connect-sync-how-to-make-a-change-to-the-default-configuration"></a>Synchronizacja programu Azure AD Connect: jak wprowadzić zmianę do konfiguracji domyślnej
W tym temacie ma na celu zademonstrować sposób wprowadzania zmian w domyślnej konfiguracji synchronizacji Azure AD Connect. Ona instrukcje dla niektórych typowych scenariuszy. Z tym wiedzy należy wprowadzać pewne zmiany do własnych konfiguracji na podstawie własnych reguł biznesowych.

## <a name="synchronization-rules-editor"></a>Edytor reguł synchronizacji
Edytor reguł synchronizacji jest używany do Zobacz i zmienianie konfiguracji domyślnej. Możesz go znaleźć w folderze, w Menu Start **Azure AD Connect** grupy.  
![Edytor reguł synchronizacji z Menu Start](./media/active-directory-aadconnectsync-change-the-configuration/startmenu2.png)

Po otwarciu, zostanie wyświetlony domyślnych reguł out-of-box.

![Edytor reguł synchronizacji](./media/active-directory-aadconnectsync-change-the-configuration/sre2.png)

### <a name="navigating-in-the-editor"></a>Nawigacja w edytorze
Listy rozwijane w górnej części edytora pozwala szybko znaleźć określonej reguły. Na przykład jeśli chcesz wyświetlić reguły, gdzie znajduje się atrybut proxyAddresses, możesz zmienić listach rozwijanych do następującego:  
![Filtrowanie SRE](./media/active-directory-aadconnectsync-change-the-configuration/filtering.png)  
Aby zresetować filtrowania i załadować nową konfigurację, naciśnij **F5** na klawiaturze.

Do góry po prawej stronie, znajduje się przycisk **Dodaj nową regułę**. Ten przycisk służy do tworzenia reguły niestandardowej.

Na dole masz przycisków dla działania w regule wybranego synchronizacji. **Edytuj** i **usunąć** oczekiwaniami ich. **Eksportuj** tworzy skrypt programu PowerShell dla ponownego tworzenia reguły synchronizacji. Ta procedura umożliwia przeniesienie reguły synchronizacji z jednego serwera na inny.

## <a name="create-your-first-custom-rule"></a>Tworzenie pierwszej reguły niestandardowe
Najbardziej typowe zmiana jest zmiany przepływów atrybutów. Dane w katalogu źródłowym nie może być tak jak Azure AD. W tym przykładzie w tej sekcji mają upewnij się, że dana nazwa użytkownika jest zawsze w **przypadku prawidłowego**.

### <a name="disable-the-scheduler"></a>Wyłączanie harmonogramu
[Harmonogramu](active-directory-aadconnectsync-feature-scheduler.md) domyślnie uruchamiane co 30 minut. Chcesz upewnij się, że nie jest uruchomiona podczas wprowadzania zmian i rozwiązywanie problemów z nowych zasad. Aby tymczasowo wyłączyć harmonogram, uruchom program PowerShell i uruchom`Set-ADSyncScheduler -SyncCycleEnabled $false`

![Wyłączanie harmonogramu](./media/active-directory-aadconnectsync-change-the-configuration/schedulerdisable.png)  

### <a name="create-the-rule"></a>Utworzyć regułę
1. Kliknij przycisk **Dodaj nową regułę**.
2. Na **opis** strony wprowadź następujące:  
   ![Liczba przychodzących reguł filtrowania](./media/active-directory-aadconnectsync-change-the-configuration/description2.png)  
   * Nazwa: Nadaj regule nazwę opisową.
   * Opis: Niektóre wyjaśnienie, ktoś inny może zrozumieć, co to jest reguła dla.
   * Połączony system: obiekt znajduje się w systemie. W takim przypadku wybierz łącznik usługi Active Directory.
   * Typ obiektu połączony System/Metaverse: Wybierz **użytkownika** i **osoby** odpowiednio.
   * Typ łącza: Zmień tę wartość na **Join**.
   * Pierwszeństwo: Podaj wartość, która jest unikatowa w systemie. Niższa wartość liczbowa oznacza wyższy priorytet.
   * Tag: Może pozostać puste. Tylko reguły out-of-box firmy Microsoft powinna mieć to pole wypełniane przy użyciu wartości.
3. Na **filtru Scoping** wprowadź **givenName ISNOTNULL**.  
   ![Reguła filtr zakresów ruchu przychodzącego](./media/active-directory-aadconnectsync-change-the-configuration/scopingfilter.png)  
   W tej sekcji służy do definiowania, które obiekty reguła powinna mieć zastosowanie do. Jeśli pole pozostanie puste, reguła powinna zostać zastosowana do wszystkich obiektów użytkowników. Ale obejmowałoby sal konferencyjnych, kont usług i innych obiektów użytkownika z systemem innym niż osób.
4. Na **dołączyć reguły**, pozostaw puste.
5. Na **przekształcenia** Zmień dla przepływu do **wyrażenia**. Wybierz atrybut docelowy **givenName**, a w źródle wprowadź `PCase([givenName])`.
   ![Reguła ruchu przychodzącego przekształcenia](./media/active-directory-aadconnectsync-change-the-configuration/transformations.png)  
   Aparat synchronizacji jest rozróżniana wielkość liter, zarówno na nazwę funkcji, jak i nazwę atrybutu. Jeśli wpiszesz problem, zobacz ostrzeżenie podczas dodawania reguły. Edytor umożliwia zapisać i kontynuować, więc będzie musiał ponownie reguły i Popraw regułę.
6. Kliknij przycisk **Dodaj** można zapisać reguły.

Nowe reguły niestandardowe powinny być widoczne z innych reguł synchronizacji w systemie.

### <a name="verify-the-change"></a>Sprawdź zmiany
Dzięki tej zmianie nowego chcesz upewnij się, że działa zgodnie z oczekiwaniami i nie jest zgłaszanie błędów. W zależności od liczby obiektów, do których masz istnieją dwa różne sposoby, aby wykonać ten krok.

1. Uruchom pełną synchronizację wszystkich obiektów
2. Uruchom Podgląd i pełną synchronizację dla pojedynczego obiektu

Uruchom **usługi synchronizacji** z start menu. Kroki opisane w tej sekcji są w tego narzędzia.

1. **Pełna synchronizacja wszystkich obiektów**  
   Wybierz **łączniki** u góry. Identyfikacja łącznika wprowadzone zmiany w poprzedniej sekcji, w tym przypadku usług domenowych Active Directory i zaznacz go. Wybierz **Uruchom** z działań i wybierz **pełną synchronizację** i **OK**.
   ![Pełna synchronizacja](./media/active-directory-aadconnectsync-change-the-configuration/fullsync.png)  
   Obiekty są teraz zaktualizować w magazynie metaverse. Teraz należy przyjrzeć się obiektu w magazynie metaverse.
2. **Podgląd i pełną synchronizację dla pojedynczego obiektu**  
   Wybierz **łączniki** u góry. Identyfikacja łącznika wprowadzone zmiany w poprzedniej sekcji, w tym przypadku usług domenowych Active Directory i zaznacz go. Wybierz **wyszukiwania przestrzeni łącznika**. Użyj zakresu można znaleźć obiektu, którego chcesz użyć do testowania zmiany. Wybierz obiekt i kliknij przycisk **Podgląd**. W nowym ekranie Wybierz **zatwierdzić Podgląd**.  
   ![Zatwierdź podglądu](./media/active-directory-aadconnectsync-change-the-configuration/commitpreview.png)  
   Zmiana teraz przywiązuje dużą wagę do środowiska metaverse.

**Przyjrzyj się obiektu w magazynie metaverse**  
Teraz należy wybrać kilka obiektów próbki do upewnij się, że ma wartość i reguła zastosowana. Wybierz **wyszukiwanie Metaverse** od góry. Dodaj dowolny filtr, aby odnaleźć odpowiednich obiektów. W wyniku wyszukiwania Otwórz obiektu. Przyjrzyj się wartości atrybutów, a także Sprawdź, czy w **reguły synchronizacji** kolumny, która jest stosowana reguła zgodnie z oczekiwaniami.  
![Wyszukiwanie magazynu metaverse](./media/active-directory-aadconnectsync-change-the-configuration/mvsearch.png)  

### <a name="enable-the-scheduler"></a>Włącz harmonogram
Jeśli wszystko, co jest zgodne z oczekiwaniami, możesz włączyć ponownie harmonogramu. Z programu PowerShell, uruchom `Set-ADSyncScheduler -SyncCycleEnabled $true`.

## <a name="other-common-attribute-flow-changes"></a>Inne typowe zmiany przepływu atrybutów
Poprzedniej sekcji opisano sposób wprowadzania zmian do przepływu atrybutu. W tej sekcji znajdują się dodatkowe przykłady. Kroki dotyczące sposobu tworzenia reguły synchronizacji jest skrócona, ale pełny procedurę można znaleźć w poprzedniej sekcji.

### <a name="use-another-attribute-than-the-default"></a>Użyj atrybutu innego niż domyślny
W firmie Fabrikam Brak lasu, gdzie są używane lokalne alfabetu imię, nazwisko i nazwę wyświetlaną. Zestaw znaków łacińskich reprezentację te atrybuty można znaleźć w atrybutach rozszerzenia. Podczas kompilowania na globalnej liście adresowej w usłudze Azure AD i Office 365, organizacja chce tych atrybutów, aby zamiast tego należy użyć.

Z konfiguracji domyślnej obiekt z lokalnego lasu wygląda następująco:  
![Przepływ atrybutów 1](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp1.png)

Aby utworzyć regułę z innych przepływów atrybutów, wykonaj następujące czynności:

* Uruchom **Synchronization Rule Editor** z start menu.
* Z **przychodzący** wybrane do lewej strony, kliknij przycisk **Dodaj nową regułę**.
* Nadaj regule nazwę i opis. Wybierz w lokalnej usłudze Active Directory i typy odpowiednich obiektów. W **typu łącza**, wybierz pozycję **Join**. Pierwszeństwa wybierz numer, który nie jest używany przez inną regułę. Reguły out-of-box rozpoczyna się od 100, dlatego wartość 50 może być używana w tym przykładzie.
  ![Przepływ atrybutu 2](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp2.png)
* Pozostaw puste zakresu (to znaczy stosuje się do wszystkich obiektów użytkowników w lesie).
* Pozostaw puste zasady sprzężenia (let oznacza to regułę out-of-box obsługi wszelkie sprzężenia).
* W przekształcenia należy utworzyć następujące przepływów:  
  ![Przepływ atrybutu 3](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp3.png)
* Kliknij przycisk **Dodaj** można zapisać reguły.
* Przejdź do **Menedżera usługi synchronizacji**. Na **łączniki**, wybierz łącznik, gdzie dodana reguła. Wybierz **Uruchom**, i **pełnej synchronizacji**. Pełna synchronizacja ponownie oblicza wszystkie obiekty przy użyciu bieżące reguły.

Jest to wynik dla tego samego obiektu przy użyciu tej reguły niestandardowe:  
![Przepływ atrybutu 4](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp4.png)

### <a name="length-of-attributes"></a>Długość atrybutów
Atrybuty ciągu są domyślnie ustawione jako można indeksować, a maksymalna długość to 448 znaków. Następnie podczas pracy z atrybutami ciągów, które mogą zawierać więcej, upewnij się, że obejmują przepływ atrybutów:  
`attributeName` <- `Left([attributeName],448)`

### <a name="changing-the-userprincipalsuffix"></a>Zmiana userPrincipalSuffix
Atrybut userPrincipalName w usłudze Active Directory nie jest znany zawsze przez użytkowników i mogą nie być odpowiednie jako identyfikator logowania. Azure AD Connect, Kreator instalacji usługi synchronizacji umożliwia pobranie inny atrybut, na przykład poczty. Jednak w niektórych przypadkach atrybutu muszą zostać obliczone. Na przykład firma Contoso ma dwa katalogi usługi Azure AD, co w środowisku produkcyjnym i jeden do testowania. Mają one użytkowników w swojej dzierżawy testu, aby użyć innego sufiksu w identyfikator logowania.  
`userPrincipalName` <- `Word([userPrincipalName],1,"@") & "@contosotest.com"`

W tym wyrażeniu zająć wszystko po lewej stronie pierwszego @-sign (Word) i ZŁĄCZ.teksty z ciągiem stałym.

### <a name="convert-a-multi-value-to-a-single-value"></a>Konwertuj wielu wartościach na pojedynczą wartość
Niektóre atrybuty w usłudze Active Directory są wielowartościowe w schemacie, mimo że wyglądają jednej wartości w użytkownicy usługi Active Directory i komputery. Przykładem jest atrybut Opis.  
`description` <- `IIF(IsNullOrEmpty([description]),NULL,Left(Trim(Item([description],1)),448))`

W tym wyrażeniu w przypadku, gdy atrybut ma wartość, wykonać pierwszego elementu (element) w atrybucie, usunięcia spacji wiodących i końcowych spacji (Trim), a następnie zapewnić najpierw 448 znaków (po lewej) w ciągu.

### <a name="do-not-flow-an-attribute"></a>Nie przepływu atrybutu
W tle na scenariusz dla tej sekcji, zobacz [kontrolować proces przepływu atrybutu](active-directory-aadconnectsync-understanding-declarative-provisioning.md#control-the-attribute-flow-process).

Istnieją dwa sposoby nie przepływu atrybutu. Pierwszy jest dostępna w Kreatorze instalacji i pozwala na [Usuń wybranych atrybutów](active-directory-aadconnect-get-started-custom.md#azure-ad-app-and-attribute-filtering). Ta opcja działa, jeśli atrybut przed nigdy nie zostały zsynchronizowane. Jednak jeśli zostało rozpoczęte zsynchronizować ten atrybut i usuń go później przy użyciu tej funkcji, zatrzymuje aparatu synchronizacji Zarządzanie atrybutu i istniejące wartości są pozostawiane w usłudze Azure AD.

Jeśli chcesz usunąć wartość atrybutu i upewnij się, że nie przepływu w przyszłości, należy zamiast tego Utwórz regułę niestandardową.

W firmie Fabrikam mieć uświadomiliśmy sobie, że niektóre atrybuty, które firma Microsoft synchronizować do chmury nie będą już istnieje. Chcemy upewnić się, że te atrybuty są usuwane z usługi Azure AD.  
![Złe rozszerzenie atrybutów](./media/active-directory-aadconnectsync-change-the-configuration/badextensionattribute.png)

* Utwórz nową regułę synchronizacji ruchu przychodzącego i wypełnić opisu ![opisów](./media/active-directory-aadconnectsync-change-the-configuration/syncruledescription.png)
* Tworzenie przepływów atrybutów typu **wyrażenie** i ze źródłem **AuthoritativeNull**. Literał **AuthoritativeNull** wskazuje, że wartość powinna być pusta w MV nawet wtedy, gdy próbuje wypełnić wartości niższe pierwszeństwo reguły synchronizacji.
  ![Przekształcania atrybuty rozszerzenia](./media/active-directory-aadconnectsync-change-the-configuration/syncruletransformations.png)
* Zapisz reguły synchronizacji. Uruchom **usługi synchronizacji**, znajdź łącznik, wybierz **Uruchom**, i **pełnej synchronizacji**. Ten krok ponownie oblicza wszystkie przepływy atrybutów.
* Sprawdź, czy zamierzone zmiany do wyeksportowania wyszukując przestrzeni łącznika.
  ![Usuń przemieszczanego](./media/active-directory-aadconnectsync-change-the-configuration/deletetobeexported.png)

## <a name="create-rules-with-powershell"></a>Tworzenie reguł przy użyciu programu PowerShell
Za pomocą edytora reguły synchronizacji działa prawidłowo, jeśli masz tylko wprowadzić kilka zmian. Jeśli potrzebujesz wprowadzania zmian, programu PowerShell może być lepszym rozwiązaniem. Niektóre zaawansowane funkcje są dostępne tylko przy użyciu programu PowerShell.

### <a name="get-the-powershell-script-for-an-out-of-box-rule"></a>Pobierz skrypt programu PowerShell dla reguły out-of-box
Aby wyświetlić programu PowerShell skryptu, który utworzył regułę out-of-box, wybierz regułę w edytorze reguły synchronizacji i kliknij przycisk **wyeksportować**. Ta akcja umożliwia skrypt programu PowerShell, który utworzył regułę.

### <a name="advanced-precedence"></a>Pierwszeństwo zaawansowane
Reguły synchronizacji out-of-box rozpoczynać się od wartości pierwszeństwo 100. Jeśli masz wiele lasów i które należy podjąć wiele zmian niestandardowych, następnie reguły synchronizacji 99 może nie być wystarczającej ilości.

Możesz wydać aparatu synchronizacji, które mają dodatkowe reguły wstawiony przed regułami out-of-box. Aby uzyskać ten problem, wykonaj następujące kroki:

1. Oznacz pierwszej reguły synchronizacji out-of-box (ta reguła jest **w od użytkownika AD Join**) Edytor reguł synchronizacji i wybierz **wyeksportować**. Skopiuj wartość identyfikatora SR.  
![PowerShell przed zmianą](./media/active-directory-aadconnectsync-change-the-configuration/powershell1.png)  
2. Utwórz nową regułę synchronizacji. Edytor reguł synchronizacji służy do jej utworzenia. Eksportuj zasady do skryptu programu PowerShell.
3. We właściwości **PrecedenceBefore**, Wstaw wartość identyfikatora z reguły out-of-box. Ustaw **pierwszeństwo** do **0**. Upewnij się, atrybut Identyfikator jest unikatowy i identyfikator GUID z inną regułą nie ponownego użycia. Ponadto upewnij się, że **ImmutableTag** nie ustawiono właściwości; tej właściwości można ustawić tylko dla reguły out-of-box. Zapisz skrypt programu PowerShell i uruchom go. Wynik jest niestandardowe reguły jest przypisywana wartość priorytetu 100, a wszystkie inne zasady out-of-box jest zwiększany.  
![PowerShell po zmianie](./media/active-directory-aadconnectsync-change-the-configuration/powershell2.png)  

Może mieć wiele reguł synchronizacji niestandardowych korzystającej z tego samego **PrecedenceBefore** wartości w razie potrzeby.


## <a name="enable-synchronization-of-preferreddatalocation"></a>Włącz synchronizację PreferredDataLocation
Azure AD Connect obsługuje synchronizacji **PreferredDataLocation** atrybutu dla **użytkownika** obiektów w wersji 1.1.524.0 i po. W szczególności wprowadzono następujące zmiany:

* Schemat typu obiektu **użytkownika** w programie Azure AD Connector jest rozszerzona, aby uwzględnić atrybut PreferredDataLocation, który jest typu string i jest pojedynczej wartości.

* Schemat typu obiektu **osoby** w magazynie Metaverse jest rozszerzona, aby uwzględnić atrybut PreferredDataLocation, który jest typu string i jest pojedynczej wartości.

Domyślnie ten atrybut PreferredDataLocation nie włączono synchronizacji ponieważ nie ma odpowiedniego atrybutu PreferredDataLocation w lokalnej usłudze Active Directory. Musisz ręcznie włączyć synchronizację.

> [!IMPORTANT]
> Obecnie usługi Azure AD umożliwia atrybutu PreferredDataLocation do użytkownika obiektów synchronizowanych i chmury użytkownika obiekty bezpośrednio skonfigurowany przy użyciu programu Azure AD PowerShell. Po włączeniu synchronizacji atrybutów PreferredDataLocation, musisz zatrzymać przy użyciu programu Azure AD PowerShell do konfigurowania atrybutu na **synchronizowane obiekty użytkownika** jako Azure AD Connect zostaną one zastąpione na podstawie Źródło wartości atrybutów w lokalnej usłudze Active Directory.

> [!IMPORTANT]
> Na 1 września 2017 r, Azure AD będzie już miało możliwości atrybutu PreferredDataLocation na **synchronizowane obiekty użytkownika** bezpośrednio skonfigurować przy użyciu programu Azure AD PowerShell. Aby skonfigurować atrybut PreferredLocation synchronizowane obiekty użytkownika, należy użyć Azure tylko AD Connect.

Przed włączeniem synchronizacji atrybutu PreferredDataLocation, należy:

 * Najpierw zdecyduj, które lokalnymi atrybut usługi Active Directory mają być używane jako atrybut źródłowy. Powinien być typu **ciąg** i **jednowartościowych**.

 * Atrybut PreferredDataLocation zostały wcześniej skonfigurowane na istniejące synchronizowane obiekty użytkownika w usłudze Azure AD przy użyciu programu PowerShell usługi Azure AD, należy **Poprawka usterki systemu** wartości atrybutów do odpowiednich obiektów użytkowników lokalne usługi Active Directory.
 
    > [!IMPORTANT]
    > Jeśli jednak nie Poprawka usterki systemu wartości atrybutów, aby odpowiednie obiekty użytkownika w lokalnej usłudze Active Directory Azure AD Connect spowoduje usunięcie istniejącej wartości atrybutów w usłudze Azure AD po włączeniu synchronizacji dla atrybutu PreferredDataLocation.

 * Zalecane jest skonfigurowanie atrybutu źródłowego na co najmniej kilka lokalnych użytkowników usługi AD obiekty, które mogą służyć do weryfikacji później.
 
Kroki umożliwiające włączenie synchronizacji atrybutu PreferredDataLocation można podsumować jako:

1. Wyłączanie harmonogramu synchronizacji i sprawdź, że nie ma synchronizacji w toku

2. Dodaj atrybut źródłowy do lokalnej schematu w łączniku AD

3. Dodaj PreferredDataLocation do schematu łącznika usługi Azure AD

4. Utwórz regułę synchronizacji ruchu przychodzącego na przepływ wartości atrybutu z lokalnej usługi Active Directory

5. Utwórz regułę synchronizacji ruchu wychodzącego przepływu wartość atrybutu do usługi Azure AD

6. Uruchom pełną synchronizację cyklu

7. Włącz harmonogram synchronizacji

> [!NOTE]
> Pozostałej części tej sekcji omówiono następujące kroki w szczegółach. Zostały one opisane w kontekście wdrożenia usługi Azure AD z topologii z jednym lasem i bez reguł niestandardowych. Jeśli masz topologią wielu lasów, reguły synchronizacji niestandardowych skonfigurowane lub serwer przemieszczania, należy odpowiednio zmienić kroki.

### <a name="step-1-disable-sync-scheduler-and-verify-there-is-no-synchronization-in-progress"></a>Krok 1: Wyłącz harmonogramu synchronizacji i sprawdź, że nie ma synchronizacji w toku
Upewnij się, że synchronizacja nie ma miejsce, gdy trwa aktualizowanie reguły synchronizacji, aby uniknąć zmian niezamierzone eksportowane do usługi Azure AD. Aby wyłączyć harmonogram synchronizacji wbudowany:

 1. Uruchom sesję programu PowerShell na serwerze programu Azure AD Connect.

 2. Wyłącz zaplanowanej synchronizacji, uruchamiając polecenie cmdlet:`Set-ADSyncScheduler -SyncCycleEnabled $false`
 
 3. Uruchom **Menedżera usługi synchronizacji** , przechodząc do rozpoczęcia → usługi synchronizacji.
 
 4. Przejdź do **operacji** karcie i upewnij się, Brak operacji ze stanem *"w"toku.*

![Menedżera usługi synchronizacji — Sprawdź żadnych operacji w toku](./media/active-directory-aadconnectsync-change-the-configuration/preferredDataLocation-step1.png)

### <a name="step-2-add-the-source-attribute-to-the-on-premises-ad-connector-schema"></a>Krok 2: Dodaj atrybut źródłowy do lokalnej schematu w łączniku AD
Nie wszystkie atrybuty AD są importowane do lokalnej przestrzeni łącznika usługi AD. Aby dodać atrybut źródłowy do listy importowanych atrybuty:

 1. Przejdź do **łączniki** kartę Menedżera usługi synchronizacji.
 
 2. Kliknij prawym przyciskiem myszy **lokalnego łącznika AD** i wybierz **właściwości**.
 
 3. W wyskakującym oknie dialogowym, przejdź do **wybierz atrybuty** kartę.
 
 4. Upewnij się, że atrybut źródłowy jest zaznaczona na liście atrybutów.
 
 5. Kliknij przycisk **OK** do zapisania.

![Dodaj atrybut źródłowy do lokalnego schematu w łączniku AD](./media/active-directory-aadconnectsync-change-the-configuration/preferredDataLocation-step2.png)

### <a name="step-3-add-preferreddatalocation-to-the-azure-ad-connector-schema"></a>Krok 3: Dodawanie PreferredDataLocation schematu łącznika usługi Azure AD
Domyślnie ten atrybut PreferredDataLocation nie zostanie zaimportowane do platformy Azure AD Connect miejsca. Aby dodać atrybut PreferredDataLocation na liście atrybutów zaimportowane:

 1. Przejdź do **łączniki** kartę Menedżera usługi synchronizacji.

 2. Kliknij prawym przyciskiem myszy **łącznika usługi Azure AD** i wybierz **właściwości**.

 3. W wyskakującym oknie dialogowym, przejdź do **wybierz atrybuty** kartę.

 4. Upewnij się, że atrybut PreferredDataLocation jest zaznaczona na liście atrybutów.

 5. Kliknij przycisk **OK** do zapisania.

![Dodaj atrybut źródłowy do schematu łącznika usługi Azure AD](./media/active-directory-aadconnectsync-change-the-configuration/preferredDataLocation-step3.png)

### <a name="step-4-create-an-inbound-synchronization-rule-to-flow-the-attribute-value-from-on-premises-active-directory"></a>Krok 4: Utwórz regułę synchronizacji ruchu przychodzącego na przepływ wartości atrybutu z lokalnej usługi Active Directory
Reguła synchronizacji ruchu przychodzącego zezwala na wartość atrybutu mogą przepływać z atrybutu źródłowego z lokalnej usługi Active Directory do środowiska Metaverse:

1. Uruchom **Edytor reguł synchronizacji** , przechodząc do Edytor reguł synchronizacji START →.

2. Ustaw filtr wyszukiwania **kierunek** jako **przychodzący**.

3. Kliknij przycisk **Dodaj nową regułę** przycisk, aby utworzyć nową regułę ruchu przychodzącego.

4. W obszarze **opis** karcie, podaj następującej konfiguracji:
 
    | Atrybut | Wartość | Szczegóły |
    | --- | --- | --- |
    | Nazwa | *Podaj nazwę* | Na przykład *"w z usługi Active Directory — PreferredDataLocation użytkownika"* |
    | Opis | *Podaj opis* |  |
    | System połączony | *Wybierz lokalną łączniku AD* |  |
    | Połączony System typu obiektu | **Użytkownika** |  |
    | Typ obiektu Metaverse | **Osoby** |  |
    | Typ łącza | **Dołącz** |  |
    | Priorytet | *Wybierz liczbę z zakresu od 1 – 99* | 1 – 99 jest zarezerwowana dla reguły synchronizacji niestandardowych. Odbiera wartość, która jest używana przez inną regułę synchronizacji. |

5. Przejdź do **filtru Scoping** karta i Dodaj **pojedynczego grupę ustalania zakresu filtru z następującą klauzulę**:
 
    | Atrybut | Operator | Wartość |
    | --- | --- | --- |
    | adminDescription | NOTSTARTWITH | Użytkownika\_ | 
 
    Filtr zakresu określa, które lokalna Usługa AD obiekty ta reguła synchronizacji ruchu przychodzącego. W tym przykładzie używamy tego samego zakresu filtru używane jako *"w z usługi Active Directory — typowe użytkownika"* reguły synchronizacji OOB, co zapobiega reguły synchronizacji są stosowane do użytkownika obiekty utworzone za pomocą funkcji zapisywania zwrotnego użytkowników usługi AD platformy Azure. Może być konieczne dostosowanie zakresu filtru zgodnie z wdrożenia usługi Azure AD Connect.

6. Przejdź do **tab przekształcenia** i implementuje następującą regułę przekształcania:
 
    | Typ przepływu | Atrybut TARGET | Element źródłowy | Zastosuj raz | Scal typu |
    | --- | --- | --- | --- | --- |
    | Bezpośrednie | PreferredDataLocation | Wybierz atrybut źródłowy | Unchecked | Aktualizacja |

7. Kliknij przycisk **Dodaj** można utworzyć reguły dla ruchu przychodzącego.

![Utwórz regułę synchronizacji ruchu przychodzącego](./media/active-directory-aadconnectsync-change-the-configuration/preferredDataLocation-step4.png)

### <a name="step-5-create-an-outbound-synchronization-rule-to-flow-the-attribute-value-to-azure-ad"></a>Krok 5: Tworzenie reguły synchronizacji wychodzący przepływ wartość atrybutu do usługi Azure AD
Reguła synchronizacji ruchu wychodzącego zezwala na wartość atrybutu mogą przepływać z Metaverse z atrybutem PreferredDataLocation w usłudze Azure AD:

1. Przejdź do **reguły synchronizacji** edytora.

2. Ustaw filtr wyszukiwania **kierunek** jako **wychodzące**.

3. Kliknij przycisk **Dodaj nową regułę** przycisku.

4. W obszarze **opis** karcie, podaj następującej konfiguracji:

    | Atrybut | Wartość | Szczegóły |
    | --- | --- | --- |
    | Nazwa | *Podaj nazwę* | Na przykład "się do usługi AAD — użytkownik PreferredDataLocation" |
    | Opis | *Podaj opis* |
    | System połączony | *Wybierz łącznik AAD* |
    | Połączony System typu obiektu | Użytkownik ||
    | Typ obiektu Metaverse | **Osoby** ||
    | Typ łącza | **Dołącz** ||
    | Priorytet | *Wybierz liczbę z zakresu od 1 – 99* | 1 – 99 jest zarezerwowana dla reguły synchronizacji niestandardowych. YDo odbiera wartość, która jest używana przez inną regułę synchronizacji. |

5. Przejdź do **filtru Scoping** karta i Dodaj **pojedynczego grupę ustalania zakresu filtru z klauzulami dwóch**:
 
    | Atrybut | Operator | Wartość |
    | --- | --- | --- |
    | sourceObjectType | RÓWNOŚCI | Użytkownik |
    | cloudMastered | NOTEQUAL | True |

    Filtr zakresu określa obiekty usługi Azure AD, że ta reguła synchronizacji ruchu wychodzącego. W tym przykładzie używamy tego samego zakresu filtru z "Out do AD — tożsamości użytkownika" OOB reguły synchronizacji. Reguła synchronizacji uniemożliwia są stosowane do obiektów użytkowników, które nie są synchronizowane z lokalnej usługi Active Directory. Może być konieczne dostosowanie zakresu filtru zgodnie z wdrożenia usługi Azure AD Connect.
    
6. Przejdź do **przekształcania** karcie i implementuje następującą regułę przekształcania:

    | Typ przepływu | Atrybut TARGET | Element źródłowy | Zastosuj raz | Scal typu |
    | --- | --- | --- | --- | --- |
    | Bezpośrednie | PreferredDataLocation | PreferredDataLocation | Unchecked | Aktualizacja |

7. Zamknij **Dodaj** Tworzenie reguły ruchu wychodzącego.

![Utwórz regułę synchronizacji ruchu wychodzącego](./media/active-directory-aadconnectsync-change-the-configuration/preferredDataLocation-step5.png)

### <a name="step-6-run-full-synchronization-cycle"></a>Krok 6: Uruchom pełną synchronizację cyklu
Ogólnie rzecz biorąc, cyklu Pełna synchronizacja jest wymagana, ponieważ dodaliśmy nowe atrybuty do obu usług AD i schematu łącznika usługi Azure AD i reguły wprowadzone niestandardowe synchronizacji. Zalecane jest, aby zweryfikować zmiany przed wykonaniem operacji eksportowania ich do usługi Azure AD. Aby zweryfikować zmiany podczas uruchamiania ręcznie czynności, które tworzą cyklu pełnej synchronizacji, można użyć następujące kroki. 

1. Uruchom **pełny import** krok na **lokalnego łącznika AD**:

   1. Przejdź do **operacji** kartę Menedżera usługi synchronizacji.

   2. Kliknij prawym przyciskiem myszy **lokalnego łącznika AD** i wybierz **Uruchom...**

   3. W oknie podręcznym wybierz **pełny Import** i kliknij przycisk **OK**.
    
   4. Poczekaj na zakończenie operacji.

    > [!NOTE]
    > Pełny Import można pominąć na lokalnej łączniku usługi AD, jeśli atrybut źródłowy jest już na liście zaimportowane atrybutów. Innymi słowy, użytkownik nie miał do zmiany podczas [krok 2: Dodaj atrybut źródłowy do lokalnej schematu łącznika AD](#step-2-add-the-source-attribute-to-the-on-premises-ad-connector-schema).

2. Uruchom **pełny import** krok na **łącznika usługi Azure AD**:

   1. Kliknij prawym przyciskiem myszy **łącznika usługi Azure AD** i wybierz **Uruchom...**

   2. W oknie podręcznym wybierz **pełny Import** i kliknij przycisk **OK**.
   
   3. Poczekaj na zakończenie operacji.

3. Sprawdź zmian reguły synchronizacji do istniejącego obiektu użytkownika:

Atrybut źródłowy z lokalna Usługa Active Directory i PreferredDataLocation z usługi Azure AD zostały zaimportowane do odpowiednich miejsca łącznik. Przed wykonaniem kroku Pełna synchronizacja, zaleca się wykonanie **Podgląd** na istniejącego użytkownika obiektu w lokalnej przestrzeni łącznika usługi AD. Obiekt, przez Ciebie powinien mieć atrybut źródłowy wypełnione. Pomyślnie **Podgląd** z PreferredDataLocation w magazynie Metaverse jest dobry wskaźnik skonfigurowano synchronizacji zasady poprawnie. Aby uzyskać informacje na temat **Podgląd**, zapoznaj się z rozdziałem [zweryfikować zmianę](#verify-the-change).

4. Uruchom **pełną synchronizację** krok na **lokalnego łącznika AD**:

   1. Kliknij prawym przyciskiem myszy **lokalnego łącznika AD** i wybierz **Uruchom...**
  
   2. W oknie podręcznym wybierz **pełną synchronizację** i kliknij przycisk **OK**.
   
   3. Poczekaj na zakończenie operacji.

5. Sprawdź **oczekujące operacje eksportu** do usługi Azure AD:

   1. Kliknij prawym przyciskiem myszy **łącznika usługi Azure AD** i wybierz **przestrzeni łącznika wyszukiwania**.

   2. W oknie podręcznym przestrzeni łącznika wyszukiwania:

      1. Ustaw **zakres** do **oczekujących eksportu**.
      
      2. Sprawdź wszystkie trzy pola wyboru, w tym **dodawania, modyfikowania i usuwania**.
      
      3. Kliknij przycisk **wyszukiwania** przycisk, aby uzyskać listę obiektów ze zmianami do wyeksportowania. Aby sprawdzić zmiany dla danego obiektu, kliknij dwukrotnie obiekt.
      
      4. Sprawdź, czy zmiany są oczekiwane.

6. Uruchom **wyeksportować** krok na **łącznika usługi Azure AD**
      
   1. Kliknij prawym przyciskiem myszy **łącznika usługi Azure AD** i wybierz **Uruchom...**
   
   2. W oknie dialogowym wyskakujących uruchomić łącznik, wybierz **wyeksportować** i kliknij przycisk **OK**.
   
   3. Poczekaj na eksport do usługi Azure AD, aby zakończyć.

> [!NOTE]
> Można zauważyć, że kroki nie obejmują krok pełnej synchronizacji i eksportowania łącznika usługi Azure AD. Kroki nie są wymagane, ponieważ wartości atrybutów są przesyłane z lokalnej usługi Active Directory do usługi Azure AD tylko.

### <a name="step-7-re-enable-sync-scheduler"></a>Krok 7: Ponownie włączyć harmonogram synchronizacji
Ponowne włączenie harmonogramu synchronizacji wbudowany:

1. Uruchom sesję programu PowerShell.

2. Ponownie włączyć zaplanowanej synchronizacji, uruchamiając polecenie cmdlet:`Set-ADSyncScheduler -SyncCycleEnabled $true`



## <a name="next-steps"></a>Następne kroki
* Dowiedz się więcej o model konfiguracji w [Aprowizacją deklaratywną opis](active-directory-aadconnectsync-understanding-declarative-provisioning.md).
* Dowiedz się więcej o języku wyrażenia w [opis deklaratywne inicjowania obsługi administracyjnej wyrażenia](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md).

**Tematy poglądowe**

* [Synchronizacja programu Azure AD Connect: zrozumienie i dostosowywanie synchronizacji](active-directory-aadconnectsync-whatis.md)
* [Integrowanie tożsamości lokalnych z usługą Azure Active Directory](active-directory-aadconnect.md)
