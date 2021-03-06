---
title: "Azure Active Directory Domain Services: Dołącz Maszynę wirtualną systemu Windows Server do domeny zarządzanej | Dokumentacja firmy Microsoft"
description: "Dołącz maszynę wirtualną systemu Windows Server do usługi Azure AD DS"
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
ms.openlocfilehash: 7b5c23f1f4b6180d8b664f1371ccfd8a075572e6
ms.sourcegitcommit: b32d6948033e7f85e3362e13347a664c0aaa04c1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/13/2018
---
# <a name="join-a-windows-server-virtual-machine-to-a-managed-domain"></a>Przyłączanie maszyny wirtualnej z systemem Windows Server do domeny zarządzanej
W tym artykule pokazano, jak wdrożyć maszynę wirtualną systemu Windows Server przy użyciu portalu Azure. Następnie widoczny jest sposób Dołącz maszynę wirtualną do domeny zarządzanej usługi Azure Active Directory Domain Services (Azure AD DS).

## <a name="step-1-create-a-windows-server-virtual-machine"></a>Krok 1: Tworzenie maszyny wirtualnej systemu Windows Server
Aby utworzyć maszynę wirtualną systemu Windows, który jest dołączony do sieci wirtualnej, w której włączono usługi Azure AD DS, wykonaj następujące czynności:

