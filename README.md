# AIProviders

Monorepo contenant **aiproviders** (bibliothèque Python pour les fournisseurs IA) et **django-aiproviders** (intégration Django).

## CI

CI checks the Python 3.12 / Django 5.2 installation, package builds, Ruff,
Bandit, and Python package dependency advisories. The Django check, Ruff, and
Python Bandit findings are temporarily non-blocking; there are no automated
tests yet. The Django dependency audit is not yet available because the local
Python package is unpublished.

## Packages

### aiproviders — `python-aiproviders/`

Bibliothèque Python pour la gestion des fournisseurs d'IA. Interface unifiée vers plusieurs providers (OpenAI, Anthropic, Google, Mistral, Meta, Scaleway) via ProviderKit.

- **Providers** : OpenAI, Anthropic, Google Gemini, Mistral, Meta Llama, Scaleway
- **Services** : prompt, generate, classify, run_llm_provider
- **CLI** : `aiproviders prompt --instruction "..."`, `aiproviders provider --list`

📁 Détails : [python-aiproviders/README.md](python-aiproviders/README.md) | Docs : [python-aiproviders/docs/](python-aiproviders/docs/)

### django-aiproviders — `django-aiproviders/`

Intégration Django pour AIProviders.

📁 Docs : [django-aiproviders/docs/](django-aiproviders/docs/)

## Structure du dépôt

```
aiproviders/
├── python-aiproviders/   # Bibliothèque core
├── django-aiproviders/   # Intégration Django
└── README.md
```

## Développement

Chaque package a son propre `service.py` :

```bash
# Dans python-aiproviders/ ou django-aiproviders/
./service.py dev install-dev
./service.py dev test
./service.py quality lint
```

## Licence

MIT

## CI packaging check

CI also installs the built wheel(s) in a clean environment and checks their
dependencies and imports without publishing. This installation check is
non-blocking during the initial rollout.

## Local audit

Install the project's development/test dependencies (including both local
packages where applicable), plus the audit tools:

```bash
python -m pip install ruff mypy bandit pip-audit vulture radon xenon pytest pytest-django pytest-cov build twine
make audit
```

Use the activated environment's Python, or pass `make audit PYTHON=/path/to/python`.
On systems without Make, run `python scripts/audit.py` directly. Git is required;
the geoaddress widget tests also require Node.js. The audit reports missing test
suites and runs all available checks even when earlier checks fail.
Xenon checks the absolute C, module B, and average A complexity thresholds. Existing
lint, typing, security, dependency, or test findings can make the audit fail.
CI also runs this audit as a non-blocking job and separately reports Radon maintainability without imposing a threshold.
