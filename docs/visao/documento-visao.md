# Documento de Visão — Evolift

## 1. Contexto e problema

Durante um treino de musculação, é comum precisar lembrar quais exercícios foram feitos, quantas séries e repetições foram realizadas e qual carga foi usada.

Muitas pessoas acabam anotando essas informações no bloco de notas do celular, em planilhas ou até deixando de registrar. Depois de um tempo, pode ficar difícil consultar treinos antigos ou lembrar quanto estava sendo usado em determinado exercício.

O Evolift foi pensado para reunir essas informações em um só lugar e facilitar a organização dos treinos.

---

## 2. Justificativa

A ideia do Evolift surgiu da necessidade de ter uma forma mais organizada de registrar os treinos.

Em vez de manter informações espalhadas em diferentes lugares, o usuário poderá registrar os exercícios realizados e consultar os dados depois. Isso também facilita a comparação entre treinos e o acompanhamento da evolução ao longo do tempo.

---

## 3. Objetivos

### 3.1 Objetivo geral

Desenvolver uma aplicação web para organizar treinos de musculação e acompanhar a evolução do usuário.

### 3.2 Objetivos específicos

- cadastrar e organizar exercícios;
- criar e organizar treinos;
- registrar séries, repetições e cargas;
- manter um histórico dos treinos realizados;
- permitir a busca de exercícios e treinos;
- mostrar informações sobre a evolução do usuário;
- gerar relatórios relacionados aos treinos;
- disponibilizar parte das informações por meio de uma API REST;
- utilizar uma API externa em uma funcionalidade do sistema.

---

## 4. Público-alvo

O Evolift é voltado para pessoas que praticam musculação e querem manter seus treinos organizados.

A aplicação poderá ser utilizada tanto por quem começou a treinar recentemente quanto por pessoas que já treinam há mais tempo e querem acompanhar melhor seu histórico.

---

## 5. Stakeholders

### Administrador

Responsável por gerenciar o catálogo geral de exercícios e as contas cadastradas no Evolift. O administrador terá permissões diferentes das de um usuário comum e não terá acesso automático ao histórico pessoal de treinos.

### Usuário

Pessoa que utilizará o Evolift para criar seus treinos, registrar seus resultados e consultar o histórico.

Seus principais interesses são conseguir registrar as informações de forma simples e acompanhar sua evolução.

### Equipe do projeto

Os três integrantes responsáveis pela documentação, modelagem e demais atividades do projeto.

### Professor da disciplina

Responsável por acompanhar e avaliar o trabalho desenvolvido pelo grupo.

---

## 6. Escopo do sistema

Dentro do escopo inicial do Evolift estão:

- Cadastro e gerenciamento de exercícios;
- Criação de treinos;
- Inclusão de exercícios nos treinos;
- Registro de séries, repetições e cargas;
- Registro dos treinos realizados;
- Consulta ao histórico;
- Busca de exercícios e treinos;
- Acompanhamento da evolução;
- Geração de relatórios;
- Disponibilização de alguns dados através de uma API REST;
- Utilização de uma API externa relacionada ao contexto do sistema.
- Cadastro de exercícios personalizados pelos usuários;
- Consulta ao catálogo externo de exercícios da wger;
- Gerenciamento do catálogo geral de exercícios pelo administrador;
- Gerenciamento das contas cadastradas pelo administrador.

---

## 7. Itens fora do escopo

Neste projeto, não pretendemos incluir:

- prescrição automática de treinos;
- diagnósticos relacionados à saúde;
- acompanhamento médico;
- sistema de pagamentos ou assinaturas;
- funcionamento como rede social;
- chat entre usuários;
- acompanhamento direto entre aluno e personal trainer.

Essas funcionalidades não fazem parte da proposta inicial do Evolift e aumentariam bastante o tamanho do projeto.

---

## 8. Funcionalidades previstas

### F01 — Gerenciar exercícios

Permitir que o usuário cadastre, consulte, edite e exclua seus próprios exercícios personalizados.

