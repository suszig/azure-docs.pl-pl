---
title: Jednostki SKU rejestru kontenera platformy Azure
description: "Porównanie różnych warstwach usług dostępnych w rejestrze kontenera platformy Azure"
services: container-registry
documentationcenter: 
author: stevelas
manager: balans
editor: mmacy
ms.service: container-registry
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/16/2017
ms.author: stevelas
ms.openlocfilehash: 630bc088fcb6d3c7e5bb3a9713107c3fb6653ec6
ms.sourcegitcommit: 9ae92168678610f97ed466206063ec658261b195
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/17/2017
---
# <a name="azure-container-registry-skus"></a>Jednostki SKU rejestru kontenera platformy Azure

Rejestru kontenera platformy Azure (ACR) są dostępne w wielu warstw usług, znany jako jednostki SKU. Te jednostki SKU zapewniają przewidywalne ceny i kilka opcji jak chcesz użyć prywatnych rejestru Docker na platformie Azure. Wybieranie wyższego poziomu jednostki SKU zawiera więcej wydajność i skalę. Jednak wszystkie jednostki SKU Podaj te same możliwości programowe umożliwiające deweloperom wprowadzenie Basic i przekonwertować na standardowa i Premium w miarę wzrostu użycia rejestru.

## <a name="basic"></a>Podstawowa
Punkt wejścia optymalizacji kosztów dla deweloperów poznawania rejestru kontenera platformy Azure. Rejestry organizacji IANA podstawowa mają te same możliwości programowe jako standardowa i Premium (integracji uwierzytelniania usługi Azure Active Directory, usuwanie obrazów i punkty zaczepienia sieci web), istnieją jednak ograniczenia rozmiaru i użycia.

## <a name="standard"></a>Standardowa
Rejestry organizacji IANA standardowe oferuje te same możliwości jako podstawowa z limitami zasobów pamięci masowej i przepływność obrazu. Rejestry organizacji IANA standardowe powinien zaspokoić potrzeby większości scenariuszy produkcji.

## <a name="premium"></a>Premium
Rejestry organizacji IANA Premium zawierają wyższe limity ograniczenia, takie jak magazyn i jednoczesnych operacji włączania dużych scenariuszy. Oprócz wyższej przepustowości obrazu Premium dostępne są dodatkowe funkcje, takie jak [— replikacja geograficzna](container-registry-geo-replication.md) zarządzania pojedynczego rejestru w wielu regionach, obsługa rejestru Zamknij sieci, do każdego wdrożenia.

## <a name="classic"></a>Wdrożenie klasyczne
Rejestr Classic jednostki SKU włączone początkowa wersja usługi rejestru kontenera platformy Azure w usłudze Azure. Rejestry organizacji IANA klasycznego bazują na konto magazynu Azure tworzy w ramach subskrypcji, co ogranicza możliwość dla ACR zapewnienie wyższego poziomu funkcji, takich jak zwiększona przepływność i replikacja geograficzna. Ze względu na ich ograniczone możliwości planujemy zastąpić klasycznego jednostki SKU w przyszłości.

