<properties
    pageTitle="Jak subskrypcje platformy Azure są kojarzone z usługą Azure Active Directory | Microsoft Azure"
    description="Logowanie do platformy Microsoft Azure i powiązane zagadnienia, takie jak relacja między subskrypcją platformy Azure i usługą Azure Active Directory."
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="stevenpo"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="04/26/2016"
    ms.author="curtand"/>

# Jak subskrypcje platformy Azure są kojarzone z usługą Azure Active Directory

Ten temat opisuje logowanie do platformy Microsoft Azure i powiązane zagadnienia, takie jak relacja między subskrypcją platformy Azure i usługą Azure Active Directory (Azure AD).

## Konta, których można użyć do logowania
Zacznijmy od kont, których można użyć do logowania. Istnieją dwa typy: konto Microsoft (wcześniej znane jako Microsoft Live ID) i konto służbowe, które jest kontem zapisanym w usłudze Azure AD.

 Konto Microsoft  | Konto Azure AD
    ------------- | -------------
System obsługi tożsamości konsumentów firmy Microsoft | System obsługi tożsamości firm firmy Microsoft
Uwierzytelnianie w usługach przeznaczonych dla konsumentów, takich jak Hotmail i MSN | Uwierzytelnianie w usługach przeznaczonych dla firm, takich jak Office 365
Konsumenci tworzą własne konta Microsoft, na przykład w ramach rejestracji poczty e-mail | Firmy i organizacje tworzą własne konta służbowe i zarządzają nimi
Tożsamości są tworzone i przechowywane w systemie kont firmy Microsoft | Tożsamości są tworzone za pomocą platformy Azure lub innej usługi, takiej jak Office 365, i są przechowywane w wystąpieniu usługi Azure AD przypisanym do organizacji

Mimo że platforma Azure pierwotnie zezwalała na dostęp użytkowników wyłącznie za pomocą konta Microsoft, teraz zezwala na dostęp użytkowników z *obu* systemów. Jest to możliwe, ponieważ wszystkie właściwości platformy Azure ufają usłudze Azure AD na potrzeby uwierzytelniania, dzięki czemu usługa Azure AD uwierzytelnia użytkowników w organizacji, i istnieje relacja federacji, w ramach której usługa Azure AD ufa systemowi obsługi tożsamości konsumentów firmy Microsoft na potrzeby uwierzytelniania konsumentów. W ten sposób usługa Azure AD może uwierzytelniać konta „gościa” firmy Microsoft i „natywne” konta usługi Azure AD.

Na przykład tutaj użytkownik z kontem Microsoft loguje się do klasycznego portalu Azure.

> [AZURE.NOTE]
> Aby zalogować się do klasycznego portalu Azure, użytkownik msmith@hotmail.com musi mieć subskrypcję platformy Azure. Konto musi być kontem administratora usługi lub współadministratora subskrypcji.

![][1]

Ponieważ ten adres w usłudze Hotmail to konto konsumenta, logowanie jest uwierzytelniane przez system obsługi tożsamości konsumentów używający kont firmy Microsoft. System obsługi tożsamości usługi Azure AD ufa uwierzytelnieniu wykonanemu przez system obsługi kont Microsoft i wystawia token dostępu do usług platformy Azure.

## Jak subskrypcja platformy Azure jest powiązana z usługą Azure AD

Każda subskrypcja platformy Azure jest połączona relacją zaufania z wystąpieniem usługi Azure AD. Oznacza to, że subskrypcja ufa katalogowi na potrzeby uwierzytelniania użytkowników, usług i urządzeń. Wiele subskrypcji może ufać temu samemu katalogowi, ale dana subskrypcja może ufać tylko jednemu katalogowi. Zaufany katalog można zobaczyć na karcie Ustawienia subskrypcji. Możesz [zmodyfikować ustawienia subskrypcji](active-directory-understanding-resource-access.md), aby zmienić zaufany katalog.

