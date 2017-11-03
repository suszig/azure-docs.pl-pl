---
title: "Przy użyciu pulpitu zdalnego z rolami Azure | Dokumentacja firmy Microsoft"
description: "Przy użyciu pulpitu zdalnego z rolami systemu Azure"
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: f5727ebe-9f57-4d7d-aff1-58761e8de8c1
ms.service: multiple
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: eab135d10c0d6df8ca72ac47d6804017a998a3d2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="using-remote-desktop-with-azure-roles"></a>Przy użyciu pulpitu zdalnego z rolami systemu Azure
Przy użyciu zestawu SDK platformy Azure i usługi pulpitu zdalnego, można uzyskać dostępu do ról platformy Azure i maszyn wirtualnych, które są obsługiwane przez platformę Azure. W programie Visual Studio można skonfigurować usługi pulpitu zdalnego z projektu usługi w chmurze Azure. Aby włączyć usługi pulpitu zdalnego, musi utworzyć projekt pracy, który zawiera co najmniej jedną rolę, a następnie opublikuj go do platformy Azure.

> [!IMPORTANT]
> Możesz uzyskać dostęp Azure roli do rozwiązywania problemów lub tylko programowanie. Każda maszyna wirtualna służy do uruchamiania konkretnej roli w aplikacji platformy Azure, nie można uruchomić inne aplikacje klienckie. Jeśli chcesz używać platformy Azure do hostowania maszyny wirtualnej, która służy do celów, zobacz Uzyskiwanie dostępu do maszyn wirtualnych platformy Azure z poziomu Eksploratora serwera.
> 
> 

## <a name="to-enable-and-use-remote-desktop-for-an-azure-role"></a>Aby włączyć i za pomocą pulpitu zdalnego dla roli Azure
1. W Eksploratorze rozwiązań Otwórz menu skrótów projektu usługi w chmurze, a następnie wybierz **publikowania**.
   
    **Publikowanie aplikacji platformy Azure** zostanie wyświetlony Kreator.
   
    ![Publikowanie polecenia dla projektu usługi w chmurze](./media/vs-azure-tools-remote-desktop-roles/IC799161.png)
2. W dolnej części **ustawień publikowania platformy Azure Microsoft** strony kreatora wybierz pozycję **Włącz pulpit zdalny** wszystkie pola wyboru ról. 
   
    **Konfigurację usług pulpitu zdalnego** zostanie wyświetlone okno dialogowe.
3. W dolnej części **konfigurację usług pulpitu zdalnego** oknie dialogowym wybierz **więcej opcji** przycisku. 
   
    Zostaną wyświetlone pole listy rozwijanej, które można tworzyć lub wybrać certyfikat, dzięki czemu można zaszyfrować informacji poświadczeń przy nawiązywaniu połączeń za pośrednictwem pulpitu zdalnego.
4. Na liście rozwijanej wybierz  **&lt;Utwórz >**, lub wybierz istniejący z listy. 
   
    Jeśli wybierzesz istniejącego certyfikatu, pominęli następujące kroki.
   
   > [!NOTE]
   > Certyfikaty, które są potrzebne dla połączeń usług pulpitu zdalnego są inne niż certyfikaty, które są używane dla innych operacji platformy Azure. Certyfikat dostępu zdalnego musi mieć klucz prywatny.
   > 
   > 
   
    **Tworzenie certyfikatu** zostanie wyświetlone okno dialogowe.
   
   1. Podaj przyjazną nazwę dla nowego certyfikatu, a następnie wybierz pozycję **OK** przycisku. W polu listy rozwijanej zostanie wyświetlony nowy certyfikat.
   2. W **konfigurację usług pulpitu zdalnego** okna dialogowego wprowadź nazwę użytkownika i hasło.
      
       Nie można użyć istniejącego konta. Nie można określić administratora jako nazwa użytkownika dla nowego konta.
      
      > [!NOTE]
      > Jeśli hasło nie spełnia wymagań dotyczących złożoności, pojawi się czerwona ikona obok pola tekstowego hasła. Hasło musi zawierać wielkie litery, małe litery i liczby lub symbole.
      > 
      > 
   3. Wybierz datę na konto, które wygasa i po którym połączeń pulpitu zdalnego zostanie zablokowana.
   4. Po podane wszystkie wymagane informacje, wybierz **OK** przycisku.
      
       Wiele ustawień, które umożliwiają dostęp zdalny są dodawane do plików cscfg i csdef.
5. W **ustawień publikowania platformy Azure Microsoft** kreatora wybierz **OK** przycisku, gdy wszystko jest gotowe do publikowania usługi w chmurze.
   
    Jeśli nie chcesz opublikować, wybierz **anulować** przycisku. Ustawienia konfiguracji są zapisywane, a później można opublikować usługi w chmurze.

## <a name="connect-to-an-azure-role-by-using-remote-desktop"></a>Połącz do roli Azure przy użyciu pulpitu zdalnego
Po opublikowaniu usługi w chmurze na platformie Azure, można użyć Eksploratora serwera do zalogowania się do maszyn wirtualnych Azure hosta. 

1. W Eksploratorze serwera rozwiń **Azure** węzeł, a następnie rozwiń węzeł usługi w chmurze i jedną z jej ról, aby wyświetlić listę wystąpień.
2. Otwórz menu skrótów węzła wystąpienia, a następnie wybierz pozycję **połączyć za pomocą pulpitu zdalnego**.
   
    ![Łączenie za pośrednictwem pulpitu zdalnego](./media/vs-azure-tools-remote-desktop-roles/IC799162.png)
3. Wprowadź nazwę użytkownika i hasło, które wcześniej utworzony. Obecnie użytkownik jest zalogowany do sesji zdalnej.

