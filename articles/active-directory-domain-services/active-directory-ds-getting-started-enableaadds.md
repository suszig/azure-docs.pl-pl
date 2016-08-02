<properties
    pageTitle="Usługi domenowe Azure AD: włączanie Usług domenowych Azure AD | Microsoft Azure"
    description="Wprowadzenie do Usług domenowych Azure Active Directory (wersja zapoznawcza)"
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
    ms.date="04/25/2016"
    ms.author="maheshu"/>

# Usługi domenowe Azure AD *(wersja zapoznawcza)* — włączanie Usług domenowych Azure AD

## Zadanie 3. Włączanie Usług domenowych Azure AD
W ramach tego zadania włączysz Usługi domenowe Azure AD dla katalogu. Wykonaj poniższe czynności konfiguracyjne, aby włączyć Usługi domenowe Azure AD dla katalogu.

1. Przejdź do **klasycznego portalu Azure** ([https://manage.windowsazure.com](https://manage.windowsazure.com)).

2. W lewym okienku wybierz węzeł **Active Directory**.

3. Wybierz dzierżawę (katalog) usługi Azure AD, dla której chcesz włączyć Usługi domenowe Azure AD.

    ![Wybieranie katalogu usługi Azure AD](./media/active-directory-domain-services-getting-started/select-aad-directory.png)

4. Kliknij kartę **Konfigurowanie**.

    ![Karta Konfigurowanie w katalogu](./media/active-directory-domain-services-getting-started/configure-tab.png)

5. Przewiń w dół do sekcji o nazwie **Usługi domenowe**.

    ![Sekcja konfiguracji usług domenowych](./media/active-directory-domain-services-getting-started/domain-services-configuration.png)

6. Ustaw opcję o nazwie **Włącz usługi domenowe dla tego katalogu** na wartość **TAK**. Zauważysz, że na stronie pojawią się kolejne opcje konfiguracji Usług domenowych Azure AD.

    ![Włączanie Usług domenowych](./media/active-directory-domain-services-getting-started/enable-domain-services.png)

    > [AZURE.NOTE] Po włączeniu Usług domenowych Azure AD dla dzierżawy usługa Azure AD wygeneruje i będzie przechowywać skróty poświadczeń Kerberos i NTLM wymagane do uwierzytelniania użytkowników.

7. Określ **nazwę domeny DNS dla usług domenowych**.

   - Domyślnie zostanie wybrana domyślna nazwa domeny katalogu (kończąca się sufiksem domeny **.onmicrosoft.com**).

   - Lista zawiera wszystkie domeny skonfigurowane dla katalogu usługi Azure AD, w tym domeny zweryfikowane i niezweryfikowane, które można skonfigurować na karcie „Domeny”.

   - Można też dodać do tej listy niestandardową nazwę domeny, wpisując ją. W tym przykładzie wpisaliśmy niestandardową nazwę domeny „contoso100.com”.

     > [AZURE.WARNING] Upewnij się, że wybrany prefiks nazwy domeny (np. „contoso100” w nazwie domeny „contoso100.com”) jest krótszy niż 15 znaków. Nie można utworzyć domeny Usług domenowych Azure AD z prefiksem domeny dłuższym niż 15 znaków.

8. Następnym krokiem jest wybranie sieci wirtualnej, w której mają być dostępne Usługi domenowe Azure AD. Wybierz nowo utworzoną sieć wirtualną z listy rozwijanej o nazwie **Połącz usługi domenowe z tą siecią wirtualną**.

   - Upewnij się, że wybrana sieć wirtualna należy do regionu platformy Azure obsługiwanego przez Usługi domenowe Azure AD.

   - Zapoznaj się ze stroną przedstawiającą [usługi platformy Azure uporządkowane według regionów](https://azure.microsoft.com/regions/#services/), aby dowiedzieć się, w których regionach platformy Azure są dostępne Usługi domenowe Azure AD.

   - Zwróć uwagę, że sieci wirtualne należące do regionu, w którym Usługi domenowe Azure AD nie są obsługiwane, nie pojawiają się na liście rozwijanej.

   - Podobnie sieci wirtualne, które zostały utworzone przy użyciu usługi Azure Resource Manager (sieci wirtualne oparte na usłudze ARM), nie pojawiają się na liście rozwijanej. Dzieje się tak dlatego, że sieci wirtualne oparte na usłudze ARM nie są obecnie obsługiwane przez Usługi domenowe Azure AD.

9. Upewnij się, że nazwa domeny DNS wybrana dla domeny zarządzanej nie istnieje już w sieci wirtualnej. Może się tak zdarzyć w przypadku dowolnego z następujących scenariuszy:

   - Jeśli masz już domenę z tą samą nazwą domeny DNS w sieci wirtualnej.

   - Jeśli wybrana sieć wirtualna ma połączenie sieci VPN z siecią lokalną i masz domenę z tą samą nazwą domeny DNS w sieci lokalnej.

   - Jeśli masz istniejącą usługę w chmurze z tą nazwą w sieci wirtualnej.

10. Po wybraniu powyższych opcji kliknij pozycję **Zapisz** w okienku zadań w dolnej części strony, aby włączyć Usługi domenowe Azure AD.

11. Zostanie wyświetlona strona ze stanem „Oczekiwanie...”, który będzie wskazywać, że Usługi domenowe Azure AD są włączane dla katalogu.

    ![Włączanie usług domenowych — stan oczekiwania](./media/active-directory-domain-services-getting-started/enable-domain-services-pendingstate.png)

    > [AZURE.NOTE] Usługi domenowe Azure AD oferują wysoką dostępność na potrzeby domeny zarządzanej. Po pierwszym włączeniu Usług domenowych Azure AD w domenie zostaną kolejno wyświetlone adresy IP, w których Usługi domenowe będą dostępne w sieci wirtualnej. Drugi adres IP pojawi się wkrótce, kiedy tylko usługa zapewni wysoką dostępność domeny. Po skonfigurowaniu i uaktywnieniu wysokiej dostępności domeny w sekcji **usług domenowych** na karcie **Konfigurowanie** powinny być widoczne dwa adresy IP.

12. Po około 20–30 minutach w polu **Adres IP** na stronie **Konfigurowanie** zobaczysz pierwszy adres IP, pod którym są dostępne Usługi domenowe.

    ![Włączone Usługi domenowe — pierwszy adres IP zaprowizowany](./media/active-directory-domain-services-getting-started/domain-services-enabled-firstdc-available.png)

13. Gdy domena oferuje działającą opcję wysokiej dostępności, na stronie są wyświetlane dwa adresy IP. Są to adresy IP, w których Usługi domenowe Azure AD będą dostępne w wybranej sieci wirtualnej. Zanotuj te adresy IP, aby zaktualizować ustawienia DNS sieci wirtualnej. Ten krok umożliwia łączenie maszyn wirtualnych w sieci wirtualnej z domeną w celu wykonywania operacji takich jak przyłączanie do domeny.

    ![Włączone Usługi domenowe — obydwa adresy IP zaprowizowane](./media/active-directory-domain-services-getting-started/domain-services-enabled-bothdcs-available.png)

> [AZURE.NOTE] Czas potrzebny na udostępnienie zawartości katalogu w Usługach domenowych Azure AD zależy od rozmiaru katalogu usługi Azure AD (liczby użytkowników, grup itp.). Ten proces synchronizacji jest wykonywany w tle. W przypadku dużych katalogów z dziesiątkami tysięcy obiektów zsynchronizowanie i udostępnienie w Usługach domenowych Azure AD wszystkich użytkowników, członkostw w grupach i poświadczeń może potrwać dzień lub dwa.

<br>

## Zadanie 4. Aktualizowanie ustawień DNS na potrzeby sieci wirtualnej platformy Azure
Następne zadanie konfiguracji to [zaktualizowanie ustawień DNS na potrzeby sieci wirtualnej platformy Azure](active-directory-ds-getting-started-dns.md).



<!--HONumber=Jun16_HO2-->


