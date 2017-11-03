---
title: Kup niestandardowej nazwy domeny dla aplikacji sieci Web Azure
description: "Dowiedz się, jak kupić niestandardowej nazwy domeny z aplikacji sieci web w usłudze Azure App Service."
services: app-service\web
documentationcenter: 
author: cephalin
manager: cfowler
editor: 
ms.assetid: 70fb0e6e-8727-4cca-ba82-98a4d21586ff
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2016
ms.author: cephalin
ms.openlocfilehash: 3cb22b935624041ab51e64028a1b668fd694f9b5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="buy-a-custom-domain-name-for-azure-web-apps"></a>Kup niestandardowej nazwy domeny dla aplikacji sieci Web Azure

Domeny aplikacji usługi (wersja zapoznawcza) są domeny najwyższego poziomu, które są zarządzane bezpośrednio na platformie Azure. One ułatwiają zarządzanie domenami niestandardowymi dla [Azure Web Apps](app-service-web-overview.md). Ten samouczek pokazuje, jak kupić domenę usługi aplikacji i przypisywać nazwy DNS do aplikacji sieci Web Azure.

Ten artykuł dotyczy usługi Azure App Service (aplikacje sieci Web, aplikacje API Apps, Mobile Apps, Logic Apps). Dla maszyny Wirtualnej platformy Azure lub usługi Azure Storage, zobacz [domeny przypisać usługi aplikacji na maszynie Wirtualnej platformy Azure lub usługi Azure Storage](https://blogs.msdn.microsoft.com/appserviceteam/2017/07/31/assign-app-service-domain-to-azure-vm-or-azure-storage/). Dla usług w chmurze, zobacz [Konfigurowanie niestandardowej nazwy domeny dla usługi w chmurze Azure](../cloud-services/cloud-services-custom-domain-name-portal.md).

## <a name="prerequisites"></a>Wymagania wstępne

W celu ukończenia tego samouczka:

* [Utwórz aplikację usługi aplikacji](/azure/app-service/), lub użyć utworzonego w samouczku innej aplikacji.

## <a name="prepare-the-app"></a>Przygotowywanie aplikacji

Do używania niestandardowych domen w usłudze Azure Web Apps, aplikacji sieci web w [planu usługi aplikacji](https://azure.microsoft.com/pricing/details/app-service/) musi być płatną warstwy (**Shared**, **podstawowe**, **standardowe**, lub  **Premium**). W tym kroku należy upewnić się, że aplikacja sieci web jest w obsługiwanym warstwy cenowej.

### <a name="sign-in-to-azure"></a>Logowanie do platformy Azure

Otwórz [portalu Azure](https://portal.azure.com) i zaloguj się przy użyciu konta platformy Azure.

### <a name="navigate-to-the-app-in-the-azure-portal"></a>Przejdź do aplikacji w portalu Azure

Wybierz z menu po lewej stronie **usługi aplikacji**, a następnie wybierz nazwę aplikacji.

![Nawigacji w portalu do aplikacji Azure](./media/app-service-web-tutorial-custom-domain/select-app.png)

Zostanie wyświetlona strona zarządzania aplikacji usługi aplikacji.  

### <a name="check-the-pricing-tier"></a>Sprawdź warstwę cenową

W lewym obszarze nawigacji strony aplikacji, przewiń **ustawienia** a następnie wybierz opcję **skalowanie w górę (plan usługi App Service)**.

![Skalowanie w pionie menu](./media/app-service-web-tutorial-custom-domain/scale-up-menu.png)

Warstwa bieżąca aplikacja zostanie wyróżniona niebieskim obramowaniem. Upewnij się, że aplikacja nie znajduje się w **wolne** warstwy. Niestandardowe DNS nie jest obsługiwany w **wolne** warstwy. 

![Sprawdź warstwę cenową](./media/app-service-web-tutorial-custom-domain/check-pricing-tier.png)

Jeśli plan usługi aplikacji nie jest **wolne**, Zamknij **wybierz warstwę cenową** strony i przejść [kupić domeny](#buy-the-domain).

### <a name="scale-up-the-app-service-plan"></a>Skalowanie w górę plan usługi aplikacji

Wybierz jedno z systemem innym niż bez warstw (**Shared**, **podstawowe**, **standardowe**, lub **Premium**). 

Kliknij pozycję **Wybierz**.

![Sprawdź warstwę cenową](./media/app-service-web-tutorial-custom-domain/choose-pricing-tier.png)

Gdy zostanie wyświetlone następujące powiadomienie, zakończeniu operacji skalowania.

![Potwierdzenie operacji skalowania](./media/app-service-web-tutorial-custom-domain/scale-notification.png)

## <a name="buy-the-domain"></a>Kup domeny

### <a name="sign-in-to-azure"></a>Logowanie do platformy Azure
Otwórz [portalu Azure](https://portal.azure.com/) i zaloguj się przy użyciu konta platformy Azure.

### <a name="launch-buy-domains"></a>Uruchamianie Kup domen
W **aplikacje sieci Web** kliknij nazwę aplikacji sieci web, wybierz pozycję **ustawienia**, a następnie wybierz **domen niestandardowych**
   
![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-6.png)

W **domen niestandardowych** kliknij przycisk **kupić domen**.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-1.png)

### <a name="configure-the-domain-purchase"></a>Konfigurowanie domeny zakupu

W **aplikacji usługi domeny** strony w **wyszukiwania dla domeny** wpisz nazwę domeny, aby kupić i wpisz `Enter`. Sugerowane dostępnych domen są wyświetlane poniżej pola tekstowego. Wybierz co najmniej jedną domenę, który chcesz kupić. 
   
![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-2.png)

Kliknij przycisk **informacje kontaktowe** i wypełnij formularz informacji kontaktowych w domenie. Gdy skończysz, kliknij przycisk **OK** aby powrócić do strony domena usługi aplikacji.
   
Należy wypełnić wszystkie wymagane pola z dokładnością tak jak to możliwe. Nieprawidłowe dane, aby uzyskać więcej informacji może spowodować awarię zakupu domen. 

Następnie wybierz odpowiednie opcje dla danej domeny. Zobacz poniższą tabelę objaśnienia:

| Ustawienie | Sugerowana wartość | Opis |
|-|-|-|
|Automatycznego odnawiania | **Włączanie** | Odnawia domenę usługi App automatycznie co roku. Karta kredytowa jest obciążona w tej samej cenie zakupu w momencie odnawiania. |
|Ochrona prywatności | Włączanie | Zgódź się na "Ochrona prywatności", który jest dostępny w zapłaconej kwoty _bezpłatnie_ (z wyjątkiem domen najwyższego poziomu, którego rejestr nie obsługują ochrony prywatności, takich jak _. co.in_, _. co.uk_ i tak dalej). |
| Przypisz domyślne nazwy hostów | **www** i**@** | Wybierz powiązania z żądaną nazwą hosta, w razie potrzeby. Po zakończeniu operacji zakupu domeny aplikacji sieci web są dostępne w wybranej nazwy hostów. Jeśli aplikacja sieci web jest za [usługi Azure Traffic Manager](https://azure.microsoft.com/services/traffic-manager/), nie widzisz opcję, aby przypisać domeny głównej (@), ponieważ Menedżera ruchu jest nie rekordów A pomocy technicznej. Można zmienić przypisania nazwy hosta, po zakończeniu zakupu domeny. |

### <a name="accept-terms-and-purchase"></a>Zaakceptuj postanowienia i zakupu

Kliknij przycisk **postanowienia prawne** Przejrzyj postanowienia i opłat, a następnie kliknij pozycję **kupić**.

> [!NOTE]
> Domeny aplikacji usługi używać usługi Azure DNS domeny. Oprócz opłaty rejestracja domeny użycia dla usługi Azure DNS opłaty. Aby uzyskać informacje, zobacz [cennik usługi Azure DNS](https://azure.microsoft.com/pricing/details/dns/).
>
>

W **aplikacji usługi domeny** kliknij przycisk **OK**. Gdy trwa operacja, zostaną wyświetlone następujące powiadomienia:

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-validate.png)

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-purchase-success.png)

### <a name="test-the-hostnames"></a>Testowanie nazwy hostów

Jeśli domyślne nazwy hostów zostały przypisane do aplikacji sieci web, możesz również wyświetlone powiadomienie Powodzenie dla każdej wybranej nazwy hosta. 

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-bind-success.png)

Zobacz też wybranej nazwy hostów w **domen niestandardowych** strony w **Hostnames** sekcji. 

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-hostnames-added.png)

Aby przetestować nazwy hostów, przejdź do listy nazwy hostów w przeglądarce. W przykładzie na poprzednim zrzucie ekranu, spróbuj przejść do obszaru _kontoso.net_ i _www.kontoso.net_.

## <a name="assign-hostnames-to-web-app"></a>Przypisanie nazwy hostów do aplikacji sieci web

Jeśli nie chcesz przypisać przynajmniej jednej domyślnej nazwy hostów do aplikacji sieci web podczas procesu zakupu lub należy przypisać nazwę hosta, nie na liście, w każdej chwili można przypisać nazwy hosta w.

Nazwy hostów w domenie usługi aplikacji można również przypisywać do innych aplikacji sieci web. Kroki zależą od tego, czy domena aplikacji usługi i aplikacji sieci web należą do tej samej subskrypcji.

- Inną subskrypcję: Mapowanie niestandardowych rekordów DNS z domeny aplikacji usługi do aplikacji sieci web, takich jak zewnętrznie zakupionych domeny. Aby uzyskać informacje dotyczące dodawania niestandardowych nazw DNS w domenie usługi aplikacji, zobacz [Zarządzanie niestandardowych rekordów DNS](#custom). Aby zamapować domeny zewnętrznej zakupionych w aplikacji sieci web, zobacz [mapowania istniejącej nazwy DNS niestandardowej aplikacji sieci Web Azure](app-service-web-tutorial-custom-domain.md). 
- Tej samej subskrypcji: wykonaj następujące kroki.

### <a name="launch-add-hostname"></a>Uruchom dodać nazwy hosta
W **usługi aplikacji** wybierz nazwę aplikacji sieci web, który chcesz przypisać nazwy hostów, aby wybrać **ustawienia**, a następnie wybierz **domen niestandardowych**.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-6.png)

Upewnij się, że zakupione domeny jest wymieniony w **domenami usługi aplikacji** sekcji, ale nie wybierz go. 

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-select-domain.png)

> [!NOTE]
> Wszystkich domen usługi aplikacji w tej samej subskrypcji są wyświetlane w aplikacji sieci web **domen niestandardowych** strony. Jeśli Twoja domena to w ramach subskrypcji aplikacji sieci web, ale nie można go wyświetlić w aplikacji sieci web **domen niestandardowych** strony, spróbuj otworzyć **domen niestandardowych** strony lub odświeżania strony sieci Web. Sprawdź również, dzwonka powiadomień w górnej części portalu Azure niepowodzeń postępu lub utworzenia.
>
>

Wybierz **dodać nazwę hosta**.

### <a name="configure-hostname"></a>Konfigurowanie nazwy hosta
W **dodać nazwę hosta** okna dialogowego, wpisz nazwę FQDN domeny usługi aplikacji lub dowolnej domeny podrzędnej. Na przykład:

- kontoso.NET
- www.kontoso.NET
- ABC.kontoso.NET

Po zakończeniu wybierz **weryfikacji**. Typ rekordu nazwa hosta jest automatycznie zaznacza za użytkownika.

Wybierz **dodać nazwę hosta**.

Po zakończeniu operacji zostanie wyświetlone powiadomienie Powodzenie dla przypisanej nazwy hosta.  

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-bind-success.png)

