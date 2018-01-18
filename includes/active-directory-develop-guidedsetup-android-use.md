
## <a name="use-msal-to-get-a-token-for-the-microsoft-graph-api"></a>Użyj MSAL, aby uzyskać token dla interfejsu API programu Microsoft Graph

1.  W obszarze **aplikacji** > **java** > **{domena}. { AppName}**, otwórz `MainActivity`. 
2.  Dodaj następujące instrukcje importu:

    ```java
    import android.app.Activity;
    import android.content.Intent;
    import android.util.Log;
    import android.view.View;
    import android.widget.Button;
    import android.widget.TextView;
    import android.widget.Toast;
    import com.android.volley.*;
    import com.android.volley.toolbox.JsonObjectRequest;
    import com.android.volley.toolbox.Volley;
    import org.json.JSONObject;
    import java.util.HashMap;
    import java.util.List;
    import java.util.Map;
    import com.microsoft.identity.client.*;
    ```

3. Zastąp `MainActivity` klasy z następującym kodem:

    ```java
    public class MainActivity extends AppCompatActivity {

        final static String CLIENT_ID = "[Enter the application Id here]";
        final static String SCOPES [] = {"https://graph.microsoft.com/User.Read"};
        final static String MSGRAPH_URL = "https://graph.microsoft.com/v1.0/me";

        /* UI & Debugging Variables */
        private static final String TAG = MainActivity.class.getSimpleName();
        Button callGraphButton;
        Button signOutButton;

        /* Azure AD Variables */
        private PublicClientApplication sampleApp;
        private AuthenticationResult authResult;

        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);

            callGraphButton = (Button) findViewById(R.id.callGraph);
            signOutButton = (Button) findViewById(R.id.clearCache);

            callGraphButton.setOnClickListener(new View.OnClickListener() {
                public void onClick(View v) {
                    onCallGraphClicked();
                }
            });

            signOutButton.setOnClickListener(new View.OnClickListener() {
                public void onClick(View v) {
                    onSignOutClicked();
                }
            });

    /* Configure your sample app and save state for this activity */
            sampleApp = null;
            if (sampleApp == null) {
                sampleApp = new PublicClientApplication(
                        this.getApplicationContext(),
                        CLIENT_ID);
            }

    /* Attempt to get a user and acquireTokenSilent
    * If this fails we do an interactive request
    */
            List<User> users = null;

            try {
                users = sampleApp.getUsers();

                if (users != null && users.size() == 1) {
            /* We have 1 user */

                    sampleApp.acquireTokenSilentAsync(SCOPES, users.get(0), getAuthSilentCallback());
                } else {
            /* We have no user */

            /* Let's do an interactive request */
                    sampleApp.acquireToken(this, SCOPES, getAuthInteractiveCallback());
                }
            } catch (MsalClientException e) {
                Log.d(TAG, "MSAL Exception Generated while getting users: " + e.toString());

            } catch (IndexOutOfBoundsException e) {
                Log.d(TAG, "User at this position does not exist: " + e.toString());
            }

        }

    //
    // App callbacks for MSAL
    // ======================
    // getActivity() - returns activity so we can acquireToken within a callback
    // getAuthSilentCallback() - callback defined to handle acquireTokenSilent() case
    // getAuthInteractiveCallback() - callback defined to handle acquireToken() case
    //

        public Activity getActivity() {
            return this;
        }

        /* Callback method for acquireTokenSilent calls 
        * Looks if tokens are in the cache (refreshes if necessary and if we don't forceRefresh)
        * else errors that we need to do an interactive request.
        */
        private AuthenticationCallback getAuthSilentCallback() {
            return new AuthenticationCallback() {
                @Override
                public void onSuccess(AuthenticationResult authenticationResult) {
                /* Successfully got a token, call Graph now */
                    Log.d(TAG, "Successfully authenticated");

                /* Store the authResult */
                    authResult = authenticationResult;

                /* call graph */
                    callGraphAPI();

                /* update the UI to post call Graph state */
                    updateSuccessUI();
                }

                @Override
                public void onError(MsalException exception) {
                /* Failed to acquireToken */
                    Log.d(TAG, "Authentication failed: " + exception.toString());

                    if (exception instanceof MsalClientException) {
                    /* Exception inside MSAL, more info inside MsalError.java */
                    } else if (exception instanceof MsalServiceException) {
                    /* Exception when communicating with the STS, likely config issue */
                    } else if (exception instanceof MsalUiRequiredException) {
                    /* Tokens expired or no session, retry with interactive */
                    }
                }

                @Override
                public void onCancel() {
                /* User cancelled the authentication */
                    Log.d(TAG, "User cancelled login.");
                }
            };
        }

        /* Callback used for interactive request.  If succeeds we use the access
            * token to call the Microsoft Graph. Does not check cache
            */
        private AuthenticationCallback getAuthInteractiveCallback() {
            return new AuthenticationCallback() {
                @Override
                public void onSuccess(AuthenticationResult authenticationResult) {
                /* Successfully got a token, call graph now */
                    Log.d(TAG, "Successfully authenticated");
                    Log.d(TAG, "ID Token: " + authenticationResult.getIdToken());

                /* Store the auth result */
                    authResult = authenticationResult;

                /* call Graph */
                    callGraphAPI();

                /* update the UI to post call Graph state */
                    updateSuccessUI();
                }

                @Override
                public void onError(MsalException exception) {
                /* Failed to acquireToken */
                    Log.d(TAG, "Authentication failed: " + exception.toString());

                    if (exception instanceof MsalClientException) {
                    /* Exception inside MSAL, more info inside MsalError.java */
                    } else if (exception instanceof MsalServiceException) {
                    /* Exception when communicating with the STS, likely config issue */
                    }
                }

                @Override
                public void onCancel() {
                /* User cancelled the authentication */
                    Log.d(TAG, "User cancelled login.");
                }
            };
        }

        /* Set the UI for successful token acquisition data */
        private void updateSuccessUI() {
            callGraphButton.setVisibility(View.INVISIBLE);
            signOutButton.setVisibility(View.VISIBLE);
            findViewById(R.id.welcome).setVisibility(View.VISIBLE);
            ((TextView) findViewById(R.id.welcome)).setText("Welcome, " +
                    authResult.getUser().getName());
            findViewById(R.id.graphData).setVisibility(View.VISIBLE);
        }

        /* Use MSAL to acquireToken for the end-user
        * Callback will call Graph api w/ access token & update UI
        */
        private void onCallGraphClicked() {
            sampleApp.acquireToken(getActivity(), SCOPES, getAuthInteractiveCallback());
        }

        /* Handles the redirect from the System Browser */
        @Override
        protected void onActivityResult(int requestCode, int resultCode, Intent data) {
            sampleApp.handleInteractiveRequestRedirect(requestCode, resultCode, data);
        }

    }
    ```

