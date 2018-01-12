---
title: "Połączenie do usług Azure Analysis Services przy użyciu usługi Power BI | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak połączyć się z serwerem usług Analysis Services dla platformy Azure przy użyciu usługi Power BI."
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
ms.date: 01/10/2018
ms.author: owend
ms.openlocfilehash: ea1094d0ce858cd7df9c49f18fb81b07e31fca53
ms.sourcegitcommit: c4cc4d76932b059f8c2657081577412e8f405478
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/11/2018
---
# <a name="connect-with-power-bi"></a>Uzyskuj dostęp do usługi Power BI

Po utworzeniu serwera na platformie Azure i wdrożone modelu tabelarycznego, użytkownicy w Twojej organizacji są gotowe do łączenia i rozpocząć eksplorowanie danych. 

> [!TIP]
> Pamiętaj używać najnowszej wersji [Power BI Desktop](https://powerbi.microsoft.com/desktop/).
> 
> 
  
## <a name="connect-in-power-bi-desktop"></a>Połącz w programie Power BI Desktop

1. W programie Power BI Desktop, kliknij przycisk **Pobierz dane** > **Azure** > **usług Azure Analysis Service database**.

2. W **serwera**, wprowadź nazwę serwera. Należy uwzględnić pełny adres URL; na przykład asazure://westcentralus.asazure.windows.net/advworks.

3. W **bazy danych**, jeśli znasz nazwę bazy danych modelu tabelarycznego lub perspektywy, aby nawiązać połączenie, wklej go tutaj. W przeciwnym razie można pozostawić to pole puste i wybierz bazę danych lub perspektywy później.

4. Pozostaw wartość domyślną **połączenia na żywo** opcji, a następnie naciśnij klawisz **Connect**. Import połączenia nie są obecnie obsługiwane.

5. Jeśli zostanie wyświetlony monit, wprowadź poświadczenia logowania. 

6. W **Nawigator**, rozwiń węzeł serwera, a następnie wybierz model lub chcesz nawiązać połączenie, a następnie kliknij przycisk perspektywy **Connect**. Kliknij modelu lub perspektywy, aby wyświetlić wszystkie obiekty w tym widoku.

    Model zostanie otwarty w programie Power BI Desktop z pustym raportem w widoku raportu. Na liście pól Wyświetla wszystkie obiekty modelu ukryty. Stan połączenia jest wyświetlany w prawym dolnym rogu.

## <a name="connect-in-power-bi-service"></a>Połączenia w usłudze Power BI (usługa)

1. Utwórz plik Power BI Desktop, który ma aktywne połączenie modelu na serwerze.
2. W [usługi Power BI](https://powerbi.microsoft.com), kliknij przycisk **Pobierz dane** > **pliki**następnie znajdź i zaznacz plik pbix.



## <a name="see-also"></a>Zobacz także
[Połącz do usług Azure Analysis Services](analysis-services-connect.md)   
[Biblioteki klienta](analysis-services-data-providers.md)