> [!NOTE]
> Z powodu planowanych amortyzacja rejestru Classic jednostka SKU firma Microsoft zaleca się, że używasz dla wszystkich nowych rejestrów podstawowa, standardowa lub Premium. Uzyskać informacje o konwersji istniejącej rejestru klasycznym, zobacz [Zmiana jednostki SKU](#changing-skus).
>

## <a name="registry-sku-feature-matrix"></a>Macierz funkcji jednostki SKU rejestru

W poniższej tabeli przedstawiono funkcje i limity warstwy usług podstawowa, standardowa i Premium.

| Funkcja | Podstawowa | Standardowa | Premium |
|---|---|---|---|---|
| Magazyn | 10 giB | 100 giB| 500 giB |
| ReadOps na minutę<sup>1, 2</sup> | 1 KB | 300k | 10 000 k |
| WriteOps na minutę<sup>1, 3</sup> | 100 | 500 | 2k |
| Pobierz MB/s przepustowości<sup>1</sup> | 30 | 60 | 100 |
| Przekaż MB/s przepustowości<sup>1</sup> | 10 | 20 | 50 |
| elementów webhook | 2 | 10 | 100 |
| Replikacja geograficzna | Nie dotyczy | Nie dotyczy | [Obsługiwane *(wersja zapoznawcza)*](container-registry-geo-replication.md) |

<sup>1</sup> *ReadOps*, *WriteOps*, i *przepustowości* są szacowane wartości minimalnej. ACR dokłada starań zwiększyć wydajność, ponieważ wymaga użycia.

<sup>2</sup> [ściągania docker](https://docs.docker.com/registry/spec/api/#pulling-an-image) przekłada się wiele operacji odczytu na podstawie liczby warstw w obrazu, a także pobierania manifestu.

<sup>3</sup> [wypychania docker](https://docs.docker.com/registry/spec/api/#pushing-an-image) tłumaczy wiele operacji zapisu, na podstawie liczby warstw, które muszą zostać przeniesiony. A `docker push` obejmuje *ReadOps* można pobrać manifestu dla istniejącego obrazu.

## <a name="manage-registry-size"></a>Zarządzanie rozmiaru rejestru
Ograniczenia pamięci masowej każdej jednostki SKU są przeznaczone do zapewnienia zgodności z typowy scenariusz: podstawowe dla klientów rozpoczynających pracę, standardowych dla większości aplikacji produkcyjnych i Premium dla dużej skali, wydajności i [— replikacja geograficzna](container-registry-geo-replication.md). Cały czas życia rejestr możesz zarządzać jego rozmiar, przez okresowe usuwanie nieużywanych zawartości.

Bieżące użycie rejestru można znaleźć w rejestrze kontenera **omówienie** w portalu Azure:

![Informacje o użyciu rejestru w portalu Azure](media/container-registry-skus/registry-overview-quotas.png)

Rozmiar rejestru można zarządzać, usuwając repozytoria w portalu Azure.

W obszarze **usług**, wybierz pozycję **repozytoria**, kliknij prawym przyciskiem myszy repozytorium, aby usunąć, a następnie wybierz **usunąć**.

![Usuń repozytorium w portalu Azure](media/container-registry-skus/delete-repository-portal.png)

## <a name="changing-skus"></a>Zmiana jednostki SKU

Jednostka SKU rejestru w portalu Azure, można zmienić.

W rejestrze **omówienie** w portalu Azure wybierz **aktualizacji**, następnie wybierz nowy **SKU** z listy rozwijanej jednostki SKU.

![Aktualizacja kontenera rejestr jednostki SKU w portalu Azure](media/container-registry-skus/update-registry-sku.png)

## <a name="changing-from-classic"></a>Zmiana z klasycznym
Po zmianie rejestru klasycznego do warstwy podstawowa, standardowa lub Premium, Azure kopii istniejącego kontenera obrazów z skojarzonego konta magazynu w ramach subskrypcji na konto magazynu zarządzanych przez usługę Azure. Ten proces może potrwać pewien czas.

Podczas konwersji `docker pull` nadal działa, jednak `docker push` jest zablokowana do czasu ukończenia konwersji.

Po ukończeniu subskrypcja konta magazynu jest już używany przez ACR.

### <a name="why-change-from-classic-to-basic-standard-or-premium"></a>Dlaczego mam zmienić Classic Basic, Standard lub Premium?

Ze względu na ograniczone możliwości rejestrów klasycznym zaleca się zmiany z rejestrów klasycznego do warstw podstawowa, standardowa lub Premium. Te wyższego poziomu jednostki SKU głębsze zintegrować rejestru możliwości platformy Azure. Oto niektóre z tych funkcji:

* Integracja usługi Active Directory Azure dla poszczególnych uwierzytelniania (zobacz [logowania acr az](/cli/azure/acr?view=azure-cli-latest#az_acr_login))
* Obsługa usuwania obrazu i tagów
* [Geo-replication](container-registry-geo-replication.md) (Replikacja geograficzna)
* [Elementów Webhook](container-registry-webhook.md)

Najbardziej, rejestru Classic zależy od konta magazynu tego Azure automatycznie udostępniane w ramach subskrypcji platformy Azure, podczas tworzenia rejestru. Z kolei podstawowa, standardowa i Premium jednostki SKU zalet *zarządzanego magazynu*. Oznacza to, że Azure zarządza niewidocznie magazynu obrazów dla Ciebie — oddzielnego konta magazynu nie została utworzona w ramach własnej subskrypcji.

Oto niektóre zalety magazynu zarządzane udostępniane przez rejestrów podstawowa, standardowa i Premium:

* Kontener obrazów są [szyfrowane, gdy](../storage/common/storage-service-encryption.md).
* Obrazy są przechowywane przy użyciu [magazynu geograficznie nadmiarowego](../storage/common/storage-redundancy.md#geo-redundant-storage), jego kopii zapasowej z replikacji w przypadku obrazów.
* Możliwość [przenoszeniu jednostki SKU](#changing-skus), włączanie wyższej przepustowości, po wybraniu SKU wyższego poziomu. Każdej jednostki SKU ACR można spełnia wymagania dotyczące przepływności zwiększania potrzeb. Implementacja jak ACR osiąga żądaną przepływności jest wyrażony jako *zamiar* (przez wybranie bardziej zaawansowanych wersji), bez konieczności zarządzania szczegóły implementacji.

## <a name="pricing"></a>Cennik

Aby cenach informacji o każdej jednostki SKU rejestru dla kontenera platformy Azure, zobacz [cennik rejestru kontenera](https://azure.microsoft.com/pricing/details/container-registry/).

## <a name="next-steps"></a>Następne kroki

**Plan rejestru kontenera platformy Azure**

Odwiedź stronę [plan ACR](https://aka.ms/acr/roadmap) w witrynie GitHub, aby uzyskać informacje o nadchodzących funkcji w usłudze.

**UserVoice rejestru kontenera platformy Azure**

Przesłać opinię i głosuj na nowe sugestii funkcji w [ACR UserVoice](https://feedback.azure.com/forums/903958-azure-container-registry).