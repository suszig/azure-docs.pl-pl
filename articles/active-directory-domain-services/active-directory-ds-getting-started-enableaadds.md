---
title: "Azure Active Directory Domain Services: Włączanie usług Azure Active Directory Domain Services | Microsoft Docs"
description: "Włączanie usług Azure Active Directory Domain Services przy użyciu klasycznej witryny Azure Portal"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: c659da59-f4b5-4edd-b702-1727a8ccb36f
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/28/2017
ms.author: maheshu
ms.translationtype: Human Translation
ms.sourcegitcommit: 1500c02fa1e6876b47e3896c40c7f3356f8f1eed
ms.openlocfilehash: ed72325ca9db99405c6173eb882a92f80cd77f47
ms.contentlocale: pl-pl
ms.lasthandoff: 06/30/2017


---
<a id="enable-azure-active-directory-domain-services-using-the-azure-classic-portal" class="xliff"></a>

# Włączanie usług Azure Active Directory Domain Services przy użyciu klasycznej witryny Azure Portal

<a id="task-3-enable-azure-active-directory-domain-services" class="xliff"></a>

## Zadanie 3. Włączanie usług Azure Active Directory Domain Services
W tym zadaniu należy włączyć usługi Azure Active Directory Domain Services (Azure AD DS) dla katalogu, wykonując następujące kroki:

