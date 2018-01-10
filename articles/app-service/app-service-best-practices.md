---
title: "Najlepsze rozwiązania dotyczące usługi Azure App Service"
description: "Dowiedz się, najlepsze rozwiązania i rozwiązywanie problemów dotyczących usługi Azure App Service."
services: app-service
documentationcenter: 
author: dariagrigoriu
manager: erikre
editor: mollybos
ms.assetid: f3359464-fa44-4f4a-9ea6-7821060e8d0d
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2016
ms.author: dariagrigoriu
ms.openlocfilehash: 1a36c11fcce33c0148fa7d0a4e947a9cc37cd276
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/09/2018
---
# <a name="best-practices-for-azure-app-service"></a>Najlepsze rozwiązania dotyczące usługi Azure App Service
W tym artykule przedstawiono najlepsze rozwiązania dotyczące używania [usłudze Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714). 

## <a name="colocation"></a>Wspólnej lokalizacji
Podczas tworzenia rozwiązania, takie jak aplikacji sieci web i bazy danych zasobów platformy Azure znajdują się w różnych regionach skutki mogą być następujące:

* Większe opóźnienia w łączności między zasobami
* Opłaty finansowe dla danych wychodzących transferu między regionu wspomnianego w [Azure cennikiem](https://azure.microsoft.com/pricing/details/data-transfers).

Wspólnej lokalizacji, w tym samym regionie jest najlepsze w przypadku tworzenia rozwiązania, takie jak aplikacji sieci web i bazy danych lub magazyn konto używane do przechowywania danych zasobów platformy Azure. Podczas tworzenia zasobów, które należy upewnić się, znajdują się w tym samym regionie Azure chyba, że masz firmy lub projektowania Przyczyna nie można je. Aplikacji usługi app Service można przenieść do tego samego regionu co baza danych dzięki wykorzystaniu [usługi aplikacji — funkcja klonowania](app-service-web-app-cloning.md) obecnie dostępne dla aplikacji Plan usługi aplikacji — warstwa Premium.   

## <a name="memoryresources"></a>Gdy aplikacje korzystać z większej ilości pamięci, niż oczekiwano
Gdy zauważysz aplikacji wymaga więcej pamięci niż oczekiwano wskazywany przez monitorowanie lub usługi zalecenia należy wziąć pod uwagę [aplikacji usługi automatyczne naprawianie funkcji](https://azure.microsoft.com/blog/auto-healing-windows-azure-web-sites). Jedną z opcji dla funkcji Automatyczne naprawianie trwa akcje niestandardowe w oparciu próg pamięci. Akcje obejmują spektrum z powiadomienia e-mail do badania za pośrednictwem zrzutu pamięci, aby ograniczenie na miejscu przez odtwarzania procesu roboczego. Automatyczne naprawianie można skonfigurować za pomocą pliku web.config i za pośrednictwem interfejsu użytkownika przyjazną zgodnie z opisem w w tym blogu dla [aplikacji usługi pomocy technicznej witryny rozszerzenia](https://azure.microsoft.com/blog/additional-updates-to-support-site-extension-for-azure-app-service-web-apps).   

## <a name="CPUresources"></a>Gdy aplikacje korzystać więcej procesorów niż oczekiwano
Jeśli widać, że aplikacja wykorzystuje więcej procesorów niż oczekiwano lub napotyka powtarzany nagłego Procesora wskazywany przez monitorowanie lub usługi zalecenia należy wziąć pod uwagę skalowanie w i skalowania plan usługi aplikacji. Jeśli aplikacja jest stanowa, skalowanie w jest jedyną opcją, natomiast w przypadku aplikacji bezstanowych, skalowania w poziomie zapewni większą elastyczność i wyższe możliwości skalowania. 

Aby uzyskać więcej informacji o aplikacjach "bezstanowych", "stanowe" vs można Obejrzyj ten film: [planowanie skalowalności End-to-End wielowarstwowej aplikacji w aplikacji sieci Web platformy Microsoft Azure](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/DEV-B414#fbid=?hashlink=fbid). Aby uzyskać więcej informacji na temat opcji skalowania i skalowanie automatyczne usługi aplikacji przeczytaj: [skalowanie aplikacji sieci Web w usłudze Azure App Service](web-sites-scale.md).  

## <a name="socketresources"></a>Gdy wyczerpania zasobów gniazda
Typową przyczyną wyczerpuje wychodzące połączenia TCP jest używana bibliotek klienta, które nie są zaimplementowane do ponownego użycia połączenia TCP lub w przypadku poziomu protokół wyższej takich jak HTTP - Keep-Alive nie wykorzystywane. Przejrzyj dokumentację dla poszczególnych bibliotek odwołuje się do aplikacji w Twojego planu usługi App Service zapewnienie są skonfigurowane lub dostępne w kodzie wydajne ponowne połączeń wychodzących. Wykonaj również biblioteki dokumentacji wskazówki dotyczące utworzenia prawidłowego i wersji lub czyszczenia, aby uniknąć przeciek połączenia. W czasie dochodzenia takie biblioteki klienta wpływ postępu mogą skorygowane przez skalowanie w poziomie do wielu wystąpień.

### <a name="nodejs-and-outgoing-http-requests"></a>Node.js i wychodzących żądań http
Podczas pracy z wielu wychodzących żądań http i Node.js, zajmowanie HTTP - Keep-Alive jest szczególnie istotna. Można użyć [agentkeepalive](https://www.npmjs.com/package/agentkeepalive) `npm` pakietu w celu ułatwienia w kodzie.

Należy zawsze powinna obsługiwać `http` odpowiedzi, nawet jeśli nie wykonasz żadnej obsługi czynności. Jeśli odpowiedzi nie obsługują poprawnie, aplikacji zostanie ostatecznie zatrzymywane nie więcej gniazda nie są dostępne.

Na przykład podczas pracy z `http` lub `https` pakietu:

```
var request = https.request(options, function(response) {
    response.on('data', function() { /* do nothing */ });
});
```

Jeśli używasz w usłudze aplikacji w systemie Linux na maszynie z wieloma rdzeniami, inny najlepszym rozwiązaniem jest Użyj PM2, aby uruchomić wiele procesów Node.js do wykonywania aplikacji. Można to zrobić, określając polecenie uruchomienia z kontenera.

Na przykład, aby uruchomić czterech wystąpień:

`pm2 start /home/site/wwwroot/app.js --no-daemon -i 4`

## <a name="appbackup"></a>Gdy aplikacja kopii zapasowej uruchamia się niepowodzeniem
Dwie najbardziej typowe przyczyny niepowodzenia tworzenia kopii zapasowej aplikacji są: nieprawidłowy miejsca do magazynowania i nieprawidłowa konfiguracja bazy danych. Takie błędy zazwyczaj się zdarzyć, gdy istnieją zmiany do magazynu lub bazy danych zasobów lub zmiany dotyczące dostępu do tych zasobów (np. poświadczenia zaktualizowane dla wybranej w ustawieniach tworzenia kopii zapasowej bazy danych). Tworzenie kopii zapasowych zwykle uruchamiane zgodnie z harmonogramem i wymagają dostępu do bazy danych i magazynu (w przypadku wyprowadzanie z kopii zapasowej plików) (w przypadku kopiowania i odczytu zawartości do uwzględnienia w kopii zapasowej). Wynik nie może uzyskać dostępu do żadnego z tych zasobów będą zgodne niepowodzenia wykonywania kopii zapasowej. 

Gdy wystąpi niepowodzenie wykonywania kopii zapasowej, zapoznaj się z tematem najnowsze wyniki, aby zrozumieć, jakiego typu błędu jest wykonywane. W przypadku niepowodzenia dostępu do magazynu Przejrzyj i zaktualizowanie ustawień magazynu używane w konfiguracji kopii zapasowej. W przypadku niepowodzenia dostępu do bazy danych Przejrzyj i zaktualizuj Twojej parametry połączenia w ustawieniach aplikacji; Przejdź do aktualizacji konfiguracji kopii zapasowej, aby poprawnie obejmują wymagane bazy danych. Aby uzyskać więcej informacji na temat tworzenia kopii zapasowej aplikacji zobacz [kopii zapasowej aplikacji sieci web w usłudze Azure App Service](web-sites-backup.md) dokumentacji.

## <a name="nodejs"></a>Podczas wdrażania nowych aplikacji Node.js usługi aplikacji Azure
Usługi aplikacji Azure domyślnej konfiguracji dla aplikacji Node.js ma najlepiej odpowiadający potrzebom najbardziej typowych aplikacji. Jeśli Konfiguracja aplikacji Node.js korzystałby z spersonalizowane dostrajanie do zwiększenia wydajności lub optymalizować wykorzystanie zasobów dla zasobów Procesora i pamięci/sieci, można przejrzeć naszych najlepszych rozwiązań i kroki rozwiązywania problemów. W tym artykule dokumentacji opisano ustawienia programu iisnode, może być konieczne skonfigurowanie aplikacji Node.js, w tym artykule opisano różne scenariusze lub problemy mogą być dostępne w aplikacji i przedstawia sposób rozwiązania tych problemów: [najlepsze rozwiązania i Przewodnik rozwiązywania problemów dla węzła aplikacji w usłudze Azure App Service](app-service-web-nodejs-best-practices-and-troubleshoot-guide.md).   

