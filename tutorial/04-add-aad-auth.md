<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="961bb-101">Neste exercício, você estenderá o aplicativo do exercício anterior para dar suporte à autenticação com o Azure AD.</span><span class="sxs-lookup"><span data-stu-id="961bb-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="961bb-102">Isso é necessário para obter o token de acesso OAuth necessário para chamar o Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="961bb-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="961bb-103">Nesta etapa, você integrará a Biblioteca de [Autenticação da Microsoft para Angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) ao aplicativo.</span><span class="sxs-lookup"><span data-stu-id="961bb-103">In this step you will integrate the [Microsoft Authentication Library for Angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) into the application.</span></span>

1. <span data-ttu-id="961bb-104">Crie um novo arquivo no **diretório ./src** chamado **oauth.ts** e adicione o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="961bb-104">Create a new file in the **./src** directory named **oauth.ts** and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/oauth.example.ts":::

    <span data-ttu-id="961bb-105">Substitua `YOUR_APP_ID_HERE` pela ID do aplicativo no Portal de Registro de Aplicativos.</span><span class="sxs-lookup"><span data-stu-id="961bb-105">Replace `YOUR_APP_ID_HERE` with the application ID from the Application Registration Portal.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="961bb-106">Se você estiver usando o controle de origem, como git, agora seria um bom momento para excluir o arquivo **oauth.ts** do controle de origem para evitar o vazamento inadvertida da ID do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="961bb-106">If you're using source control such as git, now would be a good time to exclude the **oauth.ts** file from source control to avoid inadvertently leaking your app ID.</span></span>

1. <span data-ttu-id="961bb-107">Abra **./src/app/app.module.ts** e adicione as instruções a seguir `import` à parte superior do arquivo.</span><span class="sxs-lookup"><span data-stu-id="961bb-107">Open **./src/app/app.module.ts** and add the following `import` statements to the top of the file.</span></span>

    ```typescript
    import { IPublicClientApplication,
             PublicClientApplication,
             BrowserCacheLocation } from '@azure/msal-browser';
    import { MsalModule,
             MsalService,
             MSAL_INSTANCE } from '@azure/msal-angular';
    import { OAuthSettings } from '../oauth';
    ```

1. <span data-ttu-id="961bb-108">Adicione a seguinte função abaixo das `import` instruções.</span><span class="sxs-lookup"><span data-stu-id="961bb-108">Add the following function below the `import` statements.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/app.module.ts" id="MSALFactorySnippet":::

1. <span data-ttu-id="961bb-109">Adicione a `MsalModule` matriz `imports` à `@NgModule` declaração.</span><span class="sxs-lookup"><span data-stu-id="961bb-109">Add the `MsalModule` to the `imports` array inside the `@NgModule` declaration.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/app.module.ts" id="ImportsSnippet" highlight="6":::

1. <span data-ttu-id="961bb-110">Adicione e `MSALInstanceFactory` à matriz dentro da `MsalService` `providers` `@NgModule` declaração.</span><span class="sxs-lookup"><span data-stu-id="961bb-110">Add the `MSALInstanceFactory` and `MsalService` to the `providers` array inside the `@NgModule` declaration.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/app.module.ts" id="ProvidersSnippet" highlight="2-6":::

## <a name="implement-sign-in"></a><span data-ttu-id="961bb-111">Implementar login</span><span class="sxs-lookup"><span data-stu-id="961bb-111">Implement sign-in</span></span>

<span data-ttu-id="961bb-112">Nesta seção, você criará um serviço de autenticação e implementará a assinatura e a saída.</span><span class="sxs-lookup"><span data-stu-id="961bb-112">In this section you'll create an authentication service and implement sign-in and sign-out.</span></span>

1. <span data-ttu-id="961bb-113">Execute o seguinte comando em sua CLI.</span><span class="sxs-lookup"><span data-stu-id="961bb-113">Run the following command in your CLI.</span></span>

    ```Shell
    ng generate service auth
    ```

    <span data-ttu-id="961bb-114">Ao criar um serviço para isso, você pode injetá-lo facilmente em todos os componentes que precisem de acesso aos métodos de autenticação.</span><span class="sxs-lookup"><span data-stu-id="961bb-114">By creating a service for this, you can easily inject it into any components that need access to authentication methods.</span></span>

1. <span data-ttu-id="961bb-115">Depois que o comando terminar, abra **./src/app/auth.service.ts** e substitua seu conteúdo pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="961bb-115">Once the command finishes, open **./src/app/auth.service.ts** and replace its contents with the following code.</span></span>

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

1. <span data-ttu-id="961bb-116">Abra **./src/app/nav-bar/nav-bar.component.ts** e substitua seu conteúdo pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="961bb-116">Open **./src/app/nav-bar/nav-bar.component.ts** and replace its contents with the following.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/nav-bar/nav-bar.component.ts" id="navBarSnippet" highlight="3,15-22,24,34-36,38-40":::

1. <span data-ttu-id="961bb-117">Abra **./src/app/home/home.component.ts** e substitua seu conteúdo pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="961bb-117">Open **./src/app/home/home.component.ts** and replace its contents with the following.</span></span>

    :::code language="typescript" source="snippets/snippets.ts" id="homeSnippet" highlight="3,13-20,22,26-33":::

<span data-ttu-id="961bb-118">Salve suas alterações e atualize o navegador.</span><span class="sxs-lookup"><span data-stu-id="961bb-118">Save your changes and refresh the browser.</span></span> <span data-ttu-id="961bb-119">Clique no **botão Clique aqui para entrar** e você deve ser redirecionado para `https://login.microsoftonline.com` .</span><span class="sxs-lookup"><span data-stu-id="961bb-119">Click the **Click here to sign in** button and you should be redirected to `https://login.microsoftonline.com`.</span></span> <span data-ttu-id="961bb-120">Faça logon com sua conta da Microsoft e consenta com as permissões solicitadas.</span><span class="sxs-lookup"><span data-stu-id="961bb-120">Login with your Microsoft account and consent to the requested permissions.</span></span> <span data-ttu-id="961bb-121">A página do aplicativo deve ser atualizada, mostrando o token.</span><span class="sxs-lookup"><span data-stu-id="961bb-121">The app page should refresh, showing the token.</span></span>

### <a name="get-user-details"></a><span data-ttu-id="961bb-122">Obter detalhes do usuário</span><span class="sxs-lookup"><span data-stu-id="961bb-122">Get user details</span></span>

<span data-ttu-id="961bb-123">No momento, o serviço de autenticação define valores constantes para o nome de exibição e o endereço de email do usuário.</span><span class="sxs-lookup"><span data-stu-id="961bb-123">Right now the authentication service sets constant values for the user's display name and email address.</span></span> <span data-ttu-id="961bb-124">Agora que você tem um token de acesso, você pode obter detalhes do usuário do Microsoft Graph para que esses valores correspondam ao usuário atual.</span><span class="sxs-lookup"><span data-stu-id="961bb-124">Now that you have an access token, you can get user details from Microsoft Graph so those values correspond to the current user.</span></span>

1. <span data-ttu-id="961bb-125">Abra **./src/app/auth.service.ts** e adicione as instruções a seguir `import` à parte superior do arquivo.</span><span class="sxs-lookup"><span data-stu-id="961bb-125">Open **./src/app/auth.service.ts** and add the following `import` statements to the top of the file.</span></span>

    ```typescript
    import { Client } from '@microsoft/microsoft-graph-client';
    import * as MicrosoftGraph from '@microsoft/microsoft-graph-types';
    ```

1. <span data-ttu-id="961bb-126">Adicione uma nova função à classe `AuthService` chamada `getUser`.</span><span class="sxs-lookup"><span data-stu-id="961bb-126">Add a new function to the `AuthService` class called `getUser`.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/auth.service.ts" id="GetUserSnippet":::

1. <span data-ttu-id="961bb-127">Localize e remova o código a seguir `getAccessToken` no método que adiciona um alerta para exibir o token de acesso.</span><span class="sxs-lookup"><span data-stu-id="961bb-127">Locate and remove the following code in the `getAccessToken` method that adds an alert to display the access token.</span></span>

    ```typescript
    // Temporary to display token in an error box
    this.alertsService.addSuccess('Token acquired', result);
    ```

1. <span data-ttu-id="961bb-128">Localize e remova o código a seguir do `signIn` método.</span><span class="sxs-lookup"><span data-stu-id="961bb-128">Locate and remove the following code from the `signIn` method.</span></span>

    ```typescript
    // Temporary placeholder
    this.user = new User();
    this.user.displayName = "Adele Vance";
    this.user.email = "AdeleV@contoso.com";
    this.user.avatar = '/assets/no-profile-photo.png';
    ```

1. <span data-ttu-id="961bb-129">Em seu lugar, adicione o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="961bb-129">In its place, add the following code.</span></span>

    ```typescript
    this.user = await this.getUser();
    ```

    <span data-ttu-id="961bb-130">Esse novo código usa o SDK do Microsoft Graph para obter os detalhes do usuário e cria um objeto usando valores retornados `User` pela chamada da API.</span><span class="sxs-lookup"><span data-stu-id="961bb-130">This new code uses the Microsoft Graph SDK to get the user's details, then creates a `User` object using values returned by the API call.</span></span>

1. <span data-ttu-id="961bb-131">Altere o para a classe para verificar se o usuário já está conectado e `constructor` carregar seus detalhes em caso `AuthService` afirmado.</span><span class="sxs-lookup"><span data-stu-id="961bb-131">Change the `constructor` for the `AuthService` class to check if the user is already logged in and load their details if so.</span></span> <span data-ttu-id="961bb-132">Substitua o existente `constructor` pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="961bb-132">Replace the existing `constructor` with the following.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/auth.service.ts" id="ConstructorSnippet" highlight="5-7":::

1. <span data-ttu-id="961bb-133">Remova o código temporário da `HomeComponent` classe.</span><span class="sxs-lookup"><span data-stu-id="961bb-133">Remove the temporary code from the `HomeComponent` class.</span></span> <span data-ttu-id="961bb-134">Abra **./src/app/home/home.component.ts** e substitua a função `signIn` existente pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="961bb-134">Open **./src/app/home/home.component.ts** and replace the existing `signIn` function with the following.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/home/home.component.ts" id="SignInSnippet":::

<span data-ttu-id="961bb-135">Agora, se você salvar suas alterações e iniciar o aplicativo, depois de entrar, você deve terminar de volta na home page, mas a interface do usuário deve mudar para indicar que você está in-loco.</span><span class="sxs-lookup"><span data-stu-id="961bb-135">Now if you save your changes and start the app, after sign-in you should end up back on the home page, but the UI should change to indicate that you are signed-in.</span></span>

![Uma captura de tela da página inicial após o login](./images/add-aad-auth-01.png)

<span data-ttu-id="961bb-137">Clique no avatar do usuário no canto superior direito para acessar o link **Sair.**</span><span class="sxs-lookup"><span data-stu-id="961bb-137">Click the user avatar in the top right corner to access the **Sign Out** link.</span></span> <span data-ttu-id="961bb-138">Clicar em **Sair redefine** a sessão e retorna você para a home page.</span><span class="sxs-lookup"><span data-stu-id="961bb-138">Clicking **Sign Out** resets the session and returns you to the home page.</span></span>

![Uma captura de tela do menu suspenso com o link Sair](./images/add-aad-auth-02.png)

## <a name="storing-and-refreshing-tokens"></a><span data-ttu-id="961bb-140">Armazenar e atualizar tokens</span><span class="sxs-lookup"><span data-stu-id="961bb-140">Storing and refreshing tokens</span></span>

<span data-ttu-id="961bb-141">Neste ponto, seu aplicativo tem um token de acesso, que é enviado no `Authorization` header de chamadas da API.</span><span class="sxs-lookup"><span data-stu-id="961bb-141">At this point your application has an access token, which is sent in the `Authorization` header of API calls.</span></span> <span data-ttu-id="961bb-142">Esse é o token que permite que o aplicativo acesse o Microsoft Graph em nome do usuário.</span><span class="sxs-lookup"><span data-stu-id="961bb-142">This is the token that allows the app to access the Microsoft Graph on the user's behalf.</span></span>

<span data-ttu-id="961bb-143">No entanto, esse token tem vida curta.</span><span class="sxs-lookup"><span data-stu-id="961bb-143">However, this token is short-lived.</span></span> <span data-ttu-id="961bb-144">O token expira uma hora após a emissão.</span><span class="sxs-lookup"><span data-stu-id="961bb-144">The token expires an hour after it is issued.</span></span> <span data-ttu-id="961bb-145">Como o aplicativo está usando a biblioteca MSAL, você não precisa implementar nenhum armazenamento de token ou lógica de atualização.</span><span class="sxs-lookup"><span data-stu-id="961bb-145">Because the app is using the MSAL library, you do not have to implement any token storage or refresh logic.</span></span> <span data-ttu-id="961bb-146">O `MsalService` cache do token no armazenamento do navegador.</span><span class="sxs-lookup"><span data-stu-id="961bb-146">The `MsalService` caches the token in the browser storage.</span></span> <span data-ttu-id="961bb-147">O método primeiro verifica o token armazenado em `acquireTokenSilent` cache e, se não estiver expirado, ele o retornará.</span><span class="sxs-lookup"><span data-stu-id="961bb-147">The `acquireTokenSilent` method first checks the cached token, and if it is not expired, it returns it.</span></span> <span data-ttu-id="961bb-148">Se expirar, ele fará uma solicitação silenciosa para obter um novo.</span><span class="sxs-lookup"><span data-stu-id="961bb-148">If it is expired, it makes a silent request to obtain a new one.</span></span>
