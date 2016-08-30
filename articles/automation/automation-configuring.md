<properties
   pageTitle="Konfigurowanie usługi Azure Automation"
   description="Opis czynności, które należy wykonać, aby skonfigurować usługę Azure Automation do początkowego użycia."
   services="automation"
   documentationCenter=""
   authors="MGoedtel"
   manager="stevenka"
   editor="tysonn" />
<tags
   ms.service="automation"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/23/2016"
   ms.author="magoedte;bwren" />

# Konfigurowanie usługi Azure Automation

W tym artykule opisano czynności, które należy wykonać, aby rozpocząć korzystanie z usługi Azure Automation.

## Konta usługi Automation

Kiedy uruchamiasz usługę Azure Automation po raz pierwszy, musisz utworzyć co najmniej jedno konto usługi Automation. Konta usługi Automation pozwalają na izolowanie zasobów usługi Automation (elementów Runbook, zasobów, konfiguracji) znajdujących się na różnych kontach usługi Automation. Konta usługi Automation mogą służyć do rozdzielenia zasobów usługi Automation na oddzielne środowiska logiczne. Na przykład jednego konta można używać do tworzenia oprogramowania, a drugiego w środowisku produkcyjnym.

Zasoby usługi Automation na poszczególnych kontach są skojarzone z pojedynczym regionem platformy Azure, ale za pomocą kont usługi Automation można zarządzać usługami w dowolnym regionie. Głównym powodem tworzenia kont usługi Automation w różnych regionach jest istnienie zasad, które wymagają, aby dane i zasoby były izolowane w określonym regionie.

>[AZURE.NOTE] Utworzone w portalu Azure konta usługi Automation i zawarte w nich zasoby nie są dostępne w klasycznym portalu Azure. Aby zarządzać tymi kontami lub ich zasobami za pomocą programu Windows PowerShell, należy skorzystać z modułów usługi Azure Resource Manager. 
>
>Kontami usługi Automation utworzonymi w klasycznym portalu Azure można zarządzać zarówno w portalu, jak i za pomocą zestawu poleceń cmdlet. Po utworzeniu konta nie ma znaczenia, jak są tworzone zasoby w ramach konta ani jak się nimi zarządza. Jeśli planujesz dalsze używanie klasycznego portalu Azure, użyj go zamiast portalu Azure do utworzenia kont usługi Automation.


W razie problemów z kontem platformy Azure (np. zaległych płatności) konto usługi Automation może zostać zawieszone. Nie będziesz mieć wtedy dostępu do konta, działające zadania zostaną wstrzymane, a wszystkie harmonogramy zostaną wyłączone. Będzie można przeglądać konto, ale zawarte na nim zasoby nie będą widoczne. Po rozwiązaniu problemu i włączeniu konta usługi Automation, należy włączyć harmonogramy i uruchomić ponownie wszystkie elementy Runbook, które zostały wstrzymane.


## Konfigurowanie uwierzytelniania do zasobów platformy Azure

