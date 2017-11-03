---
title: Zrozumienie cennik Centrum IoT Azure | Dokumentacja firmy Microsoft
description: "Przewodnik dewelopera po — informacje o jak pomiaru i cenach współpracuje z tym Centrum IoT poszło przykłady."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 1ac90923-1edf-4134-bbd4-77fee9b68d24
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/29/2017
ms.author: elioda
ms.openlocfilehash: 05006a78cc7d82bc048ec5706465f7140eb40e94
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-iot-hub-pricing-information"></a>Informacje o cenach Centrum IoT Azure

[Centrum IoT Azure cennik] [ lnk-pricing] zawiera ogólne informacje dotyczące różnych jednostki SKU i cenach dla Centrum IoT. Ten artykuł zawiera więcej informacji na temat sposobu różne funkcje Centrum IoT są mierzone jako wiadomości przez Centrum IoT.

## <a name="charges-per-operation"></a>Opłaty na operację

| Operacja | Informacje o rozliczeniach | 
| --------- | ------------------- |
| Operacje rejestru tożsamości <br/> (Tworzenie, pobieranie, listy, aktualizowanie i usuwanie) | Nie zostanie obciążona. |
| Komunikaty z urządzenia do chmury | Pomyślnie wysłane wiadomości są naliczane w fragmentów 4 KB na ruch przychodzący do Centrum IoT, na przykład się, że komunikat 6KB rozliczany 2 wiadomości. |
| Komunikaty chmury do urządzenia | Pomyślnie wysłane wiadomości są pobierane w fragmentów 4 KB, na przykład wiadomości 6 KB rozliczany 2 wiadomości. |
| Przekazywania plików | Transfer plików do magazynu Azure nie jest pomiaru przez Centrum IoT. Komunikaty rozpoczęcia i zakończenia transferu plików są naliczane, ponieważ messaged mierzonego w przyrostach 4 KB. Na przykład przesyłania pliku 10 MB jest pobierana dwa komunikaty oraz koszt usługi Azure Storage. |
| Metody bezpośrednie | Metoda pomyślnego żądania są naliczane w fragmentów 4 KB, odpowiedzi z treściami niepustym są naliczane 4 KB jako dodatkowe komunikaty. Żądania do odłączonego urządzenia są naliczane jako komunikaty w fragmentów 4 KB. Na przykład metody z treścią 6 KB, który daje w odpowiedzi nie jednostki z urządzenia, jest rozliczany jako dwa komunikaty; Metoda o treści 6 KB, który daje w odpowiedzi 1 KB z urządzenia jest rozliczany jako dwa komunikaty żądania plus inny komunikat odpowiedzi. |
| Liczba odczytów bliźniaczej reprezentacji urządzenia | Dwie urządzenia odczytuje z urządzenia i z powrotem rozwiązania zakończenia są naliczane jako komunikaty w fragmentów 512-bajtowego. Na przykład odczytywania dwie 6 KB urządzenia jest rozliczany jako 12 wiadomości. |
| Aktualizacje dwie urządzeń (znaczniki i właściwości) | Aktualizacje dwie urządzeń z urządzenia i z zaplecza rozwiązania są naliczane jako komunikaty w fragmentów 512-bajtowego. Na przykład odczytywania dwie 6 KB urządzenia jest rozliczany jako 12 wiadomości. |
| Urządzenie dwie zapytań | Zapytania są naliczane jako komunikaty, w zależności od wielkości wynik w fragmentów 512-bajtowego. |
| Operacje zadań <br/> (tworzenie, aktualizowanie, wyświetlanie, usuwanie) | Nie zostanie obciążona. |
| Operacje na urządzenie zadania | Operacje zadania (na przykład aktualizacji dwie urządzenia i metody) są naliczane normalnego. Na przykład zadanie wynikiem wywołania metody 1000 1 KB żądań i odpowiedzi pusty treści jest pobierana 1000 komunikatów. |

> [!NOTE]
> Wszystkie rozmiary są obliczane, biorąc pod uwagę rozmiar ładunku w bajtach (protokół ramek zostanie zignorowana). W przypadku wiadomości (które mają właściwości, oraz i treść) rozmiar jest obliczana w sposób niezależny od protokołu, zgodnie z opisem w [Centrum IoT wiadomości przewodnik dewelopera][lnk-message-size].

## <a name="example-1"></a>Przykład #1

Urządzenie wysyła jeden komunikat urządzenia do chmury 1 KB na minutę do Centrum IoT, który jest odczytywany przez usługi Azure Stream Analytics. Zaplecze rozwiązania wywołuje metodę (z 512-bajtowych ładunku) na urządzeniu co 10 minut do wyzwalania określonych akcji. Urządzenie odpowiada do metody z wynikiem 200 bajtów.

Urządzenie zużywa komunikat 1 * = 60 minut * 24 godzin 1440 wiadomości na dzień komunikatów urządzenia do chmury i 2 żądania i odpowiedzi * 6 razy na godzinę * 24 godziny = 288 komunikatów dla metod, łącznie z 1728 wiadomości na dzień.

## <a name="example-2"></a>Przykład #2

Urządzenie wysyła jeden komunikat urządzenia do chmury 100 KB co godzinę. Aktualizuje również dwie jego urządzenia z 1 KB ładunków co 4 godziny. Rozwiązania kończyć, raz dziennie, odczytuje dwie 14 KB urządzenia i aktualizuje ładunków 512-bajtowych do zmiany konfiguracji.

Urządzenie zużywa 25 wiadomości (100KB / 4KB) * 24 godziny dla wiadomości urządzenia do chmury, a także 2 wiadomości (o rozmiarze 1KB/0,5 KB) * 6 razy dziennie aktualizacje dwie urządzenia, dla wszystkich 612 wiadomości na dzień.
Zaplecza rozwiązania zużywa komunikaty 28 (14KB/0,5 KB), aby odczytać dwie urządzenia, a także 1 komunikat, aby zaktualizować go, łącznie z 29 komunikatów.

Całkowita liczba urządzenia oraz zaplecza rozwiązania wykorzystywać 641 wiadomości na dzień.


[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub
[lnk-message-size]: iot-hub-devguide-messages-construct.md
