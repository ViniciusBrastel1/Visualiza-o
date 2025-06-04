# Documentação do DashBoard Clara

Este documento apresenta a documentação técnica e análise do relatório Power BI denominado 'Relatório Clara.pbix', gerado com suporte da ferramenta Power BI Query.


## Informações Gerais:

- Data de Geração da Documentação: **03/06/2025**
- Nome  do DashBoard: Relatório Clara
- Responsavel atual: Vinicius Oliveira
- Data da ultima atualização do BI: **12/11/2024**
- local de Armazenamento: SharePoint e Power BI Service.

## Qual objetivo do DashBoard?

Este dashboard está sendo desenvolvido com o objetivo de fornecer uma visão abrangente, clara e atualizada sobre o uso do cartão corporativo, permitindo o monitoramento eficiente das despesas, identificação de pendências e possíveis usos indevidos, além de apoiar a tomada de decisões estratégicas. Ele contribui diretamente para a validação da alocação de custos, controle orçamentário, redução de despesas e aumento da eficiência na aplicação de recursos, oferecendo suporte analítico aos gestores de contrato e à Diretoria Operacional.

> [!TIP]
> **Objetivo: Análise e Monitoramento do Uso do Cartão Corporativo**

### Distribuição das Despesas:

Apresentar de forma clara e visual como os gastos realizados com o cartão corporativo estão distribuídos:

- Por categoria de compra (ex: alimentação, transporte, hospedagem, etc.);
- Por grupo ou área da empresa;
- Por colaborador, destacando os que mais utilizam o cartão.
  
### Pendências:

Exibir o quantitativo de pendências relacionadas ao uso do cartão, segmentadas por:

- Tipo de pendência: ausência de recibo, falta de etiquetas ou de aprovação;
- Grupo ou área;
- Colaborador;
- Período (mensal, trimestral, etc.);

### Análise de Uso Indevido:

Identificar possíveis usos indevidos do cartão corporativo, com análise por:

- Colaborador;
- Grupo ou área;
- Categoria de despesa.
- Descontos a Serem Efetuados;

### Apontar os valores a serem descontados dos colaboradores, quando aplicável, com base em:

- Uso indevido;
- Pendências não regularizadas.

## Quais decisões de negócio ele apoia?

Validação da alocação de custos e despesas; identificação de áreas que demandam maior atenção devido a custos elevados; suporte em projetos de aumento de faturamento e redução de custos; validação do orçamento previsto versus realizado, tanto mensal quanto anualmente; análise da aplicação de recursos e atuação junto aos colaboradores que utilizam a ferramenta de forma inadequada.

## Qual é o problema ou necessidade que ele resolve?

Disponibilização de relatórios gerenciais que serão utilizados nas tomadas de decisões de cada gestor de contrato e da Diretoria Operacional. Análises financeiras e de aplicação de recursos. 

## Fontes de Dados:

### Lista de todas as fontes: (Excel, SQL, APIs, etc.)

  O Dash é alimentado por uma unica base; 'BasClara"

### Caminhos de acesso (se possível):

https://brastelservicos.sharepoint.com/sites/Planejamento966/Documentos%20Compartilhados/Forms/AllItems.aspx?id=%2Fsites%2FPlanejamento966%2FDocumentos%20Compartilhados%2F01%2E%20Controladoria%2FControles%2FCART%C3%83O%20CLARA&viewid=1bb9be98%2D08cb%2D4928%2D9605%2Deaa314ba3402&CT=1748882753376&OR=OWA%2DNT%2DMail&CID=25ef5b0e%2D9fce%2Dd839%2D1455%2Dc8f527563ac8&e=5%3A14922c49eb7648b5b488d38422745489&sharingv2=true&fromShare=true&at=9&FolderCTID=0x012000CE44E4CA23DD574680AB90C17ED3BE11

### Frequência de atualização:

 **Dash não publicado e finalizado**

### Gateway necessário?

O relatório consegue acessar os arquivos no SharePoint Online diretamente, pois ambos estão na nuvem (cloud). Isso dispensa o uso de um gateway, que só é necessário quando os dados estão em servidores locais.


## Modelagem de Dados

