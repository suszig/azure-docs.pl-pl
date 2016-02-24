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
> - [Getting Started (概要)](vs-active-directory-dotnet-getting-started.md)
> - [変更内容](vs-active-directory-dotnet-what-happened.md)
 
##コントローラーへのアクセスに対して認証を要求する 

プロジェクト内のすべてのコント ローラーが修飾された、 **Authorize** 属性です。 この属性により、ユーザーがこれらのコントローラーにアクセスする際に認証が求められます。 これらのコントローラーに匿名でアクセスできるようにするには、コントローラーからこの属性を削除します。 より細かなレベルでアクセス許可を設定するには、コントローラー クラスではなく、認証を必要とするそれぞれのメソッドに対してこの属性を割り当てます。
 
##SignIn/SignOut コントロールを追加する 

追加する、使用するビューに Signin/signout コントロールを **_LoginPartial.cshtml** 部分ビューをビューのいずれかの機能を追加します。 標準に追加される機能の例を次に示します **_Layout.cshtml** 表示します。 (div class="navbar-collapse collapse" の最後の要素にご注目ください):

<pre>
    &lt;!DOCTYPE html&gt; 
     &lt;html&gt; 
     &lt;head&gt; 
         &lt;meta charset="utf-8" /&gt; 
        &lt;meta name="viewport" content="width=device-width, initial-scale=1.0"&gt; 
        &lt;title&gt;@ViewBag.Title - My ASP.NET Application&lt;/title&gt; 
        @Styles.Render("~/Content/css") 
        @Scripts.Render("~/bundles/modernizr") 
    &lt;/head&gt; 
    &lt;body&gt; 
        &lt;div class="navbar navbar-inverse navbar-fixed-top"&gt; 
            &lt;div class="container"&gt; 
                &lt;div class="navbar-header"&gt; 
                    &lt;button type="button" class="navbar-toggle" data-toggle="collapse" data-target=".navbar-collapse"&gt; 
                        &lt;span class="icon-bar"&gt;&lt;/span&gt; 
                        &lt;span class="icon-bar"&gt;&lt;/span&gt; 
                        &lt;span class="icon-bar"&gt;&lt;/span&gt; 
                    &lt;/button&gt; 
                    @Html.ActionLink("Application name", "Index", "Home", new { area = "" }, new { @class = "navbar-brand" }) 
                &lt;/div&gt; 
                &lt;div class="navbar-collapse collapse"&gt; 
                    &lt;ul class="nav navbar-nav"&gt; 
                        &lt;li&gt;@Html.ActionLink("Home", "Index", "Home")&lt;/li&gt; 
                        &lt;li&gt;@Html.ActionLink("About", "About", "Home")&lt;/li&gt; 
                        &lt;li&gt;@Html.ActionLink("Contact", "Contact", "Home")&lt;/li&gt; 
                    &lt;/ul&gt; 
                    <span style="background-color:yellow">@Html.Partial("_LoginPartial")</span> 
                &lt;/div&gt; 
            &lt;/div&gt; 
        &lt;/div&gt; 
        &lt;div class="container body-content"&gt; 
            @RenderBody() 
            &lt;hr /&gt; 
            &lt;footer&gt; 
                &lt;p&gt;&amp;copy; @DateTime.Now.Year - My ASP.NET Application&lt;/p&gt; 
            &lt;/footer&gt; 
        &lt;/div&gt; 
        @Scripts.Render("~/bundles/jquery") 
        @Scripts.Render("~/bundles/bootstrap") 
        @RenderSection("scripts", required: false) 
    &lt;/body&gt; 
    &lt;/html&gt;
</pre>

[Azure Active Directory の詳細を確認する](http://azure.microsoft.com/services/active-directory/) 

