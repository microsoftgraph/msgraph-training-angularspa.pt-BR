<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="efd7e-101">Neste exercício, você estenderá o aplicativo do exercício anterior para oferecer suporte à autenticação com o Azure AD.</span><span class="sxs-lookup"><span data-stu-id="efd7e-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="efd7e-102">Isso é necessário para obter o token de acesso OAuth necessário para chamar o Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="efd7e-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="efd7e-103">Nesta etapa, você integrará a [biblioteca de autenticação da Microsoft para o angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="efd7e-103">In this step you will integrate the [Microsoft Authentication Library for Angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) into the application.</span></span>

<span data-ttu-id="efd7e-104">Crie um novo arquivo no `./src` diretório chamado `oauth.ts` e adicione o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="efd7e-104">Create a new file in the `./src` directory named `oauth.ts` and add the following code.</span></span>

```TypeScript
export const OAuthSettings = {
  appId: 'YOUR_APP_ID_HERE',
  scopes: [
    "user.read",
    "calendars.read"
  ]
};
```

<span data-ttu-id="efd7e-105">Substitua `YOUR_APP_ID_HERE` pela ID do aplicativo do portal de registro do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="efd7e-105">Replace `YOUR_APP_ID_HERE` with the application ID from the Application Registration Portal.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="efd7e-106">Se você estiver usando o controle de origem como o Git, agora seria uma boa hora para excluir `oauth.ts` o arquivo do controle de origem para evitar vazar inadvertidamente sua ID de aplicativo.</span><span class="sxs-lookup"><span data-stu-id="efd7e-106">If you're using source control such as git, now would be a good time to exclude the `oauth.ts` file from source control to avoid inadvertently leaking your app ID.</span></span>

<span data-ttu-id="efd7e-107">Abra `./src/app/app.module.ts` e adicione as seguintes `import` instruções à parte superior do arquivo.</span><span class="sxs-lookup"><span data-stu-id="efd7e-107">Open `./src/app/app.module.ts` and add the following `import` statements to the top of the file.</span></span>

```TypeScript
import { MsalModule } from '@azure/msal-angular';
import { OAuthSettings } from '../oauth';
```

<span data-ttu-id="efd7e-108">Em seguida, `MsalModule` adicione o `imports` à matriz dentro `@NgModule` da declaração e inicialize-o com a ID do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="efd7e-108">Then add the `MsalModule` to the `imports` array inside the `@NgModule` declaration, and initialize it with the app ID.</span></span>

```TypeScript
imports: [
  BrowserModule,
  AppRoutingModule,
  NgbModule,
  FontAwesomeModule,
  MsalModule.forRoot({
    clientID: OAuthSettings.appId
  })
],
```

## <a name="implement-sign-in"></a><span data-ttu-id="efd7e-109">Implementar logon</span><span class="sxs-lookup"><span data-stu-id="efd7e-109">Implement sign-in</span></span>

<span data-ttu-id="efd7e-110">Comece definindo uma classe simples `User` para armazenar as informações sobre o usuário que o aplicativo exibe.</span><span class="sxs-lookup"><span data-stu-id="efd7e-110">Start by defining a simple `User` class to hold the information about the user that the app displays.</span></span> <span data-ttu-id="efd7e-111">Crie um novo arquivo na `./src/app` pasta chamada `user.ts` e adicione o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="efd7e-111">Create a new file in the `./src/app` folder named `user.ts` and add the following code.</span></span>

```TypeScript
export class User {
  displayName: string;
  email: string;
  avatar: string;
}
```

<span data-ttu-id="efd7e-112">Agora, crie um serviço de autenticação.</span><span class="sxs-lookup"><span data-stu-id="efd7e-112">Now create an authentication service.</span></span> <span data-ttu-id="efd7e-113">Ao criar um serviço para isso, você pode injetar-o facilmente em qualquer componente que precise acessar os métodos de autenticação.</span><span class="sxs-lookup"><span data-stu-id="efd7e-113">By creating a service for this, you can easily inject it into any components that need access to authentication methods.</span></span> <span data-ttu-id="efd7e-114">Execute o seguinte comando em sua CLI.</span><span class="sxs-lookup"><span data-stu-id="efd7e-114">Run the following command in your CLI.</span></span>

```Shell
ng generate service auth
```

<span data-ttu-id="efd7e-115">Quando o comando terminar, abra o `./src/app/auth.service.ts` arquivo e substitua seu conteúdo pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="efd7e-115">Once the command finishes, open the `./src/app/auth.service.ts` file and replace its contents with the following code.</span></span>

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
    let result = await this.msalService.loginPopup(OAuthSettings.scopes)
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
    let result = await this.msalService.acquireTokenSilent(OAuthSettings.scopes)
      .catch((reason) => {
        this.alertsService.add('Get token failed', JSON.stringify(reason, null, 2));
      });

    // Temporary to display token in an error box
    if (result) this.alertsService.add('Token acquired', result);
    return result;
  }
}
```

<span data-ttu-id="efd7e-116">Agora que você tem o serviço de autenticação, ele pode ser injetado nos componentes que fazem logon.</span><span class="sxs-lookup"><span data-stu-id="efd7e-116">Now that you have the authentication service, it can be injected into the components that do sign-in.</span></span> <span data-ttu-id="efd7e-117">Comece com o `NavBarComponent`.</span><span class="sxs-lookup"><span data-stu-id="efd7e-117">Start with the `NavBarComponent`.</span></span> <span data-ttu-id="efd7e-118">Abra o `./src/app/nav-bar/nav-bar.component.ts` arquivo e faça as seguintes alterações.</span><span class="sxs-lookup"><span data-stu-id="efd7e-118">Open the `./src/app/nav-bar/nav-bar.component.ts` file and make the following changes.</span></span>

- <span data-ttu-id="efd7e-119">Adicionar `import { AuthService } from '../auth.service';` à parte superior do arquivo.</span><span class="sxs-lookup"><span data-stu-id="efd7e-119">Add `import { AuthService } from '../auth.service';` to the top of the file.</span></span>
- <span data-ttu-id="efd7e-120">Remova as `authenticated` propriedades `user` e da classe e remova o código que as define `ngOnInit`.</span><span class="sxs-lookup"><span data-stu-id="efd7e-120">Remove the `authenticated` and `user` properties from the class, and remove the code that sets them in `ngOnInit`.</span></span>
- <span data-ttu-id="efd7e-121">Injetar `AuthService` o adicionando o seguinte parâmetro ao `constructor`:. `private authService: AuthService`</span><span class="sxs-lookup"><span data-stu-id="efd7e-121">Inject the `AuthService` by adding the following parameter to the `constructor`: `private authService: AuthService`.</span></span>
- <span data-ttu-id="efd7e-122">Substitua o método `signIn` existente pelo seguinte:</span><span class="sxs-lookup"><span data-stu-id="efd7e-122">Replace the existing `signIn` method with the following:</span></span>

    ```TypeScript
    async signIn(): Promise<void> {
      await this.authService.signIn();
    }
    ```

- <span data-ttu-id="efd7e-123">Substitua o método `signOut` existente pelo seguinte:</span><span class="sxs-lookup"><span data-stu-id="efd7e-123">Replace the existing `signOut` method with the following:</span></span>

    ```TypeScript
    signOut(): void {
      this.authService.signOut();
    }
    ```

<span data-ttu-id="efd7e-124">Quando você terminar, o código deverá ser semelhante ao seguinte.</span><span class="sxs-lookup"><span data-stu-id="efd7e-124">When you're done, the code should look like the following.</span></span>

```TypeScript
import { Component, OnInit } from '@angular/core';

