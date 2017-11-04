---
title: "Azure przewodnik rozwiązywania problemów z Eksploratora usługi Storage | Dokumentacja firmy Microsoft"
description: "Omówienie dwóch debugowanie funkcji platformy Azure"
services: virtual-machines
documentationcenter: 
author: Deland-Han
manager: cshepard
editor: 
ms.assetid: 
ms.service: virtual-machines
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 09/08/2017
ms.author: delhan
ms.openlocfilehash: e06c73c2c00b27178f8431b83b5c5a42110b6b1e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-storage-explorer-troubleshooting-guide"></a>Podręczniku rozwiązywania problemów z Eksploratora usługi Storage platformy Azure

Eksploratora usługi Microsoft Azure Storage (wersja zapoznawcza) jest autonomiczną aplikację, która pozwala łatwo pracować z danymi usługi Azure Storage w systemie Windows, system macOS i Linux. Aplikacja może nawiązać hostowanych na Azure, suwerenne chmur i stosu Azure kont magazynu.

Ten przewodnik zawiera podsumowanie rozwiązania dla typowe problemy występujące w Eksploratorze usługi Storage.

## <a name="sign-in-issues"></a>Problemy z rejestracją

Obsługiwane są tylko konta usługi Azure Active Directory (AAD). Jeśli używasz konta usług AD FS, oczekuje się, że logowanie do Eksploratora usługi Storage nie będzie działać. Przed kontynuowaniem, spróbuj ponownie uruchomić aplikację i zobacz, czy należy rozwiązać problemy.

### <a name="error-self-signed-certificate-in-certificate-chain"></a>Błąd: Certyfikat z podpisem własnym w łańcuchu certyfikatów

Istnieje kilka przyczyn, dlaczego błąd może się pojawić i najbardziej typowych przyczyn dwóch są następujące:

1. Aplikacja jest połączony za pośrednictwem "przezroczystego obiektu pośredniczącego", co oznacza przechwycenia ruchu HTTPS, odszyfrowywania go i następnie szyfrowania za pomocą certyfikatu z podpisem własnym serwera (takich jak serwer firmy).

2. Używasz aplikacji, takich jak oprogramowanie antywirusowe, wprowadza certyfikatu SSL z podpisem własnym do otrzymywanych wiadomości protokołu HTTPS.

Gdy Eksploratora usługi Storage napotka jeden z problemów, go już nie wiadomo, czy została naruszona odebranego komunikatu protokołu HTTPS. Jeśli masz kopię certyfikatu z podpisem własnym, możesz pozwolić, Eksploratora usługi Storage zaufania temu certyfikatowi. Jeśli nie wiesz, kto jest wprowadzenia certyfikat, wykonaj następujące kroki, znajdź go:

