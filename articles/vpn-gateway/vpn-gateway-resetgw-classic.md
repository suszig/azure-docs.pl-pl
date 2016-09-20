<properties
   pageTitle="Resetowanie bramy sieci VPN Azure | Microsoft Azure"
   description="W tym artykule opisano procedurę resetowania bramy sieci VPN Azure. Artykuł ma zastosowanie do bram sieci VPN utworzonych przy użyciu klasycznego modelu wdrażania."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/16/2016"
   ms.author="cherylmc"/>

# Resetowanie bramy sieci VPN Azure przy użyciu programu PowerShell


W tym artykule opisano procedurę resetowania bramy sieci VPN platformy Azure przy użyciu poleceń cmdlet programu PowerShell. Instrukcje te mają zastosowanie do klasycznego modelu wdrażania. Obecnie nie można zresetować bram sieci wirtualnej, które są tworzone w modelu wdrażania usługi Resource Manager.

Resetowanie bramy sieci VPN Azure przydaje się w przypadku utraty połączenia sieci VPN obejmującego wiele lokalizacji w jednym lub wielu tunelach VPN S2S. W takiej sytuacji urządzenia lokalnej sieci VPN działają prawidłowo, ale nie mogą nawiązać połączenia w ramach tuneli używających protokołu IPsec z bramami sieci VPN Azure. W przypadku użycia polecenia cmdlet *Reset-AzureVNetGateway* nastąpi ponowne uruchomienie bramy i ponowne zastosowanie do niej konfiguracji obejmujących wiele lokalizacji. Brama zachowa publiczny adres IP, który został z nią wcześniej powiązany. Oznacza to, że nie będzie konieczne aktualizowanie konfiguracji routera sieci VPN pod kątem nowego publicznego adresu IP bramy sieci VPN Azure.  


Przed zresetowaniem bramy dla każdego tunelu połączenia sieci VPN typu lokacja-lokacja (site-to site, S2S) korzystającego z protokołu IPsec należy sprawdzić kluczowe elementy wymienione poniżej. Brak zgodności którychkolwiek elementów spowoduje przerwanie połączenia tuneli sieci VPN S2S. Zweryfikowanie i poprawienie konfiguracji bram sieci lokalnej i sieci VPN Azure pozwoli uniknąć niepotrzebnego ponownego uruchamiania i zakłóceń funkcjonowania innych połączeń działających w powiązaniu z bramami.

Przed zresetowaniem bramy sprawdź następujące elementy.

- Wirtualne adresy IP (VIP) bramy sieci VPN Azure i bramy lokalnej sieci VPN powinny być prawidłowo skonfigurowane zarówno w zasadach platformy Azure, jak i zasadach lokalnej sieci VPN.
- Klucz wstępny musi być taki sam w przypadku bramy sieci VPN Azure oraz bramy lokalnej sieci VPN.
- W przypadku zastosowania konkretnej konfiguracji uwzględniającej protokół IPsec/IKE, takiej jak szyfrowanie, algorytmy wyznaczania wartości skrótu i doskonałe utajnienie przekazywania (Perfect Forward Secrecy, PFS), należy się upewnić, że zarówno brama sieci VPN Azure, jak i brama lokalnej sieci VPN mają takie same konfiguracje.


## Resetowanie bramy sieci VPN przy użyciu programu PowerShell

Polecenie cmdlet programu PowerShell do resetowania bramy sieci VPN Azure to *Reset-AzureVNetGateway*. Każda brama sieci VPN Azure składa się z dwóch wystąpień maszyny wirtualnej działających w konfiguracji aktywne-w gotowości. Po wydaniu polecenia bieżące aktywne wystąpienie bramy sieci VPN Azure zostanie natychmiast uruchomione ponownie. Podczas pracy w trybie failover następuje krótka przerwa w działaniu aktywnego wystąpienia (które jest ponownie uruchamiane) na rzecz wystąpienia w konfiguracji gotowości. Przerwa powinna trwać niż dłużej niż minutę. 

Poniższy przykład pozwala zresetować bramę sieci VPN platformy Azure sieci wirtualnej o nazwie „ContosoVNet”.
 
        Reset-AzureVNetGateway –VnetName “ContosoVNet” 

        Error          :
        HttpStatusCode : OK
        Id             : f1600632-c819-4b2f-ac0e-f4126bec1ff8
        Status         : Successful
        RequestId      : 9ca273de2c4d01e986480ce1ffa4d6d9
        StatusCode     : OK


Jeśli połączenie nie zostanie przywrócone po pierwszym ponownym rozruchu komputera, należy wydać to polecenie ponownie, aby uruchomić ponownie drugie wystąpienie maszyny wirtualnej (nową aktywną bramę). Jeśli wymagane jest przeprowadzenie dwóch rozruchów jednocześnie, to ponowne uruchomienie obu wystąpień maszyn wirtualnych (aktywnego i w gotowości) będzie trwać nieco dłużej. Spowoduje to dłuższą przerwę w łączności przez sieć VPN, trwającą maksymalnie od 2 do 4 minut, podczas której nastąpi ponowny rozruch maszyn wirtualnych.

Jeśli po dwóch ponownych uruchomieniach nadal występują problemy z łącznością obejmującą wiele lokalizacji, należy otworzyć bilet pomocy technicznej z poziomu klasycznego portalu Azure w celu nawiązania kontaktu z działem pomocy technicznej Microsoft Azure.

## Następne kroki
    
Więcej informacji na temat tego polecenia cmdlet można znaleźć w artykule [PowerShell reference](https://msdn.microsoft.com/library/azure/mt270366.aspx) (Dokumentacja programu PowerShell).









<!--HONumber=sep16_HO1-->