1. Zaloguj się w [Portalu Azure](http://portal.azure.com).
2. W górnej części okienka po lewej stronie, wybierz **nowy**.
3. Wybierz pozycję **Wystąpienia obliczeniowe**, a następnie wybierz pozycję **Windows Server 2016 Datacenter**.

    ![Łącze systemu Windows Server 2016 Datacenter](./media/active-directory-domain-services-admin-guide/create-windows-vm-select-image.png)

4. W **podstawy** okienka kreatora, skonfigurowania podstawowych ustawień dla maszyny wirtualnej.

    ![W okienku podstawy](./media/active-directory-domain-services-admin-guide/create-windows-vm-basics.png)

    > [!TIP]
    > Nazwa użytkownika i hasło, które należy wprowadzić w tym miejscu są konta administratora lokalnego, który służy do logowania się do maszyny wirtualnej. Wybierz silne hasło, aby chronić maszyny wirtualnej przed atakami siłowymi hasła. Nie należy wprowadzać poświadczenia konta użytkownika domeny, w tym miejscu.
    >

5. Wybierz **rozmiar** dla maszyny wirtualnej. Zaznacz, aby wyświetlić więcej rozmiary **Wyświetl wszystkie** lub zmień **obsługiwany typ dysku** filtru.

    ![W okienku "Wybierz rozmiar"](./media/active-directory-domain-services-admin-guide/create-windows-vm-size.png)

6. W **ustawienia** okienku, wybierz sieć wirtualną, w której jest wdrażane domeny usługi Azure AD DS, zarządzane. Wybierz niż domeny zarządzanej jest wdrażana w innej podsieci. Dla innych ustawień, Zachowaj wartości domyślne, a następnie wybierz **OK**.

    ![Ustawienia sieci wirtualnej dla maszyny wirtualnej](./media/active-directory-domain-services-admin-guide/create-windows-vm-select-vnet.png)

    > [!TIP]
    > **Wybierz właściwą sieć i podsieci.**
    >
    > Wybierz albo sieć wirtualną, w której jest wdrażane domeny zarządzanej lub sieci wirtualnej podłączonej do niej przy użyciu wirtualnej sieci komunikacji równorzędnej. W przypadku wybrania odłączony sieci wirtualnej maszyny wirtualnej nie można przyłączyć do domeny zarządzanej.
    >
    > Firma Microsoft zaleca wdrażania domeny zarządzanej w dedykowanym podsieci. W związku z tym nie Wybierz podsieć, w którym włączono domeny zarządzanej.

7. Dla innych ustawień, Zachowaj wartości domyślne, a następnie wybierz **OK**.
8. Na **zakupu** , przejrzyj ustawienia, a następnie wybierz **OK** można wdrożyć maszyny wirtualnej.
9. Wdrożenie maszyna wirtualna jest przypięta do pulpitu nawigacyjnego portalu Azure.

    ![Gotowe](./media/active-directory-domain-services-admin-guide/create-windows-vm-done.png)
10. Po zakończeniu wdrożenia można przeglądać informacje o Maszynie wirtualnej na **omówienie** strony.


## <a name="step-2-connect-to-the-windows-server-virtual-machine-by-using-the-local-administrator-account"></a>Krok 2: Łączenie z maszyną wirtualną systemu Windows Server przy użyciu konta administratora lokalnego
Następnie należy połączyć się z nowo utworzony maszyny wirtualnej systemu Windows Server, aby przyłączyć się do domeny. Użyj poświadczeń administratora lokalnego określony podczas tworzenia maszyny wirtualnej.

Aby nawiązać połączenie z maszyną wirtualną, wykonaj następujące czynności:

1. W **omówienie** okienku wybierz **Connect**.  
    Plik protokołu Remote Desktop Protocol (RDP) jest utworzony i pobrane.

    ![Połącz z maszyną wirtualną systemu Windows](./media/active-directory-domain-services-admin-guide/connect-windows-vm.png)

2. Aby połączyć się z maszyną wirtualną, otwórz pobrany plik RDP. Jeśli zostanie wyświetlony monit, wybierz **Connect**.
3. Przy logowaniu, wprowadź z **poświadczenia administratora lokalnego**, która została określona podczas tworzenia maszyny wirtualnej (na przykład *localhost\mahesh*).
4. Jeśli zostanie wyświetlone ostrzeżenie o certyfikacie podczas procesu logowania, nawiązać połączenie, wybierając **tak** lub **Kontynuuj**.

W tym momencie powinna być zalogowania na nowo utworzony maszyny wirtualnej systemu Windows przy użyciu poświadczeń administratora lokalnego. Następnym krokiem jest można dołączyć maszyny wirtualnej do domeny.


## <a name="step-3-join-the-windows-server-virtual-machine-to-the-azure-ad-ds-managed-domain"></a>Krok 3: Dołącz maszynę wirtualną systemu Windows Server do usługi Azure AD DS zarządzanych domeny
Aby dołączyć maszynę wirtualną systemu Windows Server do domeny usługi Azure AD DS, zarządzane, wykonaj następujące czynności:

1. Połączenie z maszyną Wirtualną serwera systemu Windows, jak pokazano w "Krok 2." Na **Start** ekranu, otwórz **Menedżera serwera**.
2. W lewym okienku **Menedżera serwera** wybierz **lokalnego serwera**.

    ![Okno Menedżera serwera na maszynie wirtualnej](./media/active-directory-domain-services-admin-guide/join-domain-server-manager.png)

3. W obszarze **właściwości**, wybierz pozycję **grupy roboczej**. 
4. W **właściwości systemu** wybierz **zmiany** do przyłączania do domeny.

    ![Okno właściwości systemu](./media/active-directory-domain-services-admin-guide/join-domain-system-properties.png)

5. W **domeny** , określ nazwę domeny usługi Azure AD DS, zarządzane, a następnie wybierz **OK**.

    ![Określ domenę, która ma zostać umieszczony](./media/active-directory-domain-services-admin-guide/join-domain-system-properties-specify-domain.png)

6. Monit o podanie poświadczeń w celu przyłączenia do domeny. Upewnij się, że podajesz poświadczenia *użytkownika, który należy do grupy administratorów usługi Azure AD kontrolera domeny*. Tylko członkowie tej grupy mają uprawnienia do przyłączania komputerów do domeny zarządzanej.

    ![Określanie poświadczeń w oknie zabezpieczenia systemu Windows](./media/active-directory-domain-services-admin-guide/join-domain-system-properties-specify-credentials.png)

7. Możesz określić poświadczenia w jednym z następujących sposobów:

   * **Format nazwy UPN**: Określ (zalecane) sufiks głównej nazwy użytkownika (UPN) dla konta użytkownika, zgodnie z konfiguracją w usłudze Azure AD. W tym przykładzie sufiks nazwy UPN użytkownika *bob* jest  *bob@domainservicespreview.onmicrosoft.com* .

   * **SAMAccountName format**: należy określić nazwę konta w formacie SAMAccountName. W tym przykładzie użytkownik *bob* należy wprowadzić *CONTOSO100\bob*.

     > [!TIP]
     > **Zaleca się przy użyciu formatu nazwy UPN, aby określić poświadczenia.**
     >
     > Jeśli prefiks nazwy UPN użytkownika jest zbyt długa (na przykład *joehasareallylongname*), SAMAccountName mogą być generowane automatycznie. Jeśli wielu użytkowników ma tego samego prefiksu nazwy UPN (na przykład *bob*) w dzierżawie usługi Azure AD, ich format SAMAccountName może zostać wygenerowany automatycznie przez usługę. W takich przypadkach format nazwy UPN można niezawodnie zalogować się do domeny.
     >

8. Po pomyślnym dołączeniu domeny następujący komunikat przyjęciu do domeny.

    ![Zapraszamy do domeny](./media/active-directory-domain-services-admin-guide/join-domain-done.png)

9. Aby wykonać dołączenie do domeny, uruchom ponownie maszynę wirtualną.

## <a name="troubleshoot-joining-a-domain"></a>Rozwiązywanie problemów z przyłączanie do domeny
### <a name="connectivity-issues"></a>Problemy z łącznością
Jeśli maszyna wirtualna nie może znaleźć domeny, wykonaj jedną lub więcej z następujących czynności:

* Upewnij się, że maszyna wirtualna jest połączona z tej samej sieci wirtualnej, która została włączona opcja Azure AD DS w. Jeśli nie jest połączony, maszyna wirtualna nie może połączyć się z domeną i nie może przyłączyć do domeny.

* Upewnij się, że maszyna wirtualna znajduje się w sieci wirtualnej, która z kolei jest podłączony do sieci wirtualnej, w której włączono usługi Azure AD DS.

* Spróbuj na polecenie ping do domeny przy użyciu nazwy domeny zarządzanej domeny (na przykład *polecenie ping contoso100.com*). Jeśli to możliwe, spróbuj na polecenie ping adresów IP dla domeny, który jest wyświetlany na stronie Jeśli włączona jest usługa Azure AD DS (na przykład *polecenie ping 10.0.0.4*). Jeśli jesteś w stanie zbadać poleceniem ping adres IP, ale nie domenę, DNS może być niepoprawnie skonfigurowana. Sprawdź, czy adresy IP domeny są skonfigurowane jako serwery DNS dla sieci wirtualnej.

* Spróbuj opróżniania pamięci podręcznej programu rozpoznawania nazw DNS na maszynie wirtualnej (*ipconfig/flushdns*).

Jeśli wyświetlane jest okno poprosi o podanie poświadczeń w celu przyłączenia do domeny, nie masz problemy z połączeniem.

### <a name="credentials-related-issues"></a>Problemy związane z poświadczeń
Jeśli występują problemy przy użyciu poświadczeń i nie można przyłączyć do domeny, wypróbuj co najmniej jeden z następujących czynności:

* Spróbuj użyć format nazwy UPN, aby określić poświadczenia. W przypadku wielu użytkowników z tego samego prefiksu nazwy UPN w dzierżawie lub prefiks z głównej nazwy użytkownika jest zbyt długi, SAMAccountName dla Twojego konta może być generowane automatycznie. W związku z tym format SAMAccountName dla Twojego konta mogą być inne niż co oczekiwane lub użyć w domenie lokalnej.

* Spróbuj użyć poświadczenia konta użytkownika, który należy do *Administratorzy kontrolera domeny usługi AAD* grupy.

* Upewnij się, że masz [włączony synchronizacja haseł](active-directory-ds-getting-started-password-sync.md) zgodnie z kroki opisane w przewodniku Rozpoczęto pobieranie.

* Upewnij się, że używasz nazwy UPN użytkownika zgodnie z konfiguracją w usłudze Azure AD (na przykład  *bob@domainservicespreview.onmicrosoft.com* ) do logowania.

* Upewnij się, że upłynął czas potrzebny czas wystarczający do synchronizacji haseł zostanie ukończona, jak określono w pobierania przewodnika.

## <a name="related-content"></a>Zawartość pokrewna
* [Azure AD DS Wprowadzenie — przewodnik](active-directory-ds-getting-started.md)
* [Administrowanie domeny usługi Azure AD DS, zarządzane](active-directory-ds-admin-guide-administer-domain.md)
