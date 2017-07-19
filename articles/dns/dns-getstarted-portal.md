---
title: "Rozpoczynanie pracy z usługą Azure DNS przy użyciu witryny Azure Portal | Microsoft Docs"
description: "Dowiedz się, jak utworzyć strefę i rekord DNS w usłudze Azure DNS. W tym szczegółowym przewodniku pokazano, jak utworzyć po raz pierwszy strefę i rekord DNS przy użyciu witryny Azure Portal."
services: dns
documentationcenter: na
author: jtuliani
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: fb0aa0a6-d096-4d6a-b2f6-eda1c64f6182
ms.service: dns
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/10/2017
ms.author: jonatul
ms.translationtype: Human Translation
ms.sourcegitcommit: 95b8c100246815f72570d898b4a5555e6196a1a0
ms.openlocfilehash: 93b24e3d9fbb3fbb3ea995271fd63d1e82eb9c9e
ms.contentlocale: pl-pl
ms.lasthandoff: 05/18/2017

---

# <a name="get-started-with-azure-dns-using-the-azure-portal"></a>Rozpoczynanie pracy z usługą Azure DNS przy użyciu witryny Azure Portal

> [!div class="op_single_selector"]
> * [Witryna Azure Portal](dns-getstarted-portal.md)
> * [PowerShell](dns-getstarted-powershell.md)
> * [Interfejs wiersza polecenia platformy Azure 1.0](dns-getstarted-cli-nodejs.md)
> * [Interfejs wiersza polecenia platformy Azure 2.0](dns-getstarted-cli.md)

W tym artykule przedstawiono kroki umożliwiające utworzenie po raz pierwszy strefy i rekordu DNS przy użyciu witryny Azure Portal. Te kroki można również wykonać przy użyciu programu Azure PowerShell lub wieloplatformowego interfejsu wiersza polecenia platformy Azure.

Strefa DNS jest używana do hostowania rekordów DNS dla określonej domeny. Aby rozpocząć hostowanie domeny w usłudze Azure DNS, musisz utworzyć strefę DNS dla tej nazwy domeny. Każdy rekord DNS domeny zostanie utworzony w tej strefie DNS. Aby na koniec opublikować strefę DNS w Internecie, należy skonfigurować serwery nazw dla domeny. Wszystkie wymienione kroki zostały opisane poniżej.

## <a name="create-a-dns-zone"></a>Tworzenie strefy DNS

1. Logowanie się do witryny Azure Portal
2. W menu Centrum kliknij pozycję **Nowa > Sieć >** , a następnie kliknij pozycję **Strefa DNS**, aby otworzyć blok Tworzenie strefy DNS.

    ![Strefa DNS](./media/dns-getstarted-portal/openzone650.png)

4. W bloku **Tworzenie strefy DNS** wprowadź następujące wartości, a następnie kliknij pozycję **Utwórz**:


   | **Ustawienie** | **Wartość** | **Szczegóły** |
   |---|---|---|
   |**Nazwa**|contoso.com|Nazwa strefy DNS|
   |**Subskrypcja**|[Twoja subskrypcja]|Wybierz subskrypcję, aby utworzyć w jej ramach strefę DNS.|
   |**Grupa zasobów**|**Utwórz nową:** contosoDNSRG|Utwórz grupę zasobów. Nazwa grupy zasobów musi być unikatowa w obrębie wybranej subskrypcji. Aby dowiedzieć się więcej na temat grup zasobów, zapoznaj się z artykułem [Omówienie usługi Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups).|
   |**Lokalizacja**|Zachodnie stany USA||

> [!NOTE]
> Ustawienie Grupa zasobów dotyczy lokalizacji grupy zasobów i nie ma wpływu na strefę DNS. Lokalizacja strefy DNS jest zawsze „globalna” i nie jest wyświetlana.

## <a name="create-a-dns-record"></a>Tworzenie rekordu DNS

W tym przykładzie przedstawiono proces tworzenia nowego rekordu „A”. Aby uzyskać informacje o innych typach rekordów oraz sposobie modyfikowania istniejących rekordów, zobacz [Manage DNS records and record sets by using the Azure portal](dns-operations-recordsets-portal.md) (Zarządzanie rekordami i zestawami rekordów DNS przy użyciu witryny Azure Portal). 

