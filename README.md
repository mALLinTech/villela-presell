# Villela Store — Presell Meta Ads

Páginas-ponte entre anúncios do Meta Ads e produtos/seções específicas do ecommerce `villelastore01.com.br`.

Cada anúncio do Meta tem **uma** página de presell dedicada. A presell confirma a idade (+18), dispara os eventos do Pixel e redireciona o cliente para a URL final no Nuvemshop.

---

## Estrutura do projeto

```
/
├── index.html                  → página principal (home do ecommerce)
├── logo.png                    → logo compartilhado por TODAS as páginas
├── README.md                   → este arquivo
├── _template/
│   └── index.html              → MODELO. Sempre duplique daqui.
├── melzinho-do-amor/
│   └── index.html              → produto Melzinho do Amor
├── bubble/
│   └── index.html              → produto Bubble Vibes
└── sugalik/
    └── index.html              → produto Sugalik
```

URLs públicas resultantes (depois de subir no hosting):

- `https://seusite.com/` → home do ecommerce
- `https://seusite.com/melzinho-do-amor/` → produto Melzinho do Amor
- `https://seusite.com/bubble/` → produto Bubble Vibes
- `https://seusite.com/<slug>/` → qualquer página nova que criar

---

## Regras gerais (não mudam entre páginas)

| Item                  | Valor                                                                   |
|-----------------------|-------------------------------------------------------------------------|
| Pixel ID (Meta)       | `1299154598705766`                                                      |
| WhatsApp              | `5516994002350` com mensagem `Olá, pode me ajudar?`                     |
| Delay antes do redirect | `300ms` (garante que o pixel registre o evento antes da navegação)    |
| utm_source            | `metaads`                                                               |
| utm_medium            | `presell`                                                               |
| utm_campaign          | **muda por página** → `presell_<slug>` (ver tabela de exemplos abaixo)  |
| Design / layout       | **idêntico em todas** — só muda o link de destino e os identificadores  |

---

## Decisões já tomadas (e por quê)

1. **Criar páginas por demanda, não em massa.** Só crie uma página quando já tiver o criativo do anúncio pronto. Catálogo do Nuvemshop muda, e páginas órfãs viram trabalho de manutenção.
2. **Páginas de produto > páginas de categoria.** Quando o anúncio mostra um produto específico, leve o cliente direto para o produto. Conversão tende a ser maior do que despejar em uma listagem.
3. **Sempre manter o botão do WhatsApp.** Mesmo número e mesma mensagem em todas as páginas — é canal de atendimento único.
4. **CSS e JS inline em cada página.** Mantém cada página autossuficiente; uma quebrar não afeta as outras. Só vale extrair para `assets/` compartilhado se passar de ~10 páginas.
5. **Logo é único na raiz** (`/logo.png`). Cada subpasta referencia via `../logo.png` para não duplicar a imagem.

---

## Passo a passo para criar uma página nova

### 1. Definir o slug

O slug é o nome da pasta e da URL pública. Regras:

- Tudo minúsculo
- Sem acento
- Espaços viram `-` (hífen)
- Curto e descritivo (lembra o produto/categoria)

**Exemplos:**

| Produto no Nuvemshop                                                | Slug escolhido       |
|---------------------------------------------------------------------|----------------------|
| Exotic Honey Melzinho do Amor                                       | `melzinho-do-amor`   |
| Bubble Vibes Gel Deslizante                                         | `bubble`             |
| Lingerie Sexy Fantasy                                               | `lingerie-fantasy`   |
| Categoria geral de Vibradores                                       | `vibradores`         |

### 2. Duplicar o template

```bash
cp -r _template <slug>
```

Ou no Finder: copie a pasta `_template/` e renomeie a cópia para o slug.

### 3. Editar 4 pontos no `<slug>/index.html`

Todos os pontos estão marcados com `[EDITAR 1/4]` ... `[EDITAR 4/4]` no `_template/index.html`. Você só precisa substituir os placeholders.

