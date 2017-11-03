---
title: "Korzystanie z okna powłoki chmury Azure (wersja zapoznawcza) | Dokumentacja firmy Microsoft"
description: "Przegląd sposobu użycia okna powłoki chmury Azure."
services: azure
documentationcenter: 
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 10/16/2017
ms.author: juluk
ms.openlocfilehash: 59cd03f6e04e9b096fe740e8a7140e602c0baa58
ms.sourcegitcommit: a7c01dbb03870adcb04ca34745ef256414dfc0b3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/17/2017
---
# <a name="using-the-azure-cloud-shell-window"></a>Korzystanie z okna powłoki chmury Azure

Tym dokumencie opisano sposób użycia okna powłoki chmury.

## <a name="swap-between-bash-and-powershell-environments"></a>Wymiany między środowiskami Bash i programu PowerShell
![](media/using-the-shell-window/env-selector.png)

Selektor środowiska w chmurze powłoki narzędzi do wymiany między środowiskami Bash i programu PowerShell.

## <a name="restart-cloud-shell"></a>Ponownie uruchom powłokę chmury
![](media/using-the-shell-window/restart.png)
> [!WARNING]
> Ponowne uruchamianie powłoki chmurze spowoduje zresetowanie stan maszyny i wszystkie pliki nie utrwalone przez plik udziału zostaną utracone.

* Kliknij przycisk ponowne uruchomienie ikonę na pasku narzędzi powłoki chmury Resetowanie stanu maszyny.

## <a name="minimize--maximize-cloud-shell-window"></a>Minimalizowanie & maksymalizowanie okna powłoki chmury
![](media/using-the-shell-window/minmax.png)
* Kliknij ikonę Minimalizuj na górnym rogu okna, aby je ukryć. Kliknij ikonę powłoki chmury ponownie, aby odkryć.
* Kliknij ikonę Maksymalizuj, aby ustawić maksymalną wysokość okna. Aby przywrócić poprzedni rozmiar okna, kliknij przycisk Przywróć.

## <a name="concurrent-sessions"></a>Równoczesnych sesji
Chmury powłoki umożliwia wielu równoczesnych sesji na kartach przeglądarki, zezwalając każdej sesji istnieje jako oddzielne proces Bash.
Kończenie sesji, należy wyjść z okna każdej sesji, ponieważ każdy proces przebiega niezależnie, mimo że działają na tym samym komputerze.

## <a name="copy-and-paste"></a>Kopiowanie i wklejanie
[!include [copy-paste](../../includes/cloud-shell-copy-paste.md)]

## <a name="resize-cloud-shell-window"></a>Zmień rozmiar okna powłoki chmury
* Kliknij i przeciągnij od górnej krawędzi paska narzędzi w górę lub w dół do zmiany rozmiaru okna powłoki chmury.

## <a name="scrolling-text-display"></a>Przewijanie wyświetlanej tekstu
* Przewiń przy użyciu myszy lub dotykową, aby przenieść terminali tekst.

## <a name="changing-the-text-size"></a>Zmiana rozmiaru tekstu
![](media/using-the-shell-window/text-size.png)
* Kliknij ikonę ustawień u góry po lewej okna, a następnie umieść kursor nad opcji "Rozmiar" i wybierz rozmiar odpowiedni tekst. Wybór zostanie trwale znajdować się między sesjami.

## <a name="exit-command"></a>Exit — polecenie
Uruchomiona `exit` kończy aktywnej sesji. Dzieje się tak domyślnie po upływie 20 minut bez interakcji.

## <a name="next-steps"></a>Następne kroki

[Bash w chmurze powłoki — Szybki Start](quickstart.md)
[programu PowerShell w chmurze powłoki — Szybki Start](quickstart-powershell.md)