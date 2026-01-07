# BikeBoard — Testes E2E com Playwright

Resumo
---
Repositório de testes End-to-End usando Playwright para a aplicação estática servida a partir da pasta `src/`.

Pré-requisitos
---
- Node.js (recomenda-se v16+)
- npm (vem com o Node)
- Windows / macOS / Linux

Instalação das dependências do projeto
---
1. Clone o repositório e abra a pasta do projeto.
2. Instale dependências do Node:

```bash
npm install
```

3. Instale os navegadores e binários usados pelo Playwright (executar sempre após a instalação das deps):

```bash
npm run playwright:install
# ou
npx playwright install
# (em Linux, se necessário: npx playwright install-deps)
```

Como iniciar o servidor de desenvolvimento
---
O projeto serve a pasta `src/`. O Playwright espera que o site esteja disponível em `http://localhost:3000` conforme `playwright.config.js`.

- Iniciar com o script npm (padrão):

```bash
# inicia usando o script definido em package.json (usa 'serve -s src')
npm run start
```

- Se precisar garantir a porta 3000 (recomendado para rodar testes localmente):

```bash
# cross-platform (usa npx para garantir a versão local do serve)
npx serve -s src -l 3000
```

Executando os testes (modo prompt / CLI)
---
- Executar todos os testes:

```bash
npm test
# ou
npx playwright test
```

- Executar apenas um projeto (ex.: Chromium):

```bash
npm run test:chromium
# ou
npx playwright test --project=chromium
```

- Executar um único arquivo ou teste (pelo caminho ou nome):

```bash
npx playwright test playwright/e2e/ads.spec.js
npx playwright test -g "deve cadastrar um anúncio com sucesso"
```

- Verificar relatório HTML após a execução:

```bash
npx playwright show-report
# ou abrir playwright-report/index.html gerado
```

Executando testes em modo UI / Debug
---
- Abrir testes em modo de depuração (abre navegador UI e pausa em pontos interativos):

```bash
# Unix/mac
PWDEBUG=1 npm test
# PowerShell
$env:PWDEBUG=1; npm test
# Windows CMD
set PWDEBUG=1 && npm test
```

- Executar testes em modo visível (headed):

```bash
npm run test:headed
# ou
npx playwright test --headed
```

- UI do Test Runner (se disponível na versão instalada):

```bash
npm run test:ui
# ou
npx playwright test --ui
```

Exemplo de workflow (GitHub Actions) — minimal
---
```yaml
name: Playwright Tests

on: [push, pull_request]

jobs:
  e2e:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Setup Node
        uses: actions/setup-node@v4
        with:
          node-version: 18
      - name: Install deps
        run: npm ci
      - name: Install Playwright browsers
        run: npx playwright install --with-deps
      - name: Start server
        run: npx serve -s src -l 3000 &
      - name: Run tests (Chromium)
        run: npx playwright test --project=chromium --reporter=html
      - name: Upload report
        uses: actions/upload-artifact@v4
        with:
          name: playwright-report
          path: playwright-report
```

Estrutura de testes e padrões do repositório
---
- Tests: `playwright/e2e/` (ex.: `ads.spec.js`).
- Helpers e organização: `playwright/support/` contém `actions/`, `fixtures/` e `mocks/`.
  - `actions/` encapsula passos de UI (ex.: `bikeActions(page)` em `ads.actions.js`).
  - `fixtures/` contém dados reutilizáveis (ex.: `myBike` em `bike.js`).
  - `mocks/` contém interceptações de rede (ex.: `ads.mocks.js`), com flag `ENABLE_MOCKS` para ativar/desativar rapidamente.

Dicas rápidas
---
- Se os testes falharem por não encontrar o servidor, confirme se o site está rodando em `http://localhost:3000`.
- Use `--project=chromium` para reproduzir resultados idênticos aos do CI listado em `playwright.config.js`.
- Para investigar falhas, abra o relatório (`npx playwright show-report`) ou habilite `PWDEBUG`.

Assinado,

GLAUIO J SILVEIRA
