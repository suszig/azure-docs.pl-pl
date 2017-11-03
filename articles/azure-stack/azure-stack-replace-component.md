---
title: "Zastąp składnik sprzętowy w węźle jednostki skali stosu Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak zastąpić składnik sprzętowy w systemie Azure stosu zintegrowany."
services: azure-stack
documentationcenter: 
author: troettinger
manager: byronr
editor: 
ms.assetid: c6e036bf-8c80-48b5-b2d2-aa7390c1b7c9
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/20/2017
ms.author: twooley
ms.openlocfilehash: 430274025bc6cd657a733fb80feb6cd7b39a36f3
ms.sourcegitcommit: cf4c0ad6a628dfcbf5b841896ab3c78b97d4eafd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/21/2017
---
# <a name="replace-a-hardware-component-on-an-azure-stack-scale-unit-node"></a>Zastąp składnik sprzętowy w węźle jednostki skali Azure stosu

*Dotyczy: Azure stosu zintegrowane systemy*

W tym artykule opisano ogólny proces Zastąp składniki sprzętowe, które są bez wyłączania. Rzeczywiste zastąpienie, które kroki będą się różnić w oparciu z dostawcą sprzętu producenta sprzętu (OEM). W dokumentacji udostępnianej przez dostawcę pola zamiennych (FRU) szczegółowy opis kroków, które są specyficzne dla systemu Azure stosu zintegrowane.

Następujące składniki bez wyłączania:

- PROCESOR CPU *
- Pamięć *
- Płyty głównej/kontrolera zarządzania płytą główną (BMC) / wideo karty
- Karty magistrali hosta/kontrolera dysku (HBA) / IDE
- Karta sieciowa (NIC)
- Dysk systemu operacyjnego *
- Dyski danych (dyski, które nie obsługują dynamicznej wymiany, na przykład PCI-e kart) *

* Te składniki mogą obsługiwać dynamicznej wymiany, ale mogą różnić w zależności od implementacji dostawcy. W dokumentacji producenta OEM FRU szczegółowy opis kroków.

Poniższy diagram przepływu przedstawia ogólny proces FRU wymiana sprzętu z systemem innym niż wyłączania części.

![Diagram przepływu pokazujący składnika zastąpienia przepływu](media/azure-stack-replace-component/ReplaceComponentFlow.PNG)

* Tej akcji nie może być wymagany na podstawie warunku fizycznego sprzętu.

** Czy z dostawcą sprzętu OEM wykonuje wymiana składników i aktualizacje oprogramowania układowego może się różnić w zależności na obsługuje kontraktu.

## <a name="review-alert-information"></a>Przeglądanie informacji o alertach

Kondycja stosu Azure i monitorowania systemu monitoruje kondycję kart sieciowych i dysków z danymi, które są kontrolowane przez bezpośrednie miejsca do magazynowania. Nie monitoruje innych składników sprzętowych. Dla innych składników sprzętowych specyficznych dla dostawcy sprzętu rozwiązanie monitorujące, które działa na hoście cyklu życia sprzętu pojawienia się alertów.

## <a name="component-replacement-process"></a>Proces zamiany składnika

Następujące kroki służą jako ogólne omówienie proces zamiany składnika. Bez odwołujących się do dokumentacji przez producentów OEM FRU nie wykonaj następujące kroki.

1. Użyj [opróżnienia](azure-stack-node-actions.md#scale-unit-node-actions) akcji w celu uruchomienia trybu konserwacji węzła jednostki skalowania. Ta akcja nie może być wymagany na podstawie warunku fizycznego sprzętu.
2. Po węzeł jednostki skalowania jest w trybie konserwacji, użyj [wyłączenie](azure-stack-node-actions.md#scale-unit-node-actions) akcji. Ta akcja nie może być wymagany na podstawie warunku fizycznego sprzętu.
 
   > [!NOTE]
   > W rzadkich przypadkach wyłączy akcji nie działa w zamian użyj interfejsu sieci web uzyskiwania informacji na temat kontrolera zarządzania płytą główną.

3. Wymienić uszkodzony sprzętu. Czy z dostawcą sprzętu OEM wykonuje wymiana składników może różnić w zależności od umowy pomocy technicznej.  
4. Zaktualizuj oprogramowanie układowe. Wykonaj upewnij się, że składnik sprzętowy zastąpionego ma zatwierdzonego oprogramowania układowego zastosowany poziom za pomocą sprzętu hosta cyklu życia procesu aktualizacji oprogramowania układowego specyficznych dla dostawcy. Czy z dostawcą sprzętu OEM wykonuje ten krok może różnić w zależności od umowy pomocy technicznej.  
5. Użyj [naprawy](azure-stack-node-actions.md#scale-unit-node-actions) akcji, aby przywrócić węzła jednostki skalowania do jednostki skalowania.
6. Użyj uprzywilejowanych punktu końcowego [sprawdzić stan naprawy dysku wirtualnego](azure-stack-replace-disk.md#check-the-status-of-virtual-disk-repair). Nowe dyski danych zadania naprawy pełne magazynu może zająć kilka godzin w zależności od obciążenia systemu i zużytego miejsca.
7. Akcja naprawy zakończy działanie, aby zweryfikować, że wszystkie aktywne alerty zostało automatycznie zamknięte.

## <a name="next-steps"></a>Następne kroki

- Uzyskać informacji dotyczących zastępowania wyłączania dysku fizycznego, zobacz [wymienić dysk](azure-stack-replace-disk.md).
- Uzyskać informacji dotyczących zastępowania na węźle fizycznym, zobacz [zamienić węzła jednostki skali](azure-stack-replace-node.md). 