---
title: "Synchronizacja programu Azure AD Connect: Skonfiguruj preferowany danych lokalizację dla możliwości wielu geograficznie w usłudze Office 365 | Dokumentacja firmy Microsoft"
description: "Opisuje sposób put zasobami użytkowników usługi Office 365 zbliża się użytkownika z synchronizacji Azure AD Connect."
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/31/2018
ms.author: billmath
ms.openlocfilehash: 021f009e66e57665a2252646b210f0e6dc55d33c
ms.sourcegitcommit: e19742f674fcce0fd1b732e70679e444c7dfa729
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/01/2018
---
# <a name="azure-ad-connect-sync-configure-preferred-data-location-for-office-365-resources"></a>Synchronizacja programu Azure AD Connect: Skonfiguruj preferowany danych lokalizację dla zasobów usługi Office 365
W tym temacie ma na celu przeprowadzi użytkownika przez proces konfigurowania PreferredDataLocation synchronizacji Azure AD Connect. Gdy klient korzysta z możliwości wielu geograficznie w usłudze Office 365, ten atrybut służy do wyznaczania lokalizacja geograficzna danych użytkowników usługi Office 365. Warunki **region** i **geograficznie** służą wymienne.

> [!IMPORTANT]
> Multi-geograficzna jest obecnie w przeglądzie. Jeśli chcesz dołączyć do programu Podgląd, skontaktuj się z przedstawicielem firmy Microsoft.
>
>

## <a name="enable-synchronization-of-preferreddatalocation"></a>Włącz synchronizację PreferredDataLocation
Domyślnie zasobów usługi Office 365 dla użytkowników znajdują się w tej samej lokalizacji geograficznej jako dzierżawy usługi Azure AD. Na przykład jeśli dzierżawy znajduje się w Ameryce Północnej następnie skrzynek pocztowych programu Exchange użytkowników znajdują się również w Ameryce Północnej. Międzynarodowej organizacji to może nie być optymalne. Przez ustawienie preferredDataLocation atrybut można zdefiniować geograficznie użytkownika.

Ustawienia tego atrybutu, możesz mieć użytkownika usługi Office 365 zasoby, takie jak skrzynek pocztowych i OneDrive, w tej samej lokalizacji geograficznej co użytkownik i jeszcze jeden dzierżawy dla całej organizacji.

> [!IMPORTANT]
> Na kwalifikować się do wielu geograficznie, musi mieć co najmniej 5000 miejsc, w ramach subskrypcji usługi Office 365
>
>

