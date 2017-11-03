---
title: "Włącz lub wyłącz HTTPS na domeny niestandardowej Azure Content Delivery Network | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak włączyć lub wyłączyć HTTPS na punkt końcowy usługi Azure CDN z domeny niestandardowej."
services: cdn
documentationcenter: 
author: camsoper
manager: erikre
editor: 
ms.assetid: 10337468-7015-4598-9586-0b66591d939b
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/03/2017
ms.author: casoper
ms.openlocfilehash: c92f1e20acf55b8bd791fad43f17e162a5cb3847
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="enable-or-disable-https-on-an-azure-content-delivery-network-custom-domain"></a>Włącz lub wyłącz HTTPS na Azure Content Delivery Network domeny niestandardowej

[!INCLUDE [cdn-verizon-only](../../includes/cdn-verizon-only.md)]

Obsługa protokołu HTTPS dla domen niestandardowych Microsoft Azure Content Delivery Network (CDN) umożliwia dostarczanie bezpieczne zawartości za pośrednictwem protokołu SSL w celu zwiększenia bezpieczeństwa danych podczas przesyłania przy użyciu nazwy domeny. Przepływ pracy end-to-end, aby włączyć protokół HTTPS dla domeny niestandardowej jest uproszczone, za pomocą jednego kliknięcia aktywacji, pełnego zarządzania certyfikatami i wszystkie bez dodatkowych kosztów.

