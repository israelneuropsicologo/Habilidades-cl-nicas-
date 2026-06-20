
```markdown
# [span_0](start_span)Novo E-SAÚDE - Sistema de Gestão Clínica Psicológica[span_0](end_span)

[span_1](start_span)Este repositório contém o planeamento estruturado e a arquitetura técnica da nova versão do sistema E-SAÚDE, uma plataforma concebida para ser um sistema de gestão clínica psicológica completo, sem erros, com validação robusta e arquitetura escalável[span_1](end_span).

## 🎯 Objetivo Geral
[span_2](start_span)[span_3](start_span)O propósito fundamental deste projeto é reestruturar o ecossistema técnico atual para mitigar falhas críticas de sincronização, ausência de validações nativas e limitações de desempenho, preparando a infraestrutura para suportar o crescimento a longo prazo[span_2](end_span)[span_3](end_span).

## [span_4](start_span)🚀 Funcionalidades e Requisitos[span_4](end_span)

### [span_5](start_span)Requisitos Funcionais[span_5](end_span)
* **[span_6](start_span)Autenticação e Autorização**: Início de sessão seguro via OAuth (Manus), encerramento de sessão e controlo de acessos baseado em perfis (*roles*) de administrador, profissional e paciente[span_6](end_span).
* **[span_7](start_span)Gestão de Pacientes**: Fluxo completo para criar, editar, listar, pesquisar, consultar históricos clínicos e aplicar a remoção lógica (*soft delete*)[span_7](end_span).
* **[span_8](start_span)Gestão de Sessões**: Agendamento de consultas, edição, cancelamento, registo de realização e adição de notas clínicas contextuais[span_8](end_span).
* **[span_9](start_span)Gestão Financeira**: Registo sistemático de receitas, emissão de faturas, relatórios financeiros detalhados e filtros temporais personalizados[span_9](end_span).
* **[span_10](start_span)Painel de Controlo (Dashboard) e Relatórios**: Métricas operacionais em tempo real (total de pacientes, sessões e faturação), gráficos de tendência e exportação de dados para PDF/Excel[span_10](end_span).
* **[span_11](start_span)Integrações de Sistema**: Sincronização automatizada com a plataforma E-SAÚDE base, Google Calendar, Webhooks e consumo de APIs externas[span_11](end_span).
* **[span_12](start_span)Segurança e Cópia de Segurança**: Rotinas automáticas de cópia de segurança (*backup*), restauro de dados, registos de auditoria detalhados e cifragem de dados sensíveis[span_12](end_span).

### [span_13](start_span)Requisitos Não-Funcionais[span_13](end_span)
* **[span_14](start_span)Desempenho**: Tempo de resposta inferior a 500ms para consultas à base de dados, suporte estrutural para mais de 10.000 pacientes em simultâneo e implementação de chaves de paginação e cache para dados recorrentes[span_14](end_span).
* **[span_15](start_span)Segurança Estrita**: Validação obrigatória de dados de entrada (*input*) em todos os procedimentos (*procedures*), proteção ativa contra injeção de SQL e tráfego encriptado via HTTPS[span_15](end_span).
* **[span_16](start_span)Escalabilidade**: Arquitetura estritamente modular, isolamento completo de dados por utilizador e separação clara de conceitos (*separation of concerns*)[span_16](end_span).

## [span_17](start_span)🏗️ Arquitetura da Base de Dados[span_17](end_span)
[span_18](start_span)O modelo relacional foi otimizado seguindo os melhores padrões de engenharia de dados[span_18](end_span):
1. **[span_19](start_span)[span_20](start_span)Normalização até à 3NF**: Estrutura limpa, eficiente e livre de redundâncias[span_19](end_span)[span_20](end_span).
2. **[span_21](start_span)[span_22](start_span)Remoção Lógica (Soft Delete)**: Mecanismo para assegurar a persistência de dados para auditorias sem comprometer o estado operacional[span_21](end_span)[span_22](end_span).
3. **[span_23](start_span)[span_24](start_span)Registos Temporais (Timestamps)**: Rastreabilidade total através dos campos `createdAt` e `updatedAt` em todas as tabelas[span_23](end_span)[span_24](end_span).
4. **[span_25](start_span)[span_26](start_span)Isolamento de Dados**: Inclusão do campo `userId` em todas as tabelas, garantindo segurança lógica absoluta entre utilizadores[span_25](end_span)[span_26](end_span).
5. **[span_27](start_span)[span_28](start_span)Índices Estratégicos**: Aplicados em chaves estrangeiras (*foreign keys*) e colunas de pesquisa para otimizar queries e evitar varrimentos completos (*table scans*)[span_27](end_span)[span_28](end_span).

[span_29](start_span)[span_30](start_span)A base de dados organiza-se em 25 tabelas estruturadas em 6 categorias de domínio (Utilizadores, Clínica, Pacientes, Sessões, Financeiro, Leads, Integrações e Configurações)[span_29](end_span)[span_30](end_span).

## [span_31](start_span)[span_32](start_span)📂 Estrutura de Pastas Estratégica[span_31](end_span)[span_32](end_span)
[span_33](start_span)[span_34](start_span)O projeto adota uma arquitetura integrada com o ecossistema tRPC para garantir tipagem estática de ponta a ponta entre o servidor e o cliente[span_33](end_span)[span_34](end_span).

### [span_35](start_span)Servidor (Backend)[span_35](end_span)
```text
server/
[span_36](start_span)├── routers/            # Procedimentos tRPC organizados por módulos[span_36](end_span)
[span_37](start_span)│   ├── auth.ts         # Fluxos de autenticação[span_37](end_span)
[span_38](start_span)│   ├── patients.ts     # Operações de pacientes[span_38](end_span)
[span_39](start_span)│   ├── sessions.ts     # Agendamentos e consultas[span_39](end_span)
[span_40](start_span)│   ├── financial.ts    # Transações e faturas[span_40](end_span)
│   └── ...
[span_41](start_span)├── db.ts               # Auxiliares de consulta e conexões (*Query helpers*)[span_41](end_span)
[span_42](start_span)├── middleware.ts       # Camadas de validação e segurança intermédia[span_42](end_span)
[span_43](start_span)└── validators.ts       # Esquemas de validação de dados com Zod[span_43](end_span)

