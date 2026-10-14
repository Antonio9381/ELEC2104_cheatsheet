# Claude session notes — ELEC2104 cheatsheet

Running log so Claude can pick up where we left off. Claude updates it whenever the user
mentions a change or a rule, and keeps the cheatsheet consistent with it.

## Goal
A concise, well-organised exam cheatsheet for the **ELEC2104 final exam** (USYD, 2026 S2).
The user reads the textbook and points Claude at interesting material; Claude condenses it
into the cheatsheet with auto-generated headings and a table of contents.

## Sources
- Textbook: Jaeger & Blalock, *Microelectronic Circuit Design* (2015), 1355 pages.
  Local copy: `textbook/Jaeger_Blalock_Microelectronic_Circuit_Design_2015.pdf`.
  **PDF page = book page + 29** (e.g. book p.12 = PDF p.41). Verified for ch 1; recheck later chapters if something looks off.
  Section numbers in the user's requests (e.g. "1.3") are **textbook** section numbers.
- Course content (weeks):
  - Week 12 (last week): Bode plots, filters (Lecture 11, Tutorial 12).
  - Weeks 1–11: unknown so far. Ask the user for the topics.

## Rules (from the user)
- Claude works only inside this directory.
- Keep cheatsheet content **concise and organised**.
- Auto-generate headings; keep a table of contents to make searching easy.
- When the user mentions changes or rules, record them here **and** apply them to the cheatsheet.
- **Add helpful extras** (mnemonics, units, worked one-liners, sanity checks, common pitfalls) when they make
  content clearer, kept concise.
- Headings use **textbook numbering** (`\section[1]{...}`, `\subsection[1.3]{...}`) so the cheatsheet matches the book.
- **Bold variables where the book does**: phasors are bold (`\ph{V_1}` gives bold V_1), e.g. phasor divider
  `\ph{V_1} = \ph{V_i} Z_1/(Z_1+Z_2)`. Scalars and instantaneous values (v_be, V_BE) stay normal.
- **Every subsection ends with a `handy` box** ("Handy formulas") of related formulas the user may need but that aren't the
  main content, e.g. ω = 2πf, reading f from sin(2000πt), dB conversions, phasor maths, Ohm/power, prefixes.
- **Print-friendly (A4, colour printer)**: pale fills with dark accent bars (no saturated or neon fills), margins at least 7 mm
  (printer-safe), formulas big enough to read on paper (use `\dfrac` in tables). Print at **100% / Actual size**, not "fit to page".
- **End of every chapter** add: `N.K` Key Takeaways & Watch Outs, `N.F` Chapter N Formula Sheet (all formulas from the chapter),
  `N.C` Constants & Key Numbers. Physical constants go in the chapter that first uses them.
- **Font: Fira Sans + Fira Math** (unicode-math, LuaLaTeX) with smaller subscripts (`\DeclareMathSizes{5}{5}{3.8}{3}`).
  **Word-like subscripts are upright**: `R_{\mathrm{th}}`, `v_{\mathrm{sat}}`, `I_{\mathrm{max}}`, `V_{\mathrm{rms}}`, `R_{\mathrm{in}}`, `A_{\mathrm{dB}}`, ...
  Variable or notation subscripts stay italic as in the book (`V_{BE}`, `v_{be}`, `\mu_n`, `N_D`, `v_i`). For bold inside maths
  use `\symbf{}`/`\symbfup{}` (Fira Sans Bold lacks some maths glyphs, e.g. italic Ω); `\ph{}` (`\mathbf`) is fine for Latin letters.
- Files are named by textbook chapter: `sections/NN_Topic.tex` (NN = chapter number).