### <a name="close-add-hostname"></a>Zamknij dodać nazwy hosta
W **dodać nazwę hosta** Przypisz inne nazwy hosta do aplikacji sieci web, zgodnie z potrzebami. Po zakończeniu zamknij **dodać nazwę hosta** strony.

Nowo przypisanej hostname(s) powinien zostać wyświetlony w Twojej aplikacji **domen niestandardowych** strony.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-hostnames-added2.png)

### <a name="test-the-hostnames"></a>Testowanie nazwy hostów

Przejdź do listy nazwy hostów w przeglądarce. W przykładzie na poprzednim zrzucie ekranu, spróbuj przejść do obszaru _abc.kontoso.net_.

<a name="custom" />

## <a name="manage-custom-dns-records"></a>Zarządzanie niestandardowych rekordów DNS

Na platformie Azure rekordy DNS dla domeny usługi aplikacji są zarządzane przy użyciu [usługi Azure DNS](https://azure.microsoft.com/services/dns/). Można dodać, usunąć i Aktualizuj rekordy DNS, podobnie jak zewnętrznie zakupionych domeny.

### <a name="open-app-service-domain"></a>Domena Otwórz usługi aplikacji

W portalu Azure, z menu po lewej stronie wybierz **więcej usług** > **domenami usługi aplikacji**.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-access.png)

