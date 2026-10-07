# Handoff: cartões das famílias, capa de Pantones (R2 Hub 1.0)

## O que é
Na capa da página **Pantones**, à direita do painel da busca: oito cartões em pé, um por família de cor
(Amarelos, Laranjas, Vermelhos, Rosas e roxos, Azuis, Verdes, Marrons, Neutros). Cada cartão mostra quantas
cores a família tem, o nome girado e uma seta. Sete ficam à vista; o oitavo (Neutros) chega pela rolagem.
O clique abre o painel da família (lista à esquerda, a cor escolhida à direita), que **não** faz parte deste
handoff. Também ficam de fora o painel da busca (x 80 a 720), a barra do topo e o pé da página.

## Arquivos
- `familias.html`: referência navegável, abre direto no navegador. Tem o hover, o foco pelo teclado, a
  rolagem com encaixe e a entrada em escada com mola. É HTML e CSS soltos para mostrar aparência e
  comportamento, **não** o código do hub (o hub é React + TypeScript).
- `capturas/`: o hub de teste em 07/10/2026, recortado na trilha (x 690 a 1890, y 110 a 910 do palco):
  `familias-repouso.png` (e `@2x`), `familias-hover.png` (mouse nos Vermelhos), `familias-rolada.png` (um giro
  da roda: entra Neutros) e `pagina-inteira.png`. `handoff-repouso.png` é o `familias.html` na mesma moldura,
  para comparar (`comparar-hub-handoff.png` põe os dois lado a lado: diferem em 0,02% dos pixels, nas bordas
  das letras).
- `assets/seta-rolar.json`: a animação Lottie da dica de rolar (a mesma de todas as trilhas do hub).
- No código do hub: `src/components/hub10/PantonesHub10.tsx` (`FAIXAS`, os cartões), `CapaV4Hub10.tsx`
  (`TrilhaV4`, `CAPA_V4`), `trilha-encaixe.ts` (rolagem), `src/lib/transicao-hub.ts` (entrada) e
  `src/styles.css` (`.pd4-cartao`, `.pn10-seta`).

## Fidelidade e grade
Alta fidelidade, **medida no DOM** (não no olho). Palco fixo de **1920 × 1080**, escalado para caber na
janela. Grade de 24 colunas de 80 px (c1 em x 80, c23 terminando em 1840) e 67 linhas (1080 / 67 =
16,12 px por linha).

## Posição na página
| | Valor |
| --- | --- |
| Trilha | de y 40 a 957 (passa por baixo da barra, para a sombra do hover não sair cortada) |
| Primeiro cartão | x 720 (c9, logo depois do painel da busca) |
| Topo dos cartões | linha 9: 9 × 1080 / 67 = **145,07** (o navegador arredonda para 145,06) |
| Altura | 720 (até 865,07) |
| Largura | 160 cada, encostados (7 × 160 = 1120, de 720 a 1840) |
| Depois do último | 80 de margem, para ele parar na c23 |

## O cartão (160 × 720)
| Peça | Medidas (dentro do cartão) |
| --- | --- |
| Fundo | um Pantone real da família (tabela de cores abaixo) |
| Raio | 20 |
| Sombra | `7px 7px 42px rgba(0,0,0,.2)` |
| Empilhamento | cada cartão por cima do anterior (z-index 1 a 8) |
| Círculo da contagem | x 58, y 47, 44 × 44, preto `#000000`; número em Inter 700, 14 px, branco, algarismos tabulares, ponto de milhar ("1.234"); nos cartões escuros, anel de 1,5 px `#f1f1f1` |
| Nome | Fraunces 600, `SOFT 100`, `opsz 72`, 56 px, entrelinha 56, `letter-spacing -0.04em`, na tinta do cartão, sem quebra. Lê de baixo para cima (`writing-mode: vertical-rl` + `rotate(180deg)`), caixa x 52, y 122, 56 × 475, ancorado embaixo: a primeira letra fica perto de y 597 |
| Seta | círculo x 58, y 629, 44 × 44, borda 1,5 px na tinta; seta ↗ de 18 px, traço 2,2, pontas e cantos redondos (linha de 7,17 a 17,7 e a ponta 8,7 → 17,7 → 17,16, num quadro de 24) |

