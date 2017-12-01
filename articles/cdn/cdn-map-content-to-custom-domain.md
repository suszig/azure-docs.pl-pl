---
title: "Dodaj niestandardową domenę do punktu końcowego CDN | Dokumentacja firmy Microsoft"
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
ms.date: 10/09/2017
ms.author: mazha
ms.openlocfilehash: fd36b94c64ad31064dbb2e0badceaee5e5bc400f
ms.sourcegitcommit: 5a6e943718a8d2bc5babea3cd624c0557ab67bd5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/01/2017
---
# <a name="add-a-custom-domain-to-your-cdn-endpoint"></a>Dodaj niestandardową domenę do punktu końcowego CDN
Po utworzeniu profilu zazwyczaj tworzony jest również co najmniej jeden CDN [punkty końcowe](cdn-create-new-endpoint.md#create-a-new-cdn-endpoint) (poddomeną `azureedge.net`) do dostarczania zawartości przy użyciu protokołu HTTP i HTTPS. Domyślnie ten punkt końcowy jest uwzględniona w wszystkie adresy URL (na przykład `https://contoso.azureedge.net/photo.png`). Dla wygody użytkownika usługi Azure CDN umożliwia skojarzenie niestandardową domenę (na przykład `www.contoso.com`) z punktu końcowego. Po wybraniu tej opcji umożliwia domeny niestandardowej dostarczać swoją zawartość zamiast punktu końcowego. Ta opcja jest przydatna, jeśli na przykład chcesz z własnej nazwy domeny mają być widoczne dla klientów na potrzeby znakowania.

Jeśli nie masz już domenę niestandardową, należy najpierw zakupić z dostawcy domeny. Po uzyskaniu niestandardową domenę, wykonaj następujące kroki:
1. [Rekordy DNS domeny dostawcy dostępu](#step-1-access-dns-records-by-using-your-domain-provider)
2. [Tworzenie rekordów CNAME DNS](#step-2-create-the-cname-dns-records)
    - Opcja 1: Bezpośrednie mapowanie domeny niestandardowej do punktu końcowego CDN
    - Opcja 2: Mapowanie domeny niestandardowej z punktem końcowym CDN przy użyciu cdnverify 
3. [Włącz mapowania rekord CNAME w systemie Azure](#step-3-enable-the-cname-record-mapping-in-azure)
4. [Sprawdź, czy niestandardowe poddomeny odwołuje się do punktu końcowego CDN](#step-4-verify-that-the-custom-subdomain-references-your-cdn-endpoint)
5. [(Krok zależnych) Mapuj stałe domeny niestandardowej do punktu końcowego CDN](#step-5-dependent-step-map-the-permanent-custom-domain-to-the-cdn-endpoint)

## <a name="step-1-access-dns-records-by-using-your-domain-provider"></a>Krok 1: Dostępu DNS rejestruje przy użyciu dostawcy usługi domeny

Jeśli używasz platformy Azure do hosta z [domen DNS](https://docs.microsoft.com/en-us/azure/dns/dns-overview), należy przekazać dostawcę domeny DNS do usługi Azure DNS. Aby uzyskać więcej informacji, zobacz [Delegowanie domeny do usługi Azure DNS](https://docs.microsoft.com/azure/dns/dns-delegate-domain-azure-dns)

W przeciwnym razie jeśli korzystasz z dostawcą domeny do obsługi domeny DNS, zaloguj się do witryny sieci Web dostawcy domeny. Znajdź stronę zarządzania rekordami DNS konsultacji dokumentacji dostawcy lub wyszukiwanie obszarów witryny sieci web z etykietą **nazwy domeny**, **DNS**, lub **nazwy serwera zarządzania**. Często znajdziesz stronie rekordy DNS, wyświetlanie informacji o koncie i wyszukiwanie łącza, takie jak **mojej domeny**. Niektórzy dostawcy mają różne łącze, aby dodać różnych typów rekordów.

> [!NOTE]
> W przypadku niektórych dostawców, np. GoDaddy, zmiany rekordów DNS nie zaczynają obowiązywać, dopóki nie wybierzesz oddzielnego linku **Zapisz zmiany**. 


## <a name="step-2-create-the-cname-dns-records"></a>Krok 2: Tworzenie rekordów CNAME DNS

Zanim użyjesz domeny niestandardowej z punktem końcowym usługi Azure CDN, musisz najpierw utworzyć rekord nazwę kanoniczną (CNAME) przy pomocy dostawcy usługi domeny. Rekord CNAME jest typem rekordu w systemie nazw domen (DNS), mapująca domeny źródłowej do domeny docelowej, określając nazwę domeny aliasu dla nazwy domeny "canonical" lub wartość true. Dla usługi Azure CDN domeny źródłowej jest domen niestandardowych (i poddomeny) i domeny docelowej jest punktu końcowego CDN. Usługi Azure CDN sprawdza rekord CNAME DNS podczas dodawania domeny niestandardowej z punktem końcowym w portalu lub interfejsu API. 

Rekord CNAME mapuje określonych domen i poddomen, takich jak `www.contoso.com` lub `cdn.contoso.com`; nie jest możliwe do mapowania rekordu CNAME domeny głównej, takich jak `contoso.com`. Poddomeny może być skojarzony z tylko jednym punktem końcowym CDN i rekord CNAME, który utworzono będzie przekierowywać wszystkie ruchu kierowanego do poddomeny do określonego punktu końcowego. Na przykład, jeśli użytkownik jest kojarzony `www.contoso.com` z punktu końcowego CDN, nie można skojarzyć go z innego punktu końcowego platformy Azure, takich jak punkt końcowy konta magazynu lub punkt końcowy usługi w chmurze. Jednak można użyć różnych domen podrzędnych w tej samej domenie dla punktów końcowych innej usługi. Można również mapować różnych domen podrzędnych do tego samego punktu końcowego CDN.

Aby zamapować domenę niestandardową do punktu końcowego usługi CDN, użyj jednej z następujących opcji:

- Opcja 1: Bezpośredniego mapowania. Jeśli żaden ruch produkcji jest uruchomiona na domenę niestandardową, można zamapować niestandardową domenę do punktu końcowego usługi CDN bezpośrednio. Proces mapowania domeny niestandardowej do punktu końcowego CDN może spowodować przez krótki czas przestoju dla domeny podczas rejestrujesz domenę w portalu Azure. Wpis mapowanie CNAME powinny być w następującym formacie: 
 
  | NAZWA             | TYP  | WARTOŚĆ                  |
  |------------------|-------|------------------------|
  | www\.contoso.com | CNAME | Contoso\.azureedge.net |


- Opcja 2: Mapowanie, uwzględniając **cdnverify** poddomeny. Jeśli ruch produkcji, który nie można przerwać działa na domenę niestandardową, można utworzyć tymczasowego mapowanie CNAME do punktu końcowego CDN. Po wybraniu tej opcji Użyj platformy Azure **cdnverify** poddomeny zapewnienie etap pośredni rejestracji, tak, aby użytkownicy mieli dostęp do domeny bez przerwy podczas mapowania DNS ma miejsce.

   1. Utwórz nowy rekord CNAME i podaj alias poddomeny, która obejmuje **cdnverify** poddomeny. Na przykład **cdnverify.www** lub **cdnverify.cdn**. 
   2. Wprowadź nazwę hosta, który jest punkt końcowy sieci CDN w następującym formacie: `cdnverify.<EndpointName>.azureedge.net`. Wpis mapowanie CNAME powinny być w następującym formacie: 

   | NAZWA                       | TYP  | WARTOŚĆ                            |
   |----------------------------|-------|----------------------------------|
   | cdnverify.www\.contoso.com | CNAME | cdnverify.contoso\.azureedge.net | 


## <a name="step-3-enable-the-cname-record-mapping-in-azure"></a>Krok 3: Włącz mapowania rekord CNAME w systemie Azure

Po zarejestrowaniu domeny niestandardowej przy użyciu jednej z powyższych procedur można następnie włączyć funkcję domeny niestandardowej w usłudze Azure CDN. 

1. Zaloguj się do [portalu Azure](https://portal.azure.com/) i przejdź do profilu CDN z punktem końcowym do mapowania na domenę niestandardową.  
2. W **profilu CDN** bloku, wybierz punkt końcowy CDN, z którym chcesz powiązać poddomeny.
3. W lewym górnym rogu bloku punktu końcowego, kliknij przycisk **domeny niestandardowe**. 

   ![Przycisk domeny niestandardowej](./media/cdn-map-content-to-custom-domain/cdn-custom-domain-button.png)

4. W **niestandardowej nazwy hosta** tekst wprowadź domenę niestandardową, w tym poddomeny. Na przykład `www.contoso.com` lub `cdn.contoso.com`.

   ![Dodaj okno dialogowe domeny niestandardowej](./media/cdn-map-content-to-custom-domain/cdn-add-custom-domain-dialog.png)

5. Kliknij pozycję **Dodaj**.

   Platforma Azure sprawdzi, czy dla wprowadzonej nazwy domeny istnieje rekord CNAME. Jeśli rekord CNAME jest poprawny, domena niestandardowa jest weryfikowana. Może upłynąć trochę czasu, zanim rekord CNAME obejmie serwery nazw. Jeśli domena nie została zweryfikowana natychmiast, sprawdź, czy rekord CNAME jest poprawna, a następnie poczekaj kilka minut i spróbuj ponownie. Aby uzyskać **Azure CDN from Verizon** punktów końcowych (Standard i Premium), może potrwać do 90 minut dla domen niestandardowych ustawień obejmie wszystkie węzły krawędzi CDN.  


## <a name="step-4-verify-that-the-custom-subdomain-references-your-cdn-endpoint"></a>Krok 4: Sprawdź, czy niestandardowe poddomeny odwołuje się do punktu końcowego CDN

Po zakończeniu rejestracji domenę niestandardową, sprawdź, czy niestandardowe poddomeny odwołuje się do punktu końcowego CDN.
 
1. Upewnij się, że masz publicznego zawartość jest buforowana w punkcie końcowym. Na przykład jeśli punkt końcowy CDN jest skojarzony z kontem magazynu, sieci CDN buforuje zawartość w kontenerach publicznego obiektu blob. Aby przetestować domeny niestandardowej, sprawdź, czy z kontenera jest ustawiona, aby zezwolić na publiczny dostęp i zawiera co najmniej jednego obiektu blob.

2. W przeglądarce przejdź do adresu obiektu blob przy użyciu domeny niestandardowej. Na przykład, jeśli domena niestandardowa jest `cdn.contoso.com`, adres URL do pamięci podręcznej blob powinny być podobne do następującego adresu URL: `http://cdn.contoso.com/mypubliccontainer/acachedblob.jpg`.


## <a name="step-5-dependent-step-map-the-permanent-custom-domain-to-the-cdn-endpoint"></a>Krok 5 (krok zależnych): Mapuj stałe domeny niestandardowej do punktu końcowego CDN

Ten krok jest zależna od kroku 2, opcja 2 (mapowanie, uwzględniając **cdnverify** poddomeny). Jeśli używasz tymczasowy **cdnverify** poddomeny i sprawdzeniu, czy działa, możesz następnie mapować domenę niestandardową stałych do punktu końcowego CDN.

1. W witrynie sieci web dostawcy domeny należy utworzyć rekord CNAME DNS do mapowania stałego domeny niestandardowej punktu końcowego CDN. Wpis mapowanie CNAME powinny być w następującym formacie: 
 
   | NAZWA             | TYP  | WARTOŚĆ                  |
   |------------------|-------|------------------------|
   | www\.contoso.com | CNAME | Contoso\.azureedge.net |
2. Usuń rekord CNAME z **cdnverify** poddomeny, która została wcześniej utworzona.

## <a name="see-also"></a>Zobacz też
[Jak włączyć sieci dostarczania zawartości (CDN) dla platformy Azure](cdn-create-new-endpoint.md)  
[Delegowanie domeny do usługi Azure DNS](../dns/dns-domain-delegation.md)
