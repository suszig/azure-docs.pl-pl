---
title: "Za pomocą certyfikatów z pakiet integracyjny dla przedsiębiorstw | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak używać certyfikatów z pakiet integracyjny dla przedsiębiorstw | Aplikacje logiki platformy Azure"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: cgronlun
ms.assetid: 4cbffd85-fe8d-4dde-aa5b-24108a7caa7d
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/03/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: 0357e67a8920a57b2ab8b79ebd8dd3a64d888478
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/21/2018
---
# <a name="learn-about-certificates-and-enterprise-integration-pack"></a>Informacje na temat certyfikatów i pakietu integracyjnego dla przedsiębiorstw
## <a name="overview"></a>Przegląd
Integracja przedsiębiorstwa używa certyfikatów do zabezpieczenia komunikacji B2B. Można użyć dwóch typów certyfikatów, w aplikacjach integracji przedsiębiorstwa:

* Certyfikaty publiczne, które muszą być zakupione od urzędu certyfikacji (CA).
* Certyfikaty prywatne, które można wystawić samodzielnie. Te certyfikaty są czasami określane jako certyfikaty z podpisem własnym.

## <a name="what-are-certificates"></a>Co to są certyfikaty?
Certyfikaty są cyfrowe dokumenty, który sprawdzić tożsamość uczestników łączności elektronicznej i który również bezpieczna komunikacja elektronicznych.

## <a name="why-use-certificates"></a>Dlaczego warto używać certyfikatów?
Czasami komunikacji B2B musi poufne. Integracja przedsiębiorstwa używa certyfikatów do zabezpieczenia tej komunikacji na dwa sposoby:

* Zawartość wiadomości są szyfrowane
* Przez cyfrowego podpisywania wiadomości  

## <a name="upload-a-public-certificate"></a>Przekaż certyfikat publiczny

Aby użyć *certyfikatu publicznego* w aplikacjach logiki z możliwościami B2B, należy najpierw przekazać certyfikat do swojego konta integracji.  

Przekaż certyfikat, jest dostępny do zabezpieczenia wiadomości B2B podczas definiowania ich właściwości [umowy](logic-apps-enterprise-integration-agreements.md) utworzony.  

Poniżej przedstawiono szczegółowe informacje na temat przekazywania certyfikaty publiczne na koncie integracji, po zalogowaniu się do portalu Azure:

1. Wybierz **wszystkie usługi** , a następnie wprowadź **integracji** w polu filtru wyszukiwania. Wybierz **konta integracji** z listy wyników     
![Wybierz opcję Przeglądaj](media/logic-apps-enterprise-integration-certificates/overview-1.png)  
2. Wybierz konto integracji, do której chcesz dodać certyfikat.  
![Wybierz konto integracji, do której chcesz dodać certyfikat](media/logic-apps-enterprise-integration-certificates/overview-3.png)  
3. Wybierz **certyfikaty** kafelka.  
![Wybierz Kafelek certyfikatów](media/logic-apps-enterprise-integration-certificates/certificate-1.png)
4. W **certyfikaty** bloku, który zostanie otwarty, wybierz **Dodaj** przycisku.   
![Kliknij przycisk Dodaj](media/logic-apps-enterprise-integration-certificates/certificate-2.png)
5. Wprowadź **nazwa** dla Twojego certyfikatu, a następnie wybierz certyfikat wpisać jako **publicznego** z listy rozwijanej.  
6. Wybierz ikonę folderu w prawej części **certyfikatu** pola tekstowego. Po otwarciu selektor plików, Znajdź i wybierz plik certyfikatu, który chcesz przekazać do konta integracji.
7. Wybierz certyfikat, a następnie wybierz **OK** w selektora plików. Weryfikuje i wysyła certyfikat do swojego konta integracji.
8. Na koniec kopii na **Dodaj certyfikat** bloku, wybierz opcję **OK** przycisku.  
![Kliknij przycisk OK](media/logic-apps-enterprise-integration-certificates/certificate-3.png)  
9. Wybierz **certyfikaty** kafelka. Powinny pojawić się nowo dodany certyfikat.  
![Zobacz nowego certyfikatu](media/logic-apps-enterprise-integration-certificates/certificate-4.png)  

## <a name="upload-a-private-certificate"></a>Przekaż certyfikat prywatny

Aby użyć *certyfikatu prywatnego* w aplikacjach logiki z możliwościami B2B, można przekazać certyfikatu prywatnego do konta integracji, wykonując następujące czynności

1. [Przekazać klucz prywatny do magazynu klucz](../key-vault/key-vault-get-started.md "więcej informacji na temat usługi Key Vault") i podaj **nazwa klucza** 
   
   > [!TIP]
   > Musisz zezwolić aplikacji logiki, aby wykonywać operacje na magazyn kluczy. Można przyznać dostęp do nazwy głównej usługi Logic Apps za pomocą następującego polecenia programu PowerShell: `Set-AzureRmKeyVaultAccessPolicy -VaultName 'TestcertKeyVault' -ServicePrincipalName '7cd684f4-8a78-49b0-91ec-6a35d38739ba' -PermissionsToKeys decrypt, sign, get, list`  
   > 
   > 

Po poprzednim kroku, należy dodać prywatny certyfikatu do konta integracji.

Poniżej przedstawiono szczegółowe informacje na temat przekazywania prywatnej certyfikaty do konta integracji, po zalogowaniu się do portalu Azure:  
 
1. Wybierz konto integracji, do której chcesz dodać certyfikat, a następnie wybierz **certyfikaty** kafelka.  
![Wybierz Kafelek certyfikatów](media/logic-apps-enterprise-integration-certificates/certificate-1.png)  
2. W **certyfikaty** bloku, który zostanie otwarty, wybierz **Dodaj** przycisku.   
![Kliknij przycisk Dodaj](media/logic-apps-enterprise-integration-certificates/certificate-2.png)
3. Wprowadź **nazwa** dla certyfikatu i wybierz typ certyfikatu jako **prywatnej** z listy rozwijanej.   
4. Wybierz ikonę folderu w prawej części **certyfikatu** pola tekstowego. Po otwarciu selektora plików można znaleźć odpowiedniego certyfikatu publicznego, który chcesz przekazać do konta integracji.   
   
   > [!Note]
   > Podczas dodawania certyfikatu prywatnego ważne jest, aby dodać odpowiedni certyfikat publiczny do wyświetlenia w [umowy AS2](logic-apps-enterprise-integration-as2.md) odbierania i wysyłania ustawień podpisywania i szyfrowania wiadomości.
   > 
   >   

5. Wybierz **grupy zasobów**, **Key Vault**, **nazwa klucza** i wybierz **OK** przycisku.  
![Dodawanie certyfikatu](media/logic-apps-enterprise-integration-certificates/privatecertificate-1.png)  
6. Wybierz **certyfikaty** kafelka. Powinny pojawić się nowo dodany certyfikat.
![Zobacz nowego certyfikatu](media/logic-apps-enterprise-integration-certificates/privatecertificate-2.png)  



* [Tworzenie umowy z B2B](logic-apps-enterprise-integration-agreements.md)  
* [Dowiedz się więcej na temat usługi Key Vault](../key-vault/key-vault-get-started.md "więcej informacji na temat usługi Key Vault")  

