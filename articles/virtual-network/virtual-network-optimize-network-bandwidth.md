---
title: "Zoptymalizować przepływność sieci maszyny Wirtualnej | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak zoptymalizować przepływność sieci maszyny wirtualnej platformy Azure."
services: virtual-network
documentationcenter: na
author: steveesp
manager: Gerald DeGrace
editor: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/24/2017
ms.author: steveesp
ms.openlocfilehash: 914747983d4d974810836be66d6c6af343f58b60
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="optimize-network-throughput-for-azure-virtual-machines"></a>Zoptymalizować przepływność sieci maszyn wirtualnych platformy Azure

Maszyny wirtualne platformy Azure (VM) ma domyślne ustawienia sieci, które mogą być optymalizowane więcej przepustowości sieci. W tym artykule opisano, jak zoptymalizować przepływność sieci systemu Microsoft Azure Windows oraz maszyn wirtualnych systemu Linux, łącznie z głównych dystrybucje, takich jak Ubuntu i CentOS Red Hat.

## <a name="windows-vm"></a>Maszyna wirtualna z systemem Windows

Jeśli maszyna wirtualna systemu Windows jest obsługiwana z [przyspieszony sieci](virtual-network-create-vm-accelerated-networking.md), włączenie tej funkcji może być konfiguracją optymalną przepustowości. Dla wszystkich innych maszyn wirtualnych systemu Windows przy użyciu skalowanie po stronie odbierającej (RSS) mogą skorzystać z wyższej przepustowości maksymalnej niż Maszynę wirtualną bez RSS. Funkcja RSS mogą być wyłączone domyślnie na maszynie wirtualnej systemu Windows. Wykonaj poniższe kroki, aby ustalić, czy włączono funkcję RSS i ją włączyć, jeśli jest wyłączone.

1. Wprowadź `Get-NetAdapterRss` polecenia programu PowerShell, aby zobaczyć, czy funkcja RSS jest włączone dla karty sieciowej. W poniższym przykładzie danych wyjściowych zwrócony z `Get-NetAdapterRss`, funkcja RSS nie jest włączona.

    ```powershell
    Name                    : Ethernet
    InterfaceDescription    : Microsoft Hyper-V Network Adapter
    Enabled              : False
    ```
2. Wprowadź następujące polecenie, aby włączyć funkcję RSS:

    ```powershell
    Get-NetAdapter | % {Enable-NetAdapterRss -Name $_.Name}
    ```
    Poprzednie polecenie nie ma danych wyjściowych. Polecenie zmienić ustawienia karty Sieciowej, powodując utracie połączenia tymczasowego około jednej minuty. Okno dialogowe Reconnecting pojawia się podczas utraty połączenia. Po próbie trzeci zwykle przywróceniu łączności.
3. Potwierdź, że funkcja RSS jest włączona na maszynie wirtualnej, wprowadzając `Get-NetAdapterRss` polecenie ponownie. W przypadku powodzenia zwrócono następujący przykład danych wyjściowych:

    ```powershell
    Name                    :Ethernet
    InterfaceDescription    : Microsoft Hyper-V Network Adapter
    Enabled              : True
    ```

## <a name="linux-vm"></a>Maszyny Wirtualnej systemu Linux

Funkcja RSS jest zawsze włączona domyślnie w maszynie Wirtualnej systemu Linux platformy Azure. Jądra systemu Linux wydane od stycznia 2017 r obejmują nowe opcje optymalizacji sieci, które umożliwiają maszyny Wirtualnej systemu Linux w celu osiągnięcia wyższej przepustowości sieci.

### <a name="ubuntu"></a>Ubuntu

Aby uzyskać optymalizacji, najpierw zaktualizować najnowszą obsługiwaną wersję, począwszy od czerwca 2017, który jest:
```json
"Publisher": "Canonical",
"Offer": "UbuntuServer",
"Sku": "16.04-LTS",
"Version": "latest"
```
Po ukończeniu aktualizacji, wprowadź następujące polecenia, aby pobrać najnowsze jądra:

```bash
apt-get -f install
apt-get --fix-missing install
apt-get clean
apt-get -y update
apt-get -y upgrade
```

Polecenie opcjonalne:

`apt-get -y dist-upgrade`
#### <a name="ubuntu-azure-preview-kernel"></a>Ubuntu Azure w wersji zapoznawczej jądra
> [!WARNING]
> Ten jądra systemu Linux Azure w wersji zapoznawczej nie może mieć taki sam poziom dostępności i niezawodności jako obrazy Marketplace i zwolnij jądra, które są zwykle dostępności. Funkcja nie jest obsługiwane, mogą mieć ograniczone możliwości i nie może być tak niezawodna jak domyślnego jądra. Nie należy używać tego jądra dla obciążeń produkcyjnych.

Instalując proponowanych jądra systemu Linux platformy Azure można osiągnąć znaczne przepustowość. Aby wypróbować ten jądra, Dodaj następujący wiersz do /etc/apt/sources.list

```bash
#add this to the end of /etc/apt/sources.list (requires elevation)
deb http://archive.ubuntu.com/ubuntu/ xenial-proposed restricted main multiverse universe
```

Następnie uruchom następujące polecenia, jako element główny.
```bash
apt-get update
apt-get install "linux-azure"
reboot
```

### <a name="centos"></a>CentOS

Aby uzyskać optymalizacji, najpierw zaktualizować najnowszą obsługiwaną wersję, począwszy od 2017 lipca, który jest:
```json
"Publisher": "OpenLogic",
"Offer": "CentOS",
"Sku": "7.3",
"Version": "latest"
```
Po ukończeniu aktualizacji, należy zainstalować najnowsze usługi integracji systemu Linux (LIS).
LIS, zaczynając od 4.2.2-2 jest optymalizacji przepływności. Wprowadź następujące polecenia, aby zainstalować usługi LIS:

```bash
sudo yum update
sudo reboot
sudo yum install microsoft-hyper-v
```

### <a name="red-hat"></a>Red Hat

Aby uzyskać optymalizacji, najpierw zaktualizować najnowszą obsługiwaną wersję, począwszy od 2017 lipca, który jest:
```json
"Publisher": "RedHat"
"Offer": "RHEL"
"Sku": "7.3"
"Version": "7.3.2017071923"
```
Po ukończeniu aktualizacji, należy zainstalować najnowsze usługi integracji systemu Linux (LIS).
Optymalizacji przepływności jest LIS, zaczynając od 4.2. Wprowadź następujące polecenia, aby pobrać i zainstalować LIS:

```bash
mkdir lis4.2.2-2
cd lis4.2.2-2
wget https://download.microsoft.com/download/6/8/F/68FE11B8-FAA4-4F8D-8C7D-74DA7F2CFC8C/lis-rpms-4.2.2-2.tar.gz
tar xvzf lis-rpms-4.2.2-2.tar.gz
cd LISISO
install.sh #or upgrade.sh if prior LIS was previously installed
```

Dowiedz się więcej o Linux Integration Services wersji 4.2 dla funkcji Hyper-V przeglądając [stronę pobierania](https://www.microsoft.com/download/details.aspx?id=55106).

## <a name="next-steps"></a>Następne kroki
* Teraz, gdy maszyna wirtualna jest optymalizowane, zobacz wynik z [przepustowości/przepływność, testowanie maszyny Wirtualnej Azure](virtual-network-bandwidth-testing.md) dla danego scenariusza.
* Dowiedz się więcej o [sieci wirtualnej platformy Azure — często zadawane pytania (FAQ)](virtual-networks-faq.md)
