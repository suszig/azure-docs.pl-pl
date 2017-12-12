---
title: "Tłumaczenie łącza i serwer Proxy aplikacji adresów URL usługi Azure AD | Dokumentacja firmy Microsoft"
description: "Zawiera podstawowe informacje dotyczące serwera Proxy aplikacji usługi Azure AD łączników."
services: active-directory
documentationcenter: 
author: kgremban
manager: mtillman
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 877c4491b3ebc2164f7cb2ecb9392d9c71e1667a
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/11/2017
---
# <a name="redirect-hardcoded-links-for-apps-published-with-azure-ad-application-proxy"></a>Przekieruj zapisane na stałe łącza do aplikacji opublikowanych przy użyciu serwera Proxy aplikacji usługi Azure AD

Serwer Proxy aplikacji usługi Azure AD udostępnia aplikacji lokalnej do użytkowników, którzy są zdalnej lub na ich własnych urządzeń. Niektóre aplikacje, jednak zostały opracowane z lokalnych łączy osadzone w kodzie HTML. Tych linków nie działa prawidłowo po aplikacji jest używany zdalnie. Masz punktu ze sobą kilka aplikacji lokalnych użytkowników oczekiwać łącza, aby kontynuować pracę, gdy nie są one w biurze. 

Najlepszym sposobem upewnij się, że łącza działają taki sam zarówno wewnątrz, jak i poza siecią firmową jest skonfigurowanie zewnętrzne adresy URL aplikacji do być taka sama jak ich wewnętrzne adresy URL. Użyj [domen niestandardowych](active-directory-application-proxy-custom-domains.md) do konfigurowania sieci zewnętrzne adresy URL mają nazwę domeny firmowej zamiast domyślnej domeny serwera proxy aplikacji.

Jeśli domen niestandardowych nie można użyć w dzierżawie, funkcja tłumaczenia łączenia serwera proxy aplikacji zachowuje łącza działa niezależnie od tego, gdzie znajdują się użytkownicy. Jeśli masz aplikacje, które wskazują bezpośrednio do wewnętrznych punktów końcowych lub portów, możesz mapować te wewnętrzne adresy URL opublikowanych zewnętrzny URL serwera Proxy aplikacji. Po włączeniu tłumaczenia łącza, i wyszukuje serwer Proxy aplikacji za pomocą kodu HTML, CSS i JavaScript znaczniki dla łączy wewnętrznych opublikowane. Usługa Serwer Proxy aplikacji przetwarza je tak, aby użytkownicy pobierają nieprzerwaną środowisko.

>[!NOTE]
>Funkcja tłumaczenia łącze jest dla dzierżawcy, niezależnie od przyczyny, nie można używać niestandardowych domen do mieć tych samych wewnętrznych i zewnętrznych adresów URL dla aplikacji. Przed włączeniem tej funkcji należy sprawdzić, czy [domen niestandardowych w serwera Proxy aplikacji usługi Azure AD](active-directory-application-proxy-custom-domains.md) można skorzystać.
>
>Lub, jeśli aplikacja, konieczne będzie skonfigurowanie z łączem tłumaczenia jest programu SharePoint, zobacz [Konfigurowanie mapowań dostępu alternatywnego for SharePoint 2013](https://technet.microsoft.com/library/cc263208.aspx) innego podejścia do mapowania łącza.

## <a name="how-link-translation-works"></a>Jak połączyć działa tłumaczenia

Po uwierzytelnieniu gdy serwer proxy przekazuje dane aplikacji dla użytkownika serwera Proxy aplikacji skanuje aplikacji dla łącza zapisane na stałe i zastępuje je ich odpowiednich opublikowane zewnętrzne adresy URL.

Serwer Proxy aplikacji zakłada, że aplikacje są zakodowane w formacie UTF-8. Jeśli nie jest to wymagane, określ typ kodowania nagłówka odpowiedzi http, tak samo, jak `Content-Type:text/html;charset=utf-8`.

### <a name="which-links-are-affected"></a>Łączy, których dotyczy problem?

Funkcja tłumaczenia łącze wyszukuje tylko łącza, które znajdują się w kodu znaczników w treści aplikacji. Serwer Proxy aplikacji ma oddzielne funkcji tłumaczenia pliki cookie lub adresy URL w nagłówkach. 

Istnieją dwa typy typowe wewnętrzny łączy w lokalnych aplikacji:

- **Względne łączy wewnętrznych** czy punktu z zasobem udostępnionym w strukturze pliku lokalnego, takich jak `/claims/claims.html`. Te linki automatycznie działać w aplikacjach, które są publikowane za pośrednictwem serwera Proxy aplikacji i kontynuować pracę z lub bez tłumaczenia łącza. 
- **Łączy wewnętrznych zapisane na stałe** do innych aplikacji lokalnych, takich jak `http://expenses` lub opublikowane plików takich jak `http://expenses/logo.jpg`. Funkcja tłumaczenia łącze działa dla łączy wewnętrznych zapisane na stałe, a następnie zmienia je, aby wskazywał zewnętrzne adresy URL, które muszą przejść użytkowników zdalnych.

### <a name="how-do-apps-link-to-each-other"></a>Jak aplikacje są łączyć ze sobą?

Tłumaczenie łącze jest włączone dla każdej aplikacji, aby zachować kontrolę nad środowiskiem użytkownika na poziomie aplikacji. Włączyć łącze tłumaczenia dla aplikacji, gdy ma linki *z* , aplikacja ma zostać poddany translacji nie łączy *do* danej aplikacji. 

Na przykład załóżmy, że masz trzech aplikacji opublikowanych przy użyciu serwera Proxy aplikacji, że wszystkie połączyć ze sobą: korzyści, kosztów i podróży. Brak aplikacji czwarty opinii, która nie jest opublikowana przy użyciu serwera Proxy aplikacji.

Po włączeniu tłumaczenia łącze aplikacji korzyści łącza do kosztów i podróży są przekierowywane do zewnętrznych adresów URL dla tych aplikacji, ale nie zostanie przekierowana łącze do opinii, ponieważ nie istnieje żaden zewnętrzny adres URL. Łącza z kosztów i podróż do korzyści nie działa, ponieważ łącze tłumaczenia nie został włączony dla tych dwóch aplikacji.

![Łącza z zalet do innych aplikacji po włączeniu tłumaczenia łącza](./media/application-proxy-link-translation/one_app.png)

### <a name="which-links-arent-translated"></a>Łącza, które nie są tłumaczone?

W celu poprawy wydajności i bezpieczeństwa, nie są tłumaczone niektóre łącza:

- Łącza nie wewnątrz znaczników kodu. 
- Łącza nie w HTML, CSS i JavaScript. 
- Łączy wewnętrznych otwarty z innych programów. Nie można przetłumaczyć łącza wysyłanych za pośrednictwem poczty e-mail lub wiadomości błyskawicznych lub zawarte w innych dokumentów. Użytkownicy muszą wiedzieć, aby przejść do zewnętrznego adresu URL.

Jeśli potrzebujesz obsługuje jednej z tych dwóch scenariuszy, należy używać tych samych adresów URL wewnętrznych i zewnętrznych zamiast tłumaczenia łącze.  

## <a name="enable-link-translation"></a>Włącz łącze tłumaczenia

Wprowadzenie do tłumaczenia łącze jest tak proste, jak kliknięcie przycisku:

1. Zaloguj się do witryny [Azure Portal](https://portal.azure.com) jako administrator.
2. Przejdź do **usługi Azure Active Directory** > **aplikacje dla przedsiębiorstw** > **wszystkie aplikacje** > Wybierz aplikację, którymi chcesz zarządzać >  **Serwer proxy aplikacji**.
3. Włącz **translacji adresów URL w aplikacji treści** do **tak**.

   ![Wybierz opcję Tak, aby translacji adresów URL w treści aplikacji](./media/application-proxy-link-translation/select_yes.png).
4. Wybierz **zapisać** Aby zastosować zmiany.

Teraz gdy użytkownicy uzyskują dostęp do tej aplikacji, serwer proxy będzie automatycznie skanowanie w poszukiwaniu wewnętrzne adresy URL, które zostały opublikowane za pośrednictwem serwera Proxy aplikacji w dzierżawie.

## <a name="send-feedback"></a>Wyślij opinię

Chcemy Twojej pomocy, aby ta funkcja działa w przypadku wszystkich aplikacji. Firma Microsoft wyszukiwania ponad 30 tagów w kodzie HTML i CSS i rozważa którego przypadków JavaScript do obsługi. Przykład wygenerowanego linki, które nie są tłumaczonym, przesyłać fragmentu kodu [opinii serwera Proxy aplikacji](mailto:aadapfeedback@microsoft.com). 

## <a name="next-steps"></a>Następne kroki
[Domen niestandardowych za pomocą serwera Proxy aplikacji usługi Azure AD](active-directory-application-proxy-custom-domains.md) mają ten sam adres URL wewnętrznych i zewnętrznych

[Konfigurowanie mapowań dostępu alternatywnego for SharePoint 2013](https://technet.microsoft.com/library/cc263208.aspx)
