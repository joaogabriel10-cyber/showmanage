# Projeto: Site Showmanager Eventos

> *Realizando momentos, criando histórias.* · Lema: *Elegância em cada detalhe.*
>
> Protótipo de referência: `docs/prototipo.png`

**Versão do documento:** 1.1 · **Data:** 09/11/2026

> **Aviso:** o projeto é apoiado pela Claude AI (plano gratuito, com limites de uso por período). Use este arquivo como fonte única da verdade, compartilhe-o em novas sessões para manter o contexto e **valide todo conteúdo gerado por IA** (textos institucionais, depoimentos, fotos e dados comerciais) antes de publicar.

---

## 1. Visão geral

**Objetivo geral:** desenvolver um site profissional para a Showmanager Eventos, empresa especializada em planejamento, organização e decoração de eventos, facilitando o contato entre a empresa e seus clientes.

**Público-alvo:** pessoas que desejam organizar casamentos, aniversários, formaturas, festas de 15 anos, eventos empresariais, chás e confraternizações.

**Serviços oferecidos:**
- Organização de casamentos
- Festas de aniversário
- Formaturas
- Festas de 15 anos
- Eventos corporativos
- Decoração de festas
- Chás e confraternizações

**Proposta de valor:** elegância, criatividade e profissionalismo, do primeiro contato até o último brinde.

**Conceito central — "Luxo Discreto":** interface editorial, elegante e focada em conversão (pedidos de orçamento), sem excessos nem promessas absolutas. **Tom de voz:** acolhedor, seguro, organizado e inspirador.

> **Slogan:** o protótipo usa "Realizando momentos, criando histórias." (adotado no site). "Transformando momentos em memórias inesquecíveis." aparece em versões anteriores do material e pode servir de texto de apoio, a decidir.

---

## 2. Tecnologias

| Camada | Tecnologia | Função |
|---|---|---|
| Assistente de IA | Claude | Gerar código, ideias de design, corrigir erros e organizar funcionalidades |
| Linguagem | Python | Lógica do sistema: cadastro de clientes, orçamentos, gestão de eventos |
| Framework | Django | Estrutura do site: páginas, formulários, painel administrativo |
| Banco de dados | PostgreSQL | Armazenar clientes, contatos, tipos de evento, datas, locais e orçamentos |
| Design | Figma | Desenhar as telas antes da programação |
| Front-end | HTML, CSS e um pouco de JavaScript | Páginas responsivas (celular e computador) |
| Fontes | Playfair Display, Great Vibes, Montserrat | Identidade tipográfica |

> Em desenvolvimento local, o projeto usa SQLite por padrão; o PostgreSQL é ativado por variáveis de ambiente (`DB_NAME`, `DB_USER`, `DB_PASSWORD`, `DB_HOST`, `DB_PORT`).

---

## 3. Identidade visual (conforme protótipo)

| Cor | Hex | Uso |
|---|---|---|
| Preto | `#0A0807` | Fundo principal |
| Carbono | `#1A1510` | Cartões e campos |
| Dourado | `#C9A45C` | Linhas, ícones, botões |
| Champagne | `#E6C984` | Destaques e gradientes |
| Bege / Marfim | `#F3EBDC` | Textos e fundo claro |
| Taupe | `#B7A98F` | Texto secundário e bordas |

**Tipografia:** Playfair Display (títulos) · Montserrat (textos) · Great Vibes (destaques).
**Logo:** monograma "S" em script com brilho, SHOWMANAGER em maiúsculas e "E V E N T O S" espaçado.
**Direção de arte:** contraste escuro + dourado · respiro generoso · fotografia editorial de alta qualidade · bordas finas e curvas suaves (raio 12–16 px) · ícones minimalistas · evitar ornamentos em excesso · espaçamento em múltiplos de 8 px.
**Componentes:** botão dourado preenchido, botão contorno, pílulas de filtro, ícones de linha dourados, redes sociais (Instagram, WhatsApp, YouTube, TikTok).

---

## 4. Estrutura do site (conforme protótipo)

```
Início
├── Serviços   → detalhes dos serviços
├── Galeria    → fotos dos eventos (filtros: Todos, Casamentos, Aniversários, Formaturas, 15 Anos…)
├── Sobre      → nossa história e valores (+10 anos, +500 eventos, 100% satisfeitos)
└── Contato    → "Solicite um Orçamento" + Fale Conosco + Nos siga
```

