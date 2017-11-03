---
title: "Mapowanie istniejących niestandardową nazwę DNS do aplikacji sieci Web platformy Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak dodać istniejącą nazwę domeny DNS niestandardowe (niestandardowych domeny) do aplikacji sieci web, zaplecza aplikacji mobilnej lub aplikacji interfejsu API w usłudze Azure App Service."
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: erikre
editor: 
ms.assetid: dc446e0e-0958-48ea-8d99-441d2b947a7c
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 06/23/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: ac105b82b9bc25e6cc9f5cdf21f34400a09b3934
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="map-an-existing-custom-dns-name-to-azure-web-apps"></a>Mapowanie istniejących niestandardową nazwę DNS do aplikacji sieci Web Azure

Usługa [Azure Web Apps](app-service-web-overview.md) oferuje wysoce skalowalną i samonaprawialną usługę hostowaną w Internecie. W tym samouczku przedstawiono sposób odwzorowywania istniejącej nazwy DNS niestandardowej aplikacji sieci Web Azure.

![Nawigacji w portalu do aplikacji Azure](./media/app-service-web-tutorial-custom-domain/app-with-custom-dns.png)

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Mapowanie poddomeny (na przykład `www.contoso.com`) przy użyciu rekordu CNAME
> * Mapowanie domeny katalogu głównego (na przykład `contoso.com`) przy użyciu rekord a.
> * Zamapować domenę symboli wieloznacznych (na przykład `*.contoso.com`) przy użyciu rekordu CNAME
> * Mapowanie domeny przy użyciu skryptów automatyzacji

Możesz użyć dowolnej **rekord CNAME** lub **rekordu** do mapowania niestandardową nazwę DNS usługi aplikacji. 

> [!NOTE]
> Zalecane jest użycie rekord CNAME dla wszystkich niestandardowych nazw DNS z wyjątkiem domeny katalogu głównego (na przykład `contoso.com`).

Aby przeprowadzić migrację na żywo lokacji i nazwy domeny DNS do usługi App Service, zobacz [migracji active nazwy DNS w usłudze Azure App Service](app-service-custom-domain-name-migrate.md).

## <a name="prerequisites"></a>Wymagania wstępne

W celu ukończenia tego samouczka:

* [Utwórz aplikację usługi aplikacji](/azure/app-service/), lub użyć utworzonego w samouczku innej aplikacji.
* Kup nazwę domeny i upewnij się, że masz dostęp do rejestru DNS dla domeny dostawcy (takie jak GoDaddy).

  Na przykład, aby dodać wpisów DNS dla `contoso.com` i `www.contoso.com`, użytkownik musi mieć możliwość skonfigurowania ustawień DNS dla `contoso.com` domeny głównej.

  > [!NOTE]
  > Jeśli nie masz istniejącej domeny nazwa, należy wziąć pod uwagę [zakupu domeny przy użyciu portalu Azure](custom-dns-web-site-buydomains-web-app.md). 

## <a name="prepare-the-app"></a>Przygotowywanie aplikacji

