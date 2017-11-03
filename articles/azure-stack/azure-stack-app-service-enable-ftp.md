---
title: "Włącz FTP w usłudze aplikacji Azure stosu | Dokumentacja firmy Microsoft"
description: "Kroki umożliwiające FTP w usłudze aplikacji Azure stosu"
services: azure-stack
documentationcenter: 
author: ErikjeMS
manager: stefsch
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 4/6/2017
ms.author: erikje
ms.openlocfilehash: 9cadc57831ac7f7e5d32b10a4a87dab3fac02958
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="enable-ftp-in-app-service-on-azure-stack"></a>Włączanie protokołu FTP w usłudze App Service w usłudze Azure Stack

Gdy usługi aplikacji na stosie Azure zostały pomyślnie wdrożone, jeśli chcesz włączyć publikacji FTP, dzięki czemu dzierżawców może przekazywać swoje pliki aplikacji oraz zawartość, istnieją pewne dodatkowe kroki, które muszą zostać wykonane.  W przyszłych wersjach spowoduje automatyczne następujące kroki.

> [!NOTE]
> Te kroki są usługi lub Administratorzy przedsiębiorstwa, skonfigurowanie usługi aplikacji na dostawcy zasobów Azure stosu.

## <a name="enable-ftp"></a>Włącz FTP

1.  Zaloguj się do portalu Azure stosu jako administratora usługi.
2.  Przejdź do **interfejsy sieciowe** i wybierz **FTP kart** w obszarze **grupy zasobów** - **AppService lokalnego**. ![Interfejsy sieciowe Azure stosu][1]
3.  Uwaga **publicznego adresu IP** z **FTP kart**. 
![Szczegóły interfejs sieci Azure stosu][2]
4.  Następnie przejdź do **maszyn wirtualnych** i wybierz **FTP0-VM**. ![Maszyny wirtualne Azure stosu][3]
5.  Otwórz sesję pulpitu zdalnego do maszyny Wirtualnej przy użyciu **Connect** przycisk i zaloguj się do sesji przy użyciu poświadczeń administratora, należy ustawić podczas wdrażania aplikacji usługi.  
![Szczegóły maszyny wirtualnej Azure stosu][4]
6.  Otwórz **Internet Information Service (IIS) Manager** na Maszynie wirtualnej FTP (FTP0-VM).
7.  W obszarze **witryny** wybierz **Hosting witryny FTP**.
8.  Otwórz **Obsługa zapory FTP**. ![Menedżer usług IIS na FTP0 usługi aplikacji — maszyny Wirtualnej][5]
9.  Wprowadź publiczny adres IP karty Sieciowej FTP i kliknij przycisk **Zastosuj** ![Obsługa zapory FTP Menedżera usług IIS][6]

## <a name="validate-the-enabling-of-ftp"></a>Sprawdź poprawność włączenie FTP

1.  Zaloguj się do portalu Azure stosu jako administratora usługi lub dzierżawcy.
2.  Przejdź do **usługi aplikacji** i wybierz sieci Web, mobilnych lub aplikacji interfejsu API, które zostały utworzone. ![App Services][7]
3.  Uwagi szczegóły aplikacji **nazwa hosta FTP** i **nazwy użytkownika serwera FTP/wdrożenia**. ![Szczegóły aplikacji usługi aplikacji][8]
> [!NOTE]
> Jeśli nie ma pozycji w obszarze **nazwy użytkownika serwera FTP/wdrożenia**, należy ustawić wdrożenia poświadczenia pierwszego przy użyciu **poświadczenia wdrażania** bloku.

4.  Otwórz Eksploratora Windows, wprowadź nazwę hosta FTP do paska na przykład ftp://ftp.appservice.azurestack.local adresu pliku
5.  Po wyświetleniu monitu wprowadź **poświadczenia wdrażania** zanotowaną w kroku 3, jeśli włączono funkcję zobaczysz listę katalogów zawartości aplikacji usługi aplikacji. ![Wyświetlanie listy plików FTP][9]
<!--Image references-->
[1]: ./media/azure-stack-app-service-enable-ftp/azure-stack-app-service-enable-ftp-network-interfaces.png
[2]: ./media/azure-stack-app-service-enable-ftp/azure-stack-app-service-enable-ftp-network-interface-details.png
[3]: ./media/azure-stack-app-service-enable-ftp/azure-stack-app-service-enable-ftp-virtual-machines.png
[4]: ./media/azure-stack-app-service-enable-ftp/azure-stack-app-service-enable-ftp-virtual-machines-FTP0-VM.png
[5]: ./media/azure-stack-app-service-enable-ftp/azure-stack-app-service-enable-ftp-IIS-Manager.png
[6]: ./media/azure-stack-app-service-enable-ftp/azure-stack-app-service-enable-ftp-IIS-Manager-FTP-Firewall-Support.png
[7]: ./media/azure-stack-app-service-enable-ftp/azure-stack-app-service-enable-ftp-validate-app-services.png
[8]: ./media/azure-stack-app-service-enable-ftp/azure-stack-app-service-enable-ftp-validate-app-service-app-detail.png
[9]: ./media/azure-stack-app-service-enable-ftp/azure-stack-app-service-enable-ftp-validate-ftp-file-listing.png
