# NLW-Unite_CSharp
___
## Especificações

### pass.in

O pass.in é uma aplicação de gestão de participantes em eventos presenciais.
A ferramenta permite que o organizador cadastre um evento e abra uma página pública de inscrição.
Os participantes inscritos podem emitir uma credencial para check-in no dia do evento.
O sistema fará um scan da credencial do participante para permitir a entrada no evento.
___
### Requisitos

#### Requisitos funcionais
* [ ] O organizador deve poder cadastrar um novo evento;
* [ ] O organizador deve poder visualizar dados de um evento;
* [ ] O organizador deve poser visualizar a lista de participantes;
* [ ] O participante deve poder se inscrever em um evento;
* [ ] O participante deve poder visualizar seu crachá de inscrição;
* [ ] O participante deve poder realizar check-in no evento;


#### Regras de negócio
* [ ] O participante só pode se inscrever em um evento uma única vez;
* [ ] O participante só pode se inscrever em eventos com vagas disponíveis;
* [ ] O participante só pode realizar check-in em um evento uma única vez;

#### Requisitos não-funcionais
O check-in no evento será realizado através de um QRCode;
___
### Especificações da API

#### Banco de dados
Nessa aplicação vamos utilizar banco de dados relacional (SQL). Para ambiente de desenvolvimento seguiremos com o SQLite pela facilidade do ambiente.

#### Diagrama ERD

#### Estrutura do banco (SQL)
```
CREATE TABLE "events" (
    "id" TEXT NOT NULL PRIMARY KEY,
    "title" TEXT NOT NULL,
    "details" TEXT,
    "slug" TEXT NOT NULL,
    "maximum_attendees" INTEGER
);

CREATE TABLE "attendees" (
    "id" TEXT NOT NULL PRIMARY KEY,
    "name" TEXT NOT NULL,
    "email" TEXT NOT NULL,
    "event_id" TEXT NOT NULL,
    "created_at" DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
    
    CONSTRAINT "attendees_event_id_fkey" FOREIGN KEY ("event_id") REFERENCES "events" ("id") ON DELETE RESTRICT ON UPDATE CASCADE
);

CREATE TABLE "check_ins" (
    "id" INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT,
    "created_at" DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
    "attendeeId" TEXT NOT NULL,
    
    CONSTRAINT "check_ins_attendeeId_fkey" FOREIGN KEY ("attendeeId") REFERENCES "attendees" ("id") ON DELETE RESTRICT ON UPDATE CASCADE
);

CREATE UNIQUE INDEX "events_slug_key" ON "events"("slug");
CREATE UNIQUE INDEX "attendees_event_id_email_key" ON "attendees"("event_id", "email");
CREATE UNIQUE INDEX "check_ins_attendeeId_key" ON "check_ins"("attendeeId");
```

Criado durante Evento [NLW UNITE 2024](https://www.rocketseat.com.br/eventos/nlw/convite/tiago-23574), promovido pela [Rocketseat](https://www.rocketseat.com.br/).