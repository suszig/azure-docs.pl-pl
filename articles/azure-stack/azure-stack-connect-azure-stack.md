<properties
    pageTitle="Łączenie z usługą Microsoft Azure Stack POC | Microsoft Azure"
    description="Dowiedz się, jak połączyć się z witryną Azure Stack POC Portal jako administrator usługi lub dzierżawca."
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/01/2016"
    ms.author="erikje"/>


# Logowanie do maszyny wirtualnej usługi Azure Stack POC

Możesz zalogować się do maszyny wirtualnej usługi Azure Stack POC jako

- [**administrator usługi**](#log-in-as-a-service-administrator), aby zarządzać dostawcami zasobów, ofertami dzierżawy, planami, usługami, limitami przydziałów i cenami;

lub

- [**dzierżawca**](#log-in-as-a-tenant), aby aprowizować i monitorować subskrybowane usługi, takie jak usługa Web Apps, magazyn i maszyny wirtualne, oraz zarządzać nimi.

## Logowanie jako administrator usługi

1.  Zaloguj się do maszyny fizycznej usługi Azure Stack POC.

2.  Kliknij dwukrotnie ikonę pulpitu **ClientVM.AzureStack.local.rdp**, aby otworzyć usługę Podłączanie pulpitu zdalnego dla maszyny wirtualnej klienta.
 
    ![](media/azure-stack-connect-azure-stack/clientvmazurestacklocalicon.png)
    
    Automatycznie używa ona konta AzureStack\\AzureStackUser, które zostało utworzone przez skrypt wdrożenia. Użyj hasła administratora podanego w kroku 5 procesu skryptu w wierszu monitu **Enter the password for the built-in administrator** (Wprowadź hasło administratora kompilacji).

3.  Na pulpicie ClientVM.AzureStack.local kliknij dwukrotnie ikonę **Microsoft Azure Stack POC Portal** (https://portal.azurestack.local/), aby otworzyć [portal](azure-stack-key-features.md#portal).

    ![](media/azure-stack-connect-azure-stack/microsoftazurestackpocprtalicon.png)

4.  Zaloguj się przy użyciu konta administratora usługi.

## Logowanie jako dzierżawca

Administrator usługi może zalogować się jako dzierżawca, aby przetestować plany, oferty i subskrypcje, z których mogą korzystać dzierżawcy.
Przed zalogowaniem się należy [utworzyć konto dzierżawcy](azure-stack-add-new-user-aad.md), jeśli jeszcze go nie ma.

1.  Zaloguj się do maszyny fizycznej usługi Azure Stack.

2.  Kliknij dwukrotnie ikonę pulpitu **ClientVM.AzureStack.local.rdp**, aby otworzyć usługę Podłączanie pulpitu zdalnego dla maszyny wirtualnej klienta. 

    ![](media/azure-stack-connect-azure-stack/clientvmazurestacklocalicon.png)

    Automatycznie używa ona konta AzureStack\\AzureStackUser, które zostało utworzone przez skrypt wdrożenia. Użyj hasła administratora podanego w kroku 5 procesu skryptu w wierszu monitu **Enter the password for the built-in administrator** (Wprowadź hasło administratora kompilacji).

3.  Na pulpicie ClientVM.AzureStack.local kliknij dwukrotnie ikonę **Microsoft Azure Stack POC Portal** (https://portal.azurestack.local/), aby otworzyć [portal](azure-stack-key-features.md#portal).

    ![](media/azure-stack-connect-azure-stack/microsoftazurestackpocprtalicon.png)

4.  Zaloguj się przy użyciu konta dzierżawcy.

Protokół PRD może ograniczać liczbę użytkowników, którzy mogą uzyskiwać dostęp do fizycznego hosta usługi Microsoft Azure POC. Aby włączyć wielu użytkowników, zobacz [Włączanie wielu równoczesnych połączeń użytkowników](azure-stack-enable-multiple-concurrent-users.md).

## Następne kroki

[Pierwsze zadania](azure-stack-first-scenarios.md)



<!--HONumber=Sep16_HO3-->


