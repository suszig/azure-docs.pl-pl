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
ms.openlocfilehash: b1bcaf14e9805afa82c765092b62201f58c7cbc4
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/04/2017
---
# <a name="securing-azure-content-delivery-network-assets-with-token-authentication"></a>Zabezpieczanie zasobów Azure Content Delivery Network z tokenu uwierzytelniania

[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Omówienie

Token uwierzytelniania jest mechanizm, który pozwala zapobiec obsługująca zasoby do nieautoryzowanego klientów sieci dostarczania zawartości (CDN) platformy Azure. Token uwierzytelniania jest zazwyczaj wykonywane zapobiegające "hotlinking" zawartości, w którym różne witryny sieci Web, często tablica wiadomości, używa zasobów bez uprawnienia. Hotlinking może mieć wpływ na koszty dostarczania zawartości. Przez włączenie tej funkcji w sieci CDN w warstwie, żądania są uwierzytelniani przez krawędzi CDN POP przed CDN oferuje zawartość. 

## <a name="how-it-works"></a>Jak to działa

Token uwierzytelniania sprawdza, czy żądania są generowane przez zaufanych witryn, gdyż żądania zawiera wartość tokenu tego blokad zakodowane informacje obiektu żądającego. Zawartość jest udostępniany element żądający tylko wtedy, gdy dane zakodowane spełnia wymagania; w przeciwnym razie żądania są odrzucane. Wymagania można skonfigurować przy użyciu co najmniej jeden z następujących parametrów:

- Kraju: Akceptować lub odrzucać żądania, które pochodzą z krajów określony przez ich [numer kierunkowy kraju](https://msdn.microsoft.com/library/mt761717.aspx).
- Adres URL: Zezwalaj tylko te żądania, które odpowiadają określonym zasobie lub ścieżki.
- Host: Akceptować lub odrzucać żądania używające określonych hostach w nagłówku żądania.
- Odwołania: Akceptować lub odrzucać żądania od określonego odwołania.
- Adres IP: Zezwalaj tylko żądania, które pochodzą z określonego adresu IP lub podsieci IP.
- Protokół: Akceptować lub odrzucać żądania na podstawie protokołu używane do żądania zawartości.
- Czas wygaśnięcia: przypisywanie okres daty i godziny, aby upewnić się, że łącze pozostaje ważny tylko przez ograniczony czas.

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

    1. Wprowadź unikatowy klucz szyfrowania dla **klucza podstawowego** , a następnie wprowadź osobne **kopii zapasowej klucza**.

        ![Klucz tokenu uwierzytelniania CDN](./media/cdn-token-auth/cdn-token-auth-setupkey.png)
    
    2. Ustawianie parametrów szyfrowania za pomocą narzędzia szyfrowania. Za pomocą narzędzia Szyfruj można akceptować lub odrzucać żądania na podstawie czasu wygaśnięcia, kraj, odwołania, protokół i IP klienta (w dowolnej kombinacji).

        ![Narzędzia szyfrowania CDN](./media/cdn-token-auth/cdn-token-auth-encrypttool.png)

       - ec_expire: przypisuje czas wygaśnięcia tokenu, po którym token jest ważny. Żądań przesłanych po czas wygaśnięcia nie są dozwolone. Ten parametr używa sygnatury czasowej systemu Unix, opartym na liczba sekund od standardowego epoka `1/1/1970 00:00:00 GMT`. (Umożliwia konwersję pomiędzy (czas standardowy) i czas Unix można użyć narzędzia online). Na przykład, jeśli chcesz skonfigurować ten token do wygaśnięcia na `12/31/2016 12:00:00 GMT`, należy użyć wartości sygnatury czasowej systemu Unix `1483185600`, wykonując następujące czynności. 
    
         ![Przykład ec_expire CDN](./media/cdn-token-auth/cdn-token-auth-expire2.png)
    
       - ec_url_allow: pozwala dostosować tokenów do określonego zasobu lub ścieżki. Go ogranicza dostęp do żądania, którego adres URL rozpoczynać się od określonej ścieżki względnej. Adresy URL jest rozróżniana wielkość liter. Wprowadź wiele ścieżek, oddziel przecinkami poszczególnych ścieżek. W zależności od wymagań można skonfigurować różne wartości, aby zapewnić różne poziomy dostępu. 
        
          Na przykład dla adresu URL `http://www.mydomain.com/pictures/city/strasbourg.png`, ustaw wartość wejściowa i jego poziom dostępu w następujący sposób:

           - Wartość wejściowa `"/"`: wszystkie żądania są dozwolone
           - Wartość wejściowa `"/pictures`. Dopuszczalne są następujące żądania:
              - `http://www.mydomain.com/pictures.png`
              - `http://www.mydomain.com/pictures/city/strasbourg.png`
              - `http://www.mydomain.com/picturesnew/city/strasbourgh.png`
            - Wartość wejściowa `/pictures/`: tylko żądania dotyczące `/pictures/` są dozwolone. Na przykład `http://www.mydomain.com/pictures/city/strasbourg.png`.
            - Wartość wejściowa `/pictures/city/strasbourg.png`: dozwolone są tylko żądania dla tego zasobu określonego.
    
          ![Przykład ec_url_allow CDN](./media/cdn-token-auth/cdn-token-auth-url-allow4.png)
    
       - ec_country_allow: zezwala tylko na żądania, które pochodzą z co najmniej jeden określony krajów. Żądania, które pochodzą z innych krajów są odrzucane. Użyj numerów kierunkowych krajów i oddziel je przecinkami. Na przykład jeśli chcesz zezwolić na dostęp tylko Stany Zjednoczone i Francja wejściowe nam FR w polu w następujący sposób.  
        
           ![Przykład ec_country_allow CDN](./media/cdn-token-auth/cdn-token-auth-country-allow.png)

       - ec_country_deny: odrzucanie żądań, które pochodzą z co najmniej jeden określony krajów. Żądania, które pochodzą z innych krajów są dozwolone. Użyj numerów kierunkowych krajów i oddziel je przecinkami. Na przykład jeśli chcesz odmówić dostępu w Stanach Zjednoczonych i Francja wejściowe nam FR, w tym polu.
    
       - ec_ref_allow: tylko zezwala na żądania pochodzące z określonej odwołania. Odwołania określa adres URL strony sieci web, który jest połączony z żądanych zasobów. Nie dołączaj protokołu w wartości parametru odwołania. Następujące dane wejściowe są dozwolone dla wartości parametru:
           - Nazwa hosta lub nazwa hosta i ścieżkę.
           - Wiele odwołań. Aby dodać wiele odwołań, każdy odwołania należy oddzielić przecinkami. Po określeniu wartości odwołania, ale informacje odwołania nie są wysyłane w żądania z powodu konfiguracji przeglądarki, te żądania są odrzucane domyślnie. 
           - Żądania z brakującymi informacji odwołania. Aby zezwolić na te typy żądań, wprowadź tekst "Brak" lub wartość pustą. 
           - Domeny podrzędne. Aby umożliwić poddomen, wprowadź znak gwiazdki (*). Na przykład, aby umożliwić wszystkich poddomen `consoto.com` wprowadź `*.consoto.com`. 
           
          W poniższym przykładzie przedstawiono dane wejściowe, aby zezwolić na dostęp dla żądań z `www.consoto.com`, wszystkimi domenami podrzędnymi w `consoto2.com`i żądań z pustą lub Brak odwołań.
        
          ![Przykład ec_ref_allow CDN](./media/cdn-token-auth/cdn-token-auth-referrer-allow2.png)
    
       - ec_ref_deny: odrzucanie żądań z określonego odwołania. Implementacja jest taka sama, jak parametr ec_ref_allow.
         
       - ec_proto_allow: zezwala tylko na żądania od wybranego protokołu. Na przykład HTTP lub HTTPS.
        
         ![Przykład ec_proto_allow CDN](./media/cdn-token-auth/cdn-token-auth-url-allow4.png)
            
       - ec_proto_deny: odrzucanie żądań z wybranego protokołu. Na przykład HTTP lub HTTPS.
    
       - ec_clientip: ogranicza dostęp do żądającego określonego adresu IP. Obsługiwane są protokoły IPV4 i IPV6. Można określić na pojedyncze żądanie adres IP lub podsieć IP.
            
         ![Przykład ec_clientip CDN](./media/cdn-token-auth/cdn-token-auth-clientip.png)
        
    3. Opcjonalnie można sprawdzić za pomocą narzędzia opis token.

    4. Opcjonalnie można dostosować typ odpowiedzi, który jest zwracany, gdy żądanie zostanie odrzucone. Domyślnie używany jest kod odpowiedzi 403.

3. W obszarze **HTTP dużych**, kliknij przycisk **aparatu reguł**. Aparat reguł służy do definiowania ścieżki, aby zastosować funkcję, włączyć funkcję uwierzytelniania tokenu i włączyć dodatkowe funkcje tokenu związane z uwierzytelnianiem.

    1. Użyj **IF** kolumny do definiowania zasobów lub ścieżka, dla którego chcesz zastosować token uwierzytelniania. 
    2. Aby włączyć uwierzytelnianie tokenu, wybierz **Token uwierzytelniania** z **funkcje** listy rozwijanej.
        
    ![Przykład Włącz token uwierzytelniania aparat reguł CDN](./media/cdn-token-auth/cdn-rules-engine-enable2.png)

4. Aparat reguł istnieje kilka dodatkowych możliwości, które można włączyć:
    
    - **Token uwierzytelniania odmowa kodu**: Określa typ odpowiedzi, który jest zwracany do użytkownika, gdy żądanie zostanie odrzucone. Ustaw tutaj zastępują odmowa ustawienie kodu w **Token uwierzytelniania** kartę.
    - **Token uwierzytelniania Ignoruj adresu URL przypadku**: Określa, czy adres URL używany do sprawdzania poprawności tokenu jest rozróżniana wielkość liter.
    - **Token uwierzytelniania parametru**: zmienia nazwę parametru ciągu zapytania token uwierzytelniania wyświetlany w żądanym adresie URL. 
        
    ![Przykład ustawienia uwierzytelniania tokena aparat reguł CDN](./media/cdn-token-auth/cdn-rules-engine2.png)

5. Można dostosować token, który jest aplikacja, która generuje token dla funkcji uwierzytelniania opartego na tokenie. Kod źródłowy jest dostępny w [GitHub](https://github.com/VerizonDigital/ectoken).
Dostępne języki:
    
    - C
    - C#
    - PHP
    - Perl
    - Java
    - Python    


## <a name="azure-cdn-features-and-provider-pricing"></a>Azure CDN funkcji i cenach dostawcy

Aby uzyskać informacje, zobacz [Omówienie usługi CDN](cdn-overview.md).
