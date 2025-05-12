# Projeto 2

Criada em: 11 de abril de 2025 20:28
Dados - Hipóteses: Tecnologia

### **Projeto colaborativo:**

Taiza de Souza Santos Ferreira 

Kawana Souza Martins Santos

### Ferramentas utilizadas:

BigQuery, Power BI, Python.

### 

# Ficha Técnica Projeto 2 Hipóteses

### O objetivo principal desta análise é que a gravadora e o novo artista possam tomar decisões informadas que aumentem suas chances de alcançar o “sucesso”.

## **Caso**

Num mundo onde a **indústria musical** é extremamente competitiva e em constante evolução, a capacidade de tomar decisões baseadas em dados tornou-se um ativo inestimável.

Neste contexto, uma gravadora enfrenta o emocionante desafio de lançar um novo artista no cenário musical global. Felizmente, ela tem uma ferramenta poderosa em seu arsenal: um extenso conjunto de dados do Spotify com informações sobre as músicas mais ouvidas em 2023.

A gravadora levantou uma série de hipóteses sobre o que faz uma música ser mais ouvida. Essas hipóteses incluem:

- Músicas com BPM (Batidas Por Minuto) mais altos fazem mais sucesso em termos de número de streams no Spotify.
- As músicas mais populares no ranking do Spotify também possuem um comportamento semelhante em outras plataformas, como a Deezer.
- A presença de uma música em um maior número de playlists está correlacionada com um maior número de streams.
- Artistas com um maior número de músicas no Spotify têm mais streams.
- As características da música influenciam o sucesso em termos de número de streams no Spotify.

Você deve validar (refutar ou confirmar) essas hipóteses através da análise de dados e fornecer recomendações estratégicas com base em suas descobertas. O objetivo principal desta análise é que a gravadora e o novo artista possam tomar decisões informadas que aumentem suas chances de alcançar o “sucesso”.

## Passos a seguir:

# **🟦 5.1 Processar e preparar a base de dados**

### 🔵 Conectar/importar dados para outras ferramentas

Realizado download do dataset na página de insumos da laboratória, o arquivo estava compactado e eu descompactei da seguinte maneira: 

1- cliquei na opção dataset, abriu a página https://drive.google.com/file/d/11W1wfljCoRKy1Uk5R65LHWmh2mtCtMGV/view como arquivo compactado;

2- cliquei em “Abrir com o ZIP extractor”, selecionei as 3 pastas;

3- cliquei em “Extract to drive” e download das 3 pastas. 

Na BigQuery criei uma pasta chamada dados_projeto_hipoteses, importei as 3 planilhas que estavam salvas em CSV para uma subpasta chamada hipoteses com os seguintes passos:

1- Arquivo local

2- Local File

3- Arquivo local

4- Seleciono a pasta e a planilha