Relacja zaufania między subskrypcją a katalogiem różni się od relacji subskrypcji z wszystkimi innymi zasobami na platformie Azure (witrynami sieci Web, bazami danych itd.), które przypominają bardziej podrzędne zasoby subskrypcji. Jeśli subskrypcja wygaśnie, dostęp do innych zasobów skojarzonych z subskrypcją również nie będzie możliwy. Lecz katalog pozostanie na platformie Azure i będzie można skojarzyć z nim inną subskrypcję oraz kontynuować zarządzanie użytkownikami w katalogu.

Podobnie rozszerzenie usługi Azure AD widoczne w subskrypcji nie działa tak jak inne rozszerzenia w klasycznym portalu Azure. Zakres innych rozszerzeń w klasycznym portalu Azure to subskrypcja platformy Azure. Informacje widoczne w rozszerzeniu usługi Azure AD nie zależą od subskrypcji — pokazane katalogi zależą tylko od zalogowanego użytkownika.

Wszyscy użytkownicy mają jeden katalog macierzysty, który ich uwierzytelnia, ale mogą również być gośćmi w innych katalogach. W rozszerzeniu usługi Azure AD będą widoczne wszystkie katalogi, do których należy konto użytkownika. Wszelkie katalogi, do których konto nie należy, nie zostaną wyświetlone. Katalog może wystawiać tokeny dla kont służbowych w usłudze Azure AD lub użytkowników konta Microsoft (ponieważ usługa Azure AD jest sfederowana z systemem obsługi kont firmy Microsoft).

Diagram przedstawia subskrypcję użytkownika Michael Smith po utworzeniu konta za pomocą konta służbowego firmy Contoso.

![][2]

## Jak zarządzać subskrypcją i katalogiem
Role administracyjne dla subskrypcji platformy Azure zarządzają zasobami powiązanymi z subskrypcją platformy Azure. Te role i najlepsze rozwiązania dotyczące zarządzania subskrypcją są opisane w artykule [Przypisywanie ról administratorów w usłudze Azure Active Directory](active-directory-assign-admin-roles.md).

Domyślnie po utworzeniu konta do użytkownika jest przypisywana rola administratora usługi. Jeśli inni użytkownicy potrzebują logować się i uzyskiwać dostęp do usług za pomocą tej samej subskrypcji, możesz dodać ich jako współadministratorów. Administrator usługi i współadministratorzy mogą używać kont Microsoft lub kont służbowych z katalogu, z którym jest skojarzona subskrypcja platformy Azure.

Usługa Azure AD ma inny zestaw ról administracyjnych na potrzeby zarządzania katalogiem i funkcjami dotyczącymi tożsamości. Na przykład administrator globalny katalogu może dodać użytkowników i grupy do katalogu lub wymusić stosowanie uwierzytelniania wieloskładnikowego przez użytkowników. Do użytkownika, który utworzył katalog, jest przypisywana rola administratora globalnego. Może on przypisać role administratorów do innych użytkowników.

Podobnie jak w przypadku administratorów subskrypcji, role administracyjne usługi Azure AD mogą używać kont Microsoft lub służbowych. Role administracyjne usługi Azure AD są również używane przez inne usługi, takie jak Microsoft Intune i Office 365. Aby uzyskać więcej informacji, zobacz [Przypisywanie ról administratorów](active-directory-assign-admin-roles.md).

Należy koniecznie zwrócić uwagę na to, że administratorzy subskrypcji platformy Azure i administratorzy katalogu usługi Azure AD to dwa oddzielne pojęcia. Administratorzy subskrypcji platformy Azure mogą zarządzać zasobami na platformie Azure i wyświetlić rozszerzenie usługi Active Directory w klasycznym portalu Azure (ponieważ klasyczny portal Azure jest zasobem platformy Azure). Administratorzy katalogu mogą zarządzać właściwościami w katalogu.

Osoba może mieć obie te role, lecz nie jest to wymagane. Użytkownika można przypisać do roli administratora globalnego katalogu, ale nie musi być on administratorem usługi ani współadministratorem subskrypcji platformy Azure. Ten użytkownik nie może zalogować się do klasycznego portalu Azure, ponieważ nie jest administratorem subskrypcji. Może jednak wykonywać zadania administracyjne katalogu przy użyciu innych narzędzi, takich jak program PowerShell usługi Azure AD lub centrum administracyjne usługi Office 365.

## Dlaczego nie mogę zarządzać katalogiem za pomocą mojego bieżącego konta użytkownika?

Czasami użytkownik może próbować zalogować się do klasycznego portalu Azure przy użyciu konta służbowego przed utworzeniem konta subskrypcji platformy Azure. W takim przypadku zostanie wyświetlony komunikat, że nie istnieje subskrypcja dla tego konta. Komunikat zawiera link umożliwiający rozpoczęcie bezpłatnej subskrypcji próbnej.

Po utworzeniu konta w ramach bezpłatnej wersji próbnej użytkownik będzie widział katalog organizacji w klasycznym portalu Azure, lecz nie będzie mógł nim zarządzać (to znaczy dodawać użytkowników ani edytować żadnych istniejących właściwości użytkowników), ponieważ nie jest administratorem globalnym katalogu. Subskrypcja pozwala użytkownikowi na używanie klasycznego portalu Azure i zobaczenie rozszerzenia usługi Azure Active Directory, ale do zarządzania katalogiem są wymagane dodatkowe uprawnienia administratora globalnego.

## Użycie konta służbowego do zarządzania subskrypcją platformy Azure utworzoną za pomocą konta Microsoft

Najlepszym rozwiązaniem jest [utworzenie konta platformy Azure jako organizacja](sign-up-organization.md) i używanie konta służbowego do zarządzania zasobami na platformie Azure. Konta służbowe są preferowane, ponieważ organizacja, która je wystawiła, może nimi zarządzać centralnie oraz obsługują więcej funkcji niż konta Microsoft i są bezpośrednio uwierzytelniane przez usługę Azure AD. To samo konto zapewnia dostęp do innych usług online firmy Microsoft, które są dostępne dla firm i organizacji, takich jak Office 365 lub Microsoft Intune. Jeśli używasz już konta z innymi właściwościami, prawdopodobnie zechcesz użyć tego samego konta na platformie Azure. Będzie także dostępne dla Ciebie wystąpienie usługi Active Directory wspierające te właściwości, którym ma ufać subskrypcja platformy Azure.

Kontami służbowymi można zarządzać na więcej sposobów niż kontami Microsoft. Na przykład administrator może zresetować hasło konta służbowego lub wymusić stosowanie dla niego uwierzytelniania wieloskładnikowego.

