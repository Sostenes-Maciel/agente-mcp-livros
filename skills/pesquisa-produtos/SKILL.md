# Skill: Pesquisa de Produtos em Site de Vendas

## Objetivo
Pesquisar produtos em um site de vendas (e-commerce, catálogo, marketplace) e gerar um resumo estruturado em Markdown com os dados encontrados.

---

## Entradas

| Parâmetro | Tipo | Obrigatório | Descrição |
|-----------|------|-------------|-----------|
| `site` | string | Sim | URL base do site alvo (ex: `https://books.toscrape.com/`) |
| `termos_busca` | array[string] | Sim | Lista de termos/produtos a buscar (ex: `["Harry Potter", "The Chronicles of Narnia"]`) |
| `filtros` | object | Não | Filtros opcionais: `categoria_preferida`, `preco_max`, `disponibilidade`, `avaliacao_minima` |
| `quantidade_resultados` | integer | Não | Número máximo de resultados por termo (padrão: 1 — o primeiro de cada termo) |
| `limite_paginas` | integer | Não | Máximo de páginas a percorrer por categoria (padrão: 5) |

---

## Passo a Passo (Estratégia de Busca)

### 1. Verificar campo de busca nativo
- Acessar a página inicial do `site`
- Procurar por `<input type="search">`, `<input name="q">`, `placeholder` com "search", `id` com "search"
- Se existir: usar o campo de busca para cada termo em `termos_busca`
- Se **não** existir: ir para o passo 2

### 2. Navegar pela categoria mais provável
- Identificar menu de categorias (geralmente em `.side_categories`, `.nav`, `nav`, `.categories`)
- Mapear categorias relevantes baseadas nos termos (ex: "Fantasy", "Childrens", "Young Adult" para livros de fantasia/infantis)
- Acessar a primeira categoria mais provável

### 3. Percorrer produtos na categoria (com paginação limitada)
- Em cada página da categoria:
  - Extrair cards de produto (geralmente `.product_pod`, `.product-item`, `.item`)
  - Para cada card, coletar: **título**, **preço**, **disponibilidade**, **avaliação (estrelas)**, **categoria**, **link da página de detalhes**
  - Verificar se o título corresponde a algum termo de busca (busca parcial, case-insensitive)
  - Se encontrar: registrar resultado e, se for o primeiro daquele termo, acessar página de detalhes para pegar **descrição completa**
- Ir para próxima página (link `.next a`, `rel="next"`, ou paginação numerada)
- **Parar** quando:
  - Encontrou `quantidade_resultados` para todos os termos, OU
  - Atingiu `limite_paginas` páginas percorridas, OU
  - Não há mais páginas

### 4. Se não encontrou na categoria principal
- Tentar categorias alternativas relacionadas (máximo 3 categorias extras)
- Aplicar mesmo processo do passo 3

### 5. Gerar arquivo de saída Markdown
- Consolidar todos os achados
- Para termos não encontrados: registrar "não encontrado" + onde procurou
- Salvar no caminho definido na saída

---

## Regras Obrigatórias

1. **Não inventar resultados** — Se não encontrou, registrar explicitamente "não encontrado" e listar onde foi procurado (categorias, páginas, URLs)

2. **Não registrar dados pessoais de vendedores** — Coletar apenas: título do produto, preço, disponibilidade, avaliação, categoria, descrição pública. Ignorar: nome do vendedor, CNPJ, endereço, telefone, email, dados bancários, etc.

3. **Respeitar o limite de páginas** — Parar obrigatoriamente ao atingir `limite_paginas` (padrão 5) por categoria, mesmo sem encontrar resultados

4. **Não fazer scraping agressivo** — Aguardar carregamento da página, não fazer requisições paralelas excessivas, respeitar `robots.txt` se houver

5. **Tratar erros de navegação** — Se página retornar 404/500, registrar erro e continuar; se site bloquear, parar e reportar

6. **Dados em moeda/local original** — Não converter preços, manter formato original do site (ex: £, $, R$)

---

## Formato do Arquivo de Saída (Markdown)

```markdown
# Resultado da Busca - [Nome do Site]

**Data da busca:** [DD/MM/AAAA]  
**Site:** [URL base]  
**Estratégia:** [Resumo da estratégia usada: campo de busca / categorias + paginação]  
**Termos buscados:** [lista]  
**Limite de páginas:** [N]  
**Filtros aplicados:** [JSON ou "nenhum"]

---

## 1. [Termo de Busca 1]

### Livros/Produtos encontrados: [N] (categoria: [X], página: [Y])

| Título | Preço | Disponibilidade | Avaliação | Categoria |
|--------|-------|-----------------|-----------|-----------|
| [Título completo] | [Preço] | [Status] | [Estrelas] | [Categoria] |

### Resumo do primeiro resultado ([Título do primeiro])

> [Resumo de 2-3 linhas da descrição em português, traduzido se necessário]

---

## 2. [Termo de Busca 2]

### Resultado: **Não encontrado**

**Onde foi procurado:**
- Categoria **[Nome]** ([N] páginas: page-1, page-2, ...)
- Categoria **[Nome]** ([N] páginas: ...) — *busca adicional*

**Total de páginas percorridas:** [N]

[Nenhum produto com "[termo]" no título foi encontrado...]

---

## Observações

- [Observações relevantes: ausência de campo de busca, categorias usadas, limitações do catálogo, etc.]
```

---

## Exemplo de Uso (Referência)

```json
{
  "site": "https://books.toscrape.com/",
  "termos_busca": ["Harry Potter", "The Chronicles of Narnia"],
  "filtros": {
    "categoria_preferida": ["Fantasy", "Childrens", "Young Adult"]
  },
  "quantidade_resultados": 1,
  "limite_paginas": 5
}
```

**Resultado esperado:** Arquivo `resultado-busca.md` com a estrutura acima, contendo 3 livros de Harry Potter encontrados (Fantasy, página 2) e Narnia não encontrado após buscar em 5 categorias / 8 páginas.