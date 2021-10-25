# Analise de dados Enem
![GitHub](https://img.shields.io/github/license/EryckNoronha/dados-enem)

```python
import pandas as pd
import pandas_profiling as pp
import matplotlib
import matplotlib.pyplot as plt
```

```python
#lendo o arqivo csv
#nrows= 1000 
d = pd.read_csv('MICRODADOS_ENEM_2018.csv',sep= ';', encoding= 'ISO-8859-1', nrows= 10000)
```
```python
#para saber o nome de todas a s colunas dentro de um array
d.columns.values
```

```python
colunas_selecionadas_d = ['TP_SEXO','NO_MUNICIPIO_ESC','NU_IDADE', 'CO_UF_ESC', 'SG_UF_ESC',
       'TP_DEPENDENCIA_ADM_ESC', 'TP_LOCALIZACAO_ESC', 'TP_SIT_FUNC_ESC',
       'IN_BAIXA_VISAO', 'IN_CEGUEIRA', 'IN_SURDEZ',
       'IN_DEFICIENCIA_AUDITIVA', 'IN_SURDO_CEGUEIRA',
       'IN_DEFICIENCIA_FISICA', 'IN_DEFICIENCIA_MENTAL',
       'IN_DEFICIT_ATENCAO', 'IN_DISLEXIA', 'IN_DISCALCULIA',
       'IN_AUTISMO', 'IN_VISAO_MONOCULAR', 'IN_OUTRA_DEF', 'IN_GESTANTE',
       'IN_LACTANTE', 'IN_IDOSO', 'IN_ESTUDA_CLASSE_HOSPITALAR',
       'IN_SEM_RECURSO', 'IN_BRAILLE', 'IN_AMPLIADA_24', 'IN_AMPLIADA_18',
       'IN_LEDOR', 'IN_ACESSO', 'IN_TRANSCRICAO', 'IN_LIBRAS',
       'IN_LEITURA_LABIAL', 'IN_MESA_CADEIRA_RODAS',
       'IN_MESA_CADEIRA_SEPARADA', 'IN_APOIO_PERNA', 'IN_GUIA_INTERPRETE',
       'IN_COMPUTADOR', 'IN_CADEIRA_ESPECIAL', 'IN_CADEIRA_CANHOTO',
       'IN_CADEIRA_ACOLCHOADA', 'IN_PROVA_DEITADO', 'IN_MOBILIARIO_OBESO',
       'IN_LAMINA_OVERLAY', 'IN_PROTETOR_AURICULAR', 'IN_MEDIDOR_GLICOSE',
       'IN_MAQUINA_BRAILE', 'IN_SOROBAN', 'IN_MARCA_PASSO', 'IN_SONDA',
       'IN_MEDICAMENTOS', 'IN_SALA_INDIVIDUAL', 'IN_SALA_ESPECIAL',
       'IN_SALA_ACOMPANHANTE', 'IN_MOBILIARIO_ESPECIFICO',
       'IN_MATERIAL_ESPECIFICO', 'IN_NOME_SOCIAL','NU_NOTA_REDACAO']
```

```python
#crio uma variavel com as colunas desejadas neste caso criei:colunas_selecionadas_d
#atribuo a variavel do data frame o comando filter
#chamo as novas colunas com a função items

d_selcionados=d.filter(items=colunas_selecionadas_d)
```
```python
d_selcionados.head()
```
```python
coluna_no_município_residencia = d_selcionados['NO_MUNICIPIO_ESC']
```
```python
#value_counts - conta cada item da coluna
#sort_index - ordem alfabetica
coluna_no_município_residencia.value_counts().sort_index()
```
```python
i = d_selcionados['NU_IDADE']
```
```python
i.value_counts().sort_index()
```
```python
i.hist(bins=30)
```
```python
#selecionando coluna
colunasSexo = d_selcionados['TP_SEXO']
```
```python
#contando so valores por categoria
tp_sexo = colunasSexo.value_counts()
```
```python
tp_sexo
```
```python
#calculando a porcentagem dos valores
porcentagemSexo = [100*x/tp_sexo.sum() for x in tp_sexo]
```
```python
porcentagemSexo
```
```python
#se eu quiser apenas os dados do sexo femino indico entre colchetes o index
somenteFeminino = tp_sexo[0]
```
```python
somenteFeminino
```
```python
colunasSexoNotaRedacao = d_selcionados.filter(items=['TP_SEXO','NU_NOTA_REDACAO'])
colunasSexoNotaRedacao
```
```python
#excluindo os dados NaN
#na mesma variavel uso .dropna()
colunasSexoNotaRedacao = colunasSexoNotaRedacao.dropna()
colunasSexoNotaRedacao.head()
```
```python
#o comando groupby agrupo por tipo da coluna selecionada
# comando count() conta tudo
colunasSexoNotaRedacao.groupby('TP_SEXO').count()
```
```python
#selecionando valor minimo
colunasSexoNotaRedacao.groupby('TP_SEXO').max()
```
```python
#buscando a menor nota de redação maior que zero
colunasSexoNotaRedacao[colunasSexoNotaRedacao.NU_NOTA_REDACAO > 0].groupby('TP_SEXO').min()
```
```python
#calculando a media
colunasSexoNotaRedacao.groupby('TP_SEXO').mean()
```
```python
#calculando a mediana
colunasSexoNotaRedacao.groupby('TP_SEXO').median()
```
```python
#O comando decribe já tras todas aas informações
#std é o disvio padrão
colunasSexoNotaRedacao.groupby('TP_SEXO').describe()
```
```python
#posso chamar nas variavael as colunas
colunasSelecionadas = ['NU_INSCRICAO','NU_NOTA_MAT','NU_NOTA_REDACAO','Q001','Q002']
```
```python
#Fazer uma variavel para as colunas desejadas
socialEnem = d.filter(items=colunasSelecionadas).dropna()
socialEnem
```
```python
#criando um dicionario 
Q001eQ002={
'A':'Nunca estudou',
'B':'Não completou a 4ª série/5º ano do Ensino Fundamental.',
'C':'Completou a 4ª série/5º ano, mas não completou a 8ª série/9º ano do Ensino Fundamental.',
'D':'Completou a 8ª série/9º ano do Ensino Fundamental, mas não completou o Ensino Médio.',
'E':'Completou o Ensino Médio, mas não completou a Faculdade.',
'F':'Completou a Faculdade, mas não completou a Pós-graduação.',
'G':'Completou a Pós-graduação.',
'H':'Não sei.'}
```
```python
#criando uma nova coluna para responder as questões Q001 e Q002
socialEnem['Resposta_Q001'] = [Q001eQ002[resp] for resp in socialEnem.Q001]
socialEnem['Resposta_Q002'] = [Q001eQ002[resp] for resp in socialEnem.Q002]
```
```python
socialEnem.head()
```
```python
#criando variavel para analisar reposta
#sort_values - ordena os valores, by é por onde,ascending=False quero do maior para o menor 
a = socialEnem.filter(items=['NU_INSCRICAO','Resposta_Q001']).groupby('Resposta_Q001').count().sort_values(by='NU_INSCRICAO',ascending=False)
a
```
```python
#grafico de linha
a.plot()
```
```python
#adicionando uma coluna existente
#mostrei na varialvel entre colchetes
#d é o dataframe original ponto o nome da coluna que quero
socialEnem['SG_UF_RESIDENCIA'] = d.SG_UF_RESIDENCIA
socialEnem.head()
```
```python
#analisando pelo estado do RJ
#as \ permitem que pule linha sem dar erro
#where = onde - a coluna residencia for igial a RJ
analiseMae = socialEnem.filter(items=['NU_INSCRICAO','Resposta_Q002'])\
             .where(socialEnem.SG_UF_RESIDENCIA == 'RJ')\
             .groupby('Resposta_Q002')\
             .count().sort_values(by='NU_INSCRICAO', ascending = False)
analiseMae
```
```python
#para arrumar as casas decimais que desejo
pd.set_option('display.precision',2)
```
```python
#fazendo a média das residencias
mediaMaesEstado = socialEnem.filter(items=['SG_UF_RESIDENCIA','NU_NOTA_REDACAO','Resposta_Q002'])\
                  .groupby(['SG_UF_RESIDENCIA','Resposta_Q002'])\
                  .mean()
mediaMaesEstado
```
```python
#fazendo grafico com matplotlib
fig, ax = plt.subplots(figsize=(35,35))
plt.suptitle('Nota Redação x Extado')

socialEnem.filter(items=['SG_UF_RESIDENCIA','Q002','NU_NOTA_REDACAO'])\
                  .where(socialEnem.Q002 != 'H')\
                  .groupby(['Q002','SG_UF_RESIDENCIA'])\
                  .mean().sort_values(by='NU_NOTA_REDACAO', ascending=False)\
                  .unstack().plot(ax=ax, )
```

