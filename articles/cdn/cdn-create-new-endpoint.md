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
ms.date: 01/23/2017
ms.author: mazha
translationtype: Human Translation
ms.sourcegitcommit: bdf6e27463fcc6186a3b15a55653fa468da91bdc
ms.openlocfilehash: d263e911d0d0b3cdc1e48e300a3c8a0994b38c39


---
# <a name="getting-started-with-azure-cdn"></a>Wprowadzenie do usługi Azure CDN
W tym temacie opisano włączanie usługi Azure CDN z tworzeniem nowego profilu i punktu końcowego usługi CDN.

> [!IMPORTANT]
> Wprowadzenie do sposobu działania usługi Azure CDN oraz lista funkcji zostały przedstawione w artykule [Omówienie usługi CDN](cdn-overview.md).
> 
> 

## <a name="create-a-new-cdn-profile"></a>Tworzenie nowego profilu CDN
Profil CDN jest kolekcją punktów końcowych usługi CDN.  Każdy profil zawiera jeden lub więcej punktów końcowych usługi CDN.  Możesz używać wielu profilów, aby organizować punkty końcowe usługi CDN według domeny internetowej, aplikacji sieci Web lub innych kryteriów.

> [!NOTE]
> Domyślnie jedna subskrypcja platformy Azure jest ograniczona do ośmiu profilów usługi CDN. Każdy profil CDN jest ograniczony do dziesięciu punktów końcowych usługi CDN.
> 
> Cennik usługi CDN jest stosowany na poziomie profilu CDN. Jeśli chcesz używać różnych warstw cenowych usługi Azure CDN, musisz mieć wiele profilów CDN.
> 
> 

[!INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]

## <a name="create-a-new-cdn-endpoint"></a>Tworzenie nowego punktu końcowego usługi CDN
**Aby utworzyć nowy punkt końcowy usługi CDN**

