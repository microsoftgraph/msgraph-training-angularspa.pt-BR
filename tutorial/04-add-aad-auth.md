<!-- markdownlint-disable MD002 MD041 -->

Neste exercício, você estenderá o aplicativo do exercício anterior para dar suporte à autenticação com o Azure AD. Isso é necessário para obter o token de acesso OAuth necessário para chamar o Microsoft Graph. Nesta etapa, você integrará a Biblioteca de [Autenticação da Microsoft para Angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) ao aplicativo.

1. Crie um novo arquivo no **diretório ./src** chamado **oauth.ts** e adicione o código a seguir.

    :::code language="typescript" source="../demo/graph-tutorial/src/oauth.example.ts":::

    Substitua `YOUR_APP_ID_HERE` pela ID do aplicativo no Portal de Registro de Aplicativos.

    > [!IMPORTANT]
    > Se você estiver usando o controle de origem, como git, agora seria um bom momento para excluir o arquivo **oauth.ts** do controle de origem para evitar o vazamento inadvertida da ID do aplicativo.

1. Abra **./src/app/app.module.ts** e adicione as instruções a seguir `import` à parte superior do arquivo.

    ```typescript
    import { IPublicClientApplication,
             PublicClientApplication,
             BrowserCacheLocation } from '@azure/msal-browser';
    import { MsalModule,
             MsalService,
             MSAL_INSTANCE } from '@azure/msal-angular';
    import { OAuthSettings } from '../oauth';
    ```

1. Adicione a seguinte função abaixo das `import` instruções.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/app.module.ts" id="MSALFactorySnippet":::

1. Adicione a `MsalModule` matriz `imports` à `@NgModule` declaração.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/app.module.ts" id="ImportsSnippet" highlight="6":::

1. Adicione e `MSALInstanceFactory` à matriz dentro da `MsalService` `providers` `@NgModule` declaração.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/app.module.ts" id="ProvidersSnippet" highlight="2-6":::

## <a name="implement-sign-in"></a>Implementar login

Nesta seção, você criará um serviço de autenticação e implementará a assinatura e a saída.

1. Execute o seguinte comando em sua CLI.

    ```Shell
    ng generate service auth
    ```

    Ao criar um serviço para isso, você pode injetá-lo facilmente em todos os componentes que precisem de acesso aos métodos de autenticação.

1. Depois que o comando terminar, abra **./src/app/auth.service.ts** e substitua seu conteúdo pelo código a seguir.

    ```typescript
    import { Injectable } from '@angular/core';
    import { AccountInfo } from '@azure/msal-browser';
    import { MsalService } from '@azure/msal-angular';

    import { AlertsService } from './alerts.service';
    import { OAuthSettings } from '../oauth';
    import { User } from './user';

    @Injectable({
      providedIn: 'root'
    })

    export class AuthService {
      public authenticated: boolean;
      public user?: User;

      constructor(
        private msalService: MsalService,
        private alertsService: AlertsService) {

        this.authenticated = false;
        this.user = undefined;
      }

      // Prompt the user to sign in and
      // grant consent to the requested permission scopes
      async signIn(): Promise<void> {
        const result = await this.msalService
          .loginPopup(OAuthSettings)
          .toPromise()
          .catch((reason) => {
            this.alertsService.addError('Login failed',
              JSON.stringify(reason, null, 2));
          });

        if (result) {
          this.msalService.instance.setActiveAccount(result.account);
          this.authenticated = true;
          // Temporary placeholder
          this.user = new User();
          this.user.displayName = 'Adele Vance';
          this.user.email = 'AdeleV@contoso.com';
          this.user.avatar = '/assets/no-profile-photo.png';
        }
      }

      // Sign out
      async signOut(): Promise<void> {
        await this.msalService.logout().toPromise();
        this.user = undefined;
        this.authenticated = false;
      }

      // Silently request an access token
      async getAccessToken(): Promise<string> {
        const result = await this.msalService
          .acquireTokenSilent({
            scopes: OAuthSettings.scopes
          })
          .toPromise()
          .catch((reason) => {
            this.alertsService.addError('Get token failed', JSON.stringify(reason, null, 2));
          });

        if (result) {
          // Temporary to display token in an error box
          this.alertsService.addSuccess('Token acquired', result.accessToken);
          return result.accessToken;
        }

        // Couldn't get a token
        this.authenticated = false;
        return '';
      }
    }
    ```

