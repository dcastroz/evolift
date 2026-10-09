# Contrato inicial da API REST — Evolift

> Proposta documental da Fase 1. As rotas e os exemplos abaixo descrevem o funcionamento planejado do Evolift; a API ainda não foi implementada. Os formatos e as validações serão confirmados antes do desenvolvimento.

## Convenções

- Prefixo previsto: `/api/v1`.
- Requisições e respostas usam JSON (`Content-Type: application/json`); datas e horários usam ISO 8601 com fuso horário.
- As rotas de dados exigem autenticação, exceto criação de conta e login. A proposta é utilizar sessões do Django com cookies `HttpOnly` e `Secure` em produção, além de proteção CSRF nas operações de escrita.
- Os dados de treinos, sessões, exercícios pessoais, histórico e desempenho pertencem ao usuário autenticado. O acesso a um recurso pessoal de outra pessoa deve retornar `404`, sem revelar sua existência.
- As rotas `/admin/` exigem o perfil `administrador`. O administrador pode gerenciar contas e o catálogo geral, mas não recebe acesso automático aos treinos e históricos pessoais dos usuários.
- O cadastro público cria contas com perfil `usuario` e situação ativa. Um usuário comum não pode definir o próprio perfil como administrador.
- IDs numéricos são ilustrativos; o formato definitivo dependerá do banco e da implementação.
- Dados inválidos devem retornar `400`; conflitos de estado ou de unicidade usam `409` quando aplicável. As mensagens de erro seguem o formato apresentado no final deste documento.
- Listagens são paginadas. O valor padrão proposto para `limit` é 20, e o valor máximo será definido na implementação.
- No modelo de dados, os campos `perfil` e `ativo` de `USUARIO` aparecem na API como `role` e `active`. O campo `origem` de `EXERCICIO` aparece como `origin`, com os valores `pessoal`, `geral` ou `wger`.
- O horário de início de uma sessão é `started_at`, o de término é `finished_at`, e o estado é `status` (`em_andamento` ou `concluida`). Esses campos correspondem a `iniciado_em`, `finalizado_em` e `status` no modelo de dados.

## Rotas previstas

### Contas e autenticação

| Método e rota | Uso | Autenticação | Sucesso esperado | Outros status relevantes |
|---|---|---|---|---|
| `POST /api/v1/users` | Criar conta de usuário comum (UC01) | Não | `201 Created` | `400` dados inválidos; `409` e-mail já cadastrado |
| `POST /api/v1/auth/login` | Realizar login (UC02) | Não | `200 OK` e cookie de sessão | `400` formato inválido; `401` credenciais incorretas; `403` conta inativa |
| `POST /api/v1/auth/logout` | Encerrar sessão autenticada | Sim | `204 No Content` | `401` sessão inválida; `403` CSRF inválido |

### Exercícios e treinos

| Método e rota | Uso | Autenticação | Sucesso esperado | Outros status relevantes |
|---|---|---|---|---|
| `GET /api/v1/exercises` | Buscar exercícios próprios e dos catálogos disponíveis (UC05) | Sim | `200 OK` | `400` filtros inválidos; `401` |
| `POST /api/v1/exercises` | Cadastrar exercício pessoal (UC12) | Sim | `201 Created` | `400`, `401` |
| `GET /api/v1/exercises/{id}` | Consultar exercício acessível ao usuário | Sim | `200 OK` | `401`, `404` |
| `PATCH /api/v1/exercises/{id}` | Editar somente exercício pessoal do usuário (UC12) | Sim | `200 OK` | `400`, `401`, `404` |
| `DELETE /api/v1/exercises/{id}` | Excluir exercício pessoal sem dependências históricas | Sim | `204 No Content` | `401`, `404`, `409` se referenciado por treino ou sessão |
| `GET /api/v1/external/exercises` | Pesquisar o catálogo da wger normalizado e armazenado em cache | Sim | `200 OK` | `400`, `401`, `503` se a fonte estiver indisponível e não houver cache |
| `GET /api/v1/workouts` | Listar treinos do usuário (UC03) | Sim | `200 OK` | `401` |
| `POST /api/v1/workouts` | Criar treino (UC03) | Sim | `201 Created` | `400`, `401` |
| `GET /api/v1/workouts/{id}` | Consultar treino e seus exercícios (UC03) | Sim | `200 OK` | `401`, `404` |
| `PATCH /api/v1/workouts/{id}` | Editar treino próprio (UC03) | Sim | `200 OK` | `400`, `401`, `404` |
| `DELETE /api/v1/workouts/{id}` | Excluir treino que não possua sessões registradas | Sim | `204 No Content` | `401`, `404`, `409` se houver histórico |
| `POST /api/v1/workouts/{id}/exercises` | Adicionar exercício a um treino (UC04) | Sim | `201 Created` | `400`, `401`, `404`, `409` posição duplicada |
| `DELETE /api/v1/workouts/{id}/exercises/{workoutExerciseId}` | Remover exercício do treino planejado, preservando sessões já registradas | Sim | `204 No Content` | `401`, `404` |

