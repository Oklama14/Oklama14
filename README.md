<div align="center">

# Arthur Chaves Sousa

**Analista de QA · Automação de Testes com Playwright e TypeScript**

Porto Alegre, RS · Mestrando em Ciência da Computação @ PUCRS

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0A66C2?style=flat-square&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/artcs14/)
&nbsp;
[![Email](https://img.shields.io/badge/artcs2003@gmail.com-EA4335?style=flat-square&logo=gmail&logoColor=white)](mailto:artcs2003@gmail.com)

</div>

Analista de QA na **Segdev**, onde automatizo o ciclo de apólices de seguro garantia —
mais de 39 fluxos em Python, com mutation testing, análise de falhas assistida por LLM e
rastreamento histórico integrado ao Jira.

Meu foco é reduzir o que precisa ser testado à mão e confiar no que sobra. No portfólio
público mantenho duas suítes em Playwright, somando **158 testes** sobre interface,
contrato de API, segurança, acessibilidade e cena 3D em WebXR.

📌 **Aberto a oportunidades em QA Automation / SDET** — remoto ou Porto Alegre.

---

## 🎭 Automação de Testes

### [devcomply-e2e](https://github.com/Oklama14/devcomply-e2e)

**82 testes** em Playwright + TypeScript sobre uma aplicação Angular 20 / NestJS 11,
executados em três engines de browser e publicados a cada push.
**[→ Relatório da última execução](https://oklama14.github.io/devcomply-e2e/)**

| Área | Testes | Cobertura |
| --- | :---: | --- |
| Autenticação (UI) | 12 | login, cadastro, logout, validações, aceite de políticas |
| Proteção de rotas | 14 | rotas privadas, token expirado, token malformado, wildcard |
| Projetos (UI) | 9 | CRUD, isolamento entre contas, XSS refletido, estado vazio |
| Checklist LGPD | 9 | carga das perguntas, persistência, progresso, base legal |
| Relatório com IA | 6 | sucesso, erro 500, cota estourada, validação de payload |
| API (contrato) | 19 | CRUD, IDOR, rate limit, vazamento de dados sensíveis |
| Acessibilidade | 13 | axe-core em 9 telas, navegação por teclado, rótulos |

**Decisões que valem olhar**

- **Autenticação via API, não via UI.** Login pela interface a cada teste é lento e frágil.
  Um projeto `setup` cria os usuários via API e monta o `storageState` com o JWT — o login
  pela interface continua testado, mas como funcionalidade, não como pré-requisito.
- **Orçamento de autenticação entre processos.** A API limita `/auth` a 5 req/min e os
  workers do Playwright são processos separados, então um contador em memória não
  resolveria. A suíte usa um *token bucket* persistido em arquivo com lock — e ainda testa
  o rate limit explicitamente, verificando o `429`.
- **Dados descartáveis.** Cada execução cria contas próprias e as remove no teardown via
  `DELETE /users/me`, o mesmo recurso de exclusão que a LGPD exige da aplicação.
- **IA mockada por padrão.** As rotas do Gemini são interceptadas para exercitar sucesso,
  erro 500, cota estourada e timeout. Só o teste `@real` bate no provedor de verdade.

`@smoke` `@regression` `@security` `@a11y` `@api` `@e2e` `@responsive` `@real`

<br/>

### [vr-narrativas-e2e](https://github.com/Oklama14/vr-narrativas-e2e)

**76 testes** em 11 arquivos sobre uma aplicação WebXR que vive inteira dentro de um
único `<canvas>`. Sem DOM, sem `getByRole`, sem `locator.click()`.

A suíte pega a posição de mundo da entidade A-Frame, projeta pela câmera ativa e clica no
pixel resultante — deixando o raycaster resolver, como aconteceria com o usuário:

```ts
alvo.object3D.getWorldPosition(ponto);  // posição de mundo da entidade
ponto.project(camera);                  // projeção pela câmera ativa
await page.mouse.click(x, y);           // clique no pixel → raycaster do A-Frame
```

Se o objeto estiver invisível, coberto ou fora do campo de visão, o teste falha — e o erro
descreve a causa em vez de estourar um timeout mudo.

| Área | Cobertura |
| --- | --- |
| Jornadas | mapa → drill-down → indicadores → linha do tempo → composição → comparação regional → tour guiado |
| Regras de interação | em cada etapa, só os objetos daquela etapa podem ser clicáveis |
| Integridade dos dados | séries alinhadas, percentuais na faixa válida, nacional = soma das regiões |
| Desktop × VR | cursor por mouse no desktop, gaze com `fuse` ao entrar em VR |
| Anti-regressão de texto | nenhuma manchete ou leitura derivada pode conter `NaN` / `undefined` |

**Dois bugs encontrados, mesma causa raiz.** Os botões "Voltar" ficam perto da borda do
campo de visão. Como o FOV do A-Frame é definido na vertical, o alcance horizontal depende
da proporção da janela:

| Viewport | Proporção | Voltar do drill | Voltar global |
| --- | :---: | --- | --- |
| 1440×720 | 2.00 | visível | visível |
| 1366×768 | 1.78 | visível | **fora da tela (−12 px)** |
| 1280×800 | 1.60 | **fora da tela (−58 px)** | **fora da tela (−84 px)** |

Em 16:10 o usuário que abre uma região **fica sem saída**. Nos dois casos
`object3D.visible === true` — um teste que checasse só visibilidade passaria. O defeito só
aparece porque a suíte projeta o objeto pela câmera antes de clicar.

Os testes correspondentes usam `test.fail()` condicional: falham como esperado onde o
defeito é conhecido, e **acusam erro no dia em que passarem**, avisando que o bug foi
corrigido.

---

## 🛠️ Stack

**Testes & Automação**

![Playwright](https://img.shields.io/badge/Playwright-2EAD33?style=flat-square&logo=playwright&logoColor=white)
![Cypress](https://img.shields.io/badge/Cypress-17202C?style=flat-square&logo=cypress&logoColor=white)
![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)
![k6](https://img.shields.io/badge/k6-7D64FF?style=flat-square&logo=k6&logoColor=white)
![Postman](https://img.shields.io/badge/Postman-FF6C37?style=flat-square&logo=postman&logoColor=white)
![Insomnia](https://img.shields.io/badge/Insomnia-4000BF?style=flat-square&logo=insomnia&logoColor=white)
![axe-core](https://img.shields.io/badge/axe--core-663399?style=flat-square)
![Jira](https://img.shields.io/badge/Jira-0052CC?style=flat-square&logo=jira&logoColor=white)

**Desenvolvimento**

![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=flat-square&logo=typescript&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=flat-square&logo=javascript&logoColor=black)
![Angular](https://img.shields.io/badge/Angular-DD0031?style=flat-square&logo=angular&logoColor=white)
![NestJS](https://img.shields.io/badge/NestJS-E0234E?style=flat-square&logo=nestjs&logoColor=white)
![Node.js](https://img.shields.io/badge/Node.js-339933?style=flat-square&logo=node.js&logoColor=white)
![Flutter](https://img.shields.io/badge/Flutter-02569B?style=flat-square&logo=flutter&logoColor=white)

**Infra & Dados**

![GitHub Actions](https://img.shields.io/badge/GitHub_Actions-2088FF?style=flat-square&logo=github-actions&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=flat-square&logo=postgresql&logoColor=white)
![Git](https://img.shields.io/badge/Git-F05032?style=flat-square&logo=git&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white)

**IA & Documentação**

![Gemini API](https://img.shields.io/badge/Gemini_API-4285F4?style=flat-square&logo=google&logoColor=white)
![Claude API](https://img.shields.io/badge/Claude_API-D97757?style=flat-square)
![LaTeX](https://img.shields.io/badge/LaTeX-008080?style=flat-square&logo=latex&logoColor=white)

---

## 🎓 Pesquisa

**Mestrado em Ciência da Computação — PUCRS** · 2026–2027

Minha linha é **visualizações narrativas em ambientes de realidade virtual**: como
narrativa, imersão e interação se combinam para tornar dados complexos compreensíveis
dentro de uma cena 3D. O trabalho cruza visualização de dados, UI/UX e IHC.

O laboratório VR que serve de base à pesquisa transforma os indicadores da Plataforma
Nilo Peçanha (2017–2024) numa narrativa navegável em WebXR — e é também o sistema sob
teste da suíte `vr-narrativas-e2e`.

📄 **DevComply** — submetido ao **IHC 2026**, trilha Ideias Inovadoras.

---

## 💻 Outros Projetos

**[DevComply](https://github.com/Oklama14/DevComply)** — assistente de conformidade com
LGPD para desenvolvedores. Analisa código e documentação em busca de gaps de privacidade
em tempo real. Angular · NestJS · Gemini.

**[Currículo](https://github.com/Oklama14/Curriculo)** — pipeline que extrai requisitos de
uma vaga, reescreve as experiências com LLM e gera o PDF final em LaTeX otimizado para
ATS. Python · Gemini · LaTeX.

**[LifeOS](https://github.com/Oklama14/LifeOS)** — sistema de produtividade pessoal para
metas, hábitos e rotinas. Iniciado em React, migrando para Flutter.

---

<div align="center">
  <sub>⚽ Visca el Barça</sub>
</div>
