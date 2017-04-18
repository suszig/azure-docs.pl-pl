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
ms.date: 03/06/2017
ms.author: maheshu
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: abb27292d4b5533fe6f3d66d6921fea8c82f18dd
ms.lasthandoff: 04/12/2017


---
# <a name="update-dns-settings-for-the-azure-virtual-network"></a>Aktualizowanie ustawień DNS dla sieci wirtualnej platformy Azure
## <a name="task-4-update-dns-settings-for-the-azure-virtual-network"></a>Zadanie 4. Aktualizowanie ustawień DNS dla sieci wirtualnej platformy Azure
W poprzednich zadaniach konfiguracji pomyślnie włączono usługi Azure Active Directory Domain Services dla katalogu. Następnym krokiem jest zapewnienie komputerom w sieci wirtualnej możliwości łączenia się z tymi usługami i korzystania z nich. W tym artykule zaktualizujesz ustawienia serwera DNS dla sieci wirtualnej, wskazując dwa adresy IP, pod którymi usługi Azure Active Directory Domain Services są dostępne w sieci wirtualnej.

> [!NOTE]
> Po włączeniu usług Azure Active Directory Domain Services dla katalogu zanotuj ich adresy IP wyświetlane na karcie **Konfigurowanie** katalogu.
>
>

Wykonaj poniższe czynności, aby zaktualizować ustawienia serwera DNS dla sieci wirtualnej, w której włączono usługi Azure Active Directory Domain Services:

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
> Po zaktualizowaniu ustawień serwera DNS dla sieci wirtualnej może upłynąć pewien czas do momentu zastosowania zaktualizowanej konfiguracji DNS na maszynach wirtualnych w sieci. Jeśli maszyna wirtualna nie może połączyć się z domeną, możesz opróżnić pamięć podręczną DNS (polecenie „ipconfig /flushdns”) na maszynie wirtualnej. To polecenie wymusza odświeżenie ustawień DNS na maszynie wirtualnej.
>
>

## <a name="next-steps"></a>Następne kroki
Zadanie 5. [Włączanie synchronizacji haseł w usługach Azure Active Directory Domain Services](active-directory-ds-getting-started-password-sync.md)

