<properties
    pageTitle="Tożsamość hybrydowa: porównanie narzędzi do integracji katalogów | Microsoft Azure"
    description="Na tej stronie udostępniona jest kompleksowa tabela zawierająca porównanie różnych narzędzi integracji katalogów, których można użyć do integracji katalogów."
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/08/2016"
    ms.author="billmath"/>


# Tożsamość hybrydowa: porównanie narzędzi do integracji katalogów

Narzędzia do integracji katalogów były rozbudowywane i rozwijane przez wiele lat.  Ten dokument zapewnia skonsolidowany widok tych narzędzi i ułatwia porównanie funkcji, które są w nich dostępne.

<!-- The hardcoded link is a workaround for campaign ids not working in acom links-->

>[AZURE.NOTE] Program Azure AD Connect obejmuje składniki i funkcje wcześniej wydane jako narzędzia Dirsync i AAD Sync. Te narzędzia nie są już udostępniane oddzielnie, a wszystkie przyszłe ulepszenia będą dołączane do aktualizacji programu Azure AD Connect, dzięki czemu zawsze będzie wiadomo, gdzie można uzyskać najnowsze funkcje.
>
>Narzędzia DirSync i Azure AD Sync są przestarzałe. Więcej informacji można znaleźć [tutaj](active-directory-aadconnect-dirsync-deprecated.md).


Poniższy klucz ma zastosowanie do wszystkich tabel.

● = dostępna teraz  
PW = w przyszłym wydaniu  
PZ = publiczna wersja zapoznawcza  

## Synchronizacja danych lokalnych z chmurą

| Funkcja  | Azure Active Directory Connect  | Usługi synchronizacji usługi Azure Active Directory (AAD Sync) | Narzędzie do synchronizacji z usługą Azure Active Directory (DirSync)| Forefront Identity Manager 2010 R2 (FIM) |Microsoft Identity Manager 2016 (MIM)|
| :-------- |:--------:|:--------:|:--------:|:--------:|:--------:
| Połączenie z pojedynczym lokalnym lasem usługi AD | ● | ● | ● | ● |● |
| Połączenie z wieloma lokalnymi lasami usługi AD |●  | ● |  | ● |● |
| Połączenie z wieloma lokalnymi organizacjami Exchange | ● |  |  |  | |
| Połączenie z pojedynczym lokalnym katalogiem LDAP | PW |  |  | ● |● |
| Połączenie z wieloma lokalnymi katalogami LDAP |PW  |  |  | ● |● |
| Połączenie z lokalnymi katalogami usługi AD i lokalnymi katalogami LDAP |PW  |  |  | ● |● |
| Połączenie z systemami niestandardowymi (np. SQL, Oracle, MySQL itp.) | PW |  |  | ● |● |
| Synchronizowanie atrybutów zdefiniowanych przez klienta (rozszerzenia katalogów) | ● |  |  |  |  |
|Połączenie z lokalnymi systemami kadr (np. SAP, Oracle eBusiness, PeopleSoft)| PW |  |  | ● |● |
|Obsługuje reguły synchronizacji programu FIM i łączniki do aprowizowania dla systemów lokalnych|  |  |  | ● |● |

## Synchronizacja chmury z danymi lokalnymi

| Funkcja  | Azure Active Directory Connect  | Usługi synchronizacji usługi Azure Active Directory | Narzędzie do synchronizacji z usługą Azure Active Directory (DirSync) | Forefront Identity Manager 2010 R2 (FIM) |Microsoft Identity Manager 2016 (MIM)|
| :-------- |:--------:|:--------:|:--------:|:--------:|:--------:
| Zapisywanie zwrotne urządzeń | ● |  | ● |  ||
| Zapisywanie zwrotne atrybutów (dla wdrożenia hybrydowego programu Exchange) | ● | ● | ● | ● |● |
| Zapisywanie zwrotne obiektów grup i użytkowników |  ●|  | |  ||
| Zapisywanie zwrotne haseł (z funkcji samoobsługowego resetowania hasła i zmiany hasła) |  ● | ● |  |  ||



## Obsługa funkcji uwierzytelniania

| Funkcja  | Azure Active Directory Connect | Usługi synchronizacji usługi Azure Active Directory | Narzędzie do synchronizacji z usługą Azure Active Directory (DirSync) | Forefront Identity Manager 2010 R2 (FIM) |Microsoft Identity Manager 2016 (MIM)|
| :-------- |:--------:|:--------:|:--------:|:--------:|:--------:
| Synchronizacja haseł dla pojedynczego lokalnego lasu usługi AD | ● | ● | ● |  ||
| Synchronizacja haseł dla wielu lokalnych lasów usługi AD |  ●| ● |  |  ||
| Logowanie jednokrotne z federacją | ● | ● | ● | ● |● |
| Zapisywanie zwrotne haseł (z funkcji samoobsługowego resetowania hasła i zmiany hasła) |●  | ● |  |  ||



## Konfiguracja i instalacja

| Funkcja  | Azure Active Directory Connect  | Usługi synchronizacji usługi Azure Active Directory | Narzędzie do synchronizacji z usługą Azure Active Directory (DirSync) | Microsoft Identity Manager 2016 (MIM) |
| :-------- |:--------:|:--------:|:--------:|:--------:
| Obsługuje instalację na kontrolerze domeny | ● | ● | ● |  |
| Obsługuje instalację przy użyciu programu SQL Express | ● | ● | ● |  |
| Łatwe uaktualnienie z narzędzia DirSync |● |  |  |  |
| Lokalizacja środowiska użytkownika administratora na języki systemu Windows Server | ● | ● | ● |  |
|Lokalizacja środowiska użytkownika końcowego na języki systemu Windows Server| |  |  |● |
| Obsługa systemu Windows Server 2008 i Windows Server 2008 R2 | ● dla synchronizacji, nie dla federacji| ● | ●  | ● |
| Obsługa systemu Windows Server 2012 i Windows Server 2012 R2 | ● | ● | ● | ● |

## Filtrowanie i konfiguracja

Funkcja  | Azure Active Directory Connect | Usługi synchronizacji usługi Azure Active Directory | Narzędzie do synchronizacji z usługą Azure Active Directory (DirSync) | Forefront Identity Manager 2010 R2 (FIM)| Microsoft Identity Manager 2016 (MIM)
:-------- |:--------:|:--------:|:--------:|:--------:|:--------:|
Filtrowanie według domen i jednostek organizacyjnych | ● | ● | ● | ●  | ●
Filtrowanie według wartości atrybutów obiektów | ● | ● | ● | ●| ●
Umożliwia synchronizowanie minimalnego zestawu atrybutów (MinSync) | ● | ● |  ||
Umożliwia stosowanie różnych szablonów usług dla przepływów atrybutów |●  | ● |  ||
Umożliwia usuwanie atrybutów z przepływu z usługi AD do usługi Azure AD | ● | ● |  |  |
Umożliwia zaawansowane dostosowywanie przepływów atrybutów | ● | ● |  | ●  | ●

## Następne kroki
Dowiedz się więcej na temat [integrowania tożsamości lokalnych z usługą Azure Active Directory](active-directory-aadconnect.md).



<!--HONumber=Sep16_HO3-->


