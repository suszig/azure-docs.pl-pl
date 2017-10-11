---
title: "Kontrolowanie aplikacji sieci Web platformy Azure przy użyciu usługi Azure Traffic Manager"
description: "Ten artykuł zawiera podsumowanie informacji dla usługi Azure Traffic Manager, ponieważ odnosi się do aplikacji sieci web platformy Azure."
services: app-service\web
documentationcenter: 
author: cephalin
writer: cephalin
manager: erikre
editor: mollybos
ms.assetid: dabda633-e72f-4dd4-bf1c-6e945da456fd
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/25/2016
ms.author: cephalin
ms.openlocfilehash: fb7d391e3118a9dccde5501c3f30c6f580932a30
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/03/2017
---
# <a name="controlling-azure-web-app-traffic-with-azure-traffic-manager"></a>Kontrolowanie aplikacji sieci Web platformy Azure przy użyciu usługi Azure Traffic Manager
> [!NOTE]
> Ten artykuł zawiera informacje podsumowania dla Menedżera ruchu Microsoft Azure w odniesieniu do aplikacji sieci Web usługi aplikacji Azure. Więcej informacji na temat usługi Azure Traffic Manager sam można znaleźć, przechodząc na stronę łącza na końcu tego artykułu.
> 
> 

## <a name="introduction"></a>Wprowadzenie
Usługi Azure Traffic Manager można użyć do kontrolowania sposobu żądania od klientów sieci web są dystrybuowane do aplikacji web apps w usłudze Azure App Service. Woluminowi punktów końcowych aplikacji sieci web do profilu Menedżera ruchu Azure usługi Azure Traffic Manager przechowuje informacje o stanu aplikacji sieci web (uruchomionej, zatrzymanej lub usunięte), dzięki czemu można zdecydować, które z tych punktów końcowych powinny odbierać dane.

## <a name="load-balancing-methods"></a>Metody równoważenia obciążenia
Trzy metody równoważenia obciążenia różnych korzysta z usługi Azure Traffic Manager. Te są opisane na poniższej liście odnoszą się do aplikacji sieci web platformy Azure.

* **Tryb failover**: Jeśli klony aplikacji sieci web znajdują się w różnych regionach, możesz ta metoda umożliwia skonfigurowanie jednej aplikacji sieci web do obsługi całego ruchu klientów sieci web i skonfiguruj inną aplikację sieci web w innym regionie do obsłużenia tego ruchu w przypadku pierwszej aplikacji sieci web staje się niedostępny.
* **Działanie okrężne**: Jeśli masz klony aplikacji sieci web w różnych regionach, umożliwia tej metody Dystrybuuj ruch równomiernie na aplikacje sieci web w różnych regionach.
* **Wydajność**: metoda wydajności dystrybuuje ruch na podstawie najkrótszego czasu obiegu do klientów. Metody wydajności można użyć dla aplikacji sieci web, w tym samym regionie lub w różnych regionach.

## <a name="web-apps-and-traffic-manager-profiles"></a>Aplikacje sieci Web i profilów usługi Traffic Manager
Aby skonfigurować kontrolę ruchu w sieci web aplikacji, Utwórz profil w usłudze Azure Traffic Manager czy używa jednego z trzech obciążenia równoważenia metod opisanych wcześniej, a następnie dodaj punkty końcowe (w tym przypadku aplikacji sieci web) dla których chcesz kontrolować ruch do profilu. Stan aplikacji sieci web (uruchomiona, zatrzymana lub usunięte) jest regularnie przekazywane do profilu, aby usługi Azure Traffic Manager można kierować ruch odpowiednio.

Podczas przy użyciu usługi Azure Traffic Manager przy użyciu platformy Azure, należy uwzględnić następujące kwestie:

* Wdrożeń sieci web aplikacji tylko w obrębie tego samego regionu aplikacje sieci Web zawiera już trybu failover i funkcja okrężnego niezależnie tryb aplikacji sieci web.
* W przypadku wdrożeń w tym samym regionie, korzystających z aplikacji sieci Web w połączeniu z innej usługi w chmurze platformy Azure można połączyć oba typy punktów końcowych na potrzeby scenariuszy hybrydowych.
* Można określić tylko jeden punkt końcowy aplikacji sieci web dla regionu w profilu. Po wybraniu aplikacji sieci web jako punktu końcowego dla regionu co pozostałe aplikacje sieci web, w tym regionie stają się niedostępne do wybrania dla tego profilu.
* Punkty końcowe aplikacji sieci web, które można określić w profilu Menedżera ruchu Azure będą wyświetlane w obszarze **nazwy domen** sekcja na stronie konfiguracji dla aplikacji sieci web w profilu, ale nie będzie można skonfigurować istnieje.
* Po dodaniu aplikacji sieci web do profilu, **adres URL witryny** na pulpicie nawigacyjnym w sieci Web portalu stronę instalacji aplikacji wyświetli adresu URL domeny niestandardowej aplikacji sieci web, jeśli zdefiniowano jeden. W przeciwnym razie będzie wyświetlany adres URL profilu Menedżera ruchu (na przykład `contoso.trafficmgr.com`). Nazwa domeny bezpośrednie aplikacji sieci web i adres URL Menedżera ruchu będą widoczne na stronie Konfigurowanie aplikacji sieci web w obszarze **nazwy domen** sekcji.
* Niestandardowa nazwa domeny będą działać zgodnie z oczekiwaniami, ale oprócz dodanie ich do aplikacji sieci web, musisz również skonfigurować mapy DNS, aby wskazywał adres URL Menedżera ruchu. Aby uzyskać informacje na temat sposobu konfigurowania domeny niestandardowej dla aplikacji sieci web platformy Azure, zobacz [Konfigurowanie niestandardowej nazwy domeny dla usługi Azure witryny sieci web](app-service-web-tutorial-custom-domain.md).
* Można dodawać tylko aplikacje sieci web, które są w trybie standard lub premium, aby profil Menedżera ruchu Azure.

## <a name="next-steps"></a>Następne kroki
Koncepcyjna i techniczne omówienie usługi Azure Traffic Manager, zobacz [Omówienie Menedżera ruchu](../traffic-manager/traffic-manager-overview.md).

Aby uzyskać więcej informacji dotyczących korzystania z Menedżera ruchu z aplikacjami sieci Web, zobacz wpisy blogu [przy użyciu usługi Azure Traffic Manager z witryn sieci Web Azure](http://blogs.msdn.com/b/waws/archive/2014/03/18/using-windows-azure-traffic-manager-with-waws.aspx) i [usługi Azure Traffic Manager teraz można zintegrować z witryn sieci Web Azure](https://azure.microsoft.com/blog/2014/03/27/azure-traffic-manager-can-now-integrate-with-azure-web-sites/).