| Tela do protótipo | Conteúdo |
|---|---|
| 01 Página inicial | Hero "Realizando momentos, criando histórias", 2 botões, 4 diferenciais (Planejamento Personalizado, Equipe Especializada, Decoração Exclusiva, Momentos Inesquecíveis) |
| 02 Serviços | Cartões com foto + cartão "Conte com a Showmanager…" |
| 03 Galeria | Pílulas de filtro, grade de fotos, "Ver mais fotos" |
| 04 Sobre nós | Texto, foto, estatísticas |
| 05 Orçamento e Contato | Formulário, e-mail, telefone, redes |
| 06 Versão mobile | Menu hambúrguer e layout em coluna |
| 08 Elementos extras | Newsletter, depoimento com estrelas, banner final |

**Fluxo do cliente:** visita o site → conhece os serviços → preenche o orçamento → solicitação salva no banco → empresa vê no `/admin` e responde.

---

### Responsividade

| Dispositivo | Largura | Comportamento |
|---|---|---|
| Mobile | ≤ 640 px | Uma coluna, formulário empilhado, botões em largura total |
| Tablet | 641–900 px | Duas colunas nos serviços, navegação compacta |
| Desktop | > 900 px | Grade ampla, hero em duas colunas, menu completo |

Alvos de toque de pelo menos 44×44 px, foco visível em dourado, títulos curtos no mobile. Os breakpoints são recomendações iniciais: **validar em dispositivos reais**.

---

## 5. Modelo de dados

### 5.1 Site (app `eventos`)

| Modelo | Campos principais |
|---|---|
| **SolicitacaoOrcamento** | nome, e-mail, telefone, tipo de evento, data, local, nº de convidados, mensagem, status (novo / em contato / fechado / perdido), criado em |
| **Newsletter** | e-mail, criado em |
| **Servico** | título, descrição, tipo de evento, imagem, ordem |
| **FotoGaleria** | título, categoria, imagem, evento de origem, destaque na home, criado em |
| **Depoimento** | nome, texto, estrelas, foto, evento, aprovado para o site |

### 5.2 Gestão interna (app `gestao`)

Banco relacional para a operação da empresa, com foco em **permutas** e **divisão de setores**. Disponível no `/admin`.

| Módulo | Tabelas |
|---|---|
| Clientes e projetos | `Cliente` (nome, e-mail, WhatsApp, cadastro) · `CategoriaEvento` · `Evento` (cliente, categoria, título, data, local, status, orçamento) |
| Setores e equipe | `Setor` · `Colaborador` (setor, vínculo, contato, ativo) |
| Fornecedores e permutas | `Fornecedor` (razão social, categoria, modelo: pago / permuta / patrocínio / misto) · `AcordoPermuta` (evento, fornecedor, recebido, oferecido, status) |
| Tarefas (Kanban) | `Tarefa` (evento, setor, colaborador, título, status Kanban, prazo, prioridade) |
| Financeiro | `Lancamento` (evento, fornecedor, receita/despesa, valor, status) |
| Portfólio e site | `FotoGaleria` e `Depoimento` ligados ao `Evento` |

**Regras:** 1 Cliente → N Eventos · 1 Evento → N Tarefas, N Permutas, N Lançamentos, N Mídias e N Depoimentos · 1 Setor → N Colaboradores e N Tarefas.

**Atalho:** em *Solicitações de orçamento*, a ação **"Converter em Cliente + Evento"** cria o cliente e o evento na Gestão e marca a solicitação como fechada.

---

## 6. Fases do projeto

### Fase 0 — Planejamento ✅ *concluída*
- [x] Definir objetivo, público-alvo e serviços
- [x] Definir nome da empresa: **Showmanager Eventos**
- [x] Escolher estilo visual (elegante e sofisticado) e paleta preto/dourado
- [x] Reunir referências visuais de sites e identidade de marca
- [x] Definir a stack (Python, Django, PostgreSQL, Figma, Claude)
- [x] Criar o layout de referência da página inicial

### Fase 1 — Design e protótipo 🔄 *em andamento*
- [x] Layout da página inicial
- [x] Protótipo de todas as telas, versão mobile, fluxo e elementos extras (`docs/prototipo.png`)
- [ ] Exportar o logotipo final em SVG/PNG a partir do protótipo
- [ ] Reunir fotos reais em alta resolução (hero, serviços, galeria, sobre) — hoje o site usa placeholders
- [ ] Guia de estilo (cores, tipografia, botões, ícones, espaçamentos)

