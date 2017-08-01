---
title: "Azure Active Directory Domain Services: Aktualizowanie ustawień DNS na potrzeby sieci wirtualnej platformy Azure | Microsoft Docs"
description: "Wprowadzenie do usługi Active Directory Domain Services"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: d4f3e82c-6807-4690-b298-4eabad2b7927
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/27/2017
ms.author: maheshu
ms.translationtype: Human Translation
ms.sourcegitcommit: 1500c02fa1e6876b47e3896c40c7f3356f8f1eed
ms.openlocfilehash: c704ee189072ce8ed196d1ef0a23edd528a10025
ms.contentlocale: pl-pl
ms.lasthandoff: 06/30/2017


---
# <a name="enable-azure-active-directory-domain-services-preview"></a>Włączanie usług Azure Active Directory Domain Services (wersja zapoznawcza)

## <a name="task-4-update-dns-settings-for-the-azure-virtual-network"></a>Zadanie 4. Aktualizowanie ustawień DNS dla sieci wirtualnej platformy Azure
W poprzednich zadaniach konfiguracji pomyślnie włączono usługi Azure Active Directory Domain Services dla katalogu. Następnym krokiem jest zapewnienie komputerom w sieci wirtualnej możliwości łączenia się z tymi usługami i korzystania z nich. W tym artykule zaktualizujesz ustawienia serwera DNS dla sieci wirtualnej, wskazując dwa adresy IP, pod którymi usługi Azure Active Directory Domain Services są dostępne w sieci wirtualnej.

Wykonaj poniższe kroki, aby zaktualizować ustawienia serwera DNS dla sieci wirtualnej, w której włączono usługi Azure Active Directory Domain Services:

1. Karta **Przegląd** zawiera zestaw **wymaganych kroków konfiguracji**, które należy wykonać po pełnym zaaprowizowaniu domeny zarządzanej. Pierwszym krokiem konfiguracji jest **zaktualizowanie ustawień serwera DNS dla sieci wirtualnej**.

    ![Usługi Domain Services — karta Przegląd po pełnej aprowizacji](./media/getting-started/domain-services-provisioned-overview.png)

2. Gdy domena zostanie w pełni zaaprowizowana, na tym kafelku będą wyświetlane dwa adresy IP. Każdy z tych adresów IP reprezentuje kontroler domeny dla domeny zarządzanej.

3. Aby skopiować pierwszy adres IP do schowka, kliknij przycisk kopiowania obok niego. Następnie kliknij przycisk **Skonfiguruj serwery DNS**.

4. Wklej pierwszy adres IP w polu tekstowym **Dodaj serwer DNS** w bloku **Serwery DNS**. Przewiń ekran w lewo, aby skopiować drugi adres IP, i wklej go w polu **Dodaj serwer DNS**.

    ![Usługi Domain Services — aktualizacja usługi DNS](./media/getting-started/domain-services-update-dns.png)

5. Gdy wszystko będzie gotowe do aktualizacji serwerów DNS dla sieci wirtualnej, kliknij pozycję **Zapisz**.

> [!NOTE]
> Maszyny wirtualne w sieci otrzymują nowe ustawienia DNS dopiero po ponownym uruchomieniu. Jeśli chcesz, aby od razu miały zaktualizowane ustawienia usługi DNS, wyzwól ich ponowne uruchomienie przez portal, program PowerShell lub interfejs wiersza polecenia.
>
>

## <a name="next-step"></a>Następny krok
[Zadanie 5. Włączanie synchronizacji haseł w usługach Azure Active Directory Domain Services](active-directory-ds-getting-started-password-sync.md)

