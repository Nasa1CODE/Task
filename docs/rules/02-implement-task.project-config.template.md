# Project Config — Rule 02 Implement Task

> Companion file for `docs/rules/02-implement-task.md`. The rule is generic; **this file carries every project-specific fact the implementation step needs.**
> Copy to `docs/rules/02-implement-task.project-config.md`, fill every `{{PLACEHOLDER}}`, delete every `<!-- HOWTO -->` comment.
> **Porting the workflow to a new project = rewriting this file only. Never edit the rule.**

```yaml
project: {{PROJECT_NAME}}
config_version: 1.0.0
updated_at: {{YYYY-MM-DD}}
owner: {{TECH_LEAD}}
extends: docs/rules/01-create-plan.project-config.md
```

**Do not duplicate the Rule 01 keys here.** These are read from `01-create-plan.project-config.md` and must stay defined in exactly one place:
`SERVICE_TAGS` · `LAYER_READ_ORDER` · `VERIFY_COMMANDS` · `DB_DOCS_PATH` · `IMPACT_TOOL` · `I18N_REQUIRED` · `INTEGRATION_SURFACES` · `COMMON_IMPACT_ZONES` · `STATUS_VOCABULARY` · `TASK_FORMAT` · `BRANCH_RULE` · `EXTRA_RULES`

<!-- HOWTO: a project may merge both configs into docs/rules/project-config.md. The rules require the keys, not the filenames. -->

---

## 1. APPROVAL_EVIDENCE [REQUIRED]

What proves that Gate 1 passed. The agent checks **all** of these before writing code.

| Evidence | Where | Value that means approved |
|---|---|---|
| Plan status | `plan.md` front matter | `{{status: approved}}` |
| Approver record | `{{plan.md "## Approval" block}}` | `{{TechLead name + date}}` |
| Requirement status | `{{expect.md front matter}}` | `{{status: approved}}` |
| Ticket state | `{{Jira status}}` | `{{Ready for Dev}}` |

Rules:
- The agent **never** sets any of these itself.
- {{Any additional local rule, e.g. "a plan older than N days must be re-confirmed with the TechLead".}}

---

## 2. IMPLEMENT_ORDER [REQUIRED]

The order in which files are created or changed **inside one task**, per stack.

| Stack | Implementation order |
|---|---|
| {{Backend — Laravel}} | {{migration → model → repository/query → service → controller → route → request/resource/policy}} |
| {{Backend — Spring Boot}} | {{entity → repository → request/response DTO → service → controller → messaging producer/consumer → config}} |
| {{Backend — ASP.NET Zero / ABP}} | {{entity → DbSet/migration → DTO → AppService → permission → controller/MVC}} |
| {{Frontend — React}} | {{API module → query/slice → component/page → i18n keys → route → style}} |
| {{Frontend — Angular}} | {{service → model → component → template → routing → style}} |
| {{Worker / job service}} | {{handler → config/schedule → retry policy}} |

Rule: {{backend first, then frontend, then config/DB — unless the plan states otherwise}}.

---

## 3. CODE_STYLE_RULES [REQUIRED]

Rules the diff must satisfy. These are what "match the existing style" means in this project.

| Aspect | Rule |
|---|---|
| Logging | {{@Slf4j / log.info(); never System.out or echo}} |
| Transactions | {{@Transactional(rollbackFor = {Exception.class}); no manual commits}} |
| Error handling | {{custom exception types + global handler; never swallow exceptions}} |
| Response wrapper | {{Response<T> / ResponseData<T> / ApiResource}} |
| Null / validation | {{helper utilities to use}} |
| Naming | {{entity / DTO / service / repository / migration naming rules}} |
| Comment & message language | {{Vietnamese / English}} |
| File encoding | {{UTF-8 without BOM}} |
| Business logic placement | {{in services; never in controllers or UI components}} |
| Frontend styling | {{use existing component library and custom classes before writing new CSS}} |
| Required page/layout wrapper | {{markup every new page must use, or "none"}} |
| Outbound HTTP | {{must go through the shared helper X; never a raw client}} |

---

## 4. FORBIDDEN_CHANGES [REQUIRED]

Change types that must **not** be made unless the approved plan states them explicitly. Hitting one of these is a stop condition.

| Change | Requires |
|---|---|
| {{Entity / DTO field added, removed or retyped}} | {{stated in plan.md and re-approved}} |
| {{Public API contract — path, payload, status codes}} | {{stated in plan.md + consumer notice}} |
| {{RPC / proto contract}} | {{TechLead approval; all clients listed}} |
| {{Message payload / topic}} | {{TechLead approval; producer and consumers listed}} |
| {{Database schema}} | {{migration per the DB rule; never direct SQL on a shared environment}} |
| {{Permissions / roles / auth config}} | {{TechLead approval}} |
| {{Shared utility or base class}} | {{impact analysis + TechLead approval}} |
| {{Build, CI or infrastructure config}} | {{DevOps approval}} |

Do-not-touch paths: see the do-not-touch list in `AGENTS.md`. {{Add project-specific paths here.}}

---

## 5. MOCKABLE_INTEGRATIONS [REQUIRED — write `none` if all integrations run locally]

Integrations that cannot be exercised from a development environment, and what to do instead.

| Integration | Available locally | Policy while implementing | How it is verified instead |
|---|---|---|---|
| {{HSM / signing service}} | No | {{mock the client; never point at production}} | {{staging test after deploy}} |
| {{Partner/government portal}} | No | {{skip; guard behind config flag}} | {{UAT run by QA}} |
| {{Message broker}} | {{Yes — local docker}} | {{use the local instance}} | {{consumer integration test}} |

Rule: anything mocked or skipped must be listed in `report.md` §5.

---

## 6. DEPENDENCY_POLICY [REQUIRED]

