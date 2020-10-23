<!-- markdownlint-disable MD002 MD041 -->

Nesta seção, você adicionará a capacidade de criar eventos no calendário do usuário.

1. Abra **./src/app/Graph.Service.TS** e adicione a função a seguir à `GraphService` classe.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/graph.service.ts" id="AddEventSnippet":::

## <a name="create-a-new-event-form"></a>Criar um novo formulário de eventos

1. Criar um componente angular para exibir um formulário e chamar essa nova função. Execute o seguinte comando em sua CLI.

    ```Shell
    ng generate component new-event
    ```

1. Depois que o comando for concluído, adicione o componente à `routes` matriz em **./src/app/app-Routing.Module.TS**.

    ```typescript
    import { NewEventComponent } from './new-event/new-event.component';

    const routes: Routes = [
      { path: '', component: HomeComponent },
      { path: 'calendar', component: CalendarComponent },
      { path: 'newevent', component: NewEventComponent },
    ];
    ```

1. Crie um novo arquivo no diretório **./src/App/New-Event** chamado **New-Event. TS** e adicione o código a seguir.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/new-event/new-event.ts" id="NewEventSnippet":::

    Essa classe servirá como o modelo para o novo formulário de eventos.

1. Abra **./src/App/New-Event/New-Event.Component.TS** e substitua seu conteúdo pelo código a seguir.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/new-event/new-event.component.ts" id="NewEventComponentSnippet":::

1. Abra **./src/app/new-event/new-event.component.html** e substitua seu conteúdo pelo código a seguir.

    :::code language="html" source="../demo/graph-tutorial/src/app/new-event/new-event.component.html" id="NewEventFormSnippet":::

1. Salve as alterações e atualize o aplicativo. Selecione o botão **novo evento** na página calendário e, em seguida, use o formulário para criar um evento no calendário do usuário.

    ![Uma captura de tela do novo formulário de evento](images/create-event.png)
