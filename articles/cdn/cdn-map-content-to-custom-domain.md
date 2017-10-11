---
title: "Mapuj zawartość Azure CDN na domenę niestandardową | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak zamapować zawartość usługi Azure CDN na domenę niestandardową."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 289f8d9e-8839-4e21-b248-bef320f9dbfc
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: cd6db44f7776859d1e6a893543cf0666182ca41a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="map-azure-cdn-content-to-a-custom-domain"></a>Mapuj zawartość Azure CDN na domenę niestandardową
Aby można było korzystać z własnej nazwy domeny w adresach URL do zawartości w pamięci podręcznej, a nie za pomocą poddomeny azureedge.net pliku można zamapować niestandardową domenę do punktu końcowego usługi CDN.

Istnieją dwa sposoby, aby zamapować domenę niestandardową do punktu końcowego usługi CDN:

1. [Utwórz rekord CNAME u rejestratora domen i mapowania domeny niestandardowej, a poddomeny na punkt końcowy CDN](#register-a-custom-domain-for-an-azure-cdn-endpoint).
   
    Rekord CNAME jest funkcją systemu DNS, która mapuje domenę źródłową, taką jak `www.contosocdn.com` lub `cdn.contoso.com`, na domenę docelową. W takim przypadku domeny źródłowej jest Twoje niestandardowe domen i poddomen (poddomeny, takiej jak **www** lub **cdn** jest zawsze wymagane). Domena docelowego jest punktu końcowego CDN.  
   
    Proces mapowania domeny niestandardowej do punktu końcowego CDN jednak można powodowały przez krótki czas przestoju dla domeny podczas rejestrujesz domenę w portalu Azure.
2. [Dodaj etap pośredni rejestracji z **cdnverify**](#register-a-custom-domain-for-an-azure-cdn-endpoint-using-the-intermediary-cdnverify-subdomain)
   
    Jeśli aktualnie obsługiwanych aplikacji przy użyciu umowy poziomu usług (SLA), który wymaga istnienia bez przestojów domenę niestandardową, a następnie platformy Azure można użyć **cdnverify** poddomeny zapewnienie etap pośredni rejestracji dzięki czemu użytkownicy będą mogli na dostęp do Twojej domeny gdy mapowanie DNS ma miejsce.  

Po zarejestrowaniu domeny niestandardowej przy użyciu jednej z powyższych procedur można [Sprawdź, czy niestandardowe poddomeny odwołuje się do punktu końcowego CDN](#verify-that-the-custom-subdomain-references-your-cdn-endpoint).

> [!NOTE]
> Należy utworzyć rekord CNAME u rejestratora domen, aby zamapować domenę na punkt końcowy CDN. Rekordy CNAME mapy określonych poddomen, takich jak `www.contoso.com` lub `cdn.contoso.com`. Nie jest możliwe do mapowania rekordu CNAME domeny głównej, takich jak `contoso.com`.
> 
> Poddomeny może być skojarzony tylko z jednym punktem końcowym CDN. Rekord CNAME, który utworzono będzie przekierowywać wszystkie ruchu kierowanego do poddomeny do określonego punktu końcowego.  Na przykład, jeśli użytkownik jest kojarzony `www.contoso.com` z punktu końcowego CDN, możesz nie można powiązać go z innymi punkty końcowe systemu Azure, takich jak punkt końcowy konta magazynu lub punkt końcowy usługi w chmurze. Jednak można użyć różnych domen podrzędnych w tej samej domenie dla punktów końcowych innej usługi. Można również mapować różnych domen podrzędnych do tego samego punktu końcowego CDN.
> 
> Aby uzyskać **Azure CDN from Verizon** punktów końcowych (Standard i Premium), należy pamiętać, że trwa maksymalnie **90 minut** zmiany domeny niestandardowej CDN obejmie krawędzi węzłów.
> 
> 

## <a name="register-a-custom-domain-for-an-azure-cdn-endpoint"></a>Zarejestruj domeny niestandardowej dla punktu końcowego usługi Azure CDN
1. Zaloguj się do [portalu Azure](https://portal.azure.com/).
2. Kliknij przycisk **Przeglądaj**, następnie **profilów usługi CDN**, następnie profilu CDN z punktem końcowym, który chcesz Mapuj na domenę niestandardową.  
3. W **profilu CDN** bloku, kliknij punkt końcowy CDN, z którym chcesz powiązać poddomeny.
4. W górnej części bloku punktu końcowego, kliknij przycisk **Dodawanie domeny niestandardowe** przycisku.  W **dodać niestandardową domenę** bloku, wyświetlana nazwa hosta punktu końcowego, pochodzących z punktu końcowego CDN, do użycia podczas tworzenia nowego rekordu CNAME. Format adresu nazwa hosta będzie wyświetlany jako  **&lt;EndpointName >. azureedge.net**.  Możesz skopiować tę nazwę hosta do użycia w tworzenie rekordu CNAME.  
5. Przejdź do witryny sieci Web swojego rejestratora domen i znajdź sekcję tworzenia rekordów DNS. Może ona być zlokalizowana w sekcjach, takich jak **Domain Name** (Nazwa domeny), **DNS** (System DNS) lub **Name Server Management** (Zarządzanie serwerem nazw).
6. Znajdź sekcję zarządzania rekordami CNAME. W tym celu może być konieczne przejście do strony ustawień zaawansowanych i poszukanie słów CNAME, Alias lub Subdomains (Poddomeny).
7. Utwórz nowy rekord CNAME mapujący z wybranym poddomeny (na przykład **www** lub **cdn**) na nazwę hosta dostarczone w **dodać niestandardową domenę** bloku. 
8. Wróć do **dodać niestandardową domenę** bloku, a następnie wprowadź domenę niestandardową, w tym poddomeny, w oknie dialogowym. Na przykład, wprowadź nazwę domeny w formacie `www.contoso.com` lub `cdn.contoso.com`.   
   
   Azure Sprawdź, czy istnieje rekord CNAME dla nazwy domeny, który został wprowadzony. Jeśli rekord CNAME jest poprawny, domena niestandardowa jest weryfikowana.  Aby uzyskać **Azure CDN from Verizon** punktów końcowych (Standard i Premium), może potrwać do 90 minut Propagacja do wszystkich węzłów CDN krawędzi, jednak ustawień domeny niestandardowej.  
   
   Należy pamiętać, że w niektórych przypadkach może trwać czas dla rekordu CNAME obejmie serwery nazw w Internecie. Jeśli domena nie została zweryfikowana natychmiast i uważasz, że rekord CNAME jest poprawna, zaczekaj kilka minut i spróbuj ponownie.

## <a name="register-a-custom-domain-for-an-azure-cdn-endpoint-using-the-intermediary-cdnverify-subdomain"></a>Zarejestruj domeny niestandardowej dla punktu końcowego usługi Azure CDN przy użyciu cdnverify pośredniczące poddomeny
1. Zaloguj się do [portalu Azure](https://portal.azure.com/).
2. Kliknij przycisk **Przeglądaj**, następnie **profilów usługi CDN**, następnie profilu CDN z punktem końcowym, który chcesz Mapuj na domenę niestandardową.  
3. W **profilu CDN** bloku, kliknij punkt końcowy CDN, z którym chcesz powiązać poddomeny.
4. W górnej części bloku punktu końcowego, kliknij przycisk **Dodawanie domeny niestandardowe** przycisku.  W **dodać niestandardową domenę** bloku, wyświetlana nazwa hosta punktu końcowego, pochodzących z punktu końcowego CDN, do użycia podczas tworzenia nowego rekordu CNAME. Format adresu nazwa hosta będzie wyświetlany jako  **&lt;EndpointName >. azureedge.net**.  Możesz skopiować tę nazwę hosta do użycia w tworzenie rekordu CNAME.
5. Przejdź do witryny sieci Web swojego rejestratora domen i znajdź sekcję tworzenia rekordów DNS. Może ona być zlokalizowana w sekcjach, takich jak **Domain Name** (Nazwa domeny), **DNS** (System DNS) lub **Name Server Management** (Zarządzanie serwerem nazw).
6. Znajdź sekcję zarządzania rekordami CNAME. Może być konieczne przejdź do strony ustawień zaawansowanych i poszukaj słowa **CNAME**, **Alias**, lub **poddomen**.
7. Utwórz nowy rekord CNAME i podaj alias poddomeny, która obejmuje **cdnverify** poddomeny. Na przykład będzie poddomeny, którą można określić w formacie **cdnverify.www** lub **cdnverify.cdn**. Następnie podaj nazwę hosta, który jest punkt końcowy CDN, w formacie **cdnverify.&lt; EndpointName >. azureedge.net**. Mapowanie DNS powinien wyglądać podobnie jak:`cdnverify.www.consoto.com   CNAME   cdnverify.consoto.azureedge.net`  
8. Wróć do **dodać niestandardową domenę** bloku, a następnie wprowadź domenę niestandardową, w tym poddomeny, w oknie dialogowym. Na przykład, wprowadź nazwę domeny w formacie `www.contoso.com` lub `cdn.contoso.com`. Uwaga: w tym kroku, nie należy do domeny podrzędnej z jest poprzedzony **cdnverify**.  
   
    Azure Sprawdź, czy istnieje rekord CNAME dla nazwy domeny cdnverify, który został wprowadzony.
9. W tym momencie domeny niestandardowej została zweryfikowana przez platformę Azure, ale nie ruch do Twojej domeny jest jeszcze rozsyłane do punktu końcowego CDN. Po upływie wystarczająco długi, aby umożliwić ustawienia domeny niestandardowej CDN obejmie krawędzi węzłów (90 minut **Azure CDN from Verizon**, 1 – 2 minuty dla **Azure CDN from Akamai**), należy ponownie serwery DNS Witryna sieci web rejestratora i utwórz inny rekord CNAME mapujący z poddomeny na punkt końcowy CDN. Na przykład określić poddomeny jako **www** lub **cdn**i nazwy hosta jako  **&lt;EndpointName >. azureedge.net**. W ramach tego kroku zakończeniu rejestracji domeny niestandardowej.
10. Ponadto możesz usunąć rekord CNAME, które zostały utworzone za pomocą **cdnverify**, ponieważ konieczne było tylko jako etap pośredniczące.  

## <a name="verify-that-the-custom-subdomain-references-your-cdn-endpoint"></a>Sprawdź, czy niestandardowe poddomeny odwołuje się do punktu końcowego CDN
* Po zakończeniu rejestracji domeny niestandardowej jest dostępna zawartość jest buforowana na punkt końcowy CDN przy użyciu domeny niestandardowej.
  Najpierw upewnić się, że masz publicznego zawartość jest buforowana w punkcie końcowym. Na przykład jeśli punkt końcowy CDN jest skojarzony z kontem magazynu, sieci CDN buforuje zawartość w kontenerach publicznego obiektu blob. Aby przetestować domeny niestandardowej, upewnij się, czy z kontenera ustawiono zezwolić na dostęp publiczny i czy zawiera on co najmniej jeden obiekt blob.
* W przeglądarce przejdź do adresu obiektu blob przy użyciu domeny niestandardowej. Na przykład, jeśli domena niestandardowa jest `cdn.contoso.com`, adres URL do pamięci podręcznej blob będzie podobny do następującego adresu URL: http://cdn.contoso.com/mypubliccontainer/acachedblob.jpg

## <a name="see-also"></a>Zobacz też
[Jak włączyć sieci dostarczania zawartości (CDN) dla platformy Azure](cdn-create-new-endpoint.md)  

