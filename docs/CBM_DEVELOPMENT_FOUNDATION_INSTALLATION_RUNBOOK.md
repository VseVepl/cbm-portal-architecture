# CBM Development Foundation Installation and Validation Runbook

| Document attribute | Value |
|---|---|
| Document status | Proposed installation plan; not executed |
| Baseline date | 19 November 2026 |
| Repository branch | `architecture/domain-architecture-continuation-2026-11-19` |
| Language | English |
| Parent plan | [CBM Development Phases and Lifecycle Plan](CBM_DEVELOPMENT_PHASES_AND_LIFECYCLE_PLAN.md) |
| Authorization | Planning only; no installation or infrastructure change is authorized |

## 1. Purpose and safety boundary

This runbook defines the recommended first-stage software and development-tool installation baseline for CBM. It is designed to be executed later by an authorized engineer on a newly provisioned, supported development host. It is not a command to execute now.

**Do not run this runbook against an existing server, production host, shared OpenHands/LiteLLM host, or any machine containing workloads until its owner explicitly authorizes the target and a change window.** Inventory the host first. Do not remove existing packages or containers merely because they are not in the proposed baseline.

The approved project direction includes Laravel backend, React frontend, Tailwind CSS, PostgreSQL 18+ as the mandatory database baseline, and a preference against unnecessary third-party dependencies. Exact versions must be captured at execution time from official sources and committed through project manifests/lockfiles.

## 2. Recommended baseline and compatibility decision

### 2.1 Host platform

**Recommendation:** Ubuntu 26.04 LTS, 64-bit, on a supported x86-64 or ARM64 architecture, for a fresh development host. Ubuntu 24.04 LTS is an acceptable compatibility fallback if a required vendor/runtime package is not yet supported on 26.04, provided its standard support window and package repositories are verified at provisioning time.

Rationale:
- LTS is preferred over interim releases for a long-lived development foundation.
- Ubuntu 26.04 LTS is the current LTS generation as of this document baseline.
- Docker's official Ubuntu installation documentation lists Ubuntu 26.04 LTS and 24.04 LTS as supported.
- Do not select an OS solely because a container image happens to run on it; validate host package support, security maintenance, architecture and all required extensions.

**Hardware planning profile (not a production sizing commitment):**
- Developer workstation: 4+ modern CPU cores, 16 GB RAM recommended, SSD with at least 100 GB free for source, package caches, databases and browser binaries.
- Comfortable local integration profile: 8 CPU cores, 32 GB RAM and 200+ GB SSD free where multiple containers, browser workers and databases run concurrently.
- CI runners should be sized from measured build/test concurrency.
- Production and DR capacity must be derived from approved workload, availability, growth and recovery objectives; do not reuse developer sizing.

### 2.2 Selected runtime/toolchain matrix

