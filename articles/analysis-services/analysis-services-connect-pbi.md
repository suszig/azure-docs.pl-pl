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
ms.date: 11/01/2017
ms.author: owend
ms.openlocfilehash: 5f3ad1c4df2c21a8d52a8a3c4d7d311af32720a9
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/02/2017
---
# <a name="connect-with-power-bi"></a>Uzyskuj dostęp do usługi Power BI

Po utworzeniu serwera na platformie Azure i wdrożone modelu tabelarycznego, użytkownicy w Twojej organizacji są gotowe do łączenia i rozpocząć eksplorowanie danych. 

> [!TIP]
> Pamiętaj używać najnowszej wersji [Power BI Desktop](https://powerbi.microsoft.com/desktop/).
> 
> 
  
## <a name="connect-in-power-bi-desktop"></a>Połącz w programie Power BI Desktop

1. W programie Power BI Desktop, kliknij przycisk **Pobierz dane** > **Azure** > **usług Azure Analysis Service database**.

2. W **serwera**, wprowadź nazwę serwera. 
    
    Należy uwzględnić pełny adres URL. Na przykład asazure://westcentralus.asazure.windows.net/advworks.

3. W **bazy danych**, jeśli znasz nazwę bazy danych modelu tabelarycznego lub perspektywy, aby nawiązać połączenie, wklej go tutaj. W przeciwnym razie można pozostawić to pole puste i wybierz bazę danych lub perspektywy później.

4. Pozostaw wartość domyślną **połączenia na żywo** opcji, a następnie naciśnij klawisz **Connect**. 

5. Jeśli zostanie wyświetlony monit, wprowadź poświadczenia logowania. 

6. W **Nawigator**, rozwiń węzeł serwera, a następnie wybierz model lub chcesz nawiązać połączenie, następnie kliknij przycisk perspektywy **Connect**. Kliknij modelu lub perspektywy, aby wyświetlić wszystkie obiekty w tym widoku.

    Model zostanie otwarty w programie Power BI Desktop z pustym raportem w widoku raportu. Na liście pól Wyświetla wszystkie obiekty modelu ukryty. Stan połączenia jest wyświetlany w prawym dolnym rogu.

## <a name="connect-in-power-bi-service"></a>Połączenia w usłudze Power BI (usługa)

1. Utwórz plik Power BI Desktop, który ma aktywne połączenie modelu na serwerze.
2. W [usługi Power BI](https://powerbi.microsoft.com), kliknij przycisk **Pobierz dane** > **pliki**. Znajdź i zaznacz plik.



## <a name="see-also"></a>Zobacz też
[Połącz do usług Azure Analysis Services](analysis-services-connect.md)   
[Biblioteki klienta](analysis-services-data-providers.md)