### Sessões, histórico, evolução e relatórios

| Método e rota | Uso | Autenticação | Sucesso esperado | Outros status relevantes |
|---|---|---|---|---|
| `POST /api/v1/workout-sessions` | Iniciar uma sessão e registrar seu horário de início (UC06) | Sim | `201 Created` | `400`, `401`, `404` |
| `POST /api/v1/workout-sessions/{id}/exercises` | Registrar exercício realizado na sessão em andamento | Sim | `201 Created` | `400`, `401`, `404`, `409` sessão concluída |
| `POST /api/v1/workout-sessions/{id}/exercises/{sessionExerciseId}/sets` | Registrar uma série, repetições e carga (UC13) | Sim | `201 Created` | `400`, `401`, `404`, `409` número repetido ou sessão concluída |
| `PATCH /api/v1/workout-sessions/{id}/exercises/{sessionExerciseId}/sets/{setId}` | Corrigir uma série enquanto a sessão estiver em andamento (UC13) | Sim | `200 OK` | `400`, `401`, `404`, `409` sessão concluída |
| `POST /api/v1/workout-sessions/{id}/finish` | Finalizar a sessão, registrar o término e gerar obrigatoriamente o resumo (UC07 inclui UC08) | Sim | `200 OK` | `400`, `401`, `404`, `409` sessão já concluída ou estado incompatível |
| `GET /api/v1/workout-sessions` | Consultar sessões próprias por período ou treino (UC09) | Sim | `200 OK` | `400` filtros inválidos; `401` |
| `GET /api/v1/workout-sessions/{id}` | Consultar sessão própria com exercícios, séries e estado | Sim | `200 OK` | `401`, `404` |
| `GET /api/v1/workout-sessions/{id}/summary` | Consultar o resumo de sessão concluída (UC08) | Sim | `200 OK` | `401`, `404`, `409` sessão em andamento |
| `GET /api/v1/search?q={texto}` | Buscar treinos e exercícios disponíveis (UC05) | Sim | `200 OK` | `400` consulta inválida; `401` |
| `GET /api/v1/progress?exercise_id={id}&from={data}&to={data}` | Consultar evolução de cargas e repetições (UC14) | Sim | `200 OK` | `400`, `401`, `404` |
| `GET /api/v1/reports/summary?from={data}&to={data}` | Consultar relatório consolidado por período (UC15) | Sim | `200 OK` | `400` período inválido; `401` |

### Administração

| Método e rota | Uso | Autenticação | Sucesso esperado | Outros status relevantes |
|---|---|---|---|---|
| `GET /api/v1/admin/exercises` | Listar exercícios do catálogo geral (UC10) | Administrador | `200 OK` | `401`, `403` sem permissão |
| `POST /api/v1/admin/exercises` | Cadastrar exercício do catálogo geral (UC10) | Administrador | `201 Created` | `400`, `401`, `403` |
| `GET /api/v1/admin/exercises/{id}` | Consultar um exercício do catálogo geral (UC10) | Administrador | `200 OK` | `401`, `403`, `404` |
| `PATCH /api/v1/admin/exercises/{id}` | Editar exercício do catálogo geral (UC10) | Administrador | `200 OK` | `400`, `401`, `403`, `404` |
| `DELETE /api/v1/admin/exercises/{id}` | Excluir exercício geral quando não houver dependências de treinos ou sessões | Administrador | `204 No Content` | `401`, `403`, `404`, `409` se existirem dependências |
| `GET /api/v1/admin/users` | Listar e pesquisar contas cadastradas (UC11) | Administrador | `200 OK` | `401`, `403` |
| `GET /api/v1/admin/users/{id}` | Consultar dados administrativos de uma conta (UC11) | Administrador | `200 OK` | `401`, `403`, `404` |
| `PATCH /api/v1/admin/users/{id}` | Atualizar dados administrativos ou ativar/inativar conta (UC11) | Administrador | `200 OK` | `400`, `401`, `403`, `404` |

