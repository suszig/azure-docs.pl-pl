---
title: "Administrowanie katalogiem usługi Azure AD | Microsoft Docs"
description: "Wyjaśnienia dotyczące pojęcia „dzierżawa usługi Azure AD” i zarządzania nią za pomocą usługi Azure Active Directory"
services: active-directory
documentationcenter: 
author: MarkusVi
writer: markvi
manager: femila
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/11/2017
ms.author: markvi
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 001ffc0f9c7465552392a9848ef1487a4c0eafce
ms.lasthandoff: 12/07/2016


---
# <a name="administer-your-azure-ad-directory"></a>Administrowanie katalogiem usługi Azure AD
## <a name="what-is-an-azure-ad-tenant"></a>Co to jest dzierżawa usługi Azure AD?
W przypadku fizycznego miejsca pracy słowo „dzierżawa” może oznaczać budynek zajmowany przez grupę lub firmę. Na przykład organizacja może posiadać biuro w budynku. Ten budynek może znajdować się przy jednej ulicy razem z budynkami kilku innych organizacji. Twoja organizacja będzie wtedy dzierżawiła ten budynek. Budynek jest zasobem organizacji, zapewnia bezpieczeństwo i możliwość bezpiecznego prowadzenia działalności. Jest także oddzielony od budynków innych firm przy ulicy. To zapewnia, że Twoja organizacja i jej zasoby są izolowane od innych organizacji.

W przypadku miejsca pracy w chmurze dzierżawę można zdefiniować jako klienta lub organizację, do której należy określone wystąpienie danej usługi w chmurze. Na platformie obsługi tożsamości udostępnianej w ramach platformy Microsoft Azure dzierżawa to po prostu dedykowane wystąpienie usługi Azure Active Directory (Azure AD), które otrzymuje i posiada organizacja po utworzeniu konta w usłudze w chmurze firmy Microsoft, takiej jak Azure lub Office 365.

Każdy katalog usługi Azure AD jest odrębny i oddzielony od innych katalogów usługi Azure AD. Podobnie jak budynek biurowy, który jest zabezpieczonym zasobem przeznaczonym tylko dla Twojej organizacji, katalog Azure AD został zaprojektowany jako zabezpieczony zasób do użytku tylko Twojej organizacji. Architektura usługi Azure AD chroni dane klientów i informacje o tożsamości przez zmieszaniem. Oznacza to, że użytkownicy i administratorzy jednego katalogu usługi Azure AD nie mogą przypadkowo ani złośliwie uzyskać dostępu do danych w innym katalogu.

![Zarządzanie usługą Azure Active Directory][1]

## <a name="how-can-i-get-an-azure-ad-directory"></a>Jak mogę uzyskać katalog usługi Azure AD?
Usługa Azure AD zapewnia podstawowe możliwości zarządzania katalogiem i tożsamościami, na których bazuje większość usług w chmurze firmy Microsoft, w tym:

* Azure
* Usługa Microsoft Office 365
* Usługa Microsoft Dynamics CRM Online
* Microsoft Intune

Otrzymasz katalog usługi Azure AD, gdy utworzysz konto w ramach dowolnej z tych usług w chmurze firmy Microsoft. W razie potrzeby możesz utworzyć dodatkowe katalogi. Na przykład możesz zachować pierwszy katalog jako katalog produkcyjny i utworzyć kolejny na potrzeby testowania lub wdrażania przejściowego.

> [!NOTE]
> Po utworzeniu konta w ramach pierwszej usługi zalecamy użycie tego samego konta administratora skojarzonego z Twoją organizacją podczas rejestrowania się w innych usługach w chmurze firmy Microsoft.
> 
> 

Przy pierwszym tworzeniu konta w ramach usługi w chmurze firmy Microsoft należy podać szczegóły dotyczące własnej organizacji oraz rejestracji nazwy jej domeny internetowej. Te informacje są następnie używane do utworzenia nowego wystąpienia katalogu usługi Azure AD dla Twojej organizacji. Ten sam katalog jest używany na potrzeby uwierzytelniania logowania przy subskrybowaniu wielu usług w chmurze firmy Microsoft.