import { AuthService } from '../auth.service';

@Component({
  selector: 'app-nav-bar',
  templateUrl: './nav-bar.component.html',
  styleUrls: ['./nav-bar.component.css']
})
export class NavBarComponent implements OnInit {

  // Should the collapsed nav show?
  showNav: boolean;

  constructor(private authService: AuthService) { }

  ngOnInit() {
    this.showNav = false;
  }

  // Used by the Bootstrap navbar-toggler button to hide/show
  // the nav in a collapsed state
  toggleNavBar(): void {
    this.showNav = !this.showNav;
  }

  async signIn(): Promise<void> {
    await this.authService.signIn();
  }

  signOut(): void {
    this.authService.signOut();
  }
}
```

<span data-ttu-id="efd7e-125">Desde que você `authenticated` removeu `user` as propriedades e na classe, você também precisa atualizar o `./src/app/nav-bar/nav-bar.component.html` arquivo.</span><span class="sxs-lookup"><span data-stu-id="efd7e-125">Since you removed the `authenticated` and `user` properties on the class, you also need to update the `./src/app/nav-bar/nav-bar.component.html` file.</span></span> <span data-ttu-id="efd7e-126">Abra esse arquivo e faça as seguintes alterações.</span><span class="sxs-lookup"><span data-stu-id="efd7e-126">Open that file and make the following changes.</span></span>

- <span data-ttu-id="efd7e-127">Substitua todas as instâncias do `authenticated` por `authService.authenticated`.</span><span class="sxs-lookup"><span data-stu-id="efd7e-127">Replace all instances of `authenticated` with `authService.authenticated`.</span></span>
- <span data-ttu-id="efd7e-128">Substitua todas as instâncias `user` de `authService.user`por.</span><span class="sxs-lookup"><span data-stu-id="efd7e-128">Replace all instance of `user` with `authService.user`.</span></span>

<span data-ttu-id="efd7e-129">Em seguida, `HomeComponent` atualize a classe.</span><span class="sxs-lookup"><span data-stu-id="efd7e-129">Next update the `HomeComponent` class.</span></span> <span data-ttu-id="efd7e-130">Faça todas as alterações que você fez `./src/app/home/home.component.ts` na `NavBarComponent` classe com as seguintes exceções.</span><span class="sxs-lookup"><span data-stu-id="efd7e-130">Make all of the same changes in `./src/app/home/home.component.ts` that you made to the `NavBarComponent` class with the following exceptions.</span></span>

- <span data-ttu-id="efd7e-131">Não há nenhum `signOut` método na `HomeComponent` classe.</span><span class="sxs-lookup"><span data-stu-id="efd7e-131">There is no `signOut` method in the `HomeComponent` class.</span></span>
- <span data-ttu-id="efd7e-132">Substitua o `signIn` método por uma versão um pouco diferente.</span><span class="sxs-lookup"><span data-stu-id="efd7e-132">Replace the `signIn` method with a slightly different version.</span></span> <span data-ttu-id="efd7e-133">Este código chama `getAccessToken` para obter um token de acesso, que, por sua vez, irá gerar o token como um erro.</span><span class="sxs-lookup"><span data-stu-id="efd7e-133">This code calls `getAccessToken` to get an access token, which, for now, will output the token as an error.</span></span>

    ```TypeScript
    async signIn(): Promise<void> {
      await this.authService.signIn();

      // Temporary to display the token
      if (this.authService.authenticated) {
        let token = await this.authService.getAccessToken();
      }
    }
    ```

<span data-ttu-id="efd7e-134">Quando terminar, o arquivo deverá ter a seguinte aparência.</span><span class="sxs-lookup"><span data-stu-id="efd7e-134">When your done, the file should look like the following.</span></span>

```TypeScript
import { Component, OnInit } from '@angular/core';
import { AuthService } from '../auth.service';

