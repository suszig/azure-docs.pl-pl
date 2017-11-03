---
title: "Azure stosu Szybki Start — Tworzenie portalu maszyny Wirtualnej"
description: "Azure stosu Szybki Start — Utwórz Maszynę wirtualną systemu Linux przy użyciu portalu"
services: azure-stack
cloud: azure-stack
author: vhorne
manager: byronr
ms.service: azure-stack
ms.topic: quickstart
ms.date: 09/25/2017
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: 5f815bafdcc7c05ec7f4149fb7c9df178f0f80e2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-linux-virtual-machine-with-the-azure-stack-portal"></a>Utwórz maszynę wirtualną systemu Linux przy użyciu portalu Azure stosu

*Dotyczy: Azure stosu zintegrowanych systemów i Azure stosu Development Kit*

Maszyny wirtualne platformy Azure stosu mogą być tworzone za pośrednictwem portalu Azure stosu. Ta metoda zapewnia interfejs użytkownika opartego na przeglądarce, aby utworzyć i skonfigurować maszynę wirtualną i wszystkie powiązane zasoby. Ta opcja szybkiego startu pokazano, jak szybko utworzyć maszyny wirtualnej systemu Linux i zainstalować serwer sieci web na nim.

## <a name="prerequisites"></a>Wymagania wstępne

* **Obraz systemu Linux w stosie Azure marketplace**

   Domyślnie, stos Azure marketplace nie zawiera obrazu systemu Linux. Tak, przed utworzeniem maszyny wirtualnej systemu Linux, upewnij się, pobrała operator stosu Azure **Ubuntu Server 16.04 LT** obrazu przy użyciu procedury opisanej w [pobieranie elementów marketplace Azure Azure Stos](../azure-stack-download-azure-marketplace-item.md) tematu.

