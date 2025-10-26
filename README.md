<div align="center">
<img src="https://capsule-render.vercel.app/api?type=venom&height=300&color=gradient&text=CYNE%20API&textBg=false&desc=API%20de%20Agregação%20de%20Dados%20de%20Filmes%20e%20Séries&animation=fadeIn" align="center" style="width: 100%" />

---

</div>

<p align="center">
  <img src="https://img.shields.io/badge/Status-Online-brightgreen?style=for-the-badge" alt="Status">
  <img src="https://img.shields.io/badge/Versão-1.0.0-blue?style=for-the-badge" alt="Versão">
  <img src="https://img.shields.io/badge/Consumo-Gratuito-orange?style=for-the-badge" alt="Consumo">
  <a href="https://creativecommons.org/licenses/by-nc/4.0/">
    <img src="https://img.shields.io/badge/License-CC%20BY--NC%204.0-blue.svg?style=for-the-badge" alt="License: CC BY-NC 4.0">
  </a>
  <img src="https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white" alt="Node.js">
  <img src="https://img.shields.io/badge/Express-000000?style=for-the-badge&logo=express&logoColor=white" alt="Express">
  <img src="https://img.shields.io/badge/Powered%20by-Claude%20AI-9370DB?style=for-the-badge&logo=anthropic&logoColor=white" alt="Powered by Claude AI">
</p>

<br/>

# 🎬 CYNE - CineAgregador API

> **Uma API RESTful rápida e leve para agregação de dados de filmes e séries de TV**  
> Consolidando informações do TMDb, OMDb e outras fontes públicas em um único endpoint.

<br/>

## 📖 Índice

<details>
<summary><b>📋 Clique para expandir o índice completo</b></summary>

- [🌟 Sobre o Projeto](#-sobre-o-projeto)
- [🚀 Base URL](#-base-url)
- [🔑 Fontes de Dados](#-fontes-de-dados)
- [📡 Endpoints Disponíveis](#-endpoints-disponíveis)
  - [1. Informações da API](#1-informações-da-api)
  - [2. Detalhes de Mídia](#2-detalhes-de-mídia)
  - [3. Busca de Mídia](#3-busca-de-mídia)
  - [4. Título Aleatório](#4-título-aleatório)
  - [5. Descobrir Títulos](#5-descobrir-títulos)
- [💡 Exemplos de Uso](#-exemplos-de-uso)
- [⚠️ Limites e Restrições](#️-limites-e-restrições)
- [🟢 Status](#status)
- [🛠️ Tecnologias](#️-tecnologias)
- [📜 Licença](#-licença)
- [👨‍💻 Créditos](#créditos)

</details>

<br/>

---

## 🌟 Sobre o Projeto

**CYNE - CineAgregador API** é uma solução backend open-source que agrega dados de múltiplas APIs de cinema e televisão, fornecendo informações consolidadas, avaliações, links de players e recomendações em um único endpoint JSON.

### ✨ Principais Características

- ✅ **Agregação Multi-Fonte**: Combina dados do TMDb, OMDb e outras APIs
- 🎯 **Busca Inteligente**: Sistema de busca com retry e tratamento robusto de erros
- 🎬 **Players Integrados**: Links diretos para VidSrc, WarezCDN e legendas
- 🔄 **Sistema de Retry**: Até 4 tentativas automáticas com delay exponencial
- 📊 **Estatísticas Completas**: IMDb Rating, Metascore, TMDb Score e mais
- 🎲 **Modo Aleatório**: Descubra títulos populares de forma aleatória
- 🔍 **Filtros Avançados**: Descoberta por gênero, ano e ordenação

<br/>

---

## 🚀 Base URL
> [!WARNING]
>  O endpoint base fornecido (bore.pub:15776) utiliza um serviço de tunneling e é estritamente temporário e instável. A disponibilidade e o tempo de atividade da API não são garantidos, e o serviço pode ser descontinuado sem aviso prévio.
> Foi Disponibilizada apenas para fins de teste (PoC).
```
http://bore.pub:15776/api/v1
```

> [!NOTE]  
> Todos os endpoints retornam dados no formato **JSON**.  
> A API não requer autenticação para consumo público.

<br/>

---

## 🔑 Fontes de Dados

A CYNE API agrega informações das seguintes fontes:

| Fonte | Descrição | Dados Fornecidos |
|:------|:----------|:-----------------|
| **[TMDb](https://www.themoviedb.org/)** | The Movie Database | Informações completas, trailers, elenco, imagens |
| **[OMDb](https://www.omdbapi.com/)** | Open Movie Database | Ratings IMDb, Metascore, prêmios |
| **VidSrc** | Player de Streaming | Links de embed para assistir |
| **WarezCDN** | Player de Streaming | Links alternativos de embed |
| **OpenSubtitles** | Legendas | Busca de legendas por IMDb ID |

<br/>

---

## 📡 Endpoints Disponíveis

<details open>
<summary><h3>1. Informações da API</h3></summary>

Retorna informações gerais sobre a API e endpoints disponíveis.

#### 📍 Endpoint
```
GET /
```

#### 📥 Resposta de Exemplo

```json
{
  "api_name": "CineAgregador API (Open Source Api Free Cinema)",
  "version": "1.0.0",
  "description": "API de agregação de dados de filmes e séries...",
  "endpoints": {
    "details": "/api/v1/details?id={tmdb_id|imdb_id}&type={movie|tv}",
    "search": "/api/v1/search?q={query_string}&page={number}",
    "random": "/api/v1/random?type={movie|tv}",
    "discover": "/api/v1/discover?genre={id}&year={year}",
    "poster": "/api/v1/details?id={tmdb_id}&type={movie}&field=poster_url"
  }
}
```

</details>

---

<details open>
<summary><h3>2. Detalhes de Mídia</h3></summary>

Obtém informações completas e agregadas de um filme ou série específico.

#### 📍 Endpoint
```
GET /api/v1/details
```

#### 📋 Parâmetros

| Parâmetro | Obrigatório | Tipo | Descrição |
|:----------|:------------|:-----|:----------|
| `id` | ✅ Sim | String/Number | ID do TMDb ou IMDb (formato `tt...`) |
| `type` | ✅ Sim | String | Tipo de mídia: `movie` ou `tv` |
| `field` | ❌ Não | String | Campo específico para filtrar (ex: `poster_url`, `cast`) |

#### 🎯 Exemplos de Requisição

```bash
# Filme usando IMDb ID
GET /api/v1/details?id=tt0133093&type=movie

# Série usando TMDb ID
GET /api/v1/details?id=62560&type=tv

# Filtrar apenas o poster
GET /api/v1/details?id=550&type=movie&field=poster_url
```

#### 📥 Resposta de Exemplo (Filme)

<details>
<summary><b>Clique para ver a resposta completa</b></summary>

```json
{
  "id": 550,
  "imdb_id": "tt0137523",
  "type": "movie",
  "title": "Clube da Luta",
  "original_title": "Fight Club",
  "year": "1999",
  "release_date": "1999-10-15",
  "sinopse": "Um funcionário de escritório insone e um fabricante de sabão...",
  "plot_omdb": "An insomniac office worker and a devil-may-care soap maker...",
  "tagline": "Mischief. Mayhem. Soap.",
  "poster_url": "https://image.tmdb.org/t/p/w500/pB8BM7pdSp6B6Ih7QZ4DrQ3PmJK.jpg",
  "backdrop_url": "https://image.tmdb.org/t/p/original/hZkgoQYus5vegHoetLkCJzb17zJ.jpg",
  "trailer_embed_url": "https://www.youtube.com/embed/SUXWAEX2jlg",
  "imdb_rating": "8.8",
  "tmdb_score": "84%",
  "metascore": "66",
  "total_votes": 28456,
  "genres": ["Drama"],
  "runtime": "139 min",
  "status": "Released",
  "language": "en",
  "director_or_creator": "David Fincher",
  "cast": [
    "Brad Pitt",
    "Edward Norton",
    "Helena Bonham Carter",
    "Meat Loaf",
    "Jared Leto"
  ],
  "player_embed": {
    "vidsrc": "https://vidsrc.xyz/embed/movie/tt0137523",
    "warezcdn": "https://embed.warezcdn.com/filme/tt0137523",
    "opensubtitles_search": "https://www.opensubtitles.org/en/search/imdbid-0137523"
  },
  "external_links": {
    "justwatch_search": "https://www.justwatch.com/br/busca?q=Fight%20Club",
    "google_search": "https://www.google.com/search?q=Fight+Club+onde+assistir+streaming"
  },
  "keywords": ["insomnia", "support group", "dual identity"],
  "tmdb_raw": {
    "recommendations": [...],
    "similar": [...]
  }
}
```

</details>

#### 📥 Resposta de Exemplo (Série)

<details>
<summary><b>Clique para ver a resposta completa</b></summary>

```json
{
  "id": 62560,
  "imdb_id": "tt4955642",
  "type": "tv",
  "title": "The Good Place",
  "original_title": "The Good Place",
  "year": "2016",
  "release_date": "2016-09-19",
  "sinopse": "Eleanor Shellstrop desperta no pós-vida e descobre...",
  "runtime": "25 min (por ep.)",
  "director_or_creator": "Michael Schur",
  "player_embed": {
    "vidsrc": "https://vidsrc.xyz/embed/tv?imdb=tt4955642&season={season}&episode={episode}",
    "warezcdn": "https://embed.warezcdn.com/serie/tt4955642/{season}/{episode}"
  }
}
```

</details>
</details>

> [!WARNING]  
> Para séries de TV, os links de embed requerem substituição manual dos parâmetros `{season}` e `{episode}`.

---



<details open>
<summary><h3>3. Busca de Mídia</h3></summary>

Realiza busca multi-mídia (filmes e séries) por título.

#### 📍 Endpoint
```
GET /api/v1/search
```

#### 📋 Parâmetros

| Parâmetro | Obrigatório | Tipo | Descrição |
|:----------|:------------|:-----|:----------|
| `q` | ✅ Sim | String | Query de busca (mínimo 2 caracteres) |
| `page` | ❌ Não | Number | Número da página (padrão: 1) |

#### 🎯 Exemplo de Requisição

```bash
GET /api/v1/search?q=matrix&page=1
```

#### 📥 Resposta de Exemplo

```json
{
  "query": "matrix",
  "page": 1,
  "total_results": 142,
  "total_pages": 8,
  "results": [
    {
      "id": 603,
      "type": "movie",
      "title": "Matrix",
      "year": 1999,
      "poster_url": "https://image.tmdb.org/t/p/w342/f89U3ADr1oiB1s9GkdPOEpXUk5H.jpg",
      "score": "82%"
    },
    {
      "id": 604,
      "type": "movie",
      "title": "Matrix Reloaded",
      "year": 2003,
      "poster_url": "https://image.tmdb.org/t/p/w342/9TGHDvWrqKBzwDxDodHYXEmOE6J.jpg",
      "score": "70%"
    }
  ]
}
```
</details>

> [!NOTE]  
> A busca filtra automaticamente apenas filmes e séries de TV, excluindo outros tipos de mídia.


---

<details open>
<summary><h3>4. Título Aleatório</h3></summary>

Retorna um título popular aleatório. Útil para a funcionalidade "Me Surpreenda".

#### 📍 Endpoint
```
GET /api/v1/random
```

#### 📋 Parâmetros

| Parâmetro | Obrigatório | Tipo | Descrição |
|:----------|:------------|:-----|:----------|
| `type` | ❌ Não | String | Tipo: `movie` ou `tv` (padrão: `movie`) |

#### 🎯 Exemplos de Requisição

```bash
# Filme aleatório
GET /api/v1/random

# Série aleatória
GET /api/v1/random?type=tv
```

#### 📥 Comportamento

Este endpoint **redireciona automaticamente** para `/api/v1/details` com um título popular aleatório, retornando todos os detalhes completos.
</details>

> [!TIP]  
> Perfeito para implementar features de "Descoberta Aleatória" ou "Sorte de Hoje".


---

<details open>
<summary><h3>5. Descobrir Títulos</h3></summary>

Descubra títulos com filtros avançados por gênero, ano e ordenação.

#### 📍 Endpoint
```
GET /api/v1/discover
```

#### 📋 Parâmetros

| Parâmetro | Obrigatório | Tipo | Descrição |
|:----------|:------------|:-----|:----------|
| `type` | ❌ Não | String | Tipo: `movie` ou `tv` (padrão: `movie`) |
| `genre` | ❌ Não | Number | ID do gênero (ex: 28 = Ação) |
| `year` | ❌ Não | Number | Ano de lançamento |
| `sort` | ❌ Não | String | Ordenação (padrão: `popularity.desc`) |
| `page` | ❌ Não | Number | Número da página (padrão: 1) |

#### 🎯 Exemplos de Requisição

```bash
# Filmes de ação de 2023
GET /api/v1/discover?genre=28&year=2023&type=movie

# Séries mais bem avaliadas
GET /api/v1/discover?type=tv&sort=vote_average.desc

# Filmes populares de drama
GET /api/v1/discover?genre=18&sort=popularity.desc
```

#### 📥 Resposta de Exemplo

```json
{
  "page": 1,
  "total_results": 9847,
  "total_pages": 493,
  "results": [
    {
      "id": 872585,
      "type": "movie",
      "title": "Oppenheimer",
      "year": 2023,
      "poster_url": "https://image.tmdb.org/t/p/w342/8Gxv8gSFCU0XGDykEGv7zR1n2ua.jpg",
      "score": "82%"
    }
  ]
}
```

#### 🎨 IDs de Gêneros Comuns

| ID | Gênero | ID | Gênero |
|:---|:-------|:---|:-------|
| 28 | Ação | 18 | Drama |
| 12 | Aventura | 27 | Terror |
| 16 | Animação | 10749 | Romance |
| 35 | Comédia | 878 | Ficção Científica |
| 80 | Crime | 53 | Thriller |
</details>
<br/> 

> [!TIP]  
> Veja a lista completa de gêneros em: [TMDb Genres](https://api.themoviedb.org/3/genre/movie/list?api_key=YOUR_KEY)


---

## 💡 Exemplos de Uso

### JavaScript (Fetch API)

```javascript
// Buscar detalhes de um filme
async function getMovieDetails(imdbId) {
  const response = await fetch(
    `http://bore.pub:15776/api/v1/details?id=${imdbId}&type=movie`
  );
  const data = await response.json();
  console.log(data);
}

// Buscar títulos
async function searchMovies(query) {
  const response = await fetch(
    `http://bore.pub:15776/api/v1/search?q=${encodeURIComponent(query)}`
  );
  const data = await response.json();
  return data.results;
}

// Título aleatório
async function getRandomMovie() {
  const response = await fetch('http://bore.pub:15776/api/v1/random?type=movie');
  const data = await response.json();
  return data;
}
```

### Python (Requests)

```python
import requests

# Buscar detalhes
def get_movie_details(tmdb_id):
    url = f"http://bore.pub:15776/api/v1/details"
    params = {"id": tmdb_id, "type": "movie"}
    response = requests.get(url, params=params)
    return response.json()

# Buscar títulos
def search_movies(query):
    url = f"http://bore.pub:15776/api/v1/search"
    params = {"q": query, "page": 1}
    response = requests.get(url, params=params)
    return response.json()
```

### cURL

```bash
# Detalhes de um filme
curl "http://bore.pub:15776/api/v1/details?id=tt0133093&type=movie"

# Busca
curl "http://bore.pub:15776/api/v1/search?q=inception&page=1"

# Título aleatório
curl "http://bore.pub:15776/api/v1/random?type=tv"
```

<br/>

---

## ⚠️ Limites e Restrições

> [!WARNING]  
> **Política de Uso Justo**

- 🚫 **Sem Rate Limit Oficial**: Mas recomendamos máximo de 60 requisições/minuto
- ⏱️ **Timeout**: As requisições têm timeout de 15 segundos
- 🔄 **Retry Automático**: O sistema tenta até 4 vezes automaticamente
- 📊 **Uso Comercial**: Consulte a licença CC BY-NC 4.0

> [!NOTE]  
> A API é fornecida "como está", sem garantias de uptime 100%.  
> Para uso em produção, considere implementar cache e tratamento de erros robusto.

### 🔴 Códigos de Erro Comuns

| Código | Significado | Ação Recomendada |
|:-------|:------------|:-----------------|
| 400 | Parâmetros inválidos | Verifique a documentação |
| 404 | Título não encontrado | Confirme o ID e tipo |
| 503 | Serviço temporariamente indisponível | Aguarde alguns segundos e tente novamente |

<br/>

---

## Status

A API está atualmente na **Fase BETA**. O serviço está ativo e funcional para fins de teste e demonstração.

>[!NOTE]
>Para monitoramento em tempo real da saúde da API, latência e o status das dependências externas (TMDb, OMDb), consulte a nossa [Página de Status (BETA)](http://bore.pub:15776/status).

---

## 🛠️ Tecnologias

A CYNE API foi construída com:

- **[Node.js](https://nodejs.org/)** - Runtime JavaScript
- **[Express.js](https://expressjs.com/)** - Framework web minimalista
- **[node-fetch](https://github.com/node-fetch/node-fetch)** - Cliente HTTP
- **[TMDb API](https://developers.themoviedb.org/)** - Dados de filmes e séries
- **[OMDb API](https://www.omdbapi.com/)** - Ratings e metadados
- **Claude AI** - Assistência no desenvolvimento

<br/>

---

## 📜 Licença

> [!WARNING]  
> Este projeto está licenciado sob a **Creative Commons Attribution-NonCommercial 4.0 International (CC BY-NC 4.0)**.

### ✅ Você PODE:
- ✔️ Usar a API gratuitamente para projetos pessoais
- ✔️ Modificar e adaptar para suas necessidades
- ✔️ Compartilhar com atribuição de créditos

### ❌ Você NÃO PODE:
- ❌ Usar para fins comerciais sem permissão
- ❌ Remover os créditos do criador
- ❌ Revender ou monetizar diretamente a API

📄 Consulte o arquivo [LICENSE](./LICENSE.md) para mais detalhes.

<br/>

---

## Créditos

<div align="center">
<a href="https://github.com/JempUnkn" aria-label="Repositório JEMP">
  <img src="https://raw.githubusercontent.com/JempUnkn/webtv-beta/main/app-icon.png" width="154" alt="JempUnkn">
</a>
<br>
<sub><strong>Created by <a href="https://github.com/JempUnkn">JEMP</a></strong></sub><br>
<sub>Powered by <strong>Claude AI</strong> (Anthropic)</sub><br>
<sub>© 2025 <strong>JEMP</strong> — Todos os direitos reservados.</sub>
</div>

<br/>

---

<div align="center">

### 🌟 Se esta API foi útil, considere dar uma ⭐ no repositório!

**[⬆ Voltar ao topo](#-cyne---cineagregador-api)**

</div>



