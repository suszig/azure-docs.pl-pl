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
ms.openlocfilehash: 88956b324a543c5347e16b1278f6b2179a3b9c24
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/03/2017
---
# <a name="securing-azure-content-delivery-network-assets-with-token-authentication"></a>Zabezpieczanie zasobów Azure Content Delivery Network z tokenu uwierzytelniania

[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Omówienie

Token uwierzytelniania jest mechanizm, który pozwala zapobiec obsługująca zasoby do nieautoryzowanego klientów sieci dostarczania zawartości (CDN) platformy Azure. Token uwierzytelniania jest zazwyczaj wykonywane zapobiegające "hotlinking" zawartości, w którym różne witryny sieci Web, często tablica wiadomości, używa zasobów bez uprawnienia. Hotlinking może mieć wpływ na koszty dostarczania zawartości. Przez włączenie tej funkcji w sieci CDN w warstwie, żądania są uwierzytelniani przez krawędzi CDN POP przed CDN oferuje zawartość. 

## <a name="how-it-works"></a>Jak to działa

Token uwierzytelniania sprawdza, czy żądania są generowane przez zaufanych witryn, gdyż żądania zawiera wartość tokenu tego blokad zakodowane informacje obiektu żądającego. Zawartość jest udostępniany element żądający tylko wtedy, gdy dane zakodowane spełnia wymagania; w przeciwnym razie żądania są odrzucane. Wymagania można skonfigurować przy użyciu co najmniej jeden z następujących parametrów:

- Kraju: akceptować lub odrzucać żądania, które pochodzą z określonego krajów.  [Lista poprawnych kodów kraju.](https://msdn.microsoft.com/library/mt761717.aspx) 
- Adres URL: Zezwalaj tylko na określonym zasobie lub ścieżka do żądania.  
- Host: akceptować lub odrzucać żądania przy użyciu określonych hostach w nagłówku żądania.
- Odwołania: akceptować lub odrzucać określonego odwołania do żądania.
- Adres IP: Zezwalaj tylko na żądania, które pochodzą z określonego adresu IP lub podsieci IP.
- Protokół: zezwala lub blokuje żądania na podstawie protokołu używane do żądania zawartości.
- Czas wygaśnięcia: przypisywanie okres daty i godziny, aby upewnić się, że łącze tylko pozostaje ważny przez pewien czas.

Aby uzyskać więcej informacji, zobacz przykłady szczegółowej konfiguracji dla każdego parametru w [Konfigurowanie uwierzytelniania tokenu](#setting-up-token-authentication).

Po wygenerowaniu zaszyfrowany token, zostanie ona dodana jako ciąg zapytania do końca ścieżki adresu URL pliku. Na przykład `http://www.domain.com/content.mov?a4fbc3710fd3449a7c99986b`.

## <a name="reference-architecture"></a>Architektura odwołań

Na poniższym diagramie przepływu pracy w tym artykule opisano sposób CDN do pracy z aplikacji sieci web używa tokenu uwierzytelniania.

![Przepływ tokenu uwierzytelniania CDN](./media/cdn-token-auth/cdn-token-auth-workflow2.png)

## <a name="token-validation-logic-on-cdn-endpoint"></a>Logikę weryfikacji tokenu na punkt końcowy CDN
    
Poniższy schemat opisano, jak Azure CDN weryfikuje żądanie klienta, gdy token uwierzytelniania jest skonfigurowane na punkt końcowy CDN.

![Logikę weryfikacji tokenu CDN](./media/cdn-token-auth/cdn-token-auth-validation-logic.png)

## <a name="setting-up-token-authentication"></a>Konfigurowanie uwierzytelniania tokenu

1. Z [portalu Azure](https://portal.azure.com), przejdź do swojego profilu CDN, a następnie kliknij przycisk **Zarządzaj** można uruchomić portalu dodatkowym.

    ![Przycisk Zarządzaj profil CDN](./media/cdn-rules-engine/cdn-manage-btn.png)

2. Umieść kursor nad **HTTP dużych**, a następnie kliknij przycisk **Token uwierzytelniania** w wysuwane okno. Skonfiguruj parametry szyfrowania na tej karcie i klucza szyfrowania.

    1. Wprowadź unikatowy klucz szyfrowania dla **klucz podstawowy**.  Wprowadź inny dla **kopii zapasowej klucza**

        ![Klucz tokenu uwierzytelniania CDN](./media/cdn-token-auth/cdn-token-auth-setupkey.png)
    
    2. Ustawianie parametrów szyfrowania za pomocą narzędzia szyfrowania (akceptować lub odrzucać żądania oparte na czas wygaśnięcia, kraj, odwołania, protokołu, adresu IP klienta. Można użyć dowolnej kombinacji.)

        ![Narzędzia szyfrowania CDN](./media/cdn-token-auth/cdn-token-auth-encrypttool.png)

        - WE-wygaśnie: przypisuje czas wygaśnięcia tokenu po upływie określonego czasu. Żądań przesłanych po czas wygaśnięcia nie są dozwolone. Sygnatura czasowa Unix używa tego parametru (oparte na sekund od standardowego epoka 1/1/1970 GMT 00:00:00. Służy online narzędzia zapewnienie konwersji między czasem Unix a (czas standardowy).) Na przykład, jeśli chcesz skonfigurować ten token do wygaśnięcia na `12/31/2016 12:00:00 GMT`, należy użyć wartości sygnatury czasowej systemu Unix `1483185600`, wykonując następujące czynności.
    
        ![Przykład ec_expire CDN](./media/cdn-token-auth/cdn-token-auth-expire2.png)
    
        - Zezwalaj adresu url WE: pozwala dostosować tokenów do określonego zasobu lub ścieżki. Go ogranicza dostęp do żądania, którego adres URL rozpoczynać się od określonej ścieżki względnej. Można wprowadzić wiele ścieżek, oddzielając każda ścieżka przecinkami. Adresy URL jest rozróżniana wielkość liter. W zależności od wymagań można skonfigurować inną wartość, aby zapewnić różne poziomy dostępu. Poniżej przedstawiono kilka scenariuszy:
        
            Jeśli adres URL: http://www.mydomain.com/pictures/city/strasbourg.png. Zobacz wartości wejściowej "" i jego dostęp na poziomie odpowiednio

            1. Wartość wejściowa "/": wszystkie żądania będą dozwolone
            2. Wartość wejściowa "/ obrazy": następujące żądania będą Zezwalaj
            
                - http://www.myDomain.com/Pictures.PNG
                - http://www.myDomain.com/Pictures/City/Strasbourg.PNG
                - http://www.myDomain.com/picturesnew/City/strasbourgh.PNG
            3. Wprowadź wartość "/ obrazy /": tylko żądania dla /pictures/ będą dozwolone
            4. Wartość wejściowa "/ pictures/city/strasbourg.png": tylko zezwala na żądania dla tego zasobu
    
        ![Przykład ec_url_allow CDN](./media/cdn-token-auth/cdn-token-auth-url-allow4.png)
    
        - Zezwalaj kraju WE: zezwala tylko na żądania, które pochodzą z co najmniej jeden określony krajów. Żądania, które pochodzą z innych krajów zostaną odrzucone. Umożliwia ustawienie parametrów numer kierunkowy kraju i oddzielając każdy kod kraju przecinkami. Na przykład jeśli chcesz zezwolić na dostęp w Stanach Zjednoczonych i Francja wejściowe nam FR w kolumnie jako poniżej.  
        
        ![Przykład ec_country_allow CDN](./media/cdn-token-auth/cdn-token-auth-country-allow.png)

        - Odmów kraju WE: odrzucanie żądań pochodzących z jednego lub więcej określonych krajów. Może być żądań pochodzących z innych krajów. Umożliwia ustawienie parametrów numer kierunkowy kraju i oddzielając każdy kod kraju przecinkami. Na przykład jeśli chcesz odmówić dostępu w Stanach Zjednoczonych i Francja wejściowe nam FR w kolumnie.
    
        - ec_ref_allow: zezwala na żądania pochodzące z tylko określonego odwołania. Odwołania określa adres URL strony sieci web, który jest połączony z żądanych zasobów. Nie dołączaj protokołu w wartości parametru odwołania. Następujące dane wejściowe są dozwolone dla wartości parametru:
           - Nazwa hosta lub nazwa hosta i ścieżkę.
           - Wiele odwołań. Aby dodać wiele odwołań, każdy odwołania należy oddzielić przecinkami. Po określeniu wartości odwołania, ale informacje odwołania nie są wysyłane w żądania z powodu konfiguracji przeglądarki, te żądania są odrzucane domyślnie. 
           - Żądania z brakującymi informacji odwołania. Aby zezwolić na te typy żądań, wprowadź tekst "Brak" lub wartość pustą. 
           - Domeny podrzędne. Aby umożliwić poddomen, wprowadź znak gwiazdki (*). Na przykład, aby umożliwić wszystkich poddomen `consoto.com` wprowadź `*.consoto.com`. 
           
          W poniższym przykładzie przedstawiono dane wejściowe, aby zezwolić na dostęp dla żądań z `www.consoto.com`, wszystkimi domenami podrzędnymi w `consoto2.com`i żądań z pustą lub Brak odwołań.
        
          ![Przykład ec_ref_allow CDN](./media/cdn-token-auth/cdn-token-auth-referrer-allow2.png)
    
        - Odmów ref WE: odrzucanie żądań z określonego odwołania. Zobacz szczegółowe informacje i przykład w parametrze "WE ref — Zezwalaj".
         
        - Zezwalaj proto WE: zezwala tylko na żądania od wybranego protokołu. Na przykład http lub https.
        
        ![Przykład ec_proto_allow CDN](./media/cdn-token-auth/cdn-token-auth-url-allow4.png)
            
        - Odmów proto WE: odrzucanie żądań z wybranego protokołu. Na przykład http lub https.
    
        - we clientip: ogranicza dostęp do adresu IP określonego żądającego. Obsługiwane są protokoły IPV4 i IPV6. Można określić pojedyncze żądanie adresu IP lub podsieci IP.
            
        ![Przykład ec_clientip CDN](./media/cdn-token-auth/cdn-token-auth-clientip.png)
        
    3. Można przetestować token za pomocą narzędzia opis.

    4. Można również dostosować typ odpowiedzi, który zostanie zwrócony do użytkownika, gdy żądanie zostanie odrzucone. Domyślnie używamy 403.

3. Teraz kliknij **aparatu reguł** w obszarze **HTTP dużych**. Użyjesz na tej karcie można zdefiniować ścieżki, aby zastosować funkcję, włączyć funkcję uwierzytelniania tokenu i włączyć dodatkowe funkcje tokenu związane z uwierzytelnianiem.

    - Kolumna "IF" służy do definiowania zasobów lub ścieżkę, którą chcesz zastosować token uwierzytelniania. 
    - Kliknij, aby dodać "Token uwierzytelniania" z listy rozwijanej funkcji, aby włączyć uwierzytelnianie tokena.
        
    ![Przykład Włącz token uwierzytelniania aparat reguł CDN](./media/cdn-token-auth/cdn-rules-engine-enable2.png)

4. W **aparatu reguł** jest kilka dodatkowych możliwości, możesz włączyć.
    
    - Token uwierzytelniania odmowa kodu: Określa typ odpowiedzi, który zostanie zwrócony do użytkownika, gdy żądanie zostanie odrzucone. Reguł ustawionych w tym miejscu zastępuje ustawienie kodu odmowa na karcie tokenu uwierzytelniania.
    - Ignoruj token uwierzytelniania: Określa, czy adres URL używany do sprawdzania poprawności tokenu będzie uwzględniana wielkość liter.
    - Parametr tokenu uwierzytelniania: Zmień nazwę parametru ciągu zapytania token uwierzytelniania przedstawiający w żądanym adresie URL. 
        
    ![Przykład ustawienia uwierzytelniania tokena aparat reguł CDN](./media/cdn-token-auth/cdn-rules-engine2.png)

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
