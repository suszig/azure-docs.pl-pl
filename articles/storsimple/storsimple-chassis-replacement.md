---
title: "Zastąp podstawę na urządzeniu z serii StorSimple 8000 | Dokumentacja firmy Microsoft"
description: "Opisuje sposób usunięty i zastąpiony obudowy StorSimple obudowy podstawowego lub EBOD obudowy."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 537659ed-4c46-49c1-b1e4-186262f2542d
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/02/2017
ms.author: alkohli
ms.openlocfilehash: 7bc23e64331ad18c604ffaa29476766827119cd4
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/04/2017
---
# <a name="replace-the-chassis-on-your-storsimple-device"></a>Zastąp podstawę na urządzeniu StorSimple
> [!NOTE]
> Klasyczny portal dla urządzenia StorSimple jest przestarzały. Menedżerowie urządzenia StorSimple zostanie automatycznie przełączona do nowego portalu Azure, zgodnie z harmonogramem wycofywanie. Otrzymasz wiadomość e-mail i powiadomienie portalu dla tego przeniesienia. Ten dokument również zostaną wkrótce wycofane. Aby wyświetlić wersję tego artykułu dla nowego portalu Azure, przejdź do [Zastąp podstawę na urządzeniu StorSimple](storsimple-8000-chassis-replacement.md). Wszelkie pytania dotyczące przeniesienie, zobacz [— często zadawane pytania: przejść do portalu Azure](storsimple-8000-move-azure-portal-faq.md).

## <a name="overview"></a>Omówienie
W tym samouczku wyjaśniono, jak usunąć i Zastąp podstawę urządzenia z serii StorSimple 8000. Model StorSimple 8100 to urządzenie jednej obudowie (wspólnej obudowie), 8600 jest urządzeniem podwójną obudowy (dwie obudowy). W przypadku modelu 8600 są potencjalnie dwóch obudowy, które może zakończyć się niepowodzeniem w urządzeniu: obudowa obudowa podstawowego lub podstawę dla obudowa EBOD.

W obu przypadkach obudowy zastąpienia, który jest dostarczany przez firmę Microsoft jest pusta. Nie zasilania i chłodzenia modułów (PCMs), moduły kontrolera stacje dysków półprzewodnikowych (SSD), dysków twardych (HDD) lub EBOD moduły będą dołączone.

> [!IMPORTANT]
> Przed usunięcie i zastąpienie obudowy, zapoznaj się z informacjami bezpieczeństwa w [wymiana składników sprzętowych StorSimple](storsimple-hardware-component-replacement.md).
> 
> 

## <a name="remove-the-chassis"></a>Usuń obudowy
Wykonaj poniższe kroki, aby usunąć obudowy na urządzeniu StorSimple.

#### <a name="to-remove-a-chassis"></a>Aby usunąć podstawę
1. Upewnij się, że urządzenia StorSimple jest zamknięta i odłączone od źródeł zasilania.
2. Usuń wszystkie sieci i kabli SAS, jeśli ma to zastosowanie.
3. Usuń jednostkę w stojaku.
4. Usuwanie wszystkich dysków i zanotuj miejsc, w których są usuwane. Aby uzyskać więcej informacji, zobacz [Usuń dysk](storsimple-disk-drive-replacement.md#remove-the-disk-drive).
5. Na obudowę EBOD (jeśli jest to podstawę, która nie powiodła się) Usuń EBOD modułów kontrolera. Aby uzyskać więcej informacji, zobacz [usunąć kontroler EBOD](storsimple-ebod-controller-replacement.md#remove-an-ebod-controller). 
   
    Na głównej obudowy (jeśli jest to podstawę, która nie powiodła się) Usuń kontrolery i zanotuj miejsc, w których są usuwane. Aby uzyskać więcej informacji, zobacz [usunąć kontroler](storsimple-controller-replacement.md#remove-a-controller).

## <a name="install-the-chassis"></a>Zainstaluj obudowy
Wykonaj poniższe kroki, aby zainstalować obudowy na urządzeniu StorSimple.

#### <a name="to-install-a-chassis"></a>Aby zainstalować podstawę
1. Zainstaluj obudowy w stojaku. Aby uzyskać więcej informacji, zobacz [urządzenia w stojaku do urządzenia 8100 StorSimple](storsimple-8100-hardware-installation.md#rack-mount-your-storsimple-8100-device) lub [urządzenia w stojaku do urządzenia 8600 StorSimple](storsimple-8600-hardware-installation.md#rack-mount-your-storsimple-8600-device).
2. Po obudowy jest zamontowane w stojaku, należy zainstalować moduły kontrolera w tej samej pozycji, które poprzednio były zainstalowane w.
3. Zainstaluj dyski w tej samej pozycji i gniazd, które poprzednio były zainstalowane w.
   
   > [!NOTE]
   > Firma Microsoft zaleca, aby najpierw zainstalować dyski SSD w gniazdach, a następnie zainstaluj dysków twardych.
   > 
   > 
4. Z urządzeniem, zamontowane w stojaku i składniki zainstalowane Podłącz urządzenie do źródła zasilania odpowiednie, a następnie Włącz urządzenie. Aby uzyskać więcej informacji, zobacz [Podłączanie kabli do urządzenia StorSimple 8100](storsimple-8100-hardware-installation.md#cable-your-storsimple-8100-device) lub [Podłączanie kabli do urządzenia StorSimple 8600](storsimple-8600-hardware-installation.md#cable-your-storsimple-8600-device).

## <a name="next-steps"></a>Następne kroki
Dowiedz się więcej o [wymiana składników sprzętowych StorSimple](storsimple-hardware-component-replacement.md).

