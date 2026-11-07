# Olhar e corrigir

Não dá pra julgar uma página que você não viu. Depois de montar, tire print no desktop
(1440×900) e no celular (390×844), abra os prints e olhe de verdade.

## Bloqueia na hora

Qualquer um destes reprova a página:

- Erro no console, arquivo que não carregou, vídeo que devia aparecer e não aparece.
- Rolagem horizontal no celular.
- Tela branca piscando antes do fundo escuro (falta o fundo em `html, body` na primeira regra).
- Texto ilegível: em cima da parte movimentada da imagem, com pouco contraste, ou cortado.
- Fonte que não carregou (título caindo pra fonte do sistema).
- Elementos se sobrepondo sem querer, menu quebrando em duas linhas.
- Texto de enchimento ("Lorem", "Seu slogan aqui").
- O efeito não funciona: a cabeça não vira, a lanterna não abre, o loop dá pulo.

## Nota de 1 a 10 em cada

1. **Três segundos:** um ponto de foco claro; dá pra saber o que é e pra onde olhar.
2. **Efeito:** o efeito escolhido é a coisa que alguém gravaria a tela pra mostrar.
3. **Tipografia:** título grande e firme, texto calmo, no máximo duas famílias.
4. **Espaço:** vazio generoso e de propósito; o olho vai do asset pra mensagem e pro botão.
5. **Cor e luz:** paleta curta, um destaque usado com intenção, texto protegido sobre a imagem.
6. **Asset integrado:** a imagem ou o vídeo faz parte da página, não é um retângulo colado. A
   borda do asset some no fundo.
7. **Movimento:** entrada coreografada (0,6 a 1,2 s no total), efeito com atraso físico,
   movimento reduzido respeitado.
8. **Celular:** sem mouse, o efeito fica parado num quadro bonito; texto e asset recortados pro celular.
9. **Texto:** específico, curto, com ponto de vista; botão com verbo.
10. **Acabamento:** hover e foco, raios e bordas consistentes, `<title>`, nada pulando ao carregar.

**A régua é média 8 ou mais, sem nota abaixo de 7.** O primeiro rascunho costuma dar 6 ou 7.

## Ciclo de correção

1. Anote as três notas mais baixas e, pra cada uma, a mudança com valor ("título de 64px pra
   clamp(56px, 8vw, 128px), espaçamento -0.04em"), não com adjetivo.
2. Corrija, tire print de novo, olhe de novo.
3. Até três rodadas. Se algo não tiver conserto (asset ruim), diga com clareza o que resolveria,
   em geral regerar a imagem.
