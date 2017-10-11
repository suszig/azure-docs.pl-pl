---
title: "Zabezpieczanie zasobów Azure CDN z tokenu uwierzytelniania | Dokumentacja firmy Microsoft"
description: "Bezpieczny dostęp do zasobów usługi Azure CDN przy użyciu tokenu uwierzytelniania."
services: cdn
documentationcenter: .net
author: zhangmanling
manager: zhangmanling
editor: 
ms.assetid: 837018e3-03e6-4f9c-a23e-4b63d5707a64
ms.service: cdn
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 11/11/2016
ms.author: mezha
ms.openlocfilehash: 42b182c314795b1ebf69639ec7ac5583208dc7c1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="securing-azure-cdn-assets-with-token-authentication"></a>Zabezpieczanie zasobów Azure CDN z tokenu uwierzytelniania

[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

##<a name="overview"></a>Omówienie

Token uwierzytelniania jest mechanizm, który pozwala zapobiec obsługująca zasoby do nieautoryzowanego klientów usługi Azure CDN.  Jest to zazwyczaj wykonywane zapobiegające "hotlinking" zawartości, w których inną witrynę sieci Web, często tablicy komunikat, używa zasobów bez uprawnienia.  Może to mieć wpływ na koszty dostarczania zawartości. Przez włączenie tej funkcji w sieci CDN w warstwie, żądania są uwierzytelniane przez krawędź CDN POP przed dostarczeniem zawartości. 

## <a name="how-it-works"></a>Jak to działa

Token uwierzytelniania sprawdza, czy żądania są generowane przez zaufanych witryn, gdyż żądania zawiera wartość tokenu zawierające zakodowanego informacji na temat obiektu żądającego. Zawartość tylko będzie obsługiwana żądającej, gdy dane zakodowane spełniają wymagania, w przeciwnym razie żądania zostaną odrzucone. Można skonfigurować wymaganie przy użyciu jednego lub wielu parametrów poniżej.

- Kraju: akceptować lub odrzucać żądania, które pochodzą z określonego krajów.  [Lista poprawnych kodów kraju.](https://msdn.microsoft.com/library/mt761717.aspx) 
- Adres URL: Zezwalaj tylko na określonym zasobie lub ścieżka do żądania.  
- Host: akceptować lub odrzucać żądania przy użyciu określonych hostach w nagłówku żądania.
- Odwołania: akceptować lub odrzucać określonego odwołania do żądania.
- Adres IP: Zezwalaj tylko na żądania, które pochodzą z określonego adresu IP lub podsieci IP.
- Protokół: zezwala lub blokuje żądania na podstawie protokołu używane do żądania zawartości.
- Czas wygaśnięcia: przypisywanie okres daty i godziny, aby upewnić się, że łącze tylko pozostaje ważny przez pewien czas.

Zobacz przykład szczegółowej konfiguracji poszczególnych parametrów.

## <a name="reference-architecture"></a>Architektura odwołań

Wymienione poniżej architektura referencyjna, konfigurowania token uwierzytelniania w sieci CDN do pracy z aplikacji sieci Web.

![Przycisk Zarządzaj blok profilu CDN](./media/cdn-token-auth/cdn-token-auth-workflow2.png)

## <a name="token-validation-logic-on-cdn-endpoint"></a>Logikę weryfikacji tokenu na punkt końcowy CDN
    
Ten wykres opisano, jak Azure CDN weryfikuje żądanie klienta, gdy token uwierzytelniania jest skonfigurowane na punkt końcowy CDN.

![Przycisk Zarządzaj blok profilu CDN](./media/cdn-token-auth/cdn-token-auth-validation-logic.png)

## <a name="setting-up-token-authentication"></a>Konfigurowanie uwierzytelniania tokenu

1. Z [portalu Azure](https://portal.azure.com), przejdź do swojego profilu CDN, a następnie kliknij przycisk **Zarządzaj** przycisk, aby uruchomić portalu dodatkowym.

    ![Przycisk Zarządzaj blok profilu CDN](./media/cdn-rules-engine/cdn-manage-btn.png)

2. Umieść kursor nad **HTTP dużych**, a następnie kliknij przycisk **Token uwierzytelniania** w wysuwane okno. Zostanie skonfigurowany klucz szyfrowania i szyfrowania parametrów na tej karcie.

    1. Wprowadź unikatowy klucz szyfrowania dla **klucz podstawowy**.  Wprowadź inny dla **kopii zapasowej klucza**

        ![Klucz tokenu uwierzytelniania CDN](./media/cdn-token-auth/cdn-token-auth-setupkey.png)
    
    2. Ustawianie parametrów szyfrowania za pomocą narzędzia szyfrowania (akceptować lub odrzucać żądania oparte na czas wygaśnięcia, kraj, odwołania, protokołu, adresu IP klienta. Można użyć dowolnej kombinacji.)

        ![Przycisk Zarządzaj blok profilu CDN](./media/cdn-token-auth/cdn-token-auth-encrypttool.png)

        - WE-wygaśnie: przypisuje czas wygaśnięcia tokenu po upływie określonego czasu. Żądań przesłanych po czas wygaśnięcia będą odrzucane. Sygnatura czasowa Unix używa tego parametru (oparte na sekund od standardowego epoka 1/1/1970 GMT 00:00:00. Służy online narzędzia zapewnienie konwersji między czasem Unix a (czas standardowy).)  Na przykład, jeśli chcesz skonfigurować token wygasło na 12/31 grudnia 2016 r. 12:00:00 GMT, użyj czasu Unix: 1483185600 zgodnie z poniższymi instrukcjami:
    
        ![Przycisk Zarządzaj blok profilu CDN](./media/cdn-token-auth/cdn-token-auth-expire2.png)
    
        - Zezwalaj adresu url WE: pozwala dostosować tokenów do określonego zasobu lub ścieżki. Go ogranicza dostęp do żądania, którego adres URL rozpoczynać się od określonej ścieżki względnej. Można wprowadzić wiele ścieżek, oddzielając każda ścieżka przecinkami. Adresy URL jest rozróżniana wielkość liter. W zależności od wymagań można skonfigurować inną wartość, aby zapewnić różne poziomy dostępu. Poniżej przedstawiono kilka scenariuszy:
        
            Jeśli adres URL: http://www.mydomain.com/pictures/city/strasbourg.png. Zobacz wartości wejściowej "" i jego dostęp na poziomie odpowiednio

            1. Wartość wejściowa "/": wszystkie żądania będą dozwolone
            2. Wartość wejściowa "/ obrazy": następujące żądania będą Zezwalaj
            
                - http://www.myDomain.com/Pictures.PNG
                - http://www.myDomain.com/Pictures/City/Strasbourg.PNG
                - http://www.myDomain.com/picturesnew/City/strasbourgh.PNG
            3. Wprowadź wartość "/ obrazy /": tylko żądania dla /pictures/ będą dozwolone
            4. Wartość wejściowa "/ pictures/city/strasbourg.png": tylko zezwala na żądania dla tego zasobu
    
        ![Przycisk Zarządzaj blok profilu CDN](./media/cdn-token-auth/cdn-token-auth-url-allow4.png)
    
        - Zezwalaj kraju WE: zezwala tylko na żądania, które pochodzą z co najmniej jeden określony krajów. Żądania, które pochodzą z innych krajów zostaną odrzucone. Umożliwia ustawienie parametrów numer kierunkowy kraju i oddzielając każdy kod kraju przecinkami. Na przykład jeśli chcesz zezwolić na dostęp w Stanach Zjednoczonych i Francja wejściowe nam FR w kolumnie jako poniżej.  
        
        ![Przycisk Zarządzaj blok profilu CDN](./media/cdn-token-auth/cdn-token-auth-country-allow.png)

        - Odmów kraju WE: odrzucanie żądań pochodzących z jednego lub więcej określonych krajów. Może być żądań pochodzących z innych krajów. Umożliwia ustawienie parametrów numer kierunkowy kraju i oddzielając każdy kod kraju przecinkami. Na przykład jeśli chcesz odmówić dostępu w Stanach Zjednoczonych i Francja wejściowe nam FR w kolumnie.
    
        - Zezwalaj ref WE: tylko zezwala na żądania pochodzące z określonej odwołania. Odwołania określa adres URL strony sieci web, z którym powiązany żądanych zasobów. Wartość parametru odwołania nie powinien zawierać nazwę protokołu. Można wprowadzić nazwę hosta i/lub określonej ścieżki na tej nazwy hosta. Można również dodać wiele odwołań w jeden parametr, oddzielając każdy z nich przecinkami. Jeśli podano wartość odwołania, ale informacje odwołania nie są wysyłane w żądania z powodu konfiguracji niektóre przeglądarki, te żądania zostaną odrzucone domyślnie. Można przypisać "Brak" lub wartość pusta w parametrze, aby zezwolić na te żądania z brakującymi informacji odwołania. Można również użyć "*. consoto.com" umożliwia wszystkich poddomen consoto.com.  Na przykład jeśli chcesz zezwolić na dostęp dla żądań z www.consoto.com, wszystkimi domenami podrzędnymi w consoto2.com i erquests z reffers puste lub nie została podana wartość wejściowa jest poniżej:
        
        ![Przycisk Zarządzaj blok profilu CDN](./media/cdn-token-auth/cdn-token-auth-referrer-allow2.png)
    
        - Odmów ref WE: odrzucanie żądań z określonego odwołania. Zobacz szczegółowe informacje i przykład w parametrze "WE ref — Zezwalaj".
         
        - Zezwalaj proto WE: zezwala tylko na żądania od wybranego protokołu. Na przykład http lub https.
        
        ![Przycisk Zarządzaj blok profilu CDN](./media/cdn-token-auth/cdn-token-auth-url-allow4.png)
            
        - Odmów proto WE: odrzucanie żądań z wybranego protokołu. Na przykład http lub https.
    
        - we clientip: ogranicza dostęp do adresu IP określonego żądającego. Obsługiwane są protokoły IPV4 i IPV6. Można określić pojedyncze żądanie adresu IP lub podsieci IP.
            
        ![Przycisk Zarządzaj blok profilu CDN](./media/cdn-token-auth/cdn-token-auth-clientip.png)
        
    3. Można sprawdzić za pomocą narzędzia wywieranych przez token.

    4. Można również dostosować typ odpowiedzi, który zostanie zwrócony do użytkownika, gdy żądanie zostanie odrzucone. Domyślnie używamy 403.

3. Teraz kliknij **aparatu reguł** w obszarze **HTTP dużych**. Użyje na tej karcie można zdefiniować ścieżki, aby zastosować funkcję, włączyć funkcję uwierzytelniania tokenu i włączyć możliwości związane z dodatkowego uwierzytelniania tokenu.

    - Kolumna "IF" służy do definiowania zasobów lub ścieżkę, którą chcesz zastosować token uwierzytelniania. 
    - Kliknij, aby dodać "Token uwierzytelniania" z listy rozwijanej funkcji, aby włączyć uwierzytelnianie tokena.
        
    ![Przycisk Zarządzaj blok profilu CDN](./media/cdn-token-auth/cdn-rules-engine-enable2.png)

4. W **aparatu reguł** jest kilka dodatkowych możliwości, możesz włączyć.
    
    - Token uwierzytelniania odmowa kodu: Określa typ odpowiedzi, który zostanie zwrócony do użytkownika, gdy żądanie zostanie odrzucone. Reguł ustawionych w tym miejscu zastępuje ustawienie kodu odmowa na karcie tokenu uwierzytelniania.
    - Ignoruj token uwierzytelniania: Określa, czy adres URL używany do sprawdzania poprawności tokenu będzie uwzględniana wielkość liter.
    - Parametr tokenu uwierzytelniania: Zmień nazwę parametru ciągu zapytania token uwierzytelniania przedstawiający w żądanym adresie URL. 
        
    ![Przycisk Zarządzaj blok profilu CDN](./media/cdn-token-auth/cdn-rules-engine2.png)

5. Można dostosować z tokenem, który jest aplikacja, która generuje token dla funkcji uwierzytelniania opartego na tokenie. Kod źródłowy jest dostępny w tym miejscu w [GitHub](https://github.com/VerizonDigital/ectoken).
Dostępne języki:
    
    - C
    - C#
    - PHP
    - Perl
    - Java
    - Python    


## <a name="azure-cdn-features-and-provider-pricing"></a>Azure CDN funkcji i cenach dostawcy

Zobacz [Omówienie usługi CDN](cdn-overview.md) tematu.
