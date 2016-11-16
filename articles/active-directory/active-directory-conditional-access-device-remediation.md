---
title: "Rozwiązywanie problemów z dostępem w usłudze Azure Active Directory | Microsoft Docs"
description: "Dowiedz się, jakie czynności możesz wykonać, aby rozwiązać problemy z dostępem do zasobów online Twojej organizacji."
services: active-directory
keywords: "dostęp warunkowy oparty na urządzeniach, rejestracja urządzenia, włączanie rejestracji urządzenia, rejestracja urządzenia i MDM"
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 8ad0156c-0812-4855-8563-6fbff6194174
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/11/2016
ms.author: markvi
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: fd2076f22c6048fda83d6da3b069e2805afb453f


---
# <a name="troubleshooting-for-azure-active-directory-access-issues"></a>Rozwiązywanie problemów z dostępem w usłudze Azure Active Directory
Próbujesz uzyskać dostęp do sieci intranet usługi SharePoint Online Twojej organizacji i otrzymujesz komunikat o błędzie „odmowa dostępu”. Co robisz?


W tym artykule opisano czynności pomocne w rozwiązywaniu problemów z dostępem do zasobów online organizacji.

Aby uzyskać pomoc dotyczącą rozwiązywania problemów z dostępem do usługi Azure Active Directory (Azure AD), przejdź do sekcji tego artykułu, która dotyczy platformy Twojego urządzenia:

* Urządzenie z systemem Windows
* Urządzenie z systemem iOS (zajrzyj tu wkrótce, aby uzyskać pomoc dotyczącą telefonu iPhone i tabletu iPad)
* Urządzenie z systemem Android (zajrzyj tu wkrótce, aby uzyskać pomoc dotyczącą telefonu i tabletu z systemem Android)

## <a name="access-from-a-windows-device"></a>Dostęp z urządzenia z systemem Windows
Jeśli na urządzeniu jest uruchomiona jedna z następujących platform, poszukaj w następnych sekcjach komunikatu o błędzie, który jest wyświetlany, gdy próbujesz uzyskać dostęp do aplikacji lub usługi:

* Windows 10
* Windows 8.1
* Windows 8
* Windows 7
* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2

### <a name="device-is-not-registered"></a>Urządzenie nie jest zarejestrowane
Jeśli urządzenie nie jest zarejestrowane w usłudze Azure AD, a aplikacja jest chroniona za pomocą zasad opartych na urządzeniach, może zostać wyświetlona strona zawierająca jeden z tych komunikatów o błędach:

![Komunikaty „Nie można dostać się tam z tego miejsca” dotyczące niezarejestrowanych urządzeń](./media/active-directory-conditional-access-device-remediation/01.png "Scenario")

Jeśli urządzenie jest przyłączone do domeny w usłudze Active Directory w Twojej organizacji, wypróbuj następujące rozwiązania:

1. Upewnij się, że logujesz się do systemu Windows przy użyciu Twojego konta służbowego (Twojego konta usługi Active Directory).
2. Połącz się z siecią firmową za pośrednictwem wirtualnej sieci prywatnej (VPN) lub funkcji DirectAccess.
3. Po nawiązaniu połączenia naciśnij klawisze logo Windows + L, aby zablokować sesję systemu Windows.
4. Wprowadź poświadczenia konta służbowego, aby odblokować sesję systemu Windows.
5. Zaczekaj chwilę, a następnie spróbuj ponownie, aby uzyskać dostęp do aplikacji lub usługi.
6. Jeśli zostanie wyświetlona ta sama strona, kliknij link **Więcej szczegółów**, a następnie skontaktuj się z administratorem i podaj mu szczegóły.

Jeśli urządzenie nie jest przyłączone do domeny i jest na nim uruchomiony system Windows 10, masz dwie możliwości:

* Uruchom funkcję Azure AD Join
* Dodaj swoje konto służbowe lub szkolne do systemu Windows

Aby uzyskać informacje o różnicach między tymi dwoma opcjami, zobacz [Korzystanie z urządzeń z systemem Windows 10 w miejscu pracy](active-directory-azureadjoin-windows10-devices.md).

Aby uruchomić funkcję Azure AD Join, wykonaj następujące czynności dla platformy, która działa na Twoim urządzeniu. (Funkcja Azure AD Join nie jest dostępna dla telefonów z systemem Windows).

**Rocznicowa aktualizacja systemu Windows 10**