@Component({
  selector: 'app-home',
  templateUrl: './home.component.html',
  styleUrls: ['./home.component.css']
})
export class HomeComponent implements OnInit {

  constructor(private authService: AuthService) { }

  ngOnInit() {
  }

  async signIn(): Promise<void> {
    await this.authService.signIn();

    // Temporary to display the token
    if (this.authService.authenticated) {
      let token = await this.authService.getAccessToken();
    }
  }
}
```

<span data-ttu-id="efd7e-135">Por fim, faça as mesmas substituições `./src/app/home/home.component.html` que você fez para a barra de navegação.</span><span class="sxs-lookup"><span data-stu-id="efd7e-135">Finally, make the same replacements in `./src/app/home/home.component.html` that you made for the nav bar.</span></span>

<span data-ttu-id="efd7e-136">Salve suas alterações e atualize o navegador.</span><span class="sxs-lookup"><span data-stu-id="efd7e-136">Save your changes and refresh the browser.</span></span> <span data-ttu-id="efd7e-137">Clique no botão **clique aqui para entrar** e você deve ser redirecionado para `https://login.microsoftonline.com`o.</span><span class="sxs-lookup"><span data-stu-id="efd7e-137">Click the **Click here to sign in** button and you should be redirected to `https://login.microsoftonline.com`.</span></span> <span data-ttu-id="efd7e-138">Faça logon com sua conta da Microsoft e concorde com as permissões solicitadas.</span><span class="sxs-lookup"><span data-stu-id="efd7e-138">Login with your Microsoft account and consent to the requested permissions.</span></span> <span data-ttu-id="efd7e-139">A página do aplicativo deve ser atualizada, mostrando o token.</span><span class="sxs-lookup"><span data-stu-id="efd7e-139">The app page should refresh, showing the token.</span></span>