Aby zamapować niestandardową nazwę DNS w aplikacji sieci web, aplikacji sieci web firmy [planu usługi aplikacji](https://azure.microsoft.com/pricing/details/app-service/) musi być płatną warstwy (**Shared**, **podstawowe**, **standardowe**, lub  **Premium**). W tym kroku należy upewnić się, że aplikacja usługi aplikacji jest w obsługiwanym warstwy cenowej.

### <a name="sign-in-to-azure"></a>Logowanie do platformy Azure

Otwórz [portalu Azure](https://portal.azure.com) i zaloguj się przy użyciu konta platformy Azure.

### <a name="navigate-to-the-app-in-the-azure-portal"></a>Przejdź do aplikacji w portalu Azure

Wybierz z menu po lewej stronie **usługi aplikacji**, a następnie wybierz nazwę aplikacji.

![Nawigacji w portalu do aplikacji Azure](./media/app-service-web-tutorial-custom-domain/select-app.png)

Zostanie wyświetlona strona zarządzania aplikacji usługi aplikacji.  

<a name="checkpricing"></a>

### <a name="check-the-pricing-tier"></a>Sprawdź warstwę cenową

W lewym obszarze nawigacji strony aplikacji, przewiń **ustawienia** a następnie wybierz opcję **skalowanie w górę (plan usługi App Service)**.

![Skalowanie w pionie menu](./media/app-service-web-tutorial-custom-domain/scale-up-menu.png)

Warstwa bieżąca aplikacja zostanie wyróżniona niebieskim obramowaniem. Upewnij się, że aplikacja nie znajduje się w **wolne** warstwy. Niestandardowe DNS nie jest obsługiwany w **wolne** warstwy. 

![Sprawdź warstwę cenową](./media/app-service-web-tutorial-custom-domain/check-pricing-tier.png)

Jeśli plan usługi aplikacji nie jest **wolne**, Zamknij **wybierz warstwę cenową** strony i przejść [mapy rekord CNAME](#cname).

<a name="scaleup"></a>

### <a name="scale-up-the-app-service-plan"></a>Skalowanie w górę plan usługi aplikacji

Wybierz jedno z systemem innym niż bez warstw (**Shared**, **podstawowe**, **standardowe**, lub **Premium**). 

Kliknij pozycję **Wybierz**.

![Sprawdź warstwę cenową](./media/app-service-web-tutorial-custom-domain/choose-pricing-tier.png)

Gdy zostanie wyświetlone następujące powiadomienie, zakończeniu operacji skalowania.

![Potwierdzenie operacji skalowania](./media/app-service-web-tutorial-custom-domain/scale-notification.png)

<a name="cname"></a>

## <a name="map-a-cname-record"></a>Mapa rekord CNAME

W przykładzie samouczek, Dodaj rekord CNAME dla `www` domeny podrzędnej (na przykład `www.contoso.com`).

[!INCLUDE [Access DNS records with domain provider](../../includes/app-service-web-access-dns-records.md)]

### <a name="create-the-cname-record"></a>Utwórz rekord CNAME

Dodaj rekord CNAME do mapowania poddomeny hostname domyślnej aplikacji (`<app_name>.azurewebsites.net`, gdzie `<app_name>` to nazwa aplikacji).

Aby uzyskać `www.contoso.com` przykład domeny, Dodaj rekord CNAME, który mapuje nazwę `www` do `<app_name>.azurewebsites.net`.

Po dodaniu CNAME stronie rekordy DNS wygląda jak w następującym przykładzie:

![Nawigacji w portalu do aplikacji Azure](./media/app-service-web-tutorial-custom-domain/cname-record.png)

### <a name="enable-the-cname-record-mapping-in-azure"></a>Włącz mapowania rekord CNAME w systemie Azure

W lewym obszarze nawigacji strony aplikacji w portalu Azure, wybierz **domen niestandardowych**. 

![Menu domeny niestandardowej](./media/app-service-web-tutorial-custom-domain/custom-domain-menu.png)

W **domen niestandardowych** strony aplikacji, należy dodać w pełni kwalifikowana nazwa DNS niestandardowe (`www.contoso.com`) do listy.

Wybierz  **+**  obok opcji **dodać nazwę hosta**.

![Dodaj nazwy hosta](./media/app-service-web-tutorial-custom-domain/add-host-name-cname.png)

Wpisz w pełni kwalifikowaną nazwę domeny dodaje rekord CNAME, takich jak `www.contoso.com`. 

Wybierz **zweryfikować**.

**Dodać nazwę hosta** przycisk jest aktywny. 

Upewnij się, że **typu rekordu Hostname** ustawiono **CNAME (www.example.com lub wszystkie poddomeny)**.

Wybierz **dodać nazwę hosta**.

![Dodaj nazwę DNS do aplikacji](./media/app-service-web-tutorial-custom-domain/validate-domain-name-cname.png)

Może upłynąć trochę czasu, zanim nowej nazwy hosta zostaną odzwierciedlone w aplikacji **domen niestandardowych** strony. Spróbuj odświeżyć przeglądarkę, aby zaktualizować dane.

![Dodaje rekord CNAME](./media/app-service-web-tutorial-custom-domain/cname-record-added.png)

Możesz pominąć krok lub wkradła się Literówka gdzieś wcześniej, zostanie wyświetlony błąd weryfikacji, w dolnej części strony.

![Błąd weryfikacji](./media/app-service-web-tutorial-custom-domain/verification-error-cname.png)

<a name="a"></a>

## <a name="map-an-a-record"></a>Mapa rekord a.

W przykładzie samouczek dodaniu rekordu A dla domeny katalogu głównego (na przykład `contoso.com`). 

<a name="info"></a>

### <a name="copy-the-apps-ip-address"></a>Skopiuj adres IP aplikacji

Aby zamapować rekord A, należy aplikacji zewnętrzny adres IP. Ten adres IP można znaleźć w aplikacji **domen niestandardowych** strony w portalu Azure.

W lewym obszarze nawigacji strony aplikacji w portalu Azure, wybierz **domen niestandardowych**. 

![Menu domeny niestandardowej](./media/app-service-web-tutorial-custom-domain/custom-domain-menu.png)

W **domen niestandardowych** strony, skopiuj adres IP aplikacji.

![Nawigacji w portalu do aplikacji Azure](./media/app-service-web-tutorial-custom-domain/mapping-information.png)

[!INCLUDE [Access DNS records with domain provider](../../includes/app-service-web-access-dns-records.md)]

### <a name="create-the-a-record"></a>Utwórz rekord a.

Aby mapować rekordu A dla aplikacji, usługa aplikacji wymaga **dwóch** rekordy DNS:

- **A** rekordu mapowane na adres IP aplikacji.
- A **TXT** rekordu do mapowania nazwy hosta domyślnego aplikacji `<app_name>.azurewebsites.net`. Usługa aplikacji używa tego rekordu tylko podczas konfiguracji, aby sprawdzić, czy jesteś właścicielem domeny niestandardowej. Po domeny niestandardowej jest weryfikowane i skonfigurowaniu w usłudze App Service, można usunąć tego rekordu TXT. 

Dla `contoso.com` przykład domeny utworzyć rekordy A i TXT, zgodnie z poniższą tabelą (`@` zazwyczaj reprezentuje domeny głównej). 

| Typ rekordu | Host | Wartość |
| - | - | - |
| A | `@` | Adres IP z [skopiuj adres IP aplikacji](#info) |
| TXT | `@` | `<app_name>.azurewebsites.net` |

Rekordy są dodawane, stronie rekordy DNS wygląda następująco:

![Strona rekordów DNS](./media/app-service-web-tutorial-custom-domain/a-record.png)

<a name="enable-a"></a>

### <a name="enable-the-a-record-mapping-in-the-app"></a>Włącz mapowanie rekord A w aplikacji

W aplikacji **domen niestandardowych** w portalu Azure, należy dodać w pełni kwalifikowana nazwa DNS niestandardowe (na przykład `contoso.com`) do listy.

Wybierz  **+**  obok opcji **dodać nazwę hosta**.

![Dodaj nazwy hosta](./media/app-service-web-tutorial-custom-domain/add-host-name.png)

Wpisz w pełni kwalifikowaną nazwę domeny skonfigurować rekord A, takich jak `contoso.com`.

Wybierz **zweryfikować**.

**Dodać nazwę hosta** przycisk jest aktywny. 

Upewnij się, że **typu rekordu Hostname** ustawiono **rekordu (example.com)**.

Wybierz **dodać nazwę hosta**.

![Dodaj nazwę DNS do aplikacji](./media/app-service-web-tutorial-custom-domain/validate-domain-name.png)

Może upłynąć trochę czasu, zanim nowej nazwy hosta zostaną odzwierciedlone w aplikacji **domen niestandardowych** strony. Spróbuj odświeżyć przeglądarkę, aby zaktualizować dane.

![Dodaje rekord](./media/app-service-web-tutorial-custom-domain/a-record-added.png)

Możesz pominąć krok lub wkradła się Literówka gdzieś wcześniej, zostanie wyświetlony błąd weryfikacji, w dolnej części strony.

![Błąd weryfikacji](./media/app-service-web-tutorial-custom-domain/verification-error.png)

<a name="wildcard"></a>

## <a name="map-a-wildcard-domain"></a>Zamapować domenę symboli wieloznacznych

W tym przykładzie samouczek mapy [nazwa DNS z symbolem wieloznacznym](https://en.wikipedia.org/wiki/Wildcard_DNS_record) (na przykład `*.contoso.com`) do aplikacji usługi app Service przez dodanie rekordu CNAME. 

[!INCLUDE [Access DNS records with domain provider](../../includes/app-service-web-access-dns-records.md)]

### <a name="create-the-cname-record"></a>Utwórz rekord CNAME

Dodaj rekord CNAME do mapowania nazwy symbolu wieloznacznego hostname domyślnej aplikacji (`<app_name>.azurewebsites.net`).

Dla `*.contoso.com` przykład domeny rekord CNAME przypisze nazwę `*` do `<app_name>.azurewebsites.net`.

Po dodaniu CNAME stronie rekordy DNS wygląda jak w następującym przykładzie:

![Nawigacji w portalu do aplikacji Azure](./media/app-service-web-tutorial-custom-domain/cname-record-wildcard.png)

### <a name="enable-the-cname-record-mapping-in-the-app"></a>Włącz mapowanie rekord CNAME w aplikacji

Można teraz dodawać żadnych poddomeny zgodnej z nazwą symbolu wieloznacznego w aplikacji (na przykład `sub1.contoso.com` i `sub2.contoso.com` odpowiada `*.contoso.com`). 

W lewym obszarze nawigacji strony aplikacji w portalu Azure, wybierz **domen niestandardowych**. 

![Menu domeny niestandardowej](./media/app-service-web-tutorial-custom-domain/custom-domain-menu.png)

Wybierz  **+**  obok opcji **dodać nazwę hosta**.

![Dodaj nazwy hosta](./media/app-service-web-tutorial-custom-domain/add-host-name-cname.png)

Wpisz nazwę FQDN, która pasuje do domeny symbolu wieloznacznego (na przykład `sub1.contoso.com`), a następnie wybierz **weryfikacji**.

**Dodać nazwę hosta** przycisk jest aktywny. 

Upewnij się, że **typu rekordu Hostname** ustawiono **rekord CNAME (www.example.com lub wszystkie poddomeny)**.

Wybierz **dodać nazwę hosta**.

![Dodaj nazwę DNS do aplikacji](./media/app-service-web-tutorial-custom-domain/validate-domain-name-cname-wildcard.png)

Może upłynąć trochę czasu, zanim nowej nazwy hosta zostaną odzwierciedlone w aplikacji **domen niestandardowych** strony. Spróbuj odświeżyć przeglądarkę, aby zaktualizować dane.

Wybierz  **+**  ikonę ponownie, aby dodać inną nazwę hosta odpowiadający domenie symboli wieloznacznych. Na przykład dodać `sub2.contoso.com`.

![Dodaje rekord CNAME](./media/app-service-web-tutorial-custom-domain/cname-record-added-wildcard2.png)

## <a name="test-in-browser"></a>Testowanie w przeglądarce

Przejdź do nazwy DNS, który został wcześniej skonfigurowany (na przykład `contoso.com`, `www.contoso.com`, `sub1.contoso.com`, i `sub2.contoso.com`).

![Nawigacji w portalu do aplikacji Azure](./media/app-service-web-tutorial-custom-domain/app-with-custom-dns.png)

## <a name="automate-with-scripts"></a>Zautomatyzować za pomocą skryptów

Można zautomatyzować zarządzanie domenami niestandardowymi za pomocą skryptów, za pomocą [interfejsu wiersza polecenia Azure](/cli/azure/install-azure-cli) lub [programu Azure PowerShell](/powershell/azure/overview). 

### <a name="azure-cli"></a>Interfejs wiersza polecenia platformy Azure 

Polecenie dodaje skonfigurowanej niestandardowe nazwy DNS do aplikacji usługi app Service. 

```bash 
az appservice web config hostname add \
    --webapp <app_name> \
    --resource-group <resource_group_name> \ 
    --name <fully_qualified_domain_name> 
``` 

Aby uzyskać więcej informacji, zobacz [zamapować niestandardową domenę do aplikacji sieci web](scripts/app-service-cli-configure-custom-domain.md). 

### <a name="azure-powershell"></a>Azure PowerShell 

Polecenie dodaje skonfigurowanej niestandardowe nazwy DNS do aplikacji usługi app Service. 

```PowerShell  
Set-AzureRmWebApp `
    -Name <app_name> `
    -ResourceGroupName <resource_group_name> ` 
    -HostNames @("<fully_qualified_domain_name>","<app_name>.azurewebsites.net") 
```

Aby uzyskać więcej informacji, zobacz [przypisać niestandardową domenę do aplikacji sieci web](scripts/app-service-powershell-configure-custom-domain.md).

## <a name="next-steps"></a>Następne kroki

W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Mapa poddomeny przy użyciu rekordu CNAME
> * Mapa domeny głównej, przy użyciu rekord a.
> * Zamapować domenę symboli wieloznacznych przy użyciu rekordu CNAME
> * Mapowanie domeny przy użyciu skryptów automatyzacji

Przejdź do następnego samouczek informacje na temat wiązania niestandardowego certyfikatu SSL w aplikacji sieci web.

> [!div class="nextstepaction"]
> [Powiąż istniejący certyfikat SSL niestandardowych do aplikacji sieci Web Azure](app-service-web-tutorial-custom-ssl.md)
