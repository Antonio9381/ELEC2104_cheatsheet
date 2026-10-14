# Instructions for Claude

- Work only inside this directory (`ELEC2104/cheatsheet`). Don't read or change files outside it unless the user asks.
- At the start of a session, read `claude_assist/SUMMARY.md` for context on past sessions.
- Whenever the user mentions a change or a rule, record it in `claude_assist/SUMMARY.md` (Rules section)
  and apply it to the cheatsheet so the two stay consistent.
- After meaningful changes, update `claude_assist/SUMMARY.md`: current state, a dated entry in the session log, and next steps.
- After editing any `.tex` file, rebuild with **LuaLaTeX** (`latexmk -lualatex main.tex`; pdflatex will not work) and fix any errors.
- The textbook is `textbook/*.pdf`; read pages from it directly instead of asking for screenshots.