1. Abra **./src/app/nav-bar/nav-bar.component.ts** e substitua seu conteúdo pelo seguinte.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/nav-bar/nav-bar.component.ts" id="navBarSnippet" highlight="3,15-22,24,34-36,38-40":::

1. Abra **./src/app/home/home.component.ts** e substitua seu conteúdo pelo seguinte.

    :::code language="typescript" source="snippets/snippets.ts" id="homeSnippet" highlight="3,13-20,22,26-33":::

Salve suas alterações e atualize o navegador. Clique no **botão Clique aqui para entrar** e você deve ser redirecionado para `https://login.microsoftonline.com` . Faça logon com sua conta da Microsoft e consenta com as permissões solicitadas. A página do aplicativo deve ser atualizada, mostrando o token.

### <a name="get-user-details"></a>Obter detalhes do usuário

No momento, o serviço de autenticação define valores constantes para o nome de exibição e o endereço de email do usuário. Agora que você tem um token de acesso, você pode obter detalhes do usuário do Microsoft Graph para que esses valores correspondam ao usuário atual.

1. Abra **./src/app/auth.service.ts** e adicione as instruções a seguir `import` à parte superior do arquivo.

    ```typescript
    import { Client } from '@microsoft/microsoft-graph-client';
    import * as MicrosoftGraph from '@microsoft/microsoft-graph-types';
    ```

1. Adicione uma nova função à classe `AuthService` chamada `getUser`.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/auth.service.ts" id="GetUserSnippet":::

1. Localize e remova o código a seguir `getAccessToken` no método que adiciona um alerta para exibir o token de acesso.

    ```typescript
    // Temporary to display token in an error box
    this.alertsService.addSuccess('Token acquired', result);
    ```

1. Localize e remova o código a seguir do `signIn` método.

    ```typescript
    // Temporary placeholder
    this.user = new User();
    this.user.displayName = "Adele Vance";
    this.user.email = "AdeleV@contoso.com";
    this.user.avatar = '/assets/no-profile-photo.png';
    ```

1. Em seu lugar, adicione o código a seguir.

    ```typescript
    this.user = await this.getUser();
    ```

    Esse novo código usa o SDK do Microsoft Graph para obter os detalhes do usuário e cria um objeto usando valores retornados `User` pela chamada da API.

1. Altere o para a classe para verificar se o usuário já está conectado e `constructor` carregar seus detalhes em caso `AuthService` afirmado. Substitua o existente `constructor` pelo seguinte.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/auth.service.ts" id="ConstructorSnippet" highlight="5-7":::

1. Remova o código temporário da `HomeComponent` classe. Abra **./src/app/home/home.component.ts** e substitua a função `signIn` existente pelo seguinte.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/home/home.component.ts" id="SignInSnippet":::

Agora, se você salvar suas alterações e iniciar o aplicativo, depois de entrar, você deve terminar de volta na home page, mas a interface do usuário deve mudar para indicar que você está in-loco.

![Uma captura de tela da página inicial após o login](./images/add-aad-auth-01.png)

Clique no avatar do usuário no canto superior direito para acessar o link **Sair.** Clicar em **Sair redefine** a sessão e retorna você para a home page.

![Uma captura de tela do menu suspenso com o link Sair](./images/add-aad-auth-02.png)

## <a name="storing-and-refreshing-tokens"></a>Armazenar e atualizar tokens

Neste ponto, seu aplicativo tem um token de acesso, que é enviado no `Authorization` header de chamadas da API. Esse é o token que permite que o aplicativo acesse o Microsoft Graph em nome do usuário.

No entanto, esse token tem vida curta. O token expira uma hora após a emissão. Como o aplicativo está usando a biblioteca MSAL, você não precisa implementar nenhum armazenamento de token ou lógica de atualização. O `MsalService` cache do token no armazenamento do navegador. O método primeiro verifica o token armazenado em `acquireTokenSilent` cache e, se não estiver expirado, ele o retornará. Se expirar, ele fará uma solicitação silenciosa para obter um novo.
