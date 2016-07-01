<properties
    pageTitle="Dodawanie niestandardowej nazwy domeny do usługi Azure Active Directory | Microsoft Azure"
    description="Jak dodać nazwy domeny firmy do usługi Azure Active Directory oraz jak zweryfikować nazwę domeny."
    services="active-directory"
    documentationCenter=""
    authors="jeffsta"
    manager="stevenpo"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="04/20/2016"
    ms.author="curtand;jeffsta"/>

# Dodawanie niestandardowej nazwy domeny do usługi Azure Active Directory

Masz co najmniej jedną nazwę domeny, której organizacja używa do prowadzenia działalności, a użytkownicy logują się do sieci firmowej przy użyciu nazwy domeny firmowej. Teraz, gdy używasz usługi Azure Active Directory (Azure AD), możesz dodać nazwę domeny firmowej również do usługi Azure AD. Dzięki temu można w katalogu przypisać nazwy użytkowników znajome dla użytkowników, takie jak „alicja@contoso.com”. Proces jest prosty:

- Dodaj nazwę domeny w naszym kreatorze **Dodaj domenę** w klasycznym portalu Azure.

- Pobierz wpis DNS w klasycznym portalu usługi Azure AD lub narzędziu Azure AD Connect.

- Dodaj wpis DNS dla nazwy domeny do pliku strefy DNS w witrynie sieci Web dla rejestratora DNS.

- Zweryfikuj nazwę domeny w klasycznym portalu usługi Azure AD lub narzędziu Azure AD Connect.


Dopóki niestandardowa nazwa domeny nie zostanie zweryfikowana, użytkownicy muszą logować się przy użyciu nazw użytkownika takich jak „alicja@contoso.onmicrosoft.com”, korzystających z początkowej nazwy domeny dla katalogu. Jeśli potrzebujesz wielu niestandardowych nazw domen, takich jak „contoso.com” czy „contosobank.com”, możesz je dodać (maksymalnie 900 nazw domen). Dla każdej dodawanej nazwy domeny wykonaj kroki opisane w tym artykule.

## Dodawanie niestandardowej nazwy domeny do katalogu

1. Zaloguj się do [klasycznego portalu Azure](https://manage.windowsazure.com/) przy użyciu konta użytkownika będącego administratorem globalnym katalogu usługi Azure AD.

2. W usłudze **Active Directory** otwórz katalog i wybierz kartę **Domeny**.

3. Na pasku poleceń wybierz pozycję **Dodaj**, a następnie wprowadź niestandardową nazwę domeny, taką jak „contoso.com”. Należy uwzględnić rozszerzenie .com, .net lub inne rozszerzenie najwyższego poziomu.

4. Jeśli zamierzasz skonfigurować tę domenę na potrzeby [logowania federacyjnego](https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Configuring-AD-FS-for-user-sign-in-with-Azure-AD-Connect) za pomocą lokalnej usługi Active Directory, zaznacz to pole wyboru.

5. Wybierz pozycję **Dodaj**.

Teraz, po dodaniu nazwy domeny, usługa Azure AD musi sprawdzić, czy organizacja jest właścicielem nazwy domeny. Aby usługa Azure AD mogła przeprowadzić tę weryfikację, należy dodać wpis DNS w pliku strefy DNS dla nazwy domeny. To zadanie jest wykonywane w witrynie sieci Web u rejestratora nazw domen dla nazwy domeny.

## Pobieranie wpisów DNS dla nazwy domeny

Wpisy DNS znajdują się na drugiej stronie kreatora **Dodaj domenę**, jeśli nie wybrano federowania z lokalną usługą Active Directory systemu Windows Server.

W przypadku konfigurowania domeny na potrzeby federowania nastąpi przekierowanie do pobierania narzędzia Azure AD Connect. Uruchom narzędzie Azure AD Connect, aby [uzyskać wpisy DNS, które należy dodać u rejestratora nazw domen](active-directory-aadconnect-get-started-custom.md#verify-the-azure-ad-domain-selected-for-federation). Narzędzie Azure AD Connect zweryfikuje także nazwę domeny dla usługi Azure AD.

## Dodawanie wpisu DNS do pliku strefy DNS

1.  Zaloguj się do rejestratora nazw domen dla domeny. Jeśli nie masz wystarczających uprawnień do zaktualizowania wpisu DNS, poproś osobę lub zespół posiadający taki dostęp o dodanie wpisu DNS.

2.  Zaktualizuj plik strefy DNS dla domeny, dodając wpis DNS udostępniony przez usługę Azure AD. Ten wpis DNS umożliwia usłudze Azure AD zweryfikowanie prawa własności do domeny. Ten wpis DNS nie zmienia żadnych zachowań, takich jak routing poczty lub hosting sieci Web. Propagacja rekordów DNS może potrwać do godziny.

[Instrukcje dotyczące dodawania wpisu DNS u popularnych rejestratorów DNS](https://support.office.com/article/Create-DNS-records-for-Office-365-when-you-manage-your-DNS-records-b0f3fdca-8a80-4e8e-9ef3-61e8a2a9ab23/)

## Weryfikowanie nazwy domeny w usłudze Azure AD

Po dodaniu wpisu DNS należy się upewnić, że nazwa domeny jest zweryfikowana przez usługę Azure AD. Jest to ostatni krok do osiągnięcia sukcesu.

Jeśli nadal masz otwarty kreator **Dodaj domenę**, wybierz opcję **Weryfikuj** na trzeciej stronie kreatora. Przed zweryfikowaniem należy poczekać do godziny na wykonanie propagacji wpisu DNS.

Jeśli kreator **Dodaj domenę** nie jest nadal otwarty, można zweryfikować domenę w [klasycznym portalu Azure](https://manage.windowsazure.com/):

1.  Zaloguj się przy użyciu konta użytkownika będącego administratorem globalnym katalogu usługi Azure AD.

2.  Otwórz katalog i wybierz kartę **Domeny**.

3.  Wybierz domenę, którą chcesz zweryfikować.

4.  Na pasku poleceń wybierz pozycję **Weryfikuj**, a następnie wybierz polecenie **Weryfikuj** w oknie dialogowym.

Gratulujemy sukcesu! Teraz możesz [przypisać nazwy użytkowników, które zawierają niestandardową nazwę domeny](active-directory-add-domain-add-users.md). W razie problemów ze zweryfikowaniem nazwy domeny zobacz sekcję [Rozwiązywanie problemów](#troubleshooting).

## Rozwiązywanie problemów
Jeśli nie można zweryfikować niestandardowej nazwy domeny, istnieje kilka możliwych przyczyn. Zaczniemy od najczęstszych i skończymy na najrzadziej występujących.

- Próba zweryfikowania nazwy domeny została podjęta przed ukończeniem propagacji wpisu DNS. Poczekaj chwilę i spróbuj ponownie.

- Rekord DNS nie został w ogóle wprowadzony. Sprawdź wpis DNS i poczekaj na jego propagację, a następnie spróbuj ponownie.

- Nazwa domeny została już zweryfikowana w innym katalogu. Znajdź nazwę domeny i usuń ją z innego katalogu, a następnie spróbuj ponownie.

- Rekord DNS zawiera błąd. Popraw błąd i spróbuj ponownie.

- Nie masz wystarczających uprawnień do aktualizowania rekordów DNS. Udostępnij wpisy DNS osobie lub zespołowi w organizacji, którzy mają odpowiedni dostęp, i poproś o dodanie wpisu DNS.


## Następne kroki

-   [Przypisywanie nazw użytkowników, które zawierają niestandardową nazwę domeny](active-directory-add-domain-add-users.md)
-   [Zarządzanie niestandardowymi nazwami domen](active-directory-add-manage-domain-names.md)
-   [Zapoznanie z koncepcjami związanymi z zarządzaniem domenami w usłudze Azure AD](active-directory-add-domain-concepts.md)
-   [Wyświetlanie znakowania firmowego podczas logowania użytkowników](active-directory-add-company-branding.md)
-   [Zarządzanie nazwami domen w usłudze Azure AD przy użyciu programu PowerShell](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)



<!--HONumber=Jun16_HO2-->