## How to add material (for the user)
Quickest to slowest:
1. **Point at it:** `"add 1.3 and 1.4"`, `"Add Sec 10.4"`, `"Add Table 5.2"`, `"Add p. 512–515"` (book page numbers are fine).
2. **Topic + chapter:** `"Add MOSFET small-signal model, ch 13"`. Claude finds the pages.
3. **Whole chapter:** `"Summarise ch 10"`. Claude proposes the key points, then you trim.
4. **Screenshots / lecture slides / tutorial PDFs:** for things not in the textbook
   (lecturer's notation, tutorial tricks), or figures that don't extract well. Put files in
   `img/` or drag them into chat.

Tips:
- Say **why** it matters ("I always mix up the sign") and Claude will keep it to that one line.
- Batch several items in one message if you like; Claude places each one in the right section.
- To change the style: `"make equations bigger"`, `"3 columns"`. It becomes a rule here.
- `"Update the summary"` makes Claude save progress here at any time.

## Cheatsheet structure
- `main.tex`: settings (`\subject`, `\semester`, `\sheetauthor`, `\cols`) plus `\contents` and one `\input` per section.
- `src/default.tex`: preamble and macros (cleaned up 2026-10-12).
- `sections/NN_Topic.tex`: one file per textbook chapter (NN = chapter number), `\input` in chapter order.
- `cheatsheet_template.zip`: clean copy of the template, including the example section and image (style reference).
- `img/`: images. `textbook/`: the PDF (read-only source).
- Build: **`latexmk -lualatex main.tex`** (LuaLaTeX required for Fira fonts). `.latexmkrc` sets `$pdf_mode=4`,
  `main.tex` has `% !TEX program = lualatex`, and `.vscode/settings.json` sets the LaTeX Workshop recipe to latexmk-lualatex.

### Macros
| Macro | Use |
|---|---|
| Colours | `secDark/secLight`, `subDark/subLight`, `subsubDark/subsubLight`, `eqDark/eqLight` (in `src/default.tex`) |
| `\section[1]{Title}` / `\subsection[1.3]{Title}` | coloured headings with textbook numbers, added to contents and PDF bookmarks (omit `[..]` to auto-number) |
| `\subsubsection{}` | small cyan label, not in contents |
| `\contents` | compact clickable table of contents |
| `\mathbox{$...$}` | boxed key equation |
| `\tb{p.12}` | grey right-aligned textbook page tag `[J p.12]`; straight after a `\subsection` it sits on the heading line if there is room, otherwise on its own line; mid-paragraph it goes at the end of the line |
| `\SI{10}{\kilo\ohm}` | units (siunitx); `\qty` is physics' version, not siunitx's |
| `\ph{V_1}` | bold phasor (book convention) |
| `\begin{handy}...\end{handy}` | grey "Handy formulas" box at the end of each subsection |
| `\filtresp{LP}{(0,1) -- (0.5,1) -- (0.5,0)}{extra}` | tiny ideal frequency-response sketch (reuse for week 12 filters) |
| `L` column type | ragged-right `X` column for `tabularx` (use instead of `X`) |
| circuitikz | small diagrams, e.g. `\begin{circuitikz}[scale=0.6] ... \end{circuitikz}` (compact bipoles preset) |
| `\circled{1}`, `\fillblank` | circled number; end column early |

## Current state (2026-10-12)
- Template cleaned; builds with no errors. Template saved as `cheatsheet_template.zip`.
- **Chapter 1 done** (user skipped 1.1, 1.2 and 1.8): 1.3–1.7 plus 1.K/1.F/1.C, filling page 1 (4 columns, about 85%).
  Each subsection has a Handy formulas box.
- **Chapter 2 in progress** (`sections/02_SolidState.tex`, starts page 2): 2.1–2.10 done; Ch 2 fills page 1 col 4 plus all of page 2 (last column about 60% full). 2.11 (IC fabrication) is next in the book. Add 2.K/2.F/2.C when the user says the chapter is finished.

## Session log
### 2026-10-12
- Set up `CLAUDE.md` and `claude_assist/` for memory between sessions.
- Downloaded the textbook into `textbook/`.
- Learned that week 12 covers Bode plots and filters.
- Template clean-up:
  - fixed the broken `\underbrace`/`\overbrace` redefinition
  - switched `babel` from German to English
  - removed duplicate and unused packages and dead commented code
  - added `hyperref`, numbered headings that feed `\contents` and PDF bookmarks, `siunitx`, and the `\tb{}` reference macro
  - set subject and semester to ELEC2104 / 2026 S2
  - renamed `\author` to `\sheetauthor` so it no longer overrides the LaTeX command
  - replaced `soul` `\hl` headings with `\colorbox`, so math in headings works
- Zipped the template to `cheatsheet_template.zip`, then removed `sections/00_Example.tex` and `img/example.png`.
- Added 1.3 and 1.4 (`sections/01_Intro.tex`), with extras: notation table and rule, dependent-source table with gain units,
  the V/mA/kΩ unit shortcut, and a sanity-check list.
- Template: optional textbook numbering in `\section`/`\subsection`, table row padding (`\extrarowheight`),
  `\tb` sits on the heading line.
- New rule: add helpful extras concisely.
- New rule: bold variables (phasors) as the book does. Added `\ph{}`, fixed the 1.3 phasor entry.
- Added 1.5 and 1.6, with extras: divider validity conditions and the "other resistor" reminder, parallel shortcuts, loaded-divider pitfall,
  Thévenin/Norton circuitikz diagram, the R_th procedure (zero independent sources, keep dependent ones), the Ex 1.1 (β+1) pattern,
  square-wave odd-harmonics note, a trimmed Table 1.3, and a −3 dB / bandwidth preview (links to week 12 Bode).
- Added 1.7 Amplifiers (gain phasor, phase/degree pitfall, worked exercise, 1.7.1 ideal op amp with inverting circuit, virtual ground,
  non-inverting extra, 1.7.2 filter types with sketches and the "split into frequency components" method).
- New rule: a Handy formulas box at the end of every subsection. Added to 1.3–1.7 (`handy` env). Added the `\filtresp` macro.
- Print redesign: pastel heading boxes with left accent bars (magenta/amber/teal), light-blue key-equation boxes, header with
  page number (no footer), print-safe margins (0.7 cm sides, 1.15 cm top, 0.7 cm bottom), grey column rules.
- Chapter 1 end: added 1.K Key Takeaways (incl. 1.9: 3 sig figs), 1.F Formula Sheet, 1.C Constants & Key Numbers.
- New rules: print-friendly design; chapter-end K/F/C subsections.
- Added 2.1 (resistivity classes, elemental vs compound, why Si, bandgap table, periodic table snippet with a donor/acceptor preview)
  and 2.2 (covalent bonds, electron–hole pairs, n_i² = BT³exp(−E_G/kT) with B/E_G table, Ex 2.1, pn = n_i², n_i ≈ 10¹⁰ for Si).
  Extras: R = ρL/A and unit conversions, kT/V_T at 300 K, the n_i temperature-ratio formula, n_i doubling about every 9 K (computed, not from the book).
- Added 2.3 (j = Qv, mobility v = ∓μE with Si μn = 1420 / μp = 470, velocity saturation sketch, v_sat ≈ 10⁷ cm/s, Si/Ge/GaAs table
  from the exercise answers). Extras: n-type faster than p-type, E = V/L, transit time, unit checks, 2.4 preview formulas.
- Added 2.4 (σ, ρ, Ex 2.2), 2.5 (donor/acceptor table + memory aid), 2.6 (charge neutrality, exact + practical n/p formulas,
  majority T-independent vs minority T-dependent, Ex 2.3, step recipe), 2.7 (μ vs N_T plot drawn from the Fig. 2.8 equations,
  μ table computed from those equations, σ ≈ qμ(N_D−N_A), the trap: μ uses N_T = sum but carriers use the difference, Ex 2.4/2.5).
  Book errata noted: Fig. 2.8 labels both equations μ_n (the second is μ_p); Ex 2.4 text says 2e15 but the values are for 4e15.
- Template: long `\subsection` titles now wrap onto two lines instead of overflowing.
- Added 2.8 (diffusion j = ∓qD∂/∂x with sign explanation, Einstein D/μ = V_T, V_T table, gradient-unit trap), 2.9 (total = drift +
  diffusion, Einstein form, Gauss' law, equilibrium cancellation preview), 2.10 (band model with intrinsic/donor/acceptor diagrams,
  E_G = E_C − E_V, E_C − E_D ≈ 0.045 eV, E_A − E_V ≈ 0.044 eV, compensated n = N_D − N_A, comparison with kT).
  V_T convention: use the book's **25.8 mV** at 300 K (changed the 2.2 Handy box from 25.9).
- Font: compared 6 options (font_test, since deleted). The user chose **Fira Sans + Fira Math**; switched to LuaLaTeX and made
  46 word-like subscripts upright. Ch 2 now starts on page 1, column 4. (`cheatsheet_template.zip` still holds the old pdflatex/CM template.)
- Template: circuitikz, `\raggedright` text, `L` ragged table column, `needspace` (headings stay with content), smarter `\tb`.

## Next steps / open items
- Get the weekly topic list for weeks 1–11, then create the section skeleton (`sections/01_...`).
- Find which textbook chapters cover week 12 (Bode plots, filters).
