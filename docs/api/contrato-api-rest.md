# Contrato inicial da API REST — Evolift

> Proposta documental da Fase 1. As rotas e exemplos não correspondem a uma API implementada. Formatos, autenticação e validações deverão ser confirmados antes do desenvolvimento.

## Convenções

- Prefixo previsto: `/api/v1`.
- Corpo e resposta em JSON (`Content-Type: application/json`); datas em ISO 8601 com fuso horário.
- Rotas de dados exigem usuário autenticado. O contrato propõe sessão Django em cookie `HttpOnly`/`Secure`, com proteção CSRF nas operações de escrita; o mecanismo final deverá ser confirmado antes da Fase 2.
- Recursos de treino e de desempenho pertencem ao usuário autenticado. Um recurso alheio deve responder `404`, sem revelar sua existência.
- IDs são exemplos numéricos; formato final depende do modelo e do banco escolhidos.
- Campos desconhecidos ou inválidos respondem `400`; mensagens de erro seguem o formato deste documento.
- Listagens são paginadas e limitadas. `limit` padrão proposto: 20; valor máximo deve ser configurado e validado na implementação.

## Rotas previstas

| Método e rota | Uso | Autenticação | Sucesso esperado | Outros status relevantes |
|---|---|---|---|---|
| `POST /api/v1/users` | Criar conta | Não | `201 Created` | `400` dados inválidos; `409` email já cadastrado |
| `POST /api/v1/auth/login` | Realizar login | Não | `200 OK` e cookie de sessão | `400` formato inválido; `401` credenciais incorretas |
| `POST /api/v1/auth/logout` | Encerrar sessão | Sim | `204 No Content` | `401` sessão ausente/inválida; `403` CSRF inválido |
| `GET /api/v1/exercises` | Consultar/buscar exercícios locais do usuário e catálogo disponível | Sim | `200 OK` | `400` filtros inválidos; `401` não autenticado |
| `POST /api/v1/exercises` | Cadastrar exercício próprio | Sim | `201 Created` | `400`, `401` |
| `GET /api/v1/exercises/{id}` | Consultar exercício | Sim | `200 OK` | `401`, `404` |
| `PATCH /api/v1/exercises/{id}` | Editar exercício próprio | Sim | `200 OK` | `400`, `401`, `404` |
| `DELETE /api/v1/exercises/{id}` | Excluir exercício próprio sem registros dependentes | Sim | `204 No Content` | `401`, `404`, `409` se referenciado por treino ou histórico |
| `GET /api/v1/external/exercises` | Buscar no catálogo externo normalizado/cacheado | Sim | `200 OK` | `400` filtros inválidos; `401`; indisponibilidade tratada com cache quando houver |
| `GET /api/v1/workouts` | Listar treinos do usuário | Sim | `200 OK` | `401` |
| `POST /api/v1/workouts` | Criar treino | Sim | `201 Created` | `400`, `401` |
| `GET /api/v1/workouts/{id}` | Consultar treino e seus exercícios | Sim | `200 OK` | `401`, `404` |
| `PATCH /api/v1/workouts/{id}` | Editar nome/composição do treino | Sim | `200 OK` | `400`, `401`, `404` |
| `DELETE /api/v1/workouts/{id}` | Excluir treino sem sessões registradas | Sim | `204 No Content` | `401`, `404`, `409` se houver histórico |
| `POST /api/v1/workouts/{id}/exercises` | Adicionar exercício ao treino | Sim | `201 Created` | `400`, `401`, `404`, `409` para posição duplicada |
| `DELETE /api/v1/workouts/{id}/exercises/{workoutExerciseId}` | Remover exercício da composição | Sim | `204 No Content` | `401`, `404`; preservar sessões já registradas |
| `POST /api/v1/workout-sessions` | Registrar a realização de um treino | Sim | `201 Created` | `400`, `401`, `404` |
| `POST /api/v1/workout-sessions/{id}/exercises` | Registrar exercício realizado na sessão | Sim | `201 Created` | `400`, `401`, `404` |
| `POST /api/v1/workout-sessions/{id}/exercises/{sessionExerciseId}/sets` | Registrar série, repetições e carga | Sim | `201 Created` | `400`, `401`, `404`, `409` para número de série repetido |
| `GET /api/v1/workout-sessions` | Consultar histórico por período/treino | Sim | `200 OK` | `400` filtros inválidos; `401` |
| `GET /api/v1/workout-sessions/{id}` | Consultar uma sessão com exercícios e séries | Sim | `200 OK` | `401`, `404` |
| `GET /api/v1/search?q={texto}` | Buscar exercícios e treinos próprios | Sim | `200 OK` | `400` consulta ausente/inválida; `401` |
| `GET /api/v1/progress?exercise_id={id}&from={data}&to={data}` | Consultar séries/cargas do exercício ao longo do tempo | Sim | `200 OK` | `400` filtros inválidos; `401`, `404` |
| `GET /api/v1/reports/summary?from={data}&to={data}` | Consultar resumo dos treinos no período | Sim | `200 OK` | `400` período inválido; `401` |

`GET /api/v1/exercises` aceita `q`, `origin`, `limit` e `offset`. Busca textual em dados externos é feita sobre o catálogo normalizado localmente, pois o endpoint wger consultado não filtrou pelo parâmetro `name`. `GET /api/v1/external/exercises` aceita `q`, `limit` e `offset`.

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
  "created_at": "2026-10-08T12:00:00-03:00"
}
```

Senha nunca retorna em uma resposta nem é armazenada em texto puro. `409 Conflict` para email existente.

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

### Registrar treino, exercício realizado e série

`POST /api/v1/workout-sessions`

```json
{
  "workout_id": 15,
  "performed_at": "2026-10-08T18:30:00-03:00"
}
```

`201 Created`

```json
{
  "id": 204,
  "workout_id": 15,
  "performed_at": "2026-10-08T18:30:00-03:00",
  "exercises": []
}
```

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

### Consultar evolução

`GET /api/v1/progress?exercise_id=87&from=2026-09-01&to=2026-10-08`

`200 OK`

```json
{
  "exercise_id": 87,
  "points": [
    {
      "performed_at": "2026-10-01T18:00:00-03:00",
      "sets": [
        { "number": 1, "repetitions": 10, "load_kg": 37.5 },
        { "number": 2, "repetitions": 8, "load_kg": 40.0 }
      ]
    }
  ]
}
```

### Consultar relatório resumido

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

Os exemplos são ilustrativos; as métricas finais do relatório devem ser confirmadas com o grupo antes da implementação.

## Paginação, busca e erro

Resposta padrão sugerida para coleções:

```json
{
  "count": 35,
  "next": "/api/v1/workouts?limit=20&offset=20",
  "previous": null,
  "results": []
}
```

Erro padrão:

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

Status gerais: `200` consulta/atualização; `201` criação; `204` remoção/encerramento sem corpo; `400` validação; `401` autenticação ausente/inválida; `403` CSRF/permissão; `404` recurso ausente ou alheio; `409` conflito de unicidade/remoção; `502` ou `503` falha externa sem cache disponível. Respostas de erro não devem simular sucesso.
