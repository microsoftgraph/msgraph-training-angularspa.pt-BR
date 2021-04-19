<!-- markdownlint-disable MD002 MD041 -->

Neste exercício, você incorporará o Microsoft Graph ao aplicativo. Para esse aplicativo, você usará a biblioteca [microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) para fazer chamadas para o Microsoft Graph.

## <a name="get-calendar-events-from-outlook"></a>Obtenha eventos de calendário do Outlook

1. Adicione um novo serviço para manter todas as chamadas do Graph. Execute o seguinte comando em sua CLI.

    ```Shell
    ng generate service graph
    ```

    Assim como com o serviço de autenticação criado anteriormente, a criação de um serviço para isso permite que você o injete em todos os componentes que precisem de acesso ao Microsoft Graph.

1. Depois que o comando é concluído, abra **./src/app/graph.service.ts** e substitua seu conteúdo pelo seguinte.

    ```typescript
    import { Injectable } from '@angular/core';
    import { Client } from '@microsoft/microsoft-graph-client';
    import * as MicrosoftGraph from '@microsoft/microsoft-graph-types';

    import { AuthService } from './auth.service';
    import { AlertsService } from './alerts.service';

    @Injectable({
      providedIn: 'root'
    })

    export class GraphService {

      private graphClient: Client;
      constructor(
        private authService: AuthService,
        private alertsService: AlertsService) {

        // Initialize the Graph client
        this.graphClient = Client.init({
          authProvider: async (done) => {
            // Get the token from the auth service
            const token = await this.authService.getAccessToken()
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
      }

      async getCalendarView(start: string, end: string, timeZone: string): Promise<MicrosoftGraph.Event[] | undefined> {
        try {
          // GET /me/calendarview?startDateTime=''&endDateTime=''
          // &$select=subject,organizer,start,end
          // &$orderby=start/dateTime
          // &$top=50
          const result =  await this.graphClient
            .api('/me/calendarview')
            .header('Prefer', `outlook.timezone="${timeZone}"`)
            .query({
              startDateTime: start,
              endDateTime: end
            })
            .select('subject,organizer,start,end')
            .orderby('start/dateTime')
            .top(50)
            .get();

          return result.value;
        } catch (error) {
          this.alertsService.addError('Could not get events', JSON.stringify(error, null, 2));
        }
        return undefined;
      }
    }
    ```

    Considere o que este código está fazendo.

    - Ele inicializa um cliente Graph no construtor do serviço.
    - Ele implementa uma `getCalendarView` função que usa o cliente Graph da seguinte maneira:
      - O URL que será chamado é `/me/calendarview`.
      - O `header` método inclui o header, que faz com que os horários de início e término dos eventos retornados sejam no fuso horário preferencial `Prefer: outlook.timezone` do usuário.
      - O `query` método adiciona os `startDateTime` `endDateTime` parâmetros e, definindo a janela de tempo para o modo de exibição de calendário.
      - O método limita os campos retornados para cada evento para apenas aqueles que o modo de exibição `select` realmente usará.
      - O `orderby` método classifica os resultados por hora de início.

1. Crie um componente Angular para chamar esse novo método e exibir os resultados da chamada. Execute o seguinte comando em sua CLI.

    ```Shell
    ng generate component calendar
    ```

1. Depois que o comando é concluído, adicione o componente à `routes` matriz em **./src/app/app-routing.module.ts**.

    ```typescript
    import { CalendarComponent } from './calendar/calendar.component';

    const routes: Routes = [
      { path: '', component: HomeComponent },
      { path: 'calendar', component: CalendarComponent },
    ];
    ```

1. Abra **./tsconfig.jse** adicione a propriedade a seguir ao `compilerOptions` objeto.

    ```json
    "resolveJsonModule": true
    ```

1. Abra **./src/app/calendar/calendar.component.ts** e substitua seu conteúdo pelo seguinte.

    ```typescript
    import { Component, OnInit } from '@angular/core';
    import * as moment from 'moment-timezone';
    import { findIana } from 'windows-iana';
    import * as MicrosoftGraph from '@microsoft/microsoft-graph-types';

    import { AuthService } from '../auth.service';
    import { GraphService } from '../graph.service';
    import { AlertsService } from '../alerts.service';

    @Component({
      selector: 'app-calendar',
      templateUrl: './calendar.component.html',
      styleUrls: ['./calendar.component.css']
    })
    export class CalendarComponent implements OnInit {

      public events?: MicrosoftGraph.Event[];

      constructor(
        private authService: AuthService,
        private graphService: GraphService,
        private alertsService: AlertsService) { }

      ngOnInit() {
        // Convert the user's timezone to IANA format
        const ianaName = findIana(this.authService.user?.timeZone ?? 'UTC');
        const timeZone = ianaName![0].valueOf() || this.authService.user?.timeZone || 'UTC';

        // Get midnight on the start of the current week in the user's timezone,
        // but in UTC. For example, for Pacific Standard Time, the time value would be
        // 07:00:00Z
        var startOfWeek = moment.tz(timeZone).startOf('week').utc();
        var endOfWeek = moment(startOfWeek).add(7, 'day');

        this.graphService.getCalendarView(
          startOfWeek.format(),
          endOfWeek.format(),
          this.authService.user?.timeZone ?? 'UTC')
            .then((events) => {
              this.events = events;
              // Temporary to display raw results
              this.alertsService.addSuccess('Events from Graph', JSON.stringify(events, null, 2));
            });
      }
    }
    ```

Por enquanto, isso apenas renderiza a matriz de eventos no JSON na página. Salve suas alterações e reinicie o aplicativo. Entre e clique no link **Calendário** na barra de nav. Se tudo funcionar, você deverá ver um despejo JSON de eventos no calendário do usuário.

## <a name="display-the-results"></a>Exibir os resultados

Agora você pode atualizar o componente para exibir os eventos de maneira mais `CalendarComponent` fácil de usar.

1. Remova o código temporário que adiciona um alerta da `ngOnInit` função. Sua função atualizada deve ter esta aparência.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/calendar/calendar.component.ts" id="ngOnInitSnippet":::

1. Adicione uma função à classe `CalendarComponent` para formatar `DateTimeTimeZone` um objeto em uma cadeia de caracteres ISO.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/calendar/calendar.component.ts" id="formatDateTimeTimeZoneSnippet":::

1. Abra **./src/app/calendar/calendar.component.html** e substitua seu conteúdo pelo seguinte.

    :::code language="html" source="../demo/graph-tutorial/src/app/calendar/calendar.component.html" id="calendarHtml":::

Isso faz um loop pela coleção de eventos e adiciona uma linha de tabela para cada um deles. Salve as alterações e reinicie o aplicativo. Clique no link **Calendário** e o aplicativo deve renderizar uma tabela de eventos.

![Uma captura de tela da tabela de eventos](./images/add-msgraph-01.png)
