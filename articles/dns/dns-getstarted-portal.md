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
translationtype: Human Translation
ms.sourcegitcommit: b4802009a8512cb4dcb49602545c7a31969e0a25
ms.openlocfilehash: 79f0c9297c4be70f705f325274f3d9241ea4bc3f
ms.lasthandoff: 03/29/2017

---

# <a name="get-started-with-azure-dns-using-the-azure-portal"></a>Rozpoczynanie pracy z usługą Azure DNS przy użyciu witryny Azure Portal

> [!div class="op_single_selector"]
> * [Witryna Azure Portal](dns-getstarted-portal.md)
> * [PowerShell](dns-getstarted-powershell.md)
> * [Interfejs wiersza polecenia platformy Azure 1.0](dns-getstarted-cli-nodejs.md)
> * [Interfejs wiersza polecenia platformy Azure 2.0](dns-getstarted-cli.md)

W tym artykule przedstawiono kroki umożliwiające utworzenie po raz pierwszy strefy i rekordu DNS przy użyciu witryny Azure Portal. Te kroki można również wykonać przy użyciu programu Azure PowerShell lub wieloplatformowego interfejsu wiersza polecenia platformy Azure.

Strefa DNS jest używana do hostowania rekordów DNS dla określonej domeny. Aby rozpocząć hostowanie domeny w usłudze Azure DNS, musisz utworzyć strefę DNS dla tej nazwy domeny. Każdy rekord DNS domeny zostanie utworzony w tej strefie DNS. Aby na koniec opublikować strefę DNS w Internecie, należy skonfigurować serwery nazw dla domeny. Poniżej opisano każdy z tych kroków.

## <a name="create-a-dns-zone"></a>Tworzenie strefy DNS

1. Logowanie się do witryny Azure Portal
2. W menu Centrum kliknij pozycję **Nowa > Sieć >** , a następnie kliknij pozycję **Strefa DNS**, aby otworzyć blok Tworzenie strefy DNS.

    ![Strefa DNS](./media/dns-getstarted-portal/openzone650.png)

4. W bloku **Tworzenie strefy DNS** nadaj nazwę strefie DNS. Na przykład *contoso.com*.
 
    ![Tworzenie strefy](./media/dns-getstarted-portal/newzone250.png)

5. Następnie określ grupę zasobów, która ma być używana. Możesz utworzyć nową grupę zasobów lub wybrać już istniejącą. Jeśli wybierzesz opcję utworzenia nowej grupy zasobów, użyj listy rozwijanej **Lokalizacja**, aby określić lokalizację grupy zasobów. Pamiętaj, że to ustawienie dotyczy lokalizacji grupy zasobów i nie ma wpływu na strefę DNS. Lokalizacja strefy DNS jest zawsze „globalna” i nie jest wyświetlana.

6. Możesz pozostawić zaznaczone pole wyboru **Przypnij do pulpitu nawigacyjnego**, jeśli chcesz w łatwy sposób zlokalizować nową strefę na pulpicie nawigacyjnym. Następnie kliknij pozycję **Utwórz**.

    ![Przypnij do pulpitu nawigacyjnego](./media/dns-getstarted-portal/pindashboard150.png)

7. Po kliknięciu pozycji Utwórz wyświetlone zostanie konfigurowanie nowej strefy na pulpicie nawigacyjnym.

    ![Tworzenie](./media/dns-getstarted-portal/creating150.png)

8. Po utworzeniu nowej strefy na pulpicie nawigacyjnym zostanie wyświetlony nowy blok.


## <a name="create-a-dns-record"></a>Tworzenie rekordu DNS

W tym przykładzie przedstawiono proces tworzenia nowego rekordu „A”. Aby uzyskać informacje o innych typach rekordów oraz sposobie modyfikowania istniejących rekordów, zobacz [Manage DNS records and record sets by using the Azure portal](dns-operations-recordsets-portal.md) (Zarządzanie rekordami i zestawami rekordów DNS przy użyciu witryny Azure Portal). 


1. W górnej części bloku **Strefa DNS** wybierz pozycję **+ Zestaw rekordów**, aby otworzyć blok **Dodawanie zestawu rekordów**.

    ![Nowy zestaw rekordów](./media/dns-getstarted-portal/newrecordset500.png)

4. W bloku **Dodawanie zestawu rekordów** nazwij zestaw rekordów. Zestaw rekordów można na przykład nazwać „**www**”.

    ![Dodawanie zestawu rekordów](./media/dns-getstarted-portal/addrecordset500.png)

5. Wybierz typ rekordu do utworzenia. Na potrzeby tego przykładu wybierz wartość **A**.
6. Ustaw wartość dla pozycji **Czas wygaśnięcia**. Domyślny czas wygaśnięcia to jedna godzina.
7. Dodaj adres IP rekordu.
8. Kliknij przycisk **OK** na dole bloku, aby utworzyć rekord DNS.


## <a name="view-records"></a>Wyświetlanie rekordów

W dolnej części bloku Strefa DNS wyświetlone są rekordy dla strefy DNS. Powinny zostać wyświetlone domyślne rekordy DNS i SOA tworzone w każdej strefie oraz wszystkie nowo utworzone rekordy.

![strefa](./media/dns-getstarted-portal/viewzone500.png)


## <a name="update-name-servers"></a>Aktualizowanie serwerów nazw

Po poprawnym skonfigurowaniu strefy i rekordów DNS należy skonfigurować nazwę domeny w celu użycia serwerów nazw usługi Azure DNS. Umożliwi to innym użytkownikom w Internecie znalezienie Twoich rekordów DNS.

Serwery nazw dla strefy są podane w witrynie Azure Portal:

![strefa](./media/dns-getstarted-portal/viewzonens500.png)

Te serwery nazw powinny zostać skonfigurowane u rejestratora nazw domen (w miejscu zakupu nazwy domeny). Rejestrator zaoferuje opcję skonfigurowania serwerów nazw na potrzeby domeny. Aby uzyskać więcej informacji, zobacz [Delegowanie domeny do usługi Azure DNS](dns-domain-delegation.md).


## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się więcej na temat usługi Azure DNS, zobacz [Omówienie usługi Azure DNS](dns-overview.md).

Aby dowiedzieć się więcej na temat zarządzania rekordami DNS w usłudze Azure DNS, zobacz [Manage DNS records and record sets by using the Azure portal](dns-operations-recordsets-portal.md) (Zarządzanie rekordami i zestawami rekordów DNS przy użyciu witryny Azure Portal).


