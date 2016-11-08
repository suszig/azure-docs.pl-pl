---
title: Microsoft Power BI Embedded Preview — osadzanie raportu usługi Power BI przy użyciu elementu IFrame
description: Microsoft Power BI Embedded Preview — podstawowy kod służący do integracji raportu w aplikacji, sposób uwierzytelniania za pomocą tokenu aplikacji Power BI Embedded, sposób uzyskiwania raportów
services: power-bi-embedded
documentationcenter: ''
author: dvana
manager: NA
editor: ''
tags: ''

ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 05/16/2016
ms.author: derrickv

---
# Osadzanie raportu usługi Power BI przy użyciu elementu IFrame
W tym artykule opisano podstawowy kod dotyczący usługi **Power BI Embedded** związany z interfejsami API REST, tokenami aplikacji i elementem IFrame, a także fragmenty kodu JavaScript służące do integracji lub osadzania raportu w aplikacji.

Z artykułu [Wprowadzenie do usługi Microsoft Power BI Embedded Preview](power-bi-embedded-get-started.md) dowiesz się, jak skonfigurować **kolekcję obszarów roboczych**, aby pomieścić w niej co najmniej jeden **obszar roboczy** dla zawartości raportu. Następnie z artykułu [Get started with Microsoft Power BI Embedded sample](power-bi-embedded-get-started-sample.md) (Wprowadzenie do przykładu Microsoft Power BI Embedded) nauczysz się, jak zaimportować raport do **obszaru roboczego**.

