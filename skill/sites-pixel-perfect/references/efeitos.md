# Os três efeitos

Cada receita tem: como o efeito funciona, os assets, os prompts de geração no Higgsfield e o
código. Os prompts de imagem ficam em inglês, que é como os modelos respondem melhor.

## Cursor: personagem que segue o mouse

**Como funciona:** não é 3D. É um vídeo de 5 s em que o personagem vira a cabeça da esquerda pra
direita. O vídeo não toca sozinho: o movimento horizontal do mouse anda pra frente e pra trás nele.

**Assets:**

1. Imagem A, olhando pra esquerda. GPT Image 2.5, 16:9, qualidade alta, 2K:
   ```
   [personagem: material, roupa, detalhe marcante], plain [cor] seamless background,
   soft even studio light. The head is turned toward the LEFT edge of the image.
   The character sits in the right half; the left half is empty, for text.
   No text, no logos, 16:9.
   ```
2. Imagem B, olhando pra direita. Nano Banana 2.1 editando a A:
   ```
   Keep everything identical: body, clothes, light, background, framing.
   Change ONLY the head direction: now turned toward the RIGHT edge of the image.
   ```
3. Vídeo. Seedance 2.5, 1080p, 5 s, sem áudio, frame inicial = A, frame final = B. Prompt:
   `Animate.` (com início e fim travados, o prompt quase não muda o resultado).
4. Pôster: o primeiro quadro do vídeo, não a imagem A (o Seedance mexe um pouco na cor).

**Preparar o vídeo:** pra o mouse conseguir pular pra qualquer ponto sem engasgar, todo quadro
tem que ser quadro-chave.

```bash
ffmpeg -i bruto.mp4 -an -c:v libx264 -crf 20 -g 1 -pix_fmt yuv420p -movflags +faststart assets/look.mp4
ffmpeg -i assets/look.mp4 -frames:v 1 -q:v 2 assets/poster.jpg
```

Sem ffmpeg no ambiente (Claude web), use o vídeo como veio, com a fila de seek abaixo, avise que
pode engasgar e entregue os dois comandos pro usuário rodar.

**Código:**

```html
<video class="bg" id="bg" muted playsinline preload="auto" poster="assets/poster.jpg"></video>
<style>.bg{position:fixed;inset:0;z-index:0;width:100%;height:100%;object-fit:cover;object-position:70% center}</style>
<script>
const video = document.getElementById('bg'), SENSITIVITY = 0.8;
let prevX = null, targetTime = 0, seeking = false;
function seek() {
  if (!video.duration || Math.abs(video.currentTime - targetTime) < 0.001) return;
  seeking = true; video.currentTime = targetTime;
}
// fila: só pede o próximo pulo quando o anterior terminou
video.addEventListener('seeked', () => { seeking = false; if (Math.abs(video.currentTime - targetTime) > 0.001) seek(); });
addEventListener('mousemove', e => {
  if (prevX === null) { prevX = e.clientX; return; }
  const delta = e.clientX - prevX; prevX = e.clientX;
  if (!video.duration) return;
  targetTime = Math.min(video.duration, Math.max(0, targetTime + (delta / innerWidth) * SENSITIVITY * video.duration));
  if (!seeking) seek();
});
// começa no meio, olhando pra frente
video.addEventListener('loadedmetadata', () => { targetTime = video.duration / 2; seek(); }, { once: true });
video.src = 'assets/look.mp4';
</script>
```

**Pegadinha de teste local:** o `python -m http.server` não responde pedido de pedaço do arquivo
(`Range`), e sem isso o Chrome não pula: o vídeo fica preso no primeiro quadro. Teste com um
servidor que responda `Range` (`npx serve`, por exemplo). Em produção já vem resolvido.

## Lanterna: o que tem por dentro

**Como funciona:** duas imagens no mesmo lugar, uma em cima da outra. A de baixo é o que se vê. A
de cima é o que tem por dentro. O cursor abre um círculo com borda suave na de cima.

**Assets:**

1. Base. GPT Image 2.5, 16:9, qualidade alta, 2K: o sujeito na metade direita, a esquerda vazia.
2. Revelação. Nano Banana 2.1 editando a base:
   ```
   Keep everything identical: same position, size and silhouette, same background and light.
   Change ONLY what is inside: [o que aparece por dentro].
   ```
   A frase da silhueta é o que segura o alinhamento (nos nossos testes, de 0 a 1 px de diferença).

**O que rende de revelação:** raio-X, o mecanismo por dentro (relojoaria dentro do mármore), corte
técnico (carroceria em wireframe), câmera térmica, uma criatura escondida.

**Código:**