1. Zainstaluj Otwórz protokołu SSL

    - [Windows](https://slproweb.com/products/Win32OpenSSL.html) (wersje światła powinno wystarczyć)

    - Mac i Linux: powinien być dołączony do systemu operacyjnego

2. Uruchom Otwórz protokołu SSL

    - Windows: Otwórz katalog instalacyjny, kliknij przycisk **/bin/**, a następnie kliknij dwukrotnie **openssl.exe**.
    - Mac i Linux: Uruchom **openssl** z terminalu.

3. Wykonanie s_client - showcerts-Połącz microsoft.com:443

4. Poszukaj certyfikaty z podpisem własnym. Jeśli nie wiesz, które są z podpisem własnym, poszukaj gdziekolwiek podmiotu ("s:") i wystawcy ("i") są takie same.

5. Po odnalezieniu wszelkie certyfikaty z podpisem własnym dla każdego z nich, skopiować i wkleić wszystko z tym **---BEGIN CERTIFICATE---** do **---END CERTIFICATE---** do nowego pliku .cer.

6. Otwórz Eksploratora usługi Storage, kliknij pozycję **Edytuj** > **certyfikaty SSL** > **importu certyfikatów**, a następnie użyj selektora plików, aby znaleźć, wybrać, i Otwieranie plików .cer, które zostały utworzone.

Jeśli nie można znaleźć żadnych certyfikatów z podpisem własnym za pomocą powyższych kroków, skontaktuj się z nami za pomocą narzędzia opinii, aby uzyskać dalszą pomoc.

### <a name="unable-to-retrieve-subscriptions"></a>Nie można pobrać subskrypcji

Jeśli nie można pobrać subskrypcji, po pomyślnym zalogowaniu, wykonaj następujące kroki, aby rozwiązać ten problem:

- Sprawdź, czy Twoje konto ma dostęp do subskrypcji, logując się do portalu Azure.

- Upewnij się, że zalogowano się za pomocą poprawne środowisko (Azure, chińskiej wersji platformy Azure, platformy Azure w Niemczech, Azure instytucji rządowych Stanów Zjednoczonych lub niestandardowe stosu środowiska/Azure).

- Jeśli używasz serwera proxy, upewnij się, poprawnie skonfigurowany serwer proxy Eksploratora usługi Storage.

- Spróbuj usunąć i ponowne dodawanie konta.

- Spróbuj usuwanie następujących plików z katalogu głównego (czyli C:\Users\ContosoUser), a następnie ponowne dodanie konta:

    - .adalcache

    - .devaccounts

    - .extaccounts

- Obejrzyj narzędzi deweloperskich konsoli (przez naciśnięcie klawisza F12) logujesz dla komunikatów o błędach:

![Narzędzia dla deweloperów](./media/storage-explorer-troubleshooting/4022501_en_2.png)

### <a name="unable-to-see-the-authentication-page"></a>Nie można znaleźć na stronie uwierzytelniania

Jeśli nie można znaleźć na stronie uwierzytelniania, wykonaj następujące kroki, aby rozwiązać ten problem:

- W zależności od prędkości połączenia może upłynąć trochę czasu zanim strony logowania załadować, poczekaj co najmniej jedną minutę przed zamknięciem okna dialogowego uwierzytelniania.

- Jeśli używasz serwera proxy, upewnij się, poprawnie skonfigurowany serwer proxy Eksploratora usługi Storage.

- Naciśnięcie klawisza F12, aby wyświetlić konsoli dla deweloperów. Obejrzyj odpowiedzi z konsoli dla deweloperów i zobacz, czy można znaleźć żadnych clue Dlaczego uwierzytelniania nie działa.

### <a name="cannot-remove-account"></a>Nie można usunąć konta

Jeśli nie można usunąć konta lub łącze Uwierzytelnij ponownie wykonywać żadnych czynności, wykonaj następujące kroki, aby rozwiązać ten problem:

- Spróbuj usuwanie następujących plików z katalogu głównego, a następnie ponowne dodawanie konta:

    - .adalcache

    - .devaccounts

    - .extaccounts

- Jeśli chcesz usunąć SAS dołączony zasobów magazynu, Usuń następujące pliki:

    - Folder %appdata%/StorageExplorer dla systemu Windows

    - /Users/ < twoje_imie >/biblioteki/komputerze Obsługa/StorageExplorer dla komputerów Mac

    - ~/.config/StorageExplorer dla systemu Linux

> [!NOTE]
>  Należy ponownie wprowadzić swoje poświadczenia, jeśli te pliki zostaną usunięte.

## <a name="proxy-issues"></a>Problemy z serwera proxy

Najpierw upewnij się, że wszystkie poprawne są następujące wprowadzone informacje:

- Adres URL serwera proxy i numer portu

- Nazwa użytkownika i hasło, jeśli jest to wymagane przez serwer proxy

### <a name="common-solutions"></a>Typowe rozwiązania

Jeśli nadal występują problemy, wykonaj następujące kroki, aby je rozwiązać:

- Jeśli można połączyć się przez Internet, bez użycia serwera proxy, sprawdź, czy Eksploratora usługi Storage działa bez włączone ustawienia serwera proxy. Jeśli tak jest, może to być problem z ustawieniami serwera proxy. Skontaktować się z administratorem serwera proxy, aby zidentyfikować problemy.

- Sprawdź, czy inne aplikacje korzystające z serwera proxy działają zgodnie z oczekiwaniami.

- Sprawdź, czy masz połączenie z portalem Microsoft Azure za pomocą przeglądarki sieci web

- Sprawdź, czy możesz odbierać odpowiedzi z punktami końcowymi usługi. Wprowadź jeden z adresami URL punktu końcowego w przeglądarce. Jeśli można połączyć, powinien zostać wyświetlony InvalidQueryParameterValue lub podobne odpowiedzi XML.

- Jeśli ktoś inny korzysta również Eksploratora usługi Storage z serwerem proxy, sprawdź, czy można połączyć. Jeśli można się połączyć, należy skontaktować się z administratorem serwera proxy

### <a name="tools-for-diagnosing-issues"></a>Narzędzia do diagnozowania problemów

Jeśli masz sieci narzędzi, takich jak Fiddler dla systemu Windows, można zdiagnozować problemy w następujący sposób:

- Masz pracy za pośrednictwem serwera proxy, należy skonfigurować narzędzie sieci na łączenie się za pośrednictwem serwera proxy.

- Sprawdź numer portu używanego przez narzędzie do sieci.

- Wprowadź adres URL lokalnego hosta i numer portu sieci narzędzie zgodnie z ustawieniami serwera proxy w Eksploratorze usługi Storage. Jeśli odbywa się prawidłowo, narzędzie sieci uruchamia rejestrowanie żądań sieci wprowadzone przez Eksploratora usługi Storage, zarządzania i punktów końcowych usługi. Na przykład wprowadź https://cawablobgrs.blob.core.windows.net/ dla punktu końcowego obiektu blob w przeglądarce, a otrzymasz odpowiedź jest podobny do następującego, które sugeruje zasób istnieje, mimo że nie można do niego dostęp.

![Przykładowy kod](./media/storage-explorer-troubleshooting/4022502_en_2.png)

### <a name="contact-proxy-server-admin"></a>Skontaktuj się z administratorem serwera proxy

Jeśli ustawienia serwera proxy są prawidłowe, należy skontaktować się z administratorem serwera proxy i

- Upewnij się, że serwer proxy blokuje ruch do punkty końcowe systemu Azure zarządzania lub zasobu.

- Sprawdź protokół uwierzytelniania używany przez serwer proxy. Eksplorator usługi Storage aktualnie nie obsługuje serwerów proxy NTLM.

## <a name="unable-to-retrieve-children-error-message"></a>"Nie można pobrać elementów podrzędnych" komunikat o błędzie

Jeśli na platformie Azure są połączone za pośrednictwem serwera proxy, sprawdź, czy ustawienia serwera proxy są poprawne. Jeśli od właściciela subskrypcji lub konta przyznano dostęp do zasobu, sprawdź, czy znasz, lub listę uprawnień dla tego zasobu.

### <a name="issues-with-sas-url"></a>Problemy z adresem URL SAS
Jeśli łączysz się z usługą przy użyciu adresu URL SAS i występuje błąd:

- Upewnij się, że adres URL zawiera wystarczających uprawnień do odczytu lub listy zasobów.

- Upewnij się, że adres URL nie wygasł.

- Jeśli adres URL SAS jest oparta na zasadach dostępu, sprawdź, czy zasady dostępu nie został odwołany.

## <a name="next-steps"></a>Następne kroki

Jeśli te rozwiązania nie działają dla Ciebie, przesłać problem za pomocą narzędzia opinii z poczty e-mail i szczegółów o problemie uwzględnione podczas można tak, aby firma Microsoft kontaktu z Tobą w celu rozwiązania problemu.

Aby to zrobić, kliknij przycisk **pomocy** menu, a następnie kliknij przycisk **Prześlij opinię**.

![Opinia](./media/storage-explorer-troubleshooting/4022503_en_1.png)