1. Gdy utworzysz strefę DNS, w okienku **Ulubione** witryny Azure Portal kliknij pozycję **Wszystkie zasoby**. W bloku Wszystkie zasoby kliknij strefę DNS **contoso.com**. Jeśli wybrana subskrypcja zawiera kilka zasobów, możesz wpisać **contoso.com** w polu **Filtruj według nazwy...**, aby łatwo uzyskać dostęp do strefy DNS.

1. W górnej części bloku **Strefa DNS** wybierz pozycję **+ Zestaw rekordów**, aby otworzyć blok **Dodawanie zestawu rekordów**.

1. W bloku **Dodaj zestaw rekordów** wprowadź następujące wartości, a następnie kliknij przycisk **OK**. W tym przykładzie jest tworzony rekord A.

   |**Ustawienie** | **Wartość** | **Szczegóły** |
   |---|---|---|
   |**Nazwa**|www|Nazwa rekordu|
   |**Typ**|A| Typ tworzonego rekordu DNS, dozwolone wartości to A, AAAA, CNAME, MX, NS, SRV, TXT i PTR.  Aby uzyskać więcej informacji na temat typów rekordów, odwiedź stronę [Omówienie stref i rekordów DNS](dns-zones-records.md).|
   |**Czas wygaśnięcia**|1|Czas wygaśnięcia żądania DNS.|
   |**Jednostka czasu wygaśnięcia**|Godziny|Pomiar wartości czasu wygaśnięcia.|
   |**Adres IP**|ipAddressValue| Ta wartość jest adresem IP rozpoznawanym przez rekord DNS.|

## <a name="view-records"></a>Wyświetlanie rekordów

W dolnej części bloku Strefa DNS wyświetlone są rekordy dla strefy DNS. Powinny zostać wyświetlone domyślne rekordy DNS i SOA tworzone w każdej strefie oraz wszystkie nowo utworzone rekordy.

![strefa](./media/dns-getstarted-portal/viewzone500.png)


## <a name="update-name-servers"></a>Aktualizowanie serwerów nazw

Po poprawnym skonfigurowaniu strefy i rekordów DNS należy skonfigurować nazwę domeny w celu użycia serwerów nazw usługi Azure DNS. Umożliwi to innym użytkownikom w Internecie znalezienie Twoich rekordów DNS.

Serwery nazw dla strefy są podane w witrynie Azure Portal:

![strefa](./media/dns-getstarted-portal/viewzonens500.png)

Te serwery nazw powinny zostać skonfigurowane u rejestratora nazw domen (w miejscu zakupu nazwy domeny). Rejestrator zaproponuje opcję skonfigurowania serwerów nazw na potrzeby domeny. Aby uzyskać więcej informacji, zobacz [Delegowanie domeny do usługi Azure DNS](dns-domain-delegation.md).

## <a name="delete-all-resources"></a>Usuwanie wszystkich zasobów

Aby usunąć wszystkie zasoby utworzone w tym artykule, wykonaj następujące czynności:

1. W okienku **Ulubione** witryny Azure Portal kliknij pozycję **Wszystkie zasoby**. W bloku Wszystkie zasoby kliknij grupę zasobów **MyResourceGroup**. Jeśli wybrana subskrypcja zawiera kilka zasobów, możesz wpisać **MyResourceGroup** w polu **Filtruj według nazwy...**, aby łatwo uzyskać dostęp do grupy zasobów.
1. W bloku **MyResourceGroup** kliknij przycisk **Usuń**.
1. Portal wymaga wpisania nazwy grupy zasobów w celu potwierdzenia zamiaru jej usunięcia. Kliknij pozycję **Usuń**, wpisz *MyResourceGroup* jako nazwę grupy zasobów, a następnie kliknij przycisk **Usuń**. Usunięcie grupy zasobów powoduje usunięcie wszystkich zasobów w niej zawartych, dlatego zawsze należy sprawdzić zawartość grupy zasobów przed jej usunięciem. Portal usuwa wszystkie zasoby zawarte w grupie zasobów, a następnie usuwa tę grupę zasobów. Ten proces trwa kilka minut.


## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się więcej na temat usługi Azure DNS, zobacz [Omówienie usługi Azure DNS](dns-overview.md).

Aby dowiedzieć się więcej na temat zarządzania rekordami DNS w usłudze Azure DNS, zobacz [Manage DNS records and record sets by using the Azure portal](dns-operations-recordsets-portal.md) (Zarządzanie rekordami i zestawami rekordów DNS przy użyciu witryny Azure Portal).


