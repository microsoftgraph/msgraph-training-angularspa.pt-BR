<!-- markdownlint-disable MD002 MD041 -->

Neste exercício, você incorporará o Microsoft Graph no aplicativo. Para este aplicativo, você usará a biblioteca [Microsoft-Graph-Client](https://github.com/microsoftgraph/msgraph-sdk-javascript) para fazer chamadas para o Microsoft Graph.

## <a name="get-calendar-events-from-outlook"></a>Obter eventos de calendário do Outlook

1. Crie um novo arquivo no `./src/app` diretório chamado `event.ts` e adicione o código a seguir.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/event.ts" id="eventClasses":::

1. Adicione um novo serviço para manter todas as chamadas de gráfico. Execute o seguinte comando em sua CLI.

    ```Shell
    ng generate service graph
    ```

    Assim como o serviço de autenticação que você criou anteriormente, a criação de um serviço para isso permite que você o insira em qualquer componente que precise acessar o Microsoft Graph.

1. Quando o comando for concluído, abra o `./src/app/graph.service.ts` arquivo e substitua seu conteúdo pelo seguinte.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/graph.service.ts" id="graphServiceSnippet":::

    Considere o que esse código está fazendo.

    - Ele Inicializa um cliente do Graph no construtor para o serviço.
    - Ele implementa uma `getEvents` função que usa o cliente gráfico da seguinte maneira:
      - A URL que será chamada é `/me/events`.
      - O `select` método limita os campos retornados para cada evento para apenas aqueles que o modo de exibição realmente usará.
      - O `orderby` método classifica os resultados pela data e hora em que foram criados, com o item mais recente em primeiro lugar.

1. Criar um componente angular para chamar este novo método e exibir os resultados da chamada. Execute o seguinte comando em sua CLI.

    ```Shell
    ng generate component calendar
    ```

1. Depois que o comando for concluído, adicione o componente à `routes` matriz no `./src/app/app-routing.module.ts`.

    ```TypeScript
    import { CalendarComponent } from './calendar/calendar.component';

    const routes: Routes = [
      { path: '', component: HomeComponent },
      { path: 'calendar', component: CalendarComponent }
    ];
    ```

1. Abra o `./src/app/calendar/calendar.component.ts` arquivo e substitua seu conteúdo pelo seguinte.

    ```TypeScript
    import { Component, OnInit } from '@angular/core';
    import * as moment from 'moment-timezone';

    import { GraphService } from '../graph.service';
    import { Event, DateTimeTimeZone } from '../event';
    import { AlertsService } from '../alerts.service';

    @Component({
      selector: 'app-calendar',
      templateUrl: './calendar.component.html',
      styleUrls: ['./calendar.component.css']
    })
    export class CalendarComponent implements OnInit {

      public events: Event[];

      constructor(
        private graphService: GraphService,
        private alertsService: AlertsService) { }

      ngOnInit() {
        this.graphService.getEvents()
          .then((events) => {
            this.events = events;
            // Temporary to display raw results
            this.alertsService.add('Events from Graph', JSON.stringify(events, null, 2));
          });
      }
    }
    ```

Por ora, isso apenas renderiza a matriz de eventos em JSON na página. Salve suas alterações e reinicie o aplicativo. Entre e clique no link **calendário** na barra de navegação. Se tudo funcionar, você deverá ver um despejo JSON de eventos no calendário do usuário.

## <a name="display-the-results"></a>Exibir os resultados

Agora você pode atualizar o `CalendarComponent` componente para exibir os eventos de forma mais amigável.

1. Remova o código temporário que adiciona um alerta da `ngOnInit` função. A função updated deve ter a seguinte aparência.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/calendar/calendar.component.ts" id="ngOnInitSnippet":::

1. Adicione uma função à `CalendarComponent` classe para formatar um `DateTimeTimeZone` objeto em uma cadeia de caracteres ISO.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/calendar/calendar.component.ts" id="formatDateTimeTimeZoneSnippet":::

1. Abra o `./src/app/calendar/calendar.component.html` arquivo e substitua seu conteúdo pelo seguinte.

    :::code language="html" source="../demo/graph-tutorial/src/app/calendar/calendar.component.html" id="calendarHtml":::

Isso faz um loop pela coleção de eventos e adiciona uma linha de tabela para cada um. Salve as alterações e reinicie o aplicativo. Clique no link do **calendário** e o aplicativo agora deve renderizar uma tabela de eventos.

![Uma captura de tela da tabela de eventos](./images/add-msgraph-01.png)