### <a name="get-user-details"></a><span data-ttu-id="efd7e-140">Obter detalhes do usuário</span><span class="sxs-lookup"><span data-stu-id="efd7e-140">Get user details</span></span>

<span data-ttu-id="efd7e-141">No momento, o serviço de autenticação define valores constantes para o nome de exibição e endereço de email do usuário.</span><span class="sxs-lookup"><span data-stu-id="efd7e-141">Right now the authentication service sets constant values for the user's display name and email address.</span></span> <span data-ttu-id="efd7e-142">Agora que você tem um token de acesso, você pode obter detalhes do usuário do Microsoft Graph para que esses valores correspondam ao usuário atual.</span><span class="sxs-lookup"><span data-stu-id="efd7e-142">Now that you have an access token, you can get user details from Microsoft Graph so those values correspond to the current user.</span></span> <span data-ttu-id="efd7e-143">Abra `./src/app/auth.service.ts` e adicione a seguinte `import` instrução à parte superior do arquivo.</span><span class="sxs-lookup"><span data-stu-id="efd7e-143">Open `./src/app/auth.service.ts` and add the following `import` statement to the top of the file.</span></span>

```TypeScript
import { Client } from '@microsoft/microsoft-graph-client';
```

<span data-ttu-id="efd7e-144">Adicione uma nova função à classe `AuthService` chamada `getUser`.</span><span class="sxs-lookup"><span data-stu-id="efd7e-144">Add a new function to the `AuthService` class called `getUser`.</span></span>

```TypeScript
private async getUser(): Promise<User> {
  if (!this.authenticated) return null;

  let graphClient = Client.init({
    // Initialize the Graph client with an auth
    // provider that requests the token from the
    // auth service
    authProvider: async(done) => {
      let token = await this.getAccessToken()
        .catch((reason) => {
          done(reason, null);
        });

      if (token)
      {
        done(null, token);
      } else {
        done("Could not get an access token", null);
      }
    }
  });

  // Get the user from Graph (GET /me)
  let graphUser = await graphClient.api('/me').get();

  let user = new User();
  user.displayName = graphUser.displayName;
  // Prefer the mail property, but fall back to userPrincipalName
  user.email = graphUser.mail || graphUser.userPrincipalName;

  return user;
}
```

<span data-ttu-id="efd7e-145">Localize e remova o código a seguir do `signIn` método.</span><span class="sxs-lookup"><span data-stu-id="efd7e-145">Locate and remove the following code from the `signIn` method.</span></span>

```TypeScript
// Temporary placeholder
this.user = new User();
this.user.displayName = "Adele Vance";
this.user.email = "AdeleV@contoso.com";
```

<span data-ttu-id="efd7e-146">Em seu lugar, adicione o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="efd7e-146">In its place, add the following code.</span></span>

```TypeScript
this.user = await this.getUser();
```

<span data-ttu-id="efd7e-147">Este novo código usa o SDK do Microsoft Graph para obter os detalhes do usuário e, em `User` seguida, cria um objeto usando valores retornados pela chamada à API.</span><span class="sxs-lookup"><span data-stu-id="efd7e-147">This new code uses the Microsoft Graph SDK to get the user's details, then creates a `User` object using values returned by the API call.</span></span>

<span data-ttu-id="efd7e-148">Agora altere o `constructor` para a `AuthService` classe para verificar se o usuário já está conectado e carregar seus detalhes, se for o caso.</span><span class="sxs-lookup"><span data-stu-id="efd7e-148">Now change the `constructor` for the `AuthService` class to check if the user is already logged in and load their details if so.</span></span> <span data-ttu-id="efd7e-149">Substitua o existente `constructor` pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="efd7e-149">Replace the existing `constructor` with the following.</span></span>

```TypeScript
constructor(
  private msalService: MsalService,
  private alertsService: AlertsService) {

  this.authenticated = this.msalService.getUser() != null;
  this.getUser().then((user) => {this.user = user});
}
```

