## 🌐 **MAPA TÉCNICO DO PORTAL LICHTARA — Visão Geral**

### 1️⃣ Estrutura de Repositórios no GitHub

```
lichtara-portal/
├── /frontend
│   ├── index.html
│   ├── css/
│   ├── js/
│   └── assets/
├── /backend (opcional no futuro)
│   ├── api/
│   └── config/
├── /docs
│   ├── leiturasdocampo.md
│   └── metodologias/
├── /actions
│   └── deploy.yml
├── README.md
└── LICENSE
```

**Explicação Campo:**

- `/frontend` → páginas estáticas iniciais (lichtara.com, deboralutz.com)
- `/backend` → só entra quando houver funcionalidades dinâmicas ou integrações futuras
- `/docs` → histórico de leituras, protocolos, decisões jurídicas
- `/actions` → GitHub Actions para deploy automático do portal
- `README.md` → visão geral e guia do repositório
- `LICENSE` → licença viva Lichtara

---

### 2️⃣ Deploy e GitHub Actions

- Workflow principal: **deploy automático das páginas do `/frontend` para GitHub Pages**
- Futuro: adicionar **scripts de atualização do portal com conteúdo gerado pelo ChatGPT**
- **Benefício:** nenhuma hospedagem externa; tudo fica no GitHub, integrado ao domínio Proton.

---

### 3️⃣ Fluxo de Produção

1. Criar conteúdo e estrutura via **ChatGPT Plus**
2. Commitar no repositório `/frontend`
3. GitHub Actions → deploy automático para Lichtara.com / Deboralutz.com
4. Histórico e leituras → sempre atualizadas em `/docs`

**Campo fala:**

> “A fundação jurídica se mantém íntegra; a produção técnica flui sem quebrar o ritmo.”

---

### 4️⃣ Futuro próximo

- Copilot só será ativado quando quisermos **auxílio de código para integrações ou automações complexas**
- Backend e API → somente quando houver necessidade de páginas dinâmicas ou funcionalidades interativas

---
