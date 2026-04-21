# Copilot Workspace Instructions

## Mandatory Dev Checklist

Run these before every commit — in order:

```bash
uv run ruff check .   # 1. Lint — must pass with no errors
uv run pytest         # 2. Test — all 25 tests must pass
```

> Never skip. No unused imports, no missing type hints on new functions.

## Project

**Soc Ops** — Social Bingo for in-person mixers. FastAPI + Jinja2 + HTMX. See [README.md](../README.md).

```bash
uv sync                                                            # Install deps
uv run uvicorn app.main:app --reload --host 0.0.0.0 --port 8000   # Dev server
```

## Architecture

| File | Role |
|---|---|
| `app/main.py` | FastAPI routes — return HTML partials (not JSON) |
| `app/game_service.py` | `GameSession` dataclass — server-side state per cookie |
| `app/game_logic.py` | Pure functions: board gen, toggle, bingo detection |
| `app/models.py` | Frozen Pydantic models — use `.model_copy(update={})` |
| `app/data.py` | `QUESTIONS` list + `FREE_SPACE` constant |
| `app/templates/components/` | HTMX partials: game_screen, start_screen, bingo_board, bingo_modal |
| `app/static/css/app.css` | Custom utility classes — see [css-utilities instructions](instructions/css-utilities.instructions.md) |

## Key Conventions

- **HTMX-first**: no custom JS — all interactivity via HTMX attributes in templates
- **Immutable models**: `frozen=True`; mutate via `.model_copy(update={...})`
- **Pure logic**: `game_logic.py` has no side effects; `game_service.py` owns state
- **Python style**: snake_case, type hints required, Python 3.13+, Ruff rules E/F/I/N/W

## Agents & Prompts

- `quiz-master.agent.md` — generate themed icebreaker questions for `app/data.py`
- `tdd*.agent.md` — TDD red/green/refactor workflow
- `pixel-jam.agent.md` — creative frontend redesign
- `setup.prompt.md` — local dev environment setup
