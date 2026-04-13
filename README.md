<div align="center">
<img src="https://capsule-render.vercel.app/api?type=venom&height=300&color=gradient&text=CYNE%20API&textBg=false&desc=API%20de%20Agregação%20de%20Dados%20de%20Filmes%20e%20Séries&animation=fadeIn" align="center" style="width: 100%" />

---

</div>

<p align="center">
  <img src="https://img.shields.io/badge/Status-Online-brightgreen?style=for-the-badge" alt="Status">
  <img src="https://img.shields.io/badge/Versão-1.2.0-blue?style=for-the-badge" alt="Versão">
  <img src="https://img.shields.io/badge/Consumo-Gratuito-orange?style=for-the-badge" alt="Consumo">
  <a href="https://creativecommons.org/licenses/by-nc/4.0/">
    <img src="https://img.shields.io/badge/License-CC%20BY--NC%204.0-blue.svg?style=for-the-badge" alt="License: CC BY-NC 4.0">
  </a>
  <img src="https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white" alt="Node.js">
</p>

<br/>

# CYNE API Gateway — v1

Gateway REST, servindo como proxy/wrapper das APIs TMDB e OMDb com endpoints extras e geração de URLs de embed.

---

## 📡 Endpoints

### `GET /api/v1`
Status, créditos e lista de endpoints.

---

### `GET /api/v1/search`
| Param      | Tipo   | Default  | Descrição |
|------------|--------|----------|-----------|
| `q`        | string | **req.** | Texto de busca |
| `type`     | string | `multi`  | `multi` \| `movie` \| `tv` \| `person` |
| `year`     | number | —        | Filtro de ano |
| `language` | string | `pt-BR`  | Idioma do TMDB |
| `page`     | number | `1`      | Página |
| `sort_by`  | string | —        | `popularity.desc` \| `vote_average.desc` \| `release_date.desc` |
| `adult`    | bool   | `false`  | Inclui conteúdo adulto |
| `region`   | string | `BR`     | Região |
| `field`    | string | —        | Retorna apenas um campo de cada resultado |

```
/api/v1/search?q=matrix&type=movie&sort_by=vote_average.desc
/api/v1/search?q=attack on titan&type=tv&year=2013
/api/v1/search?q=inception&field=poster_path
```

---

### `GET /api/v1/id/[id]`
Busca por TMDB ID (número) ou IMDB ID (`ttXXXXX`). Detecta automaticamente filme vs série.

| Param      | Tipo   | Default  | Descrição |
|------------|--------|----------|-----------|
| `type`     | string | `movie`  | `movie` \| `tv` — usado só se ID for TMDB |
| `language` | string | `pt-BR`  | Idioma |
| `field`    | string | —        | Retorna **um** campo ex: `?field=title` |
| `fields`   | string | —        | Retorna múltiplos campos ex: `?fields=title,poster_path,vote_average` |
| `append`   | string | —        | `append_to_response` do TMDB ex: `?append=credits,videos,keywords` |
| `omdb`     | bool   | `false`  | Enriquece com ratings do OMDb (IMDb rating, Rotten Tomatoes, etc) |

```
/api/v1/id/550                        → Fight Club (TMDB)
/api/v1/id/tt0137523                  → Fight Club (IMDB) — detecta sozinho
/api/v1/id/1396?type=tv               → Breaking Bad
/api/v1/id/550?field=title            → { "title": "Fight Club" }
/api/v1/id/550?fields=title,poster_path,vote_average
/api/v1/id/550?append=credits,videos
/api/v1/id/tt0137523?omdb=true        → inclui _omdb com ratings extras
```

---

### `GET /api/v1/id/[id]/similar`
| Param      | Tipo   | Default    | Descrição |
|------------|--------|------------|-----------|
| `type`     | string | `movie`    | `movie` \| `tv` |
| `language` | string | `pt-BR`    | Idioma |
| `page`     | number | `1`        | Página |
| `mode`     | string | `similar`  | `similar` \| `recommendations` |
| `field`    | string | —          | Filtra campo |

```
/api/v1/id/550/similar?type=movie
/api/v1/id/1396/similar?type=tv&mode=recommendations
```

---

### `GET /api/v1/trending`
| Param      | Tipo   | Default | Descrição |
|------------|--------|---------|-----------|
| `media`    | string | `all`   | `all` \| `movie` \| `tv` \| `person` |
| `window`   | string | `week`  | `day` \| `week` |
| `language` | string | `pt-BR` | Idioma |
| `page`     | number | `1`     | Página |
| `field`    | string | —       | Filtra campo |

```
/api/v1/trending
/api/v1/trending?media=movie&window=day
/api/v1/trending?field=poster_path
```

---

### `GET /api/v1/discover`
Todos os filtros do endpoint `/discover` do TMDB.