Wybierz domenę do zarządzania. 

### <a name="access-dns-zone"></a>Dostęp do strefy DNS

W menu po lewej stronie domeny wybierz **strefy DNS**.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-dns-zone.png)

Ta akcja powoduje otwarcie [strefy DNS](../dns/dns-zones-records.md) strony domenę usługi aplikacji w usłudze Azure DNS. Aby uzyskać informacje na temat edytowania rekordów DNS, zobacz [jak zarządzać stref DNS w portalu Azure](../dns/dns-operations-dnszones-portal.md).

## <a name="cancel-purchase-delete-domain"></a>Anuluj zakupu (usuwania domeny)

Po zakupie domena usługi aplikacji masz pięć dni, aby anulować zakup dla pełny zwrot kosztów. Po pięć dni można usunąć domeny usługi aplikacji, ale nie można otrzymać zwrot kosztów.

### <a name="open-app-service-domain"></a>Domena Otwórz usługi aplikacji

W portalu Azure, z menu po lewej stronie wybierz **więcej usług** > **domenami usługi aplikacji**.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-access.png)

Wybierz domenę do Ciebie chcesz anulować lub usunąć. 

### <a name="delete-hostname-bindings"></a>Usuń powiązań z nazwami hostów

W menu po lewej stronie domeny wybierz **powiązań z nazwami hostów**. Poniżej przedstawiono powiązań z nazwami hostów ze wszystkich usług platformy Azure.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-hostname-bindings.png)

Nie można usunąć domeny aplikacji usługi, dopóki nie zostaną usunięte wszystkie powiązań z nazwami hostów.

Usuń powiązanie każdej nazwy hosta, wybierając **...**   >  **Usunąć**. Po usunięciu wszystkich powiązań wybierz **zapisać**.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-delete-hostname-bindings.png)

### <a name="cancel-or-delete"></a>Anuluj lub delete

W menu po lewej stronie domeny wybierz **omówienie**. 

Jeśli nie upłynął okres anulowania zakupionych domeny, wybierz **anulować zakup**. W przeciwnym razie zobacz **usunąć** zamiast tego przycisku. Aby usunąć domeny bez zwrotu, wybierz **usunąć**.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-cancel.png)

Wybierz **OK** o potwierdzenie operacji. Jeśli nie chcesz kontynuować, kliknij w dowolnym miejscu poza okno dialogowe potwierdzenia.

Po zakończeniu operacji domena jest zwolnione z subskrypcji i wszyscy kupić ponownie. 
