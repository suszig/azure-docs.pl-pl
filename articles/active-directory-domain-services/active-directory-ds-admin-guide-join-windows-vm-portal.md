---
title: "Azure Active Directory Domain Services: Dołącz Maszynę wirtualną systemu Windows Server do domeny zarządzanej | Dokumentacja firmy Microsoft"
description: "Dołącz maszynę wirtualną systemu Windows Server do usług domenowych Azure AD"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: 29316313-c76c-4fb9-8954-5fa5ec82609e
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/19/2017
ms.author: maheshu
ms.openlocfilehash: 1ea3f7271bd165bf42d520e4a0267a80dcca58d5
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/11/2017
---
# <a name="join-a-windows-server-virtual-machine-to-a-managed-domain"></a>Przyłączanie maszyny wirtualnej z systemem Windows Server do domeny zarządzanej
W tym artykule pokazano, jak wdrożyć maszynę wirtualną systemu Windows Server przy użyciu portalu Azure. Następnie widoczny jest sposób Dołącz maszynę wirtualną do domeny zarządzanej usług domenowych Azure AD.

## <a name="step-1-create-the-windows-server-virtual-machine"></a>Krok 1: Tworzenie maszyny wirtualnej systemu Windows Server
Wykonaj poniższe kroki, aby utworzyć maszynę wirtualną systemu Windows przyłączone do sieci wirtualnej, w której włączono usługi domenowe Azure AD.

