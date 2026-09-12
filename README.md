# Conferência de Insumos — Delícias DuFeDaJu

App de uma página para o controle diário de insumos da loja. Sem servidor, sem banco de
dados, sem login. Tudo o que ele guarda fica no próprio aparelho.

## Como o ciclo funciona

```
Confere no celular  →  copia e cola no grupo  →  quem compra abre o link da mensagem
                              ↑                              ↓
                    devolutiva no grupo   ←   marca o que comprou
```

O mesmo `index.html` atende os dois lados. O que decide é o endereço:

| Endereço | Quem usa | O que aparece |
|---|---|---|
| `https://usuario.github.io/insumos-delicias/` | Quem confere | Acabou / Acabando / Tem |
| `.../insumos-delicias/#c=...` | Quem compra | Não achei / Não comprei / Comprei |

A lista viaja codificada dentro do próprio link (7 itens ≈ 190 caracteres). Nada é gravado
na internet.

## Publicar no GitHub Pages

1. Crie um repositório novo, por exemplo `insumos-delicias`. Marque **Public** — no plano
   gratuito o Pages só publica a partir de repositório público.
2. Suba os arquivos desta pasta na raiz, sem subpasta. Inclua o `.nojekyll`.
3. No repositório: **Settings** → **Pages**.
4. Em **Source**, escolha **Deploy from a branch**. Branch: `main`, pasta: `/ (root)`. **Save**.
5. Aguarde um ou dois minutos e recarregue a página. O endereço aparece no topo.

A partir daí, todo commit republica o app automaticamente.

### Outras hospedagens

**Cloudflare Pages** — se você precisar do repositório privado. Gratuito, aceita repositório
privado e permite uso comercial. Conta nova, deploy a cada commit.

**Vercel** — atenção: o plano Hobby é restrito a uso pessoal e não comercial, conforme a
documentação deles. Um app de estabelecimento comercial acaba caindo na exigência de
upgrade para o Pro.

## Depois de publicado

1. Abra o endereço no celular de quem faz a conferência.
2. Abra **Ajustes** (a engrenagem). O campo **Endereço do app** já vem preenchido — é ele que
   monta o link de quem compra. Confira o nome de quem confere e o nome do grupo.
3. No Chrome, menu ⋮ → **Adicionar à tela inicial**. Vira ícone e abre sem barra de navegador.
4. O mesmo vale para o outro celular, se quiser o atalho. Não é obrigatório: o link da
   mensagem abre direto.

## Arquivos

| Arquivo | Para que serve |
|---|---|
| `index.html` | O app inteiro: HTML, CSS e JavaScript num arquivo só |
| `manifest.webmanifest` | Faz o app abrir em tela cheia quando adicionado à tela inicial |
| `icon-192.png`, `icon-512.png` | Ícone na tela inicial do Android |
| `apple-touch-icon.png` | Ícone na tela inicial do iPhone |
| `.nojekyll` | Impede o GitHub Pages de processar os arquivos antes de publicar |

## Mexer na lista de itens

O caminho normal é pela própria tela: **Ajustes → Editar itens da lista**. Tem um `×` para
remover e um campo por categoria para adicionar. O que você mexe ali vale só naquele aparelho.

Para mudar a lista **de fábrica** (a que vale para qualquer aparelho novo), edite a constante
`CATALOGO` no `index.html`. Ela fica logo no começo do `<script>`, agrupada por categoria:

```js
["Hortifruti", [
  ["Alface","un"],["Tomate","kg"],["Cebola","kg"], ...
]],
```

Item novo ganha emoji sozinho por palavra-chave. Para escolher o emoji na mão, adicione o
nome exato no objeto `EMOJI`, logo abaixo.

**Cuidado ao reordenar:** o link de compra referencia os itens pela posição na lista. Se você
apagar ou trocar itens de lugar, um link gerado antes da mudança vai apontar para o item
errado. Como a lista é refeita todo dia, basta não mexer no meio de uma conferência em aberto.

## Por que copiar e colar, e não um botão de WhatsApp

Testado e descartado. Emoji ocupa 4 bytes, e o repasse do texto por um link `wa.me` quebra
esses caracteres — chega `&#65533;` no lugar de cada um. Acentos passam porque ocupam 2 bytes.
A área de transferência não passa por URL e preserva tudo. Por isso os dois lados usam
**Copiar mensagem → abrir o grupo → colar**.

## Onde ficam os dados

No `localStorage` de cada aparelho, separado por lado:

- Quem confere: marcações, itens extras, nome, grupo e endereço do app.
- Quem compra: as marcações daquela lista. Se fechar o navegador no mercado, reabrir o link
  continua de onde parou.

Limpar os dados do site apaga tudo isso. A lista de fábrica volta intacta.