* **Dostęp do klienta SSH**

   Jeśli używasz usługi Azure stosu Development Kit (ASDK), nie masz dostępu do klienta SSH w danym środowisku. Jeśli jest to możliwe, można wybierać między kilka pakietów, które obejmują klienta SSH. Na przykład można zainstalować programu PuTTY, klient SSH i generatora klucza SSH (puttygen.exe). Aby uzyskać więcej informacji na temat możliwości, zobacz artykuł Azure związany z następujących: [jak klucze używanie SSH z systemem Windows Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/ssh-from-windows#windows-packages-and-ssh-clients).

   Ta opcja szybkiego startu używa programu PuTTY do generowania kluczy SSH i nawiązywania połączenia z maszyny wirtualnej systemu Linux. Aby pobrać i zainstalować program PuTTY, przejdź do [http://www.putty.org/](http://www.putty.org).

## <a name="create-an-ssh-key-pair"></a>Tworzenie pary kluczy SSH

Należy parę kluczy SSH, aby ukończyć tego przewodnika Szybki Start. Jeśli masz już parę kluczy SSH, możesz pominąć ten krok.

1. Przejdź do folderu instalacji programu PuTTY (domyślna lokalizacja to ```C:\Program Files\PuTTY```) i uruchom ```puttygen.exe```.
2. W oknie programu PuTTY generatora klucza upewnij się, **typ klucza do wygenerowania** ustawiono **RSA**i **liczba bitów wygenerowany klucz** ustawiono **2048**. Po wykonaniu tych czynności kliknij przycisk **Generuj**.

   ![puttygen.exe](media/azure-stack-quick-linux-portal/Putty01.PNG)

3. Aby ukończyć proces generowania kluczy, przesuń kursor myszy w oknie programu PuTTY generatora klucza.
4. Po zakończeniu generowania kluczy, kliknij **Zapisz klucz publiczny** i **Zapisz klucz prywatny** można zapisać klucze publiczne i prywatne do plików.

   ![Klucze programu puTTY](media/azure-stack-quick-linux-portal/Putty02.PNG)



## <a name="sign-in-to-the-azure-stack-portal"></a>Zaloguj się do portalu Azure stosu

Zaloguj się do portalu Azure stosu. Adres w portalu Azure stosu zależy od tego, na który produkt stosu Azure łączysz się:

* Dla usługi Azure stosu Development Kit (ASDK), przejdź do: https://portal.local.azurestack.external.
* System Azure stosu zintegrowane przejdź do adresu URL, zapewnianej przez operatora sieci Azure stosu.

## <a name="create-the-virtual-machine"></a>Tworzenie maszyny wirtualnej

1. Kliknij przycisk **nowy** znaleziono przycisku w lewym górnym rogu portalu Azure stosu.

2. Wybierz pozycję **Wystąpienia obliczeniowe**, a następnie wybierz pozycję **Ubuntu Server 16.04 LTS**.
3. Kliknij przycisk **Utwórz**.

4. Typ informacji o maszynie wirtualnej. W obszarze **Typ uwierzytelniania** wybierz pozycję **Klucz publiczny SSH**. Gdy wkleić klucz publiczny SSH (który został zapisany do pliku wcześniej), zwrócić uwagę, aby usunąć wszystkie białe wiodących lub końcowych. Po zakończeniu kliknij przycisk **OK**.

   ![Podstawowe informacje dotyczące maszyny wirtualnej](media/azure-stack-quick-linux-portal/linux-01.PNG)

5. Wybierz **D1_V2** dla maszyny wirtualnej.

   ![Rozmiar maszyny](media/azure-stack-quick-linux-portal/linux-02.PNG)

6. Na **ustawienia** , Zachowaj wartości domyślne i kliknij przycisk **OK**.

7. Na **Podsumowanie** kliknij przycisk **OK** rozpocząć wdrażanie maszyny wirtualnej.


## <a name="connect-to-the-virtual-machine"></a>Nawiązywanie połączenia z maszyną wirtualną

1. Kliknij przycisk **Connect** na stronie maszyny wirtualnej. Spowoduje to wyświetlenie parametry połączenia SSH, który może służyć do nawiązania połączenia z maszyną wirtualną.

   ![Podłącz maszynę wirtualną](media/azure-stack-quick-linux-portal/linux-03.PNG)

2. Otwórz program PuTTY.
3. Na **Konfiguracja programu PuTTY** ekranu, w obszarze **kategorii**, rozwiń węzeł **SSH** , a następnie kliknij przycisk **uwierzytelniania**. Kliknij przycisk **Przeglądaj** i wybierz zapisany wcześniej plik klucza prywatnego.

   ![Klucza prywatnego puTTY](media/azure-stack-quick-linux-portal/Putty03.PNG)
4. W obszarze **kategorii**, przewiń w górę i kliknij przycisk **sesji**.
5. W **nazwy hosta (lub adres IP)** Wklej parametry połączenia z portalu Azure stosu, który był wyświetlany poprzednio. W tym przykładzie jest ciąg ```asadmin@192.168.102.34```.
 
   ![Sesji programu puTTY](media/azure-stack-quick-linux-portal/Putty04.PNG)
6. Kliknij przycisk **Otwórz** sesji z maszyną wirtualną.

   ![Linus sesji](media/azure-stack-quick-linux-portal/Putty05.PNG)

## <a name="install-nginx"></a>Instalowanie serwera NGINX

Aby zaktualizować źródła pakietów i zainstaluj najnowszy pakiet NGINX na maszynie wirtualnej, użyj następującego skryptu bash. 

```bash 
#!/bin/bash

# update package source
sudo apt-get -y update

# install NGINX
sudo apt-get -y install nginx
```

Po zakończeniu zamknij sesję SSH i zwraca strony Przegląd maszyny wirtualnej w portalu Azure stosu.


## <a name="open-port-80-for-web-traffic"></a>Otwieranie portu 80 na potrzeby ruchu w sieci Web 

Sieciowa grupa zabezpieczeń zabezpiecza ruch przychodzący i wychodzący. Po utworzeniu maszyny wirtualnej z portalu Azure stosu tworzona jest reguła dla ruchu przychodzącego na porcie 22 dla połączenia SSH. Ponieważ ta maszyna wirtualna jest hostem serwera sieci web, reguły NSG musi zostać utworzony dla portu 80.

1. Na maszynie wirtualnej **omówienie** kliknij nazwę **grupy zasobów**.
2. Wybierz **sieciowej grupy zabezpieczeń** dla maszyny wirtualnej. Sieciową grupę zabezpieczeń można zidentyfikować za pomocą kolumny **Typ**. 
3. W menu po lewej stronie w obszarze **ustawienia**, kliknij przycisk **reguły zabezpieczeń dla ruchu przychodzącego**.
4. Kliknij pozycję **Dodaj**.
5. W polu **Nazwa** wpisz wartość **http**. Upewnij się, że w polu **Zakres portów** ustawiono wartość 80, a w polu **Akcja** — wartość **Zezwalaj**. 
6. Kliknij przycisk **OK**.


## <a name="view-the-nginx-welcome-page"></a>Wyświetlanie strony powitalnej serwera NGINX

Z NGINX zainstalowane, a port 80 jest otwarty na maszynie wirtualnej jest teraz dostępna w maszynie wirtualnej publiczny adres IP serwera sieci web. Publiczny adres IP można znaleźć na stronie Omówienie maszyny wirtualnej w portalu Azure stosu.

Otwórz przeglądarkę sieci web i przejdź do ```http://<public IP address>```.

![Domyślna witryna serwera NGINX](media/azure-stack-quick-linux-portal/linux-04.PNG)


## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Gdy grupa zasobów, maszyna wirtualna i wszystkie pokrewne zasoby nie będą już potrzebne, można je usunąć. Aby to zrobić, wybierz grupę zasobów na stronie maszyny wirtualnej, a następnie kliknij przycisk **usunąć**.

## <a name="next-steps"></a>Następne kroki

W tym szybki start zostały wdrożone prostą maszynę wirtualną systemu Linux, reguły grupy zabezpieczeń sieci i zainstalowany na serwerze sieci web. Aby dowiedzieć się więcej o maszynach wirtualnych Azure stosu, nadal [zagadnienia dotyczące maszyn wirtualnych w stosie Azure](azure-stack-vm-considerations.md).

