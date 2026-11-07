# Prompt pixel perfect: ninja com lanterna de raio-X

## 1 · Fontes

Título: uma fonte tech quadrada, em maiúsculas, 3 linhas, entrelinha justa de 1.0.
Texto e interface: Inter 400/500. Etiquetas: a fonte do título, em maiúsculas, letras afastadas 0.12em.

## 2 · Cores

Fundo: a própria imagem, do laranja queimado ao vermelho brasa. Texto: branco quente #f6e7d8.
Cartão de vidro: rgba(20,8,4,.55), desfoque 16px, borda 1px rgba(255,255,255,.12), cantos de 16px.
Destaque: o laranja que brilha no visor, só no anel da lanterna.

## 3 · Assets (gerados na sua geradora de imagens)

base.jpg, GPT Image 2.5, 16:9, qualidade alta, 2K: "Um ninja cibernético de perfil, jaqueta de armadura escura e gasta, capacete com uma linha laranja brilhando no visor, katana nas costas, brasas flutuando, fundo laranja queimado. O ninja fica na metade direita; a metade esquerda fica vazia pro texto."

xray.jpg, Nano Banana 2.1 editando a base.jpg: "Mantenha tudo idêntico: mesma posição, tamanho e silhueta. Mude SÓ o que está por dentro: um raio-X do capacete e da jaqueta, mostrando uma caveira e circuitos laranja brilhando."

loop.mp4: Seedance 2.5, 1080p, 5 s, com a base.jpg como quadro inicial e final, pra fechar o loop. Pôster = quadro 0.

## 4 · Estrutura

Hero: 100vh. Camadas: loop.mp4 → xray.jpg recortado pelo círculo do cursor → texto.
Canto superior esquerdo: título + 3 linhas de texto + três botões redondos com ícone.
Canto inferior esquerdo: cartão do produto, com miniatura e um botão vazado.
Canto inferior direito: tabela de especificações, 4 linhas, nome à esquerda e valor à direita, divisórias finas.
Canto superior direito: contador de slides "1/26".

## 5 · Animação

Ao carregar: o círculo abre de 0 até o raio dele em 600ms quando o cursor entra.
As brasas sobem em loop. O texto aparece subindo, 80ms entre cada linha.

## 6 · JavaScript

A camada do raio-X tem mask-image: radial-gradient(circle var(--r) at var(--x) var(--y), #000 70%, transparent 100%).
Quando o mouse anda, o alvo é o cursor. A cada quadro: posição += (alvo − posição) * 0.12, então o círculo persegue o cursor com atraso.
O xray.jpg é recortado pra casar com o quadro do vídeo (o Seedance desloca uns 1,5%), senão o de dentro escorrega do de fora.

## 7 · Responsivo

No toque, sem hover: o círculo passeia sozinho pelo capacete.
Abaixo de 768px: a tabela e o cartão do produto vão pra baixo do título.
prefers-reduced-motion: sem o vídeo em loop (só o pôster), círculo parado no capacete.

## 8 · Checagem

Em 1440×900: o ninja ocupa a metade direita, e o texto nunca encosta nele.
Passar o cursor pelo capacete mostra a caveira exatamente no lugar, sem emenda na borda do círculo.

