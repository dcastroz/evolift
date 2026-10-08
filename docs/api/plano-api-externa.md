# Plano de integração com API externa — wger

> Pesquisa e proposta da Fase 1. A integração ainda não foi implementada e deve ser revalidada antes da Fase 2.

## API selecionada

**wger REST API**, serviço open-source de gestão de treinos com catálogo público de exercícios.

- Base consultada: `https://wger.de/api/v2/`
- Documentação oficial: [Using the API](https://wger.readthedocs.io/en/latest/api/api.html)
- Especificação navegável/OpenAPI: [`https://wger.de/api/v2/schema`](https://wger.de/api/v2/schema)
- Uso no Evolift: complementar a busca de exercícios com nomes, descrições, grupos musculares/categorias e equipamentos. A API externa não recebe dados de conta, treino ou desempenho.

## Endpoints previstos

| Método e endpoint wger | Uso previsto | Dados relevantes |
|---|---|---|
| `GET /api/v2/exerciseinfo/` | Buscar páginas de exercícios com suas traduções | `translations` (nome/descrição), `category`, `muscles`, `muscles_secondary`, `equipment`, `license`, `license_author`, `images` |
| `GET /api/v2/language/` | Descobrir o identificador da língua disponível para solicitar traduções | ID e nome do idioma; não presumir que o ID de um idioma seja estável sem consultar o endpoint |
| `GET /api/v2/exercisecategory/` | Consultar categorias se necessário para normalizar os filtros | ID e nome de categoria |
| `GET /api/v2/equipment/` | Consultar equipamentos se necessário para normalizar os filtros | ID e nome do equipamento |

Os endpoints de categorias e equipamentos são auxiliares; a consulta principal é `exerciseinfo`. Na chamada verificada durante a pesquisa, `GET https://wger.de/api/v2/exerciseinfo/?language=2&limit=1` respondeu `200` com JSON paginado e campos de tradução, categoria, músculos, equipamentos, licença e autor. O valor `2` foi observado como um exemplo de idioma naquela resposta e não deve ser tratado como ID garantido de português.

## Paginação e pesquisa textual

O endpoint fornece `count`, `next`, `previous` e `results`, com 20 itens por página por padrão; `limit` pode ajustar o tamanho. O aplicativo deve seguir a URL `next` enquanto houver páginas, respeitando timeout e limites operacionais.

Durante o teste, adicionar `name=push` não reduziu o total nem filtrou os resultados. Portanto, o plano **não depende** de busca textual upstream: o adaptador obtém as páginas do idioma selecionado, guarda uma cópia local normalizada e executa a busca por nome no catálogo disponível localmente. O cache reduz chamadas repetidas e permite continuar a busca em indisponibilidades.

## Autenticação, chave e limites

- Os endpoints públicos de catálogo são somente leitura e a documentação informa que não exigem autenticação. Não foi necessária chave de API no teste.
- A documentação consultada informa paginação padrão de 20 itens e que os endpoints não listados entre os limites específicos são sem rate limit publicado; limites operacionais do serviço podem mudar. Um `429` pode exigir espera pelo cabeçalho `Retry-After`.
- O cliente deve usar paginação, tamanho moderado, timeout, cache e tentativas limitadas com espera progressiva; não fazer chamadas contínuas nem solicitar todas as páginas a cada busca.
- Revisar a documentação e as respostas de produção novamente antes da implementação, pois limites e campos podem mudar.

## Dados, licença e atribuição

O wger retorna licença e autor nos dados de cada exercício. A resposta observada incluiu um exercício sob CC BY-SA 4.0, mas a licença pode variar por item; não presumir uma licença única para todo o catálogo.

O Evolift deve conservar e exibir a atribuição, URL e licença do item quando disponibilizar conteúdo externo, conforme os metadados recebidos e os termos aplicáveis. Este plano prevê uso de dados textuais necessários à busca e não prevê copiar imagens. Se uma licença/metadado obrigatório estiver ausente ou se os termos não permitirem o uso pretendido, não redistribuir esse conteúdo e manter disponíveis apenas os exercícios próprios. A licença AGPL do software wger não deve ser confundida com a licença individual dos dados do exercício.

## Indisponibilidade e falhas

1. Usar timeout finito e validar status, JSON e campos antes de atualizar o catálogo local.
2. Não descartar a última cópia válida se uma atualização falhar.
3. Em `429`, respeitar `Retry-After`; em `5xx` ou timeout, fazer no máximo tentativas limitadas e não manter uma cadeia de retries.
4. Com cache disponível, responder a busca usando o último catálogo armazenado e sinalizar que os dados podem estar desatualizados.
5. Sem cache, retornar erro explícito de indisponibilidade (`503` na API Evolift) e continuar permitindo consulta aos exercícios locais. Não retornar uma lista vazia como se a pesquisa tivesse sido bem-sucedida.
6. Armazenar somente os metadados necessários e os campos de proveniência/licença; não enviar ao wger dados pessoais, credenciais, treinos ou cargas dos usuários.

## Fontes consultadas

- Documentação de uso, autenticação, paginação e rate limiting: [wger — Using the API](https://wger.readthedocs.io/en/latest/api/api.html).
- Endpoints e esquema publicado: [wger — API schema](https://wger.de/api/v2/schema).
- Teste do catálogo público: [`https://wger.de/api/v2/exerciseinfo/?language=2&limit=1`](https://wger.de/api/v2/exerciseinfo/?language=2&limit=1), consultado em 8 de outubro de 2026.
- Licença e autoria por exercício: campos `license` e `license_author` da resposta `exerciseinfo`; consultar a licença de cada item no próprio resultado.
