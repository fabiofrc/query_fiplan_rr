# query_fiplan_rr
Análise  exploratória funções sql do FIPLAN - RR 

--select * from fiplan.acwtb0070;
select  NOME_OBJETO, DESC_RESUMIDA, DESC_OBJETO from fiplan.acwtb0070
WHERE
DESC_OBJETO
LIKE '%esfera%';

select  NOME_OBJETO, DESC_RESUMIDA, DESC_OBJETO from fiplan.acwtb0070
WHERE
NOME_OBJETO = 'PROGRAMA_GOVERNO_INDICADOR';

SELECT * FROM FIPLAN.INDICADOR_PROGRAMA_GOVERNO;
SELECT * FROM FIPLAN.PROGRAMA_GOVERNO_INDICADOR;

SELECT * FROM FIPLAN.ACWTB0257 where cd_exercicio = 2019;

-- "Tabela que armazena os identificadores de uso, que são atributos que compõe a dotação orçamentária da despesa. Quando este atributo foi criado, 
-- seu papel era permitir segregar o orçamento comprometido como contrapartidas de convênio das demais dotações orçamentárias justamente para permitir 
-- a definição de regras negociam na aplicação que impedissem o remanejamento ou uso do orçamento de contrapartida com ações que não fossem a 
-- aplicação junto aos convênios celebrados pelo Estado junto ao Governo Federal. Até então, a tabela era chamada de identificador de contrapartida.
-- A partir de 2014, esta tabela passa a se chamar de Tabela de Identificador de Uso, pois passa a agregar também a função de identificar, 
-- além das dotações orçamentárias de contrapartidas de convênios, também as dotações orçamentárias criadas em razão das Emendas Parlamentares durante o 
-- processo de aprovação da Lei Orçamentária Anual (LOA). O objetivo neste caso também era permitir a aplicação de regras negociais sobre o orçamento das Emendas Parlamentares.
SELECT * FROM FIPLAN.IDENTIFICADOR_CONTRA_PARTIDA;

-- Tabela que armazena informações do documento ARR - Autorização de Repasse de Receita.
SELECT * FROM FIPLAN.ACWTB0113;

-- Tabela que armazena informações do documento BLO - Bloqueio da Despesa Orçamentária.
SELECT * FROM FIPLAN.ACWTB0198;

-- Tabela que armazena informações das dotações orçamentárias de uma DOTLIST - Lista de Dotações.
SELECT * FROM FIPLAN.ACWTB0317;

-- Tabela que armazena informações do documento NDD - Nota de Destaque (MÓDULO NÃO IMPLANTADO NO FIPLAN-RR).
SELECT * FROM FIPLAN.ACWTB0226;

-- Tabela que armazena informações dos elementos, anulantes e suplementantes, associados a um projeto do processo de créditos adicionais.
SELECT * FROM FIPLAN.ACWTB0161;

-- Tabela que armazena histórico das alterações dos identificadores de contrapartida. Esta funcionalidade automatiza procedimentos dos créditos adicionais, 
-- da geração da NPO/NPD até a efetivação do processo de crédito adicional.
SELECT * FROM FIPLAN.ACWTB0355;

-- Tabela que armazena informações do documento NPO - Nota de Provisão Orçamentária.
SELECT * FROM FIPLAN.ACWTB0053;

-- Tabela que armazena as dotações orçamentárias da despesa. Dotações Orçamentárias são valores monetários autorizados, 
-- consignados na Lei Orçamentária Anual (LOA) para atender a uma determinada programação orçamentária;
-- é toda e qualquer verba prevista como despesa em orçamentos públicos e destinada a fins específicos.
SELECT * FROM FIPLAN.DOTACAO_ORCAMENTARIA;

select * from FIPLAN.NATUREZA_RECEITA where ROWNUM <= 100;

select nr.DS_NATUREZA_RECEITA, r.VL_PROPOSTO, r.VL_ATUAL, r.DATA_CADASTRO, r.DATA_ATUALIZACAO from FIPLAN.RECEITA r
join FIPLAN.NATUREZA_RECEITA nr on nr.ID_NATUREZA_RECEITA = r.ID_NATUREZA_RECEITA;