As rotas administrativas não permitem consultar treinos, cargas ou históricos pessoais. A retirada de um exercício já utilizado no histórico exige preservar as referências existentes. **Caso o grupo decida permitir ocultar exercícios ainda referenciados, será necessário acrescentar um atributo de disponibilidade ao modelo de dados antes da implementação**, em vez de realizar exclusão física.

### Filtros e regras gerais das rotas

- `GET /api/v1/exercises`: aceita `q`, `origin`, `limit` e `offset`. A origem pode ser `pessoal`, `geral` ou `wger`.
- `GET /api/v1/external/exercises`: aceita `q`, `limit` e `offset`. A busca por nome usa o catálogo externo já normalizado no cache local; não pressupõe filtro textual por nome na API wger.
- `GET /api/v1/workout-sessions`: pode aceitar `from`, `to`, `workout_id`, `status`, `limit` e `offset`. O histórico de sessões concluídas é identificado por `status = concluida`.
- Consultas de evolução e relatório usam os registros de sessões, exercícios realizados e séries do próprio usuário; não dependem da API wger.
- Operações de escrita em sessões concluídas são bloqueadas. Uma tentativa incompatível com o estado da sessão retorna `409`.
- A conclusão da sessão e a geração do resumo são previstas como uma única operação consistente: se não for possível confirmar o resumo obrigatório, a conclusão não deve ser confirmada. Os registros já salvos devem ser preservados para uma nova tentativa, evitando duplicação.

## Exemplos de requisição e resposta

### Criar conta

`POST /api/v1/users`

```json
{
  "email": "ana@example.com",
  "password": "exemplo-nao-reutilizar",
  "name": "Ana"
}
```

`201 Created`

```json
{
  "id": 42,
  "email": "ana@example.com",
  "name": "Ana",
  "role": "usuario",
  "active": true,
  "created_at": "2026-10-08T12:00:00-03:00"
}
```

A senha nunca retorna na resposta nem é armazenada em texto puro. O perfil inicial é sempre `usuario`. Um e-mail já cadastrado resulta em `409 Conflict`.

### Criar treino e incluir exercício

`POST /api/v1/workouts`

```json
{
  "name": "Treino A"
}
```

`201 Created`

```json
{
  "id": 15,
  "name": "Treino A",
  "exercises": [],
  "created_at": "2026-10-08T12:10:00-03:00"
}
```

`POST /api/v1/workouts/15/exercises`

```json
{
  "exercise_id": 87,
  "position": 1
}
```

`201 Created`

```json
{
  "id": 31,
  "workout_id": 15,
  "exercise_id": 87,
  "position": 1
}
```

### Iniciar treino, registrar exercício e série

`POST /api/v1/workout-sessions`

```json
{
  "workout_id": 15
}
```

`201 Created`

```json
{
  "id": 204,
  "workout_id": 15,
  "started_at": "2026-10-08T18:30:00-03:00",
  "finished_at": null,
  "status": "em_andamento",
  "exercises": []
}
```

O horário de início é registrado pelo sistema no momento da criação da sessão.

`POST /api/v1/workout-sessions/204/exercises`

```json
{
  "exercise_id": 87,
  "workout_exercise_id": 31,
  "position": 1
}
```

`201 Created`

```json
{
  "id": 91,
  "exercise_id": 87,
  "position": 1,
  "sets": []
}
```

`POST /api/v1/workout-sessions/204/exercises/91/sets`

```json
{
  "number": 1,
  "repetitions": 10,
  "load_kg": 40.0
}
```

`201 Created`

```json
{
  "id": 502,
  "number": 1,
  "repetitions": 10,
  "load_kg": 40.0
}
```

### Finalizar a sessão e gerar o resumo obrigatório

`POST /api/v1/workout-sessions/204/finish`

```json
{}
```

`200 OK`

```json
{
  "id": 204,
  "workout_id": 15,
  "status": "concluida",
  "started_at": "2026-10-08T18:30:00-03:00",
  "finished_at": "2026-10-08T19:45:00-03:00",
  "summary": {
    "duration_minutes": 75,
    "total_exercises": 1,
    "total_sets": 1,
    "total_repetitions": 10,
    "exercises": [
      {
        "exercise_id": 87,
        "name": "Agachamento",
        "sets": [
          { "number": 1, "repetitions": 10, "load_kg": 40.0 }
        ]
      }
    ]
  }
}
```

