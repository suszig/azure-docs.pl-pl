---
title: "Samouczek: Konfigurowanie produktu Workday do obsługi użytkowników z usługą Azure Active Directory | Dokumentacja firmy Microsoft"
description: "Informacje o sposobie użycia produktu Workday jako źródło danych tożsamości dla usługi Active Directory i Azure Active Directory."
services: active-directory
author: asmalser-msft
documentationcenter: na
manager: mtillman
ms.assetid: 1a2c375a-1bb1-4a61-8115-5a69972c6ad6
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/26/2018
ms.author: asmalser
ms.openlocfilehash: 3a84a7ae7572145df8154ec5cbccf9f97e81866b
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/29/2018
---
# <a name="tutorial-configure-workday-for-automatic-user-provisioning"></a>Samouczek: Konfigurowanie produktu Workday do inicjowania obsługi użytkowników

Celem tego samouczka jest opisano czynności, które należy wykonać, aby zaimportować osób z produktu Workday do usługi Active Directory i Azure Active Directory, z opcjonalne zapisu pewnych atrybutów do produktu Workday. 



## <a name="overview"></a>Przegląd

[Użytkownika usługi Azure Active Directory świadczenie usługi](active-directory-saas-app-provisioning.md) integruje się z [produktu Workday kadr API](https://community.workday.com/sites/default/files/file-hosting/productionapi/Human_Resources/v21.1/Get_Workers.html) w celu obsługi administracyjnej kont użytkowników. Usługi Azure AD używa tego połączenia, aby umożliwić następujące przepływów pracy przypisywania użytkowników:

* **Inicjowanie obsługi użytkowników do usługi Active Directory** -synchronizowanie wybranego zestawu użytkowników z produktu Workday w co najmniej jeden lasów usługi Active Directory. 

* **Inicjowanie obsługi administracyjnej tylko na chmurze użytkowników do usługi Azure Active Directory** -hybrydowego użytkowników, którzy istnieje w usłudze Active Directory i Azure Active Directory można udostępnić w ostatnim przy użyciu [AAD Connect](connect/active-directory-aadconnect.md). Jednak użytkownicy, którzy są tylko w chmurze można udostępnić bezpośrednio z produktu Workday do usługi Azure Active Directory przy użyciu użytkownika usługi Azure AD, inicjowania obsługi usługi.

* **Zapisywanie zwrotne e-mail adresów do produktu Workday** -użytkownika usługi Azure AD, świadczenie usługi można zapisu wybrane atrybuty użytkownika usługi Azure AD do pracy, takie jak adres e-mail.

### <a name="scenarios-covered"></a>Omówione scenariusze

Dzień roboczy użytkownika inicjowania obsługi administracyjnej przepływów pracy obsługiwanych przez usługę inicjowania obsługi administracyjnej użytkowników usługi Azure AD automatyzować następujących zasobów ludzkich i scenariusze zarządzania cyklem życia tożsamości:

* **Nowi pracownicy wynajem** — po dodaniu nowych pracowników do produktu Workday, konto użytkownika zostanie automatycznie utworzone w usłudze Active Directory, usługi Azure Active Directory i opcjonalnie usługi Office 365 i [innych aplikacji SaaS obsługiwane przez usługę Azure AD](active-directory-saas-app-provisioning.md), z zapisu adres e-mail z produktu Workday.

* **Aktualizacje atrybutu i profilu pracownika** — po rekordu pracownika jest aktualizowany w pracy (takie jak ich nazwy, tytuł lub manager), ich konta użytkownika zostanie automatycznie zaktualizowana w usłudze Active Directory, usługi Azure Active Directory i opcjonalnie usługi Office 365 i [innych aplikacji SaaS obsługiwane przez usługę Azure AD](active-directory-saas-app-provisioning.md).

* **Liczba przerwanych pracownika** — gdy pracownik zostaje zakończone w pracy, ich konta użytkownika jest automatycznie wyłączana w usłudze Active Directory, usługi Azure Active Directory i opcjonalnie usługi Office 365 i [innych aplikacji SaaS obsługiwane przez usługę Azure AD](active-directory-saas-app-provisioning.md).

* **Pracownik ponownie wynajmuje** — gdy pracownik jest rehired w pracy, ich stare konto może być automatycznie ponownie uaktywnić lub ponownie zainicjowano obsługę administracyjną (w zależności od swoich preferencji) do usługi Active Directory, usługi Azure Active Directory i opcjonalnie usługi Office 365 i [innych aplikacji SaaS obsługiwane przez usługę Azure AD](active-directory-saas-app-provisioning.md).


## <a name="planning-your-solution"></a>Planowanie rozwiązania

Przed rozpoczęciem integracji z produktem Workday, sprawdź poniższe wymagania wstępne, a następnie przeczytaj następujące wskazówki na temat sposobu zgodny z bieżącym architektury usługi Active Directory i przypisywania wymagania z solution(s) udostępniane przez usługi Azure Active Directory użytkowników.

### <a name="prerequisites"></a>Wymagania wstępne

Scenariusz opisany w tym samouczku założono, że już następujące elementy:

* Prawidłową subskrypcję z uprawnieniami administratora globalnego usługi Azure AD Premium P1
* Dzierżawca implementacji produktu Workday do celów testowania i integracji
* Uprawnienia administratora w pracy można utworzyć użytkownika integracji systemu i wprowadzać zmiany do przetestowania danych pracowników do celów testowych
* Inicjowanie obsługi użytkowników do usługi Active Directory, aby uzyskać przyłączonych do domeny serwera z systemem usługi systemu Windows 2012 lub nowszego jest wymagany do hosta [agent synchronizacji lokalnej](https://go.microsoft.com/fwlink/?linkid=847801)
* [Azure AD Connect](connect/active-directory-aadconnect.md) synchronizacji od usługi Active Directory i Azure AD


### <a name="solution-architecture"></a>Architektura rozwiązania

Usługa Azure AD zapewnia bogaty zestaw inicjowania obsługi administracyjnej łączników, aby pomóc w określeniu, inicjowanie obsługi i tożsamości Zarządzanie cyklem życia z produktu Workday do usługi Active Directory, usługi Azure AD, aplikacji SaaS i późniejsze. Funkcje użytkownik użyje i sposób konfigurowania rozwiązania będą się różnić w zależności od środowiska i wymagań Twojej organizacji. Pierwszym krokiem wykonać zapasów, o ile poniżej są obecne i wdrożony w organizacji:

* Ile lasy usługi Active Directory są używane?
* Jak wiele domen usługi Active Directory są używane?
* Ile Active Directory jednostek organizacyjnych (OU) są używane?
* Jak wiele dzierżaw usługi Azure Active Directory są używane?
* Istnieją użytkownicy, którzy muszą być przygotowana do usługi Active Directory i Azure Active Directory (np. "hybrydowe" użytkowników)?
* Istnieją użytkownicy, którzy muszą być przygotowana do usługi Azure Active Directory, ale nie Active Directory (np. "tylko w chmurze" użytkowników)?
* Czy adresu e-mail użytkownika muszą być zapisywane z powrotem do produktu Workday?

Po uzyskaniu odpowiedzi na te pytania, możesz zaplanować pracy inicjowania obsługi wdrożenia, wykonując poniższe wskazówki.

#### <a name="using-provisioning-connector-apps"></a>Za pomocą aplikacji łącznika inicjowania obsługi administracyjnej

Azure Active Directory obsługuje wstępnie zintegrowanych łączników inicjowania obsługi administracyjnej dla produktu Workday i wiele innych aplikacji SaaS. 

Pojedynczy łącznik inicjowania obsługi administracyjnej interfejsy przy użyciu interfejsu API systemu jednego źródła i pomaga dane udostępniania system pojedynczym elementem docelowym. Większość inicjowania obsługi administracyjnej łączniki, które obsługuje usługę Azure AD dla pojedynczego systemu źródłowego i docelowego (np. Azure AD do usługi ServiceNow) i można skonfigurować przez dodanie aplikacji w galerii aplikacji usługi Azure AD (np. usługi ServiceNow). 

Jest relacją między inicjowania obsługi administracyjnej wystąpienia łącznika i wystąpień aplikacji w usłudze Azure AD:

| Systemu źródłowego | System docelowy |
| ---------- | ---------- | 
| Dzierżawy usługi Azure AD | Aplikacja SaaS |


Jednak podczas pracy z produktu Workday i usługi Active Directory, istnieje wiele systemów źródłowych i docelowych wziąć pod uwagę:

| Systemu źródłowego | System docelowy | Uwagi |
| ---------- | ---------- | ---------- |
| Dzień roboczy | Las usługi Active Directory | Każdy las jest traktowany jako system docelowy różne |
| Dzień roboczy | Dzierżawy usługi Azure AD | Co jest wymagane dla użytkowników tylko w chmurze |
| Las usługi Active Directory | Dzierżawy usługi Azure AD | Ten przepływ jest obecnie obsługiwany przez AAD Connect |
| Dzierżawy usługi Azure AD | Dzień roboczy | Dla zapisu zwrotnego adresów e-mail |

W celu ułatwienia tych wiele przepływów pracy w wielu systemach źródłowych i docelowych, usługa Azure AD zapewnia wielu inicjowania obsługi administracyjnej aplikacji łącznika, które można dodać z galerii aplikacji Azure AD:

![Galerii aplikacji usługi AAD](./media/active-directory-saas-workday-inbound-tutorial/WD_Gallery.PNG)

* **WORKDAY do udostępniania usługi Active Directory** -ta aplikacja ułatwia konta Inicjowanie obsługi użytkowników z produktu Workday do jednego lasu usługi Active Directory. Jeśli masz wiele lasów, możesz dodać jedno wystąpienie tej aplikacji z galerii aplikacji Azure AD dla każdego lasu usługi Active Directory, które należy udostępnić do.

* **WORKDAY do usługi Azure AD inicjowania obsługi administracyjnej** — gdy AAD Connect to narzędzie, które mają być używane do synchronizacji usługi Active Directory użytkowników do usługi Azure Active Directory, ta aplikacja może służyć do ułatwienia obsługi tylko w chmurze użytkowników z produktu Workday do pojedynczej dzierżawy usługi Azure Active Directory.

* **Zapisywanie zwrotne WORKDAY** -ta aplikacja umożliwia zapisywanie zwrotne adresu e-mail użytkownika z usługi Azure Active Directory do produktu Workday.

> [!TIP]
> Regularne aplikacji "Produktu Workday" służy do konfigurowania rejestracji jednokrotnej między produktu Workday i Azure Active Directory. 

Sposób instalowania i konfigurowania tych specjalnych inicjowania obsługi administracyjnej aplikacji łącznika jest przedmiotem pozostałej części tego samouczka. Aplikacje, które użytkownik chce skonfigurować zależy to dzierżawców systemach, w których należy udostępnić, ile lasy usługi Active Directory i Azure AD w środowisku.

![Przegląd](./media/active-directory-saas-workday-inbound-tutorial/WD_Overview.PNG)

## <a name="configure-a-system-integration-user-in-workday"></a>Skonfiguruj użytkownika integracji systemu w pracy
Typowe wymagania wszystkich łączników inicjowania obsługi administracyjnej produktu Workday jest wymagają podania poświadczeń dla konta integracji systemu produktu Workday do nawiązania połączenia interfejsu API kadr produktu Workday. Ta sekcja zawiera opis sposobu tworzenia konta integrator systemu w pracy.

> [!NOTE]
> Istnieje możliwość pominąć tę procedurę i zamiast tego użyć konta administratora globalnego produktu Workday jako konta integracji systemu. Może działać prawidłowo w przypadku pokazów, ale nie jest zalecane w przypadku wdrożeń produkcyjnych.

### <a name="create-an-integration-system-user"></a>Utwórz użytkownika integracji systemu

**Aby utworzyć użytkownika system integracji:**

1. Zaloguj się do konta administratora dzierżawy produktu Workday. W **Workbench produktu Workday**, wprowadź użytkownika w polu wyszukiwania, a następnie kliknij polecenie **Tworzenie użytkownika System integracji**. 
   
    ![Utwórz użytkownika](./media/active-directory-saas-workday-inbound-tutorial/IC750979.png "tworzenia użytkownika")
2. Zakończenie **Tworzenie użytkownika System integracji** zadanie, podając nazwę użytkownika i hasło dla nowego użytkownika System integracji.  
 * Pozostaw **wymagają nowe hasło przy następnej znak** opcja nie jest zaznaczona, ponieważ ten użytkownik będzie logować się programowo. 
 * Pozostaw **minut limitu czasu sesji** z jego wartość domyślna 0, która uniemożliwi sesji użytkownika przedwcześnie limit czasu. 
   
    ![Utwórz użytkownika System integracji](./media/active-directory-saas-workday-inbound-tutorial/IC750980.png "utworzyć integracji systemu użytkownika")

### <a name="create-a-security-group"></a>Utwórz grupę zabezpieczeń
Należy utworzyć grupę zabezpieczeń systemu nieograniczonego integracji i przypisz użytkownika do niego.

**Aby utworzyć grupę zabezpieczeń:**

1. Wprowadź Utwórz grupę zabezpieczeń w polu wyszukiwania, a następnie kliknij przycisk **Utwórz grupę zabezpieczeń**. 
   
    ![Grupa CreateSecurity](./media/active-directory-saas-workday-inbound-tutorial/IC750981.png "CreateSecurity grupy")
2. Zakończenie **Utwórz grupę zabezpieczeń** zadań.  
3. Wybierz grupy zabezpieczeń systemu integracji — nieograniczonego z **typ grupy zabezpieczeń dzierżawcza** listy rozwijanej.
4. Utwórz grupę zabezpieczeń, do których członkowie zostaną jawnie dodani. 
   
    ![Grupa CreateSecurity](./media/active-directory-saas-workday-inbound-tutorial/IC750982.png "CreateSecurity grupy")

### <a name="assign-the-integration-system-user-to-the-security-group"></a>Przypisz użytkownika do integracji systemu do grupy zabezpieczeń

**Aby przypisać użytkownika systemu integracji:**

1. Edytuj grupę zabezpieczeń należy wprowadzić w polu wyszukiwania, a następnie kliknij przycisk **Edytuj grupę zabezpieczeń**. 
   
    ![Edytuj grupę zabezpieczeń](./media/active-directory-saas-workday-inbound-tutorial/IC750983.png "Edytuj grupę zabezpieczeń")
2. Wyszukaj i wybierz nową grupę zabezpieczeń integracji według nazwy. 
   
    ![Edytuj grupę zabezpieczeń](./media/active-directory-saas-workday-inbound-tutorial/IC750984.png "Edytuj grupę zabezpieczeń")
3. Dodaj nowego użytkownika system integracji do nowej grupy zabezpieczeń. 
   
    ![Grupa zabezpieczeń systemu](./media/active-directory-saas-workday-inbound-tutorial/IC750985.png "grupa zabezpieczeń systemu")  

### <a name="configure-security-group-options"></a>Skonfiguruj opcje grupy zabezpieczeń
W tym kroku udzielić nowe uprawnienia grupy zabezpieczeń dla **uzyskać** i **Put** operacje na obiektach zabezpieczone przez następujące zasady zabezpieczeń domeny:

* Inicjowanie obsługi konta zewnętrznego
* Danych procesu roboczego: Raporty publicznego procesu roboczego
* Danych procesu roboczego: Wszystkie pozycje
* Danych procesu roboczego: Personel informacje o bieżącej
* Danych procesu roboczego: Tytuł firm proces roboczy w profilu

**Aby skonfigurować opcje grupy zabezpieczeń:**

1. W polu wyszukiwania wprowadź zasady zabezpieczeń domeny, a następnie kliknij łącze **zasady zabezpieczeń domeny dla Obszar funkcjonalny**.  
   
    ![Zasady zabezpieczeń domeny](./media/active-directory-saas-workday-inbound-tutorial/IC750986.png "zasady zabezpieczeń domeny")  
2. Wyszukaj systemu i wybierz **systemu** Obszar funkcjonalny.  Kliknij przycisk **OK**.  
   
    ![Zasady zabezpieczeń domeny](./media/active-directory-saas-workday-inbound-tutorial/IC750987.png "zasady zabezpieczeń domeny")  
3. Na liście zasad zabezpieczeń dla Obszar funkcjonalny systemu, rozwiń węzeł **administrowania zabezpieczeniami** i wybierz zasady zabezpieczeń domeny **zewnętrznych Inicjowanie obsługi konta**.  
   
    ![Zasady zabezpieczeń domeny](./media/active-directory-saas-workday-inbound-tutorial/IC750988.png "zasady zabezpieczeń domeny")  
4. Kliknij przycisk **Edytuj uprawnienia**, a następnie na **Edytuj uprawnienia** strony okna dialogowego Dodaj nową grupę zabezpieczeń do listy grup zabezpieczeń z **uzyskać** i **Put**  uprawnienia integracji. 
   
    ![Edytuj uprawnienia](./media/active-directory-saas-workday-inbound-tutorial/IC750989.png "uprawnienia do edycji")  
5. Powtórz krok 1 powyżej, aby powrócić do ekranu wyboru obszarów funkcjonalnych, a następnie wybierz tego czasu, wyszukaj personel, **personel Obszar funkcjonalny** i kliknij przycisk **OK**.
   
    ![Zasady zabezpieczeń domeny](./media/active-directory-saas-workday-inbound-tutorial/IC750990.png "zasady zabezpieczeń domeny")  
6. Na liście zasad zabezpieczeń dla Obszar funkcjonalny Staffing, rozwiń węzeł **danych procesu roboczego: Staffing** i powtórz kroki 4 powyżej dla każdej z tych pozostałych zasady zabezpieczeń:

   * Danych procesu roboczego: Raporty publicznego procesu roboczego
   * Danych procesu roboczego: Wszystkie pozycje
   * Danych procesu roboczego: Personel informacje o bieżącej
   * Danych procesu roboczego: Tytuł firm proces roboczy w profilu
   
7. Powtórz krok 1, powyżej, aby powrócić do ekranu wyboru funkcjonalności obszarów i tym razem wyszukiwać **informacje kontaktowe**, wybierz obszar funkcjonalny Staffing i kliknij przycisk **OK**.

8.  Na liście zasad zabezpieczeń dla Obszar funkcjonalny Staffing, rozwiń węzeł **danych procesu roboczego: informacje kontaktowe pracy**i powtórz kroki 4 powyżej dla zasad zabezpieczeń poniżej:

    * Dane procesu roboczego: Służbowy adres E-mail

    ![Zasady zabezpieczeń domeny](./media/active-directory-saas-workday-inbound-tutorial/IC750991.png "zasady zabezpieczeń domeny")  
    
### <a name="activate-security-policy-changes"></a>Aktywuj zmian zasad zabezpieczeń

**Aby aktywować zmian zasad zabezpieczeń:**

1. Wprowadź aktywować w polu wyszukiwania, a następnie kliknij łącze **aktywować oczekujących zmian zasad zabezpieczeń**. 
   
    ![Aktywuj](./media/active-directory-saas-workday-inbound-tutorial/IC750992.png "aktywacji") 
2. Rozpoczęcia zadania aktywować oczekujących zmian zasad zabezpieczeń, wprowadzając komentarz dla inspekcji, a następnie kliknij przycisk **OK**. 
   
    ![Aktywuj oczekujących zabezpieczeń](./media/active-directory-saas-workday-inbound-tutorial/IC750993.png "aktywować oczekujących zabezpieczeń")   
3. Ukończenie zadania na następnym ekranie, zaznaczając pole wyboru **Potwierdź**, a następnie kliknij przycisk **OK**. 
   
    ![Aktywuj oczekujących zabezpieczeń](./media/active-directory-saas-workday-inbound-tutorial/IC750994.png "aktywować oczekujących zabezpieczeń")  

## <a name="configuring-user-provisioning-from-workday-to-active-directory"></a>Konfigurowanie Inicjowanie obsługi użytkowników z produktu Workday do usługi Active Directory
Wykonaj te instrukcje, aby skonfigurować konto użytkownika, inicjowania obsługi administracyjnej z produktu Workday w każdym lesie usługi Active Directory, wymagające inicjowania obsługi administracyjnej.

### <a name="part-1-adding-the-provisioning-connector-app-and-creating-the-connection-to-workday"></a>Część 1: Dodawanie inicjowania obsługi administracyjnej aplikacja łącznika i tworzenie połączenia z produktu Workday

**Aby skonfigurować produktu Workday do inicjowania obsługi usługi Active Directory:**

1.  Przejdź do <https://portal.azure.com>

2.  Na pasku nawigacyjnym po lewej stronie wybierz **usługi Azure Active Directory**

3.  Wybierz **aplikacje dla przedsiębiorstw**, następnie **wszystkie aplikacje**.

4.  Wybierz **dodać aplikację**i wybierz **wszystkie** kategorii.

5.  Wyszukaj **udostępniania produktu Workday do usługi Active Directory**i Dodaj danej aplikacji w galerii.

6.  Po dodaniu aplikacji i ekran szczegółów aplikacji jest wyświetlane, wybierz pozycję **inicjowania obsługi administracyjnej**

7.  Zmień **inicjowania obsługi administracyjnej** **tryb** do **automatyczne**

8.  Zakończenie **poświadczeń administratora** sekcji w następujący sposób:

   * **Nazwa użytkownika administratora** — wprowadź nazwę użytkownika konta systemu integracji produktu Workday, z dołączoną nazwą domeny dzierżawy. **Powinien wyglądać mniej więcej tak:username@contoso4**

   * **Hasło administratora —** wprowadź hasło do konta produktu Workday integracji systemu

   * **Adres URL — dzierżawy** wprowadź adres URL punktu końcowego usługi sieci web produktu Workday dla dzierżawy. To powinien wyglądać następująco: https://wd3-impl-services1.workday.com/ccx/service/contoso4, gdzie contoso4 jest zastępowany nazwa dzierżawy poprawne, a wd3 impl jest zastępowany ciągiem poprawne środowisko.

   * **Lasu usługi Active Directory -** "Name" usługi Active Directory lasu, zwracane przez polecenia powershell Get-ADForest. Zazwyczaj jest to ciąg, takich jak: *contoso.com*

   * **Kontener usługi Active Directory -** wprowadź ciąg kontenera, który zawiera wszystkich użytkowników w lesie usługi AD. Przykład: *OU = użytkowników standardowych, OU = użytkownicy, DC = contoso, DC = test*

   * **Wiadomość E-mail z powiadomieniem —** wprowadź swój adres e-mail, a następnie zaznacz pole wyboru "Wyślij wiadomość e-mail, jeśli wystąpi błąd".

   * Kliknij przycisk **Testuj połączenie** przycisku. Jeśli test połączenia powiedzie się, kliknij przycisk **zapisać** na górze. Jeśli nie powiedzie się, sprawdź, czy poświadczenia produktu Workday są nieprawidłowe w pracy. 

![Azure Portal](./media/active-directory-saas-workday-inbound-tutorial/WD_1.PNG)

### <a name="part-2-configure-attribute-mappings"></a>Część 2: Konfigurowanie mapowań atrybutów 

W tej sekcji skonfigurujesz, jak dane użytkownika wypływających z produktu Workday do usługi Active Directory.

1.  Na karcie inicjowania obsługi administracyjnej w obszarze **mapowania**, kliknij przycisk **zsynchronizować pracowników z produktu Workday do OnPremises**.

2.  W **zakres obiektu źródłowego** pole, można wybrać, które zestawów użytkowników w produktu Workday musi należeć do zakresu na potrzeby inicjowania obsługi administracyjnej AD, definiując zestaw opartych na atrybutach filtrów. Domyślny zakres to "wszyscy użytkownicy w pracy". Przykładowe filtry:

   * Przykład: Zakres dla użytkowników z procesu roboczego identyfikatorów 1000000 i 2000000

      * Atrybut: WorkerID

      * Operator: Dopasowanie wyrażenia REGULARNEGO

      * Wartość: (1[0-9][0-9][0-9][0-9][0-9][0-9])

   * Przykład: Tylko pracownicy i nie pracowników warunkowe 

      * Atrybut: identyfikator pracownika

      * Operator: Nie ma wartości NULL

3.  W **akcji obiektów docelowych** pole globalnie filtrowania, jakie akcje mogą być wykonywane w usłudze Active Directory. **Utwórz** i **aktualizacji** są najczęściej.

4.  W **mapowania atrybutów** sekcji, można zdefiniować poszczególne produktu Workday atrybuty mapy do atrybutów usługi Active Directory.

5. Kliknij na istniejące mapowanie atrybutu go zaktualizować lub **Dodaj nowe mapowanie** u dołu ekranu, aby dodawać nowe mapowania. Mapowanie atrybutu poszczególnych obsługuje następujące właściwości:

      * **Typ mapowania**

         * **Bezpośrednie** — zapisuje wartość atrybutu produktu Workday w atrybucie AD bez zmian

         * **Stała** -zapisać wartości ciągu statycznych, stałych do atrybutu AD

         * **Wyrażenie** — umożliwia pisanie niestandardowej wartości atrybutu AD na podstawie co najmniej jeden dzień roboczy atrybutów. [Aby uzyskać więcej informacji, zobacz ten artykuł wyrażeń](active-directory-saas-writing-expressions-for-attribute-mappings.md).

      * **Atrybut źródłowy** — atrybut użytkownika z produktu Workday.

      * **Wartość domyślna** — jest to opcjonalne. Jeśli atrybut źródłowy ma wartość pustą, mapowanie zapisu wtedy tę wartość.
            Najbardziej typowych konfiguracji jest to pole pozostanie puste.

      * **Atrybut TARGET** — atrybut użytkownika w usłudze Active Directory.

      * **Zgodne obiektów przy użyciu tego atrybutu** — czy to mapowanie powinien być używany do jednoznacznego identyfikowania użytkowników między produktu Workday i usługi Active Directory. Jest to zazwyczaj ustawiona w polu Identyfikator procesu roboczego dla produktu Workday, która zwykle jest mapowana na jeden z atrybutów identyfikator pracownika w usłudze Active Directory.

      * **Dopasowywanie pierwszeństwo** — wiele pasujących atrybuty można ustawić. Jeśli dostępnych jest wiele, są oceniane pod w kolejności wynika z tego pola. Jak dopasowania zostanie znaleziony, żadne dodatkowe dopasowania atrybuty są oceniane.

      * **Zastosuj to mapowanie**
       
         * **Zawsze** — Zastosuj to mapowanie na obu Tworzenie użytkownika i aktualizowanie akcji

         * **Tylko podczas tworzenia** -Zastosuj to mapowanie tylko akcje creation użytkownika

6. Aby zapisać mapowania, kliknij przycisk **zapisać** w górnej części sekcji atrybutu mapowania.

![Azure Portal](./media/active-directory-saas-workday-inbound-tutorial/WD_2.PNG)

**Poniżej przedstawiono przykładowe Mapowanie atrybutów między produktu Workday i usługi Active Directory, z niektórych typowych wyrażeń**

-   Wyrażenie, które mapuje parentDistinguishedName atrybutu AD można go użyć do udostępnienia użytkownika do określonej jednostki Organizacyjnej, na podstawie jednego lub więcej atrybutów źródła produktu Workday. W tym przykładzie umieszcza użytkowników w różnych jednostkach organizacyjnych w zależności od ich danych Miasto w pracy.

-   Wyrażenie, który jest mapowany na atrybut userPrincipalName AD utworzyć nazwę UPN firstName.LastName@contoso.com. Zastępuje ona niedozwolone znaki specjalne.

-   [Brak dokumentację dotyczącą tutaj wyrażeń.](active-directory-saas-writing-expressions-for-attribute-mappings.md)

  
| ATRYBUT WORKDAY | ATRYBUT USŁUGI ACTIVE DIRECTORY |  ZGODNYM IDENTYFIKATOREM? | UTWÓRZ / ZAKTUALIZUJ |
| ---------- | ---------- | ---------- | ---------- |
|  **WorkerID**  |  Identyfikator pracownika | **Tak** | Napisane przy tworzeniu tylko | 
|  **Jednostki administracyjnej**   |   l   |     | Tworzenie i aktualizowanie |
|  **Firmy**         | Firmy   |     |  Tworzenie i aktualizowanie |
|  **CountryReferenceTwoLetter**      |   co |     |   Tworzenie i aktualizowanie |
| **CountryReferenceTwoLetter**    |  c  |     |         Tworzenie i aktualizowanie |
| **SupervisoryOrganization**  | Dział  |     |  Tworzenie i aktualizowanie |
|  **PreferredNameData**  |  Nazwa wyświetlana |     |   Tworzenie i aktualizowanie |
| **Identyfikator pracownika**    |  cn    |   |   Napisane przy tworzeniu tylko |
| **Faksów**      | facsimileTelephoneNumber     |     |    Tworzenie i aktualizowanie |
| **Imię**   | givenName       |     |    Tworzenie i aktualizowanie |
| **Przełącznik (\[Active\],, "0", "True", "1")** |  AccountDisabled      |     | Tworzenie i aktualizowanie |
| **Mobile**  |    Telefon komórkowy       |     |       Tworzenie i aktualizowanie |
| **EmailAddress**    | Poczty    |     |     Tworzenie i aktualizowanie |
| **ManagerReference**   | manager  |     |  Tworzenie i aktualizowanie |
| **WorkSpaceReference** | physicalDeliveryOfficeName    |     |  Tworzenie i aktualizowanie |
| **PostalCode**  |   postalCode  |     | Tworzenie i aktualizowanie |
| **LocalReference** |  preferredLanguage  |     |  Tworzenie i aktualizowanie |
| **Zastąp (Mid (Zastąp (\[identyfikator pracownika\],, "(\[ \\ \\ / \\ \\ \\ \\ \\ \\\[\\\\\]\\\\:\\\\;\\ \\|\\\\=\\\\,\\\\+\\\\\*\\ \\? \\ \\ &lt; \\ \\ &gt; \]) "," ",), 1, 20)," ([\\\\.) \* \$] (file:///\\.) *$)", , "", , )**      |    sAMAccountName            |     |         Napisane przy tworzeniu tylko |
| **Nazwisko**   |   SN   |     |  Tworzenie i aktualizowanie |
| **CountryRegionReference** |  st     |     | Tworzenie i aktualizowanie |
| **AddressLineData**    |  streetAddress  |     |   Tworzenie i aktualizowanie |
| **PrimaryWorkTelephone**  |  TelephoneNumber   |     | Tworzenie i aktualizowanie |
| **BusinessTitle**   |  tytuł     |     |  Tworzenie i aktualizowanie |
| **Join("@",Replace(Replace(Replace(Replace(Replace(Replace(Replace( Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace( Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace(Join(".", [FirstName], [LastName]), , "([Øø])", , "oe", , ), , "[Ææ]", , "ae", , ), , "([äãàâãåáąÄÃÀÂÃÅÁĄA])", , "a", , ), , "([B])", , "b", , ), , "([CçčćÇČĆ])", , "c", , ), , "([ďĎD])", , "d", , ), , "([ëèéêęěËÈÉÊĘĚE])", , "e", , ), , "([F])", , "f", , ), , "([G])", , "g", , ), , "([H])", , "h", , ), , "([ïîìíÏÎÌÍI])", , "i", , ), , "([J])", , "j", , ), , "([K])", , "k", , ), , "([ľłŁĽL])", , "l", , ), , "([M])" ,, "m",), "([ñńňÑŃŇN])", "n",), "([öòőõôóÖÒŐÕÔÓO])", "o",), "[P]", "p",), "([Q])", "q",), "([řŘR])", "r",), "([ßšśŠŚS])", "s",), "([TŤť])", "t",), "([üùûúůűÜÙÛÚŮŰU])", "u",), "([V])", "v",), "([W])", "w",), "([ýÿýŸÝY])", "y",), "([źžżŹŽŻZ])", "z",), "",,, "",), "contoso.com")**   | userPrincipalName     |     | Tworzenie i aktualizowanie                                                   
| **Przełącznika (\[jednostki administracyjnej\], "jednostki Organizacyjnej użytkowników standardowych, OU = użytkowników, OU = domyślne, OU = lokalizacjach, DC = = contoso, DC = com", "Dallas", "jednostki Organizacyjnej użytkowników standardowych, OU = użytkowników, OU = Dallas, OU = lokalizacjach, DC = = contoso, DC = com", "Austin", "jednostki Organizacyjnej użytkowników standardowych, OU = użytkowników, OU = Austin, OU = lokalizacjach, DC = = contoso, DC = com", "Seattle", "jednostki Organizacyjnej użytkowników standardowych, OU = użytkowników, OU = Seattle, OU = lokalizacjach, DC = = contoso, DC = com", "Londyn", "jednostki Organizacyjnej = użytkownicy w wersji Standard Jednostki Organizacyjnej użytkowników, OU = Londyn, OU = = lokalizacjach, DC = contoso, DC = com ")**  | parentDistinguishedName     |     |  Tworzenie i aktualizowanie |
  
