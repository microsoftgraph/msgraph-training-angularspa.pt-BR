<!-- markdownlint-disable MD002 MD041 -->

Nesta seção, você criará um novo projeto angular.

1. Abra a interface de linha de comando (CLI), navegue até um diretório onde você tem direitos para criar arquivos e execute os seguintes comandos para instalar a ferramenta de [CLI angular](https://www.npmjs.com/package/@angular/cli) e criar um novo aplicativo angular.

    ```Shell
    npm install -g @angular/cli@10.1.7
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

1. O navegador padrão é aberto [https://localhost:4200/](https://localhost:4200) com uma página angular padrão. Se o navegador não abrir, abra-o e navegue até [https://localhost:4200/](https://localhost:4200) para verificar se o novo aplicativo funciona.

## <a name="add-node-packages"></a>Adicionar pacotes de nós

Antes de prosseguir, instale alguns pacotes adicionais que serão usados posteriormente:

- [Bootstrap](https://github.com/twbs/bootstrap) para estilo e componentes comuns.
- [ng-inicialização](https://github.com/ng-bootstrap/ng-bootstrap) para usar componentes de inicialização de angular.
- [tempo](https://github.com/moment/moment) para formatar datas e horas.
- [Windows-IANA](https://github.com/rubenillodo/windows-iana)
- [MSAL-angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) para autenticação no Azure Active Directory e recuperação de tokens de acesso.
- [Microsoft-Graph-Client](https://github.com/microsoftgraph/msgraph-sdk-javascript) para fazer chamadas para o Microsoft Graph.

1. Execute os seguintes comandos em sua CLI.

    ```Shell
    npm install bootstrap@4.5.3 @ng-bootstrap/ng-bootstrap@7.0.0 msal@1.4.2 @azure/msal-angular@1.1.1
    npm install moment@2.29.1 moment-timezone@0.5.31 windows-iana@4.2.1
    npm install @microsoft/microsoft-graph-client@2.1.0 @microsoft/microsoft-graph-types@1.24.0
    ```

1. Execute o seguinte comando em sua CLI para adicionar o pacote de localização angular (exigido pela ng-Bootstrap).

    ```Shell
    ng add @angular/localize
    ```

## <a name="design-the-app"></a>Projetar o aplicativo

Nesta seção, você criará a interface do usuário para o aplicativo.

1. Abra **./src/Styles.css** e adicione as linhas a seguir.

    :::code language="css" source="../demo/graph-tutorial/src/styles.css":::

1. Adicione o módulo Bootstrap ao aplicativo. Abra **./src/app/app.Module.TS** e substitua seu conteúdo pelo seguinte.

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

1. Crie um novo arquivo na pasta **./src/app** chamado **User. TS** e adicione o código a seguir.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/user.ts" id="user":::

1. Gere um componente angular para a navegação superior na página. Na sua CLI, execute o seguinte comando.

    ```Shell
    ng generate component nav-bar
    ```

1. Quando o comando for concluído, abra **./src/app/NAV-bar/NAV-bar.Component.TS** e substitua o conteúdo pelo seguinte.

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

1. Abra **./src/app/nav-bar/nav-bar.component.html** e substitua seu conteúdo pelo seguinte.

    :::code language="html" source="../demo/graph-tutorial/src/app/nav-bar/nav-bar.component.html" id="navHtml":::

1. Criar uma home page para o aplicativo. Execute o seguinte comando em sua CLI.

    ```Shell
    ng generate component home
    ```

1. Quando o comando for concluído, abra **./src/app/Home/Home.Component.TS** e substitua o conteúdo pelo seguinte.

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

1. Abra **./src/app/home/home.component.html** e substitua seu conteúdo pelo seguinte.

    :::code language="html" source="../demo/graph-tutorial/src/app/home/home.component.html" id="homeHtml":::

1. Criar uma `Alert` classe simples. Crie um novo arquivo no diretório **./src/app** chamado **Alert. TS** e adicione o código a seguir.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/alert.ts" id="alert":::

1. Criar um serviço de alerta que o aplicativo pode usar para exibir mensagens para o usuário. Na sua CLI, execute o seguinte comando.

    ```Shell
    ng generate service alerts
    ```

1. Abra **./src/app/Alerts.Service.TS** e substitua seu conteúdo pelo seguinte.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/alerts.service.ts" id="alertsService":::

1. Gere um componente de alerta para exibir alertas. Na sua CLI, execute o seguinte comando.

    ```Shell
    ng generate component alerts
    ```

1. Quando o comando for concluído, abra **./src/app/Alerts/Alerts.Component.TS** e substitua o conteúdo pelo seguinte.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/alerts/alerts.component.ts" id="alertComponent":::

1. Abra **./src/app/alerts/alerts.component.html** e substitua seu conteúdo pelo seguinte.

    :::code language="html" source="../demo/graph-tutorial/src/app/alerts/alerts.component.html" id="alertHtml":::

1. Abra **./src/app/app-Routing.Module.TS** e substitua a `const routes: Routes = [];` linha pelo código a seguir.

    ```typescript
    import { HomeComponent } from './home/home.component';

    const routes: Routes = [
      { path: '', component: HomeComponent },
    ];
    ```

1. Abra **./src/app/app.component.html** e substitua todo o seu conteúdo pelo seguinte.

    :::code language="html" source="../demo/graph-tutorial/src/app/app.component.html" id="appHtml":::

1. Adicione um arquivo de imagem de sua escolha nomeada **no-profile-photo.png** no diretório **./src/assets** . Esta imagem será usada como foto do usuário quando o usuário não tiver nenhuma foto no Microsoft Graph.

Salve todas as suas alterações e atualize a página. Agora, o aplicativo deve ser muito diferente.

![Uma captura de tela da página inicial reprojetada](images/create-app-01.png)