-- DADOS DE RCEITA
-- Tabela que armazena informação das receitas previstas, associadas a uma elaboração de receita.
select * from FIPLAN.RECEITA;
-- Tabela que armazena informação das receitas previstas mês a mês, associadas a um registro de receita da elaboração de receita.
select rm.vl_realizado, rm.cd_mes, er.cd_exercicio, er.cd_exercicio, n.ds_natureza_receita from FIPLAN.RECEITA_MES rm 
join FIPLAN.receita r on rm.id_receita = r.id_receita
JOIN FIPLAN.elaboracao_receita er on r.id_elaboracao_receita = er.id_elaboracao_receita
join FIPLAN.NATUREZA_RECEITA n on n.id_natureza_receita = r.id_natureza_receita
join FIPLAN.exercicio e on er.cd_exercicio = e.cd_exercicio
WHERE 
--rm.cd_mes = 1 and 
er.cd_exercicio = 2019 ORDER BY rm.cd_mes asc;
--and n.ds_natureza_receita like '%RECEITAS CORRENTES%';

select * from FIPLAN.exercicio e;

select * from FIPLAN.NATUREZA_RECEITA where ds_natureza_receita like '%RECEITAS CORRENTES%';

-- DADOS DE DEPESA
select DISTINCT(DS_TIPO_DESPESA) from FIPLAN.TIPO_DESPESA;
select * from FIPLAN.TIPO_DESPESA;
select * from FIPLAN.GRUPO_DESPESA;
select * from FIPLAN.SUBELEMENTO_DESPESA;
select * from FIPLAN.ELEMENTO_DESPESA;
select * from FIPLAN.NATUREZA_DESPESA;

-- Tabela que armazena informações do documento PED - Pedido de Empenho da Despesa.
select * from FIPLAN.ACWTB0027;

select p.VALR_PED, p.SALDO_ORCAMENTARIO_ANTERIOR, e.CD_EXERCICIO from FIPLAN.ACWTB0027 p
JOIN FIPLAN.EXERCICIO e on e.CD_EXERCICIO = p.CD_EXERCICIO;


-- Tabela que armazena informações dos valores das medidas do PPA - Plano Plurianual.
select * from fiplan.ACWTB0272;

-- Tabela queo armazena informações das Unidades Orçamentárias do Estado.
select * from FIPLAN.UNIDADE_ORCAMENTARIA;

-- Tabela que armazena das Unidades Gestoras das Unidades Orçamentárias do Estado.
select  * from FIPLAN.UNIDADE_GESTORA;
select * from fiplan.ACWTB0257;
select * from fiplan.ACWTB0219;
select  count(*) from FIPLAN.USUARIO;

select
--e.CD_EXERCICIO "ANO",
uo.DS_SIGLA "ENTIDADE",
sum(t.VL_EXTRA) "EXTRA",
sum(t.VL_PESSOAL) "PESSOAL"
from FIPLAN.TETO_ORCAMENTARIO t
join FIPLAN.UNIDADE_ORCAMENTARIA uo on t.ID_UNIDADE_ORCAMENTARIA = uo.ID_UNIDADE_ORCAMENTARIA
join FIPLAN.EXERCICIO e on uo.CD_EXERCICIO = e.CD_EXERCICIO
group by e.CD_EXERCICIO, UO.DS_SIGLA order by e.CD_EXERCICIO asc;

--select * from FIPLAN.ACWTB0261;

--select d.LOCALIDADE, d.MOTIVO, d.VALR_DIARIA, d.DATA_RETORNO_VIAGEM, d.DATA_INICIO_VIAGEM, (d.DATA_RETORNO_VIAGEM - d.DATA_INICIO_VIAGEM) as quant_dias
--from FIPLAN.ACWTB0261 d order by d.DATA_INICIO_VIAGEM desc;

--select d.DATA_INICIO_VIAGEM
--from FIPLAN.ACWTB0261 d order by d.DATA_INICIO_VIAGEM desc;

select extract(year from sysdate) from dual;
select count(*) from fiplan.acwtb0070;

-- Tabela que armazena atributos e parâmetros de gestão por exercício, tratado no sistema FIPLAN como tabela de parâmetros orçamentários.
-- Os parâmetros orçamentários constituem atributos utilizados para nortear os processos de planejamento e execução orçamentária da receita e da despesa.
select *  from FIPLAN.EXERCICIO order by CD_EXERCICIO asc;