O resumo é gerado automaticamente na conclusão (UC07 `«include»` UC08), com base nos registros da própria sessão. Se houver falha que impeça gerar o resumo, o sistema não confirma a conclusão e preserva os registros já salvos.

### Consultar o resumo de uma sessão concluída

`GET /api/v1/workout-sessions/204/summary`

`200 OK`

```json
{
  "session_id": 204,
  "duration_minutes": 75,
  "total_exercises": 1,
  "total_sets": 1,
  "total_repetitions": 10,
  "exercises": [
    {
      "exercise_id": 87,
      "name": "Agachamento",
      "sets": [
        { "number": 1, "repetitions": 10, "load_kg": 40.0 }
      ]
    }
  ]
}
```

Para uma sessão ainda em andamento, a rota retorna `409 Conflict`.

### Administrar o catálogo geral

`POST /api/v1/admin/exercises`

```json
{
  "name": "Agachamento livre",
  "description": "Exercício de membros inferiores.",
  "muscle_group": "Pernas",
  "equipment": "Barra"
}
```

`201 Created`

```json
{
  "id": 120,
  "name": "Agachamento livre",
  "origin": "geral",
  "user_id": null,
  "muscle_group": "Pernas",
  "equipment": "Barra"
}
```

Somente o administrador pode criar ou editar itens do catálogo geral. Exercícios pessoais continuam sob controle do usuário proprietário.

### Gerenciar a situação de uma conta

`PATCH /api/v1/admin/users/42`

```json
{
  "active": false
}
```

`200 OK`

```json
{
  "id": 42,
  "email": "ana@example.com",
  "name": "Ana",
  "role": "usuario",
  "active": false
}
```

A inativação impede novos logins sem apagar os treinos ou os históricos da conta. A alteração de perfil exige autorização administrativa e validações próprias.

### Consultar evolução

`GET /api/v1/progress?exercise_id=87&from=2026-09-01&to=2026-10-08`

`200 OK`

```json
{
  "exercise_id": 87,
  "points": [
    {
      "started_at": "2026-10-01T18:00:00-03:00",
      "sets": [
        { "number": 1, "repetitions": 10, "load_kg": 37.5 },
        { "number": 2, "repetitions": 8, "load_kg": 40.0 }
      ]
    }
  ]
}
```

### Consultar relatório consolidado

`GET /api/v1/reports/summary?from=2026-10-01&to=2026-10-31`

`200 OK`

```json
{
  "period": {
    "from": "2026-10-01",
    "to": "2026-10-31"
  },
  "workout_sessions": 8,
  "sets_logged": 96,
  "exercises": [
    {
      "exercise_id": 87,
      "name": "Agachamento",
      "highest_load_kg": 80.0
    }
  ]
}
```

Os exemplos são ilustrativos. Os cálculos e indicadores definitivos do relatório serão confirmados com o grupo antes da implementação.

## Paginação, busca e erros

Resposta padrão sugerida para coleções:

```json
{
  "count": 35,
  "next": "/api/v1/workouts?limit=20&offset=20",
  "previous": null,
  "results": []
}
```

Formato padrão de erro:

```json
{
  "error": {
    "code": "validation_error",
    "message": "Confira os campos informados.",
    "details": {
      "load_kg": ["O valor deve ser maior ou igual a zero."]
    }
  }
}
```

Status gerais previstos: `200` consulta ou atualização; `201` criação; `204` exclusão ou encerramento sem corpo; `400` falha de validação; `401` autenticação ausente ou inválida; `403` CSRF inválido ou falta de permissão administrativa; `404` recurso inexistente ou recurso pessoal de outro usuário; `409` conflito de unicidade, dependências ou estado da sessão; `502`/`503` indisponibilidade de serviço externo sem cache disponível. Erros não devem ser retornados como se a operação tivesse sido concluída com sucesso.

## Relação com os demais documentos da Fase 1

- O Documento de Visão apresenta as funcionalidades F01 a F13 e os dois perfis de acesso.
- As especificações de casos de uso apresentam UC01 a UC15; a finalização da sessão (UC07) inclui obrigatoriamente a geração do resumo (UC08).
- O modelo de dados registra usuários com `perfil` e `ativo`, exercícios com `origem` e sessões com `iniciado_em`, `finalizado_em` e `status`.
- O plano da API externa detalha o uso do catálogo da wger, a paginação, o cache e o tratamento de indisponibilidade.
- Este contrato define apenas a interface planejada. A criação das rotas, do banco e das telas pertence à Fase 2.