5- E preenchi os itens solicitados para criar a nova pasta no (origem, destino, título da planilha, local do conjunto de dados e esquema.

Ao fazer o upload da planila track_in_spotfay ohuve um erro ao exportar pois havia no titulo da coluna artisti(s)_name com o (S) com parênteses, foi retirado o parênteses no título e corrigido erro de upload no momento do upload coloquei V2 nas opções avançadas, mapa de caracteres no nome da coluna.

Deixei a pasta do meu projeto marcado com estrela para melhor visualização.

### Os dados que tenho:

- A tabela  `track_in_spotify` para medir popularidade;
- A tabela `track_in_competition` para medir alcance em outras plataformas;
- E a tabela `track_technical_info` para entender o perfil sonoro.

1. **Tabela: `hipoteses.track_in_spotify`**

**Descrição**: Essa tabela parece conter dados de **popularidade e presença de faixas no Spotify**.

**Principais colunas**:

- `track_id`: chave de identificação
- `track_name`: nome da música.
- `artists_name`: nome(s) do(s) artista(s).
- `artist_count`: quantidade de artistas na faixa.
- `released_year`, `released_month`, `released_day`: data de lançamento.
- `in_spotify_playlists`: número de playlists no Spotify onde a faixa está.
- `in_spotify_charts`: número de vezes em que apareceu nas paradas do Spotify.
- `streams`: número total de streams.

Tabela com **informações de engajamento e popularidade** no Spotify.

2. **Tabela: `hipoteses.track_in_competition`**

**Descrição**: Dados sobre a **presença de faixas em plataformas de competição**, como Apple Music, Deezer, Shazam etc.

**Principais colunas**:

- `track_id`: chave de identificação
- `in_apple_playlist`, `in_apple_charts`
- `in_deezer_playlist`, `in_deezer_charts`
- `in_shazam_charts`

Tabela mostra **onde a faixa aparece além do Spotify**, revelando a **abrangência cross-plataforma**.

3. **Tabela: `hipoteses.track_technical_info`**

**Descrição**: Informações **técnicas** da música, possivelmente extraídas de APIs como a do Spotify.

**Possíveis colunas** (baseado no nome da tabela e no padrão):

- `track_id`: chave de identificação.
- `bpm` (batidas por minuto)
- `key` (tom)
- `mode` (maior/menor)
- `valence_%`, `energy_%`, `danceability_%`, etc.: características sonoras em porcentagem.

Tabela com **atributos musicais/estruturais** de cada faixa — útil para análises de estilo ou emoção da música.

### 🔵 Identificar e tratar valores nulos

Nessa fase do projeto eu preciso encontrar os nulos das variáveis.

Para identificar os nulos segui os seguintes passos:

1- Cliquei na pasta do projeto, abri a subpasta hipoteses e cliquei nos 3 pontinhos na lateral direita da minha primeira pasta que é track_in_competition, selecionei a opção Query, entra diretamente na página de consulta com o código estruturado :

```sql
SELECT  FROM `projeto-2-hipoteses-456812.hipoteses.track_in_competition` LIMIT 1000
```

Retirei o **LIMIT 1000** que limitava apenas até 1000 a consulta dos dados e acrescentei * entre o SELECT e FROM.

```sql
SELECT * FROM `projeto-2-hipoteses-456812.hipoteses.track_in_competition` 
```

Cliquei na opção mais e formatar consulta para o código descer e ir para as próximas linhas, facilitando a escrita dos demais itens e organizando visualmente a codificação.

Utilizei os comandos Count, where e is null para buscar nas variáveis os nulos, ficando da seguinte maneira o código:

```sql
SELECT
  COUNT (*)
FROM
  `projeto-2-hipoteses-456812.hipoteses.track_in_competition`
  WHERE in_shazam_charts IS NULL
```

**SELECT *** (mostra ao código que quero selecionar tudo)

**COUNT** (mostra ao código que quero contar)

**FROM** (me mostra de onde eu quero tirar essa informação)

destrinchando esse trecho do código:
`projeto-2-hipoteses-456812.hipoteses.track_in_competition`

projeto-2-hipoteses-456812 (é a minha pasta do projeto)

hipoteses (é a minha subpasta)

track_in_competition (é a planilha onde será feita a busca)
**WHERE** in_shazam_charts IS NULL (onde eu quero procurar essa informação, no caso a informação foi procurada na coluna **in_shazam_charts.**

**IS NULL** (quero que me retorne o que é nulo)

Na tabela track_technical_Info há títulos de colunas com o final _% para esses casos eu utilizei a crase antes e após o título no código para conseguir fazer a consulta dos nulos, segue exemplo:

```sql
SELECT
count(*)
FROM
  `projeto-2-hipoteses-456812.hipoteses.track_technical_info `
  where 'speechiness_%' is null
```

- Na tabela track_in_competition na coluna in_chazam_charts foram encontrados 50 nulos, utilizando o código abaixo.

```sql
 SELECT *
FROM `projeto-2-hipoteses-456812.laboratoria_projeto_2_hipoteses.track_in_competition`
WHERE track_id IS NULL
   OR in_apple_charts IS NULL
   OR in_apple_playlists IS NULL
   OR in_deezer_charts IS NULL
   OR in_deezer_playlists IS NULL
   OR in_shazam_charts IS NULL

```

- Na tabela **track_technical_Info na coluna KEY** , onde foi encontrado 95 nulos, usei o código abaixo para identificar os nulos.

```sql
SELECT *
FROM `projeto-2-hipoteses-456812.laboratoria_projeto_2_hipoteses.track_technical_info` 
WHERE track_id IS NULL
   OR bpm IS NULL
   OR key IS NULL
   OR mode IS NULL
   OR `danceability_%` IS NULL
   OR `valence_%` IS NULL
   OR `energy_%` IS NULL
   OR `acousticness_%` IS NULL
   OR `instrumentalness_%` IS NULL
   OR `liveness_%` IS NULL
   OR `speechiness_%` IS NULL;
```

- Não foi identificado valores nulos na tabela track_in_spotify.

```sql
SELECT *
FROM `projeto-2-hipoteses-456812.laboratoria_projeto_2_hipoteses.track_in_spotify`
WHERE track_id IS NULL
OR track_name IS NULL
OR artist_s__name IS NULL
OR artist_count IS NULL
OR released_day IS NULL
OR released_month IS NULL
OR released_year IS NULL
OR in_spotify_playlists IS NULL
OR in_spotify_charts IS NULL
OR streams IS NULL
```

### 🔵 Identificar e tratar valores duplicados

Nessa fase do projeto eu preciso encontrar os valores duplicados das variáveis.

Para identificar os duplicados segui os seguintes passos:

Cliquei na pasta do projeto, abri a subpasta hipóteses e cliquei nos 3 pontinhos na lateral direita da minha primeira pasta que é track_in_competition, selecionei a opção Query, entra diretamente na página de consulta.

Com o código abaixo fiz a consulta dos valores duplicados na tabela onde não foi encontrado nenhum valor duplicado nesta tabela, fiz também a consulta individualmente ex: track_id e in_apple_playlists, track_id e in_apple_charts…, fiz  a  para ver se batia com a consulta conjunta.

```sql
SELECT
track_id,
in_apple_playlists,
in_apple_charts,
in_deezer_charts,
in_deezer_playlists,
in_shazam_charts,
COUNT(*) AS quantidade
FROM
  `projeto-2-hipoteses-456812.laboratoria_projeto_2_hipoteses.track_in_competition`
  group by track_id, in_apple_playlists, in_apple_charts, in_deezer_charts, in_deezer_playlists, in_shazam_charts
  HAVING COUNT(*) > 1
```

**SELECT *** (mostra ao código que quero selecionar tudo)

**COUNT** (mostra ao código que quero contar)

**AS** (colocar o título na pesquisa) **ex COUNT (*) AS** quantidade

**FROM** (me mostra de onde eu quero tirar essa informação)

**GROUP BY** (qual grupo vou consultar)

**HAVING** (filtra os dados com base em critérios definidos) **ex: HAVING COUNT(*) > 1,** quer ver os duplicados.

Na tabela track_in_spotify, utilizei a consulta com todas as colunas juntas com o código abaixo, porém aparece que não há dados, mesmo assim preferi consultar as colunas individualmente.

```sql

SELECT
  track_id,
  track_name,
  artist_s__name,
  artist_count,
  released_day,
  released_month,
  released_year,
  in_spotify_charts,
  in_spotify_playlists,
  streams,
  COUNT(*) AS quantidade 
FROM
  `projeto-2-hipoteses-456812.laboratoria_projeto_2_hipoteses.track_in_spotify`
GROUP BY
  track_id,
  track_name,
  artist_s__name,
  artist_count,
  released_day,
  released_month,
  released_year,
  in_spotify_charts,
  in_spotify_playlists,
  streams
HAVING COUNT(*) > 1;
```

![image.png](image.png)

Na consulta individual encontrei 4 valores duplicados nas colunas track_name e artisti_s_name na tabela track_in_spotify.

```sql
SELECT 
track_name,
artist_s__name,
COUNT(*) AS quantidade
   FROM `projeto-2-hipoteses-456812.laboratoria_projeto_2_hipoteses.track_in_spotify`
   GROUP BY track_name, artist_s__name
   HAVING COUNT(*) > 1

```

Na tabela track_in_spotify foi encontrado 4 valores duplicados quando consultado as colunas **track_name e artist_s_name.**

![image.png](image%201.png)

- Utilizei o código abaixo para listar os duplicados para conseguir analisar os dados

```sql
-- 1. Cria uma lista com os track_name + artists_name duplicados
WITH duplicadas AS (
  SELECT 
    track_name,
    artist_s__name
  FROM `projeto-2-hipoteses-456812.laboratoria_projeto_2_hipoteses.track_in_spotify`
  GROUP BY track_name, artist_s__name
  HAVING COUNT(*) > 1
)

-- 2. Busca todos os dados associados às combinações duplicadas
SELECT *
FROM `projeto-2-hipoteses-456812.laboratoria_projeto_2_hipoteses.track_in_spotify` AS t
JOIN duplicadas AS d
ON t.track_name = d.track_name
AND t.artist_s__name = d.artist_s__name
```

Resultado:

![image.png](image%202.png)

- Realizei a consulta individual em todas as colunas para verificar que haviam mais valores duplicados.

```sql
SELECT 
track_id,
COUNT(*) AS quantidade
   FROM `projeto-2-hipoteses-456812.hipoteses.track_in_spotify` 
   GROUP BY track_id
   HAVING COUNT(*) > 1
```

Na tabela track_technical_info, não foi identificado valores duplicados, utilizei o código abaixo.

```sql
SELECT
  track_id,
  bpm,
  key,
  mode,
  `danceability_%`,
  `valence_%`,
  `acousticness_%`,
  `energy_%`,
  `instrumentalness_%`,
  `liveness_%`,
  `speechiness_%`,
  COUNT(*) AS quantidade 
FROM
  `projeto-2-hipoteses-456812.laboratoria_projeto_2_hipoteses.track_technical_info`
GROUP BY
  track_id,
  bpm,
  key,
  mode,
  `danceability_%`,
  `valence_%`,
  `acousticness_%`,
  `energy_%`,
  `instrumentalness_%`,
  `liveness_%`,
  `speechiness_%`
HAVING
  COUNT(*) > 1;
```

![image.png](image.png)

### 🔵 Identificar e tratar dados fora do escopo de análise

Nesta fase do projeto eu preciso manipular variáveis que não são úteis para análise.

Na tabela track_in_competition encontrei 50 valores nulos na coluna in_shazam_charts .

Optei por tratar os valores como integer e coloquei -1 , então quando tem o -1 quer dizer que não tem informação, solicitei que o nome da coluna retornasse como in_shazam_chats_tratado. Utilizei o IFNULL.

Salvei como VIEW com o título track_in_competition1.

```sql
SELECT
  track_id,
  in_apple_playlists,
  in_apple_charts,
  in_deezer_playlists,
  in_deezer_charts,
  IFNULL(in_shazam_charts, -1) AS in_shazam_charts_tratado
FROM
  `projeto-2-hipoteses-456812.laboratoria_projeto_2_hipoteses.track_in_competition`

```

Exemplo:

![image.png](image%203.png)

Na tabela track_technical_Info encontrei 95 valores nulos na coluna key.

- Optei por tratar os nulos criando uma string de **`sem informação´** solicitei que a coluna retornasse tratada e com o título key tratada. Utilizei o IFNULL.
- Salvei a tabela tratada com o título track_technical_Info_tratado.

```sql
SELECT
  track_id,
  bpm,
  IFNULL(key, "ND") AS key_tratada,
  mode,
  `danceability_%`, 
  `valence_%`, 
  `energy_%`, 
  `acousticness_%`, 
  `instrumentalness_%`, 
  `liveness_%`, 
  `speechiness_%`
FROM
  `projeto-2-hipoteses-456812.laboratoria_projeto_2_hipoteses.track_technical_info`
```

Exemplo:

![image.png](image%204.png)

- Ainda na tabela track_technical_info realizei a exclusão da coluna key pois havia muitos valores nulos, prejudicando a análise.

---

```sql
SELECT *
EXCEPT (key_tratada)
FROM
  `projeto-2-hipoteses-456812.hipoteses.track_techinical_info_tratada`
```

- Optei por realizar a criação de View com o titulo VW_track_technical_info.

Na tabela track_in_spotyfy encontrei 4 valores duplicados, ao analisar os 8 listados foi observado que havia demais valores diferentes, mas 2 deles havia o número de streams igual.

Exemplo:

![image.png](image%201.png)

![image.png](image%202.png)

- Opto por excluir os 4 valores duplicados porém pedi para o código abaixo excluir os que constavam valor menor de dados, de 953 listados, excluídos 4 restando 949.
- Salvei a tabela como track_in_spotify_tratado.

```sql
WITH ranked_tracks AS (
  SELECT
    track_id,
    track_name,
    artist_s__name,
    artist_count,
    released_year,
    released_day,
    released_month,
    in_spotify_playlists,
    in_spotify_charts,
    streams,
    ROW_NUMBER() OVER (
      PARTITION BY track_name, artist_s__name
      ORDER BY streams DESC, in_spotify_playlists DESC
    ) AS rank_streams
  FROM `projeto-2-hipoteses-456812.laboratoria_projeto_2_hipoteses.track_in_spotify`
)

SELECT
  track_id,
  track_name,
  artist_s__name,
  artist_count,
  released_year,
  released_day,
  released_month,
  in_spotify_playlists,
  in_spotify_charts,
  streams
FROM ranked_tracks
WHERE rank_streams = 1

 
```

- Optei por realizar a criação de View com o título VW_track_in_spotify_limpa.

Após excluir os 4 valores duplicados da tabela track_in_spotify, utilizei o código abaixo para fazer o filtro para identificar nas outras duas tabelas os ids que também teriam que ser excluidos.

```sql
SELECT DISTINCT track_id_corrigido, origem
FROM (
  SELECT t1.track_id_corrigido, "Só no Spotify" AS origem
  FROM `projeto-2-hipoteses-456812.laboratoria_projeto_2_hipoteses.track_in_spotify5` AS t1
  WHERE t1.track_id_corrigido NOT IN (
    SELECT track_id_corrigido FROM `projeto-2-hipoteses-456812.laboratoria_projeto_2_hipoteses.track_in_competition2`
  )
  OR t1.track_id_corrigido NOT IN (
    SELECT track_id_corrigido FROM `projeto-2-hipoteses-456812.laboratoria_projeto_2_hipoteses.track_technical_info4`
  )

  UNION ALL

  SELECT t2.track_id_corrigido, "Só no Competition" AS origem
  FROM `projeto-2-hipoteses-456812.laboratoria_projeto_2_hipoteses.track_in_competition2` AS t2
  WHERE t2.track_id_corrigido NOT IN (
    SELECT track_id_corrigido FROM `projeto-2-hipoteses-456812.laboratoria_projeto_2_hipoteses.track_in_spotify5`
  )
  OR t2.track_id_corrigido NOT IN (
    SELECT track_id_corrigido FROM `projeto-2-hipoteses-456812.laboratoria_projeto_2_hipoteses.track_technical_info4`
  )

  UNION ALL

  SELECT t3.track_id_corrigido, "Só no Technical" AS origem
  FROM `projeto-2-hipoteses-456812.laboratoria_projeto_2_hipoteses.track_technical_info4` AS t3
  WHERE t3.track_id_corrigido NOT IN (
    SELECT track_id_corrigido FROM `projeto-2-hipoteses-456812.laboratoria_projeto_2_hipoteses.track_in_spotify5`
  )
  OR t3.track_id_corrigido NOT IN (
    SELECT track_id_corrigido FROM `projeto-2-hipoteses-456812.laboratoria_projeto_2_hipoteses.track_in_competition2`
  )
)
ORDER BY track_id_corrigido;

```

Identificado os 4 track_id que só estavam na tabela competition e technical.

![image.png](image%205.png)

Com o código abaixo eu consegui após o filtro eu exclui os track_id que foram excluídos na tabela track_in_spotify.

```sql
SELECT *
FROM `projeto-2-hipoteses-456812.laboratoria_projeto_2_hipoteses.track_technical_info4`
WHERE CAST(track_id_corrigido AS INT64) NOT IN (
  1119309, 3814670, 5080031, 8173823
);

```

```sql
SELECT *
FROM `projeto-2-hipoteses-456812.laboratoria_projeto_2_hipoteses.track_in_competition2`
WHERE CAST(track_id_corrigido AS INT64) NOT IN (
  1119309, 3814670, 5080031, 8173823
);
 
```

Após rodar o código as 3 planilhas ficaram com o mesmo número de track_ids.

![image.png](image%206.png)

### 🔵 Identificar e tratar dados discrepantes em variáveis categóricas

Nesta fase do projeto eu preciso manipular as variáveis categóricas para análise.

Utilizado código abaixo para filtrar os arquivos com nomes corrompidos para conseguir ajustá-los, são 109 nomes nas colunas track_name e artist_s__name.

```sql
SELECT *
FROM `projeto-2-hipoteses-456812.laboratoria_projeto_2_hipoteses.track_in_spotify2`
WHERE REGEXP_CONTAINS(track_name, r'�')
   OR REGEXP_CONTAINS(artist_s__name, r'�')
```

![image.png](image%207.png)

Utilizado o código abaixo para remoção de caracteres especiais da string com o REGEXP, alterei os caracteres especiais no meio do texto por  vazio e com o INITCAP todas as letras no inicio das palavras por maiúsculo, para facilitar posteriormente uma consulta.

Com a alteração foi preciso criar 2 novas variáveis **artist_s__name_formatado** e **track_name_formatado.**

```sql
   SELECT
*,
  INITCAP(REGEXP_REPLACE(artist_s__name, r'[^\x00-\x7F]', '')) AS artist_s__name_limpo,
  INITCAP(REGEXP_REPLACE(track_name, r'[^\x00-\x7F]', '')) AS track_name_limpo
FROM
  `projeto-2-hipoteses-456812.laboratoria_projeto_2_hipoteses.track_in_spotify2`

```

- Novas variáveis com nomes maiúsculos + remoção de caracteres especiais da string

![image.png](image%208.png)

### 🔵 Identificar e tratar dados discrepantes em variáveis numéricas

Nesta fase do projeto eu preciso identificar valores discrepantes em variáveis numéricas.

Feito a consulta na tabela VW_track_in_spotify_limpa na coluna streams, utilizando MAX, MIN, AVG, não foi possível usar o avg que é a média pois estava com erro na síntese do código por haver letras na variável numérica, utilizei somente o max e min e identifiquei que o mínimo estava constando letras ao invés de números.

Consulta com MAX, MIN e AVG com o código, por haver variável de texto ocorreu erro na consulta da média (AVG):

```sql
SELECT
MAX(streams),
MIN(streams),
FROM  `projeto-2-hipoteses-456812.laboratoria_projeto_2_hipoteses.track_in_spotify2`
```

![image.png](image%209.png)

Consulta apenas com streams MAX e MIN 1 discrepância:

                   

![image.png](image%2010.png)

track_id 1 discrepância

```sql
SELECT
MAX(track_id) AS MAX,
MIN(track_id) AS MIN,
FROM  `projeto-2-hipoteses-456812.hipoteses.VW_track_in_spotify_limpa`

```

![image.png](image%2011.png)

Feito consulta nas demais colunas e não foi encontrado discrepâncias numéricas:

artist_count 0 discrepancias

```sql
SELECT
MAX(artist_count) AS MAX,
MIN(artist_count) AS MIN,
AVG (artist_count) AS MED
FROM  `projeto-2-hipoteses-456812.hipoteses.VW_track_in_spotify_limpa`
```

![image.png](image%2012.png)

released_year 0 discrepancias

```sql
SELECT
MAX(released_year) AS MAX,
MIN (released_year) AS MIN,
AVG (released_year) AS MED
FROM  `projeto-2-hipoteses-456812.hipoteses.VW_track_in_spotify_limpa`
```

![image.png](image%2013.png)

released_month 0 discrepâncias

```sql
SELECT
MAX(released_month) AS MAX,
MIN(released_month) AS MIN,
AVG(released_month) AS MED
FROM  `projeto-2-hipoteses-456812.hipoteses.VW_track_in_spotify_limpa`
```

![image.png](image%2014.png)

released_day 0 discrepâncias

```sql
SELECT
MAX(released_day) AS MAX,
MIN(released_day) AS MIN,
AVG (released_day) AS MED
FROM  `projeto-2-hipoteses-456812.hipoteses.VW_track_in_spotify_limpa`
```

![image.png](image%2015.png)

in_spotify_playlist  0

```sql
SELECT
MAX(in_spotify_playlists) AS MAX,
MIN(in_spotify_playlists) AS MIN,
AVG (in_spotify_playlists) AS MED
FROM  `projeto-2-hipoteses-456812.hipoteses.VW_track_in_spotify_limpa`
```

![image.png](image%2016.png)

in_spitify_charts 0

```sql
SELECT
MAX(in_spotify_charts) AS MAX,
MIN(in_spotify_charts) AS MIN,
AVG (in_spotify_charts) AS MED
FROM  `projeto-2-hipoteses-456812.hipoteses.VW_track_in_spotify_limpa
```

![image.png](image%2017.png)

Na tabela track_incompetition_limpa foi consultado e encontrado os seguintes dados:

A mesma discrepância na coluna track_id

```sql
SELECT
MAX(track_id) AS MAX,
MIN(track_id) AS MIN,
FROM  `projeto-2-hipoteses-456812.hipoteses.VW_track_in_competition_limpa`

```

![image.png](image%2018.png)

in_apple_charts 0

```sql
SELECT
MAX(in_apple_charts) AS MAX,
MIN(in_apple_charts) AS MIN,
AVG(in_apple_charts) AS MED
FROM  `projeto-2-hipoteses-456812.hipoteses.VW_track_in_competition_limpa`
```

![image.png](image%2019.png)

in_apple_playlists 0

```sql
SELECT
MAX(in_apple_playlists) AS MAX,
MIN(in_apple_playlists) AS MIN,
AVG(in_apple_playlists) AS MED
FROM  `projeto-2-hipoteses-456812.hipoteses.VW_track_in_competition_limpa`

```

![image.png](image%2020.png)

in_deezer_charts 0

```sql
SELECT
MAX(in_deezer_charts) AS MAX,
MIN(in_deezer_charts) AS MIN,
AVG(in_deezer_charts) AS MED
FROM  `projeto-2-hipoteses-456812.hipoteses.VW_track_in_competition_limpa`
```

![image.png](image%2021.png)

in_deezer_playlists 0

```sql
SELECT
MAX(in_deezer_playlists) AS MAX,
MIN(in_deezer_playlists) AS MIN,
AVG(in_deezer_playlists) AS MED
FROM  `projeto-2-hipoteses-456812.hipoteses.VW_track_in_competition_limpa`

```

![image.png](image%2022.png)

in_shazam_charts_tratado 0

```sql
SELECT
MAX(in_shazam_charts_tratado) AS MAX,
MIN(in_shazam_charts_tratado) AS MIN,
AVG(in_shazam_charts_tratado) AS MED
FROM  `projeto-2-hipoteses-456812.hipoteses.VW_track_in_competition_limpa`

```

![image.png](image%2023.png)

Na tabela track_technical_info_limpa encontrei as seguintes discrepâncias:

Na coluna track_id, assim como nas demais tabelas

```sql
SELECT
MAX(track_id) AS MAX,
MIN(track_id) AS MIN,
FROM  `projeto-2-hipoteses-456812.hipoteses.VW_track_technical_info_limpa`
```

![image.png](image%2024.png)

BPM 0

```sql
SELECT
MAX(bpm) AS MAX,
MIN(bpm) AS MIN,
AVG(bpm) AS MED
FROM  `projeto-2-hipoteses-456812.hipoteses.VW_track_technical_info_limpa`

```

![image.png](image%2025.png)

danceability_% 0

```sql
SELECT
  MAX(`danceability_%`) AS MAX,
  MIN(`danceability_%`) AS MIN,
  AVG(`danceability_%`) AS MED
FROM
  `projeto-2-hipoteses-456812.hipoteses.VW_track_technical_info_limpa`

```

![image.png](image%2026.png)

valence_% 0

```sql
SELECT
  MAX(`valence_%`) AS MAX,
  MIN(`valence_%`) AS MIN,
  AVG(`valence_%`) AS MED
FROM
  `projeto-2-hipoteses-456812.hipoteses.VW_track_technical_info_limpa`

```

![image.png](image%2027.png)

energy_% 0

```sql
SELECT
  MAX(`energy_%`) AS MAX,
  MIN(`energy_%`) AS MIN,
  AVG(`energy_%`) AS MED
FROM
  `projeto-2-hipoteses-456812.hipoteses.VW_track_technical_info_limpa`

```

![image.png](image%2028.png)

acousticness_%

```sql
SELECT
  MAX(`acousticness_%`) AS MAX,
  MIN(`acousticness_%`) AS MIN,
  AVG(`acousticness_%`) AS MED
FROM
  `projeto-2-hipoteses-456812.hipoteses.VW_track_technical_info_limpa`

```

![image.png](image%2029.png)

instrumentalness_% 0

```sql
SELECT
  MAX(`instrumentalness_%`) AS MAX,
  MIN(`instrumentalness_%`) AS MIN,
  AVG(`instrumentalness_%`) AS MED
FROM
  `projeto-2-hipoteses-456812.hipoteses.VW_track_technical_info_limpa`

```

![image.png](image%2030.png)

liveness_% 0

```sql
SELECT
  MAX(`liveness_%`) AS MAX,
  MIN(`liveness_%`) AS MIN,
  AVG(`liveness_%`) AS MED
FROM
  `projeto-2-hipoteses-456812.hipoteses.VW_track_technical_info_limpa`

```

![image.png](image%2031.png)

speechiness_% 0

```sql
SELECT
  MAX(`speechiness_%`) AS MAX,
  MIN(`speechiness_%`) AS MIN,
  AVG(`speechiness_%`) AS MED
FROM
  `projeto-2-hipoteses-456812.hipoteses.VW_track_technical_info_limpa`
```

![image.png](image%2032.png)

Após a busca nas 3 tabelas pude evidenciar que os valores numéricos discrepantes são:

- track_ id nas 3 tabelas com **valor 0:00**, o dado aparece em FLOAT e não INTEGER.
- streams na tabela track_in_spotify_limpa com valor **BPM110KeyAModeMajorDanceability53Valence75Energy69Acousticness7Instrumentalness0Liveness17Speechiness3**, o dado aparece em STRING e não INTEGER.

Dados tratados na fase de **verificar e alterar tipo de dados,** agora consigo realizar o restante das consultas que não consegui no track_id e streams.

tabela VW_track_tecnhical_info_limpa

```sql
SELECT
  MAX(CAST(track_id_corrigido AS INT64)) AS MAX,
  MIN(CAST(track_id_corrigido AS INT64)) AS MIN,
  AVG(CAST(track_id_corrigido AS INT64)) AS MED
FROM
  `projeto-2-hipoteses-456812.hipoteses.VW_track_technical_info_limpa`

```

![image.png](image%2033.png)

Tabela VW_track_in_competition3

```sql
SELECT
  MAX(CAST(track_id_corrigido AS INT64)) AS MAX,
  MIN(CAST(track_id_corrigido AS INT64)) AS MIN,
  AVG(CAST(track_id_corrigido AS INT64)) AS MED
FROM
  `projeto-2-hipoteses-456812.laboratoria_projeto_2_hipoteses.track_in_competition3`
```

![image.png](image%2034.png)

Tabela VW_track_in_spotify5

```sql
SELECT
  MAX(CAST(track_id_corrigido AS INT64)) AS MAX,
  MIN(CAST(track_id_corrigido AS INT64)) AS MIN,
  AVG(CAST(track_id_corrigido AS INT64)) AS MED
FROM
  `projeto-2-hipoteses-456812.laboratoria_projeto_2_hipoteses.track_in_spotify5` 
```

![image.png](image%2035.png)

### 🔵 Verificar e alterar o tipo de dados

Nesta fase do projeto eu preciso identificar dados que precisam ser alterados, que podem impactar em consultas.

Observando o esquema da tabela track_in_spotify_limpa, foi notado que na opção esquema a coluna streams está como STRING e não INTEGER.

 O erro de tipo de dado ocorre apenas nessa tabela. Segue exemplo abaixo:

![image.png](image%2036.png)

Decidi alterar o tipo de dado de STRING para INTEGER com o código abaixo

```sql
SELECT 
SAFE_CAST(streams AS INT64) AS streams_limpo
FROM
  `projeto-2-hipoteses-456812.hipoteses.VW_track_in_spotify_limpa`

```

O que eu quero nessa query:

SAFE_CAST na coluna streams alterará o STRING por INTEGER, e de devolverá com o título streams_limpa, o SAFE retorna nulos os dados que ele não consegue corrigir.

![image.png](image%2037.png)

Transformei 1 valor que retornou nulo em -1 por se tratar de um INTEGER.

```sql
SELECT 
  t.* EXCEPT (streams),
  IFNULL(SAFE_CAST(t.streams AS INT64), -1) AS streams_limpo
FROM
  `projeto-2-hipoteses-456812.hipoteses.track_in_spotify_limpa` AS t

```

![image.png](image%2038.png)

Na coluna track_id encontrei nas 3 tabelas o resultado 0:00 com o código abaixo eu alterei para 000000 com sete vezes o número 0 porque é o padrão numérico do track_id.

```sql
  SELECT 
  *, 
  REPLACE(track_id, '0:00', '0000000') AS track_id_corrigido
FROM 
  `projeto-2-hipoteses-456812.hipoteses.VW_track_in_spotify_limpa`
```

Ele me retornou com uma nova variável chamada track_id_corrigido e com o valor alterado nas 3 tabelas.

Exemplo:

![image.png](image%2039.png)

Utilizando o código abaixo, exclui a coluna track_id e mantive a track_id_corrigido

```sql
SELECT
  REPLACE(track_id, '0:00', '0000000') AS track_id_corrigido,
  track_name,
  artist_s__name_formatado,
  streams,
  artist_count,
  released_year,
  released_day,
  released_month,
  in_spotify_charts,
  in_spotify_playlists
FROM
  `projeto-2-hipoteses-456812.hipoteses.VW_track_in_spotify_limpa`

```

Após utilizar o código acima minha tabela começou a dar erro de excesso de visualização conforme print abaixo, consultei diversos meios incluindo o chat GPT que me deram a devolutiva e em instantes retornaria ao normal e conseguiria prosseguir com os trabalhos de atualização da tabela porém sem sucesso:

![image.png](image%2040.png)

Optei por salvar uma nova tabela e realizar todo o processo nela até chegar aqui, como já estava mais familiarizada com os caminhos, acabei alterando por algumas vezes os códigos e simplificando os caminhos, ao chegar nessa fase na nova tabela, precisei apenas alterar a ordem das colunas pois fui tratando tudo de uma vez, segue código abaixo de tratamento de ordem das colunas:

```sql
SELECT
track_id_corrigido,
  track_name_formatado,
  artist_s__name_formatado,
  streams_limpo,
  artist_count,
  released_year,
  released_day,
  released_month,
  in_spotify_charts,
  in_spotify_playlists
FROM
  `projeto-2-hipoteses-456812.hipoteses.track_in_spotify_limpa3` 
```

Streams corrigido com -1

![image.png](image%2041.png)

Track_id_corrigido com 0000000

![image.png](image%2042.png)

Utilizado o código abaixo na tabela VW_track_in_competition_limpa para a correção da variável numérica track_id de 0:00 por 0000000, e substituído a coluna track_id por track_id_corrigido, salvo view.

```sql
SELECT
  REPLACE(track_id, '0:00', '0000000') AS track_id_corrigido,
  in_apple_playlists,
  in_apple_charts,
  in_deezer_charts,
  in_deezer_playlists,
  in_shazam_charts_tratado
FROM
  `projeto-2-hipoteses-456812.hipoteses.VW_track_in_competition_limpa` 
```

![image.png](image%2043.png)

Utilizado o código abaixo na tabela VW_track_technical_info_limpa para a correção da variável numérica track_id de 0:00 por 0000000, e substituído a coluna track_id por track_id_corrigido, salvo view.

```sql
SELECT
  REPLACE(track_id, '0:00', '0000000') AS track_id_corrigido,
  bpm,
  mode,
  `danceability_%`,
  `valence_%`,
  `energy_%`,
  `acousticness_%`,
  `instrumentalness_%`,
  `liveness_%`,
  `speechiness_%`
FROM
   `projeto-2-hipoteses-456812.hipoteses.track_techinical_info_tratada`
```

![image.png](image%2044.png)

### 🔵 Criar novas variáveis

Nesta etapa tenho que criar uma de data de lançamento e uma variável de participação total nas playlists.

Com esse código eu concatenei utilizando o CONCAT as colunas de dia, mês e ano, tendo como resultado uma nova variável chamada **data_lancamento,** utilizei o DATE para que ele me retornasse como ano/mês/dia.

```sql
SELECT
  track_id_corrigido,
  track_name_limpo,
  artist_s__name_limpo,
  streams_limpo,
  artist_count,
  in_spotify_charts,
  in_spotify_playlists,
  DATE(CONCAT(
    CAST(released_year AS STRING), '-',
    LPAD(CAST(released_month AS STRING), 2, '0'), '-',
    LPAD(CAST(released_day AS STRING), 2, '0')
  )) AS data_lancamento
FROM
 `projeto-2-hipoteses-456812.laboratoria_projeto_2_hipoteses.track_in_spotify5` 
```

![image.png](image%2045.png)

Com o código abaixo eu fiz um Join entre as tabelas track_in_competition3 e track_in_spotify6, criando a nova variável **participacao_total_nas_playlists**, com as colunas de playlists, trazendo também as demais colunas da tabela competition para a spotify.

```sql
SELECT
  t1.track_id_corrigido,
  t1.track_name_limpo,
  t1.artist_s__name_limpo,
  t1.streams_limpo,
  t1.artist_count,
  t2.in_apple_charts,
  t2.in_deezer_charts,
  t1.in_spotify_charts,
  t1.in_spotify_playlists,
  t1.data_lancamento,
  (
    CASE WHEN t1.in_spotify_playlists = 1 THEN 1 ELSE 0 END +
    CASE WHEN t2.in_apple_playlists > 0 THEN 1 ELSE 0 END +
    CASE WHEN t2.in_deezer_playlists > 0 THEN 1 ELSE 0 END
  ) AS participacao_total_nas_playlists
FROM
  `projeto-2-hipoteses-456812.laboratoria_projeto_2_hipoteses.track_in_spotify6` AS t1
JOIN
  `projeto-2-hipoteses-456812.laboratoria_projeto_2_hipoteses.track_in_competition3` AS t2
ON
  t1.track_id_corrigido = t2.track_id_corrigido;

```

![image.png](image%2046.png)

A variável **participacao_total_nas_playlists** mostra o nível de visibilidade de uma música nas plataformas de streaming:

- **0**: Não está em nenhuma playlist (sem visibilidade).
- **1**: Está em uma plataforma de playlist (baixa visibilidade).
- **2**: Está em duas plataformas de playlists (média visibilidade).
- **3**: Está nas três plataformas de playlists (alta visibilidade).

Ela soma **+1** para cada plataforma onde a música aparece em playlists (Spotify, Apple Music e Deezer).

### 🔵 Unir tabelas

Nesta etapa eu tenho que unir as tabelas utilizando o LEFT JOIN.

Criei um View para cada tabela antes da unificação:

- **projeto-2-hipoteses-456812.laboratoria_projeto_2_hipoteses.VW_track_in_spotify1**
- **projeto-2-hipoteses-456812.laboratoria_projeto_2_hipoteses.VW_track_in_competition**
- **projeto-2-hipoteses-456812.laboratoria_projeto_2_hipoteses.VW_track_technical_info**

Utilizado código abaixo para unificação das tabelas com o LEFT JOIN e link dos Views.

```sql
CREATE TABLE `projeto-2-hipoteses-456812.laboratoria_projeto_2_hipoteses.nova_tabela_unificada` AS
SELECT
  t1.*,                              -- Tudo da tabela principal (Spotify)
  t2.* EXCEPT(track_id_corrigido, in_apple_charts, in_deezer_charts),   -- Tudo da segunda, exceto o ID
  t3.* EXCEPT(track_id_corrigido)    -- Tudo da terceira, exceto o ID
FROM `projeto-2-hipoteses-456812.laboratoria_projeto_2_hipoteses.VW_track_in_spotify1` AS t1
LEFT JOIN `projeto-2-hipoteses-456812.laboratoria_projeto_2_hipoteses.VW_track_in_competition` AS t2
  ON t1.track_id_corrigido = t2.track_id_corrigido
LEFT JOIN `projeto-2-hipoteses-456812.laboratoria_projeto_2_hipoteses.VW_track_technical_info` AS t3
  ON t1.track_id_corrigido = t3.track_id_corrigido;
```

Após a união da tabela criei uma nova variável chamada total_playlists com o código abaixo. Pois anteriormente havia criado a variável participacao_total_nas_playlists que mostra o nível de visibilidade de uma música nas plataformas de streaming, dado que não estava utilizando.

```sql
CREATE OR REPLACE TABLE `projeto-2-hipoteses-456812.laboratoria_projeto_2_hipoteses.nova_tabela_unificada1` AS
SELECT
  *,
  (
    COALESCE(in_spotify_playlists, 0) +
    COALESCE(in_apple_playlists, 0) +
    COALESCE(in_deezer_playlists, 0)
  ) AS total_playlists
FROM
  `projeto-2-hipoteses-456812.laboratoria_projeto_2_hipoteses.nova_tabela_unificada1`

```

### 🔵 Construir tabelas auxiliares

Nesta etapa eu tenho que criar tabela auxiliar utilizando o comando WITH para calcular o total de músicas por artista solo.

Com o código abaixo criei uma tabela temporária utilizando o **WITH** me trazendo o total_de_música_por_artista_solo.

Salvo como consulta e tabela com o título **tabela_temporaria_total_artista_solo.** Essa nova variável me traz os artistas solo, traz da coluna artist_count =1.

Percebi que com o WITH o código fica mais limpo e organizado.

```sql
WITH total_por_artista_solo AS (
  SELECT
    artist_s__name_limpo,
    COUNT(*) AS total_musicas_por_artista
  FROM `projeto-2-hipoteses-456812.laboratoria_projeto_2_hipoteses.tabela_unificada`
  WHERE artist_count = 1
  GROUP BY artist_s__name_limpo
)

SELECT
  t.*,
  total_por_artista_solo.total_musicas_por_artista
FROM `projeto-2-hipoteses-456812.laboratoria_projeto_2_hipoteses.tabela_unificada` AS t
LEFT JOIN total_por_artista_solo
  ON t.artist_s__name_limpo = total_por_artista_solo.artist_s__name_limpo;
```

Com o código abaixo criei uma tabela auxiliar, com a variável, **total_musica_artista_solo**, tabela salva como **tabela_auxiliar_total_musica_artista_solo.** Essa nova variável me traz os artistas solo, traz da coluna artist_count =1.

```sql
CREATE OR REPLACE TABLE `projeto-2-hipoteses-456812.laboratoria_projeto_2_hipoteses.tabela_auxiliar_com_total_musica_artista_solo` AS

WITH total_por_artista_solo AS (
  SELECT
    artist_s__name_limpo,
    COUNT(*) AS total_musicas_por_artista
  FROM `projeto-2-hipoteses-456812.laboratoria_projeto_2_hipoteses.tabela_unificada`
  WHERE artist_count = 1
  GROUP BY artist_s__name_limpo
)

SELECT
  t.*,
  total_por_artista_solo.total_musicas_por_artista
FROM `projeto-2-hipoteses-456812.laboratoria_projeto_2_hipoteses.tabela_unificada` AS t
LEFT JOIN total_por_artista_solo
  ON t.artist_s__name_limpo = total_por_artista_solo.artist_s__name_limpo;
```

# **🟪 5.2 Fazer uma análise exploratória**

### 🟣 Agrupar dados de acordo com variáveis categóricas

Nesta etapa eu tenho que agrupar variáveis categóricas em tabelas no Power BI, realizei o download do Power BI no computador.

Importei para dentro do Power BI a minha tabela unificada com os seguintes passos:

- Cliquei em obter dados;
- Selecionei a opção banco de dados;
- Selecionei Google BigQuery;
- Selecionei o modo exibição tabela no BigQuery para ver a tabela;
- Selecionei a tabela no check ao lado e exportei.

![image.png](image%2047.png)

Foi criado e agrupado 2 variáveis categóricas, música_por_artista e ano_lancamento no Power Bi com os códigos abaixo:

- Cliquei na opção modelagem, nova coluna e coloquei o seguinte código que me gerou uma nova variável na tabela com o número de músicas por artista:

```sql
musica_por_artista = 
CALCULATE(
    COUNTROWS(nova_tabela_unificada1),
    ALLEXCEPT(nova_tabela_unificada1, nova_tabela_unificada1[artist_s__name_limpo]))
```

E no código abaixo eu criei uma coluna ano_lancamento para me ajudar nas análises por ano.

```sql
ano_lancamento = YEAR(nova_tabela_unificada[data_lancamento])
```

![image.png](image%2048.png)

### 🟣 Ver variáveis categóricas

Nesta etapa eu tenho que visualizar variáveis categóricas através de gráficos de barras.

Realizei a criação de 2 gráficos de barras no Power BI, um com a quantidade de música por artista e outro com a quantidade de música por artista por ano de lançamento.

Como eu fiz:

Opção de relatório;

Visualizações;

Gráfico de barras empilhadas;

No eixo Y ano_lancamento e Eixo X contagem_musica_artista,  para  o gráfico quantidade de música porano de lançamento, com a opção contagem selecionada..

No eixo Y artit_s__name_limpo e Eixo X contagem_musica_artista, para  o gráfico quantidade de música por artista, com a opção contagem selecionada..

![image.png](image%2049.png)

### 🟣 Aplicar medidas de tendência central

Nesta fase tenho que através de tabelas no Power BI, calcular medidas de tendência central (Média e Mediana).

Criei uma tabela utilizando em linhas os artistas e em valores utilizei a variável de  total playlists, em uma coluna utilizei soma, média, mediana, contagem.

![image.png](image%2050.png)

A tabela apresenta dados sobre o número de playlists em que artistas estão presentes em plataformas de streaming. Vamos analisar as colunas e valores apresentados:

1. **Soma de total playlists**: Mostra o total de playlists em que cada artista aparece.
2. **Média de total playlists**: Representa a média de playlists para cada artista, ou seja, quantas playlists cada artista, em média, está presente.
3. **Mediana de total playlists**: O valor mediano das playlists para cada artista, que indica o ponto central, ou seja, metade das playlists está acima e metade abaixo desse valor.
4. **Contagem de total playlists**: Mostra o número total de playlists para cada artista, provavelmente o número de músicas diferentes do artista que estão nessas playlists.

**Exemplo:**

- **Taylor Swift** aparece em 137.856 playlists no total, com uma média de 4.054 playlists por música e 2.448 playlists na mediana, o que indica que algumas músicas dela estão em playlists muito populares.
- **Morgan Wallen** aparece em 9.158 playlists no total, com uma média de 832,55 playlists, sendo o valor mais baixo entre os artistas listados.

**Total**: Refere-se à soma geral de todas as playlists e a estatística total dos dados.

Esses dados podem ser usados para comparar a presença e a visibilidade dos artistas em diferentes playlists das plataformas de streaming.

### Então para melhor avaliar os números montei um gráfico de dispersão com as seguintes informações:

- **Eixo X**: Número de músicas por artista (Soma de musica_por_artista)
- **Eixo Y**: Média de participação em playlists
- **Tamanho da bolha**: Total de streams
- **Legenda**: Nome dos artistas (artist_s__name_limpo)

![image.png](image%2051.png)

Criei cartões gráficos para visualizar os resultados das medianas e médias da tendência central das variáveis streams_limpo e total_playlists.:

![image.png](image%2052.png)

**Análise dos resultados:**

- **Média de streams:** 514,34 milhões
    
    → Em média, as músicas analisadas acumulam cerca de **514 milhões de streams**.
    
- **Mediana de streams:** 289 milhões
    
    → Metade das músicas tem **menos de 289 milhões de streams**, o que indica que **alguns valores muito altos puxam a média para cima**.
    
- **Média de playlists:** 5,67 mil
    
    → Em média, uma música aparece em cerca de **5.670 playlists**.
    
- **Mediana de playlists:** 2.306
    
    → Metade das músicas está em **até 2.306 playlists**, indicando novamente a presença de **valores extremos que elevam a média**.
    

Esses dados reforçam a ideia de que **a distribuição é assimétrica**, com **algumas músicas muito populares** que distorcem as médias, enquanto a maioria tem desempenho mais modesto.

Utilizei código abaixo com a ferramenta Python para construir um gráfico de barras me mostrando visualmente os números qe me retornou de média e mediana.

```python
import matplotlib.pyplot as plt
import numpy as np
import pandas as pd

# Supondo que 'dataset' seja seu DataFrame existente

# Garantindo que os dados são numéricos e limpos
streams = pd.to_numeric(dataset['streams_limpo'], errors='coerce').dropna()
playlists = pd.to_numeric(dataset['total_playlists'], errors='coerce').dropna()

# Calculando as médias e medianas
media_streams = streams.mean()
mediana_streams = streams.median()
media_playlists = playlists.mean()
mediana_playlists = playlists.median()

# Definindo as categorias e os valores
categorias = ['Streams', 'Playlists']
valores_media = [media_streams, media_playlists]
valores_mediana = [mediana_streams, mediana_playlists]

# Criando gráfico
x = np.arange(len(categorias))
largura = 0.35

fig, ax = plt.subplots()
barras_media = ax.bar(x - largura/2, valores_media, width=largura, label='Média', alpha=0.7, color='blue')
barras_mediana = ax.bar(x + largura/2, valores_mediana, width=largura, label='Mediana', alpha=0.7, color='orange')

# Adicionando rótulos nas barras
for barra in barras_media + barras_mediana:
    altura = barra.get_height()
    ax.annotate(f'{altura:.2f}',
                xy=(barra.get_x() + barra.get_width() / 2, altura),
                xytext=(0, 3),  # 3 pontos de deslocamento vertical
                textcoords="offset points",
                ha='center', va='bottom')

# Configurando o gráfico
ax.set_xticks(x)
ax.set_xticklabels(categorias)
ax.set_ylabel('Valores')
ax.set_title('Média e Mediana - Streams vs Playlists')
ax.legend()
plt.tight_layout()
plt.show()

```

Retornando esse gráfico em barras:

![image.png](image%2053.png)

### 🟣 Ver distribuição

Nesta fase eu tenho que visualizar a distribuição através de um histograma utilizando o Python.

Com o código abaixo no Python criei um histograma com os dados total_playlists e streams_limpo.

```python
import matplotlib.pyplot as plt
import pandas as pd

# Convertendo os dados para numérico
dataset['streams_limpo'] = pd.to_numeric(dataset['streams_limpo'], errors='coerce')
dataset['total_playlists'] = pd.to_numeric(dataset['total_playlists'], errors='coerce')

# Removendo valores ausentes
streams = dataset['streams_limpo'].dropna()
playlists = dataset['total_playlists'].dropna()

# Criando os histogramas lado a lado (subplots)
fig, axes = plt.subplots(nrows=1, ncols=2, figsize=(14, 6))

# Histograma de Streams
axes[0].hist(streams, bins=10, color='blue', edgecolor='black')
axes[0].set_title('Distribuição de Streams', fontsize=14)
axes[0].set_xlabel('Quantidade de Streams', fontsize=12)
axes[0].set_ylabel('Frequência', fontsize=12)

# Histograma de Participações em Playlists
axes[1].hist(playlists, bins=10, color='green', edgecolor='black')
axes[1].set_title('Distribuição de Participações em Playlists', fontsize=14)
axes[1].set_xlabel('Participações em Playlists', fontsize=12)
axes[1].set_ylabel('Frequência', fontsize=12)

# Título geral
fig.suptitle('Histogramas de Streams e Participação em Playlists', fontsize=16)

# Ajustando layout
plt.tight_layout(rect=[0, 0, 1, 0.95])
plt.grid(True)
plt.show()

```

Segue histograma que o código me trouxe, no filtro da variável coloquei a opção **não resumir,** me trouxe essa imagem.

![image.png](image%2054.png)

**O que o gráfico mostra:**

**Distribuição de Streams (esquerda)**

- A maioria das músicas tem **menos de 500 milhões de streams**.
- Poucas músicas ultrapassam a marca de **1 bilhão de streams**.
- Isso mostra que **só algumas faixas fazem muito sucesso**, enquanto a maior parte tem desempenho mais modesto.

---

**Distribuição de Participações em Playlists (direita)**

- A maior parte das músicas aparece em **até 10 mil playlists**, com destaque para as que estão em **menos de 5 mil**.
- Poucas músicas são muito populares e aparecem em **mais de 20 mil playlists**.
- Assim como nos streams, **a distribuição é desigual**, com poucas músicas dominando a visibilidade.

Tanto os streams quanto a presença em playlists estão **concentrados em poucas músicas de grande sucesso**, enquanto a maioria tem um alcance mais limitado.

**Conclusão da análise**

A análise dos histogramas revela uma distribuição **desigual** tanto para o número de streams quanto para as participações em playlists. A maior parte das músicas concentra-se em faixas com **baixo volume de streams** e **baixa presença em playlists**, enquanto uma **pequena parcela** alcança valores muito altos em ambas as métricas.

Esse padrão indica que o mercado musical nas plataformas de streaming é **altamente concentrado**, com **poucas músicas dominando a atenção e a visibilidade**, enquanto a maioria enfrenta maior dificuldade para se destacar. Isso reforça a importância de estratégias eficazes de promoção, especialmente voltadas à inclusão em playlists relevantes, para aumentar as chances de uma música alcançar o sucesso.

### 🟣 Aplicar medidas de dispersão

Nesta fase eu tenho que calcular medidas de dispersão através do desvio padrão.

Utilizei a tabela matriz para incluir os dados do streams onde de uma forma mais organizada visualmente vejo a soma, média, variação e desvio padrão da variável.

![image.png](image%2055.png)

**Análise da Dispersão das Execuções (Streams)**

Foi analisado como as músicas são reproduzidas no dataset. Em média, cada faixa tem cerca de **51,4 milhões** de reproduções. Porém, existe uma grande diferença entre as músicas, algumas têm muitas reproduções, enquanto a maioria tem bem menos. Isso é mostrado pelo **desvio padrão** de **56,7 milhões**, que é maior que a própria média.

Poucas músicas concentram a maior parte das reproduções, enquanto a maioria tem números muito menores. Isso é comum na indústria musical e pode acontecer por vários motivos, como presença em playlists populares ou fama do artista.

Entender esse padrão ajuda a gravadora a tomar melhores decisões, como escolher onde investir em marketing, seja focando em algumas músicas principais ou tentando dar mais visibilidade para um número maior de artistas.

Os valores se mostraram longe da média, demostrando maior variabilidade dos dados.

Utilizei a tabela matriz para incluir os dados total playlists onde de uma forma mais organizada visualmente vejo a soma, média, variação e desvio padrão da variável.

![image.png](image%2056.png)

**Análise da Dispersão da Participação em Playlists**

1. **Média**: A média de 5.671,94 playlists indica que, em média, cada elemento (presumivelmente uma música ou artista) está associado a cerca de 5.672 playlists. Este valor representa um valor central do conjunto de dados.
2. **Desvio padrão**: O desvio padrão de 8.929,65 mostra que a participação em playlists é bastante dispersa em relação à média. Isso significa que, embora a média seja de cerca de 5.672 playlists, há uma grande variação entre os dados individuais. O valor elevado do desvio padrão sugere que muitos elementos estão associados a um número significativamente maior ou menor de playlists.
3. **Variação**: A variação de 79.738.566,88 é outro indicador de dispersão que confirma o grande espalhamento dos dados em torno da média. A variação é o quadrado do desvio padrão e, neste caso, ela também reflete uma grande diversidade no número de playlists associadas.

A análise da dispersão indica que a participação em playlists não é uniforme. A grande variação e desvio padrão apontam para uma distribuição desigual, onde algumas músicas ou artistas estão presentes em muitas playlists, enquanto outros estão em um número muito menor. Para entender melhor a distribuição, seria útil verificar a distribuição específica dos dados e possíveis outliers.

### 🟣 Visualizar o comportamento dos dados ao longo do tempo

Nesta etapa eu tenho que visualizar, através de gráficos de linhas, o comportamento dos dados ao longo do tempo no Power BI.

Criei um gráfico de linhas para analisar o comportamento de streams por ano, neste gráfico também é possível filtras por trimestre, mensal e dia, pois na configuração da variável deixei marcado a opção hierarquia de dados.

![image.png](image%2057.png)

- Nossa análise mostra um **impressionante crescimento no streaming de música** ao longo dos anos, com cada vez mais pessoas consumindo música desta forma.
- O ano de **2022 foi o mais expressivo**, marcando o momento de maior consumo musical na plataforma.
- Observamos uma **redução nos números de 2023**, mas isso provavelmente reflete apenas dados parciais ou em atualização do período.

Criei um gráfico de linhas para analisar o comportamento do total_playlists por ano, neste gráfico também é possível filtras por trimestre, mensal e dia, pois na configuração da variável deixei marcado a opção hierarquia de dados.

![image.png](image%2058.png)

O gráfico mostra a participação nas playlists ao longo do tempo, com dados de cada ano. A partir da década de 2000, nota-se um aumento expressivo na soma total de playlists, com um pico muito acentuado por volta de 2020. Antes disso, o gráfico se mantém bastante plano, com pouca variação, especialmente entre as décadas de 1940 a 1990. Esse aumento repentino pode refletir o crescimento da popularidade das plataformas de streaming e a maior inserção de músicas em playlists nos últimos anos, indicando uma mudança significativa no comportamento de consumo de música.

### 🟣 Calcular quartis, decis ou percentis

Nesta etapa eu tenho que criar categorias de quartis para variáveis de características no BigQuery e utilizar o IF para criar as categorias.

Com o código abaixo eu criei o quartil para as variáveis **streams_limpo, participação_total_nas_playlists, bpm, danceability, valence, energy, acousticness, instrumentalness, liveness, spechinness**. No mesmo código eu criei classificações para o quartil de baixo, médio e alto, sendo que:

Para **bpm, danceability, valence, energy, acousticness, instrumentalness, liveness, spechinness,** foi feito a classificação de caracteristicas musicais.

- Quartis 1 e 2 ficam na classificação **baixo**
- Quartil 3 na classificação **médio**;
- Quartil  4 na classificação **alto**.

Para **streams_limpo, participação_total_nas_playlists,** foi feito a classificação binária.

- Quartil 4 é **alto.**
- Quartil os demais (1,2,3) é **baixo.**

**Foi calculado quartil individualmente para cada atributo para não haver divergência de valores por se tratar de dados musicais porém cada atributo tem a sua distribuição.**

Substitui a tabela anterior pela atualizada com o mesmo nome para que ou possa ir no Power BI e somente atualizar para não precisar fazer a importação de uma nova tabela.

```sql
CREATE OR REPLACE TABLE `projeto-2-hipoteses-456812.laboratoria_projeto_2_hipoteses.nova_tabela_unificada1` AS

WITH Quartis AS (
  SELECT
    streams_limpo,
    participacao_total_nas_playlists,
    bpm,
    `danceability_%`,
    `valence_%`,
    `energy_%`,
    `acousticness_%`,
    `instrumentalness_%`,
    `liveness_%`,
    `speechiness_%`,

    -- Quartis numéricos
    NTILE(4) OVER (ORDER BY streams_limpo) AS quartil_streams,
    NTILE(4) OVER (ORDER BY participacao_total_nas_playlists) AS quartil_participacao_total_nas_playlists,
    NTILE(4) OVER (ORDER BY bpm) AS quartil_bpm,
    NTILE(4) OVER (ORDER BY `danceability_%`) AS quartil_danceability,
    NTILE(4) OVER (ORDER BY `valence_%`) AS quartil_valence,
    NTILE(4) OVER (ORDER BY `energy_%`) AS quartil_energy,
    NTILE(4) OVER (ORDER BY `acousticness_%`) AS quartil_acousticness,
    NTILE(4) OVER (ORDER BY `instrumentalness_%`) AS quartil_instrumentalness,
    NTILE(4) OVER (ORDER BY `liveness_%`) AS quartil_liveness,
    NTILE(4) OVER (ORDER BY `speechiness_%`) AS quartil_speechiness
  FROM `projeto-2-hipoteses-456812.laboratoria_projeto_2_hipoteses.nova_tabela_unificada1`
)

SELECT
  a.track_id_corrigido,
  a.track_name_limpo,
  a.artist_s__name_limpo,
  a.streams_limpo,
  a.artist_count,
  a.in_apple_charts,
  a.in_deezer_charts,
  a.in_shazam_charts_tratado,
  a.in_apple_playlists,
  a.in_deezer_playlists,
  a.in_spotify_charts,
  a.in_spotify_playlists,
  a.data_lancamento,
  a.participacao_total_nas_playlists,
  a.mode,
  a.bpm,
  a.`acousticness_%`,
  a.`valence_%`,
  a.`danceability_%`,
  a.`energy_%`,
  a.`instrumentalness_%`,
  a.`liveness_%`,
  a.`speechiness_%`,

  -- Quartis numéricos (novos campos)
  q.quartil_streams,
  q.quartil_participacao_total_nas_playlists,
  q.quartil_bpm,
  q.quartil_danceability,
  q.quartil_valence,
  q.quartil_energy,
  q.quartil_acousticness,
  q.quartil_instrumentalness,
  q.quartil_liveness,
  q.quartil_speechiness,

  -- Classificações binárias
  IF(q.quartil_streams = 4, 'alto', 'baixo') AS classificacao_streams,
  IF(q.quartil_participacao_total_nas_playlists = 4, 'alto', 'baixo') AS classificacao_participacao,

  -- Classificações por característica musical
  CASE WHEN q.quartil_bpm IN (1, 2) THEN 'baixo'
       WHEN q.quartil_bpm = 3 THEN 'médio'
       ELSE 'alto' END AS classificacao_bpm,

  CASE WHEN q.quartil_danceability IN (1, 2) THEN 'baixo'
       WHEN q.quartil_danceability = 3 THEN 'médio'
       ELSE 'alto' END AS classificacao_danceability,

  CASE WHEN q.quartil_valence IN (1, 2) THEN 'baixo'
       WHEN q.quartil_valence = 3 THEN 'médio'
       ELSE 'alto' END AS classificacao_valence,

  CASE WHEN q.quartil_energy IN (1, 2) THEN 'baixo'
       WHEN q.quartil_energy = 3 THEN 'médio'
       ELSE 'alto' END AS classificacao_energy,

  CASE WHEN q.quartil_acousticness IN (1, 2) THEN 'baixo'
       WHEN q.quartil_acousticness = 3 THEN 'médio'
       ELSE 'alto' END AS classificacao_acousticness,

  CASE WHEN q.quartil_instrumentalness IN (1, 2) THEN 'baixo'
       WHEN q.quartil_instrumentalness = 3 THEN 'médio'
       ELSE 'alto' END AS classificacao_instrumentalness,

  CASE WHEN q.quartil_liveness IN (1, 2) THEN 'baixo'
       WHEN q.quartil_liveness = 3 THEN 'médio'
       ELSE 'alto' END AS classificacao_liveness,

  CASE WHEN q.quartil_speechiness IN (1, 2) THEN 'baixo'
       WHEN q.quartil_speechiness = 3 THEN 'médio'
       ELSE 'alto' END AS classificacao_speechiness

FROM `projeto-2-hipoteses-456812.laboratoria_projeto_2_hipoteses.nova_tabela_unificada1` a
LEFT JOIN Quartis q
  ON a.streams_limpo = q.streams_limpo
  AND a.participacao_total_nas_playlists = q.participacao_total_nas_playlists
  AND a.bpm = q.bpm
  AND a.`danceability_%` = q.`danceability_%`
  AND a.`valence_%` = q.`valence_%`
  AND a.`energy_%` = q.`energy_%`
  AND a.`acousticness_%` = q.`acousticness_%`
  AND a.`instrumentalness_%` = q.`instrumentalness_%`
  AND a.`liveness_%` = q.`liveness_%`
  AND a.`speechiness_%` = q.`speechiness_%`

```

Resultado final do código, colunas com as novas variáveis de quartil e classificações:

Quartil

![image.png](image%2059.png)

Classificações

![image.png](image%2060.png)

### 🟣 Calcular correlação entre variáveis

Nesta etapa eu tenho que calcular a correlação no BigQuery via comando CORR.

Correlação de Pearson:

O coeficiente de correlação de Pearson pode assumir valores no intervalo de -1 a 1.

- Um valor 1: indica uma correlação positiva perfeita. Isto significa que à medida que uma variável aumenta, a outra variável também aumenta de forma perfeitamente linear.
- Um valor de -1: Indica uma correlação negativa perfeita. Isso significa que à medida que uma variável aumenta, a outra variável diminui de maneira perfeitamente linear.
- Um valor 0: indica a ausência de correlação linear. Não há relação linear entre as duas variáveis.

**Variável streams**

Com o código abaixo eu calculei a correlação de Pearson entre as variáveis streams_limpo e total_playlists.

O valor da correlação mostrado na imagem é 0,78353745869, o que indica uma correlação positiva forte entre o número de streams e o total de playlists em que uma música está presente.
Correlação forte e positiva: Isso sugere que, de maneira geral, à medida que o número de playlists em que uma música está incluída aumenta, também tende a aumentar o número de streams.A presença em mais playlists parece ser um fator relevante para o sucesso de uma música em termos de streams. Ou seja, se uma música está em muitas playlists, é mais provável que ela tenha um maior número de streams.

Esse valor de correlação reforça a ideia de que as playlists desempenham um papel importante na popularidade de uma música, ajudando a aumentar a visibilidade e, consequentemente, os streams.

```sql
SELECT CORR(streams_limpo, total_playlists) AS correlacao_valores
FROM `projeto-2-hipoteses-456812.laboratoria_projeto_2_hipoteses.nova_tabela_unificada1` 
```

Resultado da consulta:

![image.png](image%2061.png)

**Variável Danceability**

Com o código abaixo eu calculei a correlação de Pearson entre as variáveis streams_limpo e danceability_%, nos trazendo o valor de -0,10 que indica uma **correlação fraca e negativa**.

Ou seja **quanto mais dançável a música, levemente menor tende a ser o número de streams, mas essa relação é muito fraca**. Em termos práticos: **quase não há relação linear entre esses dois fatores**.

```sql
SELECT CORR(streams_limpo, `danceability_%`) AS correlacao_valores
FROM `projeto-2-hipoteses-456812.laboratoria_projeto_2_hipoteses.nova_tabela_unificada1`

```

Resultado da consulta:

![image.png](image%2062.png)

**Variável streams + categoriais musicais**

Com o código abaixo eu calculei a correlação de Pearson entre as variáveis streams_limpo e valence, energy, acousticness, liveness, speechiness e bpm, nos trazendo o valor de -0,04, -0,02, -0,00, -0,04, -0,04, -0,11, -0,00 que indica uma **correlação fraca e negativa**. Nenhuma característica musical isolada tem **correlação significativa com a quantidade de streams**. Isso **sugere que outros fatores** (como marketing, artista, presença em playlists, ou viralização) influenciam mais o sucesso.

```sql
SELECT
  CORR(streams_limpo, `valence_%`) AS corr_valence,
  CORR(streams_limpo, `energy_%`) AS corr_energy,
  CORR(streams_limpo, `acousticness_%`) AS corr_acousticness,
  CORR(streams_limpo, `instrumentalness_%`) AS corr_instrumentalness,
  CORR(streams_limpo, `liveness_%`) AS corr_liveness,
  CORR(streams_limpo, `speechiness_%`) AS corr_speechiness,
  CORR(streams_limpo, bpm) AS corr_bpm
FROM `projeto-2-hipoteses-456812.laboratoria_projeto_2_hipoteses.nova_tabela_unificada1`

```

Resultado da consulta:

![image.png](image%2063.png)

# **🟥 5.3 Aplicar técnica de análise**

### 🔴 Aplicar segmentação

Nesta etapa e tenho que analisar as categorias criadas através dos quartis para as características da música em relação à variável streams.

Alterar gráficos e tabelas pois foi alterado os quartis e classificação

Criei uma tabela matriz com as informações média de streams por classificação da variável danceability.

![image.png](image%2064.png)

O que eu tenho nos dados:
**Baixo (Quartis 1 e 2):** 557 registros

**Médio (Quartil 3):** 511 registros

**Alto (Quartil 4):** 430 registros

**Interpretação:**

- A maioria dos dados está concentrada nos níveis **baixo** e **médio** de danceability.
- A menor quantidade está no nível **alto**, o que pode indicar que músicas com alta danceability são menos comuns na sua base de dados.
- A maior parte das músicas tende a ter danceability **baixa ou média**, então essas faixas talvez sigam um padrão mais comum da indústria.
- Músicas com alta danceability são **menos frequentes** e, dependendo do contexto, isso pode representar um **diferencial competitivo** ou um **nicho menos explorado**.

Média de streams por classificação da variável Acousticness.

![image.png](image%2065.png)

 **Streams semelhantes para "alto" e "baixo" acousticness**:

- As músicas com valores **altos** e **baixos** de *acousticness* têm a **mesma média de streams (531)**.
- Isso indica que tanto músicas muito acústicas quanto músicas pouco acústicas atraem um público semelhante em termos de popularidade (número médio de reproduções).

 **Menor média para a categoria "média"**:

- Músicas com *acousticness* **média** têm **menos streams em média (463)**.
- Isso pode indicar que músicas com características acústicas moderadas são **menos atraentes ou menos marcantes** para o público, ficando no “meio do caminho” entre os dois extremos.

 **Interpretação possível**:

- O padrão sugere que o público tende a preferir **características mais marcantes ou distintas**, seja muito acústico (ex: voz e violão, folk, baladas) ou muito pouco acústico (ex: eletrônica, hip-hop, pop moderno).
- Músicas “intermediárias” em termos de *acousticness* podem não se destacar tanto e, por isso, têm menor desempenho médio.

Média de streams por classificação da variável Acousticness.

- **Baixo BPM**: 540 streams
- **Alto BPM**: 524 streams
- **Médio BPM**: 451 streams

![image.png](image%2066.png)

- **Músicas com BPM mais extremos (alto ou baixo) têm mais streams**:
    - Assim como na análise do *acousticness*, os **valores extremos de BPM** (muito lentas ou muito rápidas) atraem mais ouvintes.
    - Músicas **lentas** (baixo BPM) tiveram até uma média levemente maior que as rápidas, sugerindo que faixas mais suaves ou relaxantes como baladas ou lo-fi (“low fidelity”, ou seja, baixa fidelidade sonora) são ligeiramente mais populares.
- **Músicas com BPM médio tiveram menos streams**:
    - A categoria **média de BPM (451)** teve a menor média de streams, o que pode indicar que músicas com ritmo moderado **não se destacam tanto** e podem ser percebidas como menos cativantes ou específicas para certos contextos (ex: festa ou relaxamento).
- **Tendência geral**:
    - Assim como no *acousticness*, há uma **preferência do público por características musicais mais marcantes,** seja uma música mais acelerada ou mais lenta, em vez de algo intermediário.

Média de streams por classificação da variável Energy.

- **Baixa energy**: 525 streams
- **Média energy**: 504 streams
- **Alta energy**: 501 streams

![image.png](image%2067.png)

- **Leve vantagem para músicas de baixa energia**:
    - As músicas com **baixa energia** tiveram a maior média de streams. Isso pode indicar uma preferência do público por músicas mais calmas, suaves ou relaxantes (ex: lo-fi, R&B, acústicas).
- **Músicas com energia média e alta tiveram desempenho semelhante, mas levemente inferior**:
    - A diferença entre média (504) e alta (501) é pequena, o que sugere que não há uma rejeição clara por músicas com mais energia, mas também não há uma preferência marcante por elas neste conjunto.
- **Padrão semelhante às outras variáveis**:
    - Mais uma vez, observamos que **extremos (neste caso, baixa energia)** podem ter desempenho superior às faixas intermediárias, reforçando a ideia de que músicas com **características mais definidas ou específicas** tendem a se destacar.

Média de streams por classificação da variável Instrumentalness.

- **Alta instrumentalness**: 494 streams
- **Média instrumentalness**: 539 streams
- **Baixa instrumentalness**: 512 streams

![image.png](image%2068.png)

- **Músicas com instrumentalidade média foram mais populares**:
    - Diferente das outras variáveis analisadas (como *acousticness*, *BPM*, e *energy*), aqui a categoria **média** teve o **maior número médio de streams**.
- **Instrumentais puros (alta instrumentalness) tiveram a menor média**:
    - Músicas muito instrumentais, sem ou com pouquíssima voz, tiveram **menos streams**, sugerindo que o público tende a preferir músicas com vocais ou pelo menos algum equilíbrio entre voz e instrumental.
- **Padrão reverso comparado a outras variáveis**:
    - Enquanto nas outras categorias os **extremos tendem a ter mais streams**, aqui vemos o **meio-termo sendo mais popular**, indicando que o público pode gostar de uma **mistura equilibrada entre instrumental e vocal**, talvez algo como trilhas sonoras, música ambiente com vocais suaves, etc.

Média de streams por classificação da variável liveness

- **Baixa liveness**: 533 streams
- **Média liveness**: 500 streams
- **Alta liveness**: 489 streams

![image.png](image%2069.png)

- **Músicas com baixa liveness são as mais populares**:
    - **Liveness** representa a presença de som de público ao vivo (como em shows ou gravações ao vivo).
    - Músicas com **baixa liveness** (mais "limpas", de estúdio) têm, em média, **mais streams**, sugerindo que o público geralmente prefere faixas com **qualidade de estúdio**.
- **Desempenho cai à medida que a liveness aumenta**:
    - **Médias e altas liveness** tiveram menos streams, com a **alta** sendo a menor.
    - Isso indica que gravações ao vivo (com ruídos de plateia, reverberações de palco etc.) **não são tão populares em média**, talvez por serem menos adequadas para playlists pessoais, foco, ou escuta casual.
- **Tendência consistente com clareza de produção**:
    - Este dado reforça uma possível preferência por **faixas bem produzidas, com som limpo e controlado**, o que é típico de gravações de estúdio com baixa liveness.

Média de streams por classificação da variável valence.

- **Baixo valence**: **541** streams
- **Médio valence**: **504** streams
- **Alto valence**: **469** streams

![image.png](image%2070.png)

- **Músicas com valence baixo (mais tristes ou melancólicas) são as mais populares**:
    - Isso indica uma **preferência do público por faixas com carga emocional mais densa ou introspectiva**.
    - Pode estar relacionado à popularidade de gêneros como lo-fi, indie, trap melódico, R&B, ou baladas emocionais.
- **Valence alto (músicas alegres e positivas) tiveram a menor média de streams**:
    - Isso sugere que **músicas muito "felizes" ou animadas** são, em média, **menos populares**, talvez por serem mais nichadas ou menos usadas em contextos como estudo, relaxamento ou introspecção.
- **Mais um padrão onde o extremo emocional negativo se destaca**:
    - Essa tendência reforça que músicas que causam **mais impacto emocional** (mesmo que seja tristeza ou melancolia) podem gerar mais engajamento, replay ou conexão com o ouvinte.

Média de streams por classificação da variável speechiness.

- **Baixo speechiness**: **555** streams
- **Médio speechiness**: **526** streams
- **Alto speechiness**: **420** streams

![image.png](image%2071.png)

- **Músicas com pouca fala são as mais populares**:
    - **Speechiness baixo** (pouca ou nenhuma fala, como músicas cantadas normalmente) tem a **maior média de streams**.
    - Isso sugere uma clara **preferência por músicas com foco melódico** em vez de falado, como entrevistas, skits, ou faixas muito faladas (ex: spoken word).
- **Speechiness alto tem a menor média (420)**:
    - Músicas com **muita fala** têm desempenho bem inferior, indicando que **faixas com alto conteúdo falado não engajam tanto** no streaming, o que é esperado, já que muitas podem nem ser músicas completas, mas sim introduções, interlúdios ou faixas experimentais.
- **Speechiness médio (ex: rap, trap melódico)** ainda tem boa média:
    - Isso sugere que **há espaço para músicas com elementos falados**, desde que equilibrados com melodia (como ocorre no rap ou trap).

**O que mais impulsiona o sucesso (em streams) é:**

- **Músicas com**:
    - **Baixa speechiness** (pouca fala)
    - **Baixa valence** (emocionalmente densas)
    - **Baixo BPM** (lentas)
    - **Baixa liveness** (estúdio)
    - **Baixa energy** (calmas)
    - **Instrumentalness média** (equilíbrio vocal-instrumental)

Esses dados sugerem que, neste conjunto, **músicas introspectivas, suaves, de estúdio, com vocais bem definidos e melodia marcante** têm **mais chances de alcançar sucesso em termos de streams**.
Com base nas características, músicas que possuam essas qualidades tendem a atrair mais streams. É interessante observar que a combinação de elementos como uma energia mais calma, melodia marcante e vocais bem definidos pode criar uma conexão mais profunda com o público, fazendo com que a música seja mais apreciada e, consequentemente, mais reproduzida.

### 🔴 Validar hipótese

Nesta etapa eu tenho que validar as hipóteses levantadas através de correlação e gráfico de dispersão.

**Essas são as hipóteses levantadas pela gravadora.**

**Hipótese 1 -** Músicas com BPM (Batidas Por Minuto) mais altos fazem mais sucesso em termos de streams no Spotify.

**A análise refuta a hipótese de que músicas com BPM mais elevados estão associadas a um maior número de streams. O coeficiente de correlação de -0,0023 indica uma relação extremamente fraca entre as duas variáveis. Isso sugere que o ritmo da música, medido em BPM, não é um fator relevante para determinar seu sucesso em termos de streams no Spotify.**

**Hipótese 2 -** As músicas mais populares no ranking do Spotify também possuem um comportamento semelhante em outras plataformas como Deezer.

**A análise confirma que músicas populares no Spotify geralmente têm um desempenho similar no Deezer, corroborando a hipótese da gravadora. A correlação de 0,826 reforça essa relação, indicando uma forte associação entre o desempenho das músicas nas duas plataformas. Isso sugere que o sucesso em uma plataforma tende a se refletir na outra, possivelmente devido ao alcance semelhante de público ou estratégias de divulgação integradas.**

**Hipótese 3 -** A presença de uma música em um maior número de playlists é relacionada a um maior número de streams.

**A hipótese foi confirmada por meio das análises realizadas. A correlação de 0,783 indica uma relação positiva forte e estatisticamente significativa entre a presença da música em playlists e seu volume de streams. Esse resultado destaca o papel estratégico das playlists como um dos principais fatores de impulsão do desempenho das músicas nas plataformas de streaming, ampliando sua visibilidade e alcance junto ao público.**

**indica que quanto mais uma música estiver presente em playlists, maior tende a ser seu número de streams. Ou seja, a inclusão em playlists, especialmente nas mais populares, aumenta significativamente a chance de uma música ser ouvida, o que evidencia a importância de estratégias focadas em colocar faixas dentro dessas seleções para alcançar melhores resultados nas plataformas.**

**Hipótese 4 -** Artistas com maior número de músicas no Spotify têm mais streams.

**A análise mostra uma correlação positiva de 0,790 entre o número de músicas disponíveis de um artista no Spotify e o total de streams acumulados. Isso sugere que, quanto maior o número de músicas lançadas, maior tende a ser a quantidade de streams, validando a hipótese da gravadora.**

**Hipótese 5 -** As características da música influenciam no sucesso em termos de streams no Spotify.

**A análise mostrou que todas as características musicais apresentaram correlações muito fracas com o número de streams, variando entre -0,11 e -0,00. Isso indica que, isoladamente, essas características não têm influência significativa no sucesso de uma música em termos de streams no Spotify. Isso sugere que outros fatores, como marketing, engajamento dos fãs ou presença em mídias e playlists, podem ter um papel mais relevante.**

### **Conclusão da validação das hipóteses**

As análises realizadas fornecem insights valiosos sobre os fatores que influenciam o sucesso de músicas nas plataformas de streaming. De maneira geral, os dados mostram que **variáveis relacionadas à visibilidade e presença estratégica** (como número de playlists e quantidade de músicas disponíveis) têm **maior impacto no volume de streams** do que características técnicas das músicas, como BPM ou outros atributos sonoros.

Dentre os principais destaques:

- A **presença em playlists** se mostrou um dos fatores mais influentes, reforçando a necessidade de estratégias de curadoria e negociação com plataformas para inclusão de músicas em listas populares.
- O **desempenho em plataformas é consistente**, indicando que o sucesso em uma tende a se refletir nas demais.
- A **quantidade de músicas lançadas por um artista** também impacta diretamente os streams, sugerindo que a regularidade nas publicações pode ampliar o alcance e engajamento.
- Já os **atributos musicais isolados** não demonstraram influência significativa no desempenho, indicando que fatores externos como marketing, fãs e exposição são mais determinantes.

Essas conclusões oferecem à gravadora **um direcionamento claro para suas ações de lançamento e promoção**, focando menos em padrões técnicos das faixas e mais em estratégias de **distribuição, presença digital e posicionamento nas plataformas**.

### **Hipótese 1 - Músicas com BPM (Batidas Por Minuto) mais altos fazem mais sucesso em termos de streams no Spotify.**

No BigQuery apliquei a correlação de Pearson nas variáveis bpm e streams_limpo, para avaliar a **hipótese 1 da gravadora.**

```sql
SELECT 
  CORR(bpm, streams_limpo) AS correlation_valores
FROM 
  `projeto-2-hipoteses-456812.laboratoria_projeto_2_hipoteses.nova_tabela_unificada1`

```

Um resultado de **-0,0023** na correlação significa que a correlação entre as duas variáveis (no caso, **BPM** e **streams_limpo**) é **próxima de zero**, o que indica que não há uma relação linear significativa entre elas.

![image.png](image%2072.png)

O gráfico de dispersão entre as duas variáveis confirma a ausência de correlação significativa. A linha de tendência linear, praticamente horizontal, e a grande dispersão dos pontos reforçam que não há uma relação linear clara entre elas.

![image.png](image%2073.png)

### **Hipótese 2 -** As músicas mais populares no ranking do Spotify também possuem um comportamento semelhante em outras plataformas como Deezer.

Apliquei a correlação entre as variáveis spotify playlists e charts e deezer playlists e charts no BigQuery, para avaliar a **hipótese 2 da gravadora.**

```sql
SELECT
  CORR(in_spotify_charts + in_spotify_playlists,
       in_deezer_charts + in_deezer_playlists) AS correlacao_popularidade
FROM
  `projeto-2-hipoteses-456812.laboratoria_projeto_2_hipoteses.nova_tabela_unificada1`

```

![image.png](image%2074.png)

Com resultado de 0,83 que indica:

- **Correlação forte e positiva**, indicando que **músicas populares no Spotify também tendem a ser populares no Deezer**, com base em sua presença em **charts e playlists**.
- Isso **confirma** uma **tendência consistente de popularidade entre as plataformas**, pelo menos no seu conjunto de dados.

![image.png](image%2075.png)

A análise de dispersão revela uma correlação clara entre a presença de músicas nos charts do Spotify e do Deezer, indicando que o sucesso em uma plataforma tende a se refletir na outra. Embora a relação com playlists seja mais dispersa, ainda é possível observar uma leve tendência positiva, especialmente nas faixas com maior visibilidade.

Para confirmação dos dados criei duas novas variáveis Popularidade_Deezer e Popularidade_spotify, na opção modelagem, nova coluna no power BI, com os seguintes códigos de comando.

```sql
Popularidade_Deezer = 'nova_tabela_unificada1'[in_deezer_charts] + 'nova_tabela_unificada1'[in_deezer_playlists]
```

```sql
Popularidade_Spotify = 'nova_tabela_unificada1'[in_spotify_charts] + 'nova_tabela_unificada1'[in_spotify_playlists]
```

E montei um gráfico de dispersão com as duas variáveis

![image.png](image%2076.png)

A análise demonstra uma **forte correlação positiva entre a popularidade no Spotify e no Deezer,**

sugerindo que músicas populares em uma plataforma **tendem a também ser populares na outra.**

Esse comportamento indica que o sucesso de uma faixa **não está isolado** a uma única plataforma, e pode refletir **tendências de consumo musical mais amplas** entre os usuários de streaming.

Reforçando a análise anterior.

### **Hipótese 3 - A presença de uma música em um maior número de playlists é relacionada a um maior número de streams.**

Apliquei a correlação entre as variávei total_playlists e streams_limpo, no BigQuery, para avaliar a **hipótese 3 da gravadora.**

```sql
SELECT CORR(streams_limpo, total_playlists) AS correlacao_valores
FROM `projeto-2-hipoteses-456812.laboratoria_projeto_2_hipoteses.nova_tabela_unificada1` 
```

![image.png](image%2077.png)

O valor da correlação mostrado na imagem é 0,78353745869, o que indica uma correlação positiva forte entre o número de streams e o total de playlists em que uma música está presente.
Correlação forte e positiva: Isso sugere que, de maneira geral, à medida que o número de playlists em que uma música está incluída aumenta, também tende a aumentar o número de streams. A presença em mais playlists parece ser um fator relevante para o sucesso de uma música em termos de streams. Ou seja, se uma música está em muitas playlists, é mais provável que ela tenha um maior número de streams.

Esse valor de correlação reforça a ideia de que as playlists desempenham um papel importante na popularidade de uma música, ajudando a aumentar a visibilidade e, consequentemente, os streams.

![image.png](image%2078.png)

O gráfico de dispersão reforça a análise da correlação acima, apresenta uma forte correlação positiva entre as duas variáveis.

### **Hipótese 4 - Artistas com maior número de músicas no Spotify têm mais streams.**

No BigQuery fiz a correlação entre streams e in_spotify_playlists para analisar a **hipótese 4 da gravadora.**

Utilizado o código abaixo para calcular a correlação:

```sql
SELECT 
  CORR(streams_limpo, in_spotify_playlists) AS correlation_valores
FROM 
  `projeto-2-hipoteses-456812.laboratoria_projeto_2_hipoteses.nova_tabela_unificada1`
```

Resultado:

![image.png](image%2079.png)

Uma **correlação de 0,790** entre o **número de músicas no Spotify por artista** e o **total de streams** indica uma **forte correlação positiva**. Isso **apoia sua Hipótese 4** com bastante evidência estatística.

A correlação entre o número de músicas de um artista no Spotify e o total de streams foi de 0,790, indicando uma forte relação positiva. Isso sugere que artistas com mais músicas tendem a ter mais streams, possivelmente por estarem mais presentes na plataforma e alcançarem um público maior. Apesar de não indicar causa direta, o resultado reforça a ideia de que lançar mais músicas pode ajudar no desempenho em streams.

![image.png](image%2080.png)

O gráfico de dispersão reforça a análise da correlação acima, apresenta uma forte correlação positiva entre as duas variáveis.

### **Hipótese 5 - As características da música influenciam no sucesso em termos de streams no Spotify.**

No BigQuery calculei a correlação entre streams e as caracteristicas musicais com o seguinte código:

```sql
  SELECT
  CORR(streams_limpo, `danceability_%`) AS corr_danceability,
  CORR(streams_limpo, `energy_%`) AS corr_energy,
  CORR(streams_limpo, `valence_%`) AS corr_valence,
  CORR(streams_limpo, `speechiness_%`) AS corr_speechiness,
  CORR(streams_limpo, `acousticness_%`) AS corr_acousticness,
  CORR(streams_limpo, `instrumentalness_%`) AS corr_instrumentalness,
  CORR(streams_limpo, `liveness_%`) AS corr_liveness,
FROM `projeto-2-hipoteses-456812.laboratoria_projeto_2_hipoteses.nova_tabela_unificada1`
WHERE streams_limpo IS NOT NULL;

```

Me trazendo o resultado:

![image.png](image%2081.png)

Esses resultados indicam que as características musicais têm pouca ou nenhuma correlação linear com o número de streams.

![image.png](image%2082.png)

Tanto os resultados da análise de correlação no BigQuery quanto o gráfico de dispersão indicam que não há uma relação linear significativa entre as características musicais e a quantidade de streams. Os coeficientes de correlação, todos próximos de zero, sugerem que atributos como danceability, energy, valence, entre outros, não têm impacto direto ou consistente no desempenho das músicas em termos de streams.

# **🟧 5.4 Resumir informações em um dashboard ou relatório**

### 🟠 Representar dados por meio de tabela resumo ou scorecards.

Nesta etapa eu tenho que criar scorecards para números gerais do banco de dados.

Através da ferramenta Power Bi criei cartões com os totais dos dados relevantes do meu dataset.

![image.png](image%2083.png)

![image.png](image%2084.png)

![image.png](image%2085.png)

![image.png](image%2086.png)

Com a opção visualização de indicador eu coloquei visualmente as características musicais do meu dataset.

![image.png](image%2087.png)

Criei uma medida para o valor máximo de BPM definido como 60 mínimo e 210 máximo, para refletir um BPM realista.

Usou uma medida com MIN e VALUE para garantir que o valor máximo do gauge não ultrapasse 1, útil para características musicais que vão de 0 a 1.

Essa medida divide por 100 para ajustar a escala padrão de 0 a 1, comum para gráficos tipo gauge.

```sql
Acousticness Corrigido = DIVIDE(AVERAGE('nova_tabela_unificada1'[acousticness_%]), 100)

```

Essa medida **retorna o maior valor de `acousticness_%` da base de dados**, **limitando esse valor ao máximo de 1.**

```sql
Acoustcness Limitado = 
MIN(1, VALUE(MAX('nova_tabela_unificada1'[acousticness_%])))
```

### 🟠 Representar dados através de gráficos simples

Nesta etapa o objetivo é representar dados através de gráficos de barras e linhas.

Criei gráficos de linha e barras para organizar os dados para reunir em meu dashboard.

![image.png](image%2088.png)

![image.png](image%2089.png)

### 🟠 Representar dados por meio de gráficos ou **Recursos** visuais avançados

Nesta etapa o objetivo **é r**epresentar dados por meio de gráfico de dispersão.

Criei três gráficos de dispersão para reunir informações importantes para incluir no dashboard.

**Gráfico 1:**

![image.png](image%2090.png)

**Gráfico 2:**

![image.png](image%2091.png)

**Gráfico 3:**

![image.png](image%2092.png)

**Gráfico 4:**

![image.png](image%2093.png)

Os gráficos representam a correlação entre as variáveis de forma positiva ou linear (sem correlação).

**Gráfico 1 *Influência StreamsxBPM***

- **O que mostra:**
    
    Confirma a ausência de correlação significativa entre BPM e a quantidade de streams. A linha de tendência é praticamente horizontal, e os pontos estão amplamente dispersos.
    
- **O que isso sugere:**
    
    O ritmo (BPM) da música não é um fator determinante para o sucesso em termos de streams. Tanto músicas rápidas quanto lentas podem atrair grandes audiências.
    
- **Recomendação para a gravadora:**
    
    Não utilizar o BPM como principal critério de seleção de músicas. Priorize aspectos mais influentes, como composição, melodia, vocal e estilo, que afetam diretamente a conexão com o público.
    

***Gráfico 2 Correlação Deezer x Spotify x Apple***

- **O que mostra:**
    
    Há uma forte correlação positiva entre a popularidade das músicas nas plataformas Deezer, Spotify e Apple Music.
    
- **O que isso sugere:**
    
    Músicas populares em uma plataforma geralmente também são bem recebidas nas demais, indicando padrões de gosto semelhantes entre os usuários das diferentes plataformas.
    
- **Recomendação para a gravadora:**
    
    Investir em uma abordagem de distribuição e marketing que atinja simultaneamente várias plataformas, maximizando o alcance e garantindo presença consistente do artista em todos os canais relevantes.
    

***Gráfico 3 Correlação streams x total playlists***

- **O que mostra:**
    
    Indica uma correlação forte e positiva entre o número de playlists em que uma música está incluída e sua quantidade total de streams.
    
- **O que isso sugere:**
    
    A presença em mais playlists amplia significativamente a visibilidade da música, o que aumenta o número de reproduções.
    
- **Recomendação para a gravadora:**
    
    Desenvolver estratégias para inserir as músicas em múltiplas playlists (editoriais, colaborativas e de usuários), otimizando as chances de descoberta por novos ouvintes.
    

***Gráfico 4 Correlação entre Spotify x Streams***

- **O que mostra:**
    
    Revela uma forte correlação positiva entre o número de inserções em playlists do Spotify e a quantidade de streams obtidos.
    
- **O que isso sugere:**
    
    A presença em playlists específicas do Spotify, principalmente as curadas por editores ou com grande número de seguidores, tem influência direta no sucesso das músicas.
    
- **Recomendação para a gravadora:**
    
    Priorizar ações voltadas ao Spotify, como submissão estratégica via Spotify for Artists, campanhas segmentadas e contato com curadores, para aumentar a presença nas playlists de destaque.
    

### 🟠 Aplicar opções de filtros para gerenciamento e interação

Nesta etapa eu tenho que incluir filtros para visualizar resultados por categoria e por data.

Inclui 4 filtro pata visualização dos dados por categoria, escolhi as seguintes informações:

Ano, música, artista para conseguir filtrar por ano as informações do dataset;

Streams para conseguir ver que não há correlação nas caracteristicas musicais.

![image.png](image%2094.png)

Unindo todas as informações eu criei um Dashboard com as informações abaixo.

- Em cartões coloquei as informações.

Total artistas;

Total Streams;

Total músicas;

Total Playlists.

- Em Indicadores coloquei as informações das características musicais.

Instrumentalness;

Liveness,

Danceability;

Acousticness;

Speechiness;

Energy;

Valence;

BPM.

- Com os gráficos de dispersão trouxe as informações de :

Influência de BPM nos Streams;

Correlação entre Deezer, Spotify e Apple;

Correlação entre streams e total de playlists;

Correlação entre spotify playlists e streams.

- Com os gráficos de barras empilhadas trouxe as informações de :

Top artistas com mais streams;

- Com os gráficos de linhas trouxe as informações de :

Comportamento das playlists por ano.

Todos os dados do dashboard voltado para as respostas das hipóteses da gravadora.

![image.png](image%2095.png)

# **🟩 5.5 Apresentar resultados**

### 🟢 Selecionar gráficos e informações relevantes

Nesta etapa eu tenho que resumir as informações para uma apresentação de 5 minutos.

Selecionei as informações relevantes para minha apresentação que será os mesmos do meu Dashboard.

O número total de artistas, streams, músicas e playlists.

O que mais influencia o sucesso da música, boas correlações, e as confirmações das hipóteses.

### 🟢 Criar uma apresentação

Nesta etapa eu tenho que criar uma apresentação de slides que oriente e apresente os resultados mais importantes.

Criado apresentação pelo Canva apresentações com 5 slides e tempo estimado de apresentação 5 minutos.

Link abaixo.

### 🟢 Apresentar resultados com conclusões e recomendações

Nesta etapa eu tenho que gravar um vídeo de no máximo 5 minutos explicando suas conclusões e recomendações.

Resultados apresentados em vídeo pelo Loom.

Link abaixo.

# **Links úteis**

### Link Apresentação canva

[https://www.canva.com/design/DAGnFAemDVg/GuheXLHHJL3FK6OlTZGbsA/edit?utm_content=DAGnFAemDVg&utm_campaign=designshare&utm_medium=link2&utm_source=sharebutton](https://www.canva.com/design/DAGnFAemDVg/GuheXLHHJL3FK6OlTZGbsA/edit?utm_content=DAGnFAemDVg&utm_campaign=designshare&utm_medium=link2&utm_source=sharebutton)

### Link Apresentação Loom

[https://www.loom.com/share/c20f7424e19a46b59068bd101d8a578a?sid=04edc197-6320-4104-b7f5-bc9ad74a9891](https://www.loom.com/share/c20f7424e19a46b59068bd101d8a578a?sid=04edc197-6320-4104-b7f5-bc9ad74a9891)

### Link notion

[https://www.notion.so/Projeto-2-1d2469bffa528024a069c609bde12bb9?pvs=4]