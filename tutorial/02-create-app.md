<!-- markdownlint-disable MD002 MD041 -->

Nesta seção, você criará um novo Angular projeto.

1. Abra sua interface de linha de comando (CLI), navegue até um diretório no qual você tem direitos para criar arquivos e execute os seguintes comandos para instalar a ferramenta [Angular CLI](https://www.npmjs.com/package/@angular/cli) e criar um novo aplicativo Angular.

    ```Shell
    npm install -g @angular/cli@11.2.9
    ng new graph-tutorial
    ```

1. A Angular CLI solicitará mais informações. Responda aos prompts da seguinte forma.

    ```Shell
    ? Do you want to enforce stricter type checking and stricter bundle budgets in the workspace? Yes
    ? Would you like to add Angular routing? Yes
    ? Which stylesheet format would you like to use? CSS
    ```

1. Depois que o comando terminar, altere para o diretório em sua CLI e execute `graph-tutorial` o seguinte comando para iniciar um servidor Web local.

    ```Shell
    ng serve --open
    ```

1. Seu navegador padrão é aberto [https://localhost:4200/](https://localhost:4200) com uma página Angular padrão. Se o navegador não abrir, abra-o e navegue até verificar [https://localhost:4200/](https://localhost:4200) se o novo aplicativo funciona.

## <a name="add-node-packages"></a>Adicionar pacotes de nó

Antes de continuar, instale alguns pacotes adicionais que você usará posteriormente:

- [bootstrap](https://github.com/twbs/bootstrap) para estilo e componentes comuns.
- [ng-bootstrap para](https://github.com/ng-bootstrap/ng-bootstrap) usar componentes Bootstrap de Angular.
- [momento](https://github.com/moment/moment) para formatação de datas e horas.
- [windows-iana](https://github.com/rubenillodo/windows-iana)
- [msal-angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) para autenticação para Azure Active Directory e recuperação de tokens de acesso.
- [microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) para fazer chamadas para o Microsoft Graph.

1. Execute os seguintes comandos em sua CLI.

    ```Shell
    npm install bootstrap@4.6.0 @ng-bootstrap/ng-bootstrap@9.1.0
    npm install @azure/msal-browser@2.14.0 @azure/msal-angular@2.0.0-beta.4
    npm install moment-timezone@0.5.33 windows-iana@5.0.2
    npm install @microsoft/microsoft-graph-client@2.2.1 @microsoft/microsoft-graph-types@1.35.0
    ```

1. Execute o seguinte comando em sua CLI para adicionar o pacote de Angular de localização (exigido pelo ng-bootstrap).

    ```Shell
    ng add @angular/localize
    ```

## <a name="design-the-app"></a>Design do aplicativo

Nesta seção, você criará a interface do usuário para o aplicativo.

1. Abra **./src/styles.css** e adicione as seguintes linhas.

    :::code language="css" source="../demo/graph-tutorial/src/styles.css":::

1. Adicione o módulo Bootstrap ao aplicativo. Abra **./src/app/app.module.ts** e substitua seu conteúdo pelo seguinte.

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

1. Crie um novo arquivo na **pasta ./src/app** chamada **user.ts** e adicione o código a seguir.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/user.ts" id="UserSnippet":::

1. Gere um Angular para a navegação superior na página. Em sua CLI, execute o seguinte comando.

    ```Shell
    ng generate component nav-bar
    ```

1. Depois que o comando é concluído, abra **./src/app/nav-bar/nav-bar.component.ts** e substitua seu conteúdo pelo seguinte.

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
      showNav: boolean = false;
      // Is a user logged in?
      authenticated: boolean = false;
      // The user
      user?: User = undefined;

      constructor() { }

      ngOnInit() { }

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
          avatar: '',
          timeZone: ''
        };
      }

      signOut(): void {
        // Temporary
        this.authenticated = false;
        this.user = undefined;
      }
    }
    ```

1. Abra **./src/app/nav-bar/nav-bar.component.html** e substitua seu conteúdo pelo seguinte.

    :::code language="html" source="../demo/graph-tutorial/src/app/nav-bar/nav-bar.component.html" id="navHtml":::

1. Crie uma home page para o aplicativo. Execute o seguinte comando em sua CLI.

    ```Shell
    ng generate component home
    ```

1. Depois que o comando é concluído, abra **./src/app/home/home.component.ts** e substitua seu conteúdo pelo seguinte.

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
      authenticated: boolean = false;
      // The user
      user?: User = undefined;

      constructor() { }

      ngOnInit() { }

      signIn(): void {
        // Temporary
        this.authenticated = true;
        this.user = {
          displayName: 'Adele Vance',
          email: 'AdeleV@contoso.com',
          avatar: '',
          timeZone: ''
        };
      }
    }
    ```

1. Abra **./src/app/home/home.component.html** e substitua seu conteúdo pelo seguinte.

    :::code language="html" source="../demo/graph-tutorial/src/app/home/home.component.html" id="homeHtml":::

1. Crie uma classe `Alert` simples. Crie um novo arquivo no **diretório ./src/app** chamado **alert.ts** e adicione o código a seguir.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/alert.ts" id="AlertSnippet":::

1. Crie um serviço de alerta que o aplicativo pode usar para exibir mensagens para o usuário. Em sua CLI, execute o seguinte comando.

    ```Shell
    ng generate service alerts
    ```

1. Abra **./src/app/alerts.service.ts** e substitua seu conteúdo pelo seguinte.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/alerts.service.ts" id="alertsService":::

1. Gere um componente de alertas para exibir alertas. Em sua CLI, execute o seguinte comando.

    ```Shell
    ng generate component alerts
    ```

1. Depois que o comando é concluído, abra **./src/app/alerts/alerts.component.ts** e substitua seu conteúdo pelo seguinte.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/alerts/alerts.component.ts" id="AlertsComponentSnippet":::

1. Abra **./src/app/alerts/alerts.component.html** e substitua seu conteúdo pelo seguinte.

    :::code language="html" source="../demo/graph-tutorial/src/app/alerts/alerts.component.html" id="AlertHtml":::

1. Abra **./src/app/app-routing.module.ts** e substitua a `const routes: Routes = [];` linha pelo código a seguir.

    ```typescript
    import { HomeComponent } from './home/home.component';

    const routes: Routes = [
      { path: '', component: HomeComponent },
    ];
    ```

1. Abra **./src/app/app.component.html** e substitua todo o conteúdo pelo seguinte.

    :::code language="html" source="../demo/graph-tutorial/src/app/app.component.html" id="AppHtml":::

1. Adicione um arquivo de imagem de sua escolha **no-profile-photo.png** no **diretório ./src/assets.** Essa imagem será usada como a foto do usuário quando o usuário não tiver nenhuma foto no Microsoft Graph.

Salve todas as alterações e atualize a página. Agora, o aplicativo deve ter uma aparência muito diferente.

![Captura de tela da home page](images/create-app-01.png)
