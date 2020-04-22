<!-- markdownlint-disable MD002 MD041 -->

Neste exercício, você estenderá o aplicativo do exercício anterior para oferecer suporte à autenticação com o Azure AD. Isso é necessário para obter o token de acesso OAuth necessário para chamar o Microsoft Graph. Nesta etapa, você integrará a [biblioteca de autenticação da Microsoft para o angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) no aplicativo.

1. Crie um novo arquivo no `./src` diretório chamado `oauth.ts` e adicione o código a seguir.

    :::code language="typescript" source="../demo/graph-tutorial/src/oauth.ts.example":::

    Substitua `YOUR_APP_ID_HERE` pela ID do aplicativo do portal de registro do aplicativo.

    > [!IMPORTANT]
    > Se você estiver usando o controle de origem como o Git, agora seria uma boa hora para excluir `oauth.ts` o arquivo do controle de origem para evitar vazar inadvertidamente sua ID de aplicativo.

1. Abra `./src/app/app.module.ts` e adicione as seguintes `import` instruções à parte superior do arquivo.

    ```TypeScript
    import { MsalModule } from '@azure/msal-angular';
    import { OAuthSettings } from '../oauth';
    ```

1. Adicione o `MsalModule` à `imports` matriz dentro da `@NgModule` declaração e inicialize-o com a ID do aplicativo.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/app.module.ts" id="imports":::

## <a name="implement-sign-in"></a>Implementar logon

Nesta seção, você criará um serviço de autenticação e implementará a entrada e a saída.

1. Execute o seguinte comando em sua CLI.

    ```Shell
    ng generate service auth
    ```

    Ao criar um serviço para isso, você pode injetar-o facilmente em qualquer componente que precise acessar os métodos de autenticação.

1. Quando o comando terminar, abra o `./src/app/auth.service.ts` arquivo e substitua seu conteúdo pelo código a seguir.

    ```TypeScript
    import { Injectable } from '@angular/core';
    import { MsalService } from '@azure/msal-angular';

    import { AlertsService } from './alerts.service';
    import { OAuthSettings } from '../oauth';
    import { User } from './user';

    @Injectable({
      providedIn: 'root'
    })

    export class AuthService {
      public authenticated: boolean;
      public user: User;

      constructor(
        private msalService: MsalService,
        private alertsService: AlertsService) {

        this.authenticated = false;
        this.user = null;
      }

      // Prompt the user to sign in and
      // grant consent to the requested permission scopes
      async signIn(): Promise<void> {
        let result = await this.msalService.loginPopup(OAuthSettings)
          .catch((reason) => {
            this.alertsService.add('Login failed', JSON.stringify(reason, null, 2));
          });

        if (result) {
          this.authenticated = true;
          // Temporary placeholder
          this.user = new User();
          this.user.displayName = "Adele Vance";
          this.user.email = "AdeleV@contoso.com";
        }
      }

      // Sign out
      signOut(): void {
        this.msalService.logout();
        this.user = null;
        this.authenticated = false;
      }

      // Silently request an access token
      async getAccessToken(): Promise<string> {
        let result = await this.msalService.acquireTokenSilent(OAuthSettings)
          .catch((reason) => {
            this.alertsService.add('Get token failed', JSON.stringify(reason, null, 2));
          });

        if (result) {
          // Temporary to display token in an error box
          this.alertsService.add('Token acquired', result.accessToken);
          return result.accessToken;
        }
        return null;
      }
    }
    ```

1. Abra o `./src/app/nav-bar/nav-bar.component.ts` arquivo e substitua seu conteúdo pelo seguinte.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/nav-bar/nav-bar.component.ts" id="navBarSnippet" highlight="3,15-22,24,26-28,36-38,40-42":::

1. Abra`./src/app/home/home.component.ts` e substitua seu conteúdo pelo seguinte.

    :::code language="typescript" source="snippets/snippets.ts" id="homeSnippet" highlight="3,12-19,21,23,25-27":::

Salve suas alterações e atualize o navegador. Clique no botão **clique aqui para entrar** e você deve ser redirecionado para `https://login.microsoftonline.com`o. Faça logon com sua conta da Microsoft e concorde com as permissões solicitadas. A página do aplicativo deve ser atualizada, mostrando o token.

### <a name="get-user-details"></a>Obter detalhes do usuário

No momento, o serviço de autenticação define valores constantes para o nome de exibição e endereço de email do usuário. Agora que você tem um token de acesso, você pode obter detalhes do usuário do Microsoft Graph para que esses valores correspondam ao usuário atual.

1. Abra `./src/app/auth.service.ts` e adicione a seguinte `import` instrução à parte superior do arquivo.

    ```TypeScript
    import { Client } from '@microsoft/microsoft-graph-client';
    ```

1. Adicione uma nova função à classe `AuthService` chamada `getUser`.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/auth.service.ts" id="getUserSnippet":::

1. Localize e remova o código a seguir no `getAccessToken` método que adiciona um alerta para exibir o token de acesso.

    ```TypeScript
    // Temporary to display token in an error box
    this.alertsService.add('Token acquired', result);
    ```

1. Localize e remova o código a seguir do `signIn` método.

    ```TypeScript
    // Temporary placeholder
    this.user = new User();
    this.user.displayName = "Adele Vance";
    this.user.email = "AdeleV@contoso.com";
    ```

1. Em seu lugar, adicione o código a seguir.

    ```TypeScript
    this.user = await this.getUser();
    ```

    Este novo código usa o SDK do Microsoft Graph para obter os detalhes do usuário e, em `User` seguida, cria um objeto usando valores retornados pela chamada à API.

1. Altere o `constructor` para a `AuthService` classe para verificar se o usuário já está conectado e carregar seus detalhes em caso afirmativo. Substitua o existente `constructor` pelo seguinte.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/auth.service.ts" id="constructorSnippet" highlight="5-6":::

1. Remova o código temporário da `HomeComponent` classe. Abra o `./src/app/home/home.component.ts` arquivo e substitua a função `signIn` existente pelo seguinte.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/home/home.component.ts" id="signInSnippet" highlight="5-6":::

Agora, se você salvar as alterações e iniciar o aplicativo, depois de entrar, você deverá terminar de volta na Home Page, mas a interface do usuário deverá mudar para indicar que você está conectado.

![Uma captura de tela da Home Page após entrar](./images/add-aad-auth-01.png)

Clique no avatar do usuário no canto superior direito para **acessar o link sair.** Clicar **em sair** redefine a sessão e retorna à Home Page.

![Uma captura de tela do menu suspenso com o link sair](./images/add-aad-auth-02.png)

## <a name="storing-and-refreshing-tokens"></a>Armazenar e atualizar tokens

Nesse ponto, seu aplicativo tem um token de acesso, que é enviado no `Authorization` cabeçalho das chamadas de API. Este é o token que permite que o aplicativo acesse o Microsoft Graph em nome do usuário.

No entanto, esse token é de vida curta. O token expira uma hora após sua emissão. Como o aplicativo está usando a biblioteca MSAL, você não precisa implementar qualquer lógica de armazenamento ou de atualização. O `MsalService` armazena em cache o token no armazenamento do navegador. O `acquireTokenSilent` método primeiro verifica o token em cache e, se ele não tiver expirado, ele o retornará. Se ele tiver expirado, ele fará uma solicitação silenciosa para obter um novo.
