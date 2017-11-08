---
title: "Zarządzanie rekordami kontroli dostępu w StorSimple | Dokumentacja firmy Microsoft"
description: "Informacje dotyczące używania rekordy kontroli dostępu (ACRs) do określenia, których hostów można połączyć do woluminu w urządzeniu StorSimple."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 2f1475d8-36a5-4cc4-84b9-adf8a310b60c
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/03/2017
ms.author: alkohli
ms.openlocfilehash: 320569e302d145d695f3be9684e97786fa82e80b
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/07/2017
---
# <a name="use-the-storsimple-manager-service-to-manage-access-control-records"></a>Zarządzanie rekordami kontroli dostępu przy użyciu usługi Menedżer StorSimple
> [!NOTE]
> Klasyczny portal dla urządzenia StorSimple jest przestarzały. Menedżerowie urządzenia StorSimple zostanie automatycznie przełączona do nowego portalu Azure, zgodnie z harmonogramem wycofywanie. Otrzymasz wiadomość e-mail i powiadomienie portalu dla tego przeniesienia. Ten dokument również zostaną wkrótce wycofane. Aby wyświetlić wersję tego artykułu dla nowego portalu Azure, przejdź do [zarządzania rekordami kontroli dostępu przy użyciu usługi Menedżer StorSimple](storsimple-8000-manage-acrs.md). Wszelkie pytania dotyczące przeniesienie, zobacz [— często zadawane pytania: przejść do portalu Azure](storsimple-8000-move-azure-portal-faq.md).

## <a name="overview"></a>Omówienie
Rekordy kontroli dostępu (ACRs) pozwala na określenie, których hostów można połączyć się wolumin w urządzeniu StorSimple. ACRs to określonego woluminu i zawierać nazwy kwalifikowanej iSCSI (nazw IQN) z hostów. Gdy host próbuje nawiązać połączenia z woluminu, urządzenie sprawdza ACR skojarzona z tego woluminu dla nazwy IQN, jeśli istnieje dopasowanie, połączenie zostanie nawiązane. Kontrola dostępu rejestruje sekcji na **Konfiguruj** strona wyświetla wszystkie rekordy kontroli dostępu z odpowiednich nazw IQN hostów.

W tym samouczku opisano następujące typowe zadania związane z ACR:

* Dodaj rekord kontroli dostępu 
* Edytuj rekord kontroli dostępu 
* Usuń rekord kontroli dostępu 

> [!IMPORTANT]
> * Podczas przypisywania ACR do woluminu, zajmie się, że wolumin nie jest jednocześnie dostępna przez więcej niż jednego hosta z systemem innym niż klastrowane, ponieważ może to spowodować uszkodzenie woluminu. 
> * Podczas usuwania ACR z woluminu, upewnij się, że odpowiedniego hosta nie korzysta wolumin usunięcia może powodować zakłócenia odczytu i zapisu.
> 
> 

## <a name="add-an-access-control-record"></a>Dodaj rekord kontroli dostępu
Użyj usługi Menedżer StorSimple **Konfiguruj** stronę, aby dodać ACRs. Zazwyczaj co ACR zostanie skojarzony z jednego woluminu.

Wykonaj poniższe kroki, aby dodać ACR.

#### <a name="to-add-an-access-control-record"></a>Aby dodać rekord kontroli dostępu
1. Na stronie docelowej usługi, wybierz usługę, kliknij dwukrotnie nazwę usługi, a następnie kliknij **Konfiguruj** kartę.
2. Na liście tabelarycznych w obszarze **rekordy kontroli dostępu**, dostawy **nazwa** dla rekordu ACR.
3. Podaj nazwę IQN hosta z systemem Windows w obszarze **Nazwa inicjatora iSCSI**. Aby uzyskać nazwy IQN hosta z systemu Windows Server, wykonaj następujące czynności:
   
   * Uruchom inicjator iSCSI firmy Microsoft na hoście z systemem Windows.
   * W oknie **Właściwości inicjatora iSCSI** na karcie **Konfiguracja** zaznacz i skopiuj ciąg z pola **Nazwa inicjatora**.
   * Wklej ten ciąg w **Nazwa inicjatora iSCSI** pola w tabeli ACRs w klasycznym portalu Azure.
4. Kliknij przycisk **zapisać** Aby zapisać nowo utworzony ACR. Tabelarycznej listę zostaną zaktualizowane w celu odzwierciedlenia to dodawanie.

## <a name="edit-an-access-control-record"></a>Edytuj rekord kontroli dostępu
Możesz użyć **Konfiguruj** strony w klasycznym portalu Azure, aby edytować ACRs. 

> [!NOTE]
> Można modyfikować tylko ACRs, które nie są obecnie w użyciu. Aby edytować ACR skojarzony z woluminem, który jest obecnie używany, musisz najpierw wykonać woluminu w trybie offline.
> 
> 

Wykonaj poniższe kroki, aby edytować ACR.

#### <a name="to-edit-an-access-control-record"></a>Aby edytować rekord kontroli dostępu
1. Na stronie docelowej usługi, wybierz usługę, kliknij dwukrotnie nazwę usługi, a następnie kliknij **Konfiguruj** kartę.
2. Tabelaryczne liście rekordy kontroli dostępu, umieść kursor nad ACR, który chcesz zmodyfikować.
3. Podaj nową nazwę i/lub IQN dla ACR.
4. Kliknij przycisk **zapisać** można zapisać zmodyfikowanej ACR. Tabelarycznej listę zostaną zaktualizowane w celu odzwierciedlenia tej zmiany.

## <a name="delete-an-access-control-record"></a>Usuń rekord kontroli dostępu
Możesz użyć **Konfiguruj** strony w klasycznym portalu Azure, aby usunąć ACRs. 

> [!NOTE]
> Można usunąć tylko ACRs, które nie są obecnie w użyciu. Aby usunąć ACR skojarzony z woluminem, który jest obecnie używany, musisz najpierw wykonać woluminu w trybie offline.
> 
> 

Wykonaj poniższe kroki, aby usunąć rekord kontroli dostępu.

#### <a name="to-delete-an-access-control-record"></a>Aby usunąć rekord kontroli dostępu
1. Na stronie docelowej usługi, wybierz usługę, kliknij dwukrotnie nazwę usługi, a następnie kliknij **Konfiguruj** kartę.
2. Tabelaryczne liście rekordy kontroli dostępu (ACRs), umieść kursor nad ACR, który chcesz usunąć.
3. Ikona usuwania (**x**) będzie wyświetlana w prawej kolumnie extreme ACR, który można wybrać. Kliknij przycisk **x** ikonę, aby usunąć ACR.
4. Po wyświetleniu monitu o potwierdzenie, kliknij przycisk **tak** aby kontynuować usuwanie. Tabelarycznej listę zostaną zaktualizowane, aby odzwierciedlić usunięcia.

## <a name="next-steps"></a>Następne kroki
* Dowiedz się więcej o [zarządzania woluminami StorSimple](storsimple-manage-volumes.md).
* Dowiedz się więcej o [przy użyciu usługi Menedżer StorSimple do administrowania urządzeniem StorSimple](storsimple-manager-service-administration.md).

