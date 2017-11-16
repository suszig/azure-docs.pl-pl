---
title: "Skonfiguruj protokół HTTPS na domeny niestandardowej Azure Content Delivery Network | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak włączyć lub wyłączyć HTTPS na punkt końcowy usługi Azure CDN z domeny niestandardowej."
services: cdn
documentationcenter: 
author: dksimpson
manager: 
editor: 
ms.assetid: 10337468-7015-4598-9586-0b66591d939b
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2017
ms.author: casoper
ms.openlocfilehash: 82de79cde208cdce1ed7cbd600f1e804ff1d45ff
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/14/2017
---
# <a name="configure-https-on-an-azure-content-delivery-network-custom-domain"></a>Skonfiguruj protokół HTTPS na Azure Content Delivery Network domeny niestandardowej

[!INCLUDE [cdn-verizon-only](../../includes/cdn-verizon-only.md)]

Firma Microsoft obsługuje protokół HTTPS dla domen niestandardowych na platformie Azure sieci dostarczania zawartości (CDN). Z obsługą domeny niestandardowej HTTPS może zapewnić bezpieczne zawartości za pomocą protokołu SSL przy użyciu nazwy domeny w celu zwiększenia bezpieczeństwa danych podczas przesyłania. Przepływ pracy, aby włączyć protokół HTTPS dla domeny niestandardowej jest uproszczone, za pomocą jednego kliknięcia aktywacji i pełnego zarządzania certyfikatami, wszystkie bez dodatkowych kosztów.

Jest krytyczne, aby zapewnić prywatność i integralność danych poufnych danych aplikacji sieci web podczas przesyłania. Przy użyciu protokołu HTTPS, należy zapewnić, że poufne dane są szyfrowane, gdy są wysyłane przez internet. Zapewnia zaufania, uwierzytelniania i chroni przed atakami aplikacji sieci web. Domyślnie usługi Azure CDN obsługuje protokół HTTPS na punktu końcowego usługi CDN. Na przykład w przypadku utworzenia punktu końcowego usługi CDN z usługi Azure CDN (takie jak `https://contoso.azureedge.net`), protokół HTTPS jest automatycznie włączone. Ponadto z obsługą protokołu HTTPS domeny niestandardowej, można również włączyć bezpieczne dostarczanie dla domeny niestandardowej (na przykład `https://www.contoso.com`). 

Niektóre z kluczowych atrybutów funkcję HTTPS są:

- Bez dodatkowych kosztów: Brak nie koszty nabycia certyfikatu lub odnowienie i bez dodatkowych kosztów dla ruchu HTTPS. Płacisz tylko za GB wyjście z sieci CDN.

- Włączanie prostego: jednym kliknięciem obsługa jest dostępna z [portalu Azure](https://portal.azure.com). Aby włączyć tę funkcję, można użyć interfejsu API REST lub innych narzędzi dla deweloperów.

- Zakończenie zarządzania certyfikatami: wszystkie certyfikatów nabywania i zarządzania jest już obsługiwane. Certyfikaty są automatycznie udostępniane i odnowione przed jego wygaśnięciem, co spowoduje usunięcie ryzyka przerwanie usługi z powodu certyfikat wygasa.

>[!NOTE] 
>Przed włączeniem obsługi protokołu HTTPS, musi mieć już określone [domenę niestandardową Azure CDN](./cdn-map-content-to-custom-domain.md).

## <a name="enabling-https"></a>Włączanie protokołu HTTPS

Aby włączyć protokół HTTPS na domenę niestandardową, wykonaj następujące kroki:

### <a name="step-1-enable-the-feature"></a>Krok 1: Włącz tę funkcję 

1. W [portalu Azure](https://portal.azure.com), przejdź do profilu sieci CDN w warstwie standardowa lub premium Verizon.

2. Na liście punktów końcowych kliknij punkt końcowy zawierający domenę niestandardową.

3. Kliknij przycisk domeny niestandardowej, dla którego chcesz włączyć protokół HTTPS.

    ![Listy domen niestandardowych](./media/cdn-custom-ssl/cdn-custom-domain.png)

4. Kliknij przycisk **na** Aby włączyć protokół HTTPS, a następnie przycisk **Zastosuj**.

    ![Stan protokołu HTTPS domeny niestandardowej](./media/cdn-custom-ssl/cdn-enable-custom-ssl.png)


### <a name="step-2-validate-domain"></a>Krok 2: Weryfikowanie domeny

>[!IMPORTANT] 
>Należy ukończyć weryfikacji domeny na domenę niestandardową HTTPS będzie aktywny. Należy zatwierdzić domeny sześć dni roboczych. Żądania, które nie zostały zatwierdzone w ciągu sześciu dni roboczych, zostaną automatycznie anulowane. 

Po włączeniu HTTPS na domenę niestandardową, urząd certyfikacji (CA) firmy DigiCert weryfikuje prawo własności do domeny kontaktując się z jego rejestratorem zgodnie z domeny [WHOIS](http://whois.domaintools.com/) rejestratorem informacji. Skontaktuj się z zostało utworzone za pomocą adresu e-mail (przez ustawienie domyślne) lub numer telefonu wymienionych w rejestracji WHOIS. 

>[!NOTE]
>Jeśli u swojego dostawcy DNS rekord certyfikatu urzędu autoryzacji (CAA), musi on zawierać DigiCert jako prawidłowego urzędu certyfikacji. Rekord CAA umożliwia właścicieli domeny z ich dostawców DNS, które urzędów certyfikacji są upoważnione do wystawiania certyfikatów dla danej domeny. Jeśli urząd certyfikacji otrzyma aby certyfikat dla domeny, która zawiera rekord CAA i urzędu certyfikacji nie jest wymieniony jako autoryzowanego wystawcę, jest zabronione wystawi certyfikat do tej domeny lub poddomeny. Informacje o zarządzaniu CAA rekordów, zobacz [rekordów Zarządzanie CAA](https://support.dnsimple.com/articles/manage-caa-record/). Narzędzia rekordu CAA, zobacz [pomocnika rekordu CAA](https://sslmate.com/caa/).

![Rekord WHOIS](./media/cdn-custom-ssl/whois-record.png)

DigiCert również wysyła wiadomość e-mail z weryfikacji na adresy e-mail dodatkowych. Informacji rejestratorem WHOIS jest prywatny, sprawdź, czy możesz zatwierdzić bezpośrednio z jednego z następujących adresów:

Administrator @&lt;your name.com domeny&gt;  
Administrator @&lt;your name.com domeny&gt;  
webmaster @&lt;your name.com domeny&gt;  
hostmaster @&lt;your name.com domeny&gt;  
Postmaster @&lt;your name.com domeny&gt;  

Powinien zostać wyświetlony wiadomości e-mail za kilka minut, podobnie do poniższego przykładu prośbą o zatwierdzenie żądania. Jeśli używasz filtru spamu, Dodaj admin@digicert.com jego listą dozwolonych adresów IP. Jeśli w ciągu 24 godzin nie otrzymasz wiadomość e-mail, skontaktuj się z pomocą techniczną firmy Microsoft.
    
![Domeny weryfikacji w wiadomości e-mail](./media/cdn-custom-ssl/domain-validation-email.png)

Po kliknięciu łącza zatwierdzenia są kierowane do następującej postaci online zatwierdzenia: 
    
![Formularz Sprawdzanie poprawności domeny](./media/cdn-custom-ssl/domain-validation-form.png)

Postępuj zgodnie z instrukcjami na formularzu. dostępne są dwie opcje weryfikacji:

- Możesz zatwierdzać wszystkie przyszłe zamówień za pomocą tego samego konta dla tej samej domeny głównej; na przykład `contoso.com`. Takie podejście jest zalecane, jeśli zamierzasz dodać dodatkowe domeny niestandardowej dla tej samej domeny głównej.

- Możesz zatwierdzać tylko nazwę określonego hosta, używany w tym żądaniu. Dodatkowe zatwierdzenia jest wymagana dla kolejnych żądań.

Po zatwierdzeniu żądania DigiCert dodaje niestandardową nazwę domeny do nazwy alternatywnej podmiotu (SAN) certyfikatu. Certyfikat jest ważny przez jeden rok i był auto odnawiany przed wygasła.

### <a name="step-3-wait-for-propagation"></a>Krok 3: Poczekaj, aż propagacji

Po zweryfikowaniu nazwy domeny może potrwać do 6-8 godzin dla domeny niestandardowej funkcji HTTPS do aktywacji. Po zakończeniu procesu niestandardowych stan HTTPS w portalu Azure jest ustawiony na **włączone** i operacji cztery kroki w oknie dialogowym domeny niestandardowej są oznaczone jako zakończone. Domena niestandardowa jest teraz gotowy do używania protokołu HTTPS.

![Włącz protokół HTTPS w oknie dialogowym](./media/cdn-custom-ssl/cdn-enable-custom-ssl-complete.png)

### <a name="operation-progress"></a>Postęp operacji

W poniższej tabeli przedstawiono postęp operacji, gdy włączyć protokół HTTPS. Po włączeniu HTTPS cztery kroki operacji są wyświetlane w oknie dialogowym domeny niestandardowej. Ponieważ każdy krok staje się aktywny, dodatkowe szczegóły są wyświetlane w polu krok zgodnie z jego postępów. Po pomyślnym zakończeniu kroku, zostanie wyświetlony zielony znacznik wyboru obok niej. 

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

\*Ten komunikat jest wyświetlany tylko wystąpił błąd. 

Jeśli wystąpi błąd przed przesłaniem żądania, zostanie wyświetlony następujący komunikat o błędzie:

<code>
We encountered an unexpected error while processing your HTTPS request. Please try again and contact support if the issue persists.
</code>

## <a name="disabling-https"></a>Wyłączanie protokołu HTTPS

Po włączeniu HTTPS na domenę niestandardową, można je później wyłączyć. Aby wyłączyć protokół HTTPS, wykonaj następujące kroki:

### <a name="step-1-disable-the-feature"></a>Krok 1: Wyłączanie funkcji 

1. W [portalu Azure](https://portal.azure.com), przejdź do profilu sieci CDN w warstwie standardowa lub premium Verizon.

2. Na liście punktów końcowych kliknij punkt końcowy zawierający domenę niestandardową.

3. Kliknij przycisk domeny niestandardowej, dla której chcesz wyłączyć protokołu HTTPS.

    ![Listy domen niestandardowych](./media/cdn-custom-ssl/cdn-custom-domain-HTTPS-enabled.png)

4. Kliknij przycisk **poza** Aby wyłączyć protokół HTTPS, a następnie przycisk **Zastosuj**.

    ![Niestandardowe okno protokołu HTTPS](./media/cdn-custom-ssl/cdn-disable-custom-ssl.png)

### <a name="step-2-wait-for-propagation"></a>Krok 2: Poczekaj, aż propagacji

Po wyłączeniu funkcji HTTPS domeny niestandardowej, może upłynąć do 6-8 godzin zostały wprowadzone. Po zakończeniu procesu niestandardowych stan HTTPS w portalu Azure jest ustawiony na **wyłączone** i operacji trzy kroki w oknie dialogowym domeny niestandardowej są oznaczone jako zakończone. Domeny niestandardowej nie można już używać protokołu HTTPS.

![Wyłącz okno protokołu HTTPS](./media/cdn-custom-ssl/cdn-disable-custom-ssl-complete.png)

### <a name="operation-progress"></a>Postęp operacji

W poniższej tabeli przedstawiono postęp operacji, gdy wyłączenie protokołu HTTPS. Po wyłączeniu HTTPS trzy kroki operacji są wyświetlane w oknie dialogowym domeny niestandardowej. Ponieważ każdy krok staje się aktywny, dodatkowe szczegóły są wyświetlane w polu kroku. Po pomyślnym zakończeniu kroku, zostanie wyświetlony zielony znacznik wyboru obok niej. 

| Postęp operacji | Szczegóły operacji | 
| --- | --- |
| Żądanie Submitting 1 | Przesyłanie żądania |
| 2 anulowania obsługi certyfikatów | Trwa usuwanie certyfikatu |
| Zakończ 3 | Usunąć certyfikatu |

## <a name="frequently-asked-questions"></a>Często zadawane pytania

1. *Kto jest dostawcę certyfikatów i jakiego typu używanego certyfikatu?*

    Firma Microsoft używa nazwy alternatywnej podmiotu (SAN) certyfikatu firmy DigiCert. Certyfikat SAN można zabezpieczyć wiele nazw FQDN z jednym certyfikatem.

2. *Można użyć dedykowanego certyfikatu?*
    
    Aktualnie nie ale nie znajduje się na plan.

3. *Co zrobić, jeśli I nie otrzymywać wiadomości e-mail weryfikacji domeny firmy DigiCert?*

    Jeśli użytkownik nie otrzyma wiadomość e-mail w ciągu 24 godzin, skontaktuj się z pomocy technicznej firmy Microsoft.

4. *Używa mniej bezpieczna niż certyfikat dedykowanych certyfikat SAN?*
    
    Certyfikat SAN wykonuje szyfrowanie i zabezpieczenia standardach jako dedykowane certyfikatu. Wszystkie wystawiane certyfikaty SSL używać algorytmu SHA-256 zabezpieczeń serwera rozszerzonej.

5. *Czy można używać domeny niestandardowej HTTPS z usługi Azure CDN from Akamai?*

    Obecnie ta funkcja jest dostępna tylko z usługą Azure CDN from Verizon. Firma Microsoft pracuje Obsługa tej funkcji w programie Azure CDN from Akamai w najbliższych miesiącach.

6. *Należy rekord certyfikatu urzędu autoryzacji z mojego dostawcy DNS?*
   Nie, rekord certyfikatu urzędu autoryzacji nie jest obecnie wymagane. Jednak jeśli istnieje, musi on zawierać DigiCert jako prawidłowego urzędu certyfikacji.


## <a name="next-steps"></a>Następne kroki

- Dowiedz się, jak skonfigurować [niestandardową domenę na punkt końcowy usługi Azure CDN](./cdn-map-content-to-custom-domain.md)


