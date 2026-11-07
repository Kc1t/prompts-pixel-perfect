# Prompt pixel perfect: robô que segue o mouse

## 1 · Fontes

Títulos: HelveticaNowDisplay-Medium, carregada de
https://db.onlinewebfonts.com/c/5ac3fe7c6abd2f62067f266d89671492?family=HelveticaNowDisplay-Medium
Texto: HelveticaNowDisplayW01-Rg, carregada de
https://db.onlinewebfonts.com/c/1aa3377e489837a26d019bba501e779d?family=HelveticaNowDisplayW01-Rg
Reserva das duas: 'Helvetica Neue', Arial, sans-serif.

## 2 · Cores

Fundo da página: #d9d7d3. Texto: #000.
Pílulas: fundo #fff, texto #000, borda 1px rgba(0,0,0,.1).
Hover da pílula: fundo #000, texto #fff, em 200ms.
Menu do celular: rgba(255,255,255,.95) com desfoque de 4px atrás.

## 3 · Assets (gerados na sua geradora de imagens)

Vídeo de fundo: assets/look.mp4, 5 s, a cabeça vira da esquerda pra direita.
position: fixed; ocupa a tela inteira; atrás de tudo; object-fit: cover; object-position: 70% center.
Sem som, playsInline, preload="auto". Sem autoplay, sem loop.
Pôster: assets/poster.jpg (o quadro 0 do vídeo).
O vídeo é feito em [sua geradora de imagens e vídeo]: duas imagens (GPT Image 2.5 e uma edição no Nano Banana 2.1) e o Seedance 2.5 entre elas.

## 4 · Estrutura

Menu: fixo no topo, espaçamento 16px 20px. Logo com 21px, letras -0.02em.
Quatro links com 23px, separados por vírgula. "Get in touch" com 23px, sublinhado.
Hero: altura da tela inteira (100dvh). Caixa de texto com no máximo 36rem, embaixo, 48px do chão.
Introdução: 2 linhas, tamanho clamp(18px, 4vw, 26px), entrelinha 1.3, desfoque de 4px.
Texto digitado: mesmo tamanho, entrelinha 1.35, altura mínima 54px.
Pílulas: 13px, espaçamento .3em 16px, cantos redondos, 4px entre as linhas.

## 5 · Animação

Máquina de escrever: começa em 600ms, 38ms por letra, cursor piscando (1 s),
o cursor some quando o texto acaba.
Pílulas: aparecem em 400ms, de opacidade 0 e 8px abaixo, em 0,4 s. Não esperam a digitação.
Links: no hover, opacidade 0.6 em 200ms.

## 6 · JavaScript

O mouse controla o vídeo: quando ele anda, diferença = x atual − x anterior.
tempo alvo += (diferença / largura da tela) * SENSIBILIDADE * duração, SENSIBILIDADE = 0.8,
sempre entre 0 e a duração.
Enfileire os pulos: só pule pra um tempo novo depois que o pulo anterior terminar ("seeked").
Quando o vídeo carrega, começa na metade da duração, com a cabeça olhando pra frente.

## 7 · Responsivo

A partir de 640px: menu com espaçamento 20px 32px, logo 26px, pílulas 15px com espaçamento .3em 20px.
A partir de 768px: mostra os links e o "Get in touch", esconde o botão de menu,
centraliza o texto do hero na vertical, espaçamento 0 40px.
Abaixo de 768px: o botão de menu abre uma tela cheia, links com 32px.
prefers-reduced-motion: sem digitação, o vídeo fica parado no quadro do meio.

## 8 · Checagem

Em 1440×900, a cabeça fica em 70% da largura e a caixa de texto nunca encosta nela.
Nenhuma camada escura nem degradê em cima do vídeo.
Levar o mouse da borda esquerda até a direita vira a cabeça da esquerda pra direita.

