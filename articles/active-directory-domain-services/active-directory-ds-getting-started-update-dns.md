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
ms.openlocfilehash: 8bee2a25f196d645b27f30f21305b1550e44e07a
ms.contentlocale: pl-pl
ms.lasthandoff: 06/30/2017


---
<a id="update-dns-settings-for-the-azure-virtual-network" class="xliff"></a>

# Aktualizowanie ustawień DNS dla sieci wirtualnej platformy Azure
<a id="task-4-update-dns-settings-for-the-azure-virtual-network" class="xliff"></a>

## Zadanie 4. Aktualizowanie ustawień DNS dla sieci wirtualnej platformy Azure
W poprzednich zadaniach konfiguracji pomyślnie włączono usługi Azure Active Directory Domain Services dla katalogu. Następnym krokiem jest zapewnienie komputerom w sieci wirtualnej możliwości łączenia się z tymi usługami i korzystania z nich. W tym artykule zaktualizujesz ustawienia serwera DNS dla sieci wirtualnej, wskazując dwa adresy IP, pod którymi usługi Azure Active Directory Domain Services są dostępne w sieci wirtualnej.

> [!NOTE]
> Po włączeniu usług Azure Active Directory Domain Services dla katalogu zanotuj ich adresy IP wyświetlane na karcie **Konfigurowanie** katalogu.
>
>

Wykonaj poniższe kroki, aby zaktualizować ustawienia serwera DNS dla sieci wirtualnej, w której włączono usługi Azure Active Directory Domain Services:

1. Przejdź do [klasycznej witryny Azure Portal](https://manage.windowsazure.com).
2. W lewym okienku wybierz opcję **Sieci**.  
    Zostanie otwarte okno **Sieci**.

    ![Okno Sieci wirtualne](./media/active-directory-domain-services-getting-started/virtual-network-select.png)
3. Na karcie **Sieci wirtualne** wybierz sieć wirtualną, w której włączono usługi Azure Active Directory Domain Services, aby wyświetlić jej właściwości.
4. Kliknij kartę **Konfiguracja**.

    ![Okno Sieci wirtualne](./media/active-directory-domain-services-getting-started/virtual-network-configure-tab.png)
5. W sekcji **Serwery DNS** wprowadź obydwa adresy IP wyświetlane w sekcji **Usługi domenowe** na karcie **Konfigurowanie** katalogu.
6. Aby zapisać ustawienia serwera DNS tej sieci wirtualnej, kliknij pozycję **Zapisz** w okienku zadań w dolnej części okna.

   ![Aktualizowanie ustawień serwera DNS dla sieci wirtualnej](./media/active-directory-domain-services-getting-started/update-dns.png)

> [!NOTE]
>  Maszyny wirtualne w sieci otrzymują nowe ustawienia DNS dopiero po ponownym uruchomieniu. Jeśli chcesz, aby od razu miały zaktualizowane ustawienia usługi DNS, wyzwól ich ponowne uruchomienie przez portal, program PowerShell lub interfejs wiersza polecenia.
>
>

<a id="next-steps" class="xliff"></a>

## Następne kroki
Zadanie 5. [Włączanie synchronizacji haseł w usługach Azure Active Directory Domain Services](active-directory-ds-getting-started-password-sync.md)

