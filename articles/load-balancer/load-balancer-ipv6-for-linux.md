---
title: "Konfigurowanie protokołu DHCPv6 dla maszyn wirtualnych systemu Linux | Dokumentacja firmy Microsoft"
description: "Jak skonfigurować DHCPv6 dla maszyn wirtualnych systemu Linux."
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
editor: 
keywords: "Protokół IPv6, usługi równoważenia obciążenia azure, podwójnego stosu, publiczny adres ip, natywnego protokołu ipv6, mobile, iot"
ms.assetid: b32719b6-00e8-4cd0-ba7f-e60e8146084b
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: 84558cb6e3a5524969f590eb0272a64ad8839ab5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="configuring-dhcpv6-for-linux-vms"></a>Konfigurowanie protokołu DHCPv6 dla maszyn wirtualnych systemu Linux

[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]

Niektóre obrazy maszyny wirtualnej systemu Linux w portalu Azure Marketplace nie mają domyślnie skonfigurowany tak, protokołu DHCPv6. Aby obsługiwać protokół IPv6, DHCPv6 muszą być skonfigurowane w w dystrybucji systemu operacyjnego Linux, którego używasz. Różne dystrybucje systemu Linux mają różne sposoby konfigurowania protokołu DHCPv6, ponieważ korzystają z różnych pakietów.

> [!NOTE]
> Ostatnie SUSE Linux i CoreOS obrazów w portalu Azure Marketplace zostały wstępnie skonfigurowany z użyciem protokołu DHCPv6. Korzystając z tych obrazów są wymagane nie dodatkowe zmiany.

Ten dokument zawiera opis sposobu zapewniają DHCPv6, dzięki czemu maszyny wirtualnej systemu Linux uzyskuje adres IPv6.

> [!WARNING]
> Nieprawidłowo edycję plików konfiguracyjnych sieci może spowodować utratę dostępu do sieci do maszyny Wirtualnej. Zaleca się przetestowanie zmiany konfiguracji w systemach nieprodukcyjnych. Instrukcje w tym artykule zostały przetestowane na najnowsze wersje obrazów systemu Linux w portalu Azure Marketplace. Dokumentacja konkretnej wersji systemu Linux bardziej szczegółowe informacje.

## <a name="ubuntu"></a>Ubuntu

1. Edytuj plik `/etc/dhcp/dhclient6.conf` i Dodaj następujący wiersz:

        timeout 10;

2. Zmień konfigurację sieci dla interfejsu eth0 przy użyciu następującej konfiguracji:

   * Na **Ubuntu 12.04 i 14.04**, przeprowadź edycję pliku`/etc/network/interfaces.d/eth0.cfg`
   * Na **Ubuntu 16.04**, przeprowadź edycję pliku`/etc/network/interfaces.d/50-cloud-init.cfg`

         iface eth0 inet6 auto
             up sleep 5
             up dhclient -1 -6 -cf /etc/dhcp/dhclient6.conf -lf /var/lib/dhcp/dhclient6.eth0.leases -v eth0 || true

3. Odnów adres IPv6:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="debian"></a>Debian

1. Edytuj plik `/etc/dhcp/dhclient6.conf` i Dodaj następujący wiersz:

        timeout 10;

2. Edytuj plik `/etc/network/interfaces` i dodaj następującą konfigurację:

        iface eth0 inet6 auto
            up sleep 5
            up dhclient -1 -6 -cf /etc/dhcp/dhclient6.conf -lf /var/lib/dhcp/dhclient6.eth0.leases -v eth0 || true

3. Odnów adres IPv6:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="rhel--centos--oracle-linux"></a>RHEL / CentOS / Oracle Linux

1. Edytuj plik `/etc/sysconfig/network` i Dodaj następujący parametr:

        NETWORKING_IPV6=yes

2. Edytuj plik `/etc/sysconfig/network-scripts/ifcfg-eth0` i dodaj następujące dwa parametry:

        IPV6INIT=yes
        DHCPV6C=yes

3. Odnów adres IPv6:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="sles-11--opensuse-13"></a>SLES 11 & openSUSE 13

Ostatnie SLES i openSUSE obrazów na platformie Azure zostały wstępnie skonfigurowany z użyciem protokołu DHCPv6. Korzystając z tych obrazów są wymagane nie dodatkowe zmiany. Jeśli masz maszyny Wirtualnej na podstawie obrazu SUSE starszych lub niestandardowego, wykonaj następujące kroki:

1. Zainstaluj `dhcp-client` pakietu, w razie potrzeby:

    ```bash
    sudo zypper install dhcp-client
    ```

2. Edytuj plik `/etc/sysconfig/network/ifcfg-eth0` i Dodaj następujący parametr:

        DHCLIENT6_MODE='managed'

3. Odnów adres IPv6:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="sles-12-and-opensuse-leap"></a>SLES 12 i openSUSE przestępnego

Ostatnie SLES i openSUSE obrazów na platformie Azure zostały wstępnie skonfigurowany z użyciem protokołu DHCPv6. Korzystając z tych obrazów są wymagane nie dodatkowe zmiany. Jeśli masz maszyny Wirtualnej na podstawie obrazu SUSE starszych lub niestandardowego, wykonaj następujące kroki:

1. Edytuj plik `/etc/sysconfig/network/ifcfg-eth0` i Zastąp ten parametr

        #BOOTPROTO='dhcp4'

    przy użyciu następującej wartości:

        BOOTPROTO='dhcp'

2. Dodaj następujący parametr do `/etc/sysconfig/network/ifcfg-eth0`:

        DHCLIENT6_MODE='managed'

3. Odnów adres IPv6:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="coreos"></a>CoreOS

Ostatnie obrazy CoreOS na platformie Azure zostały wstępnie skonfigurowany z użyciem protokołu DHCPv6. Korzystając z tych obrazów są wymagane nie dodatkowe zmiany. Jeśli masz maszyny Wirtualnej na podstawie obrazu CoreOS starszych lub niestandardowego, wykonaj następujące kroki:

1. Przeprowadź edycję pliku`/etc/systemd/network/10_dhcp.network`

        [Match]
        eth0

        [Network]
        DHCP=ipv6

2. Odnów adres IPv6:

    ```bash
    sudo systemctl restart systemd-networkd
    ```
