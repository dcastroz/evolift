# Planejamento do Projeto — Evolift

## 1. Objetivo do planejamento

Este documento apresenta a divisão das atividades, responsabilidades, marcos, riscos e estratégia de desenvolvimento do Evolift até a conclusão da Fase 2.

O projeto será desenvolvido por uma equipe composta por três integrantes, utilizando o GitHub para controle de versão e acompanhamento das contribuições.

---

## 2. Divisão de responsabilidades

| Integrante | Principais responsabilidades |
|---|---|
| Davi Castro | README, Documento de Visão e Planejamento |
| Lucas Porcedda | Casos de Uso, especificações textuais, protótipos e identidade visual |
| Eduardo Moreira | Arquitetura, modelo de dados, contrato da API REST e plano de integração com API externa |
| Todos | Implementação, testes, revisão, segurança, publicação e apresentação |

> Os responsáveis indicados representam a responsabilidade principal de cada atividade. Todos os integrantes poderão colaborar na revisão e implementação das demais partes do projeto.

---

## 3. Backlog da Fase 1

| ID | Atividade | Responsável | Status |
|---|---|---|---|
| F1-01 | Criar e configurar o repositório GitHub | Davi Castro | Concluído |
| F1-02 | Criar README inicial | Davi Castro | Concluído |
| F1-03 | Elaborar Documento de Visão | Davi Castro | Concluído |
| F1-04 | Elaborar planejamento do projeto | Davi Castro | Em andamento |
| F1-05 | Elaborar diagrama de casos de uso | Lucas Porcedda | Pendente |
| F1-06 | Elaborar especificações textuais dos casos de uso | Lucas Porcedda | Pendente |
| F1-07 | Criar identidade visual do Evolift | Lucas Porcedda | Pendente |
| F1-08 | Criar protótipos das telas principais | Lucas Porcedda | Pendente |
| F1-09 | Elaborar arquitetura da aplicação | Eduardo Moreira | Pendente |
| F1-10 | Elaborar modelo de dados | Eduardo Moreira| Pendente |
| F1-11 | Definir contrato inicial da API REST | Eduardo Moreira | Pendente |
| F1-12 | Definir integração com API externa | Eduardo Moreira | Pendente |
| F1-13 | Revisar documentação e verificar consistência | Todos | Pendente |
| F1-14 | Finalizar README da Fase 1 | Todos | Pendente |

---

## 4. Backlog inicial da Fase 2

| ID | Atividade | Responsável |
|---|---|---|
| F2-01 | Configurar projeto Django | Todos |
| F2-02 | Implementar banco de dados e migrations | Todos |
| F2-03 | Implementar autenticação e controle de acesso | Todos |
| F2-04 | Implementar cadastro e gerenciamento de exercícios | Todos |
| F2-05 | Implementar criação e gerenciamento de treinos | Todos |
| F2-06 | Implementar registro de séries, repetições e cargas | Todos |
| F2-07 | Implementar histórico de treinos | Todos |
| F2-08 | Implementar busca | Todos |
| F2-09 | Implementar relatórios e indicadores | Todos |
| F2-10 | Implementar API REST própria | Todos |
| F2-11 | Implementar consumo da API externa | Todos |
| F2-12 | Realizar testes | Todos |
| F2-13 | Publicar aplicação | Todos |
| F2-14 | Executar análise SAST | Todos |
| F2-15 | Executar análise DAST | Todos |
| F2-16 | Corrigir problemas identificados | Todos |
| F2-17 | Preparar apresentação final | Todos |

---

## 5. Marcos do projeto

### Marco 1 — Estruturação
- Repositório configurado.
- Integrantes adicionados.
- README inicial criado.

### Marco 2 — Conclusão da documentação da Fase 1
- Documento de Visão concluído.
- Casos de uso definidos.
- Arquitetura definida.
- Modelo de dados elaborado.
- APIs planejadas.
- Protótipos e identidade visual definidos.
- Planejamento concluído.

### Marco 3 — Implementação principal
- Projeto Django configurado.
- Funcionalidades principais implementadas.
- Banco de dados integrado.

### Marco 4 — APIs e relatórios
- API REST própria funcionando.
- API externa integrada.
- Busca e relatórios funcionando.

### Marco 5 — Publicação e segurança
- Aplicação publicada.
- Testes executados.
- SAST e DAST realizados.
- Problemas prioritários corrigidos.

### Marco 6 — Entrega final
- Documentação atualizada.
- Aplicação disponível.
- Apresentação preparada.

---

## 6. Estratégia de desenvolvimento

O desenvolvimento será realizado de forma incremental. Inicialmente será concluída a documentação e modelagem da aplicação. Após a validação da Fase 1, a equipe iniciará a implementação das funcionalidades em Django.

O GitHub será utilizado para controle de versão e registro das contribuições. Cada integrante deverá realizar commits identificáveis referentes às atividades pelas quais for responsável.

Durante a implementação, as funcionalidades serão desenvolvidas e testadas gradualmente, evitando concentrar todo o desenvolvimento próximo à data de entrega.

Antes da entrega final, será realizada uma revisão entre documentação e implementação para verificar se os casos de uso, modelo de dados, arquitetura, APIs e funcionalidades permanecem coerentes.

---

## 7. Riscos do projeto

| Risco | Probabilidade | Impacto | Tratamento |
|---|---|---|---|
| Atraso nas atividades | Média | Alto | Dividir responsabilidades e acompanhar o andamento |
| Indisponibilidade da API externa | Média | Médio | Implementar tratamento de erro e indisponibilidade |
| Dificuldade na integração entre funcionalidades | Média | Alto | Desenvolver e testar as funcionalidades gradualmente |
| Problemas na hospedagem | Média | Alto | Realizar a publicação antes da data final |
| Alterações de escopo | Média | Médio | Registrar alterações e atualizar a documentação |
| Conflitos no GitHub | Baixa | Médio | Utilizar commits frequentes e sincronizar o repositório |
| Falhas encontradas nas análises de segurança | Média | Alto | Executar SAST e DAST com antecedência para permitir correções |

---

## 8. Acompanhamento

O andamento das tarefas poderá ser acompanhado pelo histórico de commits e pelas atividades registradas no GitHub.

Os documentos e funcionalidades deverão ser atualizados sempre que decisões relevantes forem modificadas durante o desenvolvimento.