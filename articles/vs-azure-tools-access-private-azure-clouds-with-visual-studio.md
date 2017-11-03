---
title: "Uzyskiwanie dostępu do chmur prywatnych Azure z programem Visual Studio | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak uzyskać dostęp do zasobów chmury prywatnej w programie Visual Studio."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 9d733c8d-703b-44e7-a210-bb75874c45c8
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 03/19/2017
ms.author: kraigb
ms.openlocfilehash: b2578c837732ab05d538e9b896ed3a3035075a70
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="accessing-private-azure-clouds-with-visual-studio"></a>Uzyskiwanie dostępu do chmur prywatnych Azure z programem Visual Studio
Domyślnie program Visual Studio obsługuje punkty końcowe REST chmury publicznej Azure. W tym temacie możesz dowiedzieć się, jak do używania certyfikatu chmury prywatnej dostęp do — oraz interakcyjnie - chmury prywatnej w programie Visual Studio.

## <a name="to-access-a-private-azure-cloud-in-visual-studio"></a>Prywatne Azure dostęp do chmury w programie Visual Studio
1. W [klasycznego portalu Azure](http://go.microsoft.com/fwlink/?LinkID=213885) chmury prywatnej, Pobierz swój plik ustawień publikowania, lub skontaktuj się z administratorem dla pliku ustawień publikowania. W publicznej wersji Azure to łącze, aby pobrać ten [https://manage.windowsazure.com/publishsettings/](https://manage.windowsazure.com/publishsettings/). (Pobrany plik ma rozszerzenie `.publishsettings`)

1. Otwórz program Visual Studio

1. W **Eksploratora serwera**, kliknij prawym przyciskiem myszy **Azure** węzła i z menu kontekstowego wybierz **zarządzanie i subskrypcje filtru**.
   
    ![Zarządzanie subskrypcjami polecenia](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790778.png)

1. W **Zarządzanie subskrypcji platformy Microsoft Azure** okno dialogowe, wybierz opcję **certyfikaty** , a następnie wybierz **importu**.
   
    ![Importowanie certyfikatów Azure](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790779.png)

1. W **importu subskrypcji platformy Microsoft Azure** okno dialogowe, wybierz opcję **Przeglądaj**.

    ![Przeglądaj przycisk w oknie dialogowym Import subskrypcji platformy Microsoft Azure](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/browse-button.png)

1. W **Otwórz** okno dialogowe, przejdź do katalogu, w której zapisano plik ustawień publikowania, wybierz plik, a następnie wybierz **Otwórz**.

    ![Wybierz plik ustawień publikowania](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/select-publish-settings-file.png)

1. Gdy zwrócony do **importu subskrypcji platformy Microsoft Azure** okno dialogowe, wybierz opcję **importu**.

    ![Importowanie pliku ustawień publikowania](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790780.png)

    Certyfikaty są zaimportowane z pliku ustawień publikowania do programu Visual Studio, a teraz pozwala na interakcję z zasobami w chmurze prywatnej.
   
## <a name="next-steps"></a>Następne kroki
- [Publikowanie do usługi w chmurze Azure w programie Visual Studio](https://msdn.microsoft.com/library/azure/ee460772.aspx)
- [Porady: pobieranie i importowanie publikowania ustawienia i informacje o subskrypcji](https://msdn.microsoft.com/library/dn385850\(v=nav.70\).aspx)