### F02 — Gerenciar treinos

Permitir criar, consultar, editar e excluir treinos.

### F03 — Adicionar exercícios ao treino

Permitir escolher quais exercícios fazem parte de cada treino.

### F04 — Registrar treino realizado

Permitir que o usuário inicie e conclua uma sessão de treino, registrando os exercícios realizados e os horários de início e término.

### F05 — Registrar desempenho

Permitir registrar séries, repetições e cargas utilizadas nos exercícios.

### F06 — Consultar histórico

Permitir consultar os treinos realizados anteriormente.

### F07 — Buscar informações

Permitir pesquisar exercícios e treinos cadastrados.

### F08 — Acompanhar evolução

Permitir comparar informações dos treinos ao longo do tempo.

### F09 — Gerar relatório

Apresentar informações resumidas sobre os treinos realizados. Ao concluir uma sessão, o sistema também deverá gerar um resumo com duração, exercícios e séries registrados.

### F10 — API REST

Disponibilizar alguns dados do Evolift através de uma API REST.

### F11 — Consultar catálogo externo

Utilizar a API wger para consultar exercícios disponíveis, apresentando informações como nome, grupo muscular, equipamento e descrição, quando disponíveis.

### F12 — Gerenciar catálogo geral

Permitir que o administrador cadastre, consulte, edite e gerencie os exercícios do catálogo geral do Evolift.

### F13 — Gerenciar usuários

Permitir que o administrador consulte as contas cadastradas e controle sua situação de acesso.

---

## 9. Restrições

- o backend deverá ser desenvolvido em Python e Django;
- deverá ser utilizado um banco de dados relacional;
- a aplicação deverá possuir uma interface web responsiva;
- deverá existir uma API REST própria;
- o sistema deverá utilizar uma API externa disponível na Internet;
- senhas, tokens, chaves e outras informações sensíveis não deverão ser colocados no repositório;
- as decisões do projeto deverão seguir os requisitos definidos para o trabalho.

---

## 10. Premissas

- os integrantes terão acesso ao GitHub para trabalhar no projeto;
- os documentos serão atualizados caso alguma decisão importante do projeto seja alterada;
- a API externa escolhida deverá atender às necessidades definidas pelo grupo;
- os dados utilizados para demonstração deverão ser adequados ao contexto acadêmico;
- os integrantes deverão manter suas contribuições identificáveis no repositório.

---

## 11. Riscos iniciais

| Risco | Impacto | Como pretendemos lidar |
|---|---|---|
| Atraso em alguma atividade | Alto | Dividir as tarefas entre os integrantes e acompanhar o andamento |
| API externa escolhida não atender ao projeto | Médio | Avaliar as opções antes de fechar a escolha |
| API externa ficar indisponível | Médio | Considerar esse risco durante o planejamento da integração |
| Mudanças no escopo | Médio | Atualizar os documentos que forem afetados |
| Documentos com informações diferentes | Alto | Fazer uma revisão geral antes da entrega |
| Problemas com arquivos ou commits no GitHub | Médio | Fazer commits frequentes e manter o repositório atualizado |

---

## 12. Critérios de sucesso

Para a proposta definida na Fase 1, esperamos que o Evolift tenha um planejamento capaz de permitir:

- Organização dos treinos do usuário;
- Cadastro e consulta de exercícios;
- Registro de séries, repetições e cargas;
- Consulta ao histórico de treinos;
- Acompanhamento da evolução;
- Geração de pelo menos um relatório;
- Definição de uma API REST própria;
- Definição de uma integração útil com uma API externa;
- Coerência entre os casos de uso, arquitetura, modelo de dados, APIs e demais documentos da Fase 1.
- O usuário poderá cadastrar exercícios personalizados e consultar exercícios do catálogo externo;
- As sessões de treino terão registros de início, conclusão, séries, repetições e cargas;
- O planejamento deverá permitir a geração de um resumo ao concluir cada sessão;
- As funcionalidades administrativas terão permissões próprias, separadas das funções do usuário comum.