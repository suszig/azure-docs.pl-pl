---
title: "Połącz do usług Azure Analysis Services przy użyciu programu Excel | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak połączyć się z serwerem usług Analysis Services dla platformy Azure przy użyciu programu Excel."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: kfile
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 02/14/2018
ms.author: owend
ms.openlocfilehash: f5f77b70874f10a29b4ea4dba307a67361c8e2bc
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/21/2018
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


## <a name="see-also"></a>Zobacz także
[Biblioteki klienta](analysis-services-data-providers.md)   
[Zarządzanie serwerem](analysis-services-manage.md)     


