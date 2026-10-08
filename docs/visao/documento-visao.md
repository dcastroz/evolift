# Documento de Visão — Evolift

## 1. Contexto e problema

A prática de musculação envolve o acompanhamento constante de exercícios, séries, repetições e cargas utilizadas. Muitas pessoas realizam esse controle utilizando anotações em papel, aplicativos de notas ou planilhas, o que pode dificultar a organização e a consulta do histórico de treinos.

Além disso, quando as informações ficam dispersas, torna-se mais difícil identificar a evolução de carga, consultar treinos anteriores e acompanhar o desempenho ao longo do tempo.

O Evolift surge como uma aplicação web voltada à centralização e organização dessas informações.

---

## 2. Justificativa

O acompanhamento da evolução é uma parte importante da organização do treinamento de musculação. Entretanto, registrar manualmente as informações de cada treino pode se tornar pouco prático e dificultar consultas futuras.

O Evolift pretende oferecer uma solução simples e centralizada para registrar treinos e acompanhar o histórico de desempenho, permitindo que o usuário visualize suas informações de maneira organizada.

---

## 3. Objetivos

### 3.1 Objetivo geral

Desenvolver uma aplicação web para gerenciamento e acompanhamento de treinos de musculação.

### 3.2 Objetivos específicos

- Permitir o cadastro e gerenciamento de exercícios.
- Permitir a criação e organização de treinos.
- Registrar séries, repetições e cargas utilizadas.
- Armazenar o histórico dos treinos realizados.
- Permitir a consulta e busca de informações cadastradas.
- Apresentar indicadores relacionados à evolução do usuário.
- Gerar relatórios sobre os treinos realizados.
- Disponibilizar parte das informações por meio de uma API REST.
- Utilizar dados provenientes de uma API externa em uma funcionalidade do sistema.

---

## 4. Público-alvo

O Evolift é destinado principalmente a pessoas que praticam musculação e desejam organizar seus treinos e acompanhar sua evolução.

O sistema poderá ser utilizado tanto por praticantes iniciantes quanto por usuários que já treinam há mais tempo e desejam manter um histórico de suas atividades.

---

## 5. Stakeholders

### Usuário

Pessoa que utiliza o Evolift para organizar e registrar seus próprios treinos.

Principais interesses:

- facilidade no registro das informações;
- acesso ao histórico de treino;
- acompanhamento da evolução;
- organização dos exercícios e treinos.

### Equipe de desenvolvimento

Responsável pelo planejamento, desenvolvimento, manutenção e evolução da aplicação.

### Professor da disciplina

Responsável pela avaliação do projeto e verificação dos requisitos definidos para o trabalho.

---

## 6. Escopo do sistema

O Evolift deverá permitir que o usuário organize seus treinos de musculação e registre informações referentes às sessões realizadas.

O sistema deverá contemplar:

- gerenciamento de exercícios;
- criação de treinos;
- associação de exercícios aos treinos;
- registro de séries, repetições e cargas;
- registro de sessões de treino realizadas;
- consulta ao histórico;
- pesquisa de exercícios e treinos;
- acompanhamento da evolução;
- geração de relatórios;
- disponibilização de dados por API REST;
- integração com uma API externa.

---

## 7. Itens fora do escopo

Inicialmente, o Evolift não terá como objetivo:

- substituir a orientação de profissionais de Educação Física;
- prescrever automaticamente treinos;
- realizar diagnósticos relacionados à saúde;
- oferecer acompanhamento médico;
- funcionar como rede social;
- possuir sistema de pagamentos ou assinaturas;
- oferecer comunicação entre aluno e personal trainer.

Essas funcionalidades poderão ser consideradas futuramente, mas não fazem parte da versão inicial do projeto.

---

## 8. Funcionalidades previstas

### F01 — Gerenciar exercícios
Permitir cadastrar, consultar, editar e excluir exercícios.

### F02 — Gerenciar treinos
Permitir criar, consultar, editar e excluir treinos.

### F03 — Associar exercícios a um treino
Permitir selecionar os exercícios que farão parte de determinado treino.

### F04 — Registrar execução do treino
Permitir registrar que determinado treino foi realizado.

### F05 — Registrar desempenho
Permitir registrar séries, repetições e cargas utilizadas nos exercícios.

### F06 — Consultar histórico
Permitir visualizar os treinos realizados anteriormente.

### F07 — Pesquisar informações
Permitir pesquisar exercícios e treinos utilizando critérios definidos pelo sistema.

### F08 — Acompanhar evolução
Permitir consultar a evolução das cargas e do desempenho ao longo do tempo.

### F09 — Gerar relatório
Disponibilizar relatório com dados consolidados dos treinos realizados.

### F10 — Disponibilizar API REST
Disponibilizar informações selecionadas da aplicação através de endpoints REST.

### F11 — Consumir API externa
Utilizar informações provenientes de uma API externa em uma funcionalidade real da aplicação.

---

## 9. Restrições

- O backend deverá ser desenvolvido utilizando Python e Django.
- O sistema deverá utilizar um banco de dados relacional.
- A aplicação deverá possuir interface web responsiva.
- A aplicação deverá possuir uma API REST própria.
- O sistema deverá consumir uma API externa disponível na Internet.
- Informações sensíveis não poderão ser armazenadas diretamente no repositório.
- A aplicação deverá ser publicada em ambiente acessível pela Internet durante o período de avaliação.

---

## 10. Premissas

- O usuário possuirá acesso à Internet para utilizar a aplicação publicada.
- Os dados utilizados durante o desenvolvimento e demonstração serão fictícios ou adequados ao contexto acadêmico.
- A API externa escolhida deverá permanecer disponível durante o desenvolvimento.
- Os integrantes da equipe participarão do desenvolvimento por meio do repositório GitHub.
- As funcionalidades descritas neste documento poderão sofrer ajustes durante o desenvolvimento, desde que as alterações sejam documentadas.

---

## 11. Riscos iniciais

| Risco | Impacto | Estratégia |
|---|---|---|
| Indisponibilidade da API externa | Médio | Tratar falhas e impedir que a indisponibilidade comprometa as demais funções |
| Atrasos no desenvolvimento | Alto | Dividir tarefas entre os integrantes e acompanhar o andamento |
| Alterações no escopo | Médio | Registrar e revisar mudanças antes da implementação |
| Problemas na hospedagem | Alto | Realizar testes de publicação antes da entrega final |
| Falta de integração entre partes do sistema | Alto | Manter documentação, banco de dados, casos de uso e código alinhados |

---

## 12. Critérios de sucesso

O projeto será considerado bem-sucedido quando:

- o usuário conseguir criar e organizar seus treinos;
- exercícios puderem ser cadastrados e consultados;
- séries, repetições e cargas puderem ser registradas;
- o histórico de treinos puder ser consultado;
- o sistema apresentar informações de evolução do usuário;
- pelo menos um relatório puder ser visualizado;
- a API REST própria estiver disponível e documentada;
- a integração com a API externa estiver funcionando;
- a aplicação estiver publicada e acessível pela Internet;
- as funcionalidades planejadas estiverem coerentes com a documentação do projeto.
