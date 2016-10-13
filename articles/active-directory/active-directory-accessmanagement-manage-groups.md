<properties
    pageTitle="Zarządzanie grupami w usłudze Azure Active Directory | Microsoft Azure"
    description="Jak tworzyć grupy i zarządzać nimi, aby zarządzać użytkownikami platformy Azure przy użyciu usługi Azure Active Directory."
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/29/2016"
    ms.author="curtand"/>



# Zarządzanie grupami w usłudze Azure Active Directory

> [AZURE.SELECTOR]
- [Azure Portal](active-directory-groups-create-azure-portal.md)
- [klasyczny portal Azure](active-directory-accessmanagement-manage-groups.md)
- [PowerShell](active-directory-accessmanagement-groups-v2-cmdlets.md)


Jedną z funkcji zarządzania użytkownikami w usłudze Azure Active Directory (Azure AD) jest tworzenie grup użytkowników. Grupa służy do wykonywania zadań zarządzania, takich jak przypisywanie licencji lub uprawnień do kilku użytkowników jednocześnie. Za pomocą grup można także przypisywać uprawnienia dostępu do:

- zasobów takich jak obiekty w katalogu
- Zasobów spoza katalogu, na przykład aplikacji SaaS, usług platformy Azure, witryn programu SharePoint lub zasobów lokalnych

Ponadto właściciel zasobu może także przypisać dostęp do tego zasobu do grupy usługi Azure AD należącej do innej osoby. To przypisanie spowoduje przydzielenie dostępu do zasobu członkom tej grupy. Następnie właściciel grupy może zarządzać członkostwem w grupie. Zatem w praktyce właściciel zasobu przekazuje właścicielowi grupy uprawnienie do przypisywania użytkowników do swojego zasobu.

## Jak utworzyć grupę?

W zależności od usług, które subskrybuje Twoja organizacja, możesz utworzyć grupę w jednym z następujących miejsc:
- Klasyczny portal Azure
- Portal konta usługi Office 365
- Portal konta usługi Windows Intune

Zadania opiszemy w taki sposób, jakby były wykonywane w klasycznym portalu Azure. Aby uzyskać więcej informacji na temat zarządzania katalogiem Azure AD przy użyciu portali innych niż Azure, zobacz [Administrowanie katalogiem usługi Azure AD](active-directory-administer.md).

1. W [klasycznym portalu Azure](https://manage.windowsazure.com) wybierz pozycję **Active Directory**, a następnie wybierz nazwę katalogu swojej organizacji.

2. Wybierz kartę **Grupy**.

3. Wybierz polecenie **Dodaj grupę**.

4. W oknie **Dodawanie grupy** podaj nazwę i opis grupy.


## Jak dodawać i usuwać pojedynczych użytkowników w grupie zabezpieczeń?

**Aby dodać użytkownika do grupy**

1. W [klasycznym portalu Azure](https://manage.windowsazure.com) wybierz pozycję **Active Directory**, a następnie wybierz nazwę katalogu swojej organizacji.

2. Wybierz kartę **Grupy**.

3. Otwórz grupę, do której chcesz dodać członków. Otwórz kartę **Członkowie** wybranej grupy, jeśli nie jest jeszcze wyświetlana.

4. Wybierz polecenie **Dodaj członków**.

5. Na stronie **Dodawanie członków** wybierz nazwę użytkownika, którego chcesz dodać jako członka grupy, lub nazwę grupy takich użytkowników. Upewnij się, że nazwa została dodana do okienka **Wybrane**.


**Aby usunąć użytkownika z grupy**

1. W [klasycznym portalu Azure](https://manage.windowsazure.com) wybierz pozycję **Active Directory**, a następnie wybierz nazwę katalogu swojej organizacji.

2. Wybierz kartę **Grupy**.

3. Otwórz grupę, z której chcesz usunąć członków.

4. Wybierz kartę **Członkowie**, wybierz nazwę członka, którego chcesz usunąć z grupy, a następnie kliknij polecenie **Usuń**.

6. Po wyświetleniu monitu potwierdź, że chcesz usunąć tego członka z grupy.


## Jak dynamicznie zarządzać członkostwem w grupie?

W usłudze Azure AD można bardzo łatwo skonfigurować prostą regułę w celu określenia, którzy użytkownicy powinni być członkami grupy. Prosta reguła to taka, która wykonuje tylko jedno porównanie. Jeśli na przykład grupa jest przypisana do aplikacji SaaS, możesz skonfigurować regułę dodającą użytkowników, którzy mają stanowisko „Przedstawiciel handlowy”. Następnie ta reguła udziela dostępu do tej aplikacji SaaS wszystkim użytkownikom mającym to stanowisko w katalogu.

Po zmianie dowolnych atrybutów użytkownika system ocenia wszystkie zasady dotyczące grup dynamicznych w katalogu, aby ustalić, czy zmiana atrybutów użytkownika spowoduje dodanie elementów do grupy lub usunięcie ich z niej. Jeśli użytkownik spełnia wymagania zasad grupy, jest dodawany jako członek do tej grupy. Jeśli użytkownicy nie spełniają już zasad grupy, której są członkami, są usuwani z tej grupy.

> [AZURE.NOTE] Możesz skonfigurować reguły dynamicznego zarządzania członkostwem w grupach zabezpieczeń lub w grupach usługi Office 365. Aktualnie w ramach przypisywania do aplikacji na podstawie grup nie jest obsługiwane członkostwo w grupach zagnieżdżonych.
>
> Dynamiczne zarządzanie członkostwem w grupach wymaga przypisania licencji usługi Azure AD w wersji Premium do:
>
> - administratora zarządzającego regułą grupy
> - Wszyscy członkowie grupy

**Aby włączyć dynamiczne zarządzanie członkostwem w grupie**

1. W [klasycznym portalu Azure](https://manage.windowsazure.com) wybierz pozycję **Active Directory**, a następnie wybierz nazwę katalogu swojej organizacji.

2. Wybierz kartę **Grupy** i otwórz grupę, którą chcesz edytować.

3. Wybierz kartę **Konfiguracja**, a następnie wybierz ustawienie **Tak** dla opcji **Włącz członkostwo dynamiczne**.

4. Skonfiguruj prostą regułę grupy, aby sterować sposobem działania członkostwa dynamicznego w grupie. Upewnij się, że jest zaznaczona opcja **Dodaj użytkowników, dla których**, a następnie wybierz właściwość użytkownika z listy (na przykład dział, stanowisko itp.).

5. Następnie wybierz warunek (Nie równa się, Równa się, Nie rozpoczyna się od, Rozpoczyna się od, Nie zawiera, Zawiera, Nie jest zgodne, Jest zgodne).

6. Podaj wartość porównania wybranej właściwości użytkownika.

Aby dowiedzieć się, jak tworzyć reguły *zaawansowane* (czyli takie, które mogą zawierać więcej niż jedno porównanie) na potrzeby dynamicznego zarządzania członkostwem w grupach, zobacz [Tworzenie reguł zaawansowanych za pomocą atrybutów](active-directory-accessmanagement-groups-with-advanced-rules.md).

## Dodatkowe informacje

Te artykuły zawierają dodatkowe informacje o usłudze Azure Active Directory.

* [Zarządzanie dostępem do zasobów za pomocą grup usługi Azure Active Directory](active-directory-manage-groups.md)

* [Polecenia cmdlet usługi Azure Active Directory służące do konfigurowania ustawień grupy](active-directory-accessmanagement-groups-settings-cmdlets.md)

* [Indeks artykułów dotyczących zarządzania aplikacjami w usłudze Azure Active Directory](active-directory-apps-index.md)

* [Co to jest usługa Azure Active Directory?](active-directory-whatis.md)

* [Integrowanie tożsamości lokalnych z usługą Azure Active Directory](active-directory-aadconnect.md)



<!--HONumber=Oct16_HO1-->


