# Instruções para o Claude neste repositório

Este repositório publica no Instagram do AprovaMais OAB (@aprovamais.oab). Publicar é público e irreversível, e é a marca falando com alunos.

## Visual dos posts

As artes seguem os dois sistemas visuais da marca, que moram no repositório `aprovamais-oab` (pasta `empresa/marketing/output/`):

- Post único de feed: base `instagram-2fase-domingo/` (barra amarela de 220px no topo com o lockup, fundo #0a0a0a, texto centralizado).
- Carrossel: base `instagram-tutor-aprovacao/` (barra de 80px com lockup e contador n / total, texto alinhado à esquerda, capa e CTA em amarelo, miolo em preto).

Fontes Montserrat (títulos) e DM Sans (corrido). Sempre gerar por script e conferir o PNG renderizado antes de mostrar.

## Regras de texto (arte e legenda)

- Nunca usar travessão longo (—). Usar ponto, vírgula, dois-pontos ou "·".
- Arte enxuta: quem, o quê, quando e para onde ir. Detalhe vai na legenda.
- Carrossel com no máximo 6 telas; a última leva para uma ferramenta da plataforma.
- Não vender "grátis", "de graça" ou "sem cartão". O CTA vende o valor da ferramenta e fecha com "está na plataforma. link na bio".
- Handle certo: @aprovamais.oab (nunca @aprovamais.ia).
- Nada de segmentação interna na peça ("para donos de cursinho" e afins).

## Regra principal

Só faça push de um post com `"status": "aprovado"` quando o Roque tiver dito, no chat e para aquele post específico, que é para publicar. Aprovação de um post não vale para outro. Na dúvida, deixe `rascunho` e pergunte.

## Procedimento

1. Mostre os PNGs e a legenda no chat antes de qualquer coisa.
2. Depois do "publica":
   - imagens: `python scripts/preparar_post.py <slug> legenda.txt img1.png ...`
   - Reels: copiar o MP4 para `posts/<data>-<slug>/video.mp4` (reencodar com ffmpeg para H.264/AAC abaixo de 19 MB) e usar `"video": "video.mp4"` no `post.json`
   - troque o status no `post.json` para `aprovado` (se o Roque pediu horário, acrescente `agendado_para` com fuso -03:00)
   - confira: sem travessão, 1 a 10 imagens, legenda até 2.200 caracteres
   - commit `post: <slug>` e push na `main`
3. Acompanhe com `gh run watch` ou `gh run list --workflow publicar.yml`.
4. Faça `git pull` e leia o `post.json`: se `publicado`, mande o link ao Roque; se `erro`, explique o campo `erro` e só volte para `aprovado` com o ok dele.

## Nunca

- Nunca altere um post `publicado` ou `publicando`.
- Nunca coloque token ou senha em arquivo do repositório.
