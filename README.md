# VideoSub local

**ðŸ‡§ðŸ‡· [PortuguÃªs](README.md) Â· ðŸ‡ºðŸ‡¸ [English](README.en.md) Â· ðŸ‡ªðŸ‡¸ [EspaÃ±ol](README.es.md)**

MVP local que transforma conteÃºdo escrito em um vÃ­deo MP4, mantendo cada etapa visÃ­vel para revisÃ£o: roteiro, voz, imagens, movimento opcional, legendas e render.

## ðŸ“– Guia de uso

Guia completo (landing + passo a passo): **https://inematds.github.io/videosub/guia/**

## Requisitos

- Node.js 22 ou superior;
- pnpm 10;
- FFmpeg e FFprobe no `PATH` (o FFmpeg precisa incluir o filtro `subtitles` para gravar legendas no vÃ­deo);
- para o modo real: Codex CLI instalado e autenticado (`codex login`), alÃ©m das chaves Agnes e ElevenLabs.

## Executar

```bash
cp .env.example apps/server/.env
pnpm install
pnpm dev
```

Abra `http://localhost:5173`. A configuraÃ§Ã£o padrÃ£o usa Codex OAuth, ElevenLabs e Agnes reais. Preencha `apps/server/.env` antes de gerar voz ou imagens. Para testes explÃ­citos, cada adaptador pode ser trocado individualmente para `mock`.

Com `HOST=0.0.0.0`, a aplicaÃ§Ã£o compilada tambÃ©m fica disponÃ­vel na rede local em `http://IP-DA-MAQUINA:3333`. NÃ£o encaminhe essa porta para a internet: esta versÃ£o nÃ£o possui login de usuÃ¡rio.

O arquivo deve ficar em `apps/server/.env`, pois os scripts filtrados do pnpm executam o servidor a partir desse diretÃ³rio. O OAuth do Codex nÃ£o Ã© salvo no `.env`; o servidor reutiliza a sessÃ£o local criada por `codex login`.

## Comandos

```bash
pnpm dev        # web + API em modo desenvolvimento
pnpm test       # testes da interface e API
pnpm typecheck  # validaÃ§Ã£o TypeScript
pnpm build      # aplicaÃ§Ã£o de produÃ§Ã£o
pnpm start      # serve API, mÃ­dia e web compilada
```

### OperaÃ§Ã£o em segundo plano

```bash
./start.sh      # inicia e grava PID/log em .runtime/
./stop.sh       # encerra somente o processo registrado
./atualiza.sh   # atualiza, valida, compila e reinicia
```

O `atualiza.sh` cancela a atualizaÃ§Ã£o quando encontra alteraÃ§Ãµes locais no Git, evitando sobrescrevÃª-las.

`pnpm start` nÃ£o compila a aplicaÃ§Ã£o. Para executar a versÃ£o de produÃ§Ã£o, rode primeiro `pnpm build`, depois `pnpm start`, e abra `http://localhost:3333` (ou a porta definida por `PORT`).

## ConfiguraÃ§Ã£o

As variÃ¡veis disponÃ­veis estÃ£o documentadas em [`.env.example`](./.env.example). As essenciais sÃ£o:

- `CODEX_PROVIDER=codex`: usa o Codex OAuth real;
- `VOICE_PROVIDER=elevenlabs`: usa voz real da ElevenLabs;
- `IMAGE_PROVIDER=agnes` e `VIDEO_PROVIDER=agnes`: usam mÃ­dia real da Agnes;
- qualquer provedor pode receber `mock` somente em testes controlados;
- `PORT` e `WEB_ORIGIN`: porta da API e origem permitida pelo CORS no desenvolvimento;
- `DATA_DIR`: banco SQLite e mÃ­dias, resolvidos a partir da raiz do repositÃ³rio;
- `CODEX_PATH` e `CODEX_MODEL`: executÃ¡vel e modelo opcionais do Codex;
- `AGNES_*` e `ELEVENLABS_*`: credenciais e modelos dos provedores reais.

O perfil recomendado de vÃ­deo Ã© `agnes-video-2.5-flash`. Ele trabalha com clipes de 4â€“12 segundos; o VideoSub usa a duraÃ§Ã£o real da voz para decidir automaticamente quantos clipes gerar por cena. A interface tambÃ©m aceita mÃºsica local, volumes separados e ediÃ§Ã£o do roteiro por prompt.

Os projetos ficam em `data/videosub.db` e as mÃ­dias em `data/projects/<id>/`. O diretÃ³rio inteiro Ã© ignorado pelo Git.

Consulte [a documentaÃ§Ã£o do projeto](./docs/README.md), [os provedores previstos](./docs/PROVEDORES-E-FERRAMENTAS-v1.01.00.md) e [o sistema visual](./DESIGN.md).

---

Projeto adaptado da comunidade INEMA TDS - Marca Arckhe