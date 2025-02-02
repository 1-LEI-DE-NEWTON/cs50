# Consolidar dados da B3 (Less)
Implemente um programa que exibe dados da B3 em um formato de tabela
```
$ ./b3data arquivo1.csv arquivo2.csv codigo
```

# Fundamentação

A B3 (Brasil, Bolsa e Balcão) disponibiliza diversos arquivos com dados sobre os diversos produtos negociados em bolsa e balcão. Muitos destes arquivos estão no formato CSV (_Comma Separeted Value_, ou Valores Separados por Vírgula).

Nesse formato de arquivo, temos uma tabela com várias linhas e colunas. As linhas são delimitadas por caracteres específicos `\r`, `\n` ou `\r\n`. As colunas são delimitadas por vírgula ou qualquer outro caractere que não apareça nos próprios dados.

No caso dos arquivos disponíveis na B3, como a vírgula é um caractere usado para separar as partes inteira e fracionária (ou inteira e decimal) de números como moedas ou cotações, então os arquivos usam um ponto e vírgula como separador. Além disso, alguns arquivos usam `\n` como fim de linha enquanto outros usam `\r\n`.

## Exemplo
O código abaixo representa o trecho de um desses arquivos, mais especificamente o arquivo sobre **Cadastro de Instrumentos Listados**.
```
RptDt;TckrSymb;Asst;AsstDesc;SgmtNm;MktNm;SctyCtgyNm;XprtnDt;XprtnCd;TradgStartDt;TradgEndDt;BaseCd;ConvsCritNm;MtrtyDtTrgtPt;ReqrdConvsInd;ISIN;CFICd;DlvryNtceStartDt;DlvryNtceEndDt;OptnTp;CtrctMltplr;AsstQtnQty;AllcnRndLot;TradgCcy;DlvryTpNm;WdrwlDays;WrkgDays;ClnrDays;RlvrBasePricNm;OpngFutrPosDay;SdTpCd1;UndrlygTckrSymb1;SdTpCd2;UndrlygTckrSymb2;PureGoldWght;ExrcPric;OptnStyle;ValTpNm;PrmUpfrntInd;OpngPosLmtDt;DstrbtnId;PricFctr;DaysToSttlm;SrsTpNm;PrtcnFlg;AutomtcExrcInd;SpcfctnCd;CrpnNm;CorpActnStartDt;CtdyTrtmntTpNm;MktCptlstn;CorpGovnLvlNm
2022-01-28;PETRB25;PETR4;PETR;EQUITY CALL;EQUITY-DERIVATE;OPTION ON EQUITIES;2023-02-17;;2021-01-20;2023-02-17;;;;;BRPETR4B0US1;OCASPS;;;Call;;;100;BRL;FINANCIAL;;;;;;;;;;;19,51;AMER;;true;;197;1;1;SEM CORRECAO;true;false;;;;;;
2022-01-28;PETRB251;PETR4;PETR;EQUITY CALL;EQUITY-DERIVATE;OPTION ON EQUITIES;2023-02-17;;2021-02-17;2023-02-17;;;;;BRPETR4B0UW3;OCESPS;;;Call;;;100;BRL;FINANCIAL;;;;;;;;;;;19,76;EURO;;true;;197;1;1;SEM CORRECAO;true;false;;;;;;
2022-01-28;PETRB252;PETR4;PETR;EQUITY CALL;EQUITY-DERIVATE;OPTION ON EQUITIES;2022-02-18;;2021-01-29;2022-02-18;;;;;BRPETR4B0UU7;OCESPS;;;Call;;;100;BRL;FINANCIAL;;;;;;;;;;;19,76;EURO;;true;;197;1;1;SEM CORRECAO;true;false;;;;;;
2022-01-28;PETRB254;PETR4;PETR;EQUITY CALL;EQUITY-DERIVATE;OPTION ON EQUITIES;2022-02-18;;2021-12-21;2022-02-18;;;;;BRPETR4B0WT5;OCESPS;;;Call;;;100;BRL;FINANCIAL;;;;;;;;;;;27,76;EURO;;true;;201;1;1;SEM CORRECAO;true;false;;;;;;
2022-01-28;PETRB255;PETR3;PETR;EQUITY CALL;EQUITY-DERIVATE;OPTION ON EQUITIES;2022-02-18;;2021-11-17;2022-02-18;;;;;BRPETR3B05T6;OCASPS;;;Call;;;100;BRL;FINANCIAL;;;;;;;;;;;22,41;AMER;;true;;193;1;1;SEM CORRECAO;true;false;;;;;;
2022-01-28;PETRB256;PETR4;PETR;EQUITY CALL;EQUITY-DERIVATE;OPTION ON EQUITIES;2022-02-18;;2021-11-30;2022-02-18;;;;;BRPETR4B0W16;OCASPS;;;Call;;;100;BRL;FINANCIAL;;;;;;;;;;;22,51;AMER;;true;;200;1;1;SEM CORRECAO;true;false;;;;;;
2022-01-28;PETRB257;PETR4;PETR;EQUITY CALL;EQUITY-DERIVATE;OPTION ON EQUITIES;2022-02-18;;2021-12-21;2022-02-18;;;;;BRPETR4B0WM0;OCESPS;;;Call;;;100;BRL;FINANCIAL;;;;;;;;;;;30,26;EURO;;true;;201;1;1;SEM CORRECAO;true;false;;;;;;
2022-01-28;PETRB259;PETR4;PETR;EQUITY CALL;EQUITY-DERIVATE;OPTION ON EQUITIES;2022-02-18;;2021-11-29;2022-02-18;;;;;BRPETR4B0VO8;OCASPS;;;Call;;;100;BRL;FINANCIAL;;;;;;;;;;;24,01;AMER;;true;;200;1;1;SEM CORRECAO;true;false;;;;;;
2022-01-28;PETRC102;PETR4;PETR;EQUITY CALL;EQUITY-DERIVATE;OPTION ON EQUITIES;2022-03-18;;2022-01-27;2022-03-18;;;;;BRPETR4C0XK1;OCESPS;;;Call;;;100;BRL;FINANCIAL;;;;;;;;;;;10,26;EURO;;true;;201;1;1;SEM CORRECAO;true;false;;;;;;
2022-01-28;PETRC105;PETR4;PETR;EQUITY CALL;EQUITY-DERIVATE;OPTION ON EQUITIES;2022-03-18;;2022-01-27;2022-03-18;;;;;BRPETR4C0XM7;OCASPS;;;Call;;;100;BRL;FINANCIAL;;;;;;;;;;;10,51;AMER;;true;;201;1;1;SEM CORRECAO;true;false;;;;;;
2022-01-28;PETRC115;PETR4;PETR;EQUITY CALL;EQUITY-DERIVATE;OPTION ON EQUITIES;2022-03-18;;2022-01-27;2022-03-18;;;;;BRPETR4C0XP0;OCASPS;;;Call;;;100;BRL;FINANCIAL;;;;;;;;;;;11,51;AMER;;true;;201;1;1;SEM CORRECAO;true;false;;;;;;
```
e o trecho seguinte vem do arquivo **Posições Abertas em Derivativos**
```
RptDt;TckrSymb;ISIN;Asst;XprtnCd;SgmtNm;OpnIntrst;VartnOpnIntrst;DstrbtnId;CvrdQty;TtlBlckdPos;UcvrdQty;TtlPos;BrrwrQty;LndrQty;CurQty;FwdPric
2022-01-28;PETRB25;BRPETR4B0US1;PETR;; EQUITY CALL;;;197;60200;78300;105800;244300;109;22;;
2022-01-28;PETRB251;BRPETR4B0UW3;PETR;;EQUITY CALL;;;197;0;0;2000;2000;1;1;;
2022-01-28;PETRB252;BRPETR4B0UU7;PETR;;EQUITY CALL;;;197;7400;215500;192400;415300;17;27;;
2022-01-28;PETRB254;BRPETR4B0WT5;PETR;;EQUITY CALL;;;201;151100;3715300;398100;4264500;18;163;;
2022-01-28;PETRB256;BRPETR4B0W16;PETR;;EQUITY CALL;;;200;23100;267600;81000;371700;5;67;;
2022-01-28;PETRB257;BRPETR4B0WM0;PETR;;EQUITY CALL;;;201;283900;8116700;843700;9244300;42;307;;
2022-01-28;PETRB259;BRPETR4B0VO8;PETR;;EQUITY CALL;;;200;54800;350100;261400;666300;19;127;;
```
Pereba que existem múltiplas linhas mas todas possuem a mesma quantidade de colunas, mas o número de colunas e o conteúdo podem ser diferentes entre os arquivos.

Os dois arquivos anteriores estão relacionados por meio da coluna `TckrSymb`. Dados que possuem o mesmo `TckrSymb` em arquivos distintos, são relacionados ao mesmo produto negociado na B3. Por exemplo, o produto `PETRB257` está presente nos dois arquivos.

O arquivo **Cadastro de Instrumentos Listados**, como o próprio nome sugere, contém informações sobre todos os produtos listados (disponíveis) para negociação na B3 na data do arquivo (`RptDt`). Já o arquivo **Posições Abertas em Derivativos** contém dados dos produtos que foram negociados e ainda possuem operações em aberto na data em questão (`RptDt`).


# Obtendo dados e entendendo o conteúdo do arquivo
Para obter esses dados, você pode acessar o [site da B3](https://www.b3.com.br/pt_br/market-data-e-indices/servicos-de-dados/market-data/consultas/boletim-diario/dados-publicos-de-produtos-listados-e-de-balcao/), escolher o dia e o arquivo que deseja baixar.

Para saber como cada arquivo está organizado, basta acessar esse outro [link](https://www.b3.com.br/pt_br/market-data-e-indices/servicos-de-dados/market-data/consultas/boletim-diario/dados-publicos-de-produtos-listados-e-de-balcao/glossario/), também no site da B3.

O arquivo do trecho está acima trata-se das posições em aberto de empréstimo de ativos cujo conteúdo é descrito pelo [seguinte documento](https://www.b3.com.br/data/files/0F/47/1F/F6/D7A6E610B60806E6AC094EA8/Posicoes%20em%20Aberto%20de%20Emprestimo%20de%20Ativos.pdf).

# Objetivo
O objetivo do presente trabalho é você lê o conteúdo dos dois arquivos e consolidar em uma tabela única.
- Do arquivo **Dadastro de Instrumentos Listados**, você deve usar as seguintes colunas:
   - RptDt
   - TckrSymb
   - Asst
   - XprtnDt
   - ExrcPric
   - OptnStyle
- Do arquivo **Posições Abertas em Derivativos**, você deve usar as seguintes colunas:
   - SgmtNm
   - CvrdQty
   - TtlBlckPos
   - UcvrdQty
- Perceba que não é necessário usar as colunas `RptDt`, `TckrSymb` e `Asst` do segundo arquivo, pois ela contém o mesmo conteúdo que no primeiro arquivo.
  - Embora você não precise exibir o conteúdo dessas colunas, você precisa usar a coluna `TckrSymb` para combinar corretamente os dados dos arquivos.
- Se o arquivo contiver números em formato decimal, a vírgula deve ser substituída por um ponto.
- Números devem ser alinhados à direita e textos (strings) devem ser alinhados à esquerda.
- O resultado deve ser semelhante ao abaixo:
```
./b3data arquivo1.csv arquivo2.csv PETR4
RptDt      | TckrSymb | Asst  | XprtnDt    | ExrcPric | OptnStyle | SgmtNm      | CvrdQty | TtlBlckPos | UcvrdQty
2022-01-28 | PETRB25  | PETR4 | 2023-02-17 |    19.51 | AMER      | EQUITY CALL |   60200 |      78300 |   105800
2022-01-28 | PETRB251 | PETR4 | 2023-02-17 |    19.76 | EURO      | EQUITY CALL |       0 |          0 |     2000
2022-01-28 | PETRB252 | PETR4 | 2022-02-18 |    19.76 | EURO      | EQUITY CALL |    7400 |     215500 |   192400
2022-01-28 | PETRB254 | PETR4 | 2022-02-18 |    27.76 | EURO      | EQUITY CALL |  151100 |    3715300 |   398100
2022-01-28 | PETRB256 | PETR4 | 2022-02-18 |    22.51 | AMER      | EQUITY CALL |   23100 |     267600 |    81000
2022-01-28 | PETRB257 | PETR4 | 2022-02-18 |    30.26 | EURO      | EQUITY CALL |  283900 |    8116700 |   843700
2022-01-28 | PETRB259 | PETR4 | 2022-02-18 |    24.01 | AMER      | EQUITY CALL |   54800 |     350100 |   261400
```

- Note que nem todas as linhas aparecem, pois algumas linhas correspondentes aos produtos listados não tinha negócios em aberto na data do arquivo.
- Observe ainda que textos (strings) estão alinhados à esquerda e números estão alinhados à direita.
    - Além disso, números decimais que usam vírgula como separador, no resultado estão usando ponto decimal.
- Note que só foram exibidos os dados referentes ao código PETR4 que se encontra na coluna `Asst` do primeiro arquivo.
# Desafio
- Você consegue fazer seu código de modo que ele trate arquivos com os diversos tipos de final de linha?

<!---
## Testando seu código

Certifique-se de testar seu código nas imagens de bitmap fornecidas

Execute o comando abaixo para verificar a **corretude** do seu código. Mas tente compilar e testar antes de executar o comando
```
check50 cs50/problems/2020/x/filter/less
```

Execute o comando abaixo para garantir a **estilização** do código
```
style50 helpers.c
```


## Enviando seu código
Execute o comando abaixo, logando com seu **nome de usuário** do GitHub, para enviar seu código. Por questões de segurança, asteríscos serão exibidos em vez dos caracteres da sua senha
```
submit50 cs50/problems/2020/x/filter/less
```
--->