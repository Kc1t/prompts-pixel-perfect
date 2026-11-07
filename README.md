# Prompts pixel perfect

Os prompts, a skill e os três sites do vídeo **"O segredo das melhores landing pages"** do canal [Kauã Miguel - Dev](https://youtube.com/@kc_1t).

Um prompt pixel perfect descreve o site inteiro com medida, cor e tempo exatos, em oito partes: fontes, cores, assets, estrutura, animação, JavaScript, responsivo e checagem. Como não sobra nada pro modelo adivinhar, o resultado sai igual em qualquer IA, até nas mais baratas.

## O que tem aqui

| Pasta | O que é |
|---|---|
| [`prompts/austral.md`](prompts/austral.md) | Paisagem em três camadas que se mexem com o mouse (parallax) |
| [`prompts/robo.md`](prompts/robo.md) | Robô que vira a cabeça seguindo o mouse (o mouse controla o vídeo) |
| [`prompts/ninja.md`](prompts/ninja.md) | Ninja com uma lanterna de raio-X que revela o que tem por dentro |
| [`skill/`](skill/) | Skill `sites-pixel-perfect` pro Claude: escreve o prompt, gera os assets e confere o site por print |
| [`sites/`](sites/) | Os três sites prontos que saíram desses prompts, com os assets |

## Como usar

1. Gere os assets descritos na parte 3 de cada prompt. No vídeo eu gerei tudo no [Higgsfield](https://higgsfield.ai/s/higgsfield-general-campaign-yt-kc-1t-KStoNa), pelo [MCP](https://higgsfield.ai/mcp), sem sair do Claude.
2. Cole o prompt no Claude (ou na IA que você usa) junto com os assets.
3. Abra o site e confira com a parte 8, a checagem.

### Instalar a skill

- **Claude Code:** copie `skill/sites-pixel-perfect/` pra `~/.claude/skills/`.
- **Claude no navegador:** em Configurações, Capacidades, envie o `skill/sites-pixel-perfect.zip`.

### Rodar os sites

```bash
cd sites/austral && python3 -m http.server 8000
```

O robô precisa do servidor com suporte a Range, senão o vídeo trava no primeiro quadro:

```bash
cd sites/robo && python3 serve_range.py 8000
```

## Quanto custou (créditos do Higgsfield)

| Asset | Modelo | Créditos |
|---|---|---|
| Cada camada da AUSTRAL | GPT Image 2.6, 16:9, 2K | 2,75 |
| Vídeo do robô | Seedance 2.5 | 80 |
| Vídeo do ninja | Kling 3.0 pro, 1080p, 5 s | 8,75 |

O vídeo é o que pesa. Teste um modelo de vídeo mais barato antes do Seedance.

## Licença

A estrutura da skill foi adaptada do [MotionSites Web Builder](https://github.com/Motionsites-AI/claude-skill) (MIT), ver [`skill/sites-pixel-perfect/LICENSE.md`](skill/sites-pixel-perfect/LICENSE.md). Prompts, sites e assets: uso livre, só não revenda como seus.