1. Zaloguj się w witrynie Azure Portal pod adresem [http://portal.azure.com](http://portal.azure.com).
2. Kliknij przycisk **Nowy** znajdujący się w lewym górnym rogu witryny Azure Portal.
3. Wybierz pozycję **Wystąpienia obliczeniowe**, a następnie wybierz pozycję **Windows Server 2016 Datacenter**.

    ![Wybierz obraz](./media/active-directory-domain-services-admin-guide/create-windows-vm-select-image.png)
4. Skonfiguruj ustawienia podstawowe dla maszyny wirtualnej na **podstawy** stronie kreatora.

    ![Skonfiguruj ustawienia podstawowe dla maszyny Wirtualnej](./media/active-directory-domain-services-admin-guide/create-windows-vm-basics.png)

    > [!TIP]
    > Nazwa użytkownika i hasło wprowadzone w tym miejscu są konta administratora lokalnego, używane do logowania się do maszyny wirtualnej. Wybierz silne hasło, aby chronić maszyny wirtualnej przed atakami siłowymi hasła. Nie należy wprowadzać poświadczenia konta użytkownika domeny, w tym miejscu.
    >

5. Wybierz **rozmiar** dla maszyny wirtualnej. Aby wyświetlić więcej rozmiarów, wybierz pozycje **Wyświetl wszystkie** lub zmień filtr **Obsługiwany typ dysku**.

    ![Wybierz rozmiar maszyny wirtualnej](./media/active-directory-domain-services-admin-guide/create-windows-vm-size.png)

6. Na **ustawienia** strony kreatora wybierz pozycję wdrożeniu sieci wirtualnej, w którym usług domenowych Azure AD zarządzane domeny. Wybierz inną podsieć niż wdrożonej do domeny zarządzanej. Dla innych ustawień, Zachowaj wartości domyślne, a następnie kliknij przycisk **OK**.

    ![Wybierz sieć wirtualną maszyny wirtualnej](./media/active-directory-domain-services-admin-guide/create-windows-vm-select-vnet.png)

    > [!TIP]
    > **Wybierz właściwą sieć i podsieci.**
    > Wybierz albo sieć wirtualną, w której jest wdrażane domeny zarządzanej lub sieci wirtualnej podłączonej do jej wirtualnej sieci komunikacji równorzędnej. W przypadku wybrania odłączony sieci wirtualnej maszyny wirtualnej nie można przyłączyć do domeny zarządzanej.
    > Firma Microsoft zaleca wdrażania domeny zarządzanej w dedykowanym podsieci. W związku z tym nie Wybierz podsieć, w którym włączono domeny zarządzanej.

7. Na **zakupu** , przejrzyj ustawienia i kliknij przycisk **OK** można wdrożyć maszyny wirtualnej.
8. Wdrożenie maszyna wirtualna jest przypięta do pulpitu nawigacyjnego portalu Azure.

    ![Gotowe](./media/active-directory-domain-services-admin-guide/create-windows-vm-done.png)
9. Po zakończeniu wdrożenia wyświetlane są informacje dotyczące maszyny Wirtualnej w ramach **omówienie** strony.


## <a name="step-2-connect-to-the-windows-server-virtual-machine-using-the-local-administrator-account"></a>Krok 2: Łączenie z maszyną wirtualną systemu Windows Server przy użyciu konta administratora lokalnego
Teraz połączyć się z nowo utworzonego maszyny wirtualnej systemu Windows Server, aby przyłączyć się do domeny. Użyj poświadczeń administratora lokalnego, określone podczas tworzenia maszyny wirtualnej.

Wykonaj poniższe kroki, aby połączyć się z maszyną wirtualną.

1. Kliknij przycisk **Connect** znajdującego się na **omówienie** strony. Plik protokołu Remote Desktop Protocol (RDP) jest utworzony i pobrane.

    ![Połącz z maszyną wirtualną systemu Windows](./media/active-directory-domain-services-admin-guide/connect-windows-vm.png)
2. Aby połączyć się z maszyną wirtualną, otwórz pobrany plik RDP. Jeśli zostanie wyświetlony monit, kliknij przycisk **Połącz**.
3. W wierszu logowania wprowadź Twojej **poświadczenia administratora lokalnego**, która została określona podczas tworzenia maszyny wirtualnej. Na przykład możemy używano "localhost\mahesh" w tym przykładzie.
4. Podczas procesu logowania może pojawić się ostrzeżenie o certyfikacie. Kliknij przycisk Tak, lub Kontynuuj, aby nawiązać połączenie.

W tym momencie możesz powinny być rejestrowane nowo utworzonej maszyny wirtualnej systemu Windows przy użyciu poświadczeń administratora lokalnego. Następnym krokiem jest można dołączyć maszyny wirtualnej do domeny.


## <a name="step-3-join-the-windows-server-virtual-machine-to-the-aad-ds-managed-domain"></a>Krok 3: Sprzężenia maszyny wirtualnej systemu Windows Server do domeny zarządzanej DS usługi AAD
Wykonaj poniższe kroki, aby dołączyć maszyny wirtualnej systemu Windows Server do domeny zarządzanej usługi AAD DS.

1. Podłącz do systemu Windows Server, jak pokazano w kroku 2. Na ekranie startowym Otwórz **Menedżera serwera**.
2. Kliknij przycisk **lokalnego serwera** w lewym okienku okna Menedżera serwera.

    ![Uruchom Menedżera serwera na maszynie wirtualnej](./media/active-directory-domain-services-admin-guide/join-domain-server-manager.png)
3. Kliknij przycisk **grupy roboczej** w obszarze **właściwości** sekcji. W **właściwości systemu** strony właściwości, kliknij przycisk **zmiany** do przyłączania do domeny.

    ![Strona właściwości systemu](./media/active-directory-domain-services-admin-guide/join-domain-system-properties.png)
4. Określ nazwę domeny w domenie zarządzanej usług domenowych Azure AD w **domeny** pole tekstowe i kliknij przycisk **OK**.

    ![Określ domenę, która ma zostać umieszczony](./media/active-directory-domain-services-admin-guide/join-domain-system-properties-specify-domain.png)
5. Monit o podanie poświadczeń w celu przyłączenia do domeny. Upewnij się, że **Określ poświadczenia dla użytkownika należącego do administratorów kontrolera domeny usługi AAD** grupy. Tylko członkowie tej grupy mają uprawnienia do przyłączania komputerów do domeny zarządzanej.

    ![Określ poświadczenia dla przyłączania do domeny](./media/active-directory-domain-services-admin-guide/join-domain-system-properties-specify-credentials.png)
6. Możesz określić poświadczenia w jednym z następujących sposobów:

   * **Format nazwy UPN: (zalecane)** Określ sufiks nazwy UPN dla konta użytkownika, zgodnie z konfiguracją w usłudze Azure AD. W tym przykładzie sufiks nazwy UPN użytkownika "bob" jest "bob@domainservicespreview.onmicrosoft.com".
   * **SAMAccountName format:** można określić nazwę konta w formacie SAMAccountName. W tym przykładzie "bob" użytkownik musi wprowadzić "CONTOSO100\bob".

     > [!TIP]
     > **Zaleca się przy użyciu formatu nazwy UPN, aby określić poświadczenia.**
     > Jeśli prefiks nazwy UPN użytkownika jest zbyt długi (na przykład "joehasareallylongname"), SAMAccountName mogą być generowane automatycznie. Jeśli wielu użytkowników ma tego samego prefiksu nazwy UPN (na przykład "bob") w dzierżawie usługi Azure AD, ich format SAMAccountName mogą być automatycznie generowane przez usługę. W takich przypadkach format nazwy UPN można niezawodnie logować się do domeny.
     >

7. Po pomyślnym przyłączenie do domeny zostanie wyświetlony następujący komunikat powiadomieniem o przyjęciu do domeny. Uruchom ponownie maszynę wirtualną na zakończenie operacji sprzężenia domeny.

    ![Zapraszamy do domeny](./media/active-directory-domain-services-admin-guide/join-domain-done.png)


## <a name="troubleshooting-domain-join"></a>Rozwiązywanie problemów z przyłączania do domeny
### <a name="connectivity-issues"></a>Problemy z łącznością
Jeśli maszyna wirtualna nie może znaleźć domeny, można skorzystać z wykonać następujące kroki:

* Upewnij się, że maszyna wirtualna jest podłączony do tej samej sieci wirtualnej co włączeniu usług domenowych w. Jeśli nie, maszyny wirtualnej nie może połączyć się z domeną i dlatego nie można przyłączyć do domeny.
* Upewnij się, że maszyna wirtualna znajduje się w sieci wirtualnej, która z kolei jest podłączony do sieci wirtualnej, w której włączono usługi domenowe.
* Spróbuj wykonać polecenie ping domeny przy użyciu nazwy domeny zarządzanej domeny (na przykład "ping contoso100.com"). Jeśli to możliwe, spróbuj na polecenie ping adresów IP dla domeny wyświetlany na stronie w którym włączono usługi domenowe Azure AD (na przykład "ping 10.0.0.4"). Jeśli jesteś w stanie zbadać poleceniem ping adres IP, ale nie domenę, DNS może być niepoprawnie skonfigurowana. Sprawdź, czy adresy IP domeny są skonfigurowane jako serwery DNS dla sieci wirtualnej.
* Spróbuj opróżniania pamięci podręcznej programu rozpoznawania nazw DNS na maszynie wirtualnej ("ipconfig/flushdns").

Jeśli pojawi się okno dialogowe z monitem o podanie poświadczeń w celu przyłączenia do domeny, nie masz problemy z połączeniem.

### <a name="credentials-related-issues"></a>Problemy związane z poświadczeń
Zapoznaj się następujące kroki, jeśli występują problemy przy użyciu poświadczeń i nie można przyłączyć do domeny.

* Spróbuj użyć format nazwy UPN, aby określić poświadczenia. W przypadku wielu użytkowników z tego samego prefiksu nazwy UPN w dzierżawie lub prefiks z głównej nazwy użytkownika jest zbyt długi, SAMAccountName dla Twojego konta może być generowane automatycznie. W związku z tym format SAMAccountName dla Twojego konta mogą być inne niż co oczekiwane lub użyć w domenie lokalnej.
* Spróbuj użyć poświadczenia konta użytkownika należącego do grupy "Administratorzy usługi AAD kontrolera domeny".
* Upewnij się, że masz [włączoną synchronizację haseł](active-directory-ds-getting-started-password-sync.md) zgodnie z procedurą opisaną w przewodniku Wprowadzenie.
* Upewnij się, że używasz nazwy UPN użytkownika zgodnie z konfiguracją w usłudze Azure AD (na przykład "bob@domainservicespreview.onmicrosoft.com") do logowania.
* Upewnij się, że upłynął czas potrzebny wystarczająco długo na zakończenie określone w przewodniku wprowadzenie synchronizacji haseł.

## <a name="related-content"></a>Powiązana zawartość
* [Usługi domenowe AD Azure - Przewodnik wprowadzający](active-directory-ds-getting-started.md)
* [Administrowanie domeną zarządzaną usług Azure AD Domain Services](active-directory-ds-admin-guide-administer-domain.md)
