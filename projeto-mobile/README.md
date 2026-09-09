# Protótipo Mobile — BlogMob

**Disciplina:** Desenvolvimento Mobile
**Atividade:** Protótipo Mobile e Organização dos Estilos com BEM
**Data de entrega:** 07/09/2026
---

## 1. Integrantes do grupo
| Felipe Soares | 824156311
| Thiago Ferreira | 824156179 
| Gabriel Fornicola Amorim | 824148690 
| Matheus Tognon| 000000 

---

## 2. Descrição da aplicação

O **BlogMob** é uma plataforma de publicação de conteúdo (blog) com três grandes
áreas:

- **Área pública** — leitura de postagens organizadas por categorias, destaques,
  busca e assinatura de newsletter.
- **Área do usuário** — login, cadastro e perfil, onde o usuário acompanha suas
  postagens (em diferentes estados) e comentários.
- **Área administrativa** — gestão de categorias, criação de postagens, escolhas
  do editor, gerenciamento de usuários, fila de revisão e moderação de
  comentários.

O objetivo desta etapa **não é** produzir a interface visual final, mas
representar a **estrutura**, a **hierarquia da informação** e os **componentes
recorrentes** adequados ao contexto mobile.

---

## 3. Relação das telas elaboradas

As 14 telas estão na pasta [`/wireframes`](./wireframes). Abra
[`index.html`](./index.html) para navegar por todas de forma agrupada por fluxo.

| ID | Tela | Descrição |
|----|------|-----------|
| tela_01 | Home | Página inicial: categorias populares, destaques e escolhas do editor |
| tela_02 | Categoria | Postagens de uma categoria selecionada, com filtros e "carregar mais" |
| tela_03 | Destaques | Postagens classificadas como destaque |
| tela_04 | Newsletter | Assinatura por e-mail com aceite das condições |
| tela_05 | Admin · Categorias | Manutenção de categorias (editar/excluir) |
| tela_06 | Admin · Criar post | Formulário de nova postagem (rascunho/revisão/publicar) |
| tela_07 | Admin · Escolhas do editor | Seleção de postagens em destaque |
| tela_08 | Admin · Usuários | Gerenciamento e controle de acesso (bloquear/desbloquear) |
| tela_09 | Admin · Fila de revisão | Aprovação/reprovação de postagens enviadas |
| tela_10 | Admin · Comentários | Moderação de comentários pendentes |
| tela_11 | Busca | Resultados da pesquisa (imagem, título, categoria e data) |
| tela_12 | Login | Autenticação do usuário |
| tela_13 | Cadastro | Criação de conta com aceite dos termos |
| tela_14 | Perfil | Dados do usuário, postagens (com estados) e comentários |

---

## 4. Fluxo de navegação

As telas fazem parte de uma mesma aplicação e se relacionam em três fluxos:

```
FLUXO PÚBLICO
  tela_01 (Home)
    ├── categoria selecionada ──> tela_02 (Categoria)
    ├── menu "Destaques" ───────> tela_03 (Destaques)
    ├── "Assinar" / botão ──────> tela_04 (Newsletter)
    └── campo Buscar ───────────> tela_11 (Busca)

FLUXO DE AUTENTICAÇÃO
  tela_12 (Login) ──"Criar conta"──> tela_13 (Cadastro) ──autenticado──> tela_14 (Perfil)

FLUXO ADMINISTRATIVO
  tela_05 · tela_06 · tela_07 · tela_08 · tela_09 · tela_10
  Produção de conteúdo:
    tela_06 (Criar post) ──> tela_09 (Fila de revisão) ──aprovação──> publicação (aparece na tela_01/tela_03)
```

---

## 5. Componentes identificados e variações

A identificação considera que os elementos serão transformados em **componentes
React reutilizáveis**. Um mesmo elemento visual usado em telas diferentes é
tratado como **um componente com variações**, e não como estruturas novas.

