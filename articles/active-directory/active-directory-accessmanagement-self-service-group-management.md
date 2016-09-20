<properties
    pageTitle="Konfigurowanie usługi Azure Active Directory do samoobsługowego zarządzania dostępem do aplikacji | Microsoft Azure"
    description="Samoobsługowe zarządzanie grupami umożliwia użytkownikom tworzenie grup zabezpieczeń lub grup usługi Office 365 i zarządzanie nimi w usłudze Azure Active Directory, a także daje im możliwość żądania członkostwa w grupach zabezpieczeń lub grupach usługi Office 365."
    services="active-directory"
    documentationCenter=""
  authors="curtand"
    manager="femila"
    editor=""
    />

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/10/2016"
    ms.author="curtand"/>

# Konfigurowanie usługi Azure Active Directory do samoobsługowego zarządzania grupami

Samoobsługowe zarządzanie grupami pozwala użytkownikom na tworzenie grup zabezpieczeń lub grup Office 365 w usłudze Azure Active Directory (Azure AD) oraz zarządzanie nimi. Użytkownicy mogą również żądać członkostwa w grupie zabezpieczeń lub grupie Office 365, a następnie właściciel grupy może zatwierdzić członkostwo lub odmówić jego przypisania. Dzięki temu można przekazać codzienne zarządzanie członkostwem w grupach osobom znającym kontekst biznesowy tego członkostwa. Funkcje samoobsługowego zarządzania grupami są dostępne wyłącznie w przypadku grup zabezpieczeń i grup Office 365, natomiast nie są dostępne w przypadku list dystrybucyjnych lub grup zabezpieczeń z włączoną obsługą poczty.

Samoobsługowe zarządzanie grupami aktualnie obejmuje dwa podstawowe scenariusze: delegowane zarządzanie grupami i samoobsługowe zarządzanie grupami.

- **Delegowane zarządzanie grupami**
   — na przykład gdy administrator zarządza dostępem do aplikacji SaaS używanej w firmie. Zarządzanie prawami dostępu jest coraz większym obciążeniem, więc administrator zwraca się do właściciela firmy o utworzenie nowej grupy. Administrator przypisuje nowej grupie dostęp do aplikacji i dodaje do grupy wszystkie osoby, które już uzyskują dostęp do aplikacji. Właściciel firmy może dodawać kolejnych użytkowników, dla których aplikacja jest automatycznie aprowizowana. Właściciel firmy nie musi czekać na administratora w celu zarządzania dostępem dla użytkowników. Jeśli administrator nada to samo uprawnienie menedżerowi w innej grupie biznesowej, może on następnie zarządzać dostępem swoich użytkowników. Właściciel firmy nie może wyświetlać użytkowników menedżera ani zarządzać nimi, a menedżer nie może wyświetlać użytkowników właściciela ani zarządzać nimi. Administrator nadal może wyświetlać wszystkich użytkowników mających dostęp do aplikacji i w razie potrzeby blokować prawa dostępu.

- **Samoobsługowe zarządzanie grupami**
   — na przykład gdy dwóch użytkowników ma niezależnie skonfigurowane witryny usługi SharePoint Online i każdy z nich chce przydzielić dostęp do swojej witryny zespołowi drugiego użytkownika. W tym celu mogą utworzyć jedną grupę w usłudze Azure AD i wybrać tę grupę do przydzielenia dostępu do witryn w usłudze SharePoint Online. Gdy inna osoba będzie chciała uzyskać dostęp, wprowadzi żądanie w Panelu dostępu, a po zatwierdzeniu automatycznie uzyska dostęp do obu witryn usługi SharePoint Online. Jeden z użytkowników może stwierdzić później, że wszystkie osoby mające dostęp do witryny powinny również mieć dostęp do określonej aplikacji SaaS. Administrator aplikacji SaaS może dodać do witryny programu SharePoint Online prawa dostępu dla aplikacji. Od tego momentu zatwierdzenie żądania dostępu spowoduje przyznanie dostępu do obu witryn usługi SharePoint Online oraz do tej aplikacji SaaS.

## Włączanie samoobsługi użytkowników końcowych w grupie

1. W [klasycznym portalu Azure](https://manage.windowsazure.com) otwórz katalog Azure AD.

2. Na karcie **Konfiguracja** ustaw wartość Włączone dla pozycji **Delegowane zarządzanie grupami**.

3. Ustaw wartość Włączone dla pozycji **Użytkownicy mogą tworzyć grupy zabezpieczeń** lub **Użytkownicy mogą tworzyć grupy Office**.

W przypadku włączenia opcji **Użytkownicy mogą tworzyć grupy zabezpieczeń** wszyscy użytkownicy w katalogu mogą tworzyć nowe grupy zabezpieczeń i dodawać do nich członków. Te nowe grupy będą również wyświetlane na panelu dostępu dla innych użytkowników. Jeśli ustawienie zasad grupy to umożliwia, inni użytkownicy mogą tworzyć żądania, aby dołączyć do tych grup. Jeśli opcja **Użytkownicy mogą tworzyć grupy zabezpieczeń** jest wyłączona, użytkownicy nie mogą tworzyć grup ani zmieniać istniejących grup, których są właścicielami. Jednak nadal mogą zarządzać członkostwem w tych grupach i zatwierdzać żądania dołączenia do ich grup pochodzące od innych użytkowników.

Dodatkowo możesz użyć opcji **Użytkownicy, którzy mogą używać samoobsługi dla grup zabezpieczeń**, aby dokładniej sterować dostępem użytkowników do samoobsługowego zarządzania grupami. W przypadku włączenia opcji **Użytkownicy mogą tworzyć grupy** wszyscy użytkownicy w katalogu mogą tworzyć nowe grupy i dodawać do nich członków. Jeśli dodatkowo dla opcji **Użytkownicy, którzy mogą używać samoobsługi dla grup zabezpieczeń** zostanie wybrane ustawienie Niektórzy, zarządzanie grupami zostanie ograniczone do wybranej grupy użytkowników. Gdy ten przełącznik ma wartość Niektóre, należy dodać użytkowników do grupy SSGMSecurityGroupsUsers, zanim będą oni mogli tworzyć nowe grupy i dodawać do nich członków. Wybranie ustawienia Wszyscy dla opcji **Użytkownicy, którzy mogą używać samoobsługi dla grup zabezpieczeń** umożliwi wszystkim użytkownikom w katalogu tworzenie nowych grup.

Dodatkowo możesz użyć pola **Grupa, która może używać samoobsługi dla grup zabezpieczeń**, aby wskazać niestandardową nazwę grupy, której członkowie mogą korzystać z funkcji samoobsługi.

## Dodatkowe informacje

Te artykuły zawierają dodatkowe informacje o usłudze Azure Active Directory.

* [Zarządzanie dostępem do zasobów za pomocą grup usługi Azure Active Directory](active-directory-manage-groups.md)

* [Polecenia cmdlet usługi Azure Active Directory służące do konfigurowania ustawień grupy](active-directory-accessmanagement-groups-settings-cmdlets.md)

* [Indeks artykułów dotyczących zarządzania aplikacjami w usłudze Azure Active Directory](active-directory-apps-index.md)

* [Co to jest usługa Azure Active Directory?](active-directory-whatis.md)

* [Integrowanie tożsamości lokalnych z usługą Azure Active Directory](active-directory-aadconnect.md)



<!--HONumber=sep16_HO1-->