W niektórych przypadkach może być przydatne umożliwienie użytkownikowi z organizacji zarządzania zasobami skojarzonymi z subskrypcją platformy Azure w ramach konta Microsoft konsumenta. Aby uzyskać więcej informacji o sposobie przejścia do konfiguracji, w której różne konta zarządzają subskrypcjami lub katalogami, zobacz [Zarządzanie katalogiem dla subskrypcji usługi Office 365 na platformie Azure](#manage-the-directory-for-your-office-365-subscription-in-azure).


## Logowanie w przypadku użycia służbowego adresu e-mail dla konta Microsoft

Jeśli w przeszłości utworzono konto Microsoft konsumenta za pomocą służbowego adresu e-mail jako identyfikatora użytkownika, może zostać wyświetlona strona z żądaniem wybrania systemu kont Microsoft Azure lub systemu kont Microsoft.

![][3]

To oznacza, że istnieją konta o tej samej nazwie — jedno w usłudze Azure AD i drugie w systemie obsługi kont Microsoft konsumenta. Należy wybrać konto skojarzone z subskrypcją platformy Azure, której chcesz użyć. Jeśli zostanie wyświetlony błąd informujący, że subskrypcja nie istnieje dla użytkownika, prawdopodobnie została wybrana nieprawidłowa opcja. Wyloguj się i spróbuj ponownie. Aby uzyskać więcej informacji o błędach, które mogą uniemożliwić logowanie, zobacz [Troubleshooting "We were unable to find any subscriptions associated with your account" errors](https://social.msdn.microsoft.com/Forums/en-US/f952f398-f700-41a1-8729-be49599dd7e2/troubleshooting-we-were-unable-to-find-any-subscriptions-associated-with-your-account-errors-in?forum=windowsazuremanagement) (Rozwiązywanie problemów z błędami „Nie możemy znaleźć żadnych subskrypcji skojarzonych z Twoim kontem”).

## Zarządzanie katalogiem dla subskrypcji usługi Office 365 na platformie Azure

Przykładowa sytuacja: użytkownik utworzył konto w ramach usługi Office 365 przed zarejestrowaniem się na platformie Azure. Teraz chce zarządzać katalogiem subskrypcji usługi Office 365 w klasycznym portalu Azure. Są dwa sposoby, na które można to zrobić, w zależności od tego, czy użytkownik utworzył konto na platformie Azure.

### Użytkownik nie ma subskrypcji platformy Azure

W takim przypadku należy po prostu [utworzyć konto na platformie Azure](sign-up-organization.md) przy użyciu tego samego konta służbowego co użyte do logowania do usługi Office 365. Odpowiednie informacje z konta usługi Office 365 zostaną wstępnie wprowadzone w formularzu tworzenia konta na platformie Azure. Konto zostanie przypisane do roli administratora usługi dla subskrypcji.  

### Użytkownik nie ma subskrypcji platformy Azure używającej konta Microsoft

Jeśli użytkownik utworzył konto w usłudze Office 365 za pomocą konta służbowego, następnie utworzył konto na platformie Azure za pomocą konta Microsoft, będą istniały dwa katalogi: jeden dla konta służbowego i domyślny katalog utworzony podczas tworzenia konta na platformie Azure.

Aby zarządzać obydwoma katalogami w klasycznym portalu Azure, wykonaj następujące kroki.

> [AZURE.NOTE]
> Te kroki można wykonać tylko wtedy, gdy użytkownik jest zalogowany za pomocą konta Microsoft. Jeśli użytkownik jest zalogowany przy użyciu konta służbowego, opcja **Użyj istniejącego katalogu** jest niedostępna, ponieważ konto służbowe można uwierzytelnić tylko za pomocą jego katalogu macierzystego (tzn. katalogu, w którym jest przechowywane konto służbowe i które należy do właściciela tego konta).

1. Zaloguj się do klasycznego portalu Azure za pomocą konta Microsoft.

2. Kliknij pozycje **Nowy** > **Usługi aplikacji** > **Usługa Active Directory** > **Katalog** > **Utwórz niestandardowy**.

3. Kliknij pozycję **Użyj istniejącego katalogu**, zaznacz pozycję **Gotowe do wylogowania** i kliknij znacznik wyboru, aby zakończyć akcję.

4. Zaloguj się do klasycznego portalu Azure przy użyciu konta z uprawnieniami administratora globalnego dla katalogu służbowego.

5. Po wyświetleniu pytania **Użyć katalogu Contoso dla platformy Azure?** kliknij pozycję **Kontynuuj**.

6. Kliknij pozycję **Wyloguj się teraz**.

7. Zaloguj się ponownie do klasycznego portalu Azure za pomocą konta Microsoft. Oba katalogi będą wyświetlane w rozszerzeniu usługi Active Directory.


## Następne kroki

- Aby dowiedzieć się więcej o modyfikowaniu administratorów subskrypcji platformy Azure, zobacz [How to add or change Azure administrator roles](../billing-add-change-azure-subscription-administrator.md) (Jak dodać lub zmienić role administratora platformy Azure).

- Aby dowiedzieć się więcej o kontrolowaniu dostępu do zasobów na platformie Microsoft Azure, zobacz [Understanding resource access in Azure](active-directory-understanding-resource-access.md) (Opis dostępu do zasobów na platformie Azure).

- Aby uzyskać więcej informacji na temat sposobu przypisywania ról w usłudze Azure AD, zobacz [Przypisywanie ról administratorów w usłudze Azure Active Directory](active-directory-assign-admin-roles.md).

- [Tworzenie konta na platformie Azure jako organizacja](sign-up-organization.md)


<!--Image references-->
[1]: ./media/active-directory-how-subscriptions-associated-directory/WAAD_PassThruAuth.png
[2]: ./media/active-directory-how-subscriptions-associated-directory/WAAD_OrgAccountSubscription.png
[3]: ./media/active-directory-how-subscriptions-associated-directory/WAAD_SignInDisambiguation.PNG



<!--HONumber=Jun16_HO2-->