-- Tabela que compõe a base contábil do sistema FIPLAN, denominada CONTÁBIL CONSOLIDADO MENSAL DA PROGRAMAÇÃO FINANCEIRA.
-- Esta tabela armazena os valores consolidados dos lançamentos contábeis agrupados pela chave a seguir,
-- gravando os valores nas colunas dos meses extraídos das posições 14-15 da conta corrente contábil:
-- "	Exercício	 (CD_EXERCICIO)
-- "	Código da UO + UG 	 (ID_UNIDADE_GESTORA)
-- "	Código da Conta Contábil 	 (IDEN_CONTA_CONTABIL)
-- "	Nº Conta Corrente Contábil 	 (NUMR_CONTA_CORRENTE)
--
-- Na tabela de conta contábil (ACWTB0032), as contas contábeis de controle da programação financeira cuja CCC é "UO + UG + Fonte de Recursos + Grupo de Despesa + Mês"
-- (e o mês corresponde às posições 14-15 da CCC) são sinalizadas a partir do campo FLAG_ATUALIZA_SALDO_PROG_FINAN desta tabela,
-- quais contas terão seu saldo consolidado nesta tabela.
-- A persistência nesta tabela é feita através de trigger no banco de dados, para toda ação de INSERT,
-- DELETE ou UPDATE ocorrida na tabela de Lançamentos Contábeis Analíticos (ACWTB0088) para as contas onde campo supracitado é igual a 856 (Sim).
select * from fiplan.ACWTB0257;

-- Considerada com uma das tabelas mais importantes do sistema e que compõe a base contábil do sistema FIPLAN,
-- é a principal delas e que recebe a denominação de CONTÁBIL ANALÍTICO, justamente por armazenar os lançamentos contábeis analíticos gerados pelos documentos contábeis do sistema.
-- A persistência nesta tabela é feita pelo componente de lançamentos contábeis (motor contábil), que é disparado na confirmação das ações envolvendo os documentos contábeis.
select * from FIPLAN.ACWTB0088;

-- Tabela que armazena as contas contábeis que compõem o Plano de Contas.
select * from FIPLAN.ACWTB0032;

select * from FIPLAN.VW_PTA;

-- Tabelas que armazena os tetos orçamentários por Unidade Orçamentária, que são limitadores aplicados às Unidades Orçamentárias na elaboração da receita.
-- Além de estabelecer o teto para fixação das despesas por UO, determina ainda em cada qual o valor destinado a despesas com pessoal (grupo de despesa = 1)
-- e extra-pessoal (demais grupos de despesa).
-- Nesta tabela são armazenadas apenas informações do teto anual.
select * from FIPLAN.TETO_ORCAMENTARIO;

-- Tabela que armazena as dimensões estratégicas de gestão. O Este termo "Área Política de Atuação do Governo" foi adotado pelo FIPLAN-MT,
-- mas modificado pela equipe gestora do FIPLAN-RR quando da implantação do sistema no Estado de Roraima.
select * from FIPLAN.AREA_POLITICA;

-- Tabela que armazena as dotações orçamentárias da despesa. Dotações Orçamentárias são valores monetários autorizados,
-- consignados na Lei Orçamentária Anual (LOA) para atender a uma determinada programação orçamentária;
-- é toda e qualquer verba prevista como despesa em orçamentos públicos e destinada a fins específicos.
SELECT * FROM FIPLAN.DOTACAO_ORCAMENTARIA where cd_exercicio = 2019;

SELECT e.CD_EXERCICIO, do.VL_PROPOSTO, do.VALR_DOTACAO_CONCEDIDA, do.VALR_DOTACAO_CONCEDIDA, f.DS_FUNCAO
FROM FIPLAN.DOTACAO_ORCAMENTARIA do
join FIPLAN.EXERCICIO e on e.CD_EXERCICIO = do.CD_EXERCICIO
join FIPLAN.FUNCAO f on f.ID_FUNCAO = do.ID_FUNCAO;

SELECT * FROM FIPLAN.PROGRAMA_GOVERNO;

-- Tabela que armazena as categorias econômicas da receita e da despesa.
SELECT * FROM FIPLAN.CATEGORIA_ECONOMICA;

