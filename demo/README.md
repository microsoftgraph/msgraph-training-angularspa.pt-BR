# <a name="how-to-run-the-completed-project"></a>Como executar o projeto concluído

## <a name="prerequisites"></a>Pré-requisitos

Para executar o projeto concluído nesta pasta, você precisa do seguinte:

- [Node.js](https://nodejs.org) instalado em sua máquina de desenvolvimento. Se você não tiver Node.js, visite o link anterior para opções de download. (**Observação:** este tutorial foi escrito com o Nó versão 14.15.0. As etapas neste guia podem funcionar com outras versões, mas que não foram testadas.)
- [CLI angular](https://cli.angular.io/) instalada em sua máquina de desenvolvimento.
- Uma conta pessoal da Microsoft com uma caixa de correio Outlook.com ou uma conta de estudante ou de trabalho da Microsoft.

Se você não tiver uma conta da Microsoft, há algumas opções para obter uma conta gratuita:

- Você pode [se inscrever em uma nova conta pessoal da Microsoft.](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1)
- Você pode se inscrever no Programa de Desenvolvedores do [Office 365](https://developer.microsoft.com/office/dev-program) para obter uma assinatura gratuita do Office 365.

## <a name="register-a-web-application-with-the-azure-active-directory-admin-center"></a>Registrar um aplicativo Web no Centro de administração do Azure Active Directory

1. Abra um navegador e navegue até o [centro de administração do Azure Active Directory](https://aad.portal.azure.com). Faça logon usando uma **conta pessoal** (também conhecida como Conta da Microsoft) ou **Conta Corporativa ou de Estudante**.

1. Selecione **Azure Active Directory** na navegação esquerda e selecione **Registros de aplicativos** em **Gerenciar**.

    ![Uma captura de tela dos registros do aplicativo ](/tutorial/images/aad-portal-app-registrations.png)

1. Selecione **Novo registro**. Na página **Registrar um aplicativo**, defina os valores da seguinte forma.

    - Defina **Nome** para `Angular Graph Tutorial`.
    - Defina **Tipos de conta com suporte** para **Contas em qualquer diretório organizacional e contas pessoais da Microsoft**.
    - Em **URI de Redirecionamento**, defina o primeiro menu suspenso para `Single-page application (SPA)` e defina o valor como `http://localhost:4200`.

    ![Captura de tela da página Registrar um aplicativo](/tutorial/images/aad-register-an-app.png)

1. Escolha **Registrar**. Na página **Tutorial do Angular Graph,** copie o valor da ID do Aplicativo **(cliente)** e salve-a, você precisará dela na próxima etapa.

    ![Captura de tela da ID do aplicativo do novo registro do aplicativo](/tutorial/images/aad-application-id.png)

## <a name="configure-the-sample"></a>Configurar o exemplo

1. Renomeie o `oauth.ts.example` arquivo para `oauth.ts` .
1. Edite `oauth.ts` o arquivo e faça as seguintes alterações.
    1. Substitua `YOUR_APP_ID_HERE` pela **ID do aplicativo que** você recebeu do Portal de Registro de Aplicativos.
1. Em sua interface de linha de comando (CLI), navegue até esse diretório e execute o seguinte comando para instalar os requisitos.

    ```Shell
    npm install
    ```

## <a name="run-the-sample"></a>Executar o exemplo

1. Execute o seguinte comando em sua CLI para iniciar o aplicativo.

    ```Shell
    ng serve
    ```

1. Abra um navegador e navegue até `http://localhost:4200`.