## Cores
| Família | Pantone | Fundo | Tinta (nome e seta) | Contagem |
| --- | --- | --- | --- | --- |
| Amarelos | 7403 C | `#edd586` | `#000000` | 287 |
| Laranjas | 1565 C | `#fea06a` | `#000000` | 184 |
| Vermelhos | 7625 C | `#df523e` | `#000000` | 284 |
| Rosas e roxos | 197 C | `#e59caf` | `#000000` | 643 |
| Azuis | 7700 C | `#115c7d` | `#f1f1f1` | 531 |
| Verdes | 7730 C | `#499662` | `#000000` | 543 |
| Marrons | 10363 C | `#7b5a31` | `#f1f1f1` | 218 |
| Neutros | Warm Gray 3 C | `#bdb8af` | `#000000` | 529 |

- **Regra da tinta:** preto quando o preto passa de 4,5:1 de contraste com o fundo do cartão; senão `#f1f1f1`.
- **Fundo dos cartões:** cada um é um Pantone Solid Coated (2024) da própria família, conferido na régua de
  família do hub (`dados/familia-cor.ts`). Os Hex são os da tabela do hub.
- **Contagem:** quantas cores daquela família a tabela do hub tem (ao vivo; os números acima são os de 07/10/2026).
- **Fundo da página:** `#f3f0f1` (a paleta nova, ainda em teste; a atual é `#ece9ea`).

## Estados
- **Repouso:** como acima.
- **Hover:** o cartão sobe 8 px, cresce 3,5% (a partir do centro), passa por cima de todos e a sombra vai para
  `10px 20px 56px rgba(0,0,0,.3)`. Transição `transform .32s cubic-bezier(.22,.61,.36,1)`, sombra `.32s ease`.
  A seta enche com a tinta, o desenho dela vira a cor do cartão, e ela gira 45° em `.2s ease`.
- **Foco pelo teclado:** o mesmo da seta, mais um contorno preto de 2 px, 6 px para dentro.
- **Dica de rolar:** enquanto ninguém rolou a trilha, a seta animada das trilhas fica na margem da direita,
  centrada em x 1867,3 e no meio dos cartões (y 505,07): Lottie `assets/seta-rolar.json`, 64 × 64, girada −90°
  (aponta para a direita), entra em `.4s`. Some na primeira rolagem e não volta.
- **Fora da janela:** o cartão cujo meio sai do intervalo 720 a 1840 apaga (opacidade 0 em `.25s`) e não
  recebe clique. A sombra dos que estão dentro fica inteira (a trilha não corta nas colunas).

## Movimento
- **Entrada da página (escada com mola):** cada cartão sobe 96 px com escala 0,96 e opacidade 0 e assenta em
  `.78s cubic-bezier(.34,1.42,.62,1)`, um a cada 0,09 s. O resto da página sobe 32 px junto (`.5s`, atraso
  `.12s`). A primeira página do carregamento não anima.
- **Roda do mouse:** anda um cartão por giro (160 px), contado a partir do destino (giros seguidos somam,
  sem recomeçar do meio do caminho), com uma trava de 220 ms para o trackpad.
- **Arrastar:** a trilha acompanha a mão; ao soltar, anda até o cartão mais perto. O clique que fecha um
  arrasto não abre a família.
- **Parou de rolar** (trackpad, Shift + roda, teclado): 250 ms depois, assenta no cartão mais perto.
- **Sem `scroll-snap` do CSS:** o navegador encaixa pela caixa do cartão já transformada, e o cartão em hover
  puxava o encaixe; a trilha parava 8 px fora da coluna. O encaixe é feito pela conta (a largura do cartão).
- **Teclado:** as setas esquerda e direita levam o foco ao cartão vizinho e trazem ele para a vista.
- **Movimento reduzido** (preferência do sistema): sem transições, sem a entrada, sem o giro da seta.

## Conteúdo e acessibilidade
- Cada cartão é um `button`. Nome para leitor de tela: "Amarelos, 287 cores". O círculo, o nome
  girado e a seta são decorativos (`aria-hidden`).
- O grupo se chama "Famílias de cor".
- O título de cada cartão é o nome da família no plural; "Rosas e roxos" é o único com duas palavras.
