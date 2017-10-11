---
title: "Połącz do usług Azure Analysis Services przy użyciu programu Excel | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak połączyć się z serwerem usług Analysis Services dla platformy Azure przy użyciu programu Excel."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/15/2017
ms.author: owend
ms.openlocfilehash: d51b6980120f1cf9bc8d053d463a73ac600b915f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/29/2017
---
# <a name="connect-with-excel"></a>Łączenie z programem Excel

Po utworzeniu serwera na platformie Azure i wdrożone modelu tabelarycznego, możesz przystąpić do łączenia i rozpocząć eksplorowanie danych.


## <a name="connect-in-excel"></a>Łączenie programu Excel

Łączenie z serwerem w programie Excel jest obsługiwana przy użyciu pobieranie danych w programie Excel 2016. Łączenie za pomocą Kreatora importu tabeli w dodatku Power Pivot nie jest obsługiwane. 

**Aby połączyć w programie Excel 2016**

1. W programie Excel 2016, w **danych** wstążki, kliknij przycisk **Pobierz dane zewnętrzne** > **z innych źródeł** > **usług Analysis Services**.

2. W Kreatorze połączenia danych w **nazwy serwera**, wprowadź nazwę serwera, łącznie z protokołem i identyfikatora URI. Następnie w **poświadczeń logowania**, wybierz pozycję **poniższe nazwy użytkownika i hasła**, a następnie wpisz nazwę organizacji użytkownika, na przykład nancy@adventureworks.comi hasło.

    ![Nawiązywanie połączenia z logowania programu Excel](./media/analysis-services-connect-excel/aas-connect-excel-logon.png)

3. W **wybierz bazę danych i tabeli**, wybierz bazę danych i modelu lub perspektywy, a następnie kliknij przycisk **Zakończ**.
   
    ![Nawiązywanie połączenia z wybierz model programu Excel](./media/analysis-services-connect-excel/aas-connect-excel-select.png)


## <a name="see-also"></a>Zobacz też
[Biblioteki klienta](analysis-services-data-providers.md)   
[Zarządzanie serwerem](analysis-services-manage.md)     