### Fase 2 — Desenvolvimento do site (MVP) 🔄 *código entregue, falta rodar e validar*
- [x] Projeto Django estruturado (`config/` + apps `eventos/` e `gestao/`)
- [x] Módulo de gestão interna (clientes, eventos, setores, permutas, tarefas Kanban, financeiro)
- [x] Lightbox na galeria, foco visível e alvos de toque de 44 px
- [x] Modelos, painel administrativo e migrações a gerar
- [x] Páginas conforme protótipo: Início, Serviços, Galeria, Sobre, Orçamento e Contato (unificados), Newsletter
- [x] Site adequado ao protótipo (identidade, hero, diferenciais, estatísticas, depoimento, banner)
- [x] Formulários de orçamento e contato salvando no banco
- [x] CSS responsivo no estilo preto e dourado
- [x] Botão flutuante de WhatsApp
- [ ] Rodar `makemigrations eventos gestao` e `migrate` e testar localmente
- [ ] Criar superusuário e cadastrar serviços, fotos e depoimentos reais
- [ ] Trocar WhatsApp, e-mail e Instagram pelos dados reais
- [ ] Testar formulários, filtros e navegação em celular e computador

### Fase 3 — Conteúdo e qualidade 📅 *próxima*
- [ ] Textos finais (Sobre nós, descrição de cada serviço)
- [ ] Fotos otimizadas (WebP, tamanhos adequados) com texto alternativo
- [ ] SEO básico: títulos, meta descrições, `sitemap.xml`, `robots.txt`
- [ ] Favicon e imagem de compartilhamento (Open Graph)
- [ ] Acessibilidade: validar contraste, estados de foco e navegação por teclado; `alt` em todas as imagens
- [ ] Testes automatizados dos formulários e views
- [ ] Revisão de português e de links

### Fase 4 — Publicação 📅 *próxima*
- [ ] Escolher hospedagem (Render, Railway, Fly.io, VPS ou similar)
- [ ] Banco PostgreSQL em produção
- [ ] Registrar domínio (ex.: `showmanager.com.br`) e configurar HTTPS
- [ ] Variáveis de ambiente seguras (`SECRET_KEY`, `DEBUG=0`, `ALLOWED_HOSTS`)
- [ ] Servir arquivos estáticos e de mídia (WhiteNoise + armazenamento de imagens)
- [ ] Backup automático do banco
- [ ] Google Analytics ou alternativa e Google Search Console
- [ ] Cadastro no Google Meu Negócio

### Fase 5 — Operação e divulgação 🔮 *futura*
- [ ] Notificação por e-mail a cada nova solicitação de orçamento
- [ ] Resposta automática ao cliente ("recebemos seu pedido")
- [ ] Integração com Instagram (feed de fotos recentes)
- [ ] Campanhas: Instagram, Google Ads, parcerias com buffets, fotógrafos e espaços
- [ ] Coletar depoimentos de clientes após cada evento

### Fase 6 — Evolução do sistema 🔮 *futura*
- [ ] Quadro Kanban visual para as tarefas (hoje via `/admin`) e painel próprio além do `/admin`
- [ ] Relatórios financeiros por evento (receitas × despesas × permutas)
- [ ] Agenda de eventos com calendário e bloqueio de datas ocupadas
- [ ] Gestão de clientes (histórico, observações, funil de vendas)
- [ ] Orçamentos com valores: pacotes, itens e geração de PDF
- [ ] Contratos e pagamentos (PIX / cartão) integrados
- [ ] Área do cliente: acompanhar o planejamento do seu evento
- [ ] Blog com dicas de decoração e planejamento
- [ ] Versão em outros idiomas (se houver demanda)

---

## 7. Melhorias sugeridas

### Prioridade alta (impacto direto em vendas)
| Melhoria | Por quê |
|---|---|
| Fotos reais de alta qualidade | É o maior fator de confiança em um site de eventos |
| E-mail e WhatsApp automáticos ao receber orçamento | Resposta rápida aumenta muito a conversão |
| Botão "Solicitar orçamento" em todas as páginas | Reduz o caminho até o contato |
| Proteção anti-spam nos formulários (honeypot ou reCAPTCHA) | Evita lixo no banco de dados |
| Máscara e validação de telefone e data | Menos dados incorretos |

### Prioridade média
| Melhoria | Por quê |
|---|---|
| Carregamento sob demanda (lazy load) na galeria (o lightbox já existe) | Melhor experiência e site mais rápido |
| Página individual por tipo de evento (ex.: `/casamentos`) | Melhora o SEO e a conversão |
| Pacotes e faixas de preço ("a partir de") | Filtra clientes e alinha expectativas |
| Perguntas frequentes (FAQ) | Reduz dúvidas repetidas |
| Animações suaves ao rolar a página | Aparência mais sofisticada |
| Vídeo curto de apresentação no hero | Passa a emoção dos eventos |

### Prioridade baixa / futuro
| Melhoria | Por quê |
|---|---|
| Modo claro (bege) opcional | Alternativa visual |
| Simulador de orçamento online | Autoatendimento do cliente |
| Chatbot com IA para dúvidas iniciais | Atendimento 24 h |
| Aplicativo (PWA) para a equipe | Gestão em campo |
| Programa de indicação de clientes | Crescimento orgânico |

