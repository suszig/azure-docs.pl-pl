## <a name="prerequisites"></a>Wymagania wstępne
Zanim firma Microsoft można napisać kod zarządzania CDN, musimy wykonać pewne przygotowania do włączenia naszego kodu do interakcji z Menedżerem zasobów Azure.  Aby to zrobić, musisz:

* Utwórz grupę zasobów zawiera profil CDN, utworzone w tym samouczku
* Konfigurowanie usługi Azure Active Directory w celu zapewnienia uwierzytelniania dla aplikacji
* Zastosuj uprawnienia do grupy zasobów, aby tylko autoryzowani użytkownicy z naszych dzierżawy usługi Azure AD mogą współdziałać z naszych profilu CDN

### <a name="creating-the-resource-group"></a>Tworzenie grupy zasobów
1. Zaloguj się do [portalu Azure](https://portal.azure.com).
2. Kliknij przycisk **nowy** przycisk w lewym górnym rogu, a następnie **zarządzania**, i **grupy zasobów**.

    ![Tworzenie nowej grupy zasobów](./media/cdn-app-dev-prep/cdn-new-rg-1-include.png)
3. Wywołaj grupie zasobów *CdnConsoleTutorial*.  Wybierz subskrypcję i wybierz lokalizację, w pobliżu.  Jeśli chcesz, możesz kliknąć pozycję **Przypnij do pulpitu nawigacyjnego** pole wyboru, aby przypiąć grupy zasobów do pulpitu nawigacyjnego w portalu.  Spowoduje to ułatwić znajdowanie w przyszłości.  Po określeniu ustawień, kliknij przycisk **Utwórz**.

    ![Nazwy grupy zasobów](./media/cdn-app-dev-prep/cdn-new-rg-2-include.png)
4. Po utworzeniu grupy zasobów, jeśli nie został on Przypnij do pulpitu nawigacyjnego, możesz go znaleźć, klikając **Przeglądaj**, następnie **grup zasobów**.  Kliknij grupę zasobów, aby go otworzyć.  Zanotuj Twojej **identyfikator subskrypcji**.  Firma Microsoft będzie on potrzebny później.

    ![Nazwy grupy zasobów](./media/cdn-app-dev-prep/cdn-subscription-id-include.png)

### <a name="creating-the-azure-ad-application-and-applying-permissions"></a>Tworzenie aplikacji usługi Azure AD i stosowanie uprawnień
Istnieją dwa podejścia do uwierzytelniania aplikacji w usłudze Azure Active Directory: poszczególnych użytkowników lub nazwy głównej usługi. Nazwy głównej usługi jest podobny do konta usługi systemu Windows.  Zamiast udzielanie określonego użytkownika uprawnień do interakcji z profilów usługi CDN, zamiast tego użytkownikowi uprawnień do nazwy głównej usługi.  Nazwy główne usług są zazwyczaj używane przez procesy zautomatyzowane, nieinterakcyjnym.  Mimo że w tym samouczku jest pisanie aplikacji konsoli interaktywne, firma Microsoft będzie skupić się na podejście głównej usługi.

Tworzenie nazwy głównej usługi składa się z kilku kroków, w tym tworzenie aplikacji usługi Azure Active Directory.  Aby to zrobić, chcemy [czynności opisane w tym samouczku](../articles/resource-group-create-service-principal-portal.md).

> [!IMPORTANT]
> Należy postępować zgodnie z instrukcjami w [połączonego samouczek](../articles/resource-group-create-service-principal-portal.md).  Jest *bardzo ważne* zakończyć je dokładnie zgodnie z opisem.  Należy zwrócić uwagę użytkownika **Identyfikatorem dzierżawy**, **nazwę domeny dzierżawy** (często *. onmicrosoft.com* domeny przed określeniem domenę niestandardową), **identyfikator klienta** , i **klucza uwierzytelniania klienta**, jak firma Microsoft będą one potrzebne później.  Należy zwrócić szczególną uwagę na zabezpieczenia programu **identyfikator klienta** i **klucza uwierzytelniania klienta**, jako te poświadczenia mogą być używane przez każdy komputer można wykonać operacji jako podmiot zabezpieczeń usługi.
>
> Po przejściu do kroku o nazwie Konfiguracja wielodostępnych aplikacji, wybierz **nr**.
>
> Po przejściu do kroku [przypisywanie aplikacji do roli](../articles/azure-resource-manager/resource-group-create-service-principal-portal.md#assign-application-to-role), użyj utworzony wcześniej, grupy zasobów *CdnConsoleTutorial*, ale zamiast **czytnika** roli, przypisać  **Współautor profilu CDN** roli.  Po przypisaniu aplikacji **współautora profilu CDN** roli w danej grupie zasobów, wróć do tego samouczka. 
>
>

Po utworzeniu użytkownika nazwy głównej usługi i po przypisane **współautora profilu CDN** roli, **użytkowników** bloku grupy zasobów powinien wyglądać podobnie do poniższego.

![Blok użytkowników](./media/cdn-app-dev-prep/cdn-service-principal-include.png)

### <a name="interactive-user-authentication"></a>Interakcyjnego uwierzytelniania użytkownika
Jeśli zamiast nazwy głównej usługi, a nie trzeba uwierzytelnianie interakcyjne poszczególnych użytkowników, proces jest bardzo podobny do nazwy głównej usługi.  W rzeczywistości należy postępuj zgodnie z tą samą procedurą, ale wprowadzić kilka drobne zmiany.

> [!IMPORTANT]
> Tylko wykonaj te czynności Jeśli wybierzesz uwierzytelniania indywidualnych użytkowników zamiast nazwy głównej usługi.
>
>

1. Podczas tworzenia aplikacji, zamiast **aplikacji sieci Web**, wybierz **aplikacji natywnej**.

    ![Aplikacja macierzysta](./media/cdn-app-dev-prep/cdn-native-application-include.png)
2. Na następnej stronie pojawi się monit o **identyfikator URI przekierowania**.  Identyfikator URI nie można sprawdzić poprawności, ale pamiętaj został wprowadzony.  Będzie on potrzebny później.
3. Nie istnieje potrzeba do utworzenia **klucza uwierzytelniania klienta**.
4. Zamiast przypisywać nazwy głównej usługi do **współautora profilu CDN** roli, chcemy się przypisanie użytkownikom lub grupom.  W tym przykładzie widać, że zostały przypisane *użytkownika pokaz CDN* do **współautora profilu CDN** roli.  

    ![Dostęp użytkownika](./media/cdn-app-dev-prep/cdn-aad-user-include.png)
