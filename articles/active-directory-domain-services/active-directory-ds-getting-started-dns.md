<properties
    pageTitle="Usługi domenowe Azure AD: aktualizowanie ustawień DNS na potrzeby sieci wirtualnej platformy Azure | Microsoft Azure"
    description="Wprowadzenie do usługi Active Directory Domain Services (wersja zapoznawcza)"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/06/2016"
    ms.author="maheshu"/>


# Usługi domenowe Azure AD *(wersja zapoznawcza)* — aktualizowanie ustawień DNS dla sieci wirtualnej platformy Azure

## Zadanie 4. Aktualizowanie ustawień DNS dla sieci wirtualnej platformy Azure
Po pomyślnym włączeniu Usług domenowych Azure AD dla katalogu należy zapewnić komputerom w sieci wirtualnej możliwość łączenia się z tymi usługami i korzystania z nich. W tym celu trzeba zaktualizować ustawienia serwera DNS dla sieci wirtualnej, wskazując adresy IP, pod którymi Usługi domenowe Azure AD są dostępne w sieci wirtualnej.

> [AZURE.NOTE] Po włączeniu Usług domenowych Azure AD dla katalogu zanotuj ich adresy wyświetlane na karcie **Konfigurowanie** katalogu.

Wykonaj poniższe czynności konfiguracyjne, aby zaktualizować ustawienia serwera DNS dla sieci wirtualnej, w której włączono Usługi domenowe Azure AD.

1. Przejdź do **klasycznego portalu Azure** ([https://manage.windowsazure.com](https://manage.windowsazure.com)).

2. W okienku po lewej stronie wybierz węzeł **Sieci**.

    ![Węzeł sieci wirtualnych](./media/active-directory-domain-services-getting-started/virtual-network-select.png)

3. Na karcie **Sieci wirtualne** wybierz sieć wirtualną, w której włączono Usługi domenowe Azure AD, aby wyświetlić jej właściwości.

4. Kliknij kartę **Konfigurowanie**.

    ![Węzeł sieci wirtualnych](./media/active-directory-domain-services-getting-started/virtual-network-configure-tab.png)

5. W sekcji **Serwery DNS** wprowadź adresy IP Usług domenowych Azure AD.

6. Upewnij się, że wprowadzono obydwa adresy IP wyświetlane w sekcji **Usługi domenowe** na karcie **Konfigurowanie** katalogu.

7. Kliknij pozycję **Zapisz** w okienku zadań w dolnej części strony, aby zapisać ustawienia serwera DNS tej sieci wirtualnej.

   ![Zaktualizuj ustawienia serwera DNS dla sieci wirtualnej.](./media/active-directory-domain-services-getting-started/update-dns.png)

> [AZURE.NOTE] Po zaktualizowaniu ustawień serwera DNS dla sieci wirtualnej może upłynąć pewien czasu do momentu zastosowania zaktualizowanej konfiguracji DNS na maszynach wirtualnych w sieci. Jeśli maszyna wirtualna nie może połączyć się z domeną, możesz opróżnić pamięć podręczną DNS (np. wydając polecenie „ipconfig/flushdns”) na maszynie wirtualnej, aby wymusić odświeżenie na niej ustawień DNS.


## Zadanie 5. Włączanie synchronizacji haseł w Usługach domenowych Azure AD
Następne zadanie konfiguracyjne to [włączenie synchronizacji haseł w Usługach domenowych Azure AD](active-directory-ds-getting-started-password-sync.md).



<!--HONumber=Sep16_HO3-->