### Segurança e qualidade técnica
- Gerar nova `SECRET_KEY` e nunca versionar senhas
- `DEBUG=0` em produção, HTTPS obrigatório
- Limitar tamanho e tipo dos uploads de imagem
- Cumprir a **LGPD**: aviso de privacidade, consentimento no formulário e política de dados
- Backups periódicos e teste de restauração
- Testes automatizados e revisão de código antes de cada publicação

---

## 8. Organização interna (setores)

| Setor | Função principal |
|---|---|
| Produção Geral | Visão macro, cronograma mestre, decisões finais |
| Financeiro e Parcerias | Fluxo de caixa, permutas e patrocínios |
| Marketing e Comunicação | Divulgação orgânica, redes sociais, imprensa |
| Logística e Operações | Espaço, licenças, fornecedores, montagem |
| Experiência e Conteúdo | Curadoria de atrações e atividades |

- **Método Kanban:** A Fazer → Em Negociação → Aguardando Aprovação → Fazendo → Concluído (já no sistema).
- **Reunião semanal de 30 min:** o que fizemos? o que faremos? há algum bloqueio?
- **Ferramentas de apoio:** Notion ou Trello (tarefas), Google Workspace (documentos), WhatsApp Business, Slack ou Discord (comunicação).

## 9. Estratégias de baixo orçamento ("grandes eventos, pequenos orçamentos")

1. **Permuta:** trocar visibilidade real (logo, menções, posts) por produtos e serviços — registrada em `AcordoPermuta`.
2. **Voluntários e estagiários:** parcerias com universidades (Turismo, Marketing, Produção Cultural), com certificado, networking e experiência.
3. **Marketing orgânico:** cenários "instagramáveis" e hashtag oficial para conteúdo gerado pelos clientes.
4. **Receita híbrida:** ingressos solidários, "pague o quanto puder", venda antecipada de kits de experiência.

---

## 10. Cronograma sugerido

| Fase | Duração estimada | Entrega |
|---|---|---|
| 1 — Design e protótipo | 1–2 semanas | Telas no Figma e logo |
| 2 — Desenvolvimento (MVP) | 1–2 semanas | Site rodando localmente |
| 3 — Conteúdo e qualidade | 1 semana | Site com conteúdo real |
| 4 — Publicação | 3–5 dias | Site no ar com domínio |
| 5 — Operação e divulgação | contínua | Primeiros orçamentos |
| 6 — Evolução | 2–6 meses | Sistema de gestão completo |

---

## 11. Ideias de nome (histórico)

Momentos Mágicos Eventos · Prime Eventos · Encanto Produções · Celebra Eventos · Infinity Eventos · Dreams Eventos.
**Nome escolhido: Showmanager Eventos.**

---

## 12. Como executar o projeto atual

```bash
python -m venv venv && source venv/bin/activate   # Windows: venv\Scripts\activate
pip install -r requirements.txt
python manage.py makemigrations eventos gestao
python manage.py migrate
python manage.py createsuperuser
python manage.py runserver
```

Site: http://127.0.0.1:8000 · Painel: http://127.0.0.1:8000/admin

---

## 13. Riscos e cuidados

| Risco | Mitigação |
|---|---|
| Site sem fotos reais parece genérico | Priorizar a coleta de fotos na Fase 1 |
| Orçamentos perdidos por demora na resposta | Notificação automática e rotina diária no painel |
| Perda de dados | Backups automáticos do PostgreSQL |
| Escopo crescer demais | Entregar o MVP primeiro e evoluir por fases |
| Dados pessoais de clientes (LGPD) | Política de privacidade e acesso restrito ao painel |

---

---

## 14. Checklist antes da publicação

- [ ] Substituir imagens demonstrativas por fotos autorizadas
- [ ] Confirmar textos institucionais, estatísticas (+10 anos, +500 eventos) e depoimentos reais
- [ ] Adicionar links oficiais de WhatsApp, e-mail e redes sociais (hoje são exemplos)
- [ ] Definir destino e responsável pelo formulário (painel, e-mail ou WhatsApp)
- [ ] Validar acessibilidade, contraste e estados de foco
- [ ] Testar responsividade em dispositivos reais e navegação por teclado
- [ ] Revisar dados comerciais (telefone, e-mail, endereço)

**Contatos (a confirmar):** contato@showmanager.com.br · (66) 99999-9999 · redes sociais a definir.

---

*Documento do projeto Showmanager Eventos — atualizar a cada fase concluída.*
