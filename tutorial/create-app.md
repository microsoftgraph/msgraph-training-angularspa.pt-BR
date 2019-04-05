<!-- markdownlint-disable MD002 MD041 -->

Abra a interface de linha de comando (CLI), navegue até um diretório onde você tem direitos para criar arquivos e execute os seguintes comandos para instalar a ferramenta de [CLI angular](https://www.npmjs.com/package/@angular/cli) e criar um novo aplicativo angular.

```Shell
npm install -g @angular/cli
ng new graph-tutorial
```

A CLI angular solicitará mais informações. Responda aos prompts da seguinte maneira.

```Shell
? Would you like to add Angular routing? Yes
? Which stylesheet format would you like to use? CSS
```

Quando o comando terminar, mude para o `graph-tutorial` diretório na sua CLI e execute o seguinte comando para iniciar um servidor Web local.

```Shell
ng serve --open
```

O navegador padrão é aberto [https://localhost:4200/](https://localhost:4200) com uma página angular padrão. Se o navegador não abrir, abra-o e navegue [https://localhost:4200/](https://localhost:4200) até para verificar se o novo aplicativo funciona.

Antes de prosseguir, instale alguns pacotes adicionais que serão usados posteriormente:

- [Bootstrap](https://github.com/twbs/bootstrap) para estilo e componentes comuns.
- [ng-inicialização](https://github.com/ng-bootstrap/ng-bootstrap) para usar componentes de inicialização de angular.
- [angular-fontawesome](https://github.com/FortAwesome/angular-fontawesome) para usar fontawesome ícones em angulares.
- [fontawesome-SVG-Core](https://github.com/FortAwesome/Font-Awesome), [Free-regular-SVG-ícones](https://github.com/FortAwesome/Font-Awesome)e os ícones [gratuitos-Solid-SVG-](https://github.com/FortAwesome/Font-Awesome) para os ícones de fontawesome usados no exemplo.
- [tempo](https://github.com/moment/moment) para formatar datas e horas.
- [MSAL-angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) para autenticação no Azure Active Directory e recuperação de tokens de acesso.
- [rxjs-compat](https://github.com/ReactiveX/rxjs/tree/master/compat), obrigatório para o `msal-angular` pacote.
- [Microsoft-Graph-Client](https://github.com/microsoftgraph/msgraph-sdk-javascript) para fazer chamadas para o Microsoft Graph.

Execute o seguinte comando em sua CLI.

```Shell
npm install bootstrap@4.3.1 @fortawesome/angular-fontawesome@0.3.0 @fortawesome/fontawesome-svg-core@1.2.15
npm install @fortawesome/free-regular-svg-icons@5.7.2 @fortawesome/free-solid-svg-icons@5.7.2
npm install moment@2.24.0 moment-timezone@0.5.23 @ng-bootstrap/ng-bootstrap@4.1.0
npm install @azure/msal-angular@0.1.2 rxjs-compat@6.4.0 @microsoft/microsoft-graph-client@1.4.0
```

## <a name="design-the-app"></a>Projetar o aplicativo

Comece adicionando os arquivos CSS de inicialização ao aplicativo, bem como alguns estilos globais. Abra o `./src/styles.css` e adicione as linhas a seguir.

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

Em seguida, adicione os módulos Bootstrap e FontAwesome ao aplicativo. Abra `./src/app/app.module.ts` e adicione as seguintes `import` instruções à parte superior do arquivo.

```TypeScript
import { NgbModule } from '@ng-bootstrap/ng-bootstrap';
import { FontAwesomeModule } from '@fortawesome/angular-fontawesome';
import { library } from '@fortawesome/fontawesome-svg-core';
import { faExternalLinkAlt } from '@fortawesome/free-solid-svg-icons';
import { faUserCircle } from '@fortawesome/free-regular-svg-icons';
```

Em seguida, adicione o seguinte código após todas `import` as instruções.

```TypeScript
library.add(faExternalLinkAlt);
library.add(faUserCircle);
```

Na `@NgModule` declaração, substitua a matriz existente `imports` pelo seguinte.

```TypeScript
imports: [
  BrowserModule,
  AppRoutingModule,
  NgbModule,
  FontAwesomeModule
]
```

Agora, gere um componente angular para a navegação superior na página. Na sua CLI, execute o seguinte comando.

```Shell
ng generate component nav-bar
```

Quando o comando for concluído, abra o `./src/app/nav-bar/nav-bar.component.ts` arquivo e substitua seu conteúdo pelo seguinte.

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

Abra o `./src/app/nav-bar/nav-bar.component.html` arquivo e substitua seu conteúdo pelo seguinte.

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

Em seguida, crie uma home page para o aplicativo. Execute o seguinte comando em sua CLI.

```Shell
ng generate component home
```

Quando o comando for concluído, abra o `./src/app/home/home.component.ts` arquivo e substitua seu conteúdo pelo seguinte.

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

Em seguida, `./src/app/home/home.component.html` Abra o arquivo e substitua seu conteúdo pelo seguinte.

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

Agora, crie um serviço de alerta que o aplicativo pode usar para exibir mensagens para o usuário. Comece criando uma classe simples `Alert` . Crie um novo arquivo no `./src/app` diretório chamado `alert.ts` e adicione o código a seguir.

```TypeScript
export class Alert {
  message: string;
  debug: string;
}
```

Na sua CLI, execute o seguinte comando.

```Shell
ng generate service alerts
```

Abra o `./src/app/alerts.service.ts` arquivo e substitua seu conteúdo pelo seguinte.

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

Agora, gere um componente de alerta para exibir alertas. Na sua CLI, execute o seguinte comando.

```Shell
ng generate component alerts
```

Quando o comando for concluído, abra o `./src/app/alerts/alerts.component.ts` arquivo e substitua seu conteúdo pelo seguinte.

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

Em seguida, `./src/app/alerts/alerts.component.html` Abra o arquivo e substitua seu conteúdo pelo seguinte.

```html
<div *ngFor="let alert of alertsService.alerts">
  <ngb-alert type="danger" (close)="close(alert)">
    <p>{{alert.message}}</p>
    <pre *ngIf="alert.debug" class="alert-pre border bg-light p-2"><code>{{alert.debug}}</code></pre>
  </ngb-alert>
</div>
```

Agora, com os componentes básicos definidos, atualize o aplicativo para usá-los. Primeiro, abra o `./src/app/app-routing.module.ts` arquivo e substitua a `const routes: Routes = [];` linha pelo código a seguir.

```TypeScript
import { HomeComponent } from './home/home.component';

const routes: Routes = [
  { path: '', component: HomeComponent },
];
```

Abra o arquivo `./src/app/app.component.html` e substitua o conteúdo inteiro pelo seguinte:

```html
<app-nav-bar></app-nav-bar>
<main role="main" class="container">
  <app-alerts></app-alerts>
  <router-outlet></router-outlet>
</main>
```

Salve todas as suas alterações e atualize a página. Agora, o aplicativo deve ser muito diferente.

![Uma captura de tela da página inicial reprojetada](images/create-app-01.png)