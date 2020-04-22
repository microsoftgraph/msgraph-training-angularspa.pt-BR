<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="e8f6b-101">Nesta seção, você criará um novo projeto angular.</span><span class="sxs-lookup"><span data-stu-id="e8f6b-101">In this section, you'll create a new Angular project.</span></span>

1. <span data-ttu-id="e8f6b-102">Abra a interface de linha de comando (CLI), navegue até um diretório onde você tem direitos para criar arquivos e execute os seguintes comandos para instalar a ferramenta de [CLI angular](https://www.npmjs.com/package/@angular/cli) e criar um novo aplicativo angular.</span><span class="sxs-lookup"><span data-stu-id="e8f6b-102">Open your command-line interface (CLI), navigate to a directory where you have rights to create files, and run the following commands to install the [Angular CLI](https://www.npmjs.com/package/@angular/cli) tool and create a new Angular app.</span></span>

    ```Shell
    npm install -g @angular/cli@9.0.6
    ng new graph-tutorial
    ```

1. <span data-ttu-id="e8f6b-103">A CLI angular solicitará mais informações.</span><span class="sxs-lookup"><span data-stu-id="e8f6b-103">The Angular CLI will prompt for more information.</span></span> <span data-ttu-id="e8f6b-104">Responda aos prompts da seguinte maneira.</span><span class="sxs-lookup"><span data-stu-id="e8f6b-104">Answer the prompts as follows.</span></span>

    ```Shell
    ? Would you like to add Angular routing? Yes
    ? Which stylesheet format would you like to use? CSS
    ```

1. <span data-ttu-id="e8f6b-105">Quando o comando terminar, mude para o `graph-tutorial` diretório na sua CLI e execute o seguinte comando para iniciar um servidor Web local.</span><span class="sxs-lookup"><span data-stu-id="e8f6b-105">Once the command finishes, change to the `graph-tutorial` directory in your CLI and run the following command to start a local web server.</span></span>

    ```Shell
    ng serve --open
    ```

1. <span data-ttu-id="e8f6b-106">O navegador padrão é aberto [https://localhost:4200/](https://localhost:4200) com uma página angular padrão.</span><span class="sxs-lookup"><span data-stu-id="e8f6b-106">Your default browser opens to [https://localhost:4200/](https://localhost:4200) with a default Angular page.</span></span> <span data-ttu-id="e8f6b-107">Se o navegador não abrir, abra-o e navegue [https://localhost:4200/](https://localhost:4200) até para verificar se o novo aplicativo funciona.</span><span class="sxs-lookup"><span data-stu-id="e8f6b-107">If your browser doesn't open, open it and browse to [https://localhost:4200/](https://localhost:4200) to verify that the new app works.</span></span>

## <a name="add-node-packages"></a><span data-ttu-id="e8f6b-108">Adicionar pacotes de nós</span><span class="sxs-lookup"><span data-stu-id="e8f6b-108">Add Node packages</span></span>

<span data-ttu-id="e8f6b-109">Antes de prosseguir, instale alguns pacotes adicionais que serão usados posteriormente:</span><span class="sxs-lookup"><span data-stu-id="e8f6b-109">Before moving on, install some additional packages that you will use later:</span></span>

- <span data-ttu-id="e8f6b-110">[Bootstrap](https://github.com/twbs/bootstrap) para estilo e componentes comuns.</span><span class="sxs-lookup"><span data-stu-id="e8f6b-110">[bootstrap](https://github.com/twbs/bootstrap) for styling and common components.</span></span>
- <span data-ttu-id="e8f6b-111">[ng-inicialização](https://github.com/ng-bootstrap/ng-bootstrap) para usar componentes de inicialização de angular.</span><span class="sxs-lookup"><span data-stu-id="e8f6b-111">[ng-bootstrap](https://github.com/ng-bootstrap/ng-bootstrap) for using Bootstrap components from Angular.</span></span>
- <span data-ttu-id="e8f6b-112">[angular-fontawesome](https://github.com/FortAwesome/angular-fontawesome) para usar fontawesome ícones em angulares.</span><span class="sxs-lookup"><span data-stu-id="e8f6b-112">[angular-fontawesome](https://github.com/FortAwesome/angular-fontawesome) to use FontAwesome icons in Angular.</span></span>
- <span data-ttu-id="e8f6b-113">[fontawesome-SVG-Core](https://github.com/FortAwesome/Font-Awesome), [Free-regular-SVG-ícones](https://github.com/FortAwesome/Font-Awesome)e os ícones [gratuitos-Solid-SVG-](https://github.com/FortAwesome/Font-Awesome) para os ícones de fontawesome usados no exemplo.</span><span class="sxs-lookup"><span data-stu-id="e8f6b-113">[fontawesome-svg-core](https://github.com/FortAwesome/Font-Awesome), [free-regular-svg-icons](https://github.com/FortAwesome/Font-Awesome), and [free-solid-svg-icons](https://github.com/FortAwesome/Font-Awesome) for the FontAwesome icons used in the sample.</span></span>
- <span data-ttu-id="e8f6b-114">[tempo](https://github.com/moment/moment) para formatar datas e horas.</span><span class="sxs-lookup"><span data-stu-id="e8f6b-114">[moment](https://github.com/moment/moment) for formatting dates and times.</span></span>
- <span data-ttu-id="e8f6b-115">[MSAL-angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) para autenticação no Azure Active Directory e recuperação de tokens de acesso.</span><span class="sxs-lookup"><span data-stu-id="e8f6b-115">[msal-angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) for authenticating to Azure Active Directory and retrieving access tokens.</span></span>
- <span data-ttu-id="e8f6b-116">[Microsoft-Graph-Client](https://github.com/microsoftgraph/msgraph-sdk-javascript) para fazer chamadas para o Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="e8f6b-116">[microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) for making calls to Microsoft Graph.</span></span>

1. <span data-ttu-id="e8f6b-117">Execute os seguintes comandos em sua CLI.</span><span class="sxs-lookup"><span data-stu-id="e8f6b-117">Run the following commands in your CLI.</span></span>

    ```Shell
    npm install bootstrap@4.4.1 @fortawesome/angular-fontawesome@0.6.0 @fortawesome/fontawesome-svg-core@1.2.27
    npm install @fortawesome/free-regular-svg-icons@5.12.1 @fortawesome/free-solid-svg-icons@5.12.1
    npm install moment@2.24.0 moment-timezone@0.5.28 @ng-bootstrap/ng-bootstrap@6.0.0
    npm install msal@1.2.1 @azure/msal-angular@1.0.0-beta.4 @microsoft/microsoft-graph-client@2.0.0
    ```

1. <span data-ttu-id="e8f6b-118">Execute o seguinte comando em sua CLI para adicionar o pacote de localização angular (exigido pela ng-Bootstrap).</span><span class="sxs-lookup"><span data-stu-id="e8f6b-118">Run the following command in your CLI to add the Angular localization package (required by ng-bootstrap).</span></span>

    ```Shell
    ng add @angular/localize
    ```

## <a name="design-the-app"></a><span data-ttu-id="e8f6b-119">Projetar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="e8f6b-119">Design the app</span></span>

<span data-ttu-id="e8f6b-120">Nesta seção, você criará a interface do usuário para o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="e8f6b-120">In this section you'll create the user interface for the app.</span></span>

1. <span data-ttu-id="e8f6b-121">Abra o `./src/styles.css` e adicione as linhas a seguir.</span><span class="sxs-lookup"><span data-stu-id="e8f6b-121">Open the `./src/styles.css` and add the following lines.</span></span>

    :::code language="css" source="../demo/graph-tutorial/src/styles.css":::

1. <span data-ttu-id="e8f6b-122">Adicione os módulos Bootstrap e FontAwesome ao aplicativo.</span><span class="sxs-lookup"><span data-stu-id="e8f6b-122">Add the Bootstrap and FontAwesome modules to the app.</span></span> <span data-ttu-id="e8f6b-123">Abra `./src/app/app.module.ts` e substitua seu conteúdo pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="e8f6b-123">Open `./src/app/app.module.ts` and replace its contents with the following.</span></span>

    ```TypeScript
    import { BrowserModule } from '@angular/platform-browser';
    import { NgModule } from '@angular/core';
    import { NgbModule } from '@ng-bootstrap/ng-bootstrap';
    import { FontAwesomeModule, FaIconLibrary } from '@fortawesome/angular-fontawesome';
    import { faExternalLinkAlt } from '@fortawesome/free-solid-svg-icons';
    import { faUserCircle } from '@fortawesome/free-regular-svg-icons';

    import { AppRoutingModule } from './app-routing.module';
    import { AppComponent } from './app.component';
    import { NavBarComponent } from './nav-bar/nav-bar.component';
    import { HomeComponent } from './home/home.component';
    import { AlertsComponent } from './alerts/alerts.component';

    @NgModule({
      declarations: [
        AppComponent,
        NavBarComponent,
        HomeComponent,
        AlertsComponent
      ],
      imports: [
        BrowserModule,
        AppRoutingModule,
        NgbModule,
        FontAwesomeModule
      ],
      providers: [],
      bootstrap: [AppComponent]
    })
    export class AppModule {
      constructor(library: FaIconLibrary) {
        // Register the FontAwesome icons used by the app
        library.addIcons(faExternalLinkAlt, faUserCircle);
      }
     }
    ```

1. <span data-ttu-id="e8f6b-124">Crie um novo arquivo na `./src/app` pasta chamada `user.ts` e adicione o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="e8f6b-124">Create a new file in the `./src/app` folder named `user.ts` and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/user.ts" id="user":::

1. <span data-ttu-id="e8f6b-125">Gere um componente angular para a navegação superior na página.</span><span class="sxs-lookup"><span data-stu-id="e8f6b-125">Generate an Angular component for the top navigation on the page.</span></span> <span data-ttu-id="e8f6b-126">Na sua CLI, execute o seguinte comando.</span><span class="sxs-lookup"><span data-stu-id="e8f6b-126">In your CLI, run the following command.</span></span>

    ```Shell
    ng generate component nav-bar
    ```

1. <span data-ttu-id="e8f6b-127">Quando o comando for concluído, abra o `./src/app/nav-bar/nav-bar.component.ts` arquivo e substitua seu conteúdo pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="e8f6b-127">Once the command completes, open the `./src/app/nav-bar/nav-bar.component.ts` file and replace its contents with the following.</span></span>

    ```TypeScript
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

1. <span data-ttu-id="e8f6b-128">Abra o `./src/app/nav-bar/nav-bar.component.html` arquivo e substitua seu conteúdo pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="e8f6b-128">Open the `./src/app/nav-bar/nav-bar.component.html` file and replace its contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/src/app/nav-bar/nav-bar.component.html" id="navHtml":::

1. <span data-ttu-id="e8f6b-129">Criar uma home page para o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="e8f6b-129">Create a home page for the app.</span></span> <span data-ttu-id="e8f6b-130">Execute o seguinte comando em sua CLI.</span><span class="sxs-lookup"><span data-stu-id="e8f6b-130">Run the following command in your CLI.</span></span>

    ```Shell
    ng generate component home
    ```

1. <span data-ttu-id="e8f6b-131">Quando o comando for concluído, abra o `./src/app/home/home.component.ts` arquivo e substitua seu conteúdo pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="e8f6b-131">Once the command completes, open the `./src/app/home/home.component.ts` file and replace its contents with the following.</span></span>

    ```TypeScript
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

1. <span data-ttu-id="e8f6b-132">Abra o `./src/app/home/home.component.html` arquivo e substitua seu conteúdo pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="e8f6b-132">Open the `./src/app/home/home.component.html` file and replace its contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/src/app/home/home.component.html" id="homeHtml":::

1. <span data-ttu-id="e8f6b-133">Criar uma classe `Alert` simples.</span><span class="sxs-lookup"><span data-stu-id="e8f6b-133">Create a simple `Alert` class.</span></span> <span data-ttu-id="e8f6b-134">Crie um novo arquivo no `./src/app` diretório chamado `alert.ts` e adicione o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="e8f6b-134">Create a new file in the `./src/app` directory named `alert.ts` and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/alert.ts" id="alert":::

1. <span data-ttu-id="e8f6b-135">Criar um serviço de alerta que o aplicativo pode usar para exibir mensagens para o usuário.</span><span class="sxs-lookup"><span data-stu-id="e8f6b-135">Create an alert service that the app can use to display messages to the user.</span></span> <span data-ttu-id="e8f6b-136">Na sua CLI, execute o seguinte comando.</span><span class="sxs-lookup"><span data-stu-id="e8f6b-136">In your CLI, run the following command.</span></span>

    ```Shell
    ng generate service alerts
    ```

1. <span data-ttu-id="e8f6b-137">Abra o `./src/app/alerts.service.ts` arquivo e substitua seu conteúdo pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="e8f6b-137">Open the `./src/app/alerts.service.ts` file and replace its contents with the following.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/alerts.service.ts" id="alertsService":::

1. <span data-ttu-id="e8f6b-138">Gere um componente de alerta para exibir alertas.</span><span class="sxs-lookup"><span data-stu-id="e8f6b-138">Generate an alerts component to display alerts.</span></span> <span data-ttu-id="e8f6b-139">Na sua CLI, execute o seguinte comando.</span><span class="sxs-lookup"><span data-stu-id="e8f6b-139">In your CLI, run the following command.</span></span>

    ```Shell
    ng generate component alerts
    ```

1. <span data-ttu-id="e8f6b-140">Quando o comando for concluído, abra o `./src/app/alerts/alerts.component.ts` arquivo e substitua seu conteúdo pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="e8f6b-140">Once the command completes, open the `./src/app/alerts/alerts.component.ts` file and replace its contents with the following.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/alerts/alerts.component.ts" id="alertComponent":::

1. <span data-ttu-id="e8f6b-141">Abra o `./src/app/alerts/alerts.component.html` arquivo e substitua seu conteúdo pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="e8f6b-141">Open the `./src/app/alerts/alerts.component.html` file and replace its contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/src/app/alerts/alerts.component.html" id="alertHtml":::

1. <span data-ttu-id="e8f6b-142">Abra o `./src/app/app-routing.module.ts` arquivo e substitua a `const routes: Routes = [];` linha pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="e8f6b-142">Open the `./src/app/app-routing.module.ts` file and replace the `const routes: Routes = [];` line with the following code.</span></span>

    ```typescript
    import { HomeComponent } from './home/home.component';

    const routes: Routes = [
      { path: '', component: HomeComponent },
    ];
    ```

1. <span data-ttu-id="e8f6b-143">Abra o arquivo `./src/app/app.component.html` e substitua o conteúdo inteiro pelo seguinte:</span><span class="sxs-lookup"><span data-stu-id="e8f6b-143">Open the `./src/app/app.component.html` file and replace its entire contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/src/app/app.component.html" id="appHtml":::

<span data-ttu-id="e8f6b-144">Salve todas as suas alterações e atualize a página.</span><span class="sxs-lookup"><span data-stu-id="e8f6b-144">Save all of your changes and refresh the page.</span></span> <span data-ttu-id="e8f6b-145">Agora, o aplicativo deve ser muito diferente.</span><span class="sxs-lookup"><span data-stu-id="e8f6b-145">Now, the app should look very different.</span></span>

![Uma captura de tela da página inicial reprojetada](images/create-app-01.png)
