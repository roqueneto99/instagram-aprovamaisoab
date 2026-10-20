# Publicador do @aprovamais.oab

Os posts entram neste repositório. Uma GitHub Action publica no Instagram pela API oficial da Meta. Mesmo modelo do publicador do @roquecneto.

## Como funciona

1. O Claude monta a arte (post único ou carrossel) e mostra no chat.
2. Quando o Roque diz "publica", ele cria `posts/<data>-<slug>/` com os JPEGs e o `post.json` (status `aprovado`) e faz push.
3. A Action `Publicar no Instagram` roda, publica e grava de volta no `post.json` o status `publicado` e o link do post.
4. O Claude lê o `post.json` e devolve o link.

Nada vai ao ar sem `status: "aprovado"`. Um post `publicando` ou `publicado` nunca é reenviado, então não tem risco de duplicar.

### Formato do post.json

```json
{
  "status": "aprovado",
  "imagens": ["01.jpg", "02.jpg", "03.jpg"],
  "legenda": "Texto da legenda com #hashtags"
}
```

Para Reels (vídeo MP4 vertical, até 19 MB), troque `imagens` por `"video": "video.mp4"`. Opcional: `"capa_ms": 3000` escolhe o quadro da capa.

Para agendar, acrescente `"agendado_para": "2026-10-22T09:00:00-03:00"`. A Action roda a cada 15 minutos e publica quando a hora chegar (o GitHub pode atrasar alguns minutos).

Status: `rascunho`, `aprovado`, `publicando`, `publicado`, `erro` (com o campo `erro`). Para tentar de novo depois de um erro, volte para `aprovado`.

## Configuração (uma vez só)

### 1. Conta do Instagram

O @aprovamais.oab já é conta profissional. Não precisa de Página do Facebook.

### 2. App na Meta

Reaproveitar o app **AprovaMais Integracoes** (866768583127256), que já está no portfólio AprovaMais OAB.

1. Em developers.facebook.com, abra o app e adicione o produto **Instagram**, opção "API com login do Instagram".
2. Em Funções do app > Testadores do Instagram, adicione o @aprovamais.oab e aceite o convite no Instagram (Configurações > Apps e sites > Convites de testador).
3. Com o app em modo de desenvolvimento, as permissões valem para contas com função no app. Não precisa de App Review para publicar no próprio perfil.

### 3. Token e id da conta

1. No painel do produto Instagram, em "Gerar tokens de acesso", conecte o @aprovamais.oab e gere o token com `instagram_business_basic` e `instagram_business_content_publish`. Já sai de longa duração (60 dias).
2. Para achar o id da conta: `https://graph.instagram.com/me?fields=user_id,username&access_token=SEU_TOKEN`. O `user_id` é o `IG_USER_ID`.

### 4. Segredos no GitHub

Em Settings > Secrets and variables > Actions:

| Segredo | Valor |
|---|---|
| `IG_USER_ID` | o `user_id` do passo anterior |
| `IG_ACCESS_TOKEN` | o token |
| `GH_PAT` | token fine-grained do GitHub só deste repo, com "Secrets: Read and write" (renova o token todo mês) |

Opcional: variável `IG_API_VERSION` (padrão `v23.0`).

### 5. Repositório público

A Meta baixa as imagens de um endereço público (raw.githubusercontent, com jsDelivr de reserva), então o repositório precisa ser público. Só entra aqui o que foi aprovado para publicar.

### 6. Teste

Actions > Testar conexão com o Instagram > Run workflow. Depois Actions > Publicar no Instagram > Run workflow com "Só validar".

## Limites da API

- Carrossel: 2 a 10 imagens, JPEG.
- Legenda: até 2.200 caracteres e 30 hashtags.
- A Meta limita a 100 publicações por 24h via API.
- O token vence em 60 dias; a Action `Renovar token do Instagram` renova no dia 1 de cada mês.