### Tabelas utilizadas:

    - 'BaseClara'
    - BaseClaraHotelDiaria
    - Medida
    


### Relacionamentos entre tabelas (com print do modelo, se possível):

![image](https://github.com/user-attachments/assets/2c28b740-d305-49e6-85ad-11d15bb189eb)


## Transformações no Power Query:

**O modelo atual apresenta:**

- Erros em expressões DAX, com medidas e colunas calculadas que não estão funcionando corretamente, possivelmente devido à ausência de colunas de referência ou alterações nas tabelas de origem.

- Transformações com falhas no Power Query, incluindo etapas quebradas, referências a colunas inexistentes e erros de tipo de dados, o que impede a correta carga e modelagem das tabelas.
  
- Etiquetas e nomes de campos ausentes ou pouco descritivos, dificultando a compreensão dos dados por parte dos usuários finais e a manutenção do modelo por novos analistas.
  
- Ausência de dados ou dados incompletos, o que pode estar relacionado a falhas na atualização manual das bases no SharePoint ou à falta de validações no processo de ingestão.
- Falta de padronização e organização no modelo, com tabelas e campos não documentados, dificultando a identificação de tabelas de dimensão, fatos e KPIs.
  
Diante desse cenário, está sendo conduzido um processo de revisão completa do modelo, com foco em:

Corrigir os erros de transformação e DAX;
Reestruturar o modelo de dados com base em boas práticas de modelagem (fato/dimensão);
Padronizar nomes e aplicar descrições nos campos;
Documentar todas as fontes de dados, transformações, medidas e KPIs;
Garantir a confiabilidade e a clareza das informações apresentadas no dashboard.

 ## Medidas e Cálculos DAX:
 
### Lista de medidas criadas:

    - 'Qtd Comp Indevida'
    - 'Qtd de Diárias'
    - 'Qtd s/ Recibo'
    - 'Valor recusada Cord'
    - 'Valor s/ Aprovação'
    - 'Valor s/ Brastel'
    - 'Valor s/ CC'
    - 'Valor s/ Recibo'
    - 'Valor s/ Reembolso'
    - Valor Ticket Médio'
    - 'Valor Ticket Médio Max'
    - 'Valor Ticket Médio Min'
    - 'Valor Total c/ Hospedagem'
  
### Explicação de cada medida:

    - 'Qtd Comp Indevida':

  Essa medida chamada "Qtd Comp Indevida" retorna:0, se não houver nenhum valor na coluna Recibo;
A quantidade de registros com Status de aprovação = "Recusada", caso contrário.

 ```
Qtd Comp Indevida = 
    IF(COUNT(BaseClara[Recibo]) = 0, 0,
        CALCULATE(
            COUNT(BaseClara[Recibo]), 
            BaseClara[Status de aprovação] = "Recusada" 
        )
    )
```

    - 'Qtd de Diárias':
  Ela está tentando contar quantas linhas da coluna Qtd. Diárias existem onde o Status de aprovação é maior que 0. Se essa contagem for 0, retorna 0. Caso contrário, retorna a contagem.

> [!CAUTION]
> A condição BaseClara[Status de aprovação] > 0 só funciona se a coluna Status de aprovação for numérica. Mas, pelo nome, parece que essa coluna é textual (ex: "Aprovada", "Recusada", etc.).

Se for texto, a comparação > 0 não faz sentido e pode estar retornando BLANK() ou erro silencioso.

  
```  Qtd de Diárias =

     IF(CALCULATE(
            COUNT(BaseClara[Qtd. Diárias]), 
            BaseClara[Status de aprovação] > 0 
        ) = 0, 0,
        CALCULATE(
            COUNT(BaseClara[Qtd. Diárias]), 
            BaseClara[Status de aprovação] > 0 
        )
    )
```

    - ' Qtd s/ CC':

Esse código DAX está usando a função IF para verificar se há registros na tabela BaseClara com a etiqueta "Sem Centro de Custo", e retorna a contagem desses registros ou zero, dependendo do caso.
  
```  Qtd s/ CC =

    IF(CALCULATE(
            COUNT(BaseClara[Etiquetas]), 
                FILTER(BaseClara,
                BaseClara[Etiquetas] = "Sem Centro de Custo"
                )
        ) = BLANK(), 0,
                    CALCULATE(
                        COUNT(BaseClara[Etiquetas]), 
                        FILTER(BaseClara,
                            BaseClara[Etiquetas] = "Sem Centro de Custo"
                        )
                    )
    )
```
    - 'Qtd s/ Recibo':
   
Esse código DAX está usando a função IF para verificar se há registros na tabela BaseClara com a etiqueta "Não", e retorna a contagem desses registros ou zero, dependendo do caso.
  ```
Qtd s/ Recibo = 
    IF(CALCULATE(
        COUNT(BaseClara[Recibo]), 
            FILTER(BaseClara,
            BaseClara[Recibo] = "Não"
            )
        ) = BLANK(), 0,
        CALCULATE(
            COUNT(BaseClara[Recibo]), 
                FILTER(BaseClara,
                BaseClara[Recibo] = "Não"
                )
        )
    )
```

    - 'Valor Recusada Cordenação':

  Essa medida retorna o valor total em reais das compras recusadas, mas somente se houver algum valor na coluna Valor em R$. Caso contrário, retorna 0.
  
 ```
Valor Recusada Cordenação = 
    IF(SUM(BaseClara[Valor em R$]) = 0, 0,
        CALCULATE(
            SUM(BaseClara[Valor em R$]), 
            BaseClara[Status de aprovação] = "Recusada" 
        )
    )
```
    - 'Valor s/ Aprovação':

Essa medida retorna o valor total das compras que ainda precisam de aprovação. Se não houver nenhuma, retorna 0.

 ```
Valor s/ Aprovação = 
    IF(CALCULATE(
        SUM(BaseClara[Valor em R$]), 
            FILTER(BaseClara,
                BaseClara[Status de Aprovação] = "Necessita aprovação"
            )
        ) = BLANK(), 0,
        CALCULATE(
            SUM(BaseClara[Valor em R$]), 
            FILTER(BaseClara,
                BaseClara[Status de Aprovação] = "Necessita aprovação"
            )
        )
    )
```

    - 'Valor s/ Brastel':

  Essa medida retorna a soma de todos os valores em reais, excluindo os registros cujo titular é "Brasil S.A."

 ```
Valor s/ Brastel = 
CALCULATE(
    SUM(BaseClara[Valor em R$]),
    FILTER(BaseClara,
        BaseClara[Titular] <> "Brasil S.A."
    )
)
```
    - 'Valor s/ CC':

 Essa medida retorna o valor total em reais de registros com a etiqueta "Sem Centro de Custo", e garante que, se não houver nenhum, o resultado será 0 em vez de BLANK().
  
 ```
IF(CALCULATE(
        SUM(BaseClara[Valor em R$]), 
            FILTER(BaseClara,
                BaseClara[Etiquetas] = "Sem Centro de Custo"
            )
        ) = BLANK(), 0,
        CALCULATE(
            SUM(BaseClara[Valor em R$]), 
            FILTER(BaseClara,
                BaseClara[Etiquetas] = "Sem Centro de Custo"
            )
        )
    )
```

    - 'Valor s/ Reembolso':

  Essa medida retorna o valor total em reais das compras feitas pelo titular "SBRASIL S.A.", mas só se houver algum valor na coluna Valor em R$. Caso contrário, retorna 0.
  
 ```
Valor s/ Reembolso = 
    IF(SUM(BaseClara[Valor em R$]) = 0, 0,
        CALCULATE(
            SUM(BaseClara[Valor em R$]), 
            FILTER(BaseClara,
                BaseClara[Titular] = "SBRASIL S.A."
            )
        )
    )
```

    - 'Valor Ticket Médio':

Essa medida retorna o valor médio por diária para compras da categoria "Viagem E Hospedagem", considerando apenas registros com pelo menos uma diária.
   
  ```
   Valor Ticket Médio = 
CALCULATE(
    DIVIDE(
        SUM('BaseClara'[Valor em R$]),
        SUM('BaseClara'[Qtd. Diárias])
    ),
    'BaseClara'[Categoria da Compra] = "Viagem E Hospedagem",
    'BaseClara'[Qtd. Diárias] > 0
)
```


    - 'Valor Ticket Médio Máx':

   O que ela tenta fazer é verificar se a soma da coluna Ticket Médio é zero.Se for, retorna 0. Caso contrário, retorna o maior valor de ticket médio, excluindo registros com:
Titular = "BRASTEL"
Ticket Médio ≤ 0

> [!CAUTION]
> Uso direto de múltiplas condições no CALCULATE:
 Embora funcione em alguns casos, é mais seguro e flexível usar FILTER(...) para múltiplas condições.
> SUM(BaseClara[Ticket Médio]) = 0 pode não cobrir casos onde todos os valores são BLANK() (não zero).
   
  ```
Valor Ticket Médio Máx = 
   IF( SUM(BaseClara[Ticket Médio]) = 0, 0, 
CALCULATE( MAX(BaseClara[Ticket Médio]), BaseClara[Titular] <> "BRASTEL" , BaseClara[Ticket Médio] > 0 ))
  
 ```

    - 'Valor Ticket Médio Min':

  Verifica se a soma da coluna Ticket Médio é igual a zero.Se for, retorna 0.Caso contrário, retorna o menor valor de ticket médio, considerando apenas:
Registros onde o titular não é "BRASTEL".Registros onde o Ticket Médio é maior que 0.

> [!CAUTION]
> O uso direto de múltiplas condições no CALCULATE pode funcionar, mas é mais seguro e flexível usar FILTER(...).A verificação SUM(...) = 0 não cobre casos em que todos os valores são BLANK().

```
Valor Ticket Médio Min = 
IF( SUM(BaseClara[Ticket Médio]) = 0, 0, 
CALCULATE( MIN(BaseClara[Ticket Médio]), BaseClara[Titular] <> "BRASTEL" , BaseClara[Ticket Médio] > 0 ))

```

    - Valor Total c/ Hospedagem:

Essa medida retorna a soma dos valores em reais da tabela 'BaseClara', somente para registros que:

Têm diárias maiores que zero, e
São da categoria "Viagem E Hospedagem".
Se não houver nenhum valor na coluna [Valor em R$], retorna 0 diretamente.

 ```
Valor Total c/ Hospedagem = 
IF( SUM('BaseClara'[Valor em R$]) = 0 , 0 ,
    CALCULATE(
        SUM('BaseClara'[Valor em R$]),
        'BaseClara'[Qtd. Diárias] > 0,
        'BaseClara'[Categoria da Compra] = "Viagem E Hospedagem"
    )
)

```
### Colunas calculadas e suas funções:

    -'Ticket Médio'
  Somatório da coluna 'Ticket Médio'

    - 'Valor em R$'
Somatório da coluna 'Valor em R$'
    
    -'Valor Original'
    
Somatório da coluna 'Valor em R$'

    -Data da Transação'
    
Formatação da coluna no Tipo Data

### KPIs principais



## Filtros e Segmentações:

Filtros são usados para limitar os dados que aparecem nos visuais. E Segmentações são visuais interativos que permitem ao usuário selecionar valores para filtrar os dados dinamicamente.


### Segmentações (slicers):

- Pagina Home:

 Segmentação de dados ligado a coluna 'Data transação' da tabela 'Base Clara'.

- Pagina Hospedagem:

Segmentação de dados ligado a coluna 'Data transação' da tabela 'Base Clara'.


### Bookmarks ou drill-throughs:

Não possui nenhum bookmark ou drill aplicado.

## Atualização e Publicação:

(Frequência de atualização dos dados
Local de publicação (workspace)
Agendamentos configurados
Alertas ou assinaturas?)

## Problemas Identificados e Melhorias Sugeridas:

Etapas aplicadas no Power Query estão quebradas e com vários erros Dax.É necessário um processo de revisão completa do modelo, com foco em:

- Corrigir os erros de transformação e DAX;
- Reestruturar o modelo de dados com base em boas práticas de modelagem (fato/dimensão);
- Padronizar nomes e aplicar descrições nos campos;
- Documentar todas as fontes de dados, transformações, medidas e KPIs;
- Garantir a confiabilidade e a clareza das informações apresentadas no dashboard.

   
