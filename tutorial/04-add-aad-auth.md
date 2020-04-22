<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="88680-101">Neste exercício, você estenderá o aplicativo do exercício anterior para oferecer suporte à autenticação com o Azure AD.</span><span class="sxs-lookup"><span data-stu-id="88680-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="88680-102">Isso é necessário para obter o token de acesso OAuth necessário para chamar o Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="88680-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="88680-103">Nesta etapa, você integrará a [biblioteca de autenticação da Microsoft para o angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="88680-103">In this step you will integrate the [Microsoft Authentication Library for Angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) into the application.</span></span>

1. <span data-ttu-id="88680-104">Crie um novo arquivo no `./src` diretório chamado `oauth.ts` e adicione o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="88680-104">Create a new file in the `./src` directory named `oauth.ts` and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/oauth.ts.example":::

    <span data-ttu-id="88680-105">Substitua `YOUR_APP_ID_HERE` pela ID do aplicativo do portal de registro do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="88680-105">Replace `YOUR_APP_ID_HERE` with the application ID from the Application Registration Portal.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="88680-106">Se você estiver usando o controle de origem como o Git, agora seria uma boa hora para excluir `oauth.ts` o arquivo do controle de origem para evitar vazar inadvertidamente sua ID de aplicativo.</span><span class="sxs-lookup"><span data-stu-id="88680-106">If you're using source control such as git, now would be a good time to exclude the `oauth.ts` file from source control to avoid inadvertently leaking your app ID.</span></span>

1. <span data-ttu-id="88680-107">Abra `./src/app/app.module.ts` e adicione as seguintes `import` instruções à parte superior do arquivo.</span><span class="sxs-lookup"><span data-stu-id="88680-107">Open `./src/app/app.module.ts` and add the following `import` statements to the top of the file.</span></span>

    ```TypeScript
    import { MsalModule } from '@azure/msal-angular';
    import { OAuthSettings } from '../oauth';
    ```

1. <span data-ttu-id="88680-108">Adicione o `MsalModule` à `imports` matriz dentro da `@NgModule` declaração e inicialize-o com a ID do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="88680-108">Add the `MsalModule` to the `imports` array inside the `@NgModule` declaration, and initialize it with the app ID.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/app.module.ts" id="imports":::

## <a name="implement-sign-in"></a><span data-ttu-id="88680-109">Implementar logon</span><span class="sxs-lookup"><span data-stu-id="88680-109">Implement sign-in</span></span>

<span data-ttu-id="88680-110">Nesta seção, você criará um serviço de autenticação e implementará a entrada e a saída.</span><span class="sxs-lookup"><span data-stu-id="88680-110">In this section you'll create an authentication service and implement sign-in and sign-out.</span></span>

1. <span data-ttu-id="88680-111">Execute o seguinte comando em sua CLI.</span><span class="sxs-lookup"><span data-stu-id="88680-111">Run the following command in your CLI.</span></span>

    ```Shell
    ng generate service auth
    ```

    <span data-ttu-id="88680-112">Ao criar um serviço para isso, você pode injetar-o facilmente em qualquer componente que precise acessar os métodos de autenticação.</span><span class="sxs-lookup"><span data-stu-id="88680-112">By creating a service for this, you can easily inject it into any components that need access to authentication methods.</span></span>

1. <span data-ttu-id="88680-113">Quando o comando terminar, abra o `./src/app/auth.service.ts` arquivo e substitua seu conteúdo pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="88680-113">Once the command finishes, open the `./src/app/auth.service.ts` file and replace its contents with the following code.</span></span>

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

1. <span data-ttu-id="88680-114">Abra o `./src/app/nav-bar/nav-bar.component.ts` arquivo e substitua seu conteúdo pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="88680-114">Open the `./src/app/nav-bar/nav-bar.component.ts` file and replace its contents with the following.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/nav-bar/nav-bar.component.ts" id="navBarSnippet" highlight="3,15-22,24,26-28,36-38,40-42":::

1. <span data-ttu-id="88680-115">Abra`./src/app/home/home.component.ts` e substitua seu conteúdo pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="88680-115">Open`./src/app/home/home.component.ts` and replace its contents with the following.</span></span>

    :::code language="typescript" source="snippets/snippets.ts" id="homeSnippet" highlight="3,12-19,21,23,25-27":::

<span data-ttu-id="88680-116">Salve suas alterações e atualize o navegador.</span><span class="sxs-lookup"><span data-stu-id="88680-116">Save your changes and refresh the browser.</span></span> <span data-ttu-id="88680-117">Clique no botão **clique aqui para entrar** e você deve ser redirecionado para `https://login.microsoftonline.com`o.</span><span class="sxs-lookup"><span data-stu-id="88680-117">Click the **Click here to sign in** button and you should be redirected to `https://login.microsoftonline.com`.</span></span> <span data-ttu-id="88680-118">Faça logon com sua conta da Microsoft e concorde com as permissões solicitadas.</span><span class="sxs-lookup"><span data-stu-id="88680-118">Login with your Microsoft account and consent to the requested permissions.</span></span> <span data-ttu-id="88680-119">A página do aplicativo deve ser atualizada, mostrando o token.</span><span class="sxs-lookup"><span data-stu-id="88680-119">The app page should refresh, showing the token.</span></span>

### <a name="get-user-details"></a><span data-ttu-id="88680-120">Obter detalhes do usuário</span><span class="sxs-lookup"><span data-stu-id="88680-120">Get user details</span></span>

<span data-ttu-id="88680-121">No momento, o serviço de autenticação define valores constantes para o nome de exibição e endereço de email do usuário.</span><span class="sxs-lookup"><span data-stu-id="88680-121">Right now the authentication service sets constant values for the user's display name and email address.</span></span> <span data-ttu-id="88680-122">Agora que você tem um token de acesso, você pode obter detalhes do usuário do Microsoft Graph para que esses valores correspondam ao usuário atual.</span><span class="sxs-lookup"><span data-stu-id="88680-122">Now that you have an access token, you can get user details from Microsoft Graph so those values correspond to the current user.</span></span>

1. <span data-ttu-id="88680-123">Abra `./src/app/auth.service.ts` e adicione a seguinte `import` instrução à parte superior do arquivo.</span><span class="sxs-lookup"><span data-stu-id="88680-123">Open `./src/app/auth.service.ts` and add the following `import` statement to the top of the file.</span></span>

    ```TypeScript
    import { Client } from '@microsoft/microsoft-graph-client';
    ```

1. <span data-ttu-id="88680-124">Adicione uma nova função à classe `AuthService` chamada `getUser`.</span><span class="sxs-lookup"><span data-stu-id="88680-124">Add a new function to the `AuthService` class called `getUser`.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/auth.service.ts" id="getUserSnippet":::

1. <span data-ttu-id="88680-125">Localize e remova o código a seguir no `getAccessToken` método que adiciona um alerta para exibir o token de acesso.</span><span class="sxs-lookup"><span data-stu-id="88680-125">Locate and remove the following code in the `getAccessToken` method that adds an alert to display the access token.</span></span>

    ```TypeScript
    // Temporary to display token in an error box
    this.alertsService.add('Token acquired', result);
    ```

1. <span data-ttu-id="88680-126">Localize e remova o código a seguir do `signIn` método.</span><span class="sxs-lookup"><span data-stu-id="88680-126">Locate and remove the following code from the `signIn` method.</span></span>

    ```TypeScript
    // Temporary placeholder
    this.user = new User();
    this.user.displayName = "Adele Vance";
    this.user.email = "AdeleV@contoso.com";
    ```

1. <span data-ttu-id="88680-127">Em seu lugar, adicione o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="88680-127">In its place, add the following code.</span></span>

    ```TypeScript
    this.user = await this.getUser();
    ```

    <span data-ttu-id="88680-128">Este novo código usa o SDK do Microsoft Graph para obter os detalhes do usuário e, em `User` seguida, cria um objeto usando valores retornados pela chamada à API.</span><span class="sxs-lookup"><span data-stu-id="88680-128">This new code uses the Microsoft Graph SDK to get the user's details, then creates a `User` object using values returned by the API call.</span></span>

1. <span data-ttu-id="88680-129">Altere o `constructor` para a `AuthService` classe para verificar se o usuário já está conectado e carregar seus detalhes em caso afirmativo.</span><span class="sxs-lookup"><span data-stu-id="88680-129">Change the `constructor` for the `AuthService` class to check if the user is already logged in and load their details if so.</span></span> <span data-ttu-id="88680-130">Substitua o existente `constructor` pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="88680-130">Replace the existing `constructor` with the following.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/auth.service.ts" id="constructorSnippet" highlight="5-6":::

1. <span data-ttu-id="88680-131">Remova o código temporário da `HomeComponent` classe.</span><span class="sxs-lookup"><span data-stu-id="88680-131">Remove the temporary code from the `HomeComponent` class.</span></span> <span data-ttu-id="88680-132">Abra o `./src/app/home/home.component.ts` arquivo e substitua a função `signIn` existente pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="88680-132">Open the `./src/app/home/home.component.ts` file and replace the existing `signIn` function with the following.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/home/home.component.ts" id="signInSnippet" highlight="5-6":::

<span data-ttu-id="88680-133">Agora, se você salvar as alterações e iniciar o aplicativo, depois de entrar, você deverá terminar de volta na Home Page, mas a interface do usuário deverá mudar para indicar que você está conectado.</span><span class="sxs-lookup"><span data-stu-id="88680-133">Now if you save your changes and start the app, after sign-in you should end up back on the home page, but the UI should change to indicate that you are signed-in.</span></span>

![Uma captura de tela da Home Page após entrar](./images/add-aad-auth-01.png)

<span data-ttu-id="88680-135">Clique no avatar do usuário no canto superior direito para **acessar o link sair.**</span><span class="sxs-lookup"><span data-stu-id="88680-135">Click the user avatar in the top right corner to access the **Sign Out** link.</span></span> <span data-ttu-id="88680-136">Clicar **em sair** redefine a sessão e retorna à Home Page.</span><span class="sxs-lookup"><span data-stu-id="88680-136">Clicking **Sign Out** resets the session and returns you to the home page.</span></span>

![Uma captura de tela do menu suspenso com o link sair](./images/add-aad-auth-02.png)

## <a name="storing-and-refreshing-tokens"></a><span data-ttu-id="88680-138">Armazenar e atualizar tokens</span><span class="sxs-lookup"><span data-stu-id="88680-138">Storing and refreshing tokens</span></span>

<span data-ttu-id="88680-139">Nesse ponto, seu aplicativo tem um token de acesso, que é enviado no `Authorization` cabeçalho das chamadas de API.</span><span class="sxs-lookup"><span data-stu-id="88680-139">At this point your application has an access token, which is sent in the `Authorization` header of API calls.</span></span> <span data-ttu-id="88680-140">Este é o token que permite que o aplicativo acesse o Microsoft Graph em nome do usuário.</span><span class="sxs-lookup"><span data-stu-id="88680-140">This is the token that allows the app to access the Microsoft Graph on the user's behalf.</span></span>

<span data-ttu-id="88680-141">No entanto, esse token é de vida curta.</span><span class="sxs-lookup"><span data-stu-id="88680-141">However, this token is short-lived.</span></span> <span data-ttu-id="88680-142">O token expira uma hora após sua emissão.</span><span class="sxs-lookup"><span data-stu-id="88680-142">The token expires an hour after it is issued.</span></span> <span data-ttu-id="88680-143">Como o aplicativo está usando a biblioteca MSAL, você não precisa implementar qualquer lógica de armazenamento ou de atualização.</span><span class="sxs-lookup"><span data-stu-id="88680-143">Because the app is using the MSAL library, you do not have to implement any token storage or refresh logic.</span></span> <span data-ttu-id="88680-144">O `MsalService` armazena em cache o token no armazenamento do navegador.</span><span class="sxs-lookup"><span data-stu-id="88680-144">The `MsalService` caches the token in the browser storage.</span></span> <span data-ttu-id="88680-145">O `acquireTokenSilent` método primeiro verifica o token em cache e, se ele não tiver expirado, ele o retornará.</span><span class="sxs-lookup"><span data-stu-id="88680-145">The `acquireTokenSilent` method first checks the cached token, and if it is not expired, it returns it.</span></span> <span data-ttu-id="88680-146">Se ele tiver expirado, ele fará uma solicitação silenciosa para obter um novo.</span><span class="sxs-lookup"><span data-stu-id="88680-146">If it is expired, it makes a silent request to obtain a new one.</span></span>
