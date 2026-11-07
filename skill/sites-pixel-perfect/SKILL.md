---
name: sites-pixel-perfect
description: Monta landing pages com efeito que viraliza (personagem que segue o mouse, lanterna que revela o que tem por dentro, fundo em loop com parallax) gerando as imagens e o vídeo com o MCP do Higgsfield, escrevendo um prompt pixel perfect em oito partes e conferindo o resultado por print. Use quando pedirem um site, uma landing page ou um hero "com efeito", "cinematográfico", "pixel perfect", ou um desses três efeitos.
---

# Sites pixel perfect

Site de IA fica genérico por três motivos: plano vago, asset de banco de imagem e ninguém olhando
o resultado. Esta skill resolve os três: o asset é gerado sob medida no Higgsfield, o plano é um
prompt pixel perfect com medida em tudo, e o resultado é conferido por print antes de entregar.

Siga os passos na ordem, sempre.

## 1. Ler o pedido e decidir (sem interrogar)

Decida estas cinco coisas e diga numa mensagem curta o que escolheu:

- **O que é e pra quem:** o produto ou a pessoa, e quem a página quer convencer.
- **O clima em três palavras** (ex.: "calmo, caro, noturno").
- **O efeito**, um só, que é a assinatura da página (receitas em `references/efeitos.md`):
  - **cursor:** um personagem vira a cabeça seguindo o mouse (vídeo comandado pelo mouse);
  - **lanterna:** o cursor abre um círculo que mostra o que tem por dentro da imagem;
  - **loop:** foto que vira vídeo em loop, com parallax nas camadas.
- **Saída:** um `index.html` só (padrão), ou o prompt pixel perfect sem montar (quando pedirem só o prompt).
- **Escopo:** só o hero (padrão) ou a landing inteira.

Só pergunte se não der pra seguir sem a resposta (ex.: o nome de uma marca real).

## 2. Gerar os assets no Higgsfield

O asset é o que faz a página parecer cara. Ordem de preferência:

1. Os assets do usuário.
2. **Gerar no Higgsfield**, pelo MCP, com a receita do efeito escolhido em `references/efeitos.md`.
3. Sem como gerar: página só de tipografia e movimento. Nunca caixa cinza, banco de imagem ou URL inventada.

Regras que valem pra qualquer efeito:

- **Imagem primeiro, vídeo depois.** Imagem custa poucos créditos, vídeo custa dezenas. Só gere o
  vídeo quando as imagens estiverem certas. Diga o custo antes de gerar vídeo.
- **A segunda imagem é sempre edição da primeira** (Nano Banana editando a base), com "keep
  everything identical" e a única coisa que muda. Gerar de novo do zero desalinha tudo.
- **Direção em relação à borda da imagem**, não ao personagem ("head turned toward the LEFT edge of
  the image"). "Esquerda dele" o modelo erra.
- **A Nano Banana aceita um job por vez.** Mande um, espere terminar, mande o próximo. Duas juntas
  voltam erro de limite.
- **Metade vazia pro texto:** o sujeito numa metade, a outra lisa.
- **Fundo da imagem = fundo da página.** Anote o hex da borda da imagem; ele vira o `--bg`.

## 3. Escrever o prompt pixel perfect

Escreva o prompt em oito partes, nesta ordem, com o modelo em `references/especificacao.md`:
**fontes, cores, assets, estrutura, animação, JavaScript, responsivo, checagem.**

Tudo com valor. Nada de "gradiente bonito" ou "animação suave": cada adjetivo vem com o número
que produz ele. Escreva também o texto real da página (título, subtítulo, navegação, botões).

Se pediram só o prompt, ele é a entrega: confira com `references/revisao.md` e pare aqui.

## 4. Montar

Monte exatamente o que o prompt diz:

- Um `index.html` com `<style>` e `<script>` dentro, fontes por link, bibliotecas de CDN com versão fixa.
- A primeira regra do CSS põe o fundo em `html, body`, pra não piscar branco.
- HTML semântico, `<title>` de verdade, `alt`, foco visível e `prefers-reduced-motion` respeitado.
- No celular (sem mouse) e com movimento reduzido, o efeito fica parado num quadro bonito.
- Sem lorem ipsum, sem caixa de placeholder, sem emoji como ícone.

## 5. Olhar e corrigir (obrigatório)

Tire print da página no desktop (1440×900) e no celular (390×844) e **olhe os prints**. Com
Playwright: `npx playwright screenshot --viewport-size=1440,900 index.html desktop.png`. Sem
ferramenta de print, diga que a página não foi conferida visualmente e revise o código contra
`references/revisao.md`.

Dê nota com a régua de `references/revisao.md`, corrija os três pontos mais fracos com valores
concretos e tire print de novo. Até três rodadas, ou até a média passar de 8 sem nota abaixo de 7.

## 6. Publicar (se pedirem)

O MCP do Higgsfield sobe o site num endereço `.higgsfield.app` (`create_website` e
`deploy_website`; o passo a passo está em `get_workflow_instructions` com
`website-builder-flow`). **Todo deploy já sai público**: avise antes e não deixe chave nem dado
pessoal no site.

## 7. Entregar

- O arquivo, e como abrir.
- O print final do desktop.
- Duas ou três linhas sobre o efeito e as escolhas.
- Os créditos gastos no Higgsfield, por asset.
- O que trocar, se tiver algo provisório.

## Arquivos

- `references/efeitos.md`: as três receitas, com os prompts de geração e o código.
- `references/especificacao.md`: o modelo do prompt pixel perfect em oito partes.
- `references/revisao.md`: a régua de nota e o ciclo de correção.

Estrutura (pedido → mídia → especificação → montagem → print) adaptada do MotionSites Web
Builder, licença MIT (ver `LICENSE.md`). Prompts, receitas e código desta skill são nossos.