-- Tabela que armazena as categorias de investimento. As categorias de investimento são representadas apenas ao nível de planejamento,
-- apenas para fins de avaliação e análise do orçamento previsto. É definida na memória de cálculo do Plano Anual de Trabalho (PAT).
SELECT * FROM FIPLAN.CATEGORIA_INVESTIMENTO;


SELECT * FROM FIPLAN.TIPO_INSTITUICAO;

-- Tabela que armazena informações do PAT - Plano Anual de Trabalho. Cada PAT detalha a despesa de um determinado programa de governo,
-- em suas diferentes ações, estas, por sua vez, ligadas às Unidades Orçamentárias do Estado.
-- O termo "PTA" vem de Plano de Trabalho Anual, denominação atribuída ao PAT pelo Estado de Mato Grosso,
-- e que trouxe a tabela na implantação do FIPLAN-RR utilizando o legado do FIPLAN-MT.
SELECT * FROM FIPLAN.PTA;

-- Tabela que armazena os tipos de recurso da fonte. Este tipo é um atributo da tabela FONTE_RECURSO,
-- e possui, além da tabela para armazena os tipos de valores para o campo FLG_TIPO_RECURSO, também o domínio e itens-domínios correspondentes.
-- A tabela foi mantida por haver classes utilizando as duas estruturas, apesar de ambas terem a mesma finalidade.
SELECT * FROM FIPLAN.TIPO_RECURSO;

-- Tabela que armazena informações dos valores das medidas do PPA - Plano Plurianual.
SELECT * FROM FIPLAN.ACWTB0272;

-- Tabela que armazena informações das medidas do PPA - Plano Plurianual.
SELECT * FROM FIPLAN.ACWTB0271;

-- Tabela que armazena informações referentes à regionalização das ações do PPA - Plano Plurianual.
SELECT * FROM FIPLAN.ACWTB0270;

-- Tabela que armazena informações das ações (PAOE x UO) de um PPA - Plano Plurianual.
SELECT * FROM FIPLAN.ACWTB0269;

-- Tabela que armazena informações do PPA - Plano Plurianual.
SELECT * FROM FIPLAN.ACWTB0268;

-- Tabela associativa entre o usuário e seus programas de governo. Esta associação permite especiais quais programas de governo estão habilitados a um usuário,
-- por exercício, para lançamento do PAT e PPA.
SELECT * FROM FIPLAN.USUARIO_PROGRAMA_GOVERNO;

-- Tabela que armazena informações da consolidação do PPA, utilizada como insumo para geração dos documentos ABP (Abertura do PPA) e ALP (Alteração do PPA).
SELECT * FROM FIPLAN.ACWTB0443;

-- Tabela que armazena informações das funções de governo. As funções de governo hoje obedecem à classificação única e padronizada a todos entes da Federação,
-- em conformidade com o que preconiza a Portaria Interministerial STN/SOF Nº 163/2001.
SELECT * FROM FIPLAN.FUNCAO;

-- Tabela que armazena informações das subfunções de governo. As subfunções de governo hoje obedecem à classificação única e padronizada a todos entes da Federação,
-- em conformidade com o que preconiza a Portaria Interministerial STN/SOF Nº 163/2001.
SELECT * FROM FIPLAN.SUBFUNCAO;

select * from FIPLAN.OBJETIVO_ESTRATEGICO;

-- Tabela que armazena as ações de governo, distribuídas nos Projetos, Atividades e Operações Especiais (P/A/O/E) do Estado.
SELECT * FROM FIPLAN.PAOE;

-- Indicador do Programa de Governo
SELECT * FROM FIPLAN.INDICADOR_PROGRAMA_GOVERNO;

-- Tabela que armazena as modalidades de aplicação da despesa.
SELECT * FROM FIPLAN.MODALIDADE_APLICACAO;

-- Tabela que armazena as dimensões estratégicas de gestão. O Este termo "Área Política de Atuação do Governo" foi adotado pelo FIPLAN-MT,
-- mas modificado pela equipe gestora do FIPLAN-RR quando da implantação do sistema no Estado de Roraima.
select * from FIPLAN.AREA_POLITICA;
select distinct (DS_SIGLA) from FIPLAN.AREA_POLITICA;

-- programa de governo e indicadores
select * from FIPLAN.indicador_programa_governo ipg 
JOIN FIPLAN.programa_governo_indicador pgi on pgi.id_indicador_programa_governo = ipg.id_indicador_programa_governo;