| Param          | Tipo   | Default          | Descrição |
|----------------|--------|------------------|-----------|
| `media`        | string | `movie`          | `movie` \| `tv` |
| `sort_by`      | string | `popularity.desc`| Ex: `vote_average.desc` |
| `genre`        | string | —                | ID(s) de género ex: `28,12` |
| `year`         | number | —                | Ano exato |
| `year_gte`     | number | —                | Ano mínimo |
| `year_lte`     | number | —                | Ano máximo |
| `vote_gte`     | number | —                | Nota mínima |
| `votes_gte`    | number | —                | Mínimo de votos |
| `runtime_gte`  | number | —                | Duração mínima (min) |
| `runtime_lte`  | number | —                | Duração máxima (min) |
| `keyword`      | string | —                | Keyword ID do TMDB |
| `with_networks`| string | —                | ID de rede (TV) ex: Netflix=213 |
| `language`     | string | `pt-BR`          | Idioma |
| `page`         | number | `1`              | Página |

```
/api/v1/discover?media=movie&genre=28&year_gte=2020&vote_gte=7
/api/v1/discover?media=tv&with_networks=213&sort_by=vote_average.desc
/api/v1/discover?media=movie&runtime_lte=90&sort_by=popularity.desc
```

---

### `GET /api/v1/genres`
| Param      | Tipo   | Default | Descrição |
|------------|--------|---------|-----------|
| `media`    | string | `both`  | `movie` \| `tv` \| `both` |
| `language` | string | `pt-BR` | Idioma |

---

### `GET /api/v1/nowplaying`
Filmes em cartaz.  
Params: `language`, `region`, `page`, `field`

---

### `GET /api/v1/toprated`
Mais bem avaliados.  
Params: `media` (`movie`\|`tv`), `language`, `region`, `page`, `field`

---

### `GET /api/v1/anime`
Animes em alta — usa keyword `210024` + genre `16` (lógica extraída do CYNEBLACK).

| Param            | Tipo   | Default          | Descrição |
|------------------|--------|------------------|-----------|
| `language`       | string | `pt-BR`          | Idioma |
| `page`           | number | `1`              | Página |
| `sort_by`        | string | `popularity.desc`| Ordenação |
| `include_movies` | bool   | `false`          | Inclui filmes anime |
| `field`          | string | —                | Filtra campo |

---

### `GET /api/v1/embed/[tmdb_id]`
Gera URL de embed para qualquer servidor de streaming.

| Param     | Tipo   | Default      | Descrição |
|-----------|--------|--------------|-----------|
| `type`    | string | `movie`      | `movie` \| `tv` |
| `server`  | string | `multiembed` | Ver lista abaixo |
| `season`  | number | `1`          | Temporada (TV) |
| `episode` | number | `1`          | Episódio (TV) |
| `lang`    | string | `pt`         | Código de idioma 2 letras |
| `imdb`    | string | —            | IMDB ID manual (opcional) |
| `all`     | bool   | `false`      | Retorna URLs de todos os servidores |

```
/api/v1/embed/550?server=vidsrc&type=movie
/api/v1/embed/1396?server=warezcdn&type=tv&season=2&episode=1
/api/v1/embed/37854?server=gstream&type=tv        ← anime (One Piece)
/api/v1/embed/550?all=true                         ← todos os servidores
```

---

### `GET /api/v1/servers`
Lista todos os servidores com metadados: se precisam de IMDB ID, suporte a filmes/TV, etc.

---

## 🎬 Servidores Suportados

| ID           | Label       | IMDB necessário | Anime |
|--------------|-------------|-----------------|-------|
| `vidsrc`     | VidSRC      | ✅               | —    |
| `warezcdn`   | WarezCdn    | ✅               | —    |
| `gstream`    | GStream     | ⚠️ só filmes    | ✅   |
| `multiembed` | MultiEmbed  | ❌               | —    |
| `playerflix` | PlayerFlix  | ❌               | —    |
| `superflix`  | SuperFlix   | ✅ só filmes    | —    |

---

## 🔑 IDs de Géneros Úteis (TMDB)

| ID | Nome |
|----|------|
| 28 | Ação |
| 12 | Aventura |
| 16 | Animação / Anime |
| 35 | Comédia |
| 80 | Crime |
| 18 | Drama |
| 14 | Fantasia |
| 27 | Terror |
| 9648 | Mistério |
| 878 | Ficção Científica |
| 53 | Suspense |

## 🌐 Redes (TV) Úteis

| ID | Rede |
|----|------|
| 213 | Netflix |
| 1024 | Amazon Prime |
| 2739 | Disney+ |
| 2552 | Apple TV+ |
| 49 | HBO |
| 4330 | Crunchyroll |

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
<sub>© 2025-26 <strong>JEMP — CYNE-API Gateway v1.2.0</strong> — Todos os direitos reservados.</sub>
</div>

<br/>

---

<div align="center">

### 🌟 Se esta API foi útil, considere dar uma ⭐ no repositório!

**[⬆ Voltar ao topo](#-cyne---cineagregador-api)**

</div>





