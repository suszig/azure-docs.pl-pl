<properties 
    pageTitle="Azure Active Directory と Visual Studio 接続済みサービスの概要 (MVC プロジェクト)" 
    description="Visual Studio 接続済みサービスを使用して Azure Active Directory を接続または作成した後に、MVC プロジェクトで Azure AD の使用を開始する方法について説明します。" 
    services="active-directory" 
    documentationCenter="" 
    authors="TomArcher" 
    manager="douge" 
    editor="tglee"/>

<tags 
    ms.service="active-directory" 
    ms.workload="web" 
    ms.tgt_pltfrm="vs-getting-started" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/03/2015" 
    ms.author="tarcher"/>


# Azure Active Directory と Visual Studio 接続済みサービスの概要 (MVC プロジェクト)

> [AZURE.SELECTOR]
> - [作業の開始](vs-active-directory-dotnet-getting-started.md)
> - [変更内容](vs-active-directory-dotnet-what-happened.md)

## コントローラーへのアクセスに対して認証を要求する

プロジェクトに含まれるすべてのコントローラーには、**Authorize** 属性が設定されています。 この属性により、ユーザーがこれらのコントローラーにアクセスする際に認証が求められます。 これらのコントローラーに匿名でアクセスできるようにするには、コントローラーからこの属性を削除します。 より細かなレベルでアクセス許可を設定するには、コントローラー クラスではなく、認証を必要とするそれぞれのメソッドに対してこの属性を割り当てます。

## SignIn/SignOut コントロールを追加する

追加する、使用するビューに Signin/signout コントロールを **_LoginPartial.cshtml** 部分ビューをビューのいずれかの機能を追加します。 標準に追加される機能の例を次に示します **_Layout.cshtml** 表示します。 (div class="navbar-collapse collapse" の最後の要素にご注目ください):

<pre>
            
    <!DOCTYPE html>
    &lt;html&gt;
    &lt;head&gt;
        <meta charset="utf-8" />
       <meta name="viewport" content="width=device-width, initial-scale=1.0">
       &lt;title&gt;@ViewBag.Title - My ASP.NET Application</title>
       @Styles.Render("~/Content/css")
       @Scripts.Render("~/bundles/modernizr")
   </head>
   &lt;body&gt;
       <div class="navbar navbar-inverse navbar-fixed-top">
           <div class="container">
               <div class="navbar-header">
                   <button type="button" class="navbar-toggle" data-toggle="collapse" data-target=".navbar-collapse">
                       <span class="icon-bar"></span>
                       <span class="icon-bar"></span>
                       <span class="icon-bar"></span>
                   </button>
                   @Html.ActionLink("Application name", "Index", "Home", new { area = "" }, new { @class = "navbar-brand" })
               </div>
               <div class="navbar-collapse collapse">
                   <ul class="nav navbar-nav">
                       &lt;li&gt;@Html.ActionLink("Home", "Index", "Home")</li>
                       &lt;li&gt;@Html.ActionLink("About", "About", "Home")</li>
                       &lt;li&gt;@Html.ActionLink("Contact", "Contact", "Home")</li>
                   </ul> 
                    
            <span style="background-color:yellow">@Html.Partial("_LoginPartial")</span>
             
                </div>
           </div>
       </div>
       <div class="container body-content">
           @RenderBody()
           <hr />
           &lt;footer&gt;
               &lt;p&gt;&copy; @DateTime.Now.Year - My ASP.NET Application</p>
           </footer>
       </div>
       @Scripts.Render("~/bundles/jquery")
       @Scripts.Render("~/bundles/bootstrap")
       @RenderSection("scripts", required: false)
   </body>
   </html>
          </pre>

[Azure Active Directory の詳細についてください。](http://azure.microsoft.com/services/active-directory/)





