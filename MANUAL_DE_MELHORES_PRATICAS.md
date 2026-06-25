# Manual de Melhores Práticas - Everything Claude Code (ECC)

Este manual consolida as melhores práticas e fluxos de trabalho para extrair o máximo de desempenho e eficiência do sistema **Everything Claude Code (ECC)**.

## 1. Princípios Essenciais

1. **Agent-First (Priorize os Agentes):** Delegue tarefas para os subagentes especializados. O sistema funciona melhor quando cada agente cuida do seu respectivo domínio, em vez de depender de uma inteligência genérica.
2. **Test-Driven (Orientado a Testes):** Escreva testes antes da implementação. Exige-se sempre um mínimo de 80% de cobertura.
3. **Security-First (Segurança em Primeiro Lugar):** Nunca comprometa a segurança para ganhar velocidade. Valide todos os inputs em fronteiras do sistema e não confie em dados externos.
4. **Imutabilidade:** Sempre crie novos objetos com as devidas alterações, em vez de alterar/mutar os objetos e estados existentes.
5. **Planeje Antes de Executar:** Projetos, migrações e funcionalidades complexas devem sempre começar pela elaboração de um plano.

## 2. Orquestração e Escolha de Agentes

Sempre invoque ou delegue tarefas para os agentes certos no momento em que precisar agir:

- **`planner`**: Use ao iniciar o desenvolvimento de funcionalidades complexas ou antes de realizar refatorações estruturais.
- **`tdd-guide`**: Sempre que for criar uma nova funcionalidade ou corrigir bugs, garantindo o ciclo RED-GREEN-REFACTOR.
- **`code-reviewer`**: Assim que terminar de escrever ou modificar um código, para checar a qualidade e a manutenibilidade.
- **`security-reviewer`**: Antes de commits que envolvam autenticação, manipulação de segredos ou endpoints sensíveis.
- **`build-error-resolver`**: Para diagnosticar e resolver falhas de tipagem ou de compilação/build.
- **`architect`**: Para decisões importantes de arquitetura de software, patterns ou escalabilidade.

## 3. Fluxo de Trabalho (Workflow)

Para garantir qualidade, siga este ciclo rigorosamente:

1. **Plan (Planejamento):** Analise o problema, identifique as dependências, quebre o trabalho em pequenas fases e verifique a arquitetura do projeto.
2. **TDD:** Escreva o teste (ele deve falhar primeiro). Em seguida, faça a implementação mínima viável que o faça passar. Refatore o código para atingir métricas adequadas sem quebrar o teste.
3. **Review (Revisão):** Use o agente de revisão de código, lide com todas as pendências Críticas e Altas.
4. **Knowledge Capture (Documentação):** Registre decisões de API, runbooks ou notas cruciais diretamente na documentação correta do projeto. Não duplique dados.
5. **Commit:** Utilize os *Conventional Commits* (`feat:`, `fix:`, `refactor:`, `test:`, `docs:`, `chore:`, `perf:`).

## 4. Escrevendo Códigos de Alta Qualidade

- **Funções e Arquivos Compactos:** Mantenha arquivos focados em um propósito (coesão alta, baixo acoplamento). Idealmente com 200 a 400 linhas (com limite absoluto de 800 linhas). Funções devem ter menos de 50 linhas.
- **Prevenção de Nesting Profundo:** Evite aninhamentos complexos (profundidade maior do que 4 níveis). Faça *early returns*.
- **Tratamento Rigoroso de Erros:** Não silencie exceções com *swallow errors*. Mostre mensagens amigáveis na UI, mas exiba logs completos com contexto estruturado nos servidores.
- **Boas Práticas e Arquitetura:** Respeite padrões como o *Repository Pattern*, onde as lógicas dependem de uma interface abstrata em vez do banco de dados diretamente. Utilize formatação unificada da API (com status de sucesso, payload de dados e detalhes sobre a página/erro).

## 5. Segurança Oobrigatória

- **Segredos (Secrets):** JAMAIS insira chaves de API, senhas ou tokens direto no código-fonte. Utilize variáveis de ambiente ou gerenciadores de segredos.
- **Limpeza e Proteção:** Parametrize queries de SQL para barrar *SQL Injection*, proteja as rotas com autenticação confiável, e habilite limitadores de taxa (Rate Limiting).
- Caso ocorra exposição ou um risco de segurança se comprove: **PARE**. Acione o revisor de segurança e rotacione as credenciais comprometidas o mais rápido possível.