| #  | Onde no arquivo                              | Substituir                  | Por (exemplo Melzinho)                                              |
|----|----------------------------------------------|-----------------------------|---------------------------------------------------------------------|
| 1  | `<title>` (head)                             | (opcional)                  | manter `Villela Store - Acesso Restrito`                            |
| 2a | `href` do `#btn-site`                        | `URL_DA_CATEGORIA_AQUI`     | URL completa do produto no Nuvemshop                                |
| 2b | `href` do `#btn-site`                        | `presell_CATEGORIA`         | `presell_melzinho_do_amor` (use underscore no utm)                  |
| 3  | `fbq('track', 'ViewContent', ...)`           | `NOME_DA_CATEGORIA`         | `Melzinho do Amor` (nome legível, espaços OK)                       |
| 4  | `fbq('track', 'Contact', ...)`               | `NOME_DA_CATEGORIA`         | `Melzinho do Amor` (mesmo nome do passo 3)                          |

**Padrão de URL final montada no `href`:**

```
<URL_DO_PRODUTO>?utm_source=metaads&utm_medium=presell&utm_campaign=presell_<slug_com_underscore>
```

Note: no `utm_campaign` use **underscore** (`presell_melzinho_do_amor`), mesmo que o slug da pasta use **hífen** (`melzinho-do-amor`). Isso é uma convenção comum em UTMs.

### 4. Testar localmente

Abra o `index.html` da nova pasta no navegador e:

- Confira se o logo aparece
- Abra o console (F12 → Console)
- Clique no botão principal
- Verifique a mensagem `Evento ViewContent disparado` no console
- Confira se o navegador foi para a URL certa do produto no Nuvemshop

### 5. Subir no hosting

Sobe a pasta nova para o servidor (mesmo lugar onde está o `index.html` da home). A URL `seusite.com/<slug>/` já funciona.

---

## Exemplos reais (referência rápida)

### Melzinho do Amor

```
Pasta:          melzinho-do-amor/
URL pública:    https://seusite.com/melzinho-do-amor/
Destino:        https://villelastore01.com.br/produtos/exotic-honey-melzinho-do-amor-kits-masculino-feminino-sexy-fantasy/
utm_campaign:   presell_melzinho_do_amor
content_name:   Melzinho do Amor
```

### Bubble Vibes

```
Pasta:          bubble/
URL pública:    https://seusite.com/bubble/
Destino:        https://villelastore01.com.br/produtos/bubble-vibes-gel-deslizante-microesferas-chiclete/
utm_campaign:   presell_bubble
content_name:   Bubble Vibes
```

### Sugalik

```
Pasta:          sugalik/
URL pública:    https://seusite.com/sugalik/
Destino:        https://villelastore01.com.br/produtos/sugalik-gel-sugador-liquido-15g-sabores-feiticos2/
utm_campaign:   presell_sugalik
content_name:   Sugalik
```

Quando criar uma página nova, **adicione uma linha aqui** para manter a referência centralizada.

---

## Checklist antes de subir

- [ ] Pasta nomeada com slug em minúsculo, sem acento, com hífen
- [ ] URL de destino do Nuvemshop testada (abre o produto certo)
- [ ] `utm_campaign` único, com prefixo `presell_` e underscore
- [ ] `content_name` do `ViewContent` preenchido (não esquecer — sem ele fica difícil separar conversões no Meta)
- [ ] `content_name` do `Contact` (WhatsApp) preenchido com o mesmo nome
- [ ] Logo aparece (`../logo.png` resolve para `/logo.png` na raiz)
- [ ] Clique testado em ambiente real: console mostra evento + redirect funciona
- [ ] Pixel ID continua `1299154598705766` (não mudar por engano)
- [ ] WhatsApp continua `5516994002350` (não mudar por engano)

---

## Quando NÃO criar uma página nova

- Anúncio aponta para a home → use a `/index.html` que já existe
- Mesmo produto que outro anúncio já tem página → use a mesma URL e mude só o criativo no Meta
- Teste de criativo (sem definição clara do destino) → use a home com `utm_campaign` diferente

---

## Quando precisar mudar algo em TODAS as páginas

Se algum dia você precisar alterar logo, pixel ID, número do WhatsApp ou texto padrão, vai precisar editar **arquivo por arquivo** (essa é a contrapartida de manter tudo inline e autossuficiente).

Se isso virar problema (ex: mais de 10 páginas para manter), me chama que a gente extrai CSS/JS para `assets/` compartilhado e reduz o trabalho.
