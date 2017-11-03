---
title: "Ustawienia funkcji cloud App Discovery rejestru dla serwera Proxy usług | Dokumentacja firmy Microsoft"
description: "Celem tego tematu jest dostarczają czynności, które należy wykonać, aby ustawić wymaganego portu na komputerach z systemem agenta Cloud App Discovery."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 8d78e925-e331-40ba-904a-e4ef14260cac
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: b8584809b76d6be12a6f489f0bb819081d1803d2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="cloud-app-discovery-registry-settings-for-proxy-services"></a>Ustawienia funkcji cloud Discovery aplikacji rejestru dla usługi serwera Proxy
W tym temacie ma na celu informacje, jak wykonać, aby ustawić wymaganego portu na komputerach z systemem agenta Cloud App Discovery. Domyślnie agenta Cloud App Discovery jest skonfigurowany do użycia tylko porty 80 i 443. Jeśli planujesz zainstalowanie Cloud App Discovery w środowisku z serwera proxy, który jest używany niestandardowy port (80 ani 443), należy skonfigurować agentów do użycia tego portu. Konfiguracja jest oparta na kluczu rejestru.

> [!TIP] 
> Zapoznaj się z ulepszenia teraz bez agenta Cloud App Discovery w usłudze Azure Active Directory (Azure AD), która rozszerzono przez [Integracja z usługą Microsoft Cloud App Security](https://portal.cloudappsecurity.com).

## <a name="modify-the-port-used-by-the-computer-running-the-cloud-app-discovery-agent"></a>Zmodyfikuj port używany przez komputer z uruchomionym agentem Cloud App Discovery

1. Uruchom Edytor rejestru.
  ![Uruchom](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy01.png)
2. Przejdź do lub utwórz następujący klucz rejestru: **HKLM_LOCAL_MACHINE\Software\Microsoft\Cloud Discovery\Endpoint aplikacji**
3. Utwórz nową **ciągu wielokrotnego** wartość o nazwie **porty**. 
  ![Nowy](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy02.png)
4. Aby otworzyć **Edytowanie ciągu wielokrotnego** okna dialogowego, kliknij dwukrotnie **porty** wartość.
5. W **dane wartości**dodać wszystkie porty niestandardowe, które są używane przez Twoją organizację i wprowadź następujące wartości: <br><br>
   **80** <br>
   **8080** <br>
   **8118** <br>
   **8888** <br>
   **81** <br>
   **12080** <br>
   **6999** <br>
   **30606** <br>
   **31595** <br>
   **4080** <br>
   **443** <br>
   **1110** <br><br>
   ![Edytowanie ciągu wielokrotnego](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy03.png)
6. Kliknij przycisk **OK** zamknąć **Edytowanie ciągu wielokrotnego** okna dialogowego.

## <a name="next-steps"></a>Następne kroki

* [Jak odnajdywać niezatwierdzone aplikacje w chmurze używanych mojej organizacji](active-directory-cloudappdiscovery-whatis.md) 

