---
title: "Wprowadzenie do usługi Azure CDN | Microsoft Docs"
description: "W tym temacie przedstawiono sposób włączania usługi Azure Content Delivery Network (CDN). Samouczek przeprowadza przez tworzenie nowego profilu i punktu końcowego usługi CDN."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 4ca51224-5423-419b-98cf-89860ef516d2
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/25/2018
ms.author: mazha
ms.openlocfilehash: 81a88f6495ca9092ca3b55b8ffb3e41def3b4623
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/29/2018
---
# <a name="getting-started-with-azure-cdn"></a>Wprowadzenie do usługi Azure CDN
W tym artykule opisano włączanie usługi Azure Content Delivery Network (CDN) przez utworzenie nowego profilu i punktu końcowego usługi CDN.

> [!IMPORTANT]
> Wprowadzenie do usługi CDN i listę funkcji można znaleźć na stronie [Omówienie usługi CDN](cdn-overview.md).
> 
> 

## <a name="create-a-new-cdn-profile"></a>Tworzenie nowego profilu CDN
Profil CDN jest kolekcją punktów końcowych usługi CDN. Każdy profil może zawierać jeden lub więcej punktów końcowych usługi CDN. Do organizowania punktów końcowych usługi CDN według domeny internetowej, aplikacji internetowej lub innych kryteriów można używać wielu profilów.

