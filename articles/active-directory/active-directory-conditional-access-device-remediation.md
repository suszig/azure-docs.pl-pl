<properties
    pageTitle="Rozwiązywanie problemów: nie można dostać się tam z tego miejsca | Microsoft Azure"
    description="Ten temat ułatwia zidentyfikowanie czynności zaradczych, które można wykonać w celu uzyskania dostępu do aplikacji."
    services="active-directory"
    keywords="dostęp warunkowy oparty na urządzeniach, rejestracja urządzenia, włączanie rejestracji urządzenia, rejestracja urządzenia i MDM"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/23/2016"
    ms.author="markvi"/>


# Rozwiązywanie problemów: nie można dostać się tam z tego miejsca

Podczas uzyskiwania dostępu do aplikacji, na przykład SharePoint Online, napotykasz na odmowę dostępu do strony.  
Co zrobić w takiej sytuacji?

Ten przewodnik ułatwia zidentyfikowanie dostępnych czynności zaradczych, które można wykonać w celu uzyskania dostępu do aplikacji.



Z jakiej platformy sprzętowej korzysta Twoje urządzenie?
Odpowiedź na to pytanie określa odpowiednią sekcję w tym temacie:


-   Urządzenie z systemem Windows
-   Urządzenie z systemem iOS (iPhone lub iPad)
-   Urządzenie z systemem Android

## Dostęp z urządzenia z systemem Windows

Jeśli na Twoim urządzeniu jest uruchomiony system Windows 10, Windows 8.1, Windows 8.0, Windows 7, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 lub Windows Server 2008 R2, wybierz odpowiednią przyczynę, określając stronę, która jest wyświetlana przy próbie uzyskania dostępu do aplikacji.

### Urządzenie nie jest zarejestrowane

Jeśli urządzenie nie jest zarejestrowane w usłudze Azure Active Directory (Azure AD), a aplikacja jest chroniona za pomocą zasad opartych na urządzeniach, może zostać wyświetlona strona o następującej treści:

![Komunikaty „Nie można dostać się tam z tego miejsca” dotyczące niezarejestrowanych urządzeń](./media/active-directory-conditional-access-device-remediation/01.png "Scenario")



Jeśli urządzenie jest przyłączone do domeny w usłudze Active Directory w Twojej organizacji, możesz wypróbować następujące rozwiązania:

1.  Upewnij się, że logowanie do systemu Windows zostało przeprowadzone przy użyciu Twojego konta służbowego (konta usługi Active Directory).
2.  Połącz się z siecią firmową za pośrednictwem sieci VPN lub funkcji DirectAccess.
3.  Po nawiązaniu połączenia zablokuj sesję systemu Windows, używając klawiszy Windows + L.
4.  Odblokuj sesję systemu Windows, wprowadzając poświadczenia konta służbowego.
5.  Zaczekaj chwilę, a następnie spróbuj ponownie uzyskać dostęp do aplikacji.
6.  Jeśli zostanie wyświetlona ta sama strona, skontaktuj się z administratorem, kliknij link **Więcej szczegółów**, a następnie podaj szczegóły.

Jeśli urządzenie nie jest przyłączone do domeny i jest na nim uruchomiony system Windows 10, masz dwie możliwości:

- Uruchom funkcję Azure AD Join.
- Dodaj swoje konto służbowe lub szkolne do systemu Windows.

Aby uzyskać informacje o różnicach między tymi dwoma kontami, zobacz [Korzystanie z urządzeń z systemem Windows 10 w miejscu pracy](active-directory-azureadjoin-windows10-devices.md).

Aby uruchomić funkcję Azure AD Join, wykonaj następujące czynności (niedostępne w systemie Windows Phone):

**Aktualizacja Windows 10 Anniversary Update**

1.  Otwórz aplikację **Ustawienia**.
2.  Kliknij pozycję **Konta** > **Dostęp w pracy lub szkole**.
3.  Kliknij przycisk **Połącz**.
4.  Kliknij pozycję **Dołącz to urządzenie do usługi Azure AD** u dołu strony.
5.  Uwierzytelnij się w organizacji, w razie potrzeby podaj dowód uwierzytelniania wieloskładnikowego, a następnie wykonuj kolejne kroki aż do zakończenia.
6.  Wyloguj się, a następnie zaloguj się przy użyciu swojego konta służbowego.
7.  Spróbuj ponownie uzyskać dostęp do aplikacji.