## 6. Utilização das Skills (Habilidades) do ECC

As *skills* substituem os tradicionais *slash-commands* (`/`) do passado e são as superfícies canônicas para fluxos:
- Familiarize-se com habilidades específicas para o framework da sua stack (Ex.: `frontend-patterns`, `django-patterns`, etc.).
- Sempre que houver incertezas sobre qual componente instalar ou utilizar no ECC, consulte via terminal usando o utilitário nativo: `npx ecc consult "o problema que desejo resolver"`.

## 7. Performance e Gestão de Contexto

Evite sobrecarregar o modelo com lixo não utilizado. Em refatorações gigantes:
- Concentre-se nas partes vitais; evite ultrapassar 80% do uso da janela de contexto sem necessidade.
- Se o assistente parecer preso em erros ou esquecer instruções, aplique uma "compactação estratégica" (strategic compact) para condensar o log recente e abrir margem para novos passos precisos.

## 8. Dicionário de Agentes e Skills

Abaixo está a lista com os principais agentes e algumas das *skills* mais úteis disponíveis para integrar ao seu fluxo de trabalho:

### Agentes Especializados

| Agente | Função / Propósito | Quando Usar |
|--------|--------------------|-------------|
| **`planner`** | Planejamento da implementação | Funcionalidades complexas, refatorações pesadas e desenho de arquitetura inicial. |
| **`architect`** | Design e Escalabilidade de Sistemas | Em decisões cruciais de design de arquitetura técnica e abstrações de sistema. |
| **`tdd-guide`** | Orientador de TDD | Criação de novas features e correção de bugs estritamente via ciclos de testes. |
| **`code-reviewer`** | Revisão de Qualidade | Imediatamente após construir, modificar ou refatorar componentes/funções de código. |
| **`security-reviewer`** | Detecção de Vulnerabilidades | Antes de qualquer *commit* sensível ou na implementação de endpoints e autenticações. |
| **`build-error-resolver`**| Diagnóstico de Falhas de Compilação | Durante erros de tipagem obscuros (ex.: TypeScript complexos) e falhas no build ou CI/CD. |
| **`database-reviewer`** | Especialista em PostgreSQL e Supabase | Design de esquemas de DB, regras complexas de RLS (Row Level Security) e análise de queries. |
| **`e2e-runner`** | Especialista em Automação | Ao desenhar e debugar testes *End-to-End* críticos utilizando o Playwright. |
| **`refactor-cleaner`** | Manutenção de Débito Técnico | Na limpeza de código morto (dead code) e durante faxinas na base de código. |
| **`doc-updater`** | Sincronizador de Documentação | Para alinhar o conteúdo dos READMEs, Swagger e anotações com o estado atual da base de código. |
| **Revisores de Linguagem** | `typescript-reviewer`, `python-reviewer`, `java-reviewer`, `go-reviewer`, `rust-reviewer` | Agentes super-especializados com as *melhores práticas* e segurança específicas de cada linguagem. |
| **`loop-operator`** | Gerenciador de Operações Autônomas | Monitorar sessões em loop autônomo e intervir de forma inteligente. |

### Principais Skills (Comandos de Fluxo)

*As skills agrupam o roteiro que o modelo deve seguir e conectam os passos diretamente ao agente apropriado.*

- **`/plan`**: Dispara o *planner* para elaborar um *Implementation Plan* estruturado antes de alterar um código sequer.
- **`/security-scan`**: Executa a rigorosa suíte de testes e checklist do *AgentShield* sobre todo o código local.
- **`tdd-workflow`**: Dispara o roteiro obrigatório do TDD passo-a-passo e garante que uma meta de 80%+ de test-coverage seja alcançada.
- **`coding-standards`**: Habilidade fundamental que força todos os arquivos do projeto a se basearem em imutabilidade e a não passarem de 800 linhas.
- **`backend-patterns` / `frontend-patterns`**: Guias para padrões arquitetônicos que o modelo deve respeitar na construção de interfaces em React, Next.js ou APIs do lado do servidor (Node, Express, etc).
- **`api-design`**: Skill de *design-first* que ensina as regras para o uso de paginação padronizada, roteamento RESTful consistente e envelopes padronizados nas respostas a erros do sistema.
- **`/configure-ecc`**: Skill focada em gerenciar (wizard iterativo) a própria instalação e atualização das ferramentas do Everything Claude Code dentro do seu ecossistema pessoal.
