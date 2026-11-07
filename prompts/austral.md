# Prompt pixel perfect: AUSTRAL (paisagem em camadas)

## 1 · Fontes

Gigante: Anton 400 (a palavra AUSTRAL, os números grandes, a chamada final). Texto e interface: Inter 400/500/600. Etiquetas: JetBrains Mono 500.
Carregar do Google Fonts. Reservas: Impact, 'Arial Narrow' / system-ui / ui-monospace.

## 2 · Cores

Fundo: #f3efe6 (papel quente). Céu: #c9dcec (a cor do céu no topo da foto, fundo do hero antes da imagem carregar).
Tinta: #16222b (AUSTRAL e títulos). Papel: #fbf8f2 (texto em cima da foto).
Destaque: #c4571f, o laranja da jaqueta do trilheiro. Só no botão principal, no ponto de status e no hover dos links.

## 3 · Assets (gerados na sua geradora de imagens)

Gere três camadas com [sua geradora de imagens], no GPT Image 2.6, 16:9, qualidade alta, 2K.
As três têm o mesmo quadro 16:9 e object-position 62% 100%, pra encaixar pixel por pixel.

fundo → backdrop.webp: "Foto editorial de paisagem, cordilheira estilo Patagônia: agulhas de granito e picos com geleira baixos no quadro, nos 45% de baixo. Os 55% de cima são um céu azul-claro limpo, com muito espaço vazio pra um título gigante. Sol do fim da manhã vindo da esquerda. Sem pessoas, sem texto."

meio → midground.webp (com o fundo de referência de luz): "Uma colina de capim dourado ocupando só os 38% de baixo da imagem. Tudo acima dela é um fundo branco puro #FFFFFF, liso."

frente → foreground.webp (mesma referência): "Uma crista de granito escuro subindo do canto inferior direito. No topo, um trilheiro visto de costas, jaqueta laranja queimado, mochila cinza, um bastão de caminhada. Todo o resto é branco puro #FFFFFF."

Depois, tire o branco do meio e da frente com colorkey, e não com o removedor de fundo: ele guarda só o objeto principal e joga a rocha fora.

## 4 · Estrutura

Hero: 100dvh, altura mínima 620px, sem rolagem pra fora.
Ordem das camadas, de trás pra frente: fundo 0 → AUSTRAL 1 → meio 2 → frente 3 → sombra 4 → conteúdo 10 → menu 20.
AUSTRAL: Anton clamp(110px, 24vw, 460px), entrelinha 0.8, centralizada, a 46,5% do chão (a base das letras na altura dos pés do trilheiro).
Título "Caminhe onde / o mapa termina.": Inter clamp(32px, 3.9vw, 56px), no canto inferior esquerdo.
Cartão "Próxima saída": vidro rgba(251,248,242,.14), desfoque 14px, cantos de 18px, no canto inferior direito.

## 5 · Animação

Fundo: escala 1.08 → 1 em 1,6 s. Meio: sobe 6% → 0. Frente: sobe 10% → 0.
AUSTRAL: nasce de baixo pra cima com clip-path inset(100% 0 0 0) → inset(0), 1,1 s, começando em 0,35 s.
Título, texto, botões e cartão: aparecem subindo em 0,6 / 0,72 / 0,84 / 1,0 s. Curva cubic-bezier(.22,1,.36,1).

## 6 · JavaScript

Quando o mouse anda: alvo x = x / largura da tela − 0,5, alvo y = y / altura − 0,5.
A cada quadro: x += (alvo − x) * 0.08 (o atraso suave), e grava --mx / --my no hero.
Profundidade (px por unidade): fundo −20/−7, AUSTRAL +6/+2, meio +22/+9, frente +42/+16.
O fundo vai pra um lado, o trilheiro pro outro, e a palavra fica no meio.
No celular: as camadas derivam sozinhas, x = sen(t/5200) * 0,22.

## 7 · Responsivo

Abaixo de 640px: AUSTRAL com 30vw, a 45% do chão; título com 32px; camadas em 64% 100%.
O cartão "Próxima saída" só aparece a partir de 1024px.
prefers-reduced-motion: sem entrada, sem parallax, sem deriva.

## 8 · Checagem

Em 1440×900: AUSTRAL enorme atravessando o meio, a base das letras escondida pela estepe,
e a letra perto da direita cortada pelo trilheiro. Título branco no canto inferior esquerdo. Nenhuma camada escura cobrindo a tela inteira.