| Componente | Onde aparece | Variações (modificadores BEM) |
|------------|--------------|-------------------------------|
| **Header** | Todas as telas | `--admin` |
| **Nav** | Telas públicas e admin | barra inferior (padrão), `--side` (admin), item `--active` |
| **Search** | Home, Busca, Admin | `--in-header`, `--field` |
| **Section** | Home, Destaques, Perfil | padrão (título + ação) |
| **Card** | Home, Categoria, Destaques, Busca | `--featured`, `--compact` |
| **Button** | Login, Newsletter, Admin, Perfil | `--primary`, `--secondary`, `--disabled`, `--danger`, `--ghost`, `--block`, `--small` |
| **Form** | Login, Cadastro, Newsletter, Criar post | `--login`, `--signup`, `--newsletter` |
| **List** | Categorias, Usuários, Revisão, Comentários, Perfil | `--divided` |
| **Badge** | Perfil, Usuários, Revisão, Comentários | `--draft`, `--published`, `--review`, `--pending`, `--blocked` |
| **Tag** | Categorias populares, filtros, tags de post | `--selected`, `--outline` |
| **Stats** | Telas administrativas | padrão (grade de indicadores) |
| **Alert** | Escolhas, Revisão, Comentários | `--info`, `--success`, `--warning` |

### Exemplo — variações do **Card**

```
.card               → padrão  (imagem no topo + conteúdo)
.card--featured     → destaque (maior, com selo "Destaque")
.card--compact      → compacto (imagem lateral, para listas densas)
```

### Exemplo — variações do **Button**

```
.button              → base
.button--primary     → ação principal (preenchido)
.button--secondary   → ação secundária (contorno)
.button--disabled    → indisponível
```

---

## 6. Organização dos estilos (BEM)

Os estilos usam o padrão **BEM — Block, Element, Modifier**:

- **Bloco** — componente independente: `.card`, `.button`, `.form`
- **Elemento** — parte do bloco: `.card__title`, `.form__input`, `.nav__item`
- **Modificador** — variação: `.card--featured`, `.button--primary`, `.nav__item--active`

Cada arquivo CSS tem **uma única responsabilidade (SRP)**, evitando concentrar
tudo em um só arquivo. Isso facilita a localização das regras, a manutenção e o
reaproveitamento na futura componentização em React.

---

## 7. Organização dos arquivos

```
projeto-mobile/
├── index.html                 # Índice navegável das 14 telas (agrupado por fluxo)
├── README.md                  # Este documento
├── css/
│   ├── main.css               # Ponto de entrada: importa todos os arquivos (na ordem)
│   ├── variables.css          # Tokens (cores, espaçamentos, tipografia)
│   ├── base.css               # Reset + moldura mobile (.screen)
│   ├── header.css             # Bloco: header
│   ├── navigation.css         # Bloco: nav (inferior, lateral, menu)
│   ├── search.css             # Bloco: search (campo de busca)
│   ├── section.css            # Bloco: section (títulos de seção)
│   ├── card.css               # Bloco: card (+ variações)
│   ├── button.css             # Bloco: button (+ variações)
│   ├── form.css               # Bloco: form (campos + variações)
│   ├── list.css               # Bloco: list (listagens administrativas)
│   ├── badge.css              # Bloco: badge (estados)
│   ├── tag.css                # Bloco: tag (categorias/filtros)
│   ├── stats.css              # Bloco: stats (indicadores admin)
│   └── alert.css              # Bloco: alert (mensagens)
└── wireframes/
    ├── tela_01_home.html
    ├── tela_02_categoria.html
    ├── tela_03_destaques.html
    ├── tela_04_newsletter.html
    ├── tela_05_admin_categorias.html
    ├── tela_06_criar_post.html
    ├── tela_07_admin_escolhas.html
    ├── tela_08_admin_usuarios.html
    ├── tela_09_fila_revisao.html
    ├── tela_10_fila_comentarios.html
    ├── tela_11_busca.html
    ├── tela_12_login.html
    ├── tela_13_cadastro.html
    └── tela_14_perfil.html
```

> Correspondência direta: cada **componente identificado** na seção 5 possui um
> **arquivo CSS** próprio na pasta `css/`, com as classes BEM correspondentes.

---

## 8. Como visualizar

O protótipo é feito apenas com **HTML e CSS** (sem dependências).

1. Abra o arquivo [`index.html`](./index.html) diretamente no navegador **ou**
2. Sirva a pasta com um servidor local, por exemplo:

```bash
python -m http.server 8000
```

- **Link/referência para as telas:** pasta [`/wireframes`](./wireframes) —
  cada arquivo `tela_XX_*.html` corresponde a uma das 14 telas.

---
