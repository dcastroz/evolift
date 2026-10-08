# EvoLift

**Instituição:** CEUB  
**Curso:** Ciência da Computação  
**Disciplina:** Desenvolvimento Web  
**Turma / Semestre:** 2026.2  
**Professor:** Felippe Pires Ferreira  
**Status do projeto:** Fase 1 — Documentação e arquitetura

---

## 1. Sobre o projeto

O Evolift é um sistema web pensado para ajudar pessoas que praticam musculação a organizar seus treinos e acompanhar sua evolução.

A ideia surgiu porque muitas pessoas registram séries, repetições e cargas no bloco de notas do celular, em planilhas ou simplesmente não fazem esse acompanhamento. Com o tempo, isso pode dificultar a consulta de treinos antigos e a comparação do desempenho.

O Evolift pretende reunir essas informações em um único lugar, permitindo que o usuário organize seus treinos, registre o que foi realizado e consulte seu histórico.

### Objetivo geral

Criar uma aplicação web para organizar treinos de musculação e acompanhar a evolução do usuário.

### Objetivos específicos

- cadastrar e organizar exercícios;
- criar treinos;
- registrar séries, repetições e cargas;
- guardar o histórico dos treinos;
- permitir buscas;
- acompanhar a evolução do usuário;
- gerar relatórios relacionados aos treinos.

### Público-alvo

Pessoas que praticam musculação e querem manter seus treinos e resultados organizados.

---

## 2. Funcionalidades previstas

| Funcionalidade | Descrição |
|---|---|
| Exercícios | Cadastro e gerenciamento dos exercícios |
| Treinos | Criação e organização dos treinos do usuário |
| Registro do treino | Registro de séries, repetições e cargas |
| Histórico | Consulta dos treinos realizados anteriormente |
| Busca | Pesquisa de exercícios e treinos |
| Evolução | Acompanhamento do progresso ao longo do tempo |
| Relatórios | Apresentação de informações resumidas dos treinos |
| API REST | Disponibilização de alguns dados do sistema |
| API externa | Uso de dados externos em uma funcionalidade do Evolift |

---

## 3. Requisitos não funcionais

- a interface deverá funcionar bem em computador e celular;
- as informações deverão ser apresentadas de forma simples e organizada;
- informações sensíveis não deverão ser armazenadas diretamente no repositório;
- o projeto deverá manter uma organização compatível com as práticas utilizadas no Django;
- a documentação e os diagramas deverão permanecer coerentes entre si.

---

## 4. Tecnologias definidas até o momento

| Área | Tecnologia |
|---|---|
| Linguagem | Python |
| Backend | Django |
| Frontend | HTML, CSS e JavaScript |
| Controle de versão | Git e GitHub |

O banco de dados, a API externa e outras decisões técnicas serão registradas quando forem definidas pelo grupo durante a Fase 1.

---

## 5. Identidade visual

A identidade visual escolhida para o Evolift segue uma linha escura relacionada ao ambiente de academia.

As cores principais definidas até o momento são:

| Uso | Cor |
|---|---|
| Fundo principal | `#121212` |
| Cards e áreas secundárias | `#2A2A2A` |
| Destaques | `#B7FF00` |
| Texto principal | `#FFFFFF` |
| Texto secundário | `#BDBDBD` |

A fonte escolhida inicialmente é a **Poppins**.

Mais informações estão disponíveis no documento de identidade visual.

---

## 6. Documentação da Fase 1

A documentação do projeto está organizada dentro da pasta `docs/`.

### Documento de Visão

`docs/visao/documento-visao.md`

Apresenta o problema, justificativa, objetivos, público-alvo, escopo, funcionalidades, riscos e critérios de sucesso do Evolift.

### Casos de Uso

`docs/casos-de-uso/`

Contém o diagrama UML de casos de uso e suas especificações textuais.

### Arquitetura

`docs/arquitetura/`

Contém o diagrama e a descrição da arquitetura planejada para o sistema.

### Modelo de Dados

`docs/banco-de-dados/`

Contém o modelo de dados planejado para o Evolift.

### APIs

`docs/api/`

Contém o contrato inicial da API REST e o plano de integração com a API externa.

### Protótipos e Identidade Visual

`docs/prototipos/`

Contém a identidade visual e os protótipos das telas principais.

### Planejamento

`docs/planejamento/planejamento.md`

Contém a divisão das tarefas, backlog, marcos e riscos da Fase 1.

---

## 7. Participantes

| Integrante | Responsabilidade principal na Fase 1 |
|---|---|
| Davi Castro | README, Documento de Visão e Planejamento |
| Lucas Porcedda | Casos de Uso, especificações textuais, protótipos e identidade visual |
| Eduardo Moreira | Arquitetura, modelo de dados, contrato inicial da API REST e plano da API externa |

**Professor:** Felippe Pires Ferreira

---

## 8. Organização do repositório

```text
evolift/
├── README.md
├── docs/
│   ├── visao/
│   ├── casos-de-uso/
│   ├── arquitetura/
│   ├── banco-de-dados/
│   ├── api/
│   ├── prototipos/
│   └── planejamento/
└── images/