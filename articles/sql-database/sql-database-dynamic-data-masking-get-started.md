---
title: Azure maskowania danych dynamicznych bazy danych SQL | Dokumentacja firmy Microsoft
description: "Baza danych SQL dane dynamiczne maskowanie ogranicza ujawnienie danych poufnych przez maskowania go użytkownikom bez uprawnień"
services: sql-database
documentationcenter: 
author: ronitr
manager: jhubbard
editor: 
ms.assetid: 4b36d78e-7749-4f26-9774-eed1120a9182
ms.service: sql-database
ms.custom: security
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: On Demand
ms.date: 03/09/2017
ms.author: ronitr; ronmat
ms.openlocfilehash: b75f170870a5f595fcda41196f4de81f237f88b8
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/31/2017
---
# <a name="sql-database-dynamic-data-masking"></a>Maskowanie danych dynamicznych bazy danych SQL

Baza danych SQL dane dynamiczne maskowanie ogranicza ujawnienie danych poufnych przez maskowania go użytkownikom bez uprawnień. 

Dynamiczne maskowanie danych pomaga zapobiec nieautoryzowanemu dostępowi do danych poufnych, umożliwiając klientom wyznaczenie, jaka część danych poufnych może zostać odsłonięta, przy minimalnym wpływie na warstwę aplikacji. Jest to funkcja zabezpieczeń oparta na zasadach, która ukrywa dane poufne w zestawie wyników zapytania w wyznaczonych polach bazy danych, przy czym dane w bazie danych pozostają bez zmian.

Na przykład z przedstawicielem w gnieździe wywołanie może identyfikować obiekty wywołujące kilka cyfry numeru karty kredytowej, ale te elementy danych powinien nie pełni widoczne dla działu obsługi. Czy wszystkie maski, ale cztery ostatnie cyfry żadnych numer karty kredytowej, w wyniku ustawiono wszelkie zapytania można zdefiniować reguł maskowania. Inny przykład można zdefiniować maskę odpowiednie dane do ochrony danych osobowych (dane osobowe), dzięki czemu deweloper może wysyłać zapytania środowisk produkcyjnych w celu rozwiązywania problemów bez naruszania przepisy dotyczące zgodności.

## <a name="sql-database-dynamic-data-masking-basics"></a>Baza danych SQL dynamiczne maskowanie podstawowe informacje o danych
Należy zdefiniować dane dynamiczne maskowanie zasad w portalu Azure, wybierając dynamiczne maskowanie operacji w bloku konfiguracji bazy danych SQL lub bloku ustawienia danych.

### <a name="dynamic-data-masking-permissions"></a>Uprawnienia maskowania danych dynamicznych
Maskowanie danych dynamicznych można skonfigurować przez administratora bazy danych Azure, administrator serwera lub role zabezpieczeń ds.

### <a name="dynamic-data-masking-policy"></a>Zasady maskowania danych dynamicznych
* **Użytkownicy SQL wykluczeni z maskowania** — zestaw A użytkownicy SQL lub tożsamości usługi AAD, pobierające dane zamaskowana w SQL wyniki zapytania. Użytkownicy z uprawnieniami administratora są zawsze wykluczeni z maskowania i wyświetlić oryginalne dane bez żadnych maski.
* **Maskowanie reguły** — zestaw reguł, które definiują wyznaczonych pól do zamaskowania i funkcji maskowania, która jest używana. Wyznaczone pola można zdefiniować przy użyciu nazwy schematu bazy danych oraz nazwę tabeli i nazwę kolumny.
* **Funkcji maskowania** — zestaw metody, które kontrolują ujawnienia danych dla różnych scenariuszy.

| Funkcja maskowania | Logika maskowania |
| --- | --- |
| **Domyślne** |**Pełna maskowania zgodnie z typów danych w odpowiednich polach**<br/><br/>• Użyj XXXX lub mniej Xs, jeśli rozmiar pola jest mniejszy niż 4 znaki dla danych typu ciąg (nchar, ntext, nvarchar).<br/>• Użyj wartości 0 dla typów danych liczbowych (bigint, bit, decimal, int, pieniądze, liczbowe, smallint, smallmoney, tinyint, float, rzeczywistym).<br/>• Użyj 01-01-1900 dla typów danych daty i godziny (Data, datetime2, datetime, datetimeoffset, smalldatetime, czas).<br/>• Aby wariantu języka SQL, wartością domyślną bieżącego typu jest używany.<br/>• XML dokumentu <masked/> jest używany.<br/>• Na użytek pustą wartość specjalne typy danych (tabeli sygnatury czasowej, hierarchyid, GUID, binary, obraz, typy przestrzenne varbinary). |
| **Karty kredytowej** |**Maskowanie metodę, która udostępnia cztery ostatnie cyfry wyznaczone pola** i dodaje ciąg stałej jako prefiksu w formularzu karty kredytowej.<br/><br/>XXXX-XXXX-XXXX-1234 |
| **Adres e-mail** |**Maskowanie metodę, która udostępnia pierwszą literę i zastępuje domeny XXX.com** przy użyciu prefiksu stałym ciągiem w postaci adresu e-mail.<br/><br/>aXX@XXXX.com |
| **Liczby losowe** |**Metoda, która generuje losową liczbę maskowania** zgodnie z wybranych granic i typy danych rzeczywistych. Jeśli wyznaczonych granicach są takie same, funkcja maskowania jest stałej liczbowej.<br/><br/>![Okienko nawigacji](./media/sql-database-dynamic-data-masking-get-started/1_DDM_Random_number.png) |
| **Niestandardowy tekst** |**Metoda, która udostępnia pierwszy i ostatni znak maskowania** i dodaje ciąg niestandardowego dopełnienie w środku. Jeśli oryginalny ciąg jest krótszy niż narażonych prefiksu i sufiksu, tylko ciąg uzupełnienie jest używany. <br/>sufiks prefiks [dopełnienie]<br/><br/>![Okienko nawigacji](./media/sql-database-dynamic-data-masking-get-started/2_DDM_Custom_text.png) |

<a name="Anchor1"></a>

### <a name="recommended-fields-to-mask"></a>Zalecane pola do zamaskowania
Aparat zalecenia DDM flagi pewnych pól z bazy danych jako potencjalnie poufnych pola, które mogą być odpowiednimi obiektami do maskowania. W bloku dynamicznego maskowania danych w portalu pojawi zalecane kolumn bazy danych. To wszystko co należy zrobić, kliknij przycisk **Dodaj maskę** dla co najmniej jedną kolumnę, a następnie **zapisać** dotyczyć maskę dla tych pól.

## <a name="set-up-dynamic-data-masking-for-your-database-using-powershell-cmdlets"></a>Konfigurowanie dynamiczne maskowanie danych dla bazy danych przy użyciu poleceń cmdlet programu Powershell
Zobacz [polecenia cmdlet bazy danych Azure SQL](https://msdn.microsoft.com/library/azure/mt574084.aspx).

## <a name="set-up-dynamic-data-masking-for-your-database-using-rest-api"></a>Konfigurowanie maskowania danych dynamicznych dla bazy danych przy użyciu interfejsu API REST
Zobacz [operacje dla baz danych Azure SQL](https://msdn.microsoft.com/library/dn505719.aspx).

