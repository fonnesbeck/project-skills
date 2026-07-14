# Project Skills

An oh-my-pi-native plugin for developing data science projects from vague idea to verified implementation.

## Workflow

1. `project-skills-setup` configures repo-local defaults.
2. `project-scoping` turns a vague idea into a scoped project direction.
3. `project-spec` writes the formal project spec.
4. `project-tasks` breaks the spec into blocked, agent-sized tasks.
5. `project-implement` executes one approved task at a time.
6. `project-review` reviews completed work across code quality, reproducibility, and adaptive data-science axes.

Each command owns only its named stage. While a command is active, generic
workflows must not choose artifacts or advance the project; use the next
project-skills command only after the current command reaches its documented
stop condition.

## Project classes

The workflow supports three project classes:

- Research notebook projects.
- Analysis/modeling projects.
- End-to-end ML products.

`project-scoping` infers the class from the user's goal, artifacts, data risks, and implementation needs, then asks the user to confirm or correct it.

## Defaults

The plugin defaults to local markdown artifacts:

```text
.project-skills/config.toml
docs/project-skills/domain.md
docs/project-skills/data.md
docs/project-skills/specs/
docs/project-skills/tasks/
docs/project-skills/reviews/
evals/
```

`project-skills-setup` can override these paths for a repo.

## Install

For local development, link this repo into OMP:

```sh
omp plugin link /var/home/fonnesbeck/repos/project-skills
```

Restart OMP after linking so skill discovery reloads. Verify with:

```sh
omp plugin list
```

The plugin package is defined by `package.json`; OMP discovers the skills from the `omp.skills` entry pointing at `./skills`.

## oh-my-pi integration

The skills are written for oh-my-pi sessions. They refer to OMP-native tools and coordination patterns such as `read`, `grep`, `glob`, `todo`, `task`, `job`, `irc`, `lsp`, `edit`, `write`, local artifacts, and existing domain skills.

The workflow delegates specialized knowledge instead of duplicating it. Relevant domain skills include:

- `marimo-notebook`
- `marimo-pair`
- `model-plan-discovery`
- `pymc-modeling`
- `prior-elicitation`
- `model-evaluation`
- `pymc-testing`
- `requesting-code-review`
- `verification-before-completion`

## Attribution

This plugin is derived from Matt Pocock's `mattpocock/skills` repository under the MIT License. It adapts these source skills for data science project development and oh-my-pi:

- `setup-matt-pocock-skills`
- `grill-with-docs`
- `wayfinder`
- `to-spec`
- `to-tickets`
- `implement`
- `code-review`
