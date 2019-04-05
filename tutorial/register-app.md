<!-- markdownlint-disable MD002 MD041 -->

Neste exercício, você criará um novo registro de aplicativo Web do Azure AD usando o centro de administração do Azure Active Directory.

1. Abra um navegador e navegue até o [centro de administração do Azure Active Directory](https://aad.portal.azure.com). Faça logon usando uma **conta pessoal** (aka: conta da Microsoft) ou **conta corporativa ou de estudante**.

1. Selecione **Azure Active Directory** na navegação à esquerda e, em seguida, selecione **registros de aplicativo (visualização)** em **gerenciar**.

    ![Uma captura de tela dos registros de aplicativo ](./images/aad-portal-app-registrations.png)

1. Selecione **novo registro**. Na página **registrar um aplicativo** , defina os valores da seguinte maneira.

    - Defina **** o nome `Angular Graph Tutorial`como.
    - Defina os **tipos de conta com suporte** para **contas em qualquer diretório organizacional e contas pessoais da Microsoft**.
    - Em **URI**de redirecionamento, defina o primeiro menu `Web` suspenso como e defina o `http://localhost:4200`valor como.

    ![Uma captura de tela da página registrar um aplicativo](./images/aad-register-an-app.png)

1. Escolha **registrar**. Na página **tutorial do gráfico angular** , copie o valor da **ID do aplicativo (cliente)** e salve-o, você precisará dele na próxima etapa.

    ![Uma captura de tela da ID do aplicativo do novo registro de aplicativo](./images/aad-application-id.png)

1. Selecione **autenticação** em **gerenciar**. Localize a seção **Grant implícita** e habilite tokens de **acesso** e tokens de **ID**. Selecione **Salvar**.

    ![Uma captura de tela da seção Grant implícita](./images/aad-implicit-grant.png)