Jeśli uzyskujesz dostęp do zasobów platformy Azure przy użyciu [poleceń cmdlet platformy Azure](http://msdn.microsoft.com/library/azure/jj554330.aspx), musisz dodać uwierzytelnianie do swojej subskrypcji platformy Azure. W usłudze Azure Automation służy do tego konto organizacyjne w usłudze Azure Active Directory, które konfigurujesz jako administrator dla swojej subskrypcji. Następnie możesz utworzyć [poświadczenia](http://msdn.microsoft.com/library/dn940015.aspx) dla tego konta użytkownika, a następnie używać ich z poleceniem cmdlet [Add-AzureAccount](http://msdn.microsoft.com/library/azure/dn722528.aspx) w elemencie Runbook.

>[AZURE.NOTE] W usłudze Azure Automation nie można używać kont Microsoft nazywanych wcześniej identyfikatorami LiveID.

## Tworzenie nowego użytkownika usługi Azure Active Directory w celu zarządzania subskrypcją platformy Azure

1. Zaloguj się do klasycznego portalu Azure jako administrator usługi dla subskrypcji platformy Azure, którą chcesz zarządzać.
2. Wybierz opcję **Active Directory**
3. Wybierz nazwę katalogu, który jest skojarzony z subskrypcją platformy Azure. W razie potrzeby możesz zmienić to skojarzenie w menu **Ustawienia > Subskrypcje > Edytuj katalog**.
4. [Utwórz nowego użytkownika usługi Active Directory](http://msdn.microsoft.com/library/azure/hh967632.aspx).  Jako **typ użytkownika** wybierz **nowego użytkownika w swojej organizacji** i nie zaznaczaj opcji **Włącz usługę Multi-Factor Authentication**.
5. Zapamiętaj pełną nazwę użytkownika i tymczasowe hasło.
7. Wybierz kolejno opcje **Ustawienia > Administratorzy > Dodaj**.
8. Wpisz pełną nazwę utworzonego przed chwilą użytkownika.
9. Wybierz subskrypcję, którą ma zarządzać ten użytkownik.
10. Wyloguj się z platformy Azure, a następnie zaloguj się ponownie przy użyciu właśnie utworzonego konta. Pojawi się monit o zmianę hasła użytkownika.
11. Utwórz nowy [element zawartości poświadczeń usługi Azure Automation](automation-credentials.md) dla konta użytkownika, które zostało utworzone.  **Typem poświadczeń** powinny być **Poświadczenia programu Windows PowerShell**.

## Tworzenie konta automatyzacji

Konto automatyzacji jest kontenerem dla zasobów usługi Azure Automation. Umożliwia rozdzielenie środowisk lub dokładniejsze uporządkowanie przepływów pracy. Jeśli masz już konto automatyzacji, możesz pominąć ten krok.

1. Zaloguj się do [portalu Azure](https://portal.azure.com/).

2. Kliknij kolejno opcje **Nowy** > **Zarządzanie** > **Konto automatyzacji**

3. W bloku **Dodawanie konta Automatyzacji** skonfiguruj szczegółowe informacje o koncie usługi Automation. 

>[AZURE.NOTE] Po utworzeniu konta usługi Automation w portalu Azure konto i wszystkie skojarzone z nim zasoby nie są zwracane do klasycznego portalu Azure. 

Poniżej znajduje się lista parametrów do skonfigurowania:

|Parametr            |Opis |
|:---|:---|
| Nazwa | Nazwa konta usługi Automation. Musi mieć unikatową wartość. |
| Grupa zasobów | Grupy zasobów ułatwiają wyświetlanie powiązanych zasobów platformy Azure i zarządzanie nimi. W portalu Azure możesz wybrać istniejącą grupę zasobów lub utworzyć nową dla konta usługi Automation. Natomiast w klasycznym portalu Azure wszystkie konta usługi Automation zostają umieszczone w domyślnej grupie zasobów. |
| Subskrypcja | Wybierz subskrypcję z listy dostępnych subskrypcji. |
| Region | Region określa miejsce przechowywania zasobów usługi Automation w ramach konta. Można wybrać dowolny region z listy — nie ma to wpływu na funkcjonalność konta, ale elementy Runbook mogą działać szybciej, jeśli region konta jest blisko miejsca przechowywania innych zasobów platformy Azure. |
| Opcje konta | Ta opcja umożliwia określenie, jakie zasoby zostaną utworzone na nowym koncie usługi Automation. Wybranie opcji **Tak** powoduje utworzenie elementu Runbook samouczka. |

![Tworzenie konta](media/automation-configuration/automation-01-create-automation-account.png)

>[AZURE.NOTE] Jeśli konto usługi Automation utworzone w klasycznym portalu Azure zostanie [przeniesione do innej grupy zasobów](../resource-group-move-resources.md) za pomocą portalu Azure, przestanie być dostępne w klasycznym portalu Azure.



## Używanie poświadczeń w elemencie Runbook

Można pobrać poświadczenie w elemencie Runbook za pomocą działania [Get-AutomationPSCredential](http://msdn.microsoft.com/library/dn940015.aspx), a następnie można go użyć za pomocą działania [Add-AzureAccount](http://msdn.microsoft.com/library/azure/dn722528.aspx) w celu nawiązania połączenia z subskrypcją platformy Azure. Jeśli poświadczenie odnosi się do administratora wielu subskrypcji Azure, należy też użyć polecenia [Select-AzureSubscription](http://msdn.microsoft.com/library/dn495203.aspx), aby wskazać odpowiednią subskrypcję. Jest to pokazane poniżej w przykładowym kodzie programu Windows PowerShell, który zwykle znajduje się w górnej części większości elementów Runbook usługi Azure Automation.

    $cred = Get-AutomationPSCredential –Name "myuseraccount.onmicrosoft.com"
    Add-AzureAccount –Credential $cred
    Select-AzureSubscription –SubscriptionName "My Subscription"

Te wiersze należy powtórzyć po każdym [punkcie kontrolnym](http://technet.microsoft.com/library/dn469257.aspx#bk_Checkpoints) w elemencie Runbook. Jeśli element Runbook został zawieszony, a następnie jego działanie zostało wznowione w ramach innego procesu roboczego, konieczne jest ponowienie uwierzytelniania.

## Pokrewne artykuły
- [Usługa Azure Automation: uwierzytelnianie na platformie Azure za pomocą usługi Azure Active Directory](https://azure.microsoft.com/blog/2014/08/27/azure-automation-authenticating-to-azure-using-azure-active-directory/)
 



<!--HONumber=jun16_HO2-->


