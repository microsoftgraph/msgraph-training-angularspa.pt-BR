<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="5da13-101">Abra a interface de linha de comando (CLI), navegue até um diretório onde você tem direitos para criar arquivos e execute os seguintes comandos para instalar a ferramenta de [CLI angular](https://www.npmjs.com/package/@angular/cli) e criar um novo aplicativo angular.</span><span class="sxs-lookup"><span data-stu-id="5da13-101">Open your command-line interface (CLI), navigate to a directory where you have rights to create files, and run the following commands to install the [Angular CLI](https://www.npmjs.com/package/@angular/cli) tool and create a new Angular app.</span></span>

```Shell
npm install -g @angular/cli
ng new graph-tutorial
```

<span data-ttu-id="5da13-102">A CLI angular solicitará mais informações.</span><span class="sxs-lookup"><span data-stu-id="5da13-102">The Angular CLI will prompt for more information.</span></span> <span data-ttu-id="5da13-103">Responda aos prompts da seguinte maneira.</span><span class="sxs-lookup"><span data-stu-id="5da13-103">Answer the prompts as follows.</span></span>

```Shell
? Would you like to add Angular routing? Yes
? Which stylesheet format would you like to use? CSS
```

<span data-ttu-id="5da13-104">Quando o comando terminar, mude para o `graph-tutorial` diretório na sua CLI e execute o seguinte comando para iniciar um servidor Web local.</span><span class="sxs-lookup"><span data-stu-id="5da13-104">Once the command finishes, change to the `graph-tutorial` directory in your CLI and run the following command to start a local web server.</span></span>

```Shell
ng serve --open
```

<span data-ttu-id="5da13-105">O navegador padrão é aberto [https://localhost:4200/](https://localhost:4200) com uma página angular padrão.</span><span class="sxs-lookup"><span data-stu-id="5da13-105">Your default browser opens to [https://localhost:4200/](https://localhost:4200) with a default Angular page.</span></span> <span data-ttu-id="5da13-106">Se o navegador não abrir, abra-o e navegue [https://localhost:4200/](https://localhost:4200) até para verificar se o novo aplicativo funciona.</span><span class="sxs-lookup"><span data-stu-id="5da13-106">If your browser doesn't open, open it and browse to [https://localhost:4200/](https://localhost:4200) to verify that the new app works.</span></span>

<span data-ttu-id="5da13-107">Antes de prosseguir, instale alguns pacotes adicionais que serão usados posteriormente:</span><span class="sxs-lookup"><span data-stu-id="5da13-107">Before moving on, install some additional packages that you will use later:</span></span>

- <span data-ttu-id="5da13-108">[Bootstrap](https://github.com/twbs/bootstrap) para estilo e componentes comuns.</span><span class="sxs-lookup"><span data-stu-id="5da13-108">[bootstrap](https://github.com/twbs/bootstrap) for styling and common components.</span></span>
- <span data-ttu-id="5da13-109">[ng-inicialização](https://github.com/ng-bootstrap/ng-bootstrap) para usar componentes de inicialização de angular.</span><span class="sxs-lookup"><span data-stu-id="5da13-109">[ng-bootstrap](https://github.com/ng-bootstrap/ng-bootstrap) for using Bootstrap components from Angular.</span></span>
- <span data-ttu-id="5da13-110">[angular-fontawesome](https://github.com/FortAwesome/angular-fontawesome) para usar fontawesome ícones em angulares.</span><span class="sxs-lookup"><span data-stu-id="5da13-110">[angular-fontawesome](https://github.com/FortAwesome/angular-fontawesome) to use FontAwesome icons in Angular.</span></span>
- <span data-ttu-id="5da13-111">[fontawesome-SVG-Core](https://github.com/FortAwesome/Font-Awesome), [Free-regular-SVG-ícones](https://github.com/FortAwesome/Font-Awesome)e os ícones [gratuitos-Solid-SVG-](https://github.com/FortAwesome/Font-Awesome) para os ícones de fontawesome usados no exemplo.</span><span class="sxs-lookup"><span data-stu-id="5da13-111">[fontawesome-svg-core](https://github.com/FortAwesome/Font-Awesome), [free-regular-svg-icons](https://github.com/FortAwesome/Font-Awesome), and [free-solid-svg-icons](https://github.com/FortAwesome/Font-Awesome) for the FontAwesome icons used in the sample.</span></span>
- <span data-ttu-id="5da13-112">[tempo](https://github.com/moment/moment) para formatar datas e horas.</span><span class="sxs-lookup"><span data-stu-id="5da13-112">[moment](https://github.com/moment/moment) for formatting dates and times.</span></span>
- <span data-ttu-id="5da13-113">[MSAL-angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) para autenticação no Azure Active Directory e recuperação de tokens de acesso.</span><span class="sxs-lookup"><span data-stu-id="5da13-113">[msal-angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) for authenticating to Azure Active Directory and retrieving access tokens.</span></span>
- <span data-ttu-id="5da13-114">[rxjs-compat](https://github.com/ReactiveX/rxjs/tree/master/compat), obrigatório para o `msal-angular` pacote.</span><span class="sxs-lookup"><span data-stu-id="5da13-114">[rxjs-compat](https://github.com/ReactiveX/rxjs/tree/master/compat), required for the `msal-angular` package.</span></span>
- <span data-ttu-id="5da13-115">[Microsoft-Graph-Client](https://github.com/microsoftgraph/msgraph-sdk-javascript) para fazer chamadas para o Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="5da13-115">[microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) for making calls to Microsoft Graph.</span></span>

<span data-ttu-id="5da13-116">Execute o seguinte comando em sua CLI.</span><span class="sxs-lookup"><span data-stu-id="5da13-116">Run the following command in your CLI.</span></span>

```Shell
npm install bootstrap@4.3.1 @fortawesome/angular-fontawesome@0.3.0 @fortawesome/fontawesome-svg-core@1.2.15
npm install @fortawesome/free-regular-svg-icons@5.7.2 @fortawesome/free-solid-svg-icons@5.7.2
npm install moment@2.24.0 moment-timezone@0.5.23 @ng-bootstrap/ng-bootstrap@4.1.0
npm install @azure/msal-angular@0.1.2 rxjs-compat@6.4.0 @microsoft/microsoft-graph-client@1.4.0
```

## <a name="design-the-app"></a><span data-ttu-id="5da13-117">Projetar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="5da13-117">Design the app</span></span>

<span data-ttu-id="5da13-118">Comece adicionando os arquivos CSS de inicialização ao aplicativo, bem como alguns estilos globais.</span><span class="sxs-lookup"><span data-stu-id="5da13-118">Start by adding the Bootstrap CSS files to the app, as well as some global styles.</span></span> <span data-ttu-id="5da13-119">Abra o `./src/styles.css` e adicione as linhas a seguir.</span><span class="sxs-lookup"><span data-stu-id="5da13-119">Open the `./src/styles.css` and add the following lines.</span></span>

```CSS
@import "~bootstrap/dist/css/bootstrap.css";

/* Add padding for the nav bar */
body {
  padding-top: 4.5rem;
}

/* Style debug info in alerts */
.alert-pre {
  word-wrap: break-word;
  word-break: break-all;
  white-space: pre-wrap;
}
```

<span data-ttu-id="5da13-120">Em seguida, adicione os módulos Bootstrap e FontAwesome ao aplicativo.</span><span class="sxs-lookup"><span data-stu-id="5da13-120">Next, add the Bootstrap and FontAwesome modules to the app.</span></span> <span data-ttu-id="5da13-121">Abra `./src/app/app.module.ts` e adicione as seguintes `import` instruções à parte superior do arquivo.</span><span class="sxs-lookup"><span data-stu-id="5da13-121">Open `./src/app/app.module.ts` and add the following `import` statements to the top of the file.</span></span>

```TypeScript
import { NgbModule } from '@ng-bootstrap/ng-bootstrap';
import { FontAwesomeModule } from '@fortawesome/angular-fontawesome';
import { library } from '@fortawesome/fontawesome-svg-core';
import { faExternalLinkAlt } from '@fortawesome/free-solid-svg-icons';
import { faUserCircle } from '@fortawesome/free-regular-svg-icons';
```

<span data-ttu-id="5da13-122">Em seguida, adicione o seguinte código após todas `import` as instruções.</span><span class="sxs-lookup"><span data-stu-id="5da13-122">Then add the following code after all of the `import` statements.</span></span>

```TypeScript
library.add(faExternalLinkAlt);
library.add(faUserCircle);
```

<span data-ttu-id="5da13-123">Na `@NgModule` declaração, substitua a matriz existente `imports` pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="5da13-123">In the `@NgModule` declaration, replace the existing `imports` array with the following.</span></span>

```TypeScript
imports: [
  BrowserModule,
  AppRoutingModule,
  NgbModule,
  FontAwesomeModule
]
```

<span data-ttu-id="5da13-124">Agora, gere um componente angular para a navegação superior na página.</span><span class="sxs-lookup"><span data-stu-id="5da13-124">Now generate an Angular component for the top navigation on the page.</span></span> <span data-ttu-id="5da13-125">Na sua CLI, execute o seguinte comando.</span><span class="sxs-lookup"><span data-stu-id="5da13-125">In your CLI, run the following command.</span></span>

```Shell
ng generate component nav-bar
```

<span data-ttu-id="5da13-126">Quando o comando for concluído, abra o `./src/app/nav-bar/nav-bar.component.ts` arquivo e substitua seu conteúdo pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="5da13-126">Once the command completes, open the `./src/app/nav-bar/nav-bar.component.ts` file and replace its contents with the following.</span></span>

```TypeScript
import { Component, OnInit } from '@angular/core';

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
  user: any;

  constructor() { }

  ngOnInit() {
    this.showNav = false;
    this.authenticated = false;
    this.user = {};
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
      email: 'AdeleV@contoso.com'
    };
  }

  signOut(): void {
    // Temporary
    this.authenticated = false;
    this.user = {};
  }
}
```

<span data-ttu-id="5da13-127">Abra o `./src/app/nav-bar/nav-bar.component.html` arquivo e substitua seu conteúdo pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="5da13-127">Open the `./src/app/nav-bar/nav-bar.component.html` file and replace its contents with the following.</span></span>

```html
<nav class="navbar navbar-expand-md navbar-dark fixed-top bg-dark">
  <div class="container">
    <a routerLink="/" class="navbar-brand">Angular Graph Tutorial</a>
    <button class="navbar-toggler" type="button" (click)="toggleNavBar()" [attr.aria-expanded]="showNav"
    aria-controls="navbarCollapse" aria-expanded="false" aria-label="Toggle navigation">
      <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" [class.show]="showNav" id="navbarCollapse">
      <ul class="navbar-nav mr-auto">
        <li class="nav-item">
          <a routerLink="/" class="nav-link" routerLinkActive="active">Home</a>
        </li>
        <li *ngIf="authenticated" class="nav-item">
          <a routerLink="/calendar" class="nav-link" routerLinkActive="active">Calendar</a>
        </li>
      </ul>
      <ul class="navbar-nav justify-content-end">
        <li class="nav-item">
          <a class="nav-link" href="https://docs.microsoft.com/graph/overview" target="_blank">
            <fa-icon [icon]="[ 'fas', 'external-link-alt' ]" class="mr-1"></fa-icon>Docs
          </a>
        </li>
        <li *ngIf="authenticated" ngbDropdown placement="bottom-right" class="nav-item">
          <a ngbDropdownToggle id="userMenu" class="nav-link" href="javascript:undefined" role="button" aria-haspopup="true"
            aria-expanded="false">
            <div *ngIf="user.avatar; then userAvatar else defaultAvatar"></div>
            <ng-template #userAvatar>
              <img src="user.avatar" class="rounded-circle align-self-center mr-2" style="width: 32px;">
            </ng-template>
            <ng-template #defaultAvatar>
              <fa-icon [icon]="[ 'far', 'user-circle' ]" fixedWidth="true" size="lg"
                class="rounded-circle align-self-center mr-2"></fa-icon>
            </ng-template>
          </a>
          <div ngbDropdownMenu aria-labelledby="userMenu">
            <h5 class="dropdown-item-text mb-0">{{user.displayName}}</h5>
            <p class="dropdown-item-text text-muted mb-0">{{user.email}}</p>
            <div class="dropdown-divider"></div>
            <a class="dropdown-item" href="javascript:undefined" role="button" (click)="signOut()">Sign Out</a>
          </div>
        </li>
        <li *ngIf="!authenticated" class="nav-item">
          <a class="nav-link" href="javascript:undefined" role="button" (click)="signIn()">Sign In</a>
        </li>
      </ul>
    </div>
  </div>
</nav>
```

<span data-ttu-id="5da13-128">Em seguida, crie uma home page para o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="5da13-128">Next, create a home page for the app.</span></span> <span data-ttu-id="5da13-129">Execute o seguinte comando em sua CLI.</span><span class="sxs-lookup"><span data-stu-id="5da13-129">Run the following command in your CLI.</span></span>

```Shell
ng generate component home
```

<span data-ttu-id="5da13-130">Quando o comando for concluído, abra o `./src/app/home/home.component.ts` arquivo e substitua seu conteúdo pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="5da13-130">Once the command completes, open the `./src/app/home/home.component.ts` file and replace its contents with the following.</span></span>

```TypeScript
import { Component, OnInit } from '@angular/core';

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

<span data-ttu-id="5da13-131">Em seguida, `./src/app/home/home.component.html` Abra o arquivo e substitua seu conteúdo pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="5da13-131">Then open the `./src/app/home/home.component.html` file and replace its contents with the following.</span></span>

```html
<div class="jumbotron">
  <h1>Angular Graph Tutorial</h1>
  <p class="lead">This sample app shows how to use the Microsoft Graph API from Angular</p>
  <div *ngIf="authenticated; then welcomeUser else signInPrompt"></div>
  <ng-template #welcomeUser>
    <h4>Welcome {{ user.displayName }}!</h4>
    <p>Use the navigation bar at the top of the page to get started.</p>
  </ng-template>
  <ng-template #signInPrompt>
    <a href="javascript:undefined" class="btn btn-primary btn-large" role="button" (click)="signIn()">Click here to sign in</a>
  </ng-template>
</div>
```

<span data-ttu-id="5da13-132">Agora, crie um serviço de alerta que o aplicativo pode usar para exibir mensagens para o usuário.</span><span class="sxs-lookup"><span data-stu-id="5da13-132">Now create an alert service that the app can use to display messages to the user.</span></span> <span data-ttu-id="5da13-133">Comece criando uma classe simples `Alert` .</span><span class="sxs-lookup"><span data-stu-id="5da13-133">Start by creating a simple `Alert` class.</span></span> <span data-ttu-id="5da13-134">Crie um novo arquivo no `./src/app` diretório chamado `alert.ts` e adicione o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="5da13-134">Create a new file in the `./src/app` directory named `alert.ts` and add the following code.</span></span>

```TypeScript
export class Alert {
  message: string;
  debug: string;
}
```

<span data-ttu-id="5da13-135">Na sua CLI, execute o seguinte comando.</span><span class="sxs-lookup"><span data-stu-id="5da13-135">In your CLI, run the following command.</span></span>

```Shell
ng generate service alerts
```

<span data-ttu-id="5da13-136">Abra o `./src/app/alerts.service.ts` arquivo e substitua seu conteúdo pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="5da13-136">Open the `./src/app/alerts.service.ts` file and replace its contents with the following.</span></span>

```TypeScript
import { Injectable } from '@angular/core';
import { Alert } from './alert';

@Injectable({
  providedIn: 'root'
})
export class AlertsService {

  alerts: Alert[] = [];

  add(message: string, debug: string) {
    this.alerts.push({message: message, debug: debug});
  }

  remove(alert: Alert) {
    this.alerts.splice(this.alerts.indexOf(alert), 1);
  }
}
```

<span data-ttu-id="5da13-137">Agora, gere um componente de alerta para exibir alertas.</span><span class="sxs-lookup"><span data-stu-id="5da13-137">Now generate an alerts component to display alerts.</span></span> <span data-ttu-id="5da13-138">Na sua CLI, execute o seguinte comando.</span><span class="sxs-lookup"><span data-stu-id="5da13-138">In your CLI, run the following command.</span></span>

```Shell
ng generate component alerts
```

<span data-ttu-id="5da13-139">Quando o comando for concluído, abra o `./src/app/alerts/alerts.component.ts` arquivo e substitua seu conteúdo pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="5da13-139">Once the command completes, open the `./src/app/alerts/alerts.component.ts` file and replace its contents with the following.</span></span>

```TypeScript
import { Component, OnInit } from '@angular/core';
import { AlertsService } from '../alerts.service';
import { Alert } from '../alert';

@Component({
  selector: 'app-alerts',
  templateUrl: './alerts.component.html',
  styleUrls: ['./alerts.component.css']
})
export class AlertsComponent implements OnInit {

  constructor(private alertsService: AlertsService) { }

  ngOnInit() {
  }

  close(alert: Alert) {
    this.alertsService.remove(alert);
  }
}
```

<span data-ttu-id="5da13-140">Em seguida, `./src/app/alerts/alerts.component.html` Abra o arquivo e substitua seu conteúdo pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="5da13-140">Then open the `./src/app/alerts/alerts.component.html` file and replace its contents with the following.</span></span>

```html
<div *ngFor="let alert of alertsService.alerts">
  <ngb-alert type="danger" (close)="close(alert)">
    <p>{{alert.message}}</p>
    <pre *ngIf="alert.debug" class="alert-pre border bg-light p-2"><code>{{alert.debug}}</code></pre>
  </ngb-alert>
</div>
```

<span data-ttu-id="5da13-141">Agora, com os componentes básicos definidos, atualize o aplicativo para usá-los.</span><span class="sxs-lookup"><span data-stu-id="5da13-141">Now with those basic components defined, update the app to use them.</span></span> <span data-ttu-id="5da13-142">Primeiro, abra o `./src/app/app-routing.module.ts` arquivo e substitua a `const routes: Routes = [];` linha pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="5da13-142">First, open the `./src/app/app-routing.module.ts` file and replace the `const routes: Routes = [];` line with the following code.</span></span>

```TypeScript
import { HomeComponent } from './home/home.component';

const routes: Routes = [
  { path: '', component: HomeComponent },
];
```

<span data-ttu-id="5da13-143">Abra o arquivo `./src/app/app.component.html` e substitua o conteúdo inteiro pelo seguinte:</span><span class="sxs-lookup"><span data-stu-id="5da13-143">Open the `./src/app/app.component.html` file and replace its entire contents with the following.</span></span>

```html
<app-nav-bar></app-nav-bar>
<main role="main" class="container">
  <app-alerts></app-alerts>
  <router-outlet></router-outlet>
</main>
```

<span data-ttu-id="5da13-144">Salve todas as suas alterações e atualize a página.</span><span class="sxs-lookup"><span data-stu-id="5da13-144">Save all of your changes and refresh the page.</span></span> <span data-ttu-id="5da13-145">Agora, o aplicativo deve ser muito diferente.</span><span class="sxs-lookup"><span data-stu-id="5da13-145">Now, the app should look very different.</span></span>

![Uma captura de tela da página inicial reprojetada](images/create-app-01.png)