| Component | Proposed baseline | Status / rationale | Mandatory? |
|---|---|---|---|
| OS | Ubuntu 26.04 LTS (24.04 LTS fallback) | Current LTS baseline; verify package/vendor support | Yes, for the reference host |
| Git | Current supported stable Git 2.x from official OS/vendor source | Source control and branch/review workflow | Yes |
| PHP | PHP 8.5 latest supported patch | Laravel 13 supports PHP 8.3–8.5; choose current supported branch, then validate extensions | Yes |
| PHP extensions | Laravel-required core extensions plus PostgreSQL PDO driver; exact list verified against Laravel 13 and lockfile | No missing extension may be guessed | Yes |
| Composer | Current stable Composer 2.x | PHP dependency management; verify installer signature/checksum | Yes |
| Node.js | Node.js 26 LTS line | Active LTS at baseline; satisfies current Vite/Playwright supported Node range | Yes |
| JS package manager | npm bundled with selected Node LTS initially | Minimize extra global tooling; package manager and version pinned by project policy | Yes |
| React | React 19.3 line | Current official React docs version at baseline | Yes |
| Vite | Current stable compatible release | Official guide currently requires Node 20.19+ or 22.12+; validate selected Node 26 LTS | Yes, if Vite remains the approved build tool |
| Tailwind CSS | Tailwind CSS 4.x | Current major; use official Vite integration where compatible | Yes, per approved frontend direction |
| PostgreSQL | PostgreSQL 18, latest supported 18.x minor | Mandatory CBM baseline for dev/test/prod; use official PostgreSQL packages | Yes |
| PostgreSQL client tools | Matching PostgreSQL 18 client utilities | psql, pg_dump, pg_restore and diagnostics | Yes |
| Docker Engine + Compose | Current stable supported release | Optional reproducible local services; not required if native services are used | Conditional |
| Playwright | Current supported Playwright Test release and matching browsers | Browser E2E; version/browser binaries must match lockfile | Yes for E2E/CI |
| PHP unit test runner | PHPUnit version compatible with Laravel 13 dependency set | Laravel's default test foundation; use Composer lockfile | Yes |
| PHP formatter | Laravel Pint (first-party Laravel ecosystem) | Consistent style; avoid adding duplicate formatters | Recommended |
| PHP static analysis | PHPStan with Laravel integration only after compatibility/license review | Useful for large codebase; third-party dependency | Recommended, subject to approval |
| Frontend type checking | TypeScript | Catch contract and UI errors | Recommended |
| Frontend lint | ESLint with compatible React/TypeScript config | Static checks | Recommended |
| Dependency audit | Composer audit and npm audit; supplement with approved scanner if needed | Known advisories and supply-chain review | Yes |
| Secret scanning | GitHub secret scanning or an approved local/CI scanner | Prevent secret commits | Yes |
| License/SBOM scan | SPDX-compatible inventory/scanner selected by engineering | Transitive package/license visibility | Yes before release |
| Local TLS | mkcert or equivalent trusted local CA, if HTTPS browser flows require it | Local-only; never reuse local certs in production | Conditional |
| Local mail sink | Mailpit or equivalent, if outbound mail flows are implemented | Prevent test messages reaching real recipients | Conditional |
| Cache/queue | PostgreSQL-backed initial queue/cache where appropriate; Redis only when justified | Avoid premature service dependency | Conditional |
| Object storage | Local filesystem for early dev; S3-compatible test service only when object workflows require it | Keep storage abstraction; avoid premature dependency | Conditional |
| Reverse proxy | Not mandatory for basic local development; production choice belongs to P03/P04 | Do not install Nginx/Caddy without deployment design | Conditional |
| Editor/IDE | VS Code or another supported editor; extensions are optional | No editor vendor lock-in | Optional |

### 2.3 Compatibility decisions and cautions

1. Laravel 13's published support table lists PHP 8.3–8.5. PHP 8.5 is the recommended line only after all required extensions and locked packages pass tests. If a required dependency is incompatible, use PHP 8.4 temporarily only with a recorded compatibility exception and upgrade path.
2. PostgreSQL 18 is a CBM project baseline independent of the minimum PostgreSQL version supported by Laravel. Do not infer that Laravel's broad support for older PostgreSQL versions changes CBM's mandatory baseline.
3. Node 26 is selected because it is LTS at the baseline date. Do not use Node 27 Alpha or another non-LTS line for the project toolchain.
4. Vite, React, Tailwind and Playwright must be pinned together in `package-lock.json`; no global unpinned package install should determine project builds.
5. Composer dependencies must be pinned in `composer.lock`. Do not use unbounded wildcard constraints.
6. Exact patch versions, apt repository package versions, Composer PHAR checksum, browser revisions and container image digests must be recorded at actual installation time. This document deliberately does not invent future patch pins.
7. A major upgrade of PHP, Node, PostgreSQL, Laravel, React, Vite or Tailwind requires a compatibility branch and full regression/migration/security review.

## 3. Host preflight and installation order

Do not install until the following inventory has been captured:

- Hostname, OS release, kernel, CPU architecture and virtualization/container status.
- CPU count, RAM, swap, disk type, free space, filesystem and inode availability.
- Existing Git/PHP/Composer/Node/npm/PostgreSQL/Docker versions and package origins.
- Existing services, listeners, firewall, DNS, proxy, TLS and network restrictions.
- Existing repositories, databases, containers, volumes and data ownership.
- Package manager health and configured repositories.
- Whether the machine is dedicated to CBM development or shared.

**Stop conditions:** unsupported OS/architecture; insufficient disk/RAM; untrusted package source; package-manager errors; existing service/data conflict; unknown production/shared workload; or inability to back up existing state.

### 3.1 Dependency-aware order

| Order | Install/configure | Depends on | Validation |
|---|---|---|---|
| 0 | Host inventory and backup of any existing state | None | Signed/retained preflight report |
| 1 | OS security updates, CA certificates, time sync, package metadata | Supported OS | OS release, updates, clock and TLS validation |
| 2 | Basic utilities: curl, wget, unzip, zip, jq, gnupg, build-essential, pkg-config | OS package manager | Binary versions and package origin |
| 3 | Git | TLS CA trust and package manager | git version, identity config, SSH/HTTPS auth test |
| 4 | PostgreSQL official repository/key and server/client 18 | OS and trusted key setup | server/client version, service status, role/db/TLS checks |
| 5 | PHP 8.5 CLI and required extensions | OS repositories or approved PHP vendor source | php -v, php -m, required extension check |
| 6 | Composer 2.x | Supported PHP runtime and TLS | signature/checksum, composer diagnose |
| 7 | Node 26 LTS and npm | OS trust and supported architecture | node/npm version, registry TLS |
| 8 | Project dependency install from committed lockfiles | Git, PHP, Composer, Node/npm | clean install; no lockfile drift |
| 9 | Frontend build/test/static tooling | Node dependencies | typecheck/lint/unit/build |
| 10 | PHP format/static/test tooling | Composer dependencies | format check, static analysis, PHPUnit |
| 11 | Playwright and matching browser binaries/deps | Node lockfile and supported OS | browser launch and sample E2E |
| 12 | Optional Docker Engine/Compose | OS/network/firewall review | daemon, compose version, hello-world |
| 13 | Conditional Redis/mail/object-storage/local TLS | Accepted architecture need | health, isolation and security checks |
| 14 | Editor/IDE integration | Project manifests | editor diagnostics match CI |
| 15 | Full environment validation and manifest | All mandatory components | reproducible clean checkout report |

The exact command syntax must be generated from the official instructions applicable to the selected OS release and package repository at execution time. Do not paste Ubuntu 24.04 repository commands into Ubuntu 26.04 or vice versa.

## 4. Package and service installation details

### 4.1 Operating system and base utilities

- **Name/purpose:** Ubuntu LTS provides the supported host platform; apt manages signed OS packages.
- **Version:** Ubuntu 26.04 LTS; 24.04 LTS fallback only if required compatibility is demonstrated.
- **Source/licensing:** Ubuntu official release and repository documentation; component licenses are distributed through package metadata.
- **Dependencies/order:** first; verify architecture and repositories.
- **Configuration:** minimal installed services; security updates; accurate time synchronization; SSH hardening if remote; host firewall; trusted CA bundle.
- **Security:** do not pipe remote scripts to root shells; verify repository signing keys; avoid unsupported PPAs; restrict administrative access.
- **Validation:** `lsb_release -a` or `/etc/os-release`, `uname -m`, `apt update`, `timedatectl`, disk/RAM report.
- **Rollback/recovery:** restore VM snapshot or host backup; do not downgrade core OS packages ad hoc.
- **Environments:** local dev and CI runner host; production OS baseline must be separately approved and aligned.

### 4.2 Git and repository access

