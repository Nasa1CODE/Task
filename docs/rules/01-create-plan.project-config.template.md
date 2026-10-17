# Project Config — Rule 01 Create Plan

> Companion file for `docs/rules/01-create-plan.md`. The rule is generic; **this file carries every project-specific fact the rule needs.**
> Copy to `docs/rules/01-create-plan.project-config.md`, fill every `{{PLACEHOLDER}}`, delete every `<!-- HOWTO -->` comment.
> **Porting the workflow to a new project = rewriting this file only. Never edit the rule.**

```yaml
project: {{PROJECT_NAME}}
config_version: 1.0.0
updated_at: {{YYYY-MM-DD}}
owner: {{TECH_LEAD}}
```

Keys marked **[REQUIRED]** must have a real value. Keys marked **[OPTIONAL]** may be set to `none` — but must stay present, so the agent knows they were considered.

---

## 1. SERVICE_TAGS [REQUIRED]

Canonical tags used in `plan.md` front matter and in the `Service` column of `tasks.md`. These must match the service registry in `AGENTS.md` — this table is the short form the agent uses while writing tasks.

| Tag | Service (canonical folder) | Port | Role |
|---|---|---|---|
| `[{{tag}}]` | `{{service-folder}}` | {{port}} | {{role}} |
| `[{{tag}}]` | `{{service-folder}}` | {{port}} | {{role}} |
| `[docs]` | `docs` | — | Documentation repo |

<!-- HOWTO: one row per deployable unit. Frontend, jobs, workers and the docs repo all get a tag. -->

---

## 2. LAYER_READ_ORDER [REQUIRED]

The order in which the agent must trace source files, per stack present in this project.