1. Otwórz aplikację **Ustawienia**.
2. Kliknij pozycję **Konta** > **Dostęp w pracy lub szkole**.
3. Kliknij przycisk **Połącz**.
4. Kliknij pozycję **Dołącz to urządzenie do usługi Azure AD**.
5. Uwierzytelnij się w organizacji, jeśli zostanie wyświetlony monit, podaj dane uwierzytelniania wieloskładnikowego, a następnie wykonaj kolejne wyświetlane kroki.
6. Wyloguj się, a następnie zaloguj się przy użyciu swojego konta służbowego.
7. Spróbuj ponownie uzyskać dostęp do aplikacji.

**Windows 10 — aktualizacja z listopada 2015**

1. Otwórz aplikację **Ustawienia**.
2. Kliknij pozycję **System** > **Informacje**.
3. Kliknij pozycję **Przyłącz do usługi Azure AD**.
4. Uwierzytelnij się w organizacji, jeśli zostanie wyświetlony monit, podaj dane uwierzytelniania wieloskładnikowego, a następnie wykonaj kolejne wyświetlane kroki.
5. Wyloguj się, a następnie zaloguj się przy użyciu swojego konta służbowego (Twojego konta usługi Azure AD).
6. Spróbuj ponownie uzyskać dostęp do aplikacji.

Aby dodać swoje konto służbowe lub szkolne, wykonaj następujące czynności:

**Rocznicowa aktualizacja systemu Windows 10**

1. Otwórz aplikację **Ustawienia**.
2. Kliknij pozycję **Konta** > **Dostęp w pracy lub szkole**.
3. Kliknij przycisk **Połącz**.
4. Uwierzytelnij się w organizacji, jeśli zostanie wyświetlony monit, podaj dane uwierzytelniania wieloskładnikowego, a następnie wykonaj kolejne wyświetlane kroki.
5. Spróbuj ponownie uzyskać dostęp do aplikacji.

**Windows 10 — aktualizacja z listopada 2015**

1. Otwórz aplikację **Ustawienia**.
2. Kliknij pozycję **Konta** > **Twoje konta**.
3. Kliknij pozycję **Dodaj konto służbowe**.
4. Uwierzytelnij się w organizacji, jeśli zostanie wyświetlony monit, podaj dane uwierzytelniania wieloskładnikowego, a następnie wykonaj kolejne wyświetlane kroki.
5. Spróbuj ponownie uzyskać dostęp do aplikacji.

Jeśli urządzenie nie jest przyłączone do domeny i jest na nim uruchomiony system Windows 8.1, aby dołączyć to urządzenie do miejsca pracy i zarejestrować je w usłudze Microsoft Intune, wykonaj następujące czynności:

1. Otwórz okno **Ustawienia komputera**.
2. Kliknij pozycję **Sieć** > **Miejsce pracy**.
3. Kliknij pozycję **Dołącz**.
4. Uwierzytelnij się w organizacji, jeśli zostanie wyświetlony monit, podaj dane uwierzytelniania wieloskładnikowego, a następnie wykonaj kolejne wyświetlane kroki.
5. Kliknij pozycję **Włącz**.
6. Spróbuj ponownie uzyskać dostęp do aplikacji.

### <a name="browser-is-not-supported"></a>Przeglądarka nie jest obsługiwana
Problem odmowy dostępu możesz napotkać, jeśli próbujesz uzyskać dostęp do aplikacji lub usługi, korzystając z jednej z następujących przeglądarek:

* Chrome, Firefox lub dowolna inna przeglądarka, która nie jest przeglądarką Microsoft Edge ani Microsoft Internet Explorer w systemie Windows 10 lub Windows Server 2016.
* Firefox w systemie Windows 8.1, Windows 7, Windows Server 2012 R2, Windows Server 2012 lub Windows Server 2008 R2.

Zostanie wyświetlona strona błędu, która wygląda następująco:

![Komunikat „Nie można dostać się tam z tego miejsca” dotyczący nieobsługiwanych przeglądarek](./media/active-directory-conditional-access-device-remediation/02.png "Scenario")

Jedynym wyjściem jest korzystanie z przeglądarki obsługiwanej przez aplikację na danej platformie sprzętowej.

## <a name="next-steps"></a>Następne kroki
[Dostęp warunkowy do usługi Azure Active Directory](active-directory-conditional-access.md)




<!--HONumber=Nov16_HO2-->