Dodatkowe usługi w pełni wykorzystują wszelkie istniejące konta użytkownika, zasady, ustawienia lub integrację z lokalnym katalogiem, które zostały skonfigurowane w celu polepszenia współpracy między lokalną infrastrukturą do obsługi tożsamości organizacji a usługą Azure AD.

Na przykład jeśli pierwotnie utworzono konto w usłudze Microsoft Intune i wykonano kroki niezbędne do dalszej integracji lokalnej usługi Active Directory z katalogiem usługi Azure AD przez wdrożenie synchronizacji katalogów i/lub serwerów logowania jednokrotnego, można zarejestrować się w innej usłudze w chmurze firmy Microsoft, takiej jak Office 365, która także ma dostęp do tych samych korzyści integracji katalogu co używane teraz przez usługę Microsoft Intune.

Aby uzyskać więcej informacji na temat integracji lokalnego katalogu z usługą Azure AD, zobacz [Directory integration](active-directory-aadconnect.md) (Integracja katalogu).

### <a name="associate-an-azure-ad-directory-with-a-new-azure-subscription"></a>Kojarzenie katalogu usługi Azure AD z nową subskrypcją platformy Azure
Możesz skojarzyć nową subskrypcję platformy Azure z tym samym katalogiem, który uwierzytelnia logowanie dla istniejącej subskrypcji usługi Office 365 lub Microsoft Intune. Zaloguj się do Portalu zarządzania Azure za pomocą konta służbowego. Portal zarządzania zwróci komunikat informujący o tym, że nie może odnaleźć żadnych subskrypcji dla konta. Wybierz pozycję **Utwórz konto platformy Azure**, a Twoim katalogiem będzie można administrować za pomocą portalu. Aby uzyskać więcej informacji, zobacz [Zarządzanie katalogiem dla subskrypcji usługi Office 365 na platformie Azure](active-directory-how-subscriptions-associated-directory.md#manage-the-directory-for-your-office-365-subscription-in-azure).

Aby zobaczyć wideo przedstawiające często zadawane pytania dotyczące użycia usługi Azure AD, zobacz [Azure Active Directory - Common Sign-up, sign-in and usage questions](http://channel9.msdn.com/Series/Windows-Azure-Active-Directory/WAADCommonSignupsigninquestions) (Usługa Azure Active Directory — często zadawane pytania dotyczące tworzenia konta, logowania i użycia).

### <a name="create-an-azure-ad-directory-by-signing-up-for-a-microsoft-cloud-service-as-an-organization"></a>Tworzenie katalogu usługi Azure AD przez utworzenie konta w usłudze w chmurze firmy Microsoft jako organizacja
Jeśli jeszcze nie masz subskrypcji usługi w chmurze firmy Microsoft, użyj linków poniżej, aby utworzyć konto. Utworzenie konta w ramach pierwszej usługi spowoduje automatyczne utworzenie katalogu usługi Azure AD.

* [Microsoft Azure](https://account.windowsazure.com/organization)
* [Office 365](http://products.office.com/business/compare-office-365-for-business-plans/)
* [Microsoft Intune](https://account.manage.microsoft.com/Signup/MainSignUp.aspx?OfferId=40BE278A-DFD1-470a-9EF7-9F2596EA7FF9&ali=1)

### <a name="manage-an-azure-provisioned-default-directory"></a>Zarządzanie domyślnym katalogiem aprowizowanym przez platformę Azure
Obecnie katalog jest tworzony automatycznie podczas tworzenia konta na platformie Azure. Twoja subskrypcja jest z nim kojarzona. Jednak jeśli konto na platformie Azure zostało utworzone przed październikiem 2013 r., katalog nie został automatycznie utworzony. W takim przypadku platforma Azure mogła „uzupełnić” Twoje konto, aprowizując dla niego domyślny katalog. Twoja subskrypcja została następnie skojarzona z domyślnym katalogiem.

Uzupełnianie katalogów zostało wykonane w październiku 2013 r. w ramach ogólnej poprawy modelu zabezpieczeń platformy Azure. Katalog udostępnia funkcje obsługi tożsamości w organizacji dla wszystkich klientów platformy Azure oraz zapewnia, że wszystkie zasoby platformy Azure są dostępne w kontekście użytkownika w katalogu. Nie można używać platformy Azure bez katalogu. Aby to osiągnąć, każdy użytkownik, który utworzył konto przed 7 lipca 2013 r., ale nie miał katalogu, musiał go utworzyć. Jeśli katalog został już utworzony, Twoja subskrypcja została z nim skojarzona.

Używanie usługi Azure AD nie wiąże się z żadnymi kosztami. Katalog jest bezpłatnym zasobem. Istnieje dodatkowa warstwa Premium usługi Azure Active Directory, która jest licencjonowana osobno i udostępnia dodatkowe funkcje, takie jak znakowanie oznaczeniami firmy i samoobsługowe resetowanie hasła.

Aby zmienić nazwę wyświetlaną katalogu, kliknij katalog w portalu, a następnie polecenie **Konfiguruj**. Jak wyjaśniono dalej w tym temacie, możesz dodać nowy katalog lub usunąć katalog, który nie jest już potrzebny. Aby skojarzyć subskrypcję z innym katalogiem, kliknij rozszerzenie **Ustawienia** w obszarze nawigacji po lewej, a potem na dole strony **Subskrypcje** kliknij polecenie **Edytuj katalog**. Możesz także utworzyć niestandardową domenę przy użyciu nazwy DNS, która została zarejestrowana, zamiast domyślnej domeny *.onmicrosoft.com, która może być preferowana w przypadku usług takich jak SharePoint Online.

## <a name="how-can-i-manage-directory-data"></a>Jak mogę zarządzać danymi w katalogu
Jako administrator co najmniej jednej subskrypcji usługi w chmurze firmy Microsoft możesz użyć Portalu zarządzania Azure, portalu konta usługi Microsoft Intune lub centrum administracyjnego usługi Office 365 do zarządzania danymi katalogu Twoich organizacji. Możesz również pobrać i uruchomić polecenia cmdlet [modułu Microsoft Azure Active Directory dla Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx), które ułatwiają zarządzanie danymi zapisanymi w usłudze Azure AD.

Przy użyciu dowolnego z tych portali (lub poleceń cmdlet) możesz:

* Tworzyć konta użytkowników oraz grup i zarządzać nimi
* Zarządzać powiązanymi usługami w chmurze, które subskrybuje Twoja organizacja
* Skonfigurować lokalną integrację z usługą katalogową

Portal zarządzania Azure, centrum administracyjne usługi Office 365, portal konta usługi Microsoft Intune i polecenia cmdlet usługi Azure AD odczytują i zapisują w ramach pojedynczego, współużytkowanego wystąpienia usługi Azure AD, które jest skojarzone z katalogiem organizacji, jak pokazano na poniższej ilustracji. W ten sposób portale (lub polecenia cmdlet) działają jako interfejs frontonu, który odbiera i/lub modyfikuje dane katalogu.

![][2]

Te portale konta i skojarzone polecenia cmdlet programu PowerShell dla usługi Azure AD używane do zarządzania użytkownikami i grupami są oparte na platformie Azure AD.

Po wprowadzeniu zmiany przez użytkownika w danych organizacji za pomocą dowolnego z portali (lub polecenia cmdlet) podczas gdy jest on zalogowany w kontekście jednej z tych usług, zmiana będzie również widoczna w innych portalach po następnym logowaniu w kontekście tej usługi, ponieważ dane są dostępne dla usług w chmurze firmy Microsoft, które subskrybuje użytkownik.
Na przykład jeśli użyto centrum administracyjnego usługi Office 365, aby uniemożliwić użytkownikowi logowanie, ta akcja zablokuje także logowanie do wszystkich innych usług, które obecnie subskrybuje organizacja. Można zobaczyć, że to samo konto użytkownika jest zablokowane w kontekście portalu konta usługi Microsoft Intune.

## <a name="how-can-i-add-and-manage-multiple-directories"></a>Jak mogę dodać wiele katalogów i zarządzać nimi?
Możesz dodać katalog usługi Azure AD w Portalu zarządzania Azure. Wybierz rozszerzenie **Usługa Active Directory** po lewej stronie i kliknij polecenie **Dodaj**.

Możesz zarządzać każdym katalogiem jako w pełni niezależnym zasobem: każdy katalog jest równorzędny, obsługuje wszystkie funkcje i jest logicznie niezależny od innych katalogów zarządzanych przez Ciebie. Między katalogami nie ma relacji nadrzędny-podrzędny. Ta niezależność między katalogami obejmuje niezależność zasobów, niezależność administracyjną i niezależność synchronizacji.

* **Niezależność zasobów**. Jeśli utworzysz lub usuniesz zasób w jednym katalogu, nie ma to wpływu na żaden zasób w innym katalogu (z częściowym wyjątkiem dotyczącym zewnętrznych użytkowników opisanym poniżej). Jeśli używasz niestandardowej domeny „contoso.com” w jednym katalogu, nie można jej użyć w żadnym innym katalogu.
* **Niezależność administracyjna**.  Jeśli użytkownik nieadministracyjny katalogu „Contoso” utworzy katalog testowy „Test”, wtedy:
  
  * Narzędzia do synchronizacji katalogów w celu zsynchronizowania danych z pojedynczym lasem usługi AD.
  * Administratorzy katalogu „Contoso” nie mają bezpośrednich uprawnień administracyjnych do katalogu „Test”, chyba że administrator tego katalogu jawnie nada im odpowiednie uprawnienia. Administratorzy katalogu „Contoso” mogą kontrolować dostęp do katalogu „Test”, korzystając ze swoich możliwości kontrolowania konta użytkownika, za pomocą którego utworzono katalog „Test”.
    
    W przypadku zmiany (dodania lub usunięcia) roli administracyjnej dla użytkownika w jednym katalogu, zmiana ta nie wpłynie na żadną rolę administracyjną, którą użytkownik może mieć w innym katalogu.
* **Niezależność synchronizacji**. Każdą usługę Azure AD można skonfigurować niezależnie w celu synchronizowania danych z jednego wystąpienia:
  
  * Narzędzia do synchronizacji katalogów w celu zsynchronizowania danych z pojedynczym lasem usługi AD
  * Łącznika usługi Azure Active Directory dla programu Forefront Identity Manager w celu zsynchronizowania danych z co najmniej jednego lokalnego lasu i/lub źródeł danych niezwiązanych z usługą AD.

Zwróć uwagę, że w przeciwieństwie do innych zasobów platformy Azure katalogi nie są zasobami podrzędnymi subskrypcji platformy Azure. Więc jeśli anulujesz subskrypcję platformy Azure lub zezwolisz na jej wygaśnięcie, dane katalogu będą w dalszym ciągu dostępne przy użyciu programu PowerShell usługi Azure AD, interfejsu API Graph platformy Azure lub innych interfejsów, takich jak interfejs centrum administracyjnego usługi Office 365. Możesz także skojarzyć inną subskrypcję z katalogiem.

## <a name="how-can-i-delete-an-azure-ad-directory"></a>Jak mogę usunąć katalog usługi Azure AD?
Administrator globalny może usunąć katalog usługi Azure AD z portalu. Po usunięciu katalogu zostaną także usunięte wszystkie zasoby zawarte w katalogu. Dlatego przed usunięciem katalogu należy upewnić się, że nie będzie on potrzebny.

> [!NOTE]
> Jeśli użytkownik jest zalogowany za pomocą konta służbowego, nie może próbować usunąć własnego katalogu macierzystego. Na przykład jeśli użytkownik jest zalogowany za pomocą konta joe@contoso.onmicrosoft.com,, to nie może usunąć katalogu, którego domyślna domena to contoso.onmicrosoft.com.
> 
> 

### <a name="conditions-that-must-be-met-to-delete-an-azure-ad-directory"></a>Warunki, które muszą zostać spełnione, aby usunąć katalog usługi Azure AD
Usługa Azure AD wymaga spełnienia pewnych warunków przed usunięciem katalogu. Zmniejszają one ryzyko negatywnego wpływu, jaki usunięcie katalogu mogłoby mieć na użytkowników lub aplikacje, na przykład na możliwość zalogowania się do usługi Office 365 lub dostęp do zasobów platformy Azure. Na przykład jeśli katalog subskrypcji zostanie przypadkowo usunięty, użytkownicy nie będą mieli dostępu do zasobów platformy Azure dla tej subskrypcji.

Są sprawdzane następujące warunki:

* Jedynym użytkownikiem w usuwanym katalogu jest administrator globalny. Wszyscy inni użytkownicy muszą zostać usunięci przed usunięciem katalogu. Jeśli użytkownicy są synchronizowani z lokalnego źródła, synchronizacja będzie musiała zostać wyłączona, a użytkownicy usunięci z katalogu w chmurze za pomocą Portalu zarządzania lub modułu platformy Azure dla programu Windows PowerShell. Nie jest wymagane usunięcie grup ani kontaktów, takich jak kontakty dodane za pomocą centrum administracyjnego usługi Office 365.
* W katalogu nie może być aplikacji. Wszystkie aplikacje muszą zostać usunięte przed usunięciem katalogu.
* Z katalogiem nie mogą być skojarzone żadne z usług Microsoft Online Services, takie jak platforma Microsoft Azure, usługa Office 365 lub usługa Azure AD w warstwie Premium. Na przykład jeśli domyślny katalog został utworzony na platformie Azure, nie możesz usunąć tego katalogu, jeśli subskrypcja platformy Azure wciąż korzysta z niego na potrzeby uwierzytelniania. Nie można także usunąć katalogu, jeśli inny użytkownik skojarzył z nim subskrypcję. Aby skojarzyć subskrypcję z innym katalogiem, zaloguj się do Portalu zarządzania Azure, a następnie kliknij pozycję **Ustawienia** w obszarze nawigacji po lewej stronie. Następnie w dolnej części strony **Subskrypcje** kliknij polecenie **Edytuj katalog**. Aby uzyskać więcej informacji na temat subskrypcji platformy Azure, zobacz [Jak subskrypcje platformy Azure są kojarzone z usługą Azure AD](active-directory-how-subscriptions-associated-directory.md).

> [!NOTE]
> Jeśli użytkownik jest zalogowany za pomocą konta służbowego, nie może próbować usunąć własnego katalogu macierzystego. Na przykład jeśli użytkownik jest zalogowany za pomocą konta joe@contoso.onmicrosoft.com,, to nie może usunąć katalogu, którego domyślna domena to contoso.onmicrosoft.com.
> 
> 

* Żaden dostawca usługi Multi-Factor Authentication nie może być połączony z katalogiem.

## <a name="additional-resources"></a>Dodatkowe zasoby
* [Forum usługi Azure AD](https://social.msdn.microsoft.com/Forums/home?forum=WindowsAzureAD)
* [Forum usługi Azure Multi-Factor Authentication](https://social.msdn.microsoft.com/Forums/home?forum=windowsazureactiveauthentication)
* [Witryna Stackoverflow](http://stackoverflow.com/questions/tagged/azure)
* [Tworzenie konta na platformie Azure jako organizacja](sign-up-organization.md)
* [Zarządzanie usługą Azure AD za pomocą programu Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx)
* [Przypisywanie ról administratorów w usłudze Azure AD](active-directory-assign-admin-roles.md)

<!--Image references-->
[1]: ./media/active-directory-administer/aad_portals.png
[2]: ./media/active-directory-administer/azure_tenants.png

