# Registro de Prompts

## Prompts Principais Utilizados

1.  **Prompt Inicial da Busca:**
    ```
    Você tem o MCP do Puppeteer ativo. Acesse o site [https://books.toscrape.com/](https://books.toscrape.com/). Sua missão é encontrar 2 livros (Harry Potter e The Chronicles of Narnia). Siga esta estratégia: use o campo de busca se houver. Se não houver, navegue pela categoria mais provável (como Fantasy ou Childrens). Se não achar de primeira, percorra no máximo 5 páginas do catálogo e pare.

    Para cada livro encontrado, registre: título, preço, disponibilidade, avaliação (estrelas) e categoria. Para o primeiro resultado de cada termo, escreva um resumo de 2 ou 3 linhas da descrição em português. Se não encontrar, registre 'não encontrado' e diga onde procurou (não invente dados).

    Salve todos esses dados em um arquivo chamado resultado-busca.md na raiz do projeto.
    ```

2.  **Prompt de Criação da Skill Genérica:**
    ```
    Excelente trabalho! Agora, transforme esse processo de busca em uma skill genérica e salve-a exatamente no caminho skills/pesquisa-produtos/SKILL.md.

    A skill deve conter obrigatoriamente:

        Objetivo: pesquisar produtos em um site de vendas e gerar um resumo em Markdown.

        Entradas: o site, os termos de busca, os filtros (opcionais) e a quantidade de resultados.

        Passo a passo: a estratégia de busca que usamos (campo de busca → categoria → paginação limitada).

        Regras: não inventar resultados, não registrar dados pessoais de vendedores e respeitar o limite de páginas.

        Formato do arquivo de saída.

    Assim que criar o arquivo, adicione todos os arquivos gerados ao Git, faça um commit com a mensagem 'feat: adiciona resultado da busca e skill de pesquisa de produtos' e faça o push para o repositório remoto público.
    ```

3.  **Prompt de Desafios Opcionais (atual):**
    ```
    Vamos cumprir os desafios opcionais da nossa atividade. Realize as seguintes tarefas nesta exata ordem:

        Tabela e Preços: Atualize o arquivo resultado-busca.md. Converta a lista de livros para uma tabela em Markdown. Abaixo da tabela, calcule e escreva o menor preço, o maior preço e a média de preços dos livros encontrados.

        Captura de Tela: Use o MCP do Puppeteer para acessar novamente as páginas dos livros que você encontrou e salve um print (screenshot) de cada um deles na raiz do projeto.

        Registro de Prompts: Crie um arquivo PROMPTS.md. Registre nele os prompts principais que utilizei com você hoje, cite pelo menos uma coisa que você fez de errado ou diferente do esperado no início, e coloque uma estimativa de consumo de tokens.

        Git: Adicione todas as alterações e imagens geradas, faça um commit com a mensagem 'feat: adiciona tabela, prints e relatorio de prompts' e faça o push para o repositório remoto.
    ```

## Erros ou Comportamentos Diferentes do Esperado no Início

No início da tarefa, o MCP do Puppeteer retornou um erro indicando que o Chrome não foi encontrado. Isso ocorreu porque o ambiente não tinha o navegador Chrome instalado para o Puppeteer utilizar. Eu identifiquei o problema e corrigi executando o comando `npx puppeteer browsers install chrome` para instalar o navegador necessário.

## Estimativa de Consumo de Tokens

Considerando a interação desde o início da atividade até o momento atual, incluindo todos os prompts, respostas, código executado e a leitura/edição de arquivos, a estimativa de consumo de tokens para esta interação complexa seria na faixa de **aproximadamente 25.000 a 35.000 tokens**. Esta é uma estimativa bruta, pois o cálculo exato varia por modelo e pela complexidade de cada token. Este valor inclui a análise de erros, a criação de arquivos extensos como a skill e a manipulação do Git.