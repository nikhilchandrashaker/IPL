# IPL Ball-by-Ball Analysis

Analysis of the Indian Premier League dataset: 1,095 matches and 260,920 deliveries, 2008–2024 (`matches.csv`, `deliveries.csv`).

## Contents

| File | What it is |
|---|---|
| `ipl_field_intelligence.html` | Self-contained interactive dashboard — open in any browser, no server needed |
| `ipl_statistical_report.html` | Rendered R statistical report |
| `ipl_report.Rmd` | Source for the R report — edit and re-render with `rmarkdown::render()` |

## Dashboard (`ipl_field_intelligence.html`)

Three tabs:
- **Analytics** — season scoring trends, toss impact, team win rates, powerplay vs. death-overs comparison, career leaderboards
- **Predict** — two live models you can feed match-state inputs into:
  - Win probability (logistic regression, AUC 0.86)
  - Final score projection (ridge regression, MAE ~16 runs)
- **Replay** — ball-by-ball scrubber/playback for 98 curated matches (every final, qualifier, eliminator, semi-final, plus the two closest league games per season), with the win-probability model running live as the innings unfolds

Team names are normalized across franchise renames (Delhi Daredevils → Delhi Capitals, Kings XI Punjab → Punjab Kings, Royal Challengers Bangalore → Royal Challengers Bengaluru, Rising Pune Supergiant → Rising Pune Supergiants) so career and franchise stats aren't artificially split.

## Statistical report (`ipl_statistical_report.html`)

Built in R (ggplot2, dplyr, lme4). Four findings:

1. **Team strength, adjusted for opponent** — a Bradley–Terry model (`glm()`) rates teams by who they actually beat, not raw win %. Several mid-table teams turn out to be statistically indistinguishable once you look at the confidence intervals.
2. **Toss value** — winning the toss alone is barely different from a coin flip (χ² p = 0.59). The real edge is in the *decision*: electing to field first after winning it.
3. **Venue effects, season-adjusted** — a mixed-effects model (`lme4::lmer`) separates genuine venue scoring effects from the fact that some grounds simply hosted more matches in recent, higher-scoring seasons.
4. **Death overs vs. powerplay** — a paired t-test confirms the run-rate jump in overs 16–20 holds match-by-match, not just on average.

To re-render the report:
```r
rmarkdown::render("ipl_report.Rmd")
```
Requires: `dplyr`, `ggplot2`, `lme4`, `tidyr`, `scales`, `rmarkdown` (all installable via `apt install r-cran-<name>` on Ubuntu, no CRAN access needed).
