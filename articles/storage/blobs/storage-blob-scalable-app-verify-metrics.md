---
title: "Sprawdź przepustowości i opóźnień metryki dla konta magazynu w portalu Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak sprawdzić, przepustowości i opóźnień metryki dla konta magazynu w portalu."
services: storage
documentationcenter: 
author: georgewallace
manager: jeconnoc
editor: 
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: csharp
ms.topic: tutorial
ms.date: 12/12/2017
ms.author: gwallace
ms.custom: mvc
ms.openlocfilehash: b3102bd4e40e10fe88c12295794da37e359c56f1
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/13/2017
---
# <a name="verify-throughput-and-latency-metrics-for-a-storage-account"></a>Sprawdź przepustowości i opóźnień metryki dla konta magazynu

Ten samouczek ma cztery i końcowe część serii. W poprzednim samouczkach przedstawiono sposób przekazywania i pobierania larges liczby losowe dane do konta magazynu platformy Azure. Ten samouczek pokazuje, jak metryki umożliwia wyświetlanie przepustowości i opóźnień w portalu Azure.

W czterech części serii, możesz dowiedzieć się, jak:

> [!div class="checklist"]
> * Skonfiguruj wykresy w portalu Azure
> * Weryfikowanie metryk przepływności i opóźnienia

[Metryki magazynu Azure](../common/storage-metrics-in-azure-monitor.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) używa monitora Azure w celu zapewnienia ujednoliconego podglądu na wydajność i dostępność konta magazynu.

## <a name="configure-metrics"></a>Skonfiguruj metrykę

Przejdź do **metryki (wersja zapoznawcza)** w obszarze **ustawienia** na koncie magazynu.

Wybierz obiekt Blob z **usługi SUB** listy rozwijanej.

W obszarze **METRYKA**, wybierz jedną z metryki w poniższej tabeli:

Następujące metryki dają pogląd, opóźnienia i przepływność aplikacji. Metryki, które można skonfigurować w portalu są średnie 1 minutę. Jeśli transakcja Zakończono środku minutę minuty danych jest halfed dla średniej. W aplikacji przekroczyły operacje przekazywania i pobierania i pod warunkiem output rzeczywistej ilości czasu zajęło przekazywania i pobierania plików. Te informacje można w połączeniu z portalu metryki aby w pełni zrozumieć przepływności.

|Metryka|Definicja|
|---|---|
|**Powodzenie opóźnienie E2E**|Średnie opóźnienie end-to-end pomyślnych żądań wysyłanych do usługi magazynu lub określonej operacji interfejsu API. Ta wartość obejmuje wymagany czas przetwarzania w ramach usługi Azure Storage do odczytu żądania, wysyłania odpowiedzi i otrzyma odpowiedź.|
|**Powodzenie opóźnienia serwera**|Średni czas używaną do przetwarzania żądań pomyślnie usługi Azure Storage. Ta wartość nie obejmuje opóźnienia sieci, określona w SuccessE2ELatency. |
|**Transakcje**|Liczba żądań wysyłanych do usługi magazynu lub określonej operacji interfejsu API. Liczba ta obejmuje udane i nieudane żądania, a także żądań, które tworzone błędy. W tym przykładzie rozmiar bloku została ustawiona na 100 MB. W takim przypadku każdy blok 100 MB jest traktowany jako transakcja.|
|**Transfer danych przychodzących**|Ilość danych wejściowych. Liczba ta obejmuje transfer danych przychodzących z zewnętrznego klienta do usługi Azure Storage, a także ruch przychodzący w obrębie platformy Azure. |
|**Transfer danych wychodzących**|Ilość danych wyjściowych. Liczba ta obejmuje wyjście z zewnętrznego klienta do usługi Azure Storage, a także Wyjście w obrębie platformy Azure. W związku z tym ta liczba nie odzwierciedla wyjście rozliczeniowy. |

Wybierz **ostatnich 24 godzin (automatycznie)** obok **czas**. Wybierz **ostatniej godziny** i **minutę** dla **dokładności**, następnie kliknij przycisk **Zastosuj**.

![Metryki konta magazynu](./media/storage-blob-scalable-app-verify-metrics/figure1.png)

Wykresy może mieć więcej niż jedna Metryka do nich przypisane, ale przypisywanie więcej niż jedna Metryka wyłącza możliwość grupy według wymiarów.

## <a name="dimensions"></a>Wymiary

[Wymiary](../common/storage-metrics-in-azure-monitor.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#metrics-dimensions) są używane do przyglądać się głębiej wykresy i uzyskać więcej szczegółowych informacji. Różne metryki mają różne wymiary. Jeden wymiar, który jest dostępny jest **Nazwa interfejsu API** wymiaru. Ten wymiar dzieli się wykres do każdego oddzielnego wywołania interfejsu API. Pierwszy obraz poniżej przedstawia wykres przykład całkowita liczba transakcji dla konta magazynu. Drugi obraz zawiera takie same wykresu, ale przy użyciu interfejsu API nazwę wybranego wymiaru. Jak widać, znajduje się każdą transakcję, podając więcej szczegółów na liczbę wywołań zostały wprowadzone przez Nazwa interfejsu API.

![Metryki konta magazynu — transakcje bez wymiaru](./media/storage-blob-scalable-app-verify-metrics/transactionsnodimensions.png)

![Metryki konta magazynu — transakcje](./media/storage-blob-scalable-app-verify-metrics/transactions.png)

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Gdy grupa zasobów, maszyna wirtualna i wszystkie pokrewne zasoby nie będą już potrzebne, można je usunąć. Aby to zrobić, wybierz grupę zasobów dla maszyny Wirtualnej, a następnie kliknij przycisk Usuń.

## <a name="next-steps"></a>Następne kroki

W części czterech serii przedstawiono o wyświetlaniu metryki dla rozwiązania przykład, np.:

> [!div class="checklist"]
> * Skonfiguruj wykresy w portalu Azure
> * Weryfikowanie metryk przepływności i opóźnienia

Wykonaj to łącze, aby wyświetlić przykłady wbudowanych magazynu.

> [!div class="nextstepaction"]
> [Przykłady skryptów magazynu Azure](storage-samples-blobs-cli.md)

[previous-tutorial]: storage-blob-scalable-app-download-files.md