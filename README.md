# Análise Setorial & Desempenho por Segmento

Relatório desenvolvido em **Power BI** para a análise setorial e o desempenho por categoria de
produtos de uma rede varejista. O painel compara a performance dos diferentes setores, identificando
crescimento, participação de vendas (share) e rentabilidade de cada segmento, para orientar decisões
de mix, compra e alocação de espaço.

---

## Visão Geral

A solução organiza a base de vendas por setor e categoria em um modelo dimensional e entrega uma
leitura comparativa entre segmentos: quanto cada setor fatura, qual sua margem, qual sua
participação no total e como evolui no tempo. É a base para decidir onde investir mix e espaço e o
que revisar.

> **Nota de confidencialidade:** todos os dados exibidos no arquivo `.pbix` são fictícios e
> anonimizados, criados exclusivamente para demonstração de portfólio.

---

## Contexto de Negócio

Nem todo setor que vende muito é o que dá mais retorno. Volume alto com margem baixa pode render
menos que um setor menor e mais rentável, e categorias de baixo giro ocupam espaço e capital sem
retorno proporcional. Sem uma comparação padronizada de margem, share e giro, o mix é decidido por
inércia. Este painel dá o denominador comum para priorizar as categorias certas.

## O Problema que Resolve

- **Comparação entre setores por volume apenas**, ignorando margem e giro.
- **Mix decidido por inércia**, sem base de rentabilidade por segmento.
- **Sazonalidade não mapeada** por categoria.
- **Espaço e capital alocados** sem leitura de retorno por setor.

## Público e Decisões Apoiadas

- **Compras / Comercial:** prioriza categorias mais rentáveis e revisa o mix.
- **Diretoria:** acompanha a participação e a evolução de cada setor.
- **Gestão de categoria:** identifica sazonalidade e oportunidades por segmento.

## Impacto e Valor Gerado

Comparou margem, share e giro por setor, permitindo priorizar as categorias mais rentáveis e revisar
o mix das que apenas ocupam espaço sem retorno — melhorando a rentabilidade do portfólio de produtos.

---

## Arquitetura de Dados e Abordagem Técnica

### Fontes de dados
- Base de **vendas** por produto e período.
- Cadastros de **produtos** e **setores / categorias**.

### Transformação (Power Query / M)
ETL e higienização das tabelas de produtos, vendas e categorias; padronização da hierarquia de
setor / categoria; e cálculo das bases de margem e giro. O tratamento é centralizado no Power Query.

### Modelagem dimensional (Star Schema)
- **Fato Vendas** — grão de venda por produto / data.
- **Dimensão Setor / Categoria** — hierarquia de segmento.
- **Dimensão Produto** — atributos do item.
- **Dimensão Calendário** — base das análises temporais (YoY / MoM).

### Principais medidas (DAX) — lógica implementada

```DAX
Faturamento Setor = SUM( FatoVendas[Valor] )
Margem Setor %    = DIVIDE( [Lucro Bruto Setor], [Faturamento Setor] )
Share %           = DIVIDE( [Faturamento Setor], CALCULATE( [Faturamento Setor], ALL( DimSetor ) ) )
Giro Medio        = DIVIDE( [Unidades Vendidas], [Estoque Medio] )
Variacao YoY %    = DIVIDE( [Faturamento Setor] - [Faturamento Ano Anterior], [Faturamento Ano Anterior] )
```

> As fórmulas descrevem a lógica de cálculo do relatório e servem como referência de leitura do modelo.

---

## Indicadores e Análises (KPIs)

- **Faturamento & Margem por Setor:** receita bruta e margem gerada por cada categoria.
- **Participação de Mercado (Share %):** peso de cada setor no faturamento global.
- **Evolução Temporal & Sazonalidade:** volume por setor ao longo de meses e trimestres.
- **Ranking de Categorias:** setores com maior giro de estoque e maior rentabilidade.

## Estrutura de Páginas do Relatório

1. **Visão Setorial** — faturamento, margem e share por setor.
2. **Ranking de Categorias** — rentabilidade e giro comparados.
3. **Evolução & Sazonalidade** — séries temporais por segmento.

## Como Visualizar

O relatório é apresentado pelas imagens da seção **Prints** abaixo, que reproduzem as páginas
principais do painel. O arquivo-fonte (`.pbix`) não é versionado neste repositório por conter o
modelo de dados; pode ser disponibilizado sob solicitação para avaliação técnica.

## Estrutura de Arquivos

```text
README.md    -> Documentacao do projeto (contexto de negocio + arquitetura tecnica)
docs/        -> Prints das paginas do relatorio
.gitignore   -> Exclui binarios (.pbix) e planilhas de trabalho do versionamento
```

## Prints

As imagens abaixo apresentam as páginas principais do relatório — a imagem é o que comunica o
projeto de forma imediata. Os prints serão adicionados na pasta `docs/`.

<!-- Descomente ao adicionar a imagem:
![Visao setorial e desempenho por segmento](docs/print-1.png)
-->

## Autor

Vitor Santos — Análise de Dados e Inteligência Comercial (Varejo).
