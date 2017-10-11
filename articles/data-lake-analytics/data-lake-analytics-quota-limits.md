---
title: "Limity przydziału usługi Azure Data Lake Analytics | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak dostosować i zwiększyć limity przydziału na kontach usługi Azure Data Lake Analytics (ADLA)."
services: data-lake-analytics
keywords: Azure Data Lake Analytics
documentationcenter: 
author: omidm1
editor: omidm1
ms.assetid: 49416f38-fcc7-476f-a55e-d67f3f9c1d34
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/18/2017
ms.author: omidm
ms.openlocfilehash: 957f306ea0e80b5830ad64e5ef06c6d122d9eccc
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/03/2017
---
# <a name="azure-data-lake-analytics-quota-limits"></a>Limity przydziału usługi Azure Data Lake Analytics

Dowiedz się, jak dostosować i zwiększyć limity przydziału na kontach usługi Azure Data Lake Analytics (ADLA). Znajomość tych limitów mogą ułatwić zrozumienie zachowania zadania z języka U-SQL. Wszystkie limity przydziału są miękkie, więc może zwiększyć maksymalną wartością dotarcie z nami.

## <a name="azure-subscriptions-limits"></a>Limity subskrypcji platformy Azure

**Maksymalna liczba ADLA konta dla subskrypcji:** 5

 Jest to maksymalna liczba kont ADLA, które można utworzyć na subskrypcję. Jeśli próbujesz utworzyć konto ADLA szóstego, wystąpi błąd "Osiągnięto maksymalną liczbę kont usługi Data Lake Analytics może (5) w regionie, w obszarze Nazwa subskrypcji". W takim przypadku Usuń wszystkie nieużywane konta ADLA albo dotrzeć do nas przez [otwarcie biletu pomocy technicznej](#increase-maximum-quota-limits).

## <a name="adla-account-limits"></a>Limity konta ADLA

**Maksymalna liczba jednostek Analytics (AUs) dla konta:** 250

Jest to maksymalna liczba AUs, które można uruchomić jednocześnie w ramach Twojego konta. Jeśli Twoja Suma uruchamianie AUs we wszystkich zadań przekracza ten limit, automatycznie są kolejkowane zadania nowsza. Na przykład:

* Jeśli masz tylko jedno zadanie uruchomione z 250 AUs, podczas przesyłania drugiej zadania go będzie czekać w kolejce zadań dopiero po zakończeniu pierwszego zadania.
* Jeśli masz pięć zadania uruchomione i każdego używa 50 AUs, podczas przesyłania szóstego zadanie, które wymaga 20 AUs oczekuje w kolejce zadań, dopóki istnieją 20 AUs dostępne.

**Maksymalna liczba jednoczesnych zadań U-SQL dla danego konta:** 20

Jest to maksymalna liczba zadań, które można uruchomić jednocześnie w ramach Twojego konta. Powyżej tej wartości automatycznie są kolejkowane zadania nowsza.

## <a name="adjust-adla-quota-limits-per-account"></a>Dostosuj ADLA limity przydziału dla konta

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).
2. Wybierz istniejące konto ADLA.
3. Kliknij pozycję **Właściwości**.
4. Dostosuj **równoległości** i **równoczesnych zadań** w zależności od potrzeb.

    ![Blok portalu usługi Azure Data Lake Analytics](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-properties.png)

## <a name="increase-maximum-quota-limits"></a>Zwiększ limit przydziału maksymalnej

1. W portalu Azure, otwórz żądanie pomocy technicznej.

    ![Blok portalu usługi Azure Data Lake Analytics](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-help-support.png)

    ![Blok portalu usługi Azure Data Lake Analytics](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-support-request.png)
2. Wybierz typ problemu **przydziału**.
3. Wybierz użytkownika **subskrypcji** (Upewnij się, nie jest "wersję próbną").
4. Wybierz typ przydziału **usługi Data Lake Analytics**.

    ![Blok portalu usługi Azure Data Lake Analytics](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-support-request-basics.png)

5. W bloku problem opisano żądanego Zwiększ limit z **szczegóły** z czego potrzebujesz tej dodatkowej pojemności.

    ![Blok portalu usługi Azure Data Lake Analytics](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-support-request-details.png)

6. Sprawdź informacje kontaktowe i Utwórz żądanie obsługi.

Firma Microsoft sprawdzenia żądania przez i próbuje jak najszybciej dostosowania potrzeb biznesowych.

## <a name="next-steps"></a>Następne kroki

* [Omówienie usługi Microsoft Azure Data Lake Analytics](data-lake-analytics-overview.md)
* [Zarządzanie usługą Azure Data Lake Analytics przy użyciu programu Azure PowerShell](data-lake-analytics-manage-use-powershell.md)
* [Monitorowanie zadań usługi Azure Data Lake Analytics i rozwiązywanie problemów przy użyciu witryny Azure Portal](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)
