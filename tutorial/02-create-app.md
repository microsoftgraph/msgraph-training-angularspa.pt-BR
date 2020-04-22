<!-- markdownlint-disable MD002 MD041 -->

Nesta seção, você criará um novo projeto angular.

1. Abra a interface de linha de comando (CLI), navegue até um diretório onde você tem direitos para criar arquivos e execute os seguintes comandos para instalar a ferramenta de [CLI angular](https://www.npmjs.com/package/@angular/cli) e criar um novo aplicativo angular.

    ```Shell
    npm install -g @angular/cli@9.0.6
    ng new graph-tutorial
    ```

1. A CLI angular solicitará mais informações. Responda aos prompts da seguinte maneira.

    ```Shell
    ? Would you like to add Angular routing? Yes
    ? Which stylesheet format would you like to use? CSS
    ```

1. Quando o comando terminar, mude para o `graph-tutorial` diretório na sua CLI e execute o seguinte comando para iniciar um servidor Web local.

    ```Shell
    ng serve --open
    ```

1. O navegador padrão é aberto [https://localhost:4200/](https://localhost:4200) com uma página angular padrão. Se o navegador não abrir, abra-o e navegue [https://localhost:4200/](https://localhost:4200) até para verificar se o novo aplicativo funciona.

## <a name="add-node-packages"></a>Adicionar pacotes de nós

Antes de prosseguir, instale alguns pacotes adicionais que serão usados posteriormente:

- [Bootstrap](https://github.com/twbs/bootstrap) para estilo e componentes comuns.
- [ng-inicialização](https://github.com/ng-bootstrap/ng-bootstrap) para usar componentes de inicialização de angular.
- [angular-fontawesome](https://github.com/FortAwesome/angular-fontawesome) para usar fontawesome ícones em angulares.
- [fontawesome-SVG-Core](https://github.com/FortAwesome/Font-Awesome), [Free-regular-SVG-ícones](https://github.com/FortAwesome/Font-Awesome)e os ícones [gratuitos-Solid-SVG-](https://github.com/FortAwesome/Font-Awesome) para os ícones de fontawesome usados no exemplo.
- [tempo](https://github.com/moment/moment) para formatar datas e horas.
- [MSAL-angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) para autenticação no Azure Active Directory e recuperação de tokens de acesso.
- [Microsoft-Graph-Client](https://github.com/microsoftgraph/msgraph-sdk-javascript) para fazer chamadas para o Microsoft Graph.

1. Execute os seguintes comandos em sua CLI.

    ```Shell
    npm install bootstrap@4.4.1 @fortawesome/angular-fontawesome@0.6.0 @fortawesome/fontawesome-svg-core@1.2.27
    npm install @fortawesome/free-regular-svg-icons@5.12.1 @fortawesome/free-solid-svg-icons@5.12.1
    npm install moment@2.24.0 moment-timezone@0.5.28 @ng-bootstrap/ng-bootstrap@6.0.0
    npm install msal@1.2.1 @azure/msal-angular@1.0.0-beta.4 @microsoft/microsoft-graph-client@2.0.0
    ```

1. Execute o seguinte comando em sua CLI para adicionar o pacote de localização angular (exigido pela ng-Bootstrap).

    ```Shell
    ng add @angular/localize
    ```

## <a name="design-the-app"></a>Projetar o aplicativo

Nesta seção, você criará a interface do usuário para o aplicativo.

1. Abra o `./src/styles.css` e adicione as linhas a seguir.

    :::code language="css" source="../demo/graph-tutorial/src/styles.css":::

1. Adicione os módulos Bootstrap e FontAwesome ao aplicativo. Abra `./src/app/app.module.ts` e substitua seu conteúdo pelo seguinte.

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

1. Crie um novo arquivo na `./src/app` pasta chamada `user.ts` e adicione o código a seguir.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/user.ts" id="user":::

1. Gere um componente angular para a navegação superior na página. Na sua CLI, execute o seguinte comando.

    ```Shell
    ng generate component nav-bar
    ```

1. Quando o comando for concluído, abra o `./src/app/nav-bar/nav-bar.component.ts` arquivo e substitua seu conteúdo pelo seguinte.

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

1. Abra o `./src/app/nav-bar/nav-bar.component.html` arquivo e substitua seu conteúdo pelo seguinte.

    :::code language="html" source="../demo/graph-tutorial/src/app/nav-bar/nav-bar.component.html" id="navHtml":::

1. Criar uma home page para o aplicativo. Execute o seguinte comando em sua CLI.

    ```Shell
    ng generate component home
    ```

1. Quando o comando for concluído, abra o `./src/app/home/home.component.ts` arquivo e substitua seu conteúdo pelo seguinte.

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

1. Abra o `./src/app/home/home.component.html` arquivo e substitua seu conteúdo pelo seguinte.

    :::code language="html" source="../demo/graph-tutorial/src/app/home/home.component.html" id="homeHtml":::

1. Criar uma classe `Alert` simples. Crie um novo arquivo no `./src/app` diretório chamado `alert.ts` e adicione o código a seguir.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/alert.ts" id="alert":::

1. Criar um serviço de alerta que o aplicativo pode usar para exibir mensagens para o usuário. Na sua CLI, execute o seguinte comando.

    ```Shell
    ng generate service alerts
    ```

1. Abra o `./src/app/alerts.service.ts` arquivo e substitua seu conteúdo pelo seguinte.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/alerts.service.ts" id="alertsService":::

1. Gere um componente de alerta para exibir alertas. Na sua CLI, execute o seguinte comando.

    ```Shell
    ng generate component alerts
    ```

1. Quando o comando for concluído, abra o `./src/app/alerts/alerts.component.ts` arquivo e substitua seu conteúdo pelo seguinte.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/alerts/alerts.component.ts" id="alertComponent":::

1. Abra o `./src/app/alerts/alerts.component.html` arquivo e substitua seu conteúdo pelo seguinte.

    :::code language="html" source="../demo/graph-tutorial/src/app/alerts/alerts.component.html" id="alertHtml":::

1. Abra o `./src/app/app-routing.module.ts` arquivo e substitua a `const routes: Routes = [];` linha pelo código a seguir.

    ```typescript
    import { HomeComponent } from './home/home.component';

    const routes: Routes = [
      { path: '', component: HomeComponent },
    ];
    ```

1. Abra o arquivo `./src/app/app.component.html` e substitua o conteúdo inteiro pelo seguinte:

    :::code language="html" source="../demo/graph-tutorial/src/app/app.component.html" id="appHtml":::

Salve todas as suas alterações e atualize a página. Agora, o aplicativo deve ser muito diferente.

![Uma captura de tela da página inicial reprojetada](images/create-app-01.png)
