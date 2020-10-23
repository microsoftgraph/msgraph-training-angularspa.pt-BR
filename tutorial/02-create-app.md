<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="1758b-101">Nesta seção, você criará um novo projeto angular.</span><span class="sxs-lookup"><span data-stu-id="1758b-101">In this section, you'll create a new Angular project.</span></span>

1. <span data-ttu-id="1758b-102">Abra a interface de linha de comando (CLI), navegue até um diretório onde você tem direitos para criar arquivos e execute os seguintes comandos para instalar a ferramenta de [CLI angular](https://www.npmjs.com/package/@angular/cli) e criar um novo aplicativo angular.</span><span class="sxs-lookup"><span data-stu-id="1758b-102">Open your command-line interface (CLI), navigate to a directory where you have rights to create files, and run the following commands to install the [Angular CLI](https://www.npmjs.com/package/@angular/cli) tool and create a new Angular app.</span></span>

    ```Shell
    npm install -g @angular/cli@10.1.7
    ng new graph-tutorial
    ```

1. <span data-ttu-id="1758b-103">A CLI angular solicitará mais informações.</span><span class="sxs-lookup"><span data-stu-id="1758b-103">The Angular CLI will prompt for more information.</span></span> <span data-ttu-id="1758b-104">Responda aos prompts da seguinte maneira.</span><span class="sxs-lookup"><span data-stu-id="1758b-104">Answer the prompts as follows.</span></span>

    ```Shell
    ? Would you like to add Angular routing? Yes
    ? Which stylesheet format would you like to use? CSS
    ```

1. <span data-ttu-id="1758b-105">Quando o comando terminar, mude para o `graph-tutorial` diretório na sua CLI e execute o seguinte comando para iniciar um servidor Web local.</span><span class="sxs-lookup"><span data-stu-id="1758b-105">Once the command finishes, change to the `graph-tutorial` directory in your CLI and run the following command to start a local web server.</span></span>

    ```Shell
    ng serve --open
    ```

1. <span data-ttu-id="1758b-106">O navegador padrão é aberto [https://localhost:4200/](https://localhost:4200) com uma página angular padrão.</span><span class="sxs-lookup"><span data-stu-id="1758b-106">Your default browser opens to [https://localhost:4200/](https://localhost:4200) with a default Angular page.</span></span> <span data-ttu-id="1758b-107">Se o navegador não abrir, abra-o e navegue até [https://localhost:4200/](https://localhost:4200) para verificar se o novo aplicativo funciona.</span><span class="sxs-lookup"><span data-stu-id="1758b-107">If your browser doesn't open, open it and browse to [https://localhost:4200/](https://localhost:4200) to verify that the new app works.</span></span>

## <a name="add-node-packages"></a><span data-ttu-id="1758b-108">Adicionar pacotes de nós</span><span class="sxs-lookup"><span data-stu-id="1758b-108">Add Node packages</span></span>

<span data-ttu-id="1758b-109">Antes de prosseguir, instale alguns pacotes adicionais que serão usados posteriormente:</span><span class="sxs-lookup"><span data-stu-id="1758b-109">Before moving on, install some additional packages that you will use later:</span></span>

- <span data-ttu-id="1758b-110">[Bootstrap](https://github.com/twbs/bootstrap) para estilo e componentes comuns.</span><span class="sxs-lookup"><span data-stu-id="1758b-110">[bootstrap](https://github.com/twbs/bootstrap) for styling and common components.</span></span>
- <span data-ttu-id="1758b-111">[ng-inicialização](https://github.com/ng-bootstrap/ng-bootstrap) para usar componentes de inicialização de angular.</span><span class="sxs-lookup"><span data-stu-id="1758b-111">[ng-bootstrap](https://github.com/ng-bootstrap/ng-bootstrap) for using Bootstrap components from Angular.</span></span>
- <span data-ttu-id="1758b-112">[tempo](https://github.com/moment/moment) para formatar datas e horas.</span><span class="sxs-lookup"><span data-stu-id="1758b-112">[moment](https://github.com/moment/moment) for formatting dates and times.</span></span>
- [<span data-ttu-id="1758b-113">Windows-IANA</span><span class="sxs-lookup"><span data-stu-id="1758b-113">windows-iana</span></span>](https://github.com/rubenillodo/windows-iana)
- <span data-ttu-id="1758b-114">[MSAL-angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) para autenticação no Azure Active Directory e recuperação de tokens de acesso.</span><span class="sxs-lookup"><span data-stu-id="1758b-114">[msal-angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) for authenticating to Azure Active Directory and retrieving access tokens.</span></span>
- <span data-ttu-id="1758b-115">[Microsoft-Graph-Client](https://github.com/microsoftgraph/msgraph-sdk-javascript) para fazer chamadas para o Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="1758b-115">[microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) for making calls to Microsoft Graph.</span></span>

1. <span data-ttu-id="1758b-116">Execute os seguintes comandos em sua CLI.</span><span class="sxs-lookup"><span data-stu-id="1758b-116">Run the following commands in your CLI.</span></span>

    ```Shell
    npm install bootstrap@4.5.3 @ng-bootstrap/ng-bootstrap@7.0.0 msal@1.4.2 @azure/msal-angular@1.1.1
    npm install moment@2.29.1 moment-timezone@0.5.31 windows-iana@4.2.1
    npm install @microsoft/microsoft-graph-client@2.1.0 @microsoft/microsoft-graph-types@1.24.0
    ```

1. <span data-ttu-id="1758b-117">Execute o seguinte comando em sua CLI para adicionar o pacote de localização angular (exigido pela ng-Bootstrap).</span><span class="sxs-lookup"><span data-stu-id="1758b-117">Run the following command in your CLI to add the Angular localization package (required by ng-bootstrap).</span></span>

    ```Shell
    ng add @angular/localize
    ```

## <a name="design-the-app"></a><span data-ttu-id="1758b-118">Projetar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="1758b-118">Design the app</span></span>

<span data-ttu-id="1758b-119">Nesta seção, você criará a interface do usuário para o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1758b-119">In this section you'll create the user interface for the app.</span></span>

1. <span data-ttu-id="1758b-120">Abra **./src/Styles.css** e adicione as linhas a seguir.</span><span class="sxs-lookup"><span data-stu-id="1758b-120">Open **./src/styles.css** and add the following lines.</span></span>

    :::code language="css" source="../demo/graph-tutorial/src/styles.css":::

1. <span data-ttu-id="1758b-121">Adicione o módulo Bootstrap ao aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1758b-121">Add the Bootstrap module to the app.</span></span> <span data-ttu-id="1758b-122">Abra **./src/app/app.Module.TS** e substitua seu conteúdo pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="1758b-122">Open **./src/app/app.module.ts** and replace its contents with the following.</span></span>

    ```typescript
    import { BrowserModule } from '@angular/platform-browser';
    import { FormsModule } from '@angular/forms';
    import { NgModule } from '@angular/core';
    import { NgbModule } from '@ng-bootstrap/ng-bootstrap';

    import { AppRoutingModule } from './app-routing.module';
    import { AppComponent } from './app.component';

    @NgModule({
      declarations: [
        AppComponent
      ],
      imports: [
        BrowserModule,
        FormsModule,
        AppRoutingModule,
        NgbModule
      ],
      providers: [],
      bootstrap: [AppComponent]
    })
    export class AppModule { }
    ```

1. <span data-ttu-id="1758b-123">Crie um novo arquivo na pasta **./src/app** chamado **User. TS** e adicione o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="1758b-123">Create a new file in the **./src/app** folder named **user.ts** and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/user.ts" id="user":::

1. <span data-ttu-id="1758b-124">Gere um componente angular para a navegação superior na página.</span><span class="sxs-lookup"><span data-stu-id="1758b-124">Generate an Angular component for the top navigation on the page.</span></span> <span data-ttu-id="1758b-125">Na sua CLI, execute o seguinte comando.</span><span class="sxs-lookup"><span data-stu-id="1758b-125">In your CLI, run the following command.</span></span>

    ```Shell
    ng generate component nav-bar
    ```

1. <span data-ttu-id="1758b-126">Quando o comando for concluído, abra **./src/app/NAV-bar/NAV-bar.Component.TS** e substitua o conteúdo pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="1758b-126">Once the command completes, open **./src/app/nav-bar/nav-bar.component.ts** and replace its contents with the following.</span></span>

    ```typescript
    import { Component, OnInit } from '@angular/core';

    import { User } from '../user';

    @Component({
      selector: 'app-nav-bar',
      templateUrl: './nav-bar.component.html',
      styleUrls: ['./nav-bar.component.css']
    })
    export class NavBarComponent implements OnInit {

      // Should the collapsed nav show?
      showNav: boolean;
      // Is a user logged in?
      authenticated: boolean;
      // The user
      user: User;

      constructor() { }

      ngOnInit() {
        this.showNav = false;
        this.authenticated = false;
        this.user = null;
      }

      // Used by the Bootstrap navbar-toggler button to hide/show
      // the nav in a collapsed state
      toggleNavBar(): void {
        this.showNav = !this.showNav;
      }

      signIn(): void {
        // Temporary
        this.authenticated = true;
        this.user = {
          displayName: 'Adele Vance',
          email: 'AdeleV@contoso.com',
          avatar: null
        };
      }

      signOut(): void {
        // Temporary
        this.authenticated = false;
        this.user = null;
      }
    }
    ```

1. <span data-ttu-id="1758b-127">Abra **./src/app/nav-bar/nav-bar.component.html** e substitua seu conteúdo pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="1758b-127">Open **./src/app/nav-bar/nav-bar.component.html** and replace its contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/src/app/nav-bar/nav-bar.component.html" id="navHtml":::

1. <span data-ttu-id="1758b-128">Criar uma home page para o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1758b-128">Create a home page for the app.</span></span> <span data-ttu-id="1758b-129">Execute o seguinte comando em sua CLI.</span><span class="sxs-lookup"><span data-stu-id="1758b-129">Run the following command in your CLI.</span></span>

    ```Shell
    ng generate component home
    ```

1. <span data-ttu-id="1758b-130">Quando o comando for concluído, abra **./src/app/Home/Home.Component.TS** e substitua o conteúdo pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="1758b-130">Once the command completes, open **./src/app/home/home.component.ts** and replace its contents with the following.</span></span>

    ```typescript
    import { Component, OnInit } from '@angular/core';

    import { User } from '../user';

    @Component({
      selector: 'app-home',
      templateUrl: './home.component.html',
      styleUrls: ['./home.component.css']
    })
    export class HomeComponent implements OnInit {

      // Is a user logged in?
      authenticated: boolean;
      // The user
      user: any;

      constructor() { }

      ngOnInit() {
        this.authenticated = false;
        this.user = {};
      }

      signIn(): void {
        // Temporary
        this.authenticated = true;
        this.user = {
          displayName: 'Adele Vance',
          email: 'AdeleV@contoso.com'
        };
      }
    }
    ```

1. <span data-ttu-id="1758b-131">Abra **./src/app/home/home.component.html** e substitua seu conteúdo pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="1758b-131">Open **./src/app/home/home.component.html** and replace its contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/src/app/home/home.component.html" id="homeHtml":::

1. <span data-ttu-id="1758b-132">Criar uma `Alert` classe simples.</span><span class="sxs-lookup"><span data-stu-id="1758b-132">Create a simple `Alert` class.</span></span> <span data-ttu-id="1758b-133">Crie um novo arquivo no diretório **./src/app** chamado **Alert. TS** e adicione o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="1758b-133">Create a new file in the **./src/app** directory named **alert.ts** and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/alert.ts" id="alert":::

1. <span data-ttu-id="1758b-134">Criar um serviço de alerta que o aplicativo pode usar para exibir mensagens para o usuário.</span><span class="sxs-lookup"><span data-stu-id="1758b-134">Create an alert service that the app can use to display messages to the user.</span></span> <span data-ttu-id="1758b-135">Na sua CLI, execute o seguinte comando.</span><span class="sxs-lookup"><span data-stu-id="1758b-135">In your CLI, run the following command.</span></span>

    ```Shell
    ng generate service alerts
    ```

1. <span data-ttu-id="1758b-136">Abra **./src/app/Alerts.Service.TS** e substitua seu conteúdo pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="1758b-136">Open **./src/app/alerts.service.ts** and replace its contents with the following.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/alerts.service.ts" id="alertsService":::

1. <span data-ttu-id="1758b-137">Gere um componente de alerta para exibir alertas.</span><span class="sxs-lookup"><span data-stu-id="1758b-137">Generate an alerts component to display alerts.</span></span> <span data-ttu-id="1758b-138">Na sua CLI, execute o seguinte comando.</span><span class="sxs-lookup"><span data-stu-id="1758b-138">In your CLI, run the following command.</span></span>

    ```Shell
    ng generate component alerts
    ```

1. <span data-ttu-id="1758b-139">Quando o comando for concluído, abra **./src/app/Alerts/Alerts.Component.TS** e substitua o conteúdo pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="1758b-139">Once the command completes, open **./src/app/alerts/alerts.component.ts** and replace its contents with the following.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/alerts/alerts.component.ts" id="alertComponent":::

1. <span data-ttu-id="1758b-140">Abra **./src/app/alerts/alerts.component.html** e substitua seu conteúdo pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="1758b-140">Open **./src/app/alerts/alerts.component.html** and replace its contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/src/app/alerts/alerts.component.html" id="alertHtml":::

1. <span data-ttu-id="1758b-141">Abra **./src/app/app-Routing.Module.TS** e substitua a `const routes: Routes = [];` linha pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="1758b-141">Open **./src/app/app-routing.module.ts** and replace the `const routes: Routes = [];` line with the following code.</span></span>

    ```typescript
    import { HomeComponent } from './home/home.component';

    const routes: Routes = [
      { path: '', component: HomeComponent },
    ];
    ```

1. <span data-ttu-id="1758b-142">Abra **./src/app/app.component.html** e substitua todo o seu conteúdo pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="1758b-142">Open **./src/app/app.component.html** and replace its entire contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/src/app/app.component.html" id="appHtml":::

1. <span data-ttu-id="1758b-143">Adicione um arquivo de imagem de sua escolha nomeada **no-profile-photo.png** no diretório **./src/assets** .</span><span class="sxs-lookup"><span data-stu-id="1758b-143">Add an image file of your choosing named **no-profile-photo.png** in the **./src/assets** directory.</span></span> <span data-ttu-id="1758b-144">Esta imagem será usada como foto do usuário quando o usuário não tiver nenhuma foto no Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="1758b-144">This image will be used as the user's photo when the user has no photo in Microsoft Graph.</span></span>

<span data-ttu-id="1758b-145">Salve todas as suas alterações e atualize a página.</span><span class="sxs-lookup"><span data-stu-id="1758b-145">Save all of your changes and refresh the page.</span></span> <span data-ttu-id="1758b-146">Agora, o aplicativo deve ser muito diferente.</span><span class="sxs-lookup"><span data-stu-id="1758b-146">Now, the app should look very different.</span></span>

![Uma captura de tela da página inicial reprojetada](images/create-app-01.png)