- **Purpose:** version control, history, branch workflow and code review.
- **Version:** supported stable Git 2.x; use OS security-maintained version unless a documented feature requires newer.
- **Source:** [git-scm.com](https://git-scm.com/doc) and signed OS packages.
- **Configuration:** SSH keys or approved token credential helper; commit signing policy to be decided; user identity; safe directory settings only when needed.
- **Security:** never store PATs/passwords in remote URLs or shell history; least-privilege GitHub token; protect main and the designated working branch.
- **Validation:** fetch designated branch, verify commit ancestry, test read/write only when authorized.
- **Rollback:** restore local refs/worktree; never force-push or rewrite protected history.
- **Environments:** developer, CI, release automation.

### 4.3 PHP 8.5 and extensions

- **Purpose:** Laravel backend runtime.
- **Version:** PHP 8.5 latest supported patch at installation time; PHP 8.4 only as a documented compatibility fallback.
- **Source:** [PHP supported versions](https://www.php.net/supported-versions.php), official OS/vendor package repository.
- **Extensions:** determine exact requirements from Laravel 13 and the project's Composer lockfile. Expected categories include PDO PostgreSQL driver, mbstring, XML/DOM, curl, OpenSSL, tokenizer, fileinfo, ctype, JSON, sodium, intl and zip where required by selected packages. Do not assume every extension is required; produce a machine-checked list.
- **Dependencies/order:** install CLI and the intended PHP-FPM/runtime SAPI consistently; install PostgreSQL development headers if building extensions from source.
- **Configuration:** separate CLI and web SAPI configuration; timezone UTC internally unless domain policy specifies otherwise; memory/upload/time limits from measured workloads; error display off outside local development; logs to controlled sinks.
- **Security:** supported patch level; disable unused extensions/functions only after compatibility/security review; no secrets in phpinfo or logs.
- **Validation:** `php -v`, `php --ini`, `php -m`, `php -r` checks for PDO and pgsql driver; Laravel platform requirement check.
- **Rollback:** switch to prior supported PHP package line only after compatibility testing; restore config backups; do not remove a runtime used by other services.
- **Environments:** local, CI, test, staging and production must use compatible supported PHP major/minor lines.

### 4.4 Composer

- **Purpose:** PHP dependency resolution and installation.
- **Version:** current stable Composer 2.x at execution time.
- **Source:** [getcomposer.org/download](https://getcomposer.org/download/).
- **Dependencies:** supported PHP runtime and TLS.
- **Configuration:** use project `composer.lock`; production installs use `composer install` with appropriate no-dev/optimized-autoloader options; do not update dependencies implicitly during deployment.
- **Security:** verify installer signature/checksum as documented; review Composer advisories; do not disable TLS or platform checks to force installation.
- **Validation:** `composer --version`, `composer diagnose`, `composer validate --strict`, `composer audit`.
- **Rollback:** restore lockfile and known-good vendor build/artifact; avoid partial package updates.
- **Licensing:** collect direct/transitive license metadata and review compatibility before adoption.

### 4.5 Node.js 26 LTS and npm

- **Purpose:** frontend build, test, package management and tooling.
- **Version:** Node 26 LTS line at baseline; npm version bundled with the selected official Node distribution unless project compatibility justifies another pinned version.
- **Source:** [Node.js releases](https://nodejs.org/en/about/previous-releases) and [release schedule](https://github.com/nodejs/Release).
- **Dependencies:** supported OS/architecture and TLS.
- **Configuration:** project `.nvmrc`/toolchain manifest or equivalent; lockfile-based `npm ci`; registry configuration must use TLS and approved sources.
- **Security:** do not install arbitrary global packages; review lifecycle scripts; use `npm audit`; protect tokens.
- **Validation:** `node --version`, `npm --version`, `npm ci`, frontend build/typecheck/lint.
- **Rollback:** use prior pinned LTS runtime in a controlled toolchain manager; never modify production runtime by hand.
- **Compatibility:** Vite official guide currently requires Node 20.19+ or 22.12+; Playwright supports current 22.x/24.x/26.x lines, so Node 26 LTS is within its documented range.

### 4.6 React, Vite and Tailwind

- **Purpose:** React frontend and CSS/build pipeline.
- **Version:** React 19.3 line; current stable Vite compatible with Node 26; Tailwind CSS 4.x official integration.
- **Source:** [React versions](https://react.dev/versions), [Vite guide](https://vite.dev/guide/), [Tailwind installation](https://tailwindcss.com/docs/installation).
- **Dependencies:** Node/npm and project lockfile.
- **Configuration:** use the official Vite plugin/integration where applicable; keep React/Radix/shadcn-style components accessible; avoid adding UI libraries without a capability gap and review.
- **Security:** pin dependencies; review package license and transitive tree; do not load untrusted remote scripts/styles.
- **Validation:** production build, browser smoke test, TypeScript, lint, accessibility checks.
- **Rollback:** revert lockfile and source changes together; preserve last known-good build artifact.
- **Status:** React and Tailwind direction is approved in the handover; exact package versions/integration are technical recommendations pending the implementation architecture gate.

### 4.7 PostgreSQL 18 server/client

- **Purpose:** mandatory canonical database baseline across development, testing and production.
- **Version:** PostgreSQL 18, latest supported 18.x minor at installation time. At this research snapshot, the official versioning table reports 18.7; recheck before installation.
- **Source:** [PostgreSQL versioning policy](https://www.postgresql.org/support/versioning/), [PostgreSQL 18 docs](https://www.postgresql.org/docs/18/), official PostgreSQL apt repository where required.
- **Dependencies:** supported OS, verified repository key, storage and backup location.
- **Install order:** client/server packages; initialize cluster; secure roles/authentication; create dedicated development database/role; configure TLS/network only as needed.
- **Configuration:** UTF-8; explicit locale/collation policy; connection limits based on app workers; least-privilege roles; separate owner/migration/runtime roles where appropriate; timezone policy; logging; backup and restore.
- **Security:** do not expose PostgreSQL publicly by default; use SCRAM authentication and TLS for network connections; restrict pg_hba; no superuser app account; separate environments and tenants.
- **Validation:** server/client major 18; `pg_isready`; `psql` connection; encoding; extension inventory; create/migrate/seed disposable DB; `pg_dump` and `pg_restore` round trip; constraint and tenant-isolation tests.
- **Rollback/recovery:** restore verified backup to a compatible PostgreSQL version; do not assume major-version downgrade is supported; major upgrades require `pg_upgrade`/dump-restore plan and rehearsals.
- **Maintenance:** minor updates promptly after compatibility check; major upgrades through a dedicated migration project and full restore/reconciliation rehearsal.
- **Licensing:** PostgreSQL License (per official PostgreSQL project); record notices in SBOM/license inventory.

### 4.8 Docker Engine and Compose (conditional)

- **Purpose:** reproducible local/CI supporting services and isolated test dependencies.
- **Version:** current stable Docker Engine/Compose versions supported by selected Ubuntu LTS.
- **Source:** [Docker Engine on Ubuntu](https://docs.docker.com/engine/install/ubuntu/).
- **Dependencies:** supported Ubuntu release, repository key, kernel/network/firewall review.
- **Configuration:** rootless mode where feasible; restrict daemon access; avoid privileged containers; use non-root images; pin image tags and digests; named volumes; health checks; resource limits.
- **Security:** Docker-published ports may bypass some host firewall expectations; explicitly review Docker networking and firewall rules. Never mount host root or Docker socket into application containers without exceptional justification.
- **Validation:** `docker version`, `docker compose version`, daemon status, test image provenance, network/volume cleanup in disposable environment.
- **Rollback:** stop/remove only resources created for this project; preserve volumes until verified backup and owner approval.
- **Environments:** local and CI where justified; production containerization is a separate architecture/release decision.

### 4.9 Playwright and browser automation

- **Purpose:** browser E2E, cross-browser and user-journey verification.
- **Version:** current stable Playwright Test pinned in package lock; install its matching browser revisions.
- **Source:** [Playwright installation](https://playwright.dev/docs/intro).
- **Dependencies:** supported Node line and OS browser dependencies.
- **Configuration:** separate test base URL and credentials; synthetic data; trace/video/screenshot retention policy; isolated workers and test cleanup.
- **Security:** no production credentials/data; sanitize artifacts that could contain personal data; protect CI artifacts.
- **Validation:** launch Chromium/WebKit/Firefox where the supported target matrix requires; run smoke E2E and verify browser versions.
- **Rollback:** revert Playwright and browser revision lock together.
- **Environments:** developer, CI, QA; not installed on production hosts.

### 4.10 Quality, static analysis and security tools

- **PHPUnit:** use version resolved and locked with Laravel 13; required test runner. Validate unit, feature and integration test execution.
- **Laravel Pint:** recommended first-party formatter; use consistent CI check.
- **PHPStan/Larastan:** recommended static analysis after exact Laravel/PHP compatibility and license check; do not treat an unreviewed plugin as mandatory.
- **TypeScript:** recommended for typed frontend contracts and compile-time checks.
- **ESLint:** recommended frontend linting; plugin versions locked and reviewed.
- **Composer audit / npm audit:** mandatory dependency advisory checks; define controlled exception handling, not blanket suppression.
- **Secret scanning:** mandatory in local/CI/repository; use GitHub-native feature where available or an approved scanner.
- **License/SBOM:** mandatory before release; choose a scanner after license and output-format review.
- **Container scanning:** only if container images are built; scan base and final images and pin digest.
- **Validation:** all tools run from a clean checkout and produce machine-readable reports; CI and local versions match.
- **Rollback:** remove/revert a tool only through dependency and pipeline change review; do not disable a failing security gate to make builds green.

### 4.11 Conditional local services

| Service | Use only when | Recommended initial policy | Security/validation |
|---|---|---|---|
| Redis | Accepted cache/queue/lock/stream requirement or measured PostgreSQL limitation | Defer until architecture requires it | Bind privately; auth/TLS where networked; test failover and key isolation |
| Message broker | Asynchronous event throughput/replay requirements exceed database-backed approach | Defer; do not assume Kafka/RabbitMQ | Define delivery semantics, idempotency, dead-letter, retention and operations |
| S3-compatible storage | Object/file workflows require realistic API compatibility | Local filesystem first; add MinIO or equivalent only for tests needing S3 semantics | Private buckets, encryption, lifecycle, checksum/restore |
| Mailpit/local mail sink | Email workflows enter implementation | Capture all local/test mail; never send real messages by default | Verify recipient sink and block external SMTP |
| mkcert/local CA | HTTPS-origin browser features or local TLS testing require it | Developer-only CA | Trust only on developer machine; revoke/clean up when retired |
| Nginx/Caddy | Local reverse proxy topology is needed | Not a prerequisite for basic Laravel/Vite local dev | TLS, host routing, headers and port exposure review |
| API gateway | Central routing/rate-limit/policy need is evidenced | Defer until architecture decision | Threat model, authn/authz, observability and failure behaviour |

## 5. Installation and configuration policy

1. Use official OS repositories or official vendor repositories with signature verification.
2. Do not use arbitrary curl-to-shell installation scripts.
3. Record exact package name, version, repository, architecture, signature/checksum, license, installation timestamp and owner.
4. Pin application dependencies in lockfiles; pin container images by digest for reproducible builds.
5. Keep runtime major/minor compatibility aligned across developer, CI, test and staging; production must use supported compatible versions.
6. Use separate configuration and secrets per environment.
7. Do not copy production database dumps into development.
8. Do not expose PostgreSQL, Redis, Docker daemon, object storage or mail sinks to public networks.
9. Keep install logs and validation reports free of credentials.
10. Do not upgrade major versions as part of routine unattended updates.

## 6. Validation checklist

A foundation is not accepted until all applicable checks pass:

- [ ] Host OS/version/architecture and resource inventory recorded.
- [ ] Package sources and signing keys verified.
- [ ] Git authentication works with least-privilege credentials.
- [ ] PHP CLI and runtime versions match; required extensions verified.
- [ ] Composer signature/checksum verified; diagnose/validate/audit pass.
- [ ] Node/npm versions recorded; `npm ci` is reproducible.
- [ ] React/Vite/Tailwind build passes.
- [ ] PostgreSQL 18 server/client versions match; secure role/database created.
- [ ] PostgreSQL UTF-8/locale/timezone/connection policy documented.
- [ ] Database migration, seed and test run pass.
- [ ] PostgreSQL backup and restore round trip verified.
- [ ] PHPUnit and frontend tests pass.
- [ ] Static analysis/format/lint checks pass.
- [ ] Playwright browsers launch and smoke E2E passes.
- [ ] Dependency and license reports generated.
- [ ] Secret scan clean; no secret committed.
- [ ] Docker only if approved; daemon, Compose, network and firewall verified.
- [ ] Conditional services only if justified and validated.
- [ ] Clean-room setup succeeds from a fresh checkout.
- [ ] Exact toolchain manifest and validation report saved.
- [ ] No production data, credentials or live integrations are present.

## 7. Failure, rollback and recovery

| Failure | Required response |
|---|---|
| Package repository/key cannot be verified | Stop; do not bypass signature validation; identify official source |
| OS package conflict | Capture package state; preserve existing services; resolve compatibility before removal |
| PHP extension unavailable for selected version | Reassess supported package source or PHP fallback; do not compile unreviewed binaries |
| Composer/npm lockfile drift | Stop and identify source of drift; do not update lockfile as a workaround |
| PostgreSQL migration or restore failure | Preserve logs and source backup; use disposable recovery target; do not overwrite original data |
| Docker port/firewall conflict | Stop containers and review exposure; do not disable firewall globally |
| Browser dependencies missing | Install only documented OS dependencies for supported OS; rerun browser smoke test |
| Security/license finding | Quarantine dependency or record an approved exception with owner, scope and expiry |
| Clean-room reproduction fails | Foundation gate fails; correct manifest/runbook before onboarding developers |

Rollback must be scoped to the failed installation/configuration. Do not purge shared packages, delete volumes, reset databases or change server firewall without explicit authorization and a verified recovery point.

## 8. Version, license and maintenance policy

- Recheck official support status immediately before provisioning and on every scheduled upgrade.
- Record package licenses using exact package metadata and preserve notices.
- Prefer platform-native and first-party tooling; introduce third-party packages only for a documented capability gap.
- Lock direct and transitive dependencies; review major upgrades separately.
- Subscribe to security advisories for OS, PHP, Laravel, Node, PostgreSQL, browsers and direct dependencies.
- Generate an SBOM for release artifacts and keep provenance.
- Define package removal/deprecation procedures; do not leave abandoned tools with credentials or network listeners.
- A package's open-source license does not imply that all dependencies, browser binaries, fonts or hosted services have the same terms.

## 9. Official source register

- Ubuntu lifecycle: https://ubuntu.com/about/release-cycle
- Docker Engine Ubuntu install/support: https://docs.docker.com/engine/install/ubuntu/
- Git documentation: https://git-scm.com/doc
- PHP supported versions: https://www.php.net/supported-versions.php
- Laravel 13 release/support policy: https://laravel.com/framework/docs/13.x/releases
- Laravel database support: https://laravel.com/framework/docs/database
- Composer installation verification: https://getcomposer.org/download/
- Node.js release schedule: https://github.com/nodejs/Release
- React versions: https://react.dev/versions
- Vite requirements: https://vite.dev/guide/
- Tailwind CSS installation: https://tailwindcss.com/docs/installation
- PostgreSQL version policy: https://www.postgresql.org/support/versioning/
- PostgreSQL 18 documentation: https://www.postgresql.org/docs/18/
- PostgreSQL backup: https://www.postgresql.org/docs/18/backup.html
- Playwright installation/system requirements: https://playwright.dev/docs/intro
- OWASP SAMM: https://owaspsamm.org/model/
- OWASP ASVS: https://asvs.owasp.org/
- GitHub Actions security hardening: https://docs.github.com/en/actions/security-for-github-actions/security-guides/security-hardening-for-github-actions

## 10. Acceptance and handover

The runbook is accepted only when:
- all mandatory components have verified support and compatibility;
- exact versions and package sources are recorded at execution time;
- the installation order is followed;
- every mandatory validation item passes;
- backup/restore and clean-room reproduction succeed;
- conditional services are justified and documented;
- security and licensing findings are resolved or formally recorded;
- recovery instructions are tested;
- no real installation was performed without separate authorization.

**Current execution status: NOT EXECUTED.** This document is a researched plan only.

**End of document.**
