# Meu Bolso — controle de gastos offline

App de gastos para Android. Os dados ficam salvos só no celular, sem servidor e sem login. Para trocar de aparelho, use o backup em arquivo.

## Estrutura

```
www/                  ← o app inteiro (index.html com HTML+CSS+JS) + manifest/PWA
assets/               ← ícones usados para gerar o APK
capacitor.config.json ← config do empacotamento Android
.github/workflows/    ← gera o APK automaticamente no GitHub
netlify.toml          ← versão web (publica a pasta www)
```

## Gerar o APK (sem instalar nada no PC)

1. Suba esta pasta para um repositório no GitHub (branch `main`).
2. Abra a aba **Actions**. O workflow **Gerar APK** roda sozinho a cada push e leva cerca de 5 minutos.
3. Quando terminar, entre no run e baixe o artefato **meu-bolso-apk**. É um .zip com o `app-debug.apk` dentro.
4. Mande o APK para o celular, abra o arquivo e permita "instalar apps desconhecidos".

Para publicar na Play Store mais tarde, é preciso gerar um APK/AAB assinado (release). O APK atual é de testes (debug).

## Versão web (opcional)

Conecte o mesmo repositório ao Netlify. Ele publica a pasta `www`, que pode ser instalada como app pelo Chrome ("Adicionar à tela inicial").
Na versão web, os resumos agendados não funcionam. Só o APK agenda notificações.

## Como funciona

- **Pode gastar hoje**: soma o que sobra nas categorias do tipo *Livre* e divide pelos dias que faltam no mês.
- **Tipos de categoria**:
  - *Obrigatório*: contas do mês.
  - *Livre*: dia a dia, lazer.
  - *Guardar*: investimento e poupança; abastece as metas.
  - *Caixinha*: manutenção; o que sobra no mês acumula para o mês seguinte.
- **Contas obrigatórias**: podem ter valor fixo ou variável. As variáveis usam a média dos últimos 3 pagamentos como previsão.
- **Cartão parcelado**: cada parcela é lançada no mês dela.
- **Notificações (APK)**:
  - resumo diário no horário escolhido;
  - resumo semanal no domingo;
  - aviso de conta 2 dias antes do vencimento e no dia;
  - alerta quando uma categoria passa de 80% ou de 100%.

  Os resumos são calculados com os dados do último uso do app. Abrir o app recalcula tudo.
- **Backup**: em Ajustes → Exportar backup, o app gera um `.json` e abre o menu de compartilhar (Drive, WhatsApp, e-mail). No celular novo, use *Importar backup* na primeira tela.
- **Simulador de aluguel**: mostra o peso do aluguel na renda e cria uma meta de caução. Também pode aplicar a mudança no orçamento.

## Editar

Toda a lógica está em `www/index.html`. Depois de alterar, faça commit e push: o GitHub gera um APK novo.
