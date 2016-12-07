---
title: "Azure AD Domain Services: aktualizowanie ustawień DNS na potrzeby sieci wirtualnej platformy Azure | Microsoft Docs"
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
ms.date: 09/21/2016
ms.author: maheshu
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 81c0564390c57c7ee001ce5a574ab670faf898b4


---
# <a name="azure-ad-domain-services-update-dns-settings-for-the-azure-virtual-network"></a>Usługi domenowe Azure AD — aktualizowanie ustawień DNS dla sieci wirtualnej platformy Azure
## <a name="task-4-update-dns-settings-for-the-azure-virtual-network"></a>Zadanie 4. Aktualizowanie ustawień DNS dla sieci wirtualnej platformy Azure
W poprzednich zadaniach konfiguracji pomyślnie włączono Usługi domenowe Azure AD dla katalogu. Następnym krokiem jest zapewnienie komputerom w sieci wirtualnej możliwości łączenia się z tymi usługami i korzystania z nich. Zaktualizuj ustawienia serwera DNS dla sieci wirtualnej, wskazując dwa adresy IP, pod którymi Usługi domenowe Azure AD są dostępne w sieci wirtualnej.

> [!NOTE]
> Po włączeniu Usług domenowych Azure AD dla katalogu zanotuj ich adresy wyświetlane na karcie **Konfigurowanie** katalogu.
> 
> 

Wykonaj poniższe czynności konfiguracyjne, aby zaktualizować ustawienia serwera DNS dla sieci wirtualnej, w której włączono Usługi domenowe Azure AD.

1. Przejdź do **klasycznego portalu Azure** ([https://manage.windowsazure.com](https://manage.windowsazure.com)).
2. W okienku po lewej stronie wybierz węzeł **Sieci**.
   
    ![Węzeł sieci wirtualnych](./media/active-directory-domain-services-getting-started/virtual-network-select.png)
3. Na karcie **Sieci wirtualne** wybierz sieć wirtualną, w której włączono Usługi domenowe Azure AD, aby wyświetlić jej właściwości.
4. Kliknij kartę **Konfiguracja**.
   
    ![Węzeł sieci wirtualnych](./media/active-directory-domain-services-getting-started/virtual-network-configure-tab.png)
5. W sekcji **Serwery DNS** wprowadź adresy IP Usług domenowych Azure AD.
6. Upewnij się, że wprowadzono obydwa adresy IP wyświetlane w sekcji **Usługi domenowe** na karcie **Konfigurowanie** katalogu.
7. Aby zapisać ustawienia serwera DNS tej sieci wirtualnej, kliknij pozycję **Zapisz** w okienku zadań w dolnej części strony.
   
   ![Zaktualizuj ustawienia serwera DNS dla sieci wirtualnej.](./media/active-directory-domain-services-getting-started/update-dns.png)

> [!NOTE]
> Po zaktualizowaniu ustawień serwera DNS dla sieci wirtualnej może upłynąć pewien czasu do momentu zastosowania zaktualizowanej konfiguracji DNS na maszynach wirtualnych w sieci. Jeśli maszyna wirtualna nie może połączyć się z domeną, możesz opróżnić pamięć podręczną DNS (np. wydając polecenie „ipconfig/flushdns”) na maszynie wirtualnej. To polecenie wymusza odświeżenie ustawień DNS na maszynie wirtualnej.
> 
> 

## <a name="task-5-enable-password-synchronization-to-azure-ad-domain-services"></a>Zadanie 5. Włączanie synchronizacji haseł w Usługach domenowych Azure AD
Następne zadanie konfiguracyjne to [włączenie synchronizacji haseł w Usługach domenowych Azure AD](active-directory-ds-getting-started-password-sync.md).




<!--HONumber=Nov16_HO2-->