```html
<div class="cena"><div id="base"></div><div id="dentro"></div></div>
<style>
.cena{position:fixed;inset:0}
#base,#dentro{position:absolute;inset:0;background:center/cover no-repeat}
#base{background-image:url(assets/base.jpg)}
#dentro{background-image:url(assets/dentro.jpg);-webkit-mask-image:radial-gradient(circle 0px at -999px -999px,#fff,transparent);mask-image:radial-gradient(circle 0px at -999px -999px,#fff,transparent)}
</style>
<script>
const dentro = document.getElementById('dentro');
const raio = () => innerWidth < 480 ? 120 : innerWidth < 720 ? 160 : 260;
const calmo = matchMedia('(prefers-reduced-motion: reduce)').matches;
let tx = -999, ty = -999, x = -999, y = -999, rT = 0, r = 0, ativo = false;
function pinta() {
  const m = `radial-gradient(circle ${r.toFixed(1)}px at ${x.toFixed(1)}px ${y.toFixed(1)}px, #fff 0%, #fff 40%, rgba(255,255,255,.75) 60%, rgba(255,255,255,.4) 75%, rgba(255,255,255,.12) 88%, transparent 100%)`;
  dentro.style.webkitMaskImage = m; dentro.style.maskImage = m;
}
function move(cx, cy) { if (!ativo) { x = cx; y = cy; ativo = true; } tx = cx; ty = cy; rT = raio(); if (calmo) { x = tx; y = ty; r = rT; pinta(); } }
addEventListener('mousemove', e => move(e.clientX, e.clientY));
addEventListener('touchmove', e => { const t = e.touches[0]; if (t) move(t.clientX, t.clientY); }, { passive: true });
document.documentElement.addEventListener('mouseleave', () => { rT = 0; });
// a lanterna persegue o cursor com atraso e abre ao entrar
if (!calmo) (function tick() { x += (tx - x) * .14; y += (ty - y) * .14; r += (rT - r) * .1; if (ativo) pinta(); requestAnimationFrame(tick); })();
</script>
```

**Por cima de vídeo:** se a base for um vídeo, o Seedance desloca e escala o quadro em cerca de
1,5%. Meça o deslocamento contra o primeiro quadro e recorte a imagem de dentro pra casar.

## Loop: fundo que se mexe

**Como funciona:** uma foto que vira vídeo em loop, mais parallax: o fundo anda pra um lado e o
texto pro outro, cada um numa velocidade, seguindo o mouse com atraso.

**Assets:**

1. Foto. GPT Image 2.5, 16:9, qualidade alta, 2K, com movimento contido possível (vapor, cabelo, luz, água).
2. Vídeo. Seedance 2.5, 1080p, 5 s, sem áudio, **a mesma foto no frame inicial e no final**. Assim
   o loop fecha sozinho na maioria das vezes.
3. Se a emenda aparecer (o último quadro sai diferente), funda os últimos 12 quadros no começo:
   ```bash
   # 5 s a 24 fps: crossfade de 0,5 s do fim pro começo
   ffmpeg -i bruto.mp4 -filter_complex "[0]split[a][b];[a]trim=0.5:5,setpts=PTS-STARTPTS[m];[b]trim=0:0.5,setpts=PTS-STARTPTS[c];[m][c]xfade=transition=fade:duration=0.5:offset=4[v]" -map "[v]" -an -c:v libx264 -crf 17 -pix_fmt yuv420p -movflags +faststart assets/loop.mp4
   ```
4. Pôster = primeiro quadro do loop. O vídeo só entra depois do primeiro quadro pronto
   (`loadeddata`), por cima do pôster, pra a troca não pular.

**Código do parallax:** o JavaScript só atualiza duas variáveis; cada camada lê as duas no CSS,
pela propriedade `translate` (separada do `transform`, pra não brigar com as animações de entrada).

```css
.cena{translate:calc(var(--mx)*-22px) calc(var(--my)*-14px);scale:1.07}
.texto{translate:calc(var(--mx)*16px) calc(var(--my)*10px)}
@media (max-width:900px),(prefers-reduced-motion:reduce){.cena,.texto{translate:none;scale:1}}
```

```js
const hero = document.querySelector('.hero');
let mx = 0, my = 0, tx = 0, ty = 0;
addEventListener('mousemove', e => { tx = e.clientX / innerWidth * 2 - 1; ty = e.clientY / innerHeight * 2 - 1; });
if (matchMedia('(hover:hover) and (pointer:fine)').matches && !matchMedia('(prefers-reduced-motion: reduce)').matches)
  (function tick() {
    mx += (tx - mx) * .06; my += (ty - my) * .06;
    hero.style.setProperty('--mx', mx.toFixed(4)); hero.style.setProperty('--my', my.toFixed(4));
    requestAnimationFrame(tick);
  })();
```

## Custos medidos (Higgsfield, novembro de 2026)

| Asset | Custo |
|---|---|
| Nano Banana 2.1, 2K | ~4,7 créditos |
| Seedance 2.5, 5 s, 1080p | 80 créditos |

Um vídeo custa umas dezessete imagens: erre nas imagens, não no vídeo.
