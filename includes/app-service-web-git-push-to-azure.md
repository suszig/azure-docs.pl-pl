## <a name="push-to-azure-from-git"></a>Wypychanie z narzędzia Git na platformę Azure

Dodaj zdalną platformę Azure do lokalnego repozytorium Git.

```bash
git remote add azure <URI from previous step>
```

Wypchnij na zdalną platformę Azure w celu wdrożenia aplikacji. Zostanie wyświetlony monit o podanie hasła utworzonego wcześniej podczas tworzenia użytkownika wdrożenia. Upewnij się, że zostało wprowadzone hasło utworzone podczas [konfiguracji użytkownika wdrożenia](#configure-a-deployment-user), a nie hasło, którego używasz do logowania się w witrynie Azure Portal.

```bash
git push azure master
```

Poprzednie polecenie wyświetli informacje podobne do następującego przykładu:
