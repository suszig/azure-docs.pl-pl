---
services: virtual-machines
title: How to Log on to a Virtual Machine Running Windows Server
author: cynthn
solutions: ''
manager: timlt
editor: tysonn

{}

---
1. Kliknięcie opcji **Połącz** powoduje utworzenie i pobranie pliku Remote Desktop Protocol (rdp). Kliknij przycisk **Otwórz**, aby użyć tego pliku.
2. Zostanie wyświetlone ostrzeżenie, że plik rdp pochodzi od nieznanego wydawcy. Jest to normalne. W oknie pulpitu zdalnego kliknij przycisk **Połącz**, aby kontynuować.
   
    ![Zrzut ekranu przedstawiający ostrzeżenie o nieznanym wydawcy.](./media/virtual-machines-log-on-win-server/rdp-warn.png)
3. W oknie **Zabezpieczenia systemu Windows** wpisz poświadczenia konta na maszynie wirtualnej, a następnie kliknij przycisk **OK**.
   
    **Konto lokalne** — zazwyczaj jest to nazwa użytkownika konta lokalnego i hasło określone podczas tworzenia maszyny wirtualnej. W tym przypadku domeną jest nazwa maszyny wirtualnej wprowadzana jako *nazwa_maszyny_wirtualnej*&#92;*nazwa_użytkownika*.  
   
    **Maszyna wirtualna przyłączona do domeny** — jeśli maszyna wirtualna należy do domeny, wprowadź nazwę użytkownika w formacie *domena*&#92;*nazwa_użytkownika*. Konto musi również należeć do grupy Administratorzy albo mieć przydzielone uprawnienia dostępu zdalnego do maszyny wirtualnej.
   
    **Kontroler domeny** — jeśli maszyna wirtualna jest kontrolerem domeny, wpisz nazwę użytkownika i hasło konta administratora danej domeny.
4. Kliknij przycisk **Tak**, aby potwierdzić tożsamość maszyny wirtualnej i zakończyć logowanie.
   
   ![Zrzut ekranu przedstawiający komunikat dotyczący potwierdzenia tożsamości maszyny wirtualnej.](./media/virtual-machines-log-on-win-server/cert-warning.png)

<!--HONumber=Sep16_HO3-->