<span data-ttu-id="efd7e-150">Por fim, remova o código temporário da `HomeComponent` classe.</span><span class="sxs-lookup"><span data-stu-id="efd7e-150">Finally, remove the temporary code from the `HomeComponent` class.</span></span> <span data-ttu-id="efd7e-151">Abra o `./src/app/home/home.component.ts` arquivo e substitua a função `signIn` existente pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="efd7e-151">Open the `./src/app/home/home.component.ts` file and replace the existing `signIn` function with the following.</span></span>

```TypeScript
async signIn(): Promise<void> {
  await this.authService.signIn();
}
```

<span data-ttu-id="efd7e-152">Agora, se você salvar as alterações e iniciar o aplicativo, depois de entrar, você deverá terminar de volta na Home Page, mas a interface do usuário deverá mudar para indicar que você está conectado.</span><span class="sxs-lookup"><span data-stu-id="efd7e-152">Now if you save your changes and start the app, after sign-in you should end up back on the home page, but the UI should change to indicate that you are signed-in.</span></span>

![Uma captura de tela da Home Page após entrar](./images/add-aad-auth-01.png)

<span data-ttu-id="efd7e-154">Clique no avatar do usuário no canto superior direito para acessar o \*\*\*\* link sair.</span><span class="sxs-lookup"><span data-stu-id="efd7e-154">Click the user avatar in the top right corner to access the **Sign Out** link.</span></span> <span data-ttu-id="efd7e-155">Clicar \*\*\*\* em sair redefine a sessão e retorna à Home Page.</span><span class="sxs-lookup"><span data-stu-id="efd7e-155">Clicking **Sign Out** resets the session and returns you to the home page.</span></span>

![Uma captura de tela do menu suspenso com o link sair](./images/add-aad-auth-02.png)

## <a name="storing-and-refreshing-tokens"></a><span data-ttu-id="efd7e-157">Armazenar e atualizar tokens</span><span class="sxs-lookup"><span data-stu-id="efd7e-157">Storing and refreshing tokens</span></span>

<span data-ttu-id="efd7e-158">Nesse ponto, seu aplicativo tem um token de acesso, que é enviado no `Authorization` cabeçalho das chamadas de API.</span><span class="sxs-lookup"><span data-stu-id="efd7e-158">At this point your application has an access token, which is sent in the `Authorization` header of API calls.</span></span> <span data-ttu-id="efd7e-159">Este é o token que permite que o aplicativo acesse o Microsoft Graph em nome do usuário.</span><span class="sxs-lookup"><span data-stu-id="efd7e-159">This is the token that allows the app to access the Microsoft Graph on the user's behalf.</span></span>

<span data-ttu-id="efd7e-160">No entanto, esse token é de vida curta.</span><span class="sxs-lookup"><span data-stu-id="efd7e-160">However, this token is short-lived.</span></span> <span data-ttu-id="efd7e-161">O token expira uma hora após sua emissão.</span><span class="sxs-lookup"><span data-stu-id="efd7e-161">The token expires an hour after it is issued.</span></span> <span data-ttu-id="efd7e-162">Como o aplicativo está usando a biblioteca MSAL, você não precisa implementar qualquer lógica de armazenamento ou de atualização.</span><span class="sxs-lookup"><span data-stu-id="efd7e-162">Because the app is using the MSAL library, you do not have to implement any token storage or refresh logic.</span></span> <span data-ttu-id="efd7e-163">O `MsalService` armazena em cache o token no armazenamento do navegador.</span><span class="sxs-lookup"><span data-stu-id="efd7e-163">The `MsalService` caches the token in the browser storage.</span></span> <span data-ttu-id="efd7e-164">O `acquireTokenSilent` método primeiro verifica o token em cache e, se ele não tiver expirado, ele o retornará.</span><span class="sxs-lookup"><span data-stu-id="efd7e-164">The `acquireTokenSilent` method first checks the cached token, and if it is not expired, it returns it.</span></span> <span data-ttu-id="efd7e-165">Se ele tiver expirado, ele fará uma solicitação silenciosa para obter um novo.</span><span class="sxs-lookup"><span data-stu-id="efd7e-165">If it is expired, it makes a silent request to obtain a new one.</span></span>