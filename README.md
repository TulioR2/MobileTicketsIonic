# 🏥 MobileTicketsIonic

> Sistema de controle de atendimento em filas de laboratórios médicos, desenvolvido com **Ionic Angular + Capacitor**.

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Ionic](https://img.shields.io/badge/Ionic-7.x-3880FF?logo=ionic)
![Angular](https://img.shields.io/badge/Angular-17.x-DD0031?logo=angular)
![Capacitor](https://img.shields.io/badge/Capacitor-5.x-119EFF?logo=capacitor)

---

##  Sobre o Projeto

O **MobileTicketsIonic** é um aplicativo móvel e web para gerenciamento de filas em laboratórios médicos. O sistema permite a emissão de senhas por clientes (via totem), o controle de chamadas pela atendente e a exibição de um painel público com as últimas chamadas, além de relatórios diários e mensais.

---

## 🖥️ Telas do Sistema

### Totem de Emissão de Senhas
> O cliente escolhe o tipo de atendimento e recebe sua senha impressa no formato `YYMMDD-PPSQ`.

![Totem](docs/screenshots/totem.png)

### Painel da Atendente
> A atendente chama a próxima senha respeitando a ordem de prioridade, finaliza ou descarta atendimentos.

![Atendente](docs/screenshots/attendant.png)

### Painel Público de Chamados
> Exibe a senha em atendimento atual e as últimas 5 senhas chamadas.

![Painel](docs/screenshots/panel.png)

### Relatórios
> Relatórios diários e mensais com quantitativos por tipo, TM de atendimento e detalhamento por senha.

![Relatórios](docs/screenshots/reports.png)

---

##  Agentes do Sistema

| Agente | Sigla | Papel |
|--------|-------|-------|
| Agente Sistema | AS | Emite senhas e responde aos comandos |
| Agente Atendente | AA | Chama o próximo, realiza o atendimento no guichê |
| Agente Cliente | AC | Emite a senha no totem e aguarda no painel |

---

##  Tipos de Senha

| Tipo | Descrição | Tempo Médio |
|------|-----------|-------------|
| **SP** | Senha Prioritária (Idosos, Gestantes, PCD) | 15 min ± 5 min |
| **SG** | Senha Geral (Consultas, Coletas) | 5 min ± 3 min |
| **SE** | Retirada de Exames | 1 min (95%) ou 5 min (5%) |

---

##  Regras de Negócio

### Ordem de Prioridade
```
[SP] → [SE|SG] → [SP] → [SE|SG] → ...
```
- **SP** tem prioridade máxima
- Após uma SP, atende-se SE (se houver), senão SG
- SG possui menor prioridade
- Nenhum guichê é exclusivo

### Formato da Numeração
```
YYMMDD-PPSQ
```
- `YY` = ano (2 dígitos)
- `MM` = mês (2 dígitos)
- `DD` = dia (2 dígitos)
- `PP` = tipo da senha (SP, SE, SG)
- `SQ` = sequência por prioridade, com reinício diário

### Expediente
- **Início:** 07:00
- **Fim:** 17:00
- Senhas não atendidas ao fim do expediente são descartadas

### Desistência
- **5%** das senhas emitidas não são atendidas (descartadas sem serviço)

---

## 📊 Relatórios

- Quantitativo geral de senhas emitidas e atendidas
- Quantitativo por prioridade (SP / SE / SG)
- Detalhamento: numeração, tipo, data/hora de emissão, data/hora de atendimento, guichê, status
- Tempo Médio de Atendimento (TM) por tipo

---

##  Como Executar

### Pré-requisitos
- Node.js >= 18
- npm >= 9
- Ionic CLI (`npm install -g @ionic/cli`)
- Angular CLI (`npm install -g @angular/cli`)

### Instalação

```bash
# Clone o repositório
git clone https://github.com/SEU_USUARIO/MobileTicketsIonic.git
cd MobileTicketsIonic

# Instale as dependências
npm install

# Execute no navegador
ionic serve
```

### Build para produção

```bash
ionic build --prod
```

### Executar no Android (via Capacitor)

```bash
ionic build
npx cap add android
npx cap sync
npx cap open android
```

### Executar no iOS (via Capacitor)

```bash
ionic build
npx cap add ios
npx cap sync
npx cap open ios
```

---

##  Estrutura do Projeto

```
MobileTicketsIonic/
├── src/
│   ├── app/
│   │   ├── models/
│   │   │   ├── ticket.model.ts       # Interface Ticket e enums
│   │   │   └── queue.model.ts        # Interface QueueState
│   │   ├── services/
│   │   │   └── queue.service.ts      # Toda a lógica de fila
│   │   ├── pages/
│   │   │   ├── totem/                # Tela do cliente
│   │   │   ├── attendant/            # Tela da atendente
│   │   │   ├── panel/                # Painel público
│   │   │   └── reports/              # Relatórios
│   │   ├── tabs/
│   │   │   └── tabs.page.ts          # Navegação por abas
│   │   ├── app.routes.ts
│   │   ├── app.config.ts
│   │   └── app.component.ts
│   ├── theme/
│   │   └── variables.scss
│   ├── global.scss
│   ├── main.ts
│   └── index.html
├── capacitor.config.ts
├── ionic.config.json
├── angular.json
├── tsconfig.json
├── .gitignore
├── LICENSE
└── README.md
```

---

##  Tecnologias Utilizadas

- [Ionic Framework 7](https://ionicframework.com/)
- [Angular 17](https://angular.io/)
- [Capacitor 5](https://capacitorjs.com/)
- [TypeScript 5](https://www.typescriptlang.org/)
- [RxJS 7](https://rxjs.dev/)

---

##  Capturas para o README

Para adicionar as imagens ao README, capture as telas após executar `ionic serve`:

1. Abra `/totem` → tire print do totem com uma senha gerada
2. Abra `/attendant` → tire print com a fila populada e um atendimento em curso
3. Abra `/panel` → tire print do painel com histórico de chamadas
4. Abra `/reports` → tire print do relatório diário com dados

Salve as imagens em `docs/screenshots/` e nomeie: `totem.png`, `attendant.png`, `panel.png`, `reports.png`.

---

##  Licença

Este projeto está licenciado sob a **MIT License** — veja o arquivo [LICENSE](LICENSE) para detalhes.

---

##  Contexto Acadêmico

Projeto desenvolvido como atividade prática da disciplina de **Desenvolvimento Mobile**, no curso de **Análise e Desenvolvimento de Sistemas** — **UNINASSAU**.