**Windows 10 — aktualizacja z listopada 2015**


1.  Otwórz aplikację **Ustawienia**.
2.  Kliknij pozycję **System** > **Informacje**.
3.  Kliknij pozycję **Przyłącz do usługi Azure AD**.
4.  Uwierzytelnij się w organizacji, w razie potrzeby podaj dowód uwierzytelniania wieloskładnikowego, a następnie wykonuj kolejne kroki aż do zakończenia.
5.  Wyloguj się, a następnie zaloguj się przy użyciu swojego konta służbowego (konta usługi Azure AD).
6.  Spróbuj ponownie uzyskać dostęp do aplikacji.

Aby dodać swoje konto służbowe lub szkolne, wykonaj następujące czynności:

**Aktualizacja Windows 10 Anniversary Update**

1.  Otwórz aplikację **Ustawienia**.
2.  Kliknij pozycję **Konta** > **Dostęp w pracy lub szkole**.
3.  Kliknij przycisk **Połącz**.
4.  Uwierzytelnij się w organizacji, w razie potrzeby podaj dowód uwierzytelniania wieloskładnikowego, a następnie wykonuj kolejne kroki aż do zakończenia.
5.  Spróbuj ponownie uzyskać dostęp do aplikacji.


**Windows 10 — aktualizacja z listopada 2015**

1.  Otwórz aplikację **Ustawienia**.
2.  Kliknij pozycję **Konta** > **Twoje konta**.
3.  Kliknij pozycję **Dodaj konto służbowe**.
4.  Uwierzytelnij się w organizacji, w razie potrzeby podaj dowód uwierzytelniania wieloskładnikowego, a następnie wykonuj kolejne kroki aż do zakończenia.
5.  Spróbuj ponownie uzyskać dostęp do aplikacji.

Jeśli urządzenie nie jest przyłączone do domeny i jest na nim uruchomiony system Windows 8.1, możesz dołączyć to urządzenie do miejsca pracy i zarejestrować je w usłudze Microsoft Intune, wykonując następujące czynności:

1.  Otwórz okno **Ustawienia komputera**.
2.  Kliknij pozycję **Sieć** > **Miejsce pracy**.
3.  Kliknij pozycję **Dołącz**.
4.  Uwierzytelnij się w organizacji, w razie potrzeby podaj dowód uwierzytelniania wieloskładnikowego, a następnie wykonuj kolejne kroki aż do zakończenia.
5.  Kliknij pozycję **Włącz**.
6.  Zaczekaj na zakończenie.
7.  Spróbuj ponownie uzyskać dostęp do aplikacji.


## Nieobsługiwana przeglądarka

Jeśli próbujesz uzyskać dostęp do aplikacji z następujących przeglądarek, zostanie wyświetlona strona podobna do pokazanej wcześniej:

- Chrome, Firefox lub dowolna inna przeglądarka, która nie jest przeglądarką Microsoft Edge ani Microsoft Internet Explorer w systemie Windows 10 lub Windows Server 2016.
- Firefox w systemie Windows 8.1, Windows 7, Windows Server 2012 R2, Windows Server 2012 lub Windows Server 2008 R2.

![Komunikat „Nie można dostać się tam z tego miejsca” dotyczący nieobsługiwanych przeglądarek](./media/active-directory-conditional-access-device-remediation/02.png "Scenario")


Jedynym wyjściem jest korzystanie z przeglądarki obsługiwanej przez aplikację na danej platformie sprzętowej.

## Dostęp z urządzenia z systemem iOS
Instrukcje dotyczące telefonów iPhone i tabletów iPad pojawią się wkrótce.

## Dostęp z urządzenia z systemem Android
Instrukcje dotyczące telefonów i tabletów z systemem Android pojawią się wkrótce.

## Następne kroki

[Dostęp warunkowy do usługi Azure Active Directory](active-directory-conditional-access.md)



<!--HONumber=sep16_HO1-->