Lista wszystkich obszarach geograficznych dla usługi Office 365 można znaleźć w [danych znajdujących się w przypadku](https://aka.ms/datamaps).

Obszarach geograficznych w usłudze Office 365 dostępne dla wielu geograficznie są:

| Obszar geograficzny | wartość preferredDataLocation |
| --- | --- |
| Azja i Pacyfik | APC |
| Australia | AUS |
| Kanada | MOŻNA |
| Unii Europejskiej | EUR |
| Indie | ZNAJDŹ |
| Japonia | JPN |
| Korea Południowa | KOR |
| Wielka Brytania | GBR |
| Stany Zjednoczone | NAM |

* Jeśli obszar nie znajduje się w tej tabeli, na przykład Ameryka Południowa, następnie go nie może służyć do wielu Geo.
* Indie i Korea Południowa obszarach geograficznych są dostępne tylko w przypadku klientów z rozliczeń adresy i licencji zakupionych w tych obszarach geograficznych.
* Nie wszystkie usługi Office 365 obciążeń obsługuje ustawienia geo użytkownika.

Azure AD Connect obsługuje synchronizacji **PreferredDataLocation** atrybutu dla **użytkownika** obiektów w wersji 1.1.524.0 i po. W szczególności wprowadzono następujące zmiany:

* Schemat typu obiektu **użytkownika** w programie Azure AD Connector jest rozszerzona, aby uwzględnić atrybut PreferredDataLocation, który jest typu string pojedynczej wartości.
* Schemat typu obiektu **osoby** w magazynie Metaverse jest rozszerzona, aby uwzględnić atrybut PreferredDataLocation, który jest typu string i jest pojedynczej wartości.

Domyślnie atrybut PreferredDataLocation nie zostało włączone dla synchronizacji. Ta funkcja jest przeznaczony dla większych organizacji i nie wszyscy będzie korzystać z niego. Należy również określić atrybut do przechowywania geo usługi Office 365 dla użytkowników, ponieważ nie ma atrybutu PreferredDataLocation w lokalnej usłudze Active Directory. Ma to być inne dla każdej organizacji.

> [!IMPORTANT]
> Obecnie usługi Azure AD umożliwia atrybutu PreferredDataLocation do użytkownika obiektów synchronizowanych i chmury użytkownika obiekty bezpośrednio skonfigurowany przy użyciu programu Azure AD PowerShell. Po włączeniu synchronizacji atrybutów PreferredDataLocation, musisz zatrzymać przy użyciu programu Azure AD PowerShell do konfigurowania atrybutu na **synchronizowane obiekty użytkownika** jako Azure AD Connect zostaną one zastąpione na podstawie Źródło wartości atrybutów w lokalnej usłudze Active Directory.

> [!IMPORTANT]
> Od 1 września 2017 r, Azure AD nie umożliwia już atrybut PreferredDataLocation na **synchronizowane obiekty użytkownika** bezpośrednio skonfigurować przy użyciu programu Azure AD PowerShell. Aby skonfigurować atrybut PreferredLocation synchronizowane obiekty użytkownika, należy użyć usługi Azure AD Connect.

Przed włączeniem synchronizacji atrybutu PreferredDataLocation, należy:

* Najpierw zdecyduj, które lokalnymi atrybut usługi Active Directory mają być używane jako atrybut źródłowy. Powinien być typu **jednowartościowych ciąg**. W procedurze poniżej jednego extensionAttributes jest używany.
* Atrybut PreferredDataLocation zostały wcześniej skonfigurowane na istniejące synchronizowane obiekty użytkownika w usłudze Azure AD przy użyciu programu PowerShell usługi Azure AD, należy **Poprawka usterki systemu** wartości atrybutów do odpowiednich obiektów użytkowników lokalne usługi Active Directory.

    > [!IMPORTANT]
    > Jeśli jednak nie Poprawka usterki systemu wartości atrybutów, aby odpowiednie obiekty użytkownika w lokalnej usłudze Active Directory Azure AD Connect spowoduje usunięcie istniejącej wartości atrybutów w usłudze Azure AD po włączeniu synchronizacji dla atrybutu PreferredDataLocation.

* Zalecane jest skonfigurowanie atrybutu źródłowego na co najmniej kilka lokalnych użytkowników usługi AD obiekty, które mogą służyć do weryfikacji później.

Kroki umożliwiające włączenie synchronizacji atrybutu PreferredDataLocation można podsumować jako:

1. Wyłączanie harmonogramu synchronizacji i sprawdź, że nie ma synchronizacji w toku
2. Dodaj atrybut źródła schematu łącznika lokalnego DODAJE
3. Dodaj PreferredDataLocation do schematu łącznika usługi Azure AD
4. Utwórz regułę synchronizacji ruchu przychodzącego na przepływ wartości atrybutu z lokalnej usługi Active Directory
5. Utwórz regułę synchronizacji ruchu wychodzącego przepływu wartość atrybutu do usługi Azure AD
6. Uruchom pełną synchronizację cyklu
7. Włącz harmonogram synchronizacji
8. Sprawdź wynik

> [!NOTE]
> Pozostałej części tej sekcji omówiono następujące kroki w szczegółach. Zostały one opisane w kontekście wdrożenia usługi Azure AD z topologii z jednym lasem i bez reguł niestandardowych. Jeśli masz topologią wielu lasów, reguły synchronizacji niestandardowych skonfigurowane lub serwer przemieszczania, należy odpowiednio zmienić kroki.

## <a name="step-1-disable-sync-scheduler-and-verify-there-is-no-synchronization-in-progress"></a>Krok 1: Wyłącz harmonogramu synchronizacji i sprawdź, że nie ma synchronizacji w toku
Upewnij się, że synchronizacja nie ma miejsce, gdy trwa aktualizowanie reguły synchronizacji, aby uniknąć zmian niezamierzone eksportowane do usługi Azure AD. Aby wyłączyć harmonogram synchronizacji wbudowany:

1. Uruchom sesję programu PowerShell na serwerze programu Azure AD Connect.
2. Wyłącz zaplanowanej synchronizacji, uruchamiając polecenie cmdlet: `Set-ADSyncScheduler -SyncCycleEnabled $false`.
3. Uruchom **Menedżera usługi synchronizacji** , przechodząc do **START** > **usługi synchronizacji**.
4. Przejdź do **operacji** karcie i upewnij się, Brak operacji ze stanem *w toku*.

![Menedżera usługi synchronizacji — Sprawdź żadnych operacji w toku](./media/active-directory-aadconnectsync-feature-preferreddatalocation/preferreddatalocation-step1.png)

## <a name="step-2-add-the-source-attribute-to-the-on-premises-adds-connector-schema"></a>Krok 2: Dodawanie atrybutu źródłowego do schematu łącznika DODAJE lokalnej
Nie wszystkie atrybuty AD są importowane do lokalnej przestrzeni łącznika usługi AD. Jeśli wybrano opcję Użyj atrybutu nie jest domyślnie synchronizowane, następnie należy go zaimportować. Aby dodać atrybut źródłowy do listy importowanych atrybuty:

1. Przejdź do **łączniki** kartę Menedżera usługi synchronizacji.
2. Kliknij prawym przyciskiem myszy **lokalnego łącznika AD** i wybierz **właściwości**.
3. W wyskakującym oknie dialogowym, przejdź do **wybierz atrybuty** kartę.
4. Upewnij się, że zaznaczono atrybut źródłowy wybranego do użycia na liście atrybutów. Jeśli nie ma atrybut, kliknij pole wyboru "Pokaż wszystko".
5. Kliknij przycisk **OK** do zapisania.

![Dodaj atrybut źródłowy do lokalnego schematu w łączniku AD](./media/active-directory-aadconnectsync-feature-preferreddatalocation/preferreddatalocation-step2.png)

## <a name="step-3-add-preferreddatalocation-to-the-azure-ad-connector-schema"></a>Krok 3: Dodawanie PreferredDataLocation schematu łącznika usługi Azure AD
Domyślnie ten atrybut PreferredDataLocation nie został zaimportowany do przestrzeni łącznika usługi Azure AD. Aby dodać atrybut PreferredDataLocation na liście atrybutów zaimportowane:

1. Przejdź do **łączniki** kartę Menedżera usługi synchronizacji.
2. Kliknij prawym przyciskiem myszy **łącznika usługi Azure AD** i wybierz **właściwości**.
3. W wyskakującym oknie dialogowym, przejdź do **wybierz atrybuty** kartę.
4. Wybierz atrybut preferredDataLocation na liście atrybutów.
5. Kliknij przycisk **OK** do zapisania.

![Dodaj atrybut źródłowy do schematu łącznika usługi Azure AD](./media/active-directory-aadconnectsync-feature-preferreddatalocation/preferreddatalocation-step3.png)

## <a name="step-4-create-an-inbound-synchronization-rule-to-flow-the-attribute-value-from-on-premises-active-directory"></a>Krok 4: Utwórz regułę synchronizacji ruchu przychodzącego na przepływ wartości atrybutu z lokalnej usługi Active Directory
Reguła synchronizacji ruchu przychodzącego zezwala na wartość atrybutu mogą przepływać z atrybutu źródłowego z lokalnej usługi Active Directory do środowiska Metaverse:

1. Uruchom **Edytor reguł synchronizacji** , przechodząc do **START** > **Edytor reguł synchronizacji**.
2. Ustaw filtr wyszukiwania **kierunek** jako **przychodzący**.
3. Kliknij przycisk **Dodaj nową regułę** przycisk, aby utworzyć nową regułę ruchu przychodzącego.
4. W obszarze **opis** karcie, podaj następującej konfiguracji:

    | Atrybut | Wartość | Szczegóły |
    | --- | --- | --- |
    | Name (Nazwa) | *Podaj nazwę* | Na przykład *"w z usługi Active Directory — PreferredDataLocation użytkownika"* |
    | Opis | *Podaj opis, niestandardowe* |  |
    | System połączony | *Wybierz lokalną łączniku AD* |  |
    | Połączony System typu obiektu | **Użytkownika** |  |
    | Typ obiektu Metaverse | **Osoby** |  |
    | Typ łącza | **Dołącz** |  |
    | Pierwszeństwo | *Wybierz liczbę z zakresu od 1 – 99* | 1 – 99 jest zarezerwowana dla reguły synchronizacji niestandardowych. Odbiera wartość, która jest używana przez inną regułę synchronizacji. |

5. Zachowaj **filtru Scoping** puste, aby uwzględnić wszystkie obiekty. Może być konieczne dostosowanie zakresu filtru zgodnie z wdrożenia usługi Azure AD Connect.
6. Przejdź do **tab przekształcenia** i implementuje następującą regułę przekształcania:

    | Typ przepływu | Atrybut docelowy | Element źródłowy | Zastosuj raz | Scal typu |
    | --- | --- | --- | --- | --- |
    |Bezpośrednie | PreferredDataLocation | Wybierz atrybut źródłowy | Unchecked | Aktualizacja |

7. Kliknij przycisk **Dodaj** można utworzyć reguły dla ruchu przychodzącego.

![Utwórz regułę synchronizacji ruchu przychodzącego](./media/active-directory-aadconnectsync-feature-preferreddatalocation/preferreddatalocation-step4.png)

## <a name="step-5-create-an-outbound-synchronization-rule-to-flow-the-attribute-value-to-azure-ad"></a>Krok 5: Tworzenie reguły synchronizacji wychodzący przepływ wartość atrybutu do usługi Azure AD
Reguła synchronizacji ruchu wychodzącego zezwala na wartość atrybutu mogą przepływać z Metaverse z atrybutem PreferredDataLocation w usłudze Azure AD:

1. Przejdź do **reguły synchronizacji** edytora.
2. Ustaw filtr wyszukiwania **kierunek** jako **wychodzące**.
3. Kliknij przycisk **Dodaj nową regułę** przycisku.
4. W obszarze **opis** karcie, podaj następującej konfiguracji:

    | Atrybut | Wartość | Szczegóły |
    | ----- | ------ | --- |
    | Name (Nazwa) | *Podaj nazwę* | Na przykład "się do usługi AAD — użytkownik PreferredDataLocation" |
    | Opis | *Podaj opis* ||
    | System połączony | *Wybierz łącznik AAD* ||
    | Połączony System typu obiektu | Użytkownik ||
    | Typ obiektu Metaverse | **Osoby** ||
    | Typ łącza | **Dołącz** ||
    | Pierwszeństwo | *Wybierz liczbę z zakresu od 1 – 99* | 1 – 99 jest zarezerwowana dla reguły synchronizacji niestandardowych. Odbiera wartość, która jest używana przez inną regułę synchronizacji. |

5. Przejdź do **filtru Scoping** karta i Dodaj **pojedynczego grupę ustalania zakresu filtru z klauzulami dwóch**:

    | Atrybut | Operator | Wartość |
    | --- | --- | --- |
    | sourceObjectType | RÓWNOŚCI | Użytkownik |
    | cloudMastered | NOTEQUAL | True |

    Filtr zakresu określa obiekty usługi Azure AD, że ta reguła synchronizacji ruchu wychodzącego. W tym przykładzie używamy tego samego zakresu filtru z "Out do AD — tożsamości użytkownika" OOB reguły synchronizacji. Reguła synchronizacji uniemożliwia są stosowane do obiektów użytkowników, które nie są synchronizowane z lokalnej usługi Active Directory. Może być konieczne dostosowanie zakresu filtru zgodnie z wdrożenia usługi Azure AD Connect.

6. Przejdź do **przekształcania** karcie i implementuje następującą regułę przekształcania:

    | Typ przepływu | Atrybut docelowy | Element źródłowy | Zastosuj raz | Scal typu |
    | --- | --- | --- | --- | --- |
    | Bezpośrednie | PreferredDataLocation | PreferredDataLocation | Unchecked | Aktualizacja |

7. Zamknij **Dodaj** Tworzenie reguły ruchu wychodzącego.

![Utwórz regułę synchronizacji ruchu wychodzącego](./media/active-directory-aadconnectsync-feature-preferreddatalocation/preferreddatalocation-step5.png)

## <a name="step-6-run-full-synchronization-cycle"></a>Krok 6: Uruchom pełną synchronizację cyklu
Ogólnie rzecz biorąc, cyklu Pełna synchronizacja jest wymagana, ponieważ dodaliśmy nowe atrybuty do obu usług AD i schematu łącznika usługi Azure AD i reguły wprowadzone niestandardowe synchronizacji. Zalecane jest, aby zweryfikować zmiany przed wykonaniem operacji eksportowania ich do usługi Azure AD. Aby zweryfikować zmiany podczas uruchamiania ręcznie czynności, które tworzą cyklu pełnej synchronizacji, można użyć następujące kroki.

1. Uruchom **pełny import** krok na **lokalnego łącznika AD**:

   1. Przejdź do **operacji** kartę Menedżera usługi synchronizacji.
   2. Kliknij prawym przyciskiem myszy **lokalnego łącznika AD** i wybierz **Uruchom...** .
   3. W oknie podręcznym wybierz **pełny Import** i kliknij przycisk **OK**.
   4. Poczekaj na zakończenie operacji.

    > [!NOTE]
    > Pełny Import można pominąć na lokalnej łączniku usługi AD, jeśli atrybut źródłowy jest już na liście zaimportowane atrybutów. Innymi słowy, użytkownik nie miał do zmiany podczas [krok 2: Dodaj atrybut źródłowy do lokalnej schematu łącznika AD](#step-2-add-the-source-attribute-to-the-on-premises-adds-connector-schema).

2. Uruchom **pełny import** krok na **łącznika usługi Azure AD**:

   1. Kliknij prawym przyciskiem myszy **łącznika usługi Azure AD** i wybierz **Uruchom...**
   2. W oknie podręcznym wybierz **pełny Import** i kliknij przycisk **OK**.
   3. Poczekaj na zakończenie operacji.

3. Sprawdź zmian reguły synchronizacji do istniejącego obiektu użytkownika:

Atrybut źródłowy z lokalna Usługa Active Directory i PreferredDataLocation z usługi Azure AD zostały zaimportowane do przestrzeni łącznika odpowiednich. Przed wykonaniem kroku Pełna synchronizacja, zaleca się wykonanie **Podgląd** na istniejącego użytkownika obiektu w lokalnej przestrzeni łącznika usługi AD. Obiekt, przez Ciebie powinien mieć atrybut źródłowy wypełnione. Pomyślnie **Podgląd** z PreferredDataLocation w magazynie Metaverse jest dobry wskaźnik skonfigurowano synchronizacji zasady poprawnie. Aby uzyskać informacje na temat **Podgląd**, zapoznaj się z rozdziałem [zweryfikować zmianę](active-directory-aadconnectsync-change-the-configuration.md#verify-the-change).

4. Uruchom **pełną synchronizację** krok na **lokalnego łącznika AD**:

   1. Kliknij prawym przyciskiem myszy **lokalnego łącznika AD** i wybierz **Uruchom...** .
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

   1. Kliknij prawym przyciskiem myszy **łącznika usługi Azure AD** i wybierz **Uruchom...** .
   2. W oknie dialogowym wyskakujących uruchomić łącznik, wybierz **wyeksportować** i kliknij przycisk **OK**.
   3. Poczekaj na eksport do usługi Azure AD, aby zakończyć.

> [!NOTE]
> Można zauważyć, że kroki nie obejmują krok pełną synchronizację łącznika usługi Azure AD i eksportowanie na łączniku usługi AD. Kroki nie są wymagane, ponieważ wartości atrybutów są przesyłane z lokalnej usługi Active Directory do usługi Azure AD tylko.

## <a name="step-7-re-enable-sync-scheduler"></a>Krok 7: Ponownie włączyć harmonogram synchronizacji
Ponowne włączenie harmonogramu synchronizacji wbudowany:

1. Uruchom sesję programu PowerShell.
2. Ponownie włączyć zaplanowanej synchronizacji, uruchamiając polecenie cmdlet:`Set-ADSyncScheduler -SyncCycleEnabled $true`

## <a name="step-8-verify-the-result"></a>Krok 8: Sprawdzenie, czy wynik
Teraz nadszedł czas, aby sprawdzić konfigurację i włącz ją dla użytkowników.

1. Dodaj geo do wybranego atrybutu na koncie użytkownika. Lista dostępnych geograficznie znajdują się w [tej tabeli](#enable-synchronization-of-preferreddatalocation).  
![Atrybut AD dodane do użytkownika](./media/active-directory-aadconnectsync-feature-preferreddatalocation/preferreddatalocation-adattribute.png)
2. Poczekaj, aż ten atrybut do synchronizacji usługi Azure AD.
3. Za pomocą programu Exchange Online PowerShell, sprawdź, czy obszar skrzynki pocztowej został ustawiony prawidłowo.  
![Region skrzynki pocztowej Ustaw na koncie użytkownika w usłudze Exchange Online](./media/active-directory-aadconnectsync-feature-preferreddatalocation/preferreddatalocation-mailboxregion.png)  
Zakładając, że aby można było korzystać z tej funkcji został oznaczony dzierżawy, skrzynki pocztowej jest przenoszona do poprawne geo. Można to sprawdzić za spojrzenie na nazwę serwera, w którym znajduje się skrzynki pocztowej.
4. Aby sprawdzić, czy to ustawienie zostało skuteczne za pośrednictwem wielu skrzynek pocztowych, użyj skryptu w [galerii Technet](https://gallery.technet.microsoft.com/office/PowerShell-Script-to-a6bbfc2e). Ten skrypt ma również lista wszystkich prefiksy serwera centrów danych usługi Office 365 i który znajduje się on w geograficznie. Może służyć jako odwołanie w poprzednim kroku można zweryfikować lokalizacji skrzynki pocztowej.

## <a name="next-steps"></a>Kolejne kroki

**Dowiedz się więcej na temat Multi-geograficzną, w usłudze Office 365:**

* Geograficznie wielu sesji w Ignite: https://aka.ms/MultiGeoIgnite
* Multi geograficzną, w usłudze OneDrive: https://aka.ms/OneDriveMultiGeo
* Multi geograficzną, w programie SharePoint Online: https://aka.ms/SharePointMultiGeo

**Dowiedz się więcej na temat modelu konfiguracji w aparacie synchronizacji:**

* Dowiedz się więcej o model konfiguracji w [Aprowizacją deklaratywną opis](active-directory-aadconnectsync-understanding-declarative-provisioning.md).
* Dowiedz się więcej o języku wyrażenia w [opis deklaratywne inicjowania obsługi administracyjnej wyrażenia](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md).

**Tematy poglądowe**

* [Synchronizacja programu Azure AD Connect: zrozumienie i dostosowywanie synchronizacji](active-directory-aadconnectsync-whatis.md)
* [Integrowanie tożsamości lokalnych z usługą Azure Active Directory](active-directory-aadconnect.md)