1. W witrynie [Azure Portal](https://portal.azure.com) przejdź do swojego profilu CDN.  Być może został on przypięty do pulpitu nawigacyjnego w poprzednim kroku.  Jeśli nie, możesz go znaleźć, klikając pozycję **Przeglądaj**, następnie pozycję **Profile CDN** i klikając profil, do którego planujesz dodać punkt końcowy.
   
    Zostanie wyświetlony blok profilu CDN.
   
    ![Profil CDN][cdn-profile-settings]
2. Kliknij przycisk **Dodaj punkt końcowy**.
   
    ![Przycisk dodawania punktu końcowego][cdn-new-endpoint-button]
   
    Zostanie wyświetlony blok **Dodawanie punktu końcowego**.
   
    ![Blok dodawania punktu końcowego][cdn-add-endpoint]
3. Wprowadź **nazwę** dla tego punktu końcowego usługi CDN.  Ta nazwa będzie używana do uzyskiwania dostępu do buforowanych zasobów w domenie `<endpointname>.azureedge.net`.
4. Na liście rozwijanej **Typ źródła** wybierz typ źródła.  Wybierz typ **Storage** dla konta usługi Azure Storage, **Usługa w chmurze** dla usługi Azure Cloud Service, **Web App** dla usługi Azure Web App lub **Źródło niestandardowe** dla każdego innego publicznie dostępnego źródła serwera sieci Web (hostowanego na platformie Azure lub gdziekolwiek indziej).
   
    ![Typ źródła usługi CDN](./media/cdn-create-new-endpoint/cdn-origin-type.png)
5. Na liście rozwijanej **Nazwa hosta źródła** wybierz lub wpisz domenę źródła.  Na liście rozwijanej będą wyświetlane wszystkie dostępne źródła typu określonego w kroku 4.  W przypadku wybrania opcji *Źródło niestandardowe* w polu **Typ źródła**, wpisz domenę źródła niestandardowego.
6. W polu tekstowym **Ścieżka do źródła** wprowadź ścieżkę do zasobów, które chcesz buforować w pamięci podręcznej, lub pozostaw to pole puste, aby umożliwić buforowanie dowolnego zasobu w domenie określonej w kroku 5.
7. W polu **Nagłówek hosta źródła** wprowadź nagłówek hosta, który usługa CDN ma wysłać z każdym żądaniem, lub pozostaw wartość domyślną.
   
   > [!WARNING]
   > Niektóre typy źródeł, takie jak usługi Azure Storage i Web Apps, wymagają, aby nagłówek hosta był zgodny z domeną źródła. Pozostaw wartość domyślną, chyba że masz źródło, które wymaga nagłówka hosta innego niż jego domena.
   > 
   > 
8. W obszarach **Protokół** i **Port źródła** określ protokoły i porty używane do uzyskiwania dostępu do zasobów w źródle.  Należy wybrać co najmniej jeden protokół (HTTP lub HTTPS).
   
   > [!NOTE]
   > Ustawienie **Port źródła** określa tylko port używany przez punkt końcowy do pobierania informacji ze źródła.  Sam punkt końcowy będzie dostępny dla klientów końcowych tylko na domyślnych portach HTTP i HTTPS (80 i 443), niezależnie od wartości **Port źródła**.  
   > 
   > Punkty końcowe usługi **Azure CDN from Akamai** nie zezwalają na pełny zakres portów TCP dla źródeł.  Lista niedozwolonych portów źródłowych znajduje się w artykule [Azure CDN from Akamai Allowed Origin Ports](https://msdn.microsoft.com/library/mt757337.aspx) (Azure CDN from Akamai — dozwolone porty źródłowe).  
   > 
   > Uzyskiwanie dostępu do zawartości usługi CDN przy użyciu protokołu HTTPS podlega następującym ograniczeniom:
   > 
   > * Należy używać certyfikatu SSL dostarczonego przez usługę CDN. Certyfikaty innych firm nie są obsługiwane.
   > * Należy użyć domeny udostępnionej przez usługę CDN (`<endpointname>.azureedge.net`) w celu uzyskania dostępu do zawartości HTTPS. Obsługa protokołu HTTPS jest niedostępna w przypadku niestandardowych nazw domen (rekordy CNAME), ponieważ usługa CDN obecnie nie obsługuje niestandardowych certyfikatów.
   > 
   > 
9. Kliknij przycisk **Dodaj**, aby utworzyć nowy punkt końcowy.
10. Po utworzeniu punktu końcowego zostanie on wyświetlony na liście punktów końcowych dla profilu. Widok listy zawiera adres URL służący do uzyskiwania dostępu do zawartości buforowanej (w pamięci podręcznej), a także domeny źródła.
    
    ![Punkt końcowy usługi CDN][cdn-endpoint-success]
    
    > [!IMPORTANT]
    > Punkt końcowy nie będzie natychmiast dostępny do użycia, ponieważ propagacja rejestracji w usłudze CDN zajmuje trochę czasu.  W przypadku profilów usługi <b>Azure CDN from Akamai</b> propagacja zwykle zostanie ukończona w ciągu minuty.  W przypadku profilów usługi <b>Azure CDN from Verizon</b> propagacja zwykle zostanie ukończona w ciągu 90 minut, ale niekiedy może to zająć więcej czasu.
    > 
    > Użytkownicy, którzy spróbują użyć nazwy domeny usługi CDN przed zakończeniem propagacji konfiguracji punktu końcowego do lokalizacji POP otrzymają kody odpowiedzi HTTP 404.  Jeśli od czasu utworzenia punktu końcowego minęło kilka godzin, a nadal otrzymujesz odpowiedzi 404, zapoznaj się z artykułem [Rozwiązywanie problemów z punktami końcowymi usługi CDN zwracającymi stany 404](cdn-troubleshoot-endpoint.md).
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



<!--HONumber=Jan17_HO4-->