| Stack | Read order |
|---|---|
| {{Backend — Laravel}} | {{routes/*.php → Controller → FormRequest → Service → Repository/Model → migration}} |
| {{Backend — Spring Boot}} | {{controller/ → service/ → repository/ → entity/ → request/ + response/ → kafka/ → config/}} |
| {{Backend — ASP.NET Zero / ABP}} | {{AppService → Manager/Domain → EntityFramework repository → Entity → migration}} |
| {{Frontend — React}} | {{router → page → component → api module → query/slice → i18n → style}} |
| {{Frontend — Angular}} | {{routing module → component → service → model → style}} |

### Package / folder layout per service [OPTIONAL]

```text
{{src/main/java/<base-package>/
├── config/
├── controller/
├── service/
├── repository/
├── entity/
├── request/  response/
└── utils/}}
```

### Code conventions the plan must respect [OPTIONAL]

- {{Logging: @Slf4j / log.info(); never System.out}}
- {{Transactions: @Transactional(rollbackFor = ...)}}
- {{Response wrapper: Response<T> / ResponseData<T>}}
- {{Naming: entity / DTO / service / migration naming rules}}
- {{Comment and message language: Vietnamese / English}}

---

## 3. VERIFY_COMMANDS [REQUIRED]

The exact command the plan must cite in §11 and every task must cite in `Verify:`.

| Service | Lint / compile | Test | Build |
|---|---|---|---|
| `{{service-a}}` | `{{php -l <file>}}` | `{{./vendor/bin/phpunit --filter=X}}` | `{{composer install}}` |
| `{{service-b}}` | `{{yarn lint}}` | `{{yarn test}}` | `{{yarn build}}` |
| `{{service-c}}` | `{{./mvnw -q compile}}` | `{{./mvnw test -Dtest=X}}` | `{{./mvnw clean package -DskipTests}}` |

<!-- HOWTO: "run the tests" is not a valid entry. If a service has no test suite, write the manual check that proves the change works. -->

---

## 4. DB_DOCS_PATH [REQUIRED]

| Item | Value |
|---|---|
| Current-state document | `{{docs/database/DB-STRUCTURE.md}}` |
| Schema snapshot (do not read in full) | `{{docs/database/structure.sql}}` |
| Migration script location | `{{docs/database/}}` |
| Migration file naming | `{{YYYY-MM-DD_short-description.sql}}` |
| Migration companion rule | `{{docs/rules/05-db-migration.md}}` |
| Who applies migrations | {{DBA / DEV / CI}} |
| Database engine(s) | {{MySQL 8 / Oracle 19c / PostgreSQL 14}} |

Rules:
- Every DB proposal must start from the current-state document, not from the code.
- Never read the full schema snapshot; grep only the relevant tables.
- {{Any additional constraint: naming, charset, index policy, soft-delete convention, audit columns.}}

---

## 5. IMPACT_TOOL [OPTIONAL — set to `none` if unused]

| Item | Value |
|---|---|
| Tool | {{GitNexus / none}} |
| Setup rule | `{{docs/rules/06-impact-setup.md}}` |
| Commands | `{{npx gitnexus setup}}`, `{{npx gitnexus analyze}}`, `{{gitnexus_impact()}}`, `{{gitnexus_detect_changes()}}` |

Indexed services (the tool is mandatory only for these):

| Service | Symbols | Flows |
|---|---|---|
| `{{service-a}}` | {{3355}} | {{276}} |

All other services: read the code manually.

---

## 6. I18N_REQUIRED [REQUIRED]

`{{true | false}}`

If `true`, fill this in:

| Item | Value |
|---|---|
| Supported locales | {{vi, en}} |
| Default / fallback locale | {{vi}} |
| Locale file location per app | `{{frontend/src/locales/<lang>/<namespace>.json}}` |
| i18n library | {{react-intl / i18next / Laravel lang}} |
| Surfaces that must be translated | {{UI text, API messages, emails, notifications, exported documents}} |

Rule: no hardcoded user-visible text anywhere. Every plan section §5 must list the keys to add or change.

---

## 7. INTEGRATION_SURFACES [REQUIRED]

Every contract that can break a client. The plan's §9 and §10 must give a compatibility verdict for each one that the feature touches.

### REST / public API
- Gateway routing convention: {{path prefix → service mapping, or link to ARCHITECTURE.md}}
- Versioning policy: {{none / /v1 path / header}}
- Known external consumers: {{systems that call this API}}

### {{RPC — gRPC / proto}} [OPTIONAL]
| Server | Port | Clients | Proto location |
|---|---|---|---|
| `{{service}}` | {{:6565}} | {{list of clients}} | `{{src/main/resources/*.proto}}` |

Rule: {{a proto change breaks every client — treat as high severity and list them all}}.

### {{Messaging — Kafka / RabbitMQ / MQ}} [OPTIONAL]
| Topic / queue | Producer → Consumer | Message contract |
|---|---|---|
| `{{topic}}` | {{producer}} → {{consumer}} | {{schema location}} |

Rule: {{commit policy, ordering guarantees, schema evolution policy}}.

### {{Third-party / external systems}} [OPTIONAL]
| System | Used by | Protocol | Notes |
|---|---|---|---|
| {{HSM / signing}} | {{service}} | {{REST/SOAP}} | {{only reachable inside the corporate network}} |
| {{partner portal}} | {{service}} | {{SOAP}} | {{cannot be tested locally}} |

### {{Auth}} [OPTIONAL]
- Provider: {{Keycloak realm X}} — a realm/config change affects {{all services}}.

### {{Templates / documents}} [OPTIONAL]
- {{XML/XSD/JasperReports templates: location + which services consume them}}

---

## 8. COMMON_IMPACT_ZONES [REQUIRED]

The project's known blast radii. The agent applies these severities in Step 4 of the rule.

| Change | Affected scope | Severity |
|---|---|---|
| {{Change a proto / RPC contract}} | {{every client of that service}} | 🔴 HIGH |
| {{Change a shared database table}} | {{every service on that schema}} | 🔴 HIGH |
| {{Change a message payload format}} | {{producer + all consumers}} | 🟡 MEDIUM |
| {{Change auth realm / config}} | {{all services}} | 🔴 HIGH |
| {{Change one leaf service only}} | {{that service}} | 🟢 LOW |

---

## 9. STATUS_VOCABULARY [OPTIONAL — omit to use the rule defaults]

| Where | Allowed values |
|---|---|
| `plan.md` front matter | {{draft, plan-review, approved, in-progress, testing, done, cancelled}} |
| `tasks.md` task markers | {{`[ ]`, `[/]`, `[x]`, `[blocked]`}} |

---

## 10. TASK_FORMAT [REQUIRED]

`{{checklist | table | block}}`

Sizing convention: {{XS ≈ 1 file · S ≈ 1–2 · M ≈ 3–5 · L ≈ 6–8; split anything larger}}

Task group order in `tasks.md`: {{backend groups by service → frontend → database/config → i18n → close-out}}

---

## 11. BRANCH_RULE [REQUIRED]

| Item | Value |
|---|---|
| Feature branch pattern | `{{feature/<TICKET-ID>}}` — {{same name in every repo involved}} |
| Integration branch | `{{develop}}` |
| Deploy branch | `{{deploy/dev}}` |
| CI/CD | {{GitLab CI / Jenkins}} |
| Pre-implementation check | {{if the current branch is not the feature branch, ask before creating or switching; never switch with a dirty working tree}} |

---

## 12. EXTRA_RULES [REQUIRED]

Companion rules the agent must read when the trigger applies.

| Rule | Trigger |
|---|---|
| `{{docs/rules/03-create-testcase.md}}` | {{after the plan, before code}} |
| `{{docs/rules/05-db-migration.md}}` | {{any DB / schema / data change}} |
| `{{docs/rules/04-create-impact.md}}` | {{project uses impact.md — draft it during planning}} |
| `{{docs/rules/06-impact-setup.md}}` | {{impact tool missing or unindexed}} |
| `{{docs/DESIGN.md}}` | {{any UI change}} |

---

## 13. Project-specific planning constraints [OPTIONAL]

Anything else the plan must always account for in this project. Examples:

- {{Domain skills that must be invoked instead of improvising — list situation → skill.}}
- {{Environment limits: which integrations cannot be exercised locally, so the verification plan must say how they are checked.}}
- {{Regulatory/audit constraints: signing, retention, logging of personal data.}}
- {{Performance budgets a plan must respect.}}
- {{Areas frozen for refactoring.}}

---

## Fill-in checklist

- [ ] §1 tags match the service registry in `AGENTS.md` exactly.
- [ ] §3 commands were actually run once and confirmed to work.
- [ ] §4 current-state DB document exists and is current.
- [ ] §7 lists every contract an outside party depends on.
- [ ] §8 severities agreed with the TechLead.
- [ ] §11 branch names match what the team really uses.
- [ ] §12 links resolve to files that exist.
- [ ] One pilot feature planned end-to-end with this config before rollout.