<!--start-collapse-->
### <a name="more-information"></a>Więcej informacji
#### <a name="get-a-user-token-interactively"></a>Pobierz token użytkownika interakcyjnego
Wywoływanie `AcquireTokenAsync` metoda powoduje okna, które monituje użytkowników do logowania. Aplikacje zazwyczaj wymagają użytkownikom na logowanie interakcyjne po raz pierwszy potrzebnych do uzyskania dostępu do chronionego zasobu. Może być muszą się zalogować, gdy dyskretnej operacji można uzyskać tokenu nie powiodło się (na przykład po wygaśnięciu hasła użytkownika).

#### <a name="get-a-user-token-silently"></a>Pobierz token użytkownika dyskretnej
`AcquireTokenSilentAsync` Metoda obsługuje nabycia tokenu i odnowienia bez interakcji użytkownika. Po `AcquireTokenAsync` jest wykonywana po raz pierwszy `AcquireTokenSilentAsync` jest zwykle metodę można uzyskać tokeny, które uzyskują dostęp do chronionych zasobów w kolejnych wywołaniach, ponieważ wywołania żądania lub odnowić tokeny zostały wprowadzone w trybie dyskretnym.

Po pewnym czasie `AcquireTokenSilentAsync` metoda zakończy się niepowodzeniem. Przyczyny niepowodzenia może być, czy użytkownik został wylogowany lub zmienić swoje hasło na innym urządzeniu. Gdy MSAL wykryje, że problem można rozwiązać, gdyż akcję interaktywne, jego generowane `MsalUiRequiredException` wyjątku. Aplikacja może obsłużyć tego wyjątku na dwa sposoby:

* Można wykonać połączenie przed `AcquireTokenAsync` natychmiast. To wywołanie powoduje monitowanie użytkownika do logowania. Ten wzorzec jest zazwyczaj używany w aplikacji online w przypadku, gdy brak dostępnej zawartości w trybie offline dla użytkownika. Przykładowe wygenerowane za pomocą tego Instalatora z przewodnikiem następuje tego wzorca, który można wyświetlić w czasie działania pierwszy wykonać próbki. 
    * Ponieważ żaden użytkownik nie ma korzystali z aplikacji `PublicClientApp.Users.FirstOrDefault()` zawiera wartość null i `MsalUiRequiredException` wyjątku. 
    * Następnie kod w próbce obsługuje wyjątek przez wywołanie metody `AcquireTokenAsync`, która powoduje monitowanie użytkownika do logowania. 

* Ona zamiast tego prezentować oznaczenia wizualne dla użytkowników, którzy interakcyjne logowanie jest wymagane, dzięki czemu użytkownik może wybrać pozycję odpowiednich momentach do logowania. Lub aplikacji można ponowić próbę `AcquireTokenSilentAsync` później. Ten wzorzec jest często używany, gdy użytkownicy mogą używać innych funkcji aplikacji bez przerw w działaniu — na przykład, jeśli w trybie offline zawartość jest dostępna w aplikacji. W takim przypadku użytkownicy mogą wybrać, jeśli chcą, aby zalogować się do dostępu do chronionego zasobu albo Odśwież nieaktualne informacje. Alternatywnie aplikacji można zdecydować ponowić próbę `AcquireTokenSilentAsync` przywrócenie sieci po jest tymczasowo niedostępny. 
<!--end-collapse-->

