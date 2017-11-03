---
title: "Umożliwia dostęp do usługi Azure Resource Manager MSI maszyny Wirtualnej systemu Linux"
description: "Samouczek, który przeprowadzi Cię przez proces przy użyciu systemu Linux VM zarządzane usługi tożsamości (MSI) można uzyskać dostępu do usługi Azure Resource Manager."
services: active-directory
documentationcenter: 
author: bryanla
manager: mbaldwin
editor: bryanla
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/30/2017
ms.author: elkuzmen
ms.openlocfilehash: a2ccae76ff548847b331c7993013f22a098525ce
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/01/2017
---
# <a name="use-a-linux-vm-managed-service-identity-msi-to-access-azure-resource-manager"></a>Umożliwia dostęp do usługi Azure Resource Manager Linux VM zarządzane usługi tożsamości (MSI)

[!INCLUDE[preview-notice](../../includes/active-directory-msi-preview-notice.md)]

Ten samouczek pokazuje, jak włączyć zarządzane tożsamości usługi (MSI) dla maszyny wirtualnej systemu Linux, a następnie użyć tej tożsamości można uzyskać dostępu do interfejsu API Azure Resource Manager. Zarządzane tożsamości usługi automatycznie są zarządzane przez usługę Azure i umożliwiają uwierzytelnianie usług, które obsługują usługi Azure AD authentication bez konieczności wstawić poświadczeń do kodu. Omawiane kwestie:

> [!div class="checklist"]
> * Włącz MSI w maszynie wirtualnej systemu Linux 
> * Przyznać uprawnienia maszyny Wirtualnej do grupy zasobów w usłudze Azure Resource Manager 
> * Uzyskaj token dostępu przy użyciu tożsamości maszyny Wirtualnej i użyć go do wywołania usługi Azure Resource Manager 


Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="sign-in-to-azure"></a>Logowanie do platformy Azure

Zaloguj się do portalu Azure pod adresem [https://portal.azure.com](https://portal.azure.com).

## <a name="create-a-linux-virtual-machine-in-a-new-resource-group"></a>Utwórz maszynę wirtualną systemu Linux w nowej grupy zasobów

W tym samouczku utworzymy nową maszynę Wirtualną systemu Linux. Można również włączyć MSI na istniejącej maszyny Wirtualnej.

1. Kliknij przycisk **Nowy** znajdujący się w lewym górnym rogu witryny Azure Portal.
2. Wybierz pozycję **Wystąpienia obliczeniowe**, a następnie wybierz pozycję **Ubuntu Server 16.04 LTS**.
3. Wprowadź informacje o maszynie wirtualnej. Aby uzyskać **typ uwierzytelniania**, wybierz pozycję **klucz publiczny SSH** lub **hasło**. Utworzony poświadczeń umożliwia logowanie do maszyny Wirtualnej.

    ![Tekst alternatywny obrazu](media/msi-tutorial-linux-vm-access-arm/msi-linux-vm.png)

4. Wybierz **subskrypcji** dla maszyny wirtualnej na liście rozwijanej.
5. Aby wybrać nowy **grupy zasobów** chcesz maszyny wirtualnej utworzone w, wybierz **Utwórz nowy**. Po zakończeniu kliknij przycisk **OK**.
6. Wybierz rozmiar maszyny wirtualnej. Aby wyświetlić więcej rozmiary, wybierz **Wyświetl wszystkie** lub zmień filtr typu dysku obsługiwane. W bloku ustawień pozostaw ustawienia domyślne i kliknij przycisk **OK**.

## <a name="enable-msi-on-your-vm"></a>Włącz MSI na maszynie Wirtualnej

MSI maszyny wirtualnej umożliwia pobieranie tokenów dostępu z usługi Azure AD bez konieczności umieścić poświadczeń w kodzie. W obszarze obejmuje włączenie MSI wykonuje dwie czynności: instalacji rozszerzenia maszyny Wirtualnej MSI w maszynie Wirtualnej i umożliwia korzystanie z pliku MSI dla maszyny Wirtualnej.  

1. Wybierz **maszyny wirtualnej** chcesz włączyć MSI.
2. Na pasku nawigacyjnym po lewej stronie kliknij **konfiguracji**.
3. Zobaczysz **zarządzane tożsamość usługi**. Aby zarejestrować i włączyć MSI, wybierz **tak**, jeśli chcesz ją wyłączyć, wybierz opcję nie.
4. Upewnij się, możesz kliknąć przycisk **zapisać** Aby zapisać konfigurację.

    ![Tekst alternatywny obrazu](media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)

5. Jeśli chcesz sprawdzić, które rozszerzenia są na tym **maszyny Wirtualnej systemu Linux**, kliknij przycisk **rozszerzenia**. Po włączeniu MSI **ManagedIdentityExtensionforLinux** pojawią się na liście.

    ![Tekst alternatywny obrazu](media/msi-tutorial-linux-vm-access-arm/msi-extension-value.png)

## <a name="grant-your-vm-access-to-a-resource-group-in-azure-resource-manager"></a>Przyznać uprawnienia maszyny Wirtualnej do grupy zasobów w usłudze Azure Resource Manager 

Za pomocą Instalatora MSI kodu mogą uzyskiwać tokeny dostępu do uwierzytelniania do zasobów, które obsługują uwierzytelnianie usługi Azure AD. Interfejs API Menedżera zasobów Azure obsługuje uwierzytelnianie usługi Azure AD. Najpierw należy udzielić tej maszyny Wirtualnej tożsamości dostępu do zasobów w usłudze Azure Resource Manager, w tym przypadku grupę zasobów, w którym znajduje się maszyna wirtualna.  

1. Przejdź do karty dla **grup zasobów**.
2. Wybierz konkretnych **grupy zasobów** utworzony wcześniej.
3. Przejdź do **dostępu control(IAM)** w lewym panelu.
4. Kliknij, aby **Dodaj** nowe przypisanie roli dla maszyny Wirtualnej. Wybierz **roli** jako **czytnika**.
5. Na liście rozwijanej dalej **przypisany dostęp** zasobu **maszyny wirtualnej**.
6. Następnie sprawdź odpowiednie subskrypcji znajduje się w **subskrypcji** listy rozwijanej. I **grupy zasobów**, wybierz pozycję **wszystkich grup zasobów**.
7. Ponadto w **wybierz** wybierz maszynę wirtualną systemu Linux w listy rozwijanej i kliknij przycisk **zapisać**.

    ![Tekst alternatywny obrazu](media/msi-tutorial-linux-vm-access-arm/msi-permission-linux.png)

## <a name="get-an-access-token-using-the-vms-identity-and-use-it-to-call-resource-manager"></a>Uzyskaj token dostępu przy użyciu tożsamości maszyny Wirtualnej i użyć go do wywołania usługi Resource Manager 

Aby wykonać te kroki, konieczne będzie klienta SSH. Jeśli korzystasz z systemu Windows, możesz użyć klienta SSH w [podsystemu systemu Windows dla systemu Linux](https://msdn.microsoft.com/commandline/wsl/about). Jeśli potrzebujesz pomocy w konfigurowaniu kluczy klienta SSH, zobacz [sposobu użycia SSH kluczy systemu Windows Azure](../virtual-machines/linux/ssh-from-windows.md), lub [sposobu tworzenia i używania SSH publiczne i prywatne parę kluczy dla maszyn wirtualnych systemu Linux na platformie Azure](../virtual-machines/linux/mac-create-ssh-keys.md).

1. W portalu przejdź do maszyny Wirtualnej systemu Linux i w **omówienie**, kliknij przycisk **Connect**.  
2. **Połącz** do maszyny Wirtualnej przy użyciu klienta SSH wybranych przez użytkownika. 
3. W oknie terminalu przy użyciu programu CURL, Wyślij żądanie do lokalnego punktu końcowego MSI do Uzyskaj token dostępu usługi Azure Resource Manager.  
 
    ZWINIĘCIE żądanie tokenu dostępu jest niższa.  
    
    ```bash
    curl http://localhost:50342/oauth2/token --data "resource=https://management.azure.com/" -H Metadata:true   
    ```
    
    > [!NOTE]
    > Wartość parametru "zasobu" musi być dokładnego dopasowania dla oczekiwano przez usługę Azure AD.  W przypadku identyfikator zasobu usługi Resource Manager należy dołączyć końcowy ukośnik w identyfikatorze URI. 
    
    Odpowiedź zawiera token dostępu, musisz mieć dostęp do usługi Azure Resource Manager. 
    
    Odpowiedź:  

    ```bash
    {"access_token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6IkhIQnlLVS0wRHFBcU1aaDZaRlBkMlZXYU90ZyIsImtpZCI6IkhIQnlLVS0wRHFBcU1aaDZaRlBkMlZXYU90ZyJ9.eyJhdWQiOiJodHRwczovL21hbmFnZW1lbnQuYXp1cmUuY29tIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvNzJmOTg4YmYtODZmMS00MWFmLTkxYWItMmQ3Y2QwMTFkYjQ3LyIsImlhdCI6MTUwNDEyNjYyNywibmJmIjoxNTA0MTI2NjI3LCJleHAiOjE1MDQxMzA1MjcsImFpbyI6IlkyRmdZTGg2dENWSzRkSDlGWGtuZzgyQ21ZNVdBZ0E9IiwiYXBwaWQiOiI2ZjJmNmU2OS04MGExLTQ3NmEtOGRjZi1mOTgzZDZkMjUxYjgiLCJhcHBpZGFjciI6IjIiLCJpZHAiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83MmY5ODhiZi04NmYxLTQxYWYtOTFhYi0yZDdjZDAxMWRiNDcvIiwib2lkIjoiMTEyODJiZDgtMDNlMi00NGVhLTlmYjctZTQ1YjVmM2JmNzJlIiwic3ViIjoiMTEyODJiZDgtMDNlMi00NGVhLTlmYjctZTQ1YjVmM2JmNzJlIiwidGlkIjoiNzJmOTg4YmYtODZmMS00MWFmLTkxYWItMmQ3Y2QwMTFkYjQ3IiwidXRpIjoib0U5T3JVZFJMMHVKSEw4UFdvOEJBQSIsInZlciI6IjEuMCJ9.J6KS7b9kFgDkegJ-Vfff19LMnu3Cfps4dL2uNGucb5M76rgDM5f73VO-19wZSRhQPxWmZLETzN3SljnIMQMkYWncp79MVdBud_xqXYyLdQpGkNinpKVJhTo1j1dY27U_Cjl4yvvpBTrtH3OX9gG0GtQs7PBFTTLznqcH3JR9f-bTSEN4wUhalaIPHPciVDtJI9I24_vvMfVqxkXOo6gkL0mEPfpXZRLwrBNd607AzX0KVmLFrwA1vYJnCV-sSV8bwTh2t6CVEj240t0iyeVWVc2usJ0NY2rxPzKd_UckQ_zzrECG3kS4vuYePKz6GqNJFVzm2w2c61lX0-O1CwvQ9w","refresh_token":"","expires_in":"3599","expires_on":"1504130527","not_before":"1504126627","resource":"https://management.azure.com","token_type":"Bearer"} 
    ```
    
    Ten token dostępu umożliwia dostęp do usługi Azure Resource Manager, na przykład aby przeczytać szczegółowe informacje o grupie zasobów, do której wcześniej przyznano dostęp do tej maszyny Wirtualnej. Zastąp wartości \<identyfikator SUBSKRYPCJI\>, \<grupy zasobów\>, i \<TOKEN dostępu\> z tymi utworzony wcześniej. 
    
    > [!NOTE]
    > Adres URL jest rozróżniana wielkość liter, dlatego upewnij się, jeśli używasz tego samego dokładnej zgodnie z wcześniej używane podczas nosi nazwę grupy zasobów i wielkie litery "G" w "w grupie zasobów".  
    
    ```bash 
    curl https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>?api-version=2016-09-01 -H "Authorization: Bearer <ACCESS TOKEN>" 
    ```
    
    Odpowiedź z powrotem określone informacje o grupie zasobów: 
     
    ```bash
    {"id":"/subscriptions/98f51385-2edc-4b79-bed9-7718de4cb861/resourceGroups/DevTest","name":"DevTest","location":"westus","properties":{"provisioningState":"Succeeded"}} 
    ```
     
## <a name="related-content"></a>Zawartość pokrewna

- Omówienie MSI, zobacz [omówienie zarządzane tożsamość usługi](../active-directory/msi-overview.md).

W poniższej sekcji komentarzy umożliwia wyrazić swoją opinię i pomóc nam dostosować i kształtu zawartość.

