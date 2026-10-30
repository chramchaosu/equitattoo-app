# 1. Public repository, all rights reserved

Date: 2026-10-30. Status: accepted.

**Context.** The project needs branch protection, CodeQL and secret scanning. On a personal GitHub Free account,
branch protection covers only public repositories, and code and secret scanning on private repositories are paid
add-ons sold with Team and Enterprise plans.

**Decision.** The repository is public, with an all-rights-reserved licence.

**Consequences.** Everything required is free. Anyone can read the code, including the security rules, so safety
must rest on correct rules and tests, not on hidden code. Client data, photos and secrets never enter the repository.
