---
title: "Konfigurowanie niestandardowej nazwy domeny dla aplikacji sieci web w usłudze Azure App Service, który używa Menedżera ruchu do równoważenia obciążenia."
description: "Użyj nazwy domeny niestandardowej dla aplikacji sieci web w usłudze Azure App Service, która obejmuje usługi Traffic Manager w programie Równoważenie obciążenia."
services: app-service\web
documentationcenter: 
author: cephalin
manager: cfowler
editor: 
ms.assetid: 0f96c0e7-0901-489b-a95a-e3b66ca0a1c2
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2016
ms.author: cephalin
ms.openlocfilehash: 5f099201d9018a6f8577cb3daf127d09560fb94b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="configuring-a-custom-domain-name-for-a-web-app-in-azure-app-service-using-traffic-manager"></a>Konfigurowanie niestandardowej nazwy domeny dla aplikacji sieci web w usłudze Azure App Service przy użyciu Menedżera ruchu
[!INCLUDE [web-selector](../../includes/websites-custom-domain-selector.md)]

[!INCLUDE [intro](../../includes/custom-dns-web-site-intro-traffic-manager.md)]

Ten artykuł zawiera ogólne instrukcje dotyczące korzystania z usługi Azure App Service przy użyciu niestandardowej nazwy domeny, korzystających z usługi Traffic Manager w celu równoważenia obciążenia.

[!INCLUDE [tmwebsitefooter](../../includes/custom-dns-web-site-traffic-manager-notes.md)]

[!INCLUDE [introfooter](../../includes/custom-dns-web-site-intro-notes.md)]

<a name="understanding-records"></a>

## <a name="understanding-dns-records"></a>Opis rekordów DNS
[!INCLUDE [understandingdns](../../includes/custom-dns-web-site-understanding-dns-traffic-manager.md)]

<a name="bkmk_configsharedmode"></a>

## <a name="configure-your-web-apps-for-standard-mode"></a>Konfigurowanie aplikacji sieci web dla Tryb standardowy
[!INCLUDE [modes](../../includes/custom-dns-web-site-modes-traffic-manager.md)]

<a name="bkmk_configurecname"></a>

## <a name="add-a-dns-record-for-your-custom-domain"></a>Dodaj rekord DNS dla domeny niestandardowej
> [!NOTE]
> Jeśli zostały nabyte domeny przy użyciu aplikacji sieci Web usługi aplikacji Azure Pomiń następujące kroki i odwoływać się do ostatniego kroku [kupić domenę dla aplikacji sieci Web](custom-dns-web-site-buydomains-web-app.md) artykułu.
> 
> 

Aby skojarzyć domenę niestandardową z aplikacji sieci web w usłudze Azure App Service, niezbędny nowy wpis w tabeli DNS dla domeny niestandardowej przy użyciu narzędzi dostarczonych przez rejestratora domen, zakupionym z nazwą domeny. Wykonaj następujące kroki, aby zlokalizować i korzystania z narzędzi DNS.

1. Zaloguj się na koncie u rejestratora domen i poszukaj stronę Zarządzanie rekordami DNS. Wyszukaj łącza lub obszarów witryny oznaczone jako **nazwy domeny**, **DNS**, lub **nazwy serwera zarządzania**. Często łącze do tej strony można znaleźć wyświetlanie informacji o koncie, a następnie sprawdzając takich jak łącze **mojej domeny**.
2. Po znalezieniu strony zarządzania dla nazwy domeny, poszukaj łącza, który służy do edytowania rekordów DNS. Może to być wymieniona jako **pliku strefy**, **rekordy DNS**, lub jako **zaawansowane** link konfiguracji.
   
   * Strona najprawdopodobniej będzie mieć kilka rekordów już utworzone, takich jak kojarzenie wpis "**@**"lub"\*" ze stroną "postojowego domeny". Może również zawierać rekordy dotyczące typowych poddomen takich jak **www**.
   * Strona będzie zawierać **rekordy CNAME**, lub Udostępnij listy rozwijanej wybierz typ rekordu. Go może również zawierać inne rekordy takich jak **rekordy** i **rekordów MX**. W niektórych przypadkach rekordy CNAME zostanie wywołana przez inne nazwy, takie jak **rekord aliasu**.
   * Strony będą także mieć pól, które umożliwiają **mapy** z **nazwy hosta** lub **nazwy domeny** na inną nazwę domeny.
3. Gdy specyfice każdy Rejestrator różnią się ogólnie mapowania *z* niestandardową nazwę domeny (takich jak **contoso.com**,) *do* nazwę domeny usługi Traffic Manager (**contoso.trafficmanager.net**) używany dla aplikacji sieci web.
   
   > [!NOTE]
   > Alternatywnie Jeśli rekord jest już w użyciu, i chcesz preemptively powiązać go z aplikacji, można utworzyć dodatkowy rekord CNAME. Na przykład, aby powiązać preemptively **www.contoso.com** do aplikacji sieci web, należy utworzyć rekord CNAME z **awverify.www** do **contoso.trafficmanager.net**. Następnie można dodać "www.contoso.com" do aplikacji sieci Web bez konieczności zmieniania rekord CNAME "www". Aby uzyskać więcej informacji, zobacz [rekordy DNS Utwórz dla aplikacji sieci web w domenie niestandardowych][CREATEDNS].
   > 
   > 
4. Po zakończeniu dodawania lub modyfikowania rekordów DNS u rejestratora, Zapisz zmiany.

<a name="enabledomain"></a>

## <a name="enable-traffic-manager"></a>Menedżer ruchu
[!INCLUDE [modes](../../includes/custom-dns-web-site-enable-on-traffic-manager.md)]

## <a name="next-steps"></a>Następne kroki
Aby uzyskać więcej informacji, odwiedź stronę [Centrum deweloperów środowiska Node.js](/develop/nodejs/).

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[CREATEDNS]: ../dns/dns-web-sites-custom-domain.md
