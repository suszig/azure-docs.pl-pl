---
title: "Ograniczenia adresów IP w usłudze Azure App Service | Dokumentacja firmy Microsoft"
description: "Jak używać ograniczenia adresów IP z usługi aplikacji Azure"
author: btardif
manager: stefsch
editor: 
services: app-service\web
documentationcenter: 
ms.assetid: 3be1f4bd-8a81-4565-8a56-528c037b24bd
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/23/2017
ms.author: byvinyal
ms.openlocfilehash: 5fbd308e9f037038ad867f3d242da6573bc67081
ms.sourcegitcommit: e6029b2994fa5ba82d0ac72b264879c3484e3dd0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/24/2017
---
# <a name="azure-app-service-static-ip-restrictions"></a>Usługa aplikacji Azure ograniczenia statycznego adresu IP #

Ograniczenia adresów IP umożliwiają definiowanie listę adresów IP, które mogą uzyskać dostępu do aplikacji. Na białej liście mogą zawierać pojedyncze adresy IP lub zakres adresów IP określone przez maskę podsieci.

Gdy żądanie do aplikacji jest generowany przez klienta, adres IP jest porównywany na białej liście. Jeśli adres ip nie jest na liście, w odpowiedzi aplikacji [HTTP 403](https://en.wikipedia.org/wiki/HTTP_403) kod stanu.

Ograniczenia adresów IP są definiowane w pliku web.config, który wykorzystuje aplikacji w czasie wykonywania. W pewnych okolicznościach niektóre modułu mogą być wykonywane przed logiki ograniczenia adresu IP w potoku HTTP. W takim przypadku żądanie kończy się niepowodzeniem z inny kod błędu HTTP.

Ograniczenia adresów IP są oceniane na tym samym wystąpieniach planu usługi aplikacji przypisany do aplikacji.

Aby dodać regułę ograniczeń adresu IP do aplikacji, użyj menu można otworzyć **sieci**>**ograniczenia adresów IP** i kliknij przycisk **skonfigurować ograniczenia adresów IP**

! [Ograniczenia adresów IP] (media/app-service-ip-restrictions/ip-restrictions.png)

W tym miejscu można przejrzeć na liście reguł ograniczeń IP zdefiniowane dla aplikacji.

![ograniczenia adresów IP z listy](media/app-service-ip-restrictions/browse-ip-restrictions.png)

Możesz kliknąć **[+] Dodaj** Aby dodać nową regułę ograniczenia adresu IP.

![Dodawanie ograniczenia adresów IP](media/app-service-ip-restrictions/add-ip-restrictions.png)