```
### Cliente (Frontend)
```text
client/src/
[span_45](start_span)├── pages/              # Páginas principais da aplicação (Dashboard, Configurações, etc.)[span_45](end_span)
[span_46](start_span)├── components/         # Componentes reutilizáveis (Formulários, Tabelas com paginação)[span_46](end_span)
[span_47](start_span)├── hooks/              # Lógica e estado local encapsulados (useAuth, useFilters)[span_47](end_span)
[span_48](start_span)├── lib/                # Instanciação do cliente tRPC e configurações globais[span_48](end_span)
[span_49](start_span)└── contexts/           # Gestão de estado global (AuthContext, ThemeContext)[span_49](end_span)

```
## ⚙️ Validação de Dados e Qualidade
Para prevenir a corrupção de dados e mitigar falhas humanas no preenchimento, cada procedimento aplica validadores rigorosos através da biblioteca **Zod**. Exemplo de validação para novos registos:
```typescript
const createPatientSchema = z.object({
  name: z.string().min(3).max(100),
  email: z.string().email(),
  cpf: z.string().regex(/^\d{11}$/),
  dateOfBirth: z.date(),
  gender: z.enum(['M', 'F', '0']),
});

```
O ecossistema conta ainda com uma meta de cobertura superior a 80% em testes automatizados, divididos em testes unitários, testes de integração de fluxos e testes de ponta a ponta (E2E) com simulação de interface de utilizador.
## 📅 Cronograma de Implementação
O plano de desenvolvimento está calendarizado para uma execução faseada ao longo de 7 semanas:
 * **Semana 1**: Autenticação + Infraestrutura da Base de Dados.
 * **Semana 2**: Desenvolvimento do CRUD de Pacientes.
 * **Semana 3**: Módulo de Gestão de Sessões.
 * **Semana 4**: Arquitetura Financeira + Painel de Controlo (*Dashboard*).
 * **Semana 5**: Implementação de Integrações e Webhooks.
 * **Semana 6**: Módulo de Administração + Escrita de Testes Automatizados.
 * **Semana 7**: Implementação em Produção (*Deploy*) + Otimizações Finais.
## 📊 Matriz de Comparabilidade Técnica
| Aspeto Técnico | Arquitetura Atual | Nova Arquitetura |
|---|---|---|
| **Validação de Entrada** | Parcial | Completa (Camada Zod em todas as rotas) |
| **Cobertura de Testes** | Nenhuma | Superior a 80% (Unit, Integration, E2E) |
| **Documentação Técnica** | Mínima | Completa e centralizada no repositório |
| **Desempenho Geral** | Lenta / Gargalos de Queries N+1 | Otimizada com Índices e Caching |
| **Segurança Física** | Básica | Robusta (Auditoria, Cifragem e Perfilização) |
| **Escalabilidade** | Limitada | Completa através de Design Modular |