W tym artykule przedstawiono kroki osadzania raportu w aplikacji. Aby skorzystać z tego artykułu, należy pobrać przykład [integracji raportu z elementem IFrame](https://github.com/Azure-Samples/power-bi-embedded-iframe) z witryny GitHub. Ten przykład to prosta aplikacja formularzy sieci Web ASP.NET, która ma ilustrować podstawowy kod C# i JavaScript konieczny do integracji raportu. Bardziej zaawansowany przykład z użyciem wzorca projektowego MVC (Model-View-Controller) do integracji raportu zawarty jest w [przykładowej aplikacji sieci Web pulpitu nawigacyjnego](http://go.microsoft.com/fwlink/?LinkId=761493) w witrynie GitHub.

Rozpocznijmy opis sposobu integracji raportu **Power BI Embedded** z aplikacją.

Oto kroki integracji raportu.

* Krok 1: [pobranie raportu do obszaru roboczego](#GetReport). W tym kroku użyjesz przepływu tokenu aplikacji w celu uzyskania tokenu dostępu potrzebnego do wywołania operacji REST [Get Reports](https://msdn.microsoft.com/library/mt711510.aspx) (pobierz raporty). Po pobraniu raportu z listy **Get Reports** możesz osadzić raport w aplikacji z elementem **IFrame**.
* Krok 2: [osadzanie raportu w aplikacji](#EmbedReport). W tym kroku użyjesz tokenu osadzania dla raportu oraz fragmentów kodu JavaScript i elementu IFrame w celu integracji lub osadzenia raportu w aplikacji sieci Web.

Jeśli chcesz uruchomić przykład, aby zobaczyć sposób integracji raportu, pobierz przykład [integracji raportu z elementem IFrame](https://github.com/Azure-Samples/power-bi-embedded-iframe) z witryny GitHub i skonfiguruj trzy ustawienia pliku Web.Config:

* **AccessKey**: klucz **AccessKey** służy do generowania tokenu Web JSON (JWT) używanego do pobierania i osadzania raportów. Aby dowiedzieć się, jak uzyskać klucz **AccessKey**, zobacz artykuł [Wprowadzenie do usługi Microsoft Power BI Embedded Preview](power-bi-embedded-get-started.md).
* **WorkspaceName**: aby dowiedzieć się, jak uzyskać nazwę **WorkspaceName**, zobacz artykuł [Wprowadzenie do usługi Microsoft Power BI Embedded Preview](power-bi-embedded-get-started.md).
* **WorkspaceId**: aby dowiedzieć się, jak uzyskać identyfikator **WorkspaceId**, zobacz artykuł [Wprowadzenie do usługi Microsoft Power BI Embedded Preview](power-bi-embedded-get-started.md).

W dalszych częściach pokazano kod potrzebny do integracji raportu.

<a name="GetReport"/>

## Pobieranie raportu do obszaru roboczego
Aby zintegrować raport z aplikacją, potrzebny jest **identyfikator** i adres **embedUrl** raportu. Aby pobrać **identyfikator** i adres **embedUrl** raportu, należy wywołać operację REST [Get Reports](https://msdn.microsoft.com/library/mt711510.aspx) i wybrać raport z listy JSON. W części [Osadzanie raportu w aplikacji](#EmbedReport) użyjesz **identyfikatora** i adresu **embedUrl** raportu do osadzania raportu w swojej aplikacji.

### Odpowiedź JSON na operację Get Reports
```
{
  "@odata.context":"https://api.powerbi.com/beta/collections/{WorkspaceName}/workspaces/{WorkspaceId}/$metadata#reports","value":[
    {
      "id":"804d3664-…-e71882055dba","name":"Import report sample","webUrl":"https://embedded.powerbi.com/reports/804d3664-...-e71882055dba","embedUrl":"https://embedded.powerbi.com/appTokenReportEmbed?reportId=804d3664-...-e71882055dba"
    },{
      "id":"1d7cff58-…-380610e263a9","name":"Sample Report 2","webUrl":"https://embedded.powerbi.com/reports/1d7cff58-...-380610e263a9","embedUrl":"https://embedded.powerbi.com/appTokenReportEmbed?reportId=1d7cff58-...-380610e263a9"
    }
  ]
}

```

Do wywołania operacji REST [Get Reports](https://msdn.microsoft.com/library/mt711510.aspx) trzeba użyć tokenu aplikacji. Aby dowiedzieć się więcej o przepływie tokenu aplikacji, zobacz [Informacje o przepływie tokenu aplikacji w usłudze Power BI Embedded](power-bi-embedded-app-token-flow.md). W poniższym kodzie opisano, jak pobrać listę raportów JSON. Aby osadzić raport, zobacz część [Osadzanie raportu w aplikacji](#EmbedReport).

```
protected void getReportsButton_Click(object sender, EventArgs e)
{
    //Get an app token to generate a JSON Web Token (JWT). An app token flow is a claims-based design pattern.
    //To learn how you can code an app token flow to generate a JWT, see the PowerBIToken class.
    var appToken = PowerBIToken.CreateDevToken(workspaceName, workspaceId);

    //After you get a PowerBIToken which has Claims including your WorkspaceName and WorkspaceID,
    //you generate JSON Web Token (JWT) . The Generate() method uses classes from System.IdentityModel.Tokens: SigningCredentials,
    //JwtSecurityToken, and JwtSecurityTokenHandler.
    string jwt = appToken.Generate(accessKey);

    //Set app token textbox to JWT string to show that the JWT was generated
    appTokenTextbox.Text = jwt;

    //Construct reports uri resource string
    var uri = String.Format("https://api.powerbi.com/beta/collections/{0}/workspaces/{1}/reports", workspaceName, workspaceId);

    //Configure reports request
    System.Net.WebRequest request = System.Net.WebRequest.Create(uri) as System.Net.HttpWebRequest;
    request.Method = "GET";
    request.ContentLength = 0;

    //Set the WebRequest header to AppToken, and jwt
    //Note the use of AppToken instead of Bearer
    request.Headers.Add("Authorization", String.Format("AppToken {0}", jwt));

    //Get reports response from request.GetResponse()
    using (var response = request.GetResponse() as System.Net.HttpWebResponse)
    {
        //Get reader from response stream
        using (var reader = new System.IO.StreamReader(response.GetResponseStream()))
        {
            //Deserialize JSON string into PBIReports
            PBIReports PBIReports = JsonConvert.DeserializeObject<PBIReports>(reader.ReadToEnd());

            //Clear Textbox
            tb_reportsResult.Text = string.Empty;

            //Get each report
            foreach (PBIReport rpt in PBIReports.value)
            {
                tb_reportsResult.Text += String.Format("{0}\t{1}\t{2}\n", rpt.id, rpt.name, rpt.embedUrl);
            }
        }
    }
}
```

<a name="EmbedReport"/>

## Osadzanie raportu w aplikacji
Do osadzenia raportu w aplikacji potrzebny jest token osadzania dla raportu. Token ten jest podobny do tokenu aplikacji używanego do wywołania operacji REST usługi **Power BI Embedded**, ale jest generowany dla zasobu raportu, a nie zasobu REST. Poniżej znajduje się kod służący do pobierania tokenu aplikacji dla raportu. Aby użyć tokenu aplikacji dla raportu, zobacz część [Osadzanie raportu w swojej aplikacji](#EmbedReportJS).

<a name="EmbedReportToken"/>

### Pobieranie tokenu aplikacji dla raportu
```
protected void getReportAppTokenButton_Click(object sender, EventArgs e)
{
    //Get an embed token for a report.
    var token = PowerBIToken.CreateReportEmbedToken(workspaceName, workspaceId, getReportAppTokenTextBox.Text);

    //After you get a PowerBIToken which has Claims including your WorkspaceName and WorkspaceID,
    //you generate JSON Web Token (JWT) . The Generate() method uses classes from System.IdentityModel.Tokens: SigningCredentials,
    //JwtSecurityToken, and JwtSecurityTokenHandler.
    string jwt = token.Generate(accessKey);

    //Set textbox to JWT string. The JavaScript embed code uses the embed jwt for a report.
    reportAppTokenTextbox.Text = jwt;
}
```

<a name="EmbedReportJS"/>

### Osadzanie raportu w swojej aplikacji
Do osadzenia raportu usługi **Power BI** w aplikacji użyjesz elementu IFrame i kodu JavaScript. Oto przykład elementu IFrame i kodu JavaScript do osadzania raportu. Aby zobaczyć cały kod przykładowy służący do osadzania raportu, zobacz przykład [integracji raportu z elementem IFrame](https://github.com/Azure-Samples/power-bi-embedded-iframe) w witrynie GitHub.

![Iframe](media\\power-bi-embedded-integrate-report\\Iframe.png)

```
window.onload = function () {
    // Client side click to embed a selected report.
    var el = document.getElementById("bEmbedReportAction");
    if (el.addEventListener) {
        el.addEventListener("click", updateEmbedReport, false);
    } else {
        el.attachEvent('onclick', updateEmbedReport);
    }

};

// Update embed report
function updateEmbedReport() {
    // Check if the embed url was selected
    var embedUrl = document.getElementById('tb_EmbedURL').value;

    // To load a report do the following:
    // 1: Set the url
    // 2: Add a onload handler to submit the auth token
    var iframe = document.getElementById('iFrameEmbedReport');
    iframe.src = embedUrl;
    iframe.onload = postActionLoadReport;
}

// Post the auth token to the iFrame.
function postActionLoadReport() {

    // Get the app token.
    accessToken = document.getElementById('MainContent_reportAppTokenTextbox').value;

    // Construct the push message structure
    var m = { action: "loadReport", accessToken: accessToken };
    message = JSON.stringify(m);

    // Push the message.
    iframe = document.getElementById('iFrameEmbedReport');
    iframe.contentWindow.postMessage(message, "*");
}
```
Po osadzeniu raportu w aplikacji można go filtrować. W następnej części przedstawiono sposób filtrowania raportu za pomocą składni adresu URL.

## Filtrowanie raportu
Osadzony raport można filtrować przy użyciu składni adresu URL. W tym celu dodaj parametr ciągu zapytania do adresu URL źródła iFrame z odpowiednim filtrem. Możesz **filtrować według wartości** i **ukryć okienko filtru**.

**Filtrowanie według wartości**

Aby filtrować według wartości, należy użyć składni zapytania **$filter** z operatorem **eq** w następujący sposób:

```
https://app.powerbi.com/reportEmbed
?reportId=d2a0ea38-0694-...-ee9655d54a4a&
$filter={tableName/fieldName}%20eq%20'{fieldValue}'
```

Na przykład można filtrować dane, aby w tabeli Store pole Chain było równe „Lindseys”. Część adresu URL zawierająca filtr będzie wyglądać następująco:

```
$filter=Store/Chain%20eq%20'Lindseys'
```

> [!NOTE]
> {tableName/fieldName} nie może zawierać spacji ani znaków specjalnych. {fieldValue} akceptuje pojedyncze wartości kategorii.
> 
> 

**Ukrywanie okienka filtru**

Aby ukryć **Okienko filtru**, możesz dodać parametr **filterPaneEnabled** do ciągu zapytania raportu w następujący sposób:

```
&filterPaneEnabled=false
```

## Podsumowanie
W tym artykule przedstawiono kod służący do integracji raportu usługi **Power BI** w aplikacji. Aby szybko rozpocząć integrowanie raportu w aplikacji, pobierz te przykłady z witryny GitHub:

* [Przykład integracji raportu z elementem IFrame](https://github.com/Azure-Samples/power-bi-embedded-iframe)
* [Przykładowa aplikacja sieci Web pulpitu nawigacyjnego](http://go.microsoft.com/fwlink/?LinkId=761493)

## Zobacz też
* [Wprowadzenie do usługi Microsoft Power BI Embedded Preview](power-bi-embedded-get-started.md)
* [Rozpoczęcie pracy z przykładem](power-bi-embedded-get-started-sample.md)
* [System.IdentityModel.Tokens.SigningCredentials](https://msdn.microsoft.com/library/system.identitymodel.tokens.signingcredentials.aspx)
* [System.IdentityModel.Tokens.JwtSecurityToken](https://msdn.microsoft.com/library/system.identitymodel.tokens.jwtsecuritytoken.aspx)
* [System.IdentityModel.Tokens.JwtSecurityTokenHandler](https://msdn.microsoft.com/library/system.identitymodel.tokens.signingcredentials.aspx)
* [Pobieranie raportów](https://msdn.microsoft.com/library/mt711510.aspx)

<!--HONumber=Jun16_HO2-->


