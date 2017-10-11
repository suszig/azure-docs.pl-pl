---
title: "Włącz szyfrowanie dla konta magazynu w Centrum zabezpieczeń Azure | Dokumentacja firmy Microsoft"
description: "Ten dokument zawiera implementowania zaleceń Centrum zabezpieczeń Azure ** włączenie szyfrowania dla usługi Azure magazynu konta **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/20/2016
ms.author: terrylan
ms.openlocfilehash: b7b2e8a12cbab68da9c8fcc348e8e3c543607007
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/29/2017
---
# <a name="enable-encryption-for-azure-storage-account-in-azure-security-center"></a>Włącz szyfrowanie dla konta magazynu Azure w Centrum zabezpieczeń Azure
Centrum zabezpieczeń Azure może zalecić Włącz szyfrowanie usługi Magazyn Azure dla przechowywanych danych.

Szyfrowanie usługi Magazyn (SSE) polega na szyfrowaniu danych podczas ich zapisywania w magazynie Azure i odszyfrowywania danych przed pobierania.  SSE jest obecnie dostępny tylko dla usługi obiektów Blob platformy Azure i może służyć do blokowe i stronicowe obiekty BLOB i uzupełnialnych obiektów blob.  Aby dowiedzieć się więcej, zobacz [szyfrowanie usługi Magazyn danych magazynowanych](../storage/common/storage-service-encryption.md).


> [!Note]
> Po włączeniu szyfrowania, tylko nowe dane są szyfrowane. Wszystkie istniejące obiekty BLOB na koncie magazynu pozostać niezaszyfrowana. Aby zaszyfrować istniejące obiekty BLOB, zobacz [często zadawane pytania dotyczące magazynu usługi szyfrowania](../storage/common/storage-service-encryption.md#frequently-asked-questions-about-storage-service-encryption-for-data-at-rest).
>
>

Szyfrowanie usługi Magazyn jest obsługiwana tylko na kontach magazynu Menedżera zasobów. Klasycznych kont magazynu nie są obecnie obsługiwane. Aby poznać klasycznego i modeli wdrażania usługi Resource Manager, zobacz [modele wdrażania Azure](../azure-classic-rm.md).

> [!NOTE]
> Informacje na temat usługi przedstawiono w tym dokumencie za pomocą przykładowego wdrożenia.  Ten dokument nie jest przewodnik krok po kroku.
>
>

## <a name="implement-the-recommendation"></a>Wykonania zalecenia
1. W **zalecenia** bloku, wybierz opcję **Włącz szyfrowanie dla konta magazynu Azure**.
   ![Włączanie szyfrowania dla konta magazynu][1]
2. **Włącz szyfrowanie magazynu** zostanie otwarty blok. Ten blok zawiera listę kontami magazynu Azure, w których Magazyn, szyfrowanie jest wyłączone. W tym przykładzie załóżmy wybierz **storageacct1**.
   ![Włącz szyfrowanie magazynu][2]
3. **Szyfrowania** bloku **storageacct1** otwiera. Wybierz **włączone**.
   ![Blok szyfrowania][3]
4. Wybierz pozycję **Zapisz**.

Teraz ma włączone szyfrowanie magazynu dla **storageacct1**.


## <a name="see-also"></a>Zobacz też
Ten dokument pokazano sposób wykonania zalecenia Centrum zabezpieczeń "Włącz szyfrowanie dla konta magazynu Azure". Aby dowiedzieć się więcej na temat szyfrowanie usługi Magazyn Azure, zobacz następujące tematy:

* [Szyfrowanie usługi Magazyn Azure dla przechowywanych danych](../storage/common/storage-service-encryption.md)

Aby dowiedzieć się więcej na temat Centrum zabezpieczeń, zobacz następujące artykuły:

* [Ustawianie zasad zabezpieczeń w Centrum zabezpieczeń Azure](security-center-policies.md) — informacje o sposobie konfigurowania zasad zabezpieczeń dla subskrypcji platformy Azure i grup zasobów.
* [Monitorowanie kondycji zabezpieczeń w Centrum zabezpieczeń Azure](security-center-monitoring.md) — informacje o sposobie monitorowania kondycji zasobów platformy Azure.
* [Reagowanie na alerty zabezpieczeń w Centrum zabezpieczeń Azure i zarządzanie nimi](security-center-managing-and-responding-alerts.md) — Dowiedz się, jak reagowania na alerty zabezpieczeń i zarządzania nimi.
* [Zarządzanie zaleceniami dotyczącymi zabezpieczeń w Centrum zabezpieczeń Azure](security-center-recommendations.md) — Dowiedz się, w jaki sposób zalecenia ułatwiają ochronę zasobów platformy Azure.
* [Centrum zabezpieczeń Azure — często zadawane pytania](security-center-faq.md) — często zadawane pytania dotyczące korzystania z usługi wyszukiwania.
* [Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) — wpisy na blogu dotyczące zabezpieczeń platformy Azure i zgodności.

<!--Image references-->
[1]: ./media/security-center-enable-encryption-for-storage-account/enable-encryption-for-storage-account.png
[2]: ./media/security-center-enable-encryption-for-storage-account/enable-storage-encryption.png
[3]: ./media/security-center-enable-encryption-for-storage-account/encryption-blade.png