> [!NOTE]
> Subskrypcja platformy Azure ma limity domyślne dla następujących zasobów:
> - Liczba profilów CDN, które można utworzyć
> - Liczba punktów końcowych, które można utworzyć w profilu CDN 
> - Liczba domen niestandardowych, które można zamapować na punkt końcowy
>
> Aby dowiedzieć się więcej o limitach subskrypcji usługi CDN, zobacz [Limity CDN](https://docs.microsoft.com/azure/azure-subscription-service-limits#cdn-limits).
>
> Cennik usługi CDN jest stosowany na poziomie profilu CDN. Dlatego, aby używać różnych warstw cenowych usługi Azure CDN, musisz utworzyć wiele profilów usługi CDN.
> 
> 

[!INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]

## <a name="create-a-new-cdn-endpoint"></a>Tworzenie nowego punktu końcowego usługi CDN
**Aby utworzyć nowy punkt końcowy usługi CDN**

1. W witrynie [Azure Portal](https://portal.azure.com) przejdź do swojego profilu CDN. Być może został on przypięty do pulpitu nawigacyjnego w poprzednim kroku. Jeśli nie, możesz go znaleźć, wybierając kolejno pozycje **Wszystkie usługi** i **Profile CDN**. W okienku **Profile CDN** wybierz profil, do którego planujesz dodać punkt końcowy. 
   
    Zostanie wyświetlone okienko profilu CDN.
   
    ![Profil CDN][cdn-profile-settings]

2. Wybierz pozycję **Punkt końcowy**.
   
    ![Przycisk dodawania punktu końcowego][cdn-new-endpoint-button]
   
    Zostanie wyświetlone okienko **Dodawanie punktu końcowego**.
   
    ![Okienko Dodawanie punktu końcowego][cdn-add-endpoint]

3. W polu **Nazwa** wprowadź unikatową nazwę nowego punktu końcowego CDN. Ta nazwa jest używana do uzyskiwania dostępu do buforowanych zasobów w domenie `<endpointname>.azureedge.net`.

4. W polu **Typ źródła** wybierz typ źródła. Wybierz typ **Storage** dla konta usługi Azure Storage, **Usługa w chmurze** dla usługi Azure Cloud Service, **Web App** dla usługi Azure Web App lub **Źródło niestandardowe** dla każdego innego publicznie dostępnego źródła serwera sieci Web (hostowanego na platformie Azure lub gdziekolwiek indziej).
   
    ![Typ źródła usługi CDN](./media/cdn-create-new-endpoint/cdn-origin-type.png)

5. W polu **Nazwa hosta źródła** wybierz lub wprowadź domenę źródła. Na liście rozwijanej są wyświetlane wszystkie dostępne źródła typu określonego w kroku 4. W przypadku wybrania pozycji **Źródło niestandardowe** jako typu źródła wprowadź domenę źródła niestandardowego.
    
6. W polu **Ścieżka do źródła** wprowadź ścieżkę do zasobów, które chcesz buforować w pamięci podręcznej, lub pozostaw to pole puste, aby umożliwić buforowanie dowolnego zasobu w domenie określonej w kroku 5.
    
7. W polu **Nagłówek hosta źródła** wprowadź nagłówek hosta, który usługa Azure CDN ma wysłać z każdym żądaniem, lub pozostaw wartość domyślną.
   
   > [!WARNING]
   > Niektóre typy źródeł, takie jak usługi Azure Storage i Web Apps, wymagają, aby nagłówek hosta był zgodny z domeną źródła. Pozostaw wartość domyślną, chyba że masz źródło, które wymaga nagłówka hosta innego niż jego domena.
   > 
    
8. W obszarach **Protokół** i **Port źródła** określ protokoły i porty używane do uzyskiwania dostępu do zasobów w źródle. Należy wybrać co najmniej jeden protokół (HTTP lub HTTPS). Aby uzyskać dostęp do zawartości HTTPS, użyj domeny udostępnionej przez usługę CDN (`<endpointname>.azureedge.net`). 
   
   > [!NOTE]
   > Ustawienie **Port źródła** określa tylko port używany przez punkt końcowy do pobierania informacji ze źródła. Sam punkt końcowy jest dostępny dla klientów końcowych tylko na domyślnych portach HTTP i HTTPS (80 i 443), niezależnie od wartości **Port źródła**.  
   > 
   > Punkty końcowe w profilach usługi **Azure CDN from Akamai** nie zezwalają na pełny zakres portów TCP dla portów źródeł. Lista niedozwolonych portów źródłowych znajduje się w artykule [Azure CDN from Akamai Allowed Origin Ports](https://msdn.microsoft.com/library/mt757337.aspx) (Azure CDN from Akamai — dozwolone porty źródłowe).  
   > 
   > Gdy uzyskujesz dostęp do zawartości usługi CDN przy użyciu protokołu HTTPS, istnieją następujące ograniczenia:
   > 
   > * Należy używać certyfikatu SSL dostarczonego przez usługę CDN. Certyfikaty innych firm nie są obsługiwane.
   > * Obsługa protokołu HTTPS dla domen niestandardowych usługi Azure CDN jest dostępna tylko w przypadku produktów usługi **Azure CDN from Verizon** (Standard i Premium). Nie jest on obsługiwany w produktach **Azure CDN from Akamai**. Aby uzyskać więcej informacji, zobacz [Konfigurowanie protokołu HTTPS w domenie niestandardowej usługi Azure CDN](cdn-custom-ssl.md).
    
9. Wybierz pozycję **Dodaj**, aby utworzyć nowy punkt końcowy.
   
   Po utworzeniu punktu końcowego zostanie on wyświetlony na liście punktów końcowych dla profilu.
    
   ![Punkt końcowy usługi CDN][cdn-endpoint-success]
    
   > [!IMPORTANT]
   > Punkt końcowy nie jest natychmiast dostępny do użycia, ponieważ propagacja rejestracji zajmuje trochę czasu. W przypadku profili usługi **Azure CDN from Akamai** propagacja zwykle trwa do minuty. W przypadku profili usługi **Azure CDN from Verizon** propagacja zwykle trwa do 90 minut, ale niekiedy może to zająć więcej czasu.
    > 
    > Jeśli spróbujesz użyć nazwy domeny usługi CDN przed zakończeniem propagacji konfiguracji punktu końcowego do lokalizacji POP, możesz otrzymać kod odpowiedzi HTTP 404. Jeśli od czasu utworzenia punktu końcowego minęło kilka godzin, a nadal otrzymujesz stan odpowiedzi 404, zapoznaj się z artykułem [Rozwiązywanie problemów z punktami końcowymi usługi CDN zwracającymi stany 404](cdn-troubleshoot-endpoint.md).
    > 
    > 

## <a name="see-also"></a>Zobacz też
* [Kontrolowanie zachowania buforowania żądań z ciągami zapytań](cdn-query-string.md)
* [Jak zamapować zawartość usługi CDN na domenę niestandardową](cdn-map-content-to-custom-domain.md)
* [Wstępne ładowanie zasobów w punkcie końcowym usługi Azure CDN](cdn-preload-endpoint.md)
* [Przeczyszczanie punktu końcowego usługi Azure CDN](cdn-purge-endpoint.md)
* [Rozwiązywanie problemów z punktami końcowymi usługi CDN zwracającymi stany 404](cdn-troubleshoot-endpoint.md)

[cdn-profile-settings]: ./media/cdn-create-new-endpoint/cdn-profile-settings.png
[cdn-new-endpoint-button]: ./media/cdn-create-new-endpoint/cdn-new-endpoint-button.png
[cdn-add-endpoint]: ./media/cdn-create-new-endpoint/cdn-add-endpoint.png
[cdn-endpoint-success]: ./media/cdn-create-new-endpoint/cdn-endpoint-success.png