| Item | Value |
|---|---|
| May the agent add a library? | {{No — requires approval from TechLead}} |
| Approved package managers | {{composer / yarn (not npm) / maven}} |
| Lock file policy | {{commit the lock file; never regenerate it wholesale}} |
| Version pinning | {{exact versions / caret allowed}} |
| Where a new dependency must be recorded | {{deploy.md §Dependencies + plan.md}} |

---

## 7. REPORT_EVIDENCE [REQUIRED]

What must be pasted into `report.md` §3 for the report to be accepted.

| Service | Commands whose real output must be pasted | Expected evidence format |
|---|---|---|
| `{{service-a}}` | `{{php -l <file>}}`, `{{./vendor/bin/phpunit --filter=X}}` | {{PHPUnit summary line: tests, assertions, failures}} |
| `{{service-b}}` | `{{yarn lint}}`, `{{yarn build}}` | {{lint exit status + build summary}} |
| `{{service-c}}` | `{{./mvnw test -Dtest=X}}` | {{Surefire summary: Tests run / Failures / Errors / Skipped}} |

Additional required evidence:
- {{Screenshot or manual-check description for UI tasks.}}
- {{Regression flows that must be re-run and reported: list them.}}
- {{Impact/change-detection output, if the impact tool is in use.}}

Rule: a check that could not be run is reported as `N/A` **with a verifiable reason**. Narrated results are rejected.

---

## 8. DEPLOY_TRIGGERS [REQUIRED]

`deploy.md` is mandatory when any of these is true:

- [ ] {{Database schema change or data migration}}
- [ ] {{Config or environment variable change}}
- [ ] {{New or upgraded dependency}}
- [ ] {{New or changed scheduled job / cron / queue worker}}
- [ ] {{Message topic or contract change}}
- [ ] {{RPC / proto contract change}}
- [ ] {{Auth realm / client / role / permission change}}
- [ ] {{Storage bucket / path / policy change}}
- [ ] {{Infrastructure, container or orchestration change}}
- [ ] {{Feature flag or staged rollout}}

---

## 9. DEPLOY_COMMANDS [REQUIRED]

| Purpose | Command |
|---|---|
| Run migrations | `{{php artisan migrate}}` / `{{dotnet ef database update}}` / `{{manual SQL by DBA}}` |
| Roll back migration | `{{php artisan migrate:rollback --step=1}}` |
| Build artifact | `{{./mvnw clean package -DskipTests}}` / `{{yarn build}}` |
| Deploy mechanism | {{GitLab CI on branch X / Jenkins job Y / manual}} |
| Restart / reload | {{kubectl rollout restart deployment/<name> -n <namespace>}} |
| Rollback deployment | {{kubectl rollout undo ... / redeploy previous tag}} |
| Post-deploy smoke check | {{command or URL + expected result}} |

Environments: {{dev / uat / prod — and who is allowed to deploy to each}}.

---

## 10. DOCS_UPDATE_MAP [REQUIRED]

Where each kind of change must be reflected, for the Gate 2 diff.

| Change | Document |
|---|---|
| Business logic, input/output, state, permissions | `{{docs/specs/<domain>.md}}` |
| Architecture, integrations, queues, storage, auth | `{{docs/ARCHITECTURE.md}}` |
| UI pattern, layout, component, theme | `{{docs/DESIGN.md}}` |
| Schema, entity, data | `{{docs/database/DB-STRUCTURE.md}}` |
| Service-specific coding convention | `{{docs/conventions/<service>.md}}` |
| Environment deployment steps | `{{<service>/docs/deployment.md}}` — service-local, **not** the shared docs repo |
| New error and its resolution | `{{<service>/docs/troubleshooting.md}}` — service-local |
| Feature-level rollout notes | `{{docs/features/<TICKET-ID>/deploy.md}}` |

Gate 2 approver: {{TechLead}}. Merge mechanism: {{MR on the docs repo}}.

---

## 11. COMMIT_RULE [REQUIRED]

| Item | Value |
|---|---|
| Commit message format | `{{<TICKET-ID>: short description}}` or `{{feat(<TICKET-ID>): description}}` |
| Branch | `{{feature/<TICKET-ID>}}` — {{same name in every repo involved}} |
| Must the ticket ID appear in every commit? | {{Yes}} |
| PR/MR target | `{{develop}}` |
| Must the commit/PR link be recorded in `tasks.md` / `report.md`? | {{Yes}} |
| May the agent commit on its own? | {{No — only when explicitly asked}} |
| May the agent push / merge / deploy on its own? | {{No — only when explicitly asked}} |
| Hard stop | {{diff contains files outside task scope, secrets, or runtime config → stop and ask}} |

---

## 12. Project-specific implementation constraints [OPTIONAL]

Anything else the implementation must always respect. Examples:

- {{Domain skills that must be invoked instead of improvising — situation → skill.}}
- {{Areas frozen for refactoring, or code owned by another team.}}
- {{Performance budgets, transaction size limits, batch size limits.}}
- {{Audit / regulatory constraints: signing, retention, personal-data logging.}}
- {{Legacy modules where the old pattern must be preserved even though it differs from the current convention.}}

---

## Fill-in checklist

- [ ] §1 approval evidence matches how this team actually approves plans.
- [ ] §2 implementation order agreed with the team leads for each stack.
- [ ] §3 style rules match what code review actually enforces.
- [ ] §4 forbidden changes cover every contract an outside party depends on.
- [ ] §7 commands were run once and their real output format recorded.
- [ ] §8/§9 verified with whoever operates the deployments.
- [ ] §10 target documents all exist.
- [ ] §11 matches the git rules in `AGENTS.md` — no contradiction between the two files.
- [ ] One pilot feature implemented end-to-end with this config before rollout.
