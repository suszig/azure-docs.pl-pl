<properties
    pageTitle="Konfigurowanie usługi Azure Active Directory do samoobsługowego zarządzania dostępem do aplikacji | Microsoft Azure"
    description="Samoobsługowe zarządzanie grupami umożliwia użytkownikom tworzenie grup zabezpieczeń lub grup usługi Office 365 i zarządzanie nimi w usłudze Azure Active Directory, a także daje im możliwość żądania członkostwa w grupach zabezpieczeń lub grupach usługi Office 365."
    services="active-directory"
    documentationCenter=""
  authors="curtand"
    manager="stevenpo"
    editor=""
    />

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="05/26/2016"
    ms.author="curtand"/>

# Konfigurowanie usługi Azure Active Directory do samoobsługowego zarządzania grupami

Samoobsługowe zarządzanie grupami umożliwia użytkownikom tworzenie grup zabezpieczeń lub grup usługi Office 365 i zarządzanie nimi w usłudze Azure Active Directory (Azure AD), a także daje im możliwość żądania członkostwa w grupach zabezpieczeń lub grupach usługi Office 365, które jest następnie przydzielane lub nie przez właściciela grupy. Dzięki korzystaniu z funkcji samoobsługowego zarządzania grupami można przekazać codzienne zarządzanie członkostwem w grupach osobom znającym kontekst biznesowy tego członkostwa. Funkcje samoobsługowego zarządzania grupami są dostępne wyłącznie w przypadku grup zabezpieczeń i grup usługi Office 365, a niedostępne w przypadku list dystrybucyjnych lub grup zabezpieczeń z włączoną obsługą poczty.

Samoobsługowe zarządzanie grupami aktualnie obejmuje dwa podstawowe scenariusze: delegowane zarządzanie grupami i samoobsługowe zarządzanie grupami.

- **Delegowane zarządzanie grupami** — na przykład gdy administrator zarządza dostępem do aplikacji SaaS używanej w firmie. Zarządzanie prawami dostępu jest coraz większym obciążeniem, więc administrator zwraca się do właściciela firmy o utworzenie nowej grupy. Następnie administrator przydziela dostęp do aplikacji nowo utworzonej grupie i umieszcza w niej wszystkie osoby mające obecnie dostęp do aplikacji. Właściciel firmy może dodawać kolejnych użytkowników, którzy po chwili automatycznie uzyskają dostęp do informacji. Właściciel firmy nie musi więc czekać, aż administrator wykona te działania — może sam zarządzać dostępem użytkowników. Administrator może tak samo postąpić w przypadku asystenta administracyjnego zajmującego się inną grupą biznesową, dzięki czemu zarówno właściciel firmy, jak i asystent mogą zarządzać dostępem swoich użytkowników — a jednocześnie nie mogą wyświetlać użytkowników podlegających tej drugiej osobie, ani nimi zarządzać. Administrator nadal może wyświetlać wszystkich użytkowników mających dostęp do aplikacji i w razie potrzeby blokować prawa dostępu.

- **Samoobsługowe zarządzanie grupami** — na przykład gdy dwóch użytkowników ma niezależnie skonfigurowane witryny usługi SharePoint Online i każdy z nich chce przydzielić dostęp do swojej witryny zespołowi drugiego użytkownika. W tym celu mogą utworzyć jedną grupę w usłudze Azure AD i wybrać tę samą grupę do przydzielenia dostępu do witryn w usłudze SharePoint Online. Gdy inna osoba będzie chciała uzyskać dostęp, wprowadzi żądanie w Panelu dostępu, a po zatwierdzeniu automatycznie uzyska dostęp do obu witryn usługi SharePoint Online. Jeden z użytkowników może stwierdzić później, że wszystkie osoby mające dostęp do jego witryny powinny również uzyskać dostęp do określonej aplikacji SaaS. Zwróci się więc do administratora tej aplikacji SaaS o dodanie praw dostępu do tej aplikacji do witryny użytkownika. Od tego momentu zatwierdzenie żądania dostępu spowoduje przyznanie dostępu do obu witryn usługi SharePoint Online oraz do tej aplikacji SaaS.

## Włączanie samoobsługi użytkowników końcowych w grupie

W [klasycznym portalu Azure](https://manage.windowsazure.com) na karcie **Konfiguracja** włącz opcję **Delegowane zarządzanie grupami**, a następnie włącz opcję **Użytkownicy mogą tworzyć grupy zabezpieczeń** lub **Użytkownicy mogą tworzyć grupy Office**.

W przypadku włączenia opcji **Użytkownicy mogą tworzyć grupy zabezpieczeń** wszyscy użytkownicy w katalogu mogą tworzyć nowe grupy zabezpieczeń i dodawać do nich członków. Te nowe grupy będą również wyświetlane w Panelu dostępu wszystkich innych użytkowników, którzy będą mogli przesyłać żądania dołączenia do tych grup, jeśli zezwala na to ustawienie zasad grupy. W przypadku wyłączenia opcji **Użytkownicy mogą tworzyć grupy zabezpieczeń** użytkownicy nie mogą tworzyć grup ani zmieniać istniejących grup, których są właścicielami, ale nadal mogą zarządzać członkostwem w tych grupach i zatwierdzać żądania dołączenia do grup otrzymane od innych użytkowników.

Możesz również użyć opcji **Użytkownicy, którzy mogą używać samoobsługi dla grup zabezpieczeń**, aby bardziej szczegółowo określić możliwości samoobsługowego zarządzania grupami przez użytkowników. W przypadku włączenia opcji **Użytkownicy mogą tworzyć grupy** wszyscy użytkownicy w katalogu mogą tworzyć nowe grupy i dodawać do nich członków. Jeśli dodatkowo dla opcji **Użytkownicy, którzy mogą używać samoobsługi dla grup zabezpieczeń** zostanie wybrane ustawienie Niektórzy, zarządzanie grupami zostanie ograniczone do wybranej grupy użytkowników. Wybranie ustawienia Niektórzy spowoduje utworzenie w katalogu grupy SSGMSecurityGroupsUsers. Tylko użytkownicy należący do tej grupy będą mogli tworzyć nowe grupy zabezpieczeń w katalogu i dodawać do nich użytkowników. Wybranie ustawienia Wszyscy dla opcji **Użytkownicy, którzy mogą używać samoobsługi dla grup zabezpieczeń** umożliwi wszystkim użytkownikom w katalogu tworzenie nowych grup.

Możesz także użyć pola **Grupa, która może używać samoobsługi dla grup zabezpieczeń** (z domyślnym ustawieniem „SSGMSecurityGroupsUsers”), aby wskazać własną niestandardową nazwę grupy zawierającej wszystkich użytkowników, którzy mogą korzystać z funkcji samoobsługi i tworzyć nowe grupy w katalogu.

## Dodatkowe informacje

Te artykuły zawierają dodatkowe informacje o usłudze Azure Active Directory.

* [Zarządzanie dostępem do zasobów za pomocą grup usługi Azure Active Directory](active-directory-manage-groups.md)

* [Indeks artykułów dotyczących zarządzania aplikacjami w usłudze Azure Active Directory](active-directory-apps-index.md)

* [Co to jest usługa Azure Active Directory?](active-directory-whatis.md)

* [Integrowanie tożsamości lokalnych z usługą Azure Active Directory](active-directory-aadconnect.md)



<!--HONumber=Jun16_HO2-->


