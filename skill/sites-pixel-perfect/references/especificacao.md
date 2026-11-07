# O prompt pixel perfect

É aqui que a qualidade se decide, antes de qualquer código. O prompt lê como um briefing de
designer sênior pra dev: todo elemento visível tem valor, todo asset tem caminho, toda animação
tem atraso, duração e curva. Um hero dá umas 400 a 1.000 palavras.

## Regras de precisão

- **Nenhum adjetivo sozinho.** "Cinematográfico", "premium", "de vidro" vêm seguidos dos valores
  que produzem isso: blur em px, transparência, paradas do gradiente, durações.
- **Todo texto tem linha de estilo completa:** tamanho (`clamp()` ou por breakpoint), peso,
  entrelinha, espaçamento, cor com opacidade, largura máxima e margens.
- **Cores em hex**, com degraus de opacidade pra hierarquia (100 / 80 / 60 / 40).
- **Animação por elemento:** gatilho, atraso, duração, curva e de → para. Escalonamento escrito
  como fórmula (`delay = 0.3 + i * 0.12s`).
- **Asset:** caminho exato, elemento e atributos, encaixe (`object-fit`, `object-position`),
  modo de reprodução e uma frase sobre o que aparece, onde está o sujeito e onde está o vazio.
- **Mecânica:** o algoritmo em passos, com as constantes (fator de atraso, sensibilidade, limites).

## Conferência antes de montar

- Todo asset tem caminho que existe.
- Toda fonte citada é carregada.
- O fundo da página é igual à cor da borda do asset principal.
- Nenhum "não faça" contradiz a lista de assets ou a estrutura.

## Modelo

As oito partes, nesta ordem. Escreva "nenhum" em vez de deixar uma parte vazia.

```
1. FONTS
Heading: [família], loaded from [URL exata]. Body: [família], loaded from [URL exata].
Fallback: [pilha].

2. COLORS
Page background: [hex, igual à borda do asset]. Text: [hex].
[Cada elemento com cor própria: botão, borda, hover, overlay do menu.]

3. ASSETS
[Arquivo]: [o que mostra, onde está o sujeito, onde está o vazio].
[position, object-fit, object-position, atributos de reprodução.]
Poster: [arquivo] (frame 0 of the video).

4. LAYOUT
Navbar: [posição, padding, logo com tamanho e espaçamento, links, CTA].
Hero: [altura, caixa de texto, alinhamento, padding].
[Cada texto: tamanho, entrelinha, efeito.]
[Botões: tamanho, padding, raio, espaço entre eles.]

5. ANIMATION
[Elemento]: starts at [ms], [duração], [curva], from [estado] to [estado].
[Hovers com duração.] Not animated: [o que fica parado].

6. JAVASCRIPT
[A mecânica do efeito em passos, com as constantes.]

7. RESPONSIVE
From [px]: [o que muda]. Below [px]: [menu, tamanhos].
prefers-reduced-motion: [estado final, sem efeito].

8. CHECK
At 1440×900: [onde cada coisa fica e o que nunca encosta no quê].
[O que não pode ter: overlay, gradiente, sombra.]
[Como testar o efeito com o mouse.]
```

## Exemplo: o site do robô (efeito cursor)

```
1. FONTS
Heading: HelveticaNowDisplay-Medium, loaded from
https://db.onlinewebfonts.com/c/5ac3fe7c6abd2f62067f266d89671492?family=HelveticaNowDisplay-Medium
Body: HelveticaNowDisplayW01-Rg, loaded from
https://db.onlinewebfonts.com/c/1aa3377e489837a26d019bba501e779d?family=HelveticaNowDisplayW01-Rg
Fallback for both: 'Helvetica Neue', Arial, sans-serif.

2. COLORS
Page background: #d9d7d3. Text: #000.
Pills: background #fff, text #000, border 1px rgba(0,0,0,.1).
Pill hover: background #000, text #fff, in 200ms.
Mobile menu overlay: rgba(255,255,255,.95) with backdrop-filter: blur(4px).

3. ASSETS
Background video: assets/look.mp4, 5 s, the head turns from left to right.
position: fixed; inset: 0; z-index: 0; object-fit: cover; object-position: 70% center.
muted, playsInline, preload="auto". No autoplay, no loop.
Poster: assets/poster.jpg (frame 0 of the video).

4. LAYOUT
Navbar: fixed at the top, padding 16px 20px. Logo 21px, letter-spacing -0.02em.
Four links at 23px, separated by commas. "Get in touch" at 23px, underlined.
Hero: full height (100dvh). Text box max-width 36rem, at the bottom, padding-bottom 48px.
Intro: 2 lines, font-size clamp(18px, 4vw, 26px), line-height 1.3, filter: blur(4px).
Typed text: same size, line-height 1.35, min-height 54px.
Pills: 13px, padding .3em 16px, border-radius 999px, 4px between rows.

5. ANIMATION
Typewriter: starts at 600ms, 38ms per character, blinking caret (1s, step-end),
caret removed when the text ends.
Pills: fade in at 400ms, from opacity 0 and translateY(8px), 0.4s ease. Don't wait for the typing.
Links: hover opacity 0.6 in 200ms.

6. JAVASCRIPT
Mouse scrub: on mousemove, delta = clientX - prevX.
targetTime += (delta / innerWidth) * SENSITIVITY * duration, SENSITIVITY = 0.8,
clamped between 0 and duration.
Queue seeks: only set currentTime again after the previous seek fires "seeked".
On loadedmetadata, start at duration / 2, with the head facing forward.

7. RESPONSIVE
From 640px: navbar padding 20px 32px, logo 26px, pills 15px with padding .3em 20px.
From 768px: show the links and "Get in touch", hide the menu button,
center the hero text vertically, padding 0 40px.
Below 768px: menu button opens a full-screen overlay, links at 32px.
prefers-reduced-motion: no typing animation, the video stays on the middle frame.

8. CHECK
At 1440×900, the head sits at 70% of the width and the text box never touches it.
No overlay or gradient over the video.
Moving the mouse from the left edge to the right edge turns the head from left to right.
```