### <a name="part-3-configure-the-on-premises-synchronization-agent"></a>Część 3: Konfigurowanie agenta synchronizacji lokalnej

Aby udostępnić, aby lokalne usługi Active Directory, musi być zainstalowany agent na serwerze przyłączonym do domeny w desire lasu usługi Active Directory. Administrator domeny (lub administratorem przedsiębiorstwa) są wymagane poświadczenia, aby zakończyć procedurę.

**[Można pobrać tutaj agent synchronizacji lokalnej](https://go.microsoft.com/fwlink/?linkid=847801)**

Po zainstalowaniu agenta, uruchom poniższe polecenia programu Powershell do konfigurowania agenta w danym środowisku.

**Polecenie #1**

> CD C:\\pliki programów\\Agent synchronizacji usługi Microsoft Azure Active Directory\\modułów\\AADSyncAgent

> Import-module AADSyncAgent.psd1

**Polecenie #2**

> Add-ADSyncAgentActiveDirectoryConfiguration

* Wejście: "Nazwa katalogu", wprowadź nazwę lesie usługi Active Directory wprowadzoną w części \#2
* Wejście: Nazwa użytkownika i hasło dla lasu usługi Active Directory

**Polecenie #3**

> Add-ADSyncAgentAzureActiveDirectoryConfiguration

* Wejście: Nazwa użytkownika administratora globalnego i hasło dla dzierżawy usługi Azure AD

>[!IMPORTANT]
>Obecnie jest to znany problem z poświadczeniami administratora globalnego nie działa, jeśli używają one niestandardową domenę (przykład: admin@contoso.com). Jako obejście, Utwórz i korzystać z domeny onmicrosoft.com konta administratora globalnego (przykład: admin@contoso.onmicrosoft.com)


**Polecenie #4**

> Get-AdSyncAgentProvisioningTasks

* Akcja: Upewnij się, że dane są zwracane. To polecenie automatycznie odnajduje produktu Workday Inicjowanie obsługi aplikacji w dzierżawie usługi Azure AD. Przykładowe dane wyjściowe:

> Nazwa: Moje lesie usługi Active Directory
>
> Włączono: True
>
> DirectoryName: mydomain.contoso.com
>
> Przetwarzający: False
>
> Identifier    : WDAYdnAppDelta.c2ef8d247a61499ba8af0a29208fb853.4725aa7b-1103-41e6-8929-75a5471a5203

**Polecenie #5**

> Start-AdSyncAgentSynchronization -Automatic

**Polecenie #6**

> polecenie net stop aadsyncagent

**Polecenie #7**

> polecenie net start aadsyncagent

>[!TIP]
>Oprócz polecenia "net" w programie Powershell, usługi agenta synchronizacji można też uruchomić i zatrzymać przy użyciu **Services.msc**. Jeśli występują błędy podczas uruchamiania polecenia programu Powershell, upewnij się, że **Microsoft Azure AD inicjowania obsługi administracyjnej agenta Connect** działa w **Services.msc**.

![Usługi](./media/active-directory-saas-workday-inbound-tutorial/Services.png)  

**Dodatkowa konfiguracja dla klientów w Unii Europejskiej**

Jeśli dzierżawy usługi Azure Active Directory znajduje się w jednym z centrów danych Unii Europejskiej, należy wykonać dodatkowe czynności.

1. Otwórz **Services.msc** i Zatrzymaj **Microsoft Azure AD inicjowania obsługi administracyjnej agenta Connect** usługi.
2. Przejdź do folderu instalacji agenta (przykład: C:\Program Files\Microsoft Azure inicjowania obsługi administracyjnej agenta programu AD Connect).
3. Otwórz **SyncAgnt.exe.config** w edytorze tekstów.
4. Zastąp https://manage.hub.syncfabric.windowsazure.com/Management z **https://eu.manage.hub.syncfabric.windowsazure.com/Management**
5. Replace https://provision.hub.syncfabric.windowsazure.com/Provisioning with **https://eu.provision.hub.syncfabric.windowsazure.com/Provisioning**
6. Zapisz **SyncAgnt.exe.config** pliku.
7. Otwórz **Services.msc**i uruchom **Microsoft Azure AD inicjowania obsługi administracyjnej agenta Connect** usługi.

**Rozwiązywanie problemów z agentem**

[Dziennika zdarzeń systemu Windows](https://technet.microsoft.com/en-us/library/cc722404(v=ws.11).aspx) w systemie Windows Server komputerze hostującym agenta zawiera zdarzenia dla wszystkich operacji wykonywanych przez agenta. Aby wyświetlić te zdarzenia:
    
1. Otwórz **Eventvwr.msc**.
2. Wybierz **dzienniki systemu Windows > Aplikacja**.
3. Wyświetl wszystkie zdarzenia rejestrowane w lokalizacji source **AADSyncAgent**. 
4. Sprawdź błędy i ostrzeżenia.

W przypadku problemu uprawnień przy użyciu poświadczeń usługi Active Directory lub Azure Active Directory w poleceń programu Powershell zostanie wyświetlony błąd, taką jak: 
    
![Dzienniki zdarzeń](./media/active-directory-saas-workday-inbound-tutorial/Windows_Event_Logs.png) 


### <a name="part-4-start-the-service"></a>Część 4: Uruchom usługę programu
Po zakończeniu części 1-3, można uruchomić usługę inicjowania obsługi administracyjnej w portalu Azure.

1.  W **inicjowania obsługi administracyjnej** ustaw **stan inicjowania obsługi administracyjnej** do **na**.

2. Kliknij pozycję **Zapisz**.

3. Spowoduje to uruchomienie synchronizacji początkowej, jakie mogą mieć różną liczbą godzin, zależnie od liczby użytkowników znajdują się w pracy.

4. W każdej chwili sprawdzić **dzienniki inspekcji** kartę w portalu Azure, aby zobaczyć, jakie akcje wykonał inicjowania obsługi usługi. Dzienniki inspekcji wymieniono wszystkie zdarzenia synchronizacji poszczególnych wykonywane przez usługę inicjowania obsługi administracyjnej, takie jak użytkownicy są poza produktu Workday do odczytu i następnie później dodane lub zaktualizowane do usługi Active Directory. **[W podręczniku obsługi administracyjnej raportowania szczegółowe instrukcje na temat sposobu czytanie dzienników inspekcji](active-directory-saas-provisioning-reporting.md)**

5.  Sprawdź [dziennika zdarzeń systemu Windows](https://technet.microsoft.com/en-us/library/cc722404(v=ws.11).aspx) na komputerze z serwerem systemu Windows uruchomiony jest agent nowe błędy lub ostrzeżenia. Te zdarzenia są widoczne, uruchamiając **Eventvwr.msc** na serwerze i wybranie **dzienniki systemu Windows > Aplikacja**. Wszystkie wiadomości dotyczące inicjowania obsługi administracyjnej są rejestrowane w lokalizacji source **AADSyncAgent**. 
    

6. Jeden ukończone, będą zapisywane podsumowanie inspekcji **inicjowania obsługi administracyjnej** karcie, jak pokazano poniżej.

![Azure Portal](./media/active-directory-saas-workday-inbound-tutorial/WD_3.PNG)


## <a name="configuring-user-provisioning-to-azure-active-directory"></a>Konfigurowanie Inicjowanie obsługi użytkowników do usługi Azure Active Directory
Jak skonfigurować inicjowania obsługi administracyjnej do usługi Azure Active Directory zależy od wymagania inicjowania obsługi administracyjnej, zgodnie z opisem w poniższej tabeli.

| Scenariusz | Rozwiązanie |
| -------- | -------- |
| **Użytkownicy muszą być przygotowana do usługi Active Directory i Azure AD** | Użyj  **[AAD Connect](connect/active-directory-aadconnect.md)** |
| **Użytkownicy muszą być aprowizowana tylko do usługi Active Directory** | Użyj  **[AAD Connect](connect/active-directory-aadconnect.md)** |
| **Użytkownicy muszą być przygotowana do tylko usługi Azure AD (tylko w chmurze)** | Użyj **Workday do inicjowania obsługi usługi Azure Active Directory** aplikacji w galerii aplikacji |

Aby uzyskać instrukcje na temat konfigurowania usługi Azure AD Connect, zobacz [dokumentacji usługi Azure AD Connect](connect/active-directory-aadconnect.md).

W poniższych sekcjach opisano konfigurowanie połączenia między produktu Workday i Azure AD, aby udostępnić użytkownikom tylko w chmurze.

> [!IMPORTANT]
> Wykonaj poniższą procedurę tylko, jeśli użytkownicy tylko w chmurze, które muszą być przygotowana do usługi Azure AD i nie lokalnej usługi Active Directory.

### <a name="part-1-adding-the-azure-ad-provisioning-connector-app-and-creating-the-connection-to-workday"></a>Część 1: Dodawanie aplikacji łącznika inicjowania obsługi usługi Azure AD i tworzenie połączenia z produktu Workday

**Aby skonfigurować produktu Workday do usługi Azure Active Directory inicjowania obsługi administracyjnej dla użytkowników tylko w chmurze:**

1.  Przejdź do <https://portal.azure.com>.

2.  Na pasku nawigacyjnym po lewej stronie wybierz **usługi Azure Active Directory**

3.  Wybierz **aplikacje dla przedsiębiorstw**, następnie **wszystkie aplikacje**.

4.  Wybierz **dodać aplikację**, a następnie wybierz **wszystkie** kategorii.

5.  Wyszukaj **produktu Workday do inicjowania obsługi usługi Azure AD**i Dodaj danej aplikacji w galerii.

6.  Po dodaniu aplikacji i ekran szczegółów aplikacji jest wyświetlane, wybierz pozycję **inicjowania obsługi administracyjnej**

7.  Zmień **inicjowania obsługi administracyjnej** **tryb** do **automatyczne**

8.  Zakończenie **poświadczeń administratora** sekcji w następujący sposób:

   * **Nazwa użytkownika administratora** — wprowadź nazwę użytkownika konta systemu integracji produktu Workday, z dołączoną nazwą domeny dzierżawy. Powinien wyglądać mniej więcej tak:username@contoso4

   * **Hasło administratora —** wprowadź hasło do konta produktu Workday integracji systemu

   * **Adres URL — dzierżawy** wprowadź adres URL punktu końcowego usługi sieci web produktu Workday dla dzierżawy. To powinien wyglądać następująco: https://wd3-impl-services1.workday.com/ccx/service/contoso4, gdzie contoso4 jest zastępowany nazwa dzierżawy poprawne, a wd3 impl jest zastępowany ciągiem poprawne środowisko. Jeśli ten adres URL nie jest znany, proszę współpracować z produktu Workday integracji partnera lub pomocy technicznej przedstawicielem, aby określić prawidłowy adres URL do użycia.

   * **Wiadomość E-mail z powiadomieniem —** wprowadź swój adres e-mail, a następnie zaznacz pole wyboru "Wyślij wiadomość e-mail, jeśli wystąpi błąd".

   * Kliknij przycisk **Testuj połączenie** przycisku.

   * Jeśli test połączenia powiedzie się, kliknij przycisk **zapisać** na górze. W przypadku niepowodzenia dokładnie sprawdzić, czy produktu Workday adresu URL i poświadczenia są prawidłowe w pracy.


### <a name="part-2-configure-attribute-mappings"></a>Część 2: Konfigurowanie mapowań atrybutów 

W tej sekcji skonfigurujesz, jak dane użytkownika wypływających z produktu Workday do usługi Azure Active Directory dla użytkowników tylko w chmurze.

1.  Na karcie inicjowania obsługi administracyjnej w obszarze **mapowania**, kliknij przycisk **pracowników zsynchronizować z usługą Azure AD**.

2.   W **zakres obiektu źródłowego** pole, można wybrać, które zestawów użytkowników w pracy powinna być w zakresie obsługi do usługi Azure AD, definiując zestaw opartych na atrybutach filtrów. Domyślny zakres to "wszyscy użytkownicy w pracy". Przykładowe filtry:

   * Przykład: Zakres dla użytkowników z procesu roboczego identyfikatorów 1000000 i 2000000

      * Atrybut: WorkerID

      * Operator: Dopasowanie wyrażenia REGULARNEGO

      * Wartość: (1[0-9][0-9][0-9][0-9][0-9][0-9])

   * Przykład: Tylko warunkowe pracowników i pracowników nie regularne

      * Atrybut: ContingentID

      * Operator: Nie ma wartości NULL

3.  W **akcji obiektów docelowych** pole globalnie filtrowania, jakie akcje mogą wykonać na usługi Azure AD. **Utwórz** i **aktualizacji** są najczęściej.

4.  W **mapowania atrybutów** sekcji, można zdefiniować poszczególne produktu Workday atrybuty mapy do atrybutów usługi Active Directory.

5. Kliknij na istniejące mapowanie atrybutu go zaktualizować lub **Dodaj nowe mapowanie** u dołu ekranu, aby dodawać nowe mapowania. Mapowanie atrybutu poszczególnych obsługuje następujące właściwości:

   * **Typ mapowania**

      * **Bezpośrednie** — zapisuje wartość atrybutu produktu Workday w atrybucie AD bez zmian

      * **Stała** -zapisać wartości ciągu statycznych, stałych do atrybutu AD

      * **Wyrażenie** — umożliwia pisanie niestandardowej wartości atrybutu AD na podstawie co najmniej jeden dzień roboczy atrybutów. [Aby uzyskać więcej informacji, zobacz ten artykuł wyrażeń](active-directory-saas-writing-expressions-for-attribute-mappings.md).

   * **Atrybut źródłowy** — atrybut użytkownika z produktu Workday.

   * **Wartość domyślna** — jest to opcjonalne. Jeśli atrybut źródłowy ma wartość pustą, mapowanie zapisu wtedy tę wartość.
            Najbardziej typowych konfiguracji jest to pole pozostanie puste.

   * **Atrybut TARGET** — atrybut użytkownika w usłudze Azure AD.

   * **Zgodne obiektów przy użyciu tego atrybutu** — czy to mapowanie powinien być używany do jednoznacznego identyfikowania użytkowników między produktu Workday i Azure AD. Jest to zazwyczaj ustawiona w polu Identyfikator procesu roboczego dla produktu Workday, która zwykle jest mapowana na atrybut rozszerzenia lub atrybutu ID pracownika (nowy) w usłudze Azure AD.

   * **Dopasowywanie pierwszeństwo** — wiele pasujących atrybuty można ustawić. Jeśli dostępnych jest wiele, są oceniane pod w kolejności wynika z tego pola. Jak dopasowania zostanie znaleziony, żadne dodatkowe dopasowania atrybuty są oceniane.

   * **Zastosuj to mapowanie**

     * **Zawsze** — Zastosuj to mapowanie na obu Tworzenie użytkownika i aktualizowanie akcji

     * **Tylko podczas tworzenia** -Zastosuj to mapowanie tylko akcje creation użytkownika

6. Aby zapisać mapowania, kliknij przycisk **zapisać** w górnej części sekcji atrybutu mapowania.

### <a name="part-3-start-the-service"></a>Część 3: Uruchom usługę programu
Po zakończeniu części 1 i 2, można uruchomić usługę inicjowania obsługi administracyjnej.

1.  W **inicjowania obsługi administracyjnej** ustaw **stan inicjowania obsługi administracyjnej** do **na**.

2. Kliknij pozycję **Zapisz**.

3. Spowoduje to uruchomienie synchronizacji początkowej, jakie mogą mieć różną liczbą godzin, zależnie od liczby użytkowników znajdują się w pracy.

4. Synchronizacja poszczególnych zdarzeń można wyświetlać w **dzienników inspekcji** kartę. **[W podręczniku obsługi administracyjnej raportowania szczegółowe instrukcje na temat sposobu czytanie dzienników inspekcji](active-directory-saas-provisioning-reporting.md)**

5. Jeden ukończone, będą zapisywane podsumowanie inspekcji **inicjowania obsługi administracyjnej** karcie, jak pokazano poniżej.


## <a name="configuring-writeback-of-email-addresses-to-workday"></a>Konfigurowanie funkcji zapisywania zwrotnego adresów e-mail do pracy
Wykonaj te instrukcje, aby skonfigurować zapisywanie zwrotne adresu e-mail użytkownika z usługi Azure Active Directory do produktu Workday.

### <a name="part-1-adding-the-provisioning-connector-app-and-creating-the-connection-to-workday"></a>Część 1: Dodawanie inicjowania obsługi administracyjnej aplikacja łącznika i tworzenie połączenia z produktu Workday

**Aby skonfigurować produktu Workday do inicjowania obsługi usługi Active Directory:**

1.  Przejdź do <https://portal.azure.com>

2.  Na pasku nawigacyjnym po lewej stronie wybierz **usługi Azure Active Directory**

3.  Wybierz **aplikacje dla przedsiębiorstw**, następnie **wszystkie aplikacje**.

4.  Wybierz **dodać aplikację**, a następnie wybierz pozycję **wszystkie** kategorii.

5.  Wyszukaj **zapisywania zwrotnego produktu Workday**i Dodaj danej aplikacji w galerii.

6.  Po dodaniu aplikacji i ekran szczegółów aplikacji jest wyświetlane, wybierz pozycję **inicjowania obsługi administracyjnej**

7.  Zmień **inicjowania obsługi administracyjnej** **tryb** do **automatyczne**

8.  Zakończenie **poświadczeń administratora** sekcji w następujący sposób:

   * **Nazwa użytkownika administratora** — wprowadź nazwę użytkownika konta systemu integracji produktu Workday, z dołączoną nazwą domeny dzierżawy. Powinien wyglądać mniej więcej tak:username@contoso4

   * **Hasło administratora —** wprowadź hasło do konta produktu Workday integracji systemu

   * **Adres URL — dzierżawy** wprowadź adres URL punktu końcowego usługi sieci web produktu Workday dla dzierżawy. To powinien wyglądać następująco: https://wd3-impl-services1.workday.com/ccx/service/contoso4, gdzie contoso4 jest zastępowany nazwa dzierżawy poprawne, a wd3 impl jest zastępowany ciągiem poprawne środowisko (Jeśli to konieczne).

   * **Wiadomość E-mail z powiadomieniem —** wprowadź swój adres e-mail, a następnie zaznacz pole wyboru "Wyślij wiadomość e-mail, jeśli wystąpi błąd".

   * Kliknij przycisk **Testuj połączenie** przycisku. Jeśli test połączenia powiedzie się, kliknij przycisk **zapisać** na górze. W przypadku niepowodzenia dokładnie sprawdzić, czy produktu Workday adresu URL i poświadczenia są prawidłowe w pracy.


### <a name="part-2-configure-attribute-mappings"></a>Część 2: Konfigurowanie mapowań atrybutów 


W tej sekcji skonfigurujesz, jak dane użytkownika wypływających z produktu Workday do usługi Active Directory.

1.  Na karcie inicjowania obsługi administracyjnej w obszarze **mapowania**, kliknij przycisk **synchronizacji Azure AD użytkownikom produktu Workday**.

2.  W **zakres obiektu źródłowego** pole, można również filtrować które zestawów użytkowników w usłudze Azure Active Directory powinny mieć ich adresów e-mail zapisywane z powrotem do produktu Workday. Domyślny zakres to "wszyscy użytkownicy w usłudze Azure AD". 

3.  W **mapowania atrybutów** sekcji, można zdefiniować poszczególne produktu Workday atrybuty mapy do atrybutów usługi Active Directory. Brak mapowania dla adresu e-mail domyślnie. Jednak należy zaktualizować dopasowania Identyfikatora do dopasowania użytkowników w usłudze Azure AD z ich odpowiednie pozycje w pracy. Popularne odpowiadającej metody jest synchronizowane identyfikator pracownika do extensionAttribute1 15 lub identyfikator procesu roboczego produktu Workday w usłudze Azure AD, a następnie użyć tego atrybutu w usłudze Azure AD, aby dopasować użytkowników, po powrocie do produktu Workday.

4.  Aby zapisać mapowania, kliknij przycisk **zapisać** w górnej części sekcji atrybutu mapowania.

### <a name="part-3-start-the-service"></a>Część 3: Uruchom usługę programu
Po zakończeniu części 1 i 2, można uruchomić usługę inicjowania obsługi administracyjnej.

1.  W **inicjowania obsługi administracyjnej** ustaw **stan inicjowania obsługi administracyjnej** do **na**.

2. Kliknij pozycję **Zapisz**.

3. Spowoduje to uruchomienie synchronizacji początkowej, jakie mogą mieć różną liczbą godzin, zależnie od liczby użytkowników znajdują się w pracy.

4. Synchronizacja poszczególnych zdarzeń można wyświetlać w **dzienników inspekcji** kartę. **[W podręczniku obsługi administracyjnej raportowania szczegółowe instrukcje na temat sposobu czytanie dzienników inspekcji](active-directory-saas-provisioning-reporting.md)**

5. Jeden ukończone, będą zapisywane podsumowanie inspekcji **inicjowania obsługi administracyjnej** karcie, jak pokazano poniżej.

## <a name="known-issues"></a>Znane problemy

* Podczas uruchamiania **ADSyncAgentAzureActiveDirectoryConfiguration Dodaj** poleceń programu Powershell, obecnie jest to znany problem z poświadczeniami administratora globalnego nie działa, jeśli używają one niestandardową domenę (przykład: admin@contoso.com) . Jako obejście, tworzenia i używania konta administratora globalnego w usłudze Azure AD z domeny onmicrosoft.com (przykład: admin@contoso.onmicrosoft.com).

* Poprzedni problem z dziennikami inspekcji nie są widoczne w dzierżaw usługi Azure AD, znajduje się w Unii Europejskiej został rozwiązany. Jednak konfiguracji dodatkowych agenta jest wymagane dla dzierżaw usługi Azure AD w UE. Aby uzyskać więcej informacji, zobacz [część 3: Konfigurowanie agenta synchronizacji lokalnej](#Part 3: Configure the on-premises synchronization agent)

## <a name="additional-resources"></a>Zasoby dodatkowe
* [Samouczek: Konfigurowanie logowania jednokrotnego między produktu Workday i Azure Active Directory](active-directory-saas-workday-tutorial.md)
* [Lista samouczków dotyczących sposobów integracji aplikacji SaaS przy użyciu usługi Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory?](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a>Kolejne kroki

* [Dowiedz się, jak należy przejrzeć dzienniki i Uzyskaj raporty dotyczące inicjowania obsługi administracyjnej działania](https://docs.microsoft.com/azure/active-directory/active-directory-saas-provisioning-reporting)
