---
title: Zarządzanie katalogiem dla subskrypcji usługi Office 365 na platformie Azure | Microsoft Docs
description: Zarządzanie katalogiem subskrypcji usługi Office 365 za pomocą usługi Azure Active Directory i klasycznego portalu Azure
services: active-directory
documentationcenter: ''
author: curtand
manager: femila
editor: ''

ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/23/2016
ms.author: curtand

---
# Zarządzanie katalogiem dla subskrypcji usługi Office 365 na platformie Azure
W tym artykule opisano sposób zarządzania katalogiem, który został utworzony dla subskrypcji usługi Office 365, przy użyciu klasycznego portalu Azure. Aby zalogować się do klasycznego portalu Azure, musisz być administratorem usługi albo współadministratorem subskrypcji platformy Azure. Jeśli nie masz jeszcze subskrypcji platformy Azure, już dziś możesz zarejestrować się w celu uzyskania [bezpłatnej 30-dniowej wersji próbnej](https://azure.microsoft.com/trial/get-started-active-directory/) i za pomocą tego łącza w ciągu 5 minut wdrożyć swoje pierwsze rozwiązanie w chmurze. Aby zalogować się do usługi Office 365, użyj konta służbowego lub szkolnego.

Po uzyskaniu subskrypcji platformy Azure możesz zalogować się do klasycznego portalu Azure i uzyskać dostęp do usług Azure. Kliknij rozszerzenie usługi Active Directory, aby zarządzać tym samym katalogiem, za pomocą którego uwierzytelniani są użytkownicy usługi Office 365.

Jeśli masz już subskrypcję platformy Azure, proces zarządzania dodatkowym katalogiem jest również oczywisty. Na przykład Jan Nowak może mieć subskrypcję usługi Office 365 dla domeny Contoso.com. Ma on także subskrypcję platformy Azure, którą uzyskał przy użyciu swojego konta Microsoft jnowak@hotmail.com. W takim przypadku zarządza dwoma katalogami.

| Subskrypcja | Office 365 | Azure |
| --- | --- | --- |
|   Nazwa wyświetlana |Contoso |Domyślny katalog Azure Active Directory (Azure AD) |
|   Nazwa domeny |contoso.com |jnowakhotmail.onmicrosoft.com |

Chce zarządzać tożsamościami użytkowników w katalogu firmy Contoso, gdy jest zalogowany przy użyciu swojego konta Microsoft Azure, tak aby mógł włączać funkcje usługi Azure AD, takie jak uwierzytelnianie wieloskładnikowe. Poniższy diagram ułatwia zilustrowanie tego procesu.

![Diagram do zarządzania dwoma niezależnymi katalogami](./media/active-directory-manage-o365-subscription/AAD_O365_03.png)

W takim przypadku dwa katalogi są od siebie niezależne.

## Aby zarządzać dwoma niezależnymi katalogami
Aby Jan Nowa mógł zarządzać oboma katalogami, gdy jest zalogowany na platformie Azure jako jnowak@hotmail.com, musi wykonać następujące czynności:

> [!NOTE]
> Te kroki można wykonać tylko wtedy, gdy użytkownik jest zalogowany za pomocą konta Microsoft. Jeśli użytkownik jest zalogowany przy użyciu konta służbowego lub szkolnego, opcja **Użyj istniejącego katalogu** jest niedostępna. Konto służbowe lub szkolne może zostać uwierzytelnione tylko przez jego katalog główny (to znaczy katalog, w którym znajduje się konto służbowe lub szkolne, należący do firmy lub szkoły).
> 
> 

1. Zaloguj się do [klasycznego portalu Azure](https://manage.windowsazure.com) jako jnowak@hotmail.com.
2. Kliknij pozycje **Nowy** > **App Services** > **Active Directory** > **Katalog** > **Utwórz niestandardowy**.
3. Kliknij opcję Użyj istniejącego katalogu i zaznacz pole wyboru **Chcę się teraz wylogować**.
4. Zaloguj się do klasycznego portalu Azure jako administrator globalny domeny Contoso.onmicrosoft.com (na przykład jnowak@contoso.com).
5. Po wyświetleniu monitu **Użyć katalogu Contoso dla platformy Azure?** kliknij pozycję **Kontynuuj**.
6. Kliknij pozycję **Wyloguj się teraz**.
7. Zaloguj się do klasycznego portalu Azure jako jnowak@hotmail.com. Katalog Contoso i katalog domyślny są wyświetlane w rozszerzeniu usługi Active Directory.

Po wykonaniu tych kroków użytkownik jnowak@hotmail.com jest administratorem globalnym w katalogu firmy Contoso.

## Aby administrować zasobami jako administrator globalny
Teraz załóżmy, że Anna Kowalska musi administrować witrynami sieci Web i zasobami baz danych skojarzonymi z subskrypcją platformy Azure dla użytkownika jnowak@hotmail.com. Aby mogła to zrobić, Jan Nowak musi wykonać następujące dodatkowe czynności:

1. Zalogować się do [klasycznego portalu Azure](https://manage.windowsazure.com) przy użyciu konta administratora usługi dla subskrypcji platformy Azure (w tym przykładzie jnowak@hotmail.com).
2. Przenieść subskrypcję do katalogu Contoso: kliknąć pozycje **Ustawienia** > **Subskrypcje** > wybrać subskrypcję > **Edytuj katalog** > wybrać pozycję **Contoso (Contoso.com)**. W ramach przeniesienia wszystkie konta służbowe, które nie mają uprawnień współadministratorów subskrypcji, są usuwane.
3. Dodać Annę Kowalską jako współadministratora subskrypcji: kliknąć pozycje **Ustawienia** > **Administratorzy** > wybrać subskrypcję > **Dodaj** > wpisać **AnnaKowalska@Contoso.com**.

## Następne kroki
Aby uzyskać więcej informacji na temat relacji między subskrypcjami i katalogami, zobacz [Jak subskrypcja jest skojarzona z katalogiem](active-directory-how-subscriptions-associated-directory.md).

<!--HONumber=Sep16_HO3-->