Bardzo ważne dla zapewnienia prywatności i integralności danych z wszystkich aplikacji sieci web poufnych danych przesyłanych jest. Użycie protokołu HTTPS gwarantuje, że poufne dane są szyfrowane, gdy są wysyłane przez internet. Zapewnia zaufania, uwierzytelniania i chroni przed atakami aplikacji sieci web. Obecnie usługa Azure CDN obsługuje HTTPS na punktu końcowego usługi CDN. Na przykład w przypadku utworzenia punktu końcowego usługi CDN z usługi Azure CDN (na przykład https://contoso.azureedge.net), protokół HTTPS jest włączona domyślnie. Teraz z domeny niestandardowej HTTPS, można włączyć bezpieczne dostarczanie dla domeny niestandardowej (na przykład https://www.contoso.com) oraz. 

Niektóre z kluczowych atrybutów funkcja HTTPS są:

- Bez dodatkowych kosztów: Brak nie koszty nabycia certyfikatu lub odnowienie i bez dodatkowych kosztów dla ruchu HTTPS. Po prostu płacisz za GB wyjście z sieci CDN.

- Włączanie prostego: jeden inicjowania obsługi kliknij przycisk jest dostępny z [portalu Azure](https://portal.azure.com). Aby włączyć tę funkcję, można użyć interfejsu API REST lub innych narzędzi dla deweloperów.

- Zakończenie zarządzania certyfikatami: wszystkie certyfikatów nabywania i zarządzania jest już obsługiwane. Certyfikaty są automatycznie udostępniane i odnowione przed jego wygaśnięciem. Usuwa to całkowicie ryzyka przerw w wyniku certyfikat wygasa.

>[!NOTE] 
>Przed włączeniem obsługi protokołu HTTPS, musi mieć już określone [domenę niestandardową Azure CDN](./cdn-map-content-to-custom-domain.md).

## <a name="enabling-https"></a>Włączanie protokołu HTTPS

Aby włączyć protokół HTTPS, wykonaj następujące kroki:

### <a name="step-1-enable-the-feature"></a>Krok 1: Włącz tę funkcję 

1. W [portalu Azure](https://portal.azure.com), przejdź do profilu sieci CDN w warstwie standardowa lub premium Verizon.

2. Na liście punktów końcowych kliknij punkt końcowy zawierający domenę niestandardową.

3. Kliknij przycisk domeny niestandardowej, dla którego chcesz włączyć protokół HTTPS.

    ![Blok końcowy](./media/cdn-custom-ssl/cdn-custom-domain.png)

4. Kliknij przycisk **na** Aby włączyć protokół HTTPS, a następnie przycisk **Zastosuj**.

    ![Niestandardowe okno protokołu HTTPS](./media/cdn-custom-ssl/cdn-enable-custom-ssl.png)


### <a name="step-2-validate-domain"></a>Krok 2: Weryfikowanie domeny

>[!IMPORTANT] 
>Należy ukończyć weryfikacji domeny na domenę niestandardową HTTPS będzie aktywny. Należy zatwierdzić domeny sześć dni roboczych. Żądania, które nie zostały zatwierdzone w ciągu sześciu dni roboczych zostaną automatycznie anulowane. 

Po włączeniu HTTPS na domenę niestandardową naszych HTTPS dostawcę certyfikatów firmy DigiCert weryfikuje własność domeny kontaktując się z rejestratorem domeny, zgodnie z domeny [WHOIS](http://whois.domaintools.com/) rejestratorem informacji. Skontaktuj się z zostało utworzone za pomocą adresu e-mail (przez ustawienie domyślne) lub numer telefonu wymienionych w rejestracji WHOIS. 

![Rekord WHOIS](./media/cdn-custom-ssl/whois-record.png)

Ponadto DigiCert wyśle weryfikacji wiadomości e-mail do następujących adresów. Informacje rejestratorem WHOIS jest prywatny, sprawdź, czy możesz zatwierdzić bezpośrednio z jednego z tych adresów:

Administrator @&lt;your name.com domeny&gt;  
Administrator @&lt;your name.com domeny&gt;  
webmaster @&lt;your name.com domeny&gt;  
hostmaster @&lt;your name.com domeny&gt;  
Postmaster @&lt;your name.com domeny&gt;  

Powinien zostać wyświetlony wiadomości e-mail za kilka minut, podobnie do poniższego przykładu prośbą o zatwierdzenie żądania. Jeśli używasz filtru spamu, Dodaj admin@digicert.com jego listą dozwolonych adresów IP. Jeśli w ciągu 24 godzin nie otrzymasz wiadomość e-mail, skontaktuj się z pomocą techniczną firmy Microsoft.
    
![Niestandardowe okno protokołu HTTPS](./media/cdn-custom-ssl/domain-validation-email.png)

Po kliknięciu łącza zatwierdzenia nastąpi przekierowanie do następującej postaci online zatwierdzenia: 
    
![Niestandardowe okno protokołu HTTPS](./media/cdn-custom-ssl/domain-validation-form.png)

Postępuj zgodnie z instrukcjami na formularzu. dostępne są dwie opcje weryfikacji:

- Możesz zatwierdzać wszystkie przyszłe zamówień za pomocą tego samego konta dla tej samej domeny głównej; na przykład contoso.com. Jest to zalecane podejście, jeśli zamierzasz dodać dodatkowe domeny niestandardowe w przyszłości dla tej samej domeny głównej.

- Możesz zatwierdzać tylko nazwę określonego hosta, używany w tym żądaniu. Dodatkowe zatwierdzenie będzie wymagane dla kolejnych żądań.

Po zatwierdzeniu żądania DigiCert doda niestandardowej nazwy domeny do nazwy alternatywnej podmiotu (SAN) certyfikatu. Certyfikat jest ważny przez jeden rok i był auto odnawiany przed wygasła.

### <a name="step-3-wait-for-propagation"></a>Krok 3: Poczekaj, aż propagacji

Po zweryfikowaniu nazwy domeny może potrwać do 6-8 godzin dla domeny niestandardowej funkcji HTTPS do aktywacji. Po zakończeniu procesu stan "HTTPS niestandardowe" w portalu Azure jest ustawiony na "Włączone", a operacja cztery kroki opisane w bloku HTTPS domeny niestandardowe są oznaczone jako zakończone. Domena niestandardowa jest teraz gotowy do używania protokołu HTTPS.

![Włącz protokół HTTPS w oknie dialogowym](./media/cdn-custom-ssl/cdn-enable-custom-ssl-complete.png)

### <a name="operation-progress"></a>Postęp operacji

W poniższej tabeli przedstawiono postęp operacji, gdy włączyć protokół HTTPS. Po włączeniu HTTPS, należy wykonać cztery czynności operacji są wyświetlane w bloku HTTPS domeny niestandardowe. Ponieważ każdy krok staje się aktywny, dodatkowe szczegóły są wyświetlane w polu krok zgodnie z jego postępów. Po pomyślnym zakończeniu kroku, zostanie wyświetlony zielony znacznik wyboru obok niej. 

| Operacja kroku | Szczegóły kroku operacji | 
| --- | --- |
| Żądanie Submitting 1 | Przesyłanie żądania |
| | Twoje żądanie HTTPS jest przesyłane. |
| | Pomyślnie przesłano żądanie HTTPS. |
| 2 weryfikacji domeny | Ma Ci Wysłaliśmy wiadomość e-mail z pytaniem, aby zweryfikować własność domeny. Oczekiwanie na potwierdzenie. |
| | Prawa własności domeny został zweryfikowany pomyślnie. |
| | Żądanie sprawdzania poprawności własność domeny wygasło (klienta, prawdopodobnie nie odpowiedział w ciągu 6 dni). Protokół HTTPS nie zostaną włączone w domenie. * |
| | Domeny własność weryfikacji żądanie zostało odrzucone przez klienta. Protokół HTTPS nie zostaną włączone w domenie. * |
| Inicjowanie obsługi certyfikatów 3 | Urząd certyfikacji jest obecnie wystawiający certyfikaty wymagane do włączenia protokołu HTTPS w domenie. |
| | Certyfikat został wystawiony i jest aktualnie wdrażany do sieci CDN. Może to potrwać do 6 godzin. |
| | Certyfikat została pomyślnie wdrożona w sieci CDN. |
| Zakończ 4 | Pomyślnie włączono HTTPS w domenie. |

\*Ten komunikat nie będą wyświetlane, o ile nie wystąpił błąd. 

Jeśli wystąpi błąd przed przesłaniem żądania, zostanie wyświetlony następujący komunikat o błędzie:

<code>
We encountered an unexpected error while processing your HTTPS request. Please try again and contact support if the issue persists.
</code>

## <a name="disabling-https"></a>Wyłączanie protokołu HTTPS

Po włączeniu HTTPS, można je później wyłączyć. Aby wyłączyć protokół HTTPS, wykonaj następujące kroki:

### <a name="step-1-disable-the-feature"></a>Krok 1: Wyłączanie funkcji 

1. W [portalu Azure](https://portal.azure.com), przejdź do profilu sieci CDN w warstwie standardowa lub premium Verizon.

2. Na liście punktów końcowych kliknij punkt końcowy zawierający domenę niestandardową.

3. Kliknij przycisk domeny niestandardowej, dla której chcesz wyłączyć protokołu HTTPS.

    ![Blok końcowy](./media/cdn-custom-ssl/cdn-custom-domain-HTTPS-enabled.png)

4. Kliknij przycisk **poza** Aby wyłączyć protokół HTTPS, a następnie przycisk **Zastosuj**.

    ![Niestandardowe okno protokołu HTTPS](./media/cdn-custom-ssl/cdn-disable-custom-ssl.png)

### <a name="step-2-wait-for-propagation"></a>Krok 2: Poczekaj, aż propagacji

Po wyłączeniu funkcji HTTPS domeny niestandardowej, może upłynąć do 6-8 godzin zostały wprowadzone. Po zakończeniu procesu stan "HTTPS niestandardowe" w portalu Azure jest ustawiony na "wyłączone" i operacji trzy kroki opisane w bloku HTTPS domeny niestandardowe są oznaczone jako zakończone. Domeny niestandardowej nie można już używać protokołu HTTPS.

![Wyłącz okno protokołu HTTPS](./media/cdn-custom-ssl/cdn-disable-custom-ssl-complete.png)

### <a name="operation-progress"></a>Postęp operacji

W poniższej tabeli przedstawiono postęp operacji, gdy wyłączenie protokołu HTTPS. Po wyłączeniu HTTPS, należy wykonać trzy czynności operacji są wyświetlane w bloku HTTPS domeny niestandardowej. Ponieważ każdy krok staje się aktywny, dodatkowe szczegóły są wyświetlane w polu kroku. Po pomyślnym zakończeniu kroku, zostanie wyświetlony zielony znacznik wyboru obok niej. 

| Postęp operacji | Szczegóły operacji | 
| --- | --- |
| Żądanie Submitting 1 | Przesyłanie żądania |
| 2 anulowania obsługi certyfikatów | Trwa usuwanie certyfikatu |
| Zakończ 3 | Usunąć certyfikatu |

## <a name="frequently-asked-questions"></a>Często zadawane pytania

1. *Kto jest dostawcę certyfikatów i jakiego typu używanego certyfikatu?*

    Używamy nazwy alternatywnej podmiotu (SAN) certyfikatu dostarczonego przez DigiCert. Certyfikat SAN można zabezpieczyć wiele nazw FQDN z jednym certyfikatem.

2. *Można użyć dedykowanego certyfikatu?*
    
    Aktualnie nie ale nie znajduje się na plan.

3. *Co zrobić, jeśli I nie otrzymywać wiadomości e-mail weryfikacji domeny firmy DigiCert?*

    Jeśli użytkownik nie otrzyma wiadomość e-mail w ciągu 24 godzin, skontaktuj się z pomocy technicznej firmy Microsoft.

4. *Używa mniej bezpieczna niż certyfikat dedykowanych certyfikat SAN?*
    
    Certyfikat SAN wykonuje szyfrowanie i zabezpieczenia standardach jako dedykowane certyfikatu. Wszystkie wystawiane certyfikaty SSL są za pomocą algorytmu SHA-256 rozszerzonego serwera zabezpieczeń.

5. *Można używać protokołu HTTPS domeny niestandardowej z usługą Azure CDN from Akamai?*

    Obecnie ta funkcja jest dostępna tylko z usługą Azure CDN from Verizon. Pracujemy nad Obsługa tej funkcji w programie Azure CDN from Akamai w najbliższych miesiącach.


## <a name="next-steps"></a>Następne kroki

- Dowiedz się, jak skonfigurować [niestandardową domenę na punkt końcowy usługi Azure CDN](./cdn-map-content-to-custom-domain.md)