## <a name="call-the-microsoft-graph-api-by-using-the-token-you-just-obtained"></a>Wywołanie interfejsu API programu Microsoft Graph przy użyciu tokenu, który został uzyskany
Dodaj następujące metody do `MainActivity` klasy:

```java
/* Use Volley to make an HTTP request to the /me endpoint from MS Graph using an access token */
private void callGraphAPI() {
    Log.d(TAG, "Starting volley request to graph");

    /* Make sure we have a token to send to graph */
    if (authResult.getAccessToken() == null) {return;}

    RequestQueue queue = Volley.newRequestQueue(this);
    JSONObject parameters = new JSONObject();

    try {
        parameters.put("key", "value");
    } catch (Exception e) {
        Log.d(TAG, "Failed to put parameters: " + e.toString());
    }
    JsonObjectRequest request = new JsonObjectRequest(Request.Method.GET, MSGRAPH_URL,
            parameters,new Response.Listener<JSONObject>() {
        @Override
        public void onResponse(JSONObject response) {
            /* Successfully called graph, process data and send to UI */
            Log.d(TAG, "Response: " + response.toString());

            updateGraphUI(response);
        }
    }, new Response.ErrorListener() {
        @Override
        public void onErrorResponse(VolleyError error) {
            Log.d(TAG, "Error: " + error.toString());
        }
    }) {
        @Override
        public Map<String, String> getHeaders() throws AuthFailureError {
            Map<String, String> headers = new HashMap<>();
            headers.put("Authorization", "Bearer " + authResult.getAccessToken());
            return headers;
        }
    };

    Log.d(TAG, "Adding HTTP GET to Queue, Request: " + request.toString());

    request.setRetryPolicy(new DefaultRetryPolicy(
            3000,
            DefaultRetryPolicy.DEFAULT_MAX_RETRIES,
            DefaultRetryPolicy.DEFAULT_BACKOFF_MULT));
    queue.add(request);
}

/* Sets the Graph response */
private void updateGraphUI(JSONObject graphResponse) {
    TextView graphText = (TextView) findViewById(R.id.graphData);
    graphText.setText(graphResponse.toString());
}
```
<!--start-collapse-->
### <a name="more-information-about-making-a-rest-call-against-a-protected-api"></a>Więcej informacji na temat nawiązywania połączenia przed chronionego interfejsu API REST

W tej przykładowej aplikacji `callGraphAPI` wywołania `getAccessToken` , a następnie tworzy HTTP `GET` żądania dotyczącego zasobu, która wymaga tokenu i zwraca zawartość. Ta metoda dodaje nabytych przez niego tokenu w nagłówku autoryzacji HTTP. Dla tego przykładu zasób jest interfejsu API programu Microsoft Graph *mnie* punktu końcowego, który wyświetla informacje o profilu użytkownika.
<!--end-collapse-->

## <a name="set-up-sign-out"></a>Konfigurowanie wylogowania

Dodaj następujące metody do `MainActivity` klasy:

```java
/* Clears a user's tokens from the cache.
 * Logically similar to "sign out" but only signs out of this app.
 */
private void onSignOutClicked() {

    /* Attempt to get a user and remove their cookies from cache */
    List<User> users = null;

    try {
        users = sampleApp.getUsers();

        if (users == null) {
            /* We have no users */

        } else if (users.size() == 1) {
            /* We have 1 user */
            /* Remove from token cache */
            sampleApp.remove(users.get(0));
            updateSignedOutUI();

        }
        else {
            /* We have multiple users */
            for (int i = 0; i < users.size(); i++) {
                sampleApp.remove(users.get(i));
            }
        }

        Toast.makeText(getBaseContext(), "Signed Out!", Toast.LENGTH_SHORT)
                .show();

    } catch (MsalClientException e) {
        Log.d(TAG, "MSAL Exception Generated while getting users: " + e.toString());

    } catch (IndexOutOfBoundsException e) {
        Log.d(TAG, "User at this position does not exist: " + e.toString());
    }
}

/* Set the UI for signed-out user */
private void updateSignedOutUI() {
    callGraphButton.setVisibility(View.VISIBLE);
    signOutButton.setVisibility(View.INVISIBLE);
    findViewById(R.id.welcome).setVisibility(View.INVISIBLE);
    findViewById(R.id.graphData).setVisibility(View.INVISIBLE);
    ((TextView) findViewById(R.id.graphData)).setText("No Data");
}
```
<!--start-collapse-->
### <a name="more-information-about-user-sign-out"></a>Więcej informacji na temat wylogowania użytkownika

`onSignOutClicked` w poprzednim kodzie usuwa użytkowników w buforze użytkownika MSAL kod w praktyce określa MSAL zapomnieć bieżącego użytkownika, dzięki czemu przyszłych żądań można uzyskać tokenu powiedzie się tylko wtedy, gdy jest on interaktywne.

Mimo że aplikacja, w tym przykładzie obsługuje pojedynczy użytkowników, MSAL obsługuje scenariusze, w których może być podpisana wiele kont w tym samym czasie. Przykładem jest aplikacja poczty e-mail, gdy użytkownik ma wiele kont.
<!--end-collapse-->