1. Przejdź do [klasycznej witryny Azure Portal](https://manage.windowsazure.com).
2. W lewym okienku wybierz przycisk **Active Directory**.
3. Wybierz dzierżawę (katalog) usługi Azure Active Directory (Azure AD), dla której chcesz włączyć usługi Azure AD DS.

    ![Wybieranie katalogu usługi Azure AD](./media/active-directory-domain-services-getting-started/select-aad-directory.png)
4. Na stronie **Podgląd katalogu** kliknij kartę **Konfiguracja**.

    ![Karta Konfigurowanie w katalogu](./media/active-directory-domain-services-getting-started/configure-tab.png)
5. W sekcji **Usługi domenowe** zmień opcję **Włącz usługi domenowe dla tego katalogu** na **Tak**.  
    Na stronie zostaną wyświetlone dodatkowe opcje konfiguracji usługi Azure Active Directory Domain Services.

    ![Włączanie Usług domenowych](./media/active-directory-domain-services-getting-started/enable-domain-services.png)

   > [!NOTE]
   > Po włączeniu usług Azure Active Directory Domain Services dla dzierżawy usługa Azure AD wygeneruje i będzie przechowywać skróty poświadczeń Kerberos i NTLM wymagane do uwierzytelniania użytkowników.
   >
   >
6. Określ **nazwę domeny DNS dla usług domenowych**.

   * Domyślna nazwa domeny katalogu (z sufiksem **.onmicrosoft.com**) zostanie wybrana domyślnie.

   * Lista zawiera wszystkie domeny skonfigurowane dla katalogu usługi Azure AD, w tym domeny zweryfikowane i niezweryfikowane, które można skonfigurować na karcie **Domeny**.

   * Możesz również podać niestandardową nazwę domeny. W tym przykładzie niestandardową nazwą domeny jest *contoso100.com*.

     > [!WARNING]
     > Prefiks określony dla nazwy domeny (przykładowo *contoso100* w nazwie domeny *contoso100.com*) nie może zawierać więcej niż 15 znaków. Nie możesz utworzyć domeny usługi Azure Active Directory Domain Services z prefiksem zawierającym ponad 15 znaków.
     >
     >
7. Upewnij się, że nazwa domeny DNS wybrana dla domeny zarządzanej nie istnieje już w sieci wirtualnej. Zwłaszcza sprawdź, czy:

   * masz już domenę z tą samą nazwą domeny DNS w sieci wirtualnej,

   * wybrana sieć wirtualna ma połączenie sieci VPN z siecią lokalną i masz domenę z tą samą nazwą domeny DNS w sieci lokalnej,

   * masz istniejącą usługę w chmurze z tą nazwą w sieci wirtualnej.
8. Wybierz sieć wirtualną, w której usługi Azure Active Directory Domain Services mają być dostępne. Wybierz utworzoną sieć wirtualną i dedykowaną podsieć z listy rozwijanej o nazwie **Połącz usługi domenowe z tą siecią wirtualną**. Rozważ również następujące elementy:

   * Upewnij się, że wybrana sieć wirtualna należy do regionu świadczenia usługi Azure obsługiwanego przez usługi Azure Active Directory Domain Services. Zobacz [Usługi platformy Azure uporządkowane według regionów](https://azure.microsoft.com/regions/#services/), aby upewnić się co do regionów świadczenia usługi Azure, w których usługa Azure Active Directory Domain Services jest dostępna.

   * Sieci wirtualne należące do regionu, w którym usługi Azure Active Directory Domain Services nie są obsługiwane, nie są wyświetlane na liście rozwijanej.

   * Użyj dedykowanej podsieci w obrębie sieci wirtualnej dla usług Azure Active Directory Domain Services. *Nie* wybieraj podsieci bramy. Zobacz [zagadnienia dotyczące pracy w sieci](active-directory-ds-networking.md).

   * Podobnie sieci wirtualne, które zostały utworzone przy użyciu usługi Azure Resource Manager, nie pojawiają się na liście rozwijanej. Sieci wirtualne oparte na usłudze Resource Manager nie są obecnie obsługiwane przez usługi Azure Active Directory Domain Services.
9. Aby włączyć usługi Azure Active Directory Domain Services, kliknij pozycję **Zapisz** w okienku zadań w dolnej części strony.
    * Gdy usługi Azure Active Directory Domain Services są włączane dla katalogu, strona będzie wyświetlać stan *Oczekujące*.

        ![Okno włączania usług domenowych](./media/active-directory-domain-services-getting-started/enable-domain-services-pendingstate.png)

        > [!NOTE]
        > Usługi Azure Active Directory Domain Services oferują wysoką dostępność na potrzeby domeny zarządzanej. Po włączeniu usług Azure Active Directory Domain Services zostaną kolejno wyświetlone adresy IP, pod którymi usługi domenowe będą dostępne w sieci wirtualnej. Drugi adres IP pojawi się wkrótce po pierwszym, kiedy tylko usługa zapewni wysoką dostępność domeny. Po skonfigurowaniu i uaktywnieniu wysokiej dostępności domeny w sekcji **usług domenowych** na karcie **Konfigurowanie** powinny być widoczne dwa adresy IP.
        >
        >
    * Po około 20–30 minutach w polu **Adres IP** na stronie **Konfigurowanie** zobaczysz pierwszy adres IP, pod którym są dostępne usługi domenowe.

        ![Okno usług domenowych wyświetlające pierwszy aprowizowany adres IP](./media/active-directory-domain-services-getting-started/domain-services-enabled-firstdc-available.png)
    * Gdy domena oferuje działającą opcję wysokiej dostępności, na stronie są wyświetlane dwa adresy IP. Twoja domena zarządzana jest dostępna w wybranej sieci wirtualnej pod tymi dwoma adresami IP.

10. Zanotuj te dwa adresy IP, aby zaktualizować ustawienia DNS sieci wirtualnej. Ten krok umożliwia łączenie maszyn wirtualnych w sieci wirtualnej z domeną w celu wykonywania operacji takich jak przyłączanie do domeny.

    ![Okno usług domenowych wyświetlające oba aprowizowane adresy IP](./media/active-directory-domain-services-getting-started/domain-services-enabled-bothdcs-available.png)

> [!NOTE]
> Czas synchronizacji z domeną zarządzaną zależy od rozmiaru dzierżawy usługi Azure AD (np. liczby użytkowników lub grup). Ten proces synchronizacji jest wykonywany w tle. W przypadku dużych dzierżaw z dziesiątkami tysięcy obiektów synchronizacja wszystkich użytkowników, członkostw w grupach i poświadczeń może potrwać dzień lub dwa.
>
>

<a id="next-step" class="xliff"></a>

## Następny krok
[Zadanie 4. Aktualizowanie ustawień usługi DNS dla sieci wirtualnej platformy Azure](active-directory-ds-getting-started-update-dns.md)

