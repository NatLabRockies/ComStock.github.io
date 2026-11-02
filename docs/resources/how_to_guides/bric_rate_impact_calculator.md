---
layout: default
title: BuildStock Rate Impact Calculator
parent: Resources
nav_order: 11
---

# How-to: Calculate Custom Rates with the BuildStock Rate Impact Calculator
{: .no_toc }

The BuildStock Rate Impact Calculator (BRIC) estimates how utility rate structures and building efficiency upgrades interact to affect customer energy bills across a region. It combines NLR's ResStock (residential) and ComStock (commercial) building stock datasets with PySAM's utility rate engine to bill thousands of simulated buildings under real or custom tariffs, before and after an upgrade, and optionally under several alternative rate designs at once.

The calculator can help answer questions such as:
-	**How would a new time-of-use rate change bills for commercial buildings in my county?**
-	**Does a heat pump upgrade still save customers money under our utility's current tariff?**
-	**Which building types would pay more, and which would pay less, under an alternative rate design?**

This guide covers the workflow, explains how sampling **weights** work in each dataset (essential for reading the numbers correctly), and walks through every section of the Results page with guidance on interpretation.

<details open markdown="block">
<summary>Contents</summary>
1. TOC
{:toc}
</details>

## Before you start

1.	**[Download the app]({{  site.baseurl  }}{% link docs/for_developers/bric_installation.md %}#download).** Stand-alone zip files are available for Windows, macOS, and Linux. Unzip the file and double-click the app; no installation is needed. Step-by-step instructions, including how to run the app from source, are in [Install the BuildStock Rate Impact Calculator]({{  site.baseurl  }}{% link docs/for_developers/bric_installation.md %}).
2.	**Get a free OpenEI API key** by registering at <https://openei.org/services/api/signup/>. The key is needed to look up utility rates. Paste it into **Setup → OpenEI API Key** in the app's sidebar.
## Concepts

**Datasets.** ResStock and ComStock are NLR simulations of the U.S. residential and commercial building stock. Each dataset release contains tens of thousands of *building models* (statistically sampled combinations of building characteristics) with simulated energy use at 15-minute resolution for a 2018 actual meteorological year (AMY) weather file. Supported releases are ResStock 2024.2 and 2025.1, and ComStock 2024.2 and 2025.3.

**Upgrades (scenarios).** Each release includes a baseline (upgrade 0) and a set of efficiency measure packages (upgrades 1–16 for ResStock, 1–39 for ComStock). Every scenario you select is billed independently. Not every upgrade applies to every building; see [Upgrade vs. Baseline Comparison](#upgrade-vs-baseline-comparison).

**Rates.** Electricity rates are URDB-format tariffs, either fetched from the [OpenEI Utility Rate Database](https://openei.org/wiki/Utility_Rate_Database) or built in the app. They are evaluated by PySAM, so time-of-use periods, tiers, demand charges, fixed charges, and minimum charges are all applied against each building's 8,760-hour load profile. Fuel rates (natural gas, propane, fuel oil) are simpler custom flat or block rates applied to annual consumption (or monthly consumption, for block rates). When no fuel rate is assigned, the dataset's own default fuel bill column is used where available. A fuel rate's monthly fixed charge accrues only for buildings that actually consume that fuel; a building with zero consumption of a fuel is not billed its fixed charge.

**Rate assignment vs. rate sweep.** You assign one or more rates to each scenario. The **first** rate assigned to a scenario is its *primary* rate, the one the savings, distribution, and summary views report. Any *additional* rates assigned to a scenario bill it a second time (a rate sweep), enabling the scenario × rate matrix and the multi-rate charts. A scenario can be assigned **All Configured Rates** at once.

**Weights.** Every building model stands in for many real buildings. The `weight` column says how many. [Understanding weights](#understanding-weights) explains how the two datasets differ and exactly where weights enter the math.

## Workflow walkthrough

The app is a six-step workflow shown as a stepper across the top of the page: **1 Dataset → 2 Geography → 3 Building Filters → 4 Energy Rates → 5 Run → 6 Results**. Completed steps show a ✓ in the stepper. Use the **Next →** and **← Back** buttons at the bottom of each page to move through the workflow (Next is disabled until the current step has what it needs), or click any step in the stepper or in the sidebar's **Workflow** panel to jump directly to it.

Selections on most steps apply immediately. The Geography step's selection is applied, and building metadata downloaded, when you click **Next** or the page's **Apply selections** button. Progress is saved into a *project* (a folder under your output directory containing `config.json` and the rate definitions), which can be reloaded from the sidebar later. Loading a project jumps you to the first unfinished step.

### Step 1: Dataset
Pick the dataset type (ResStock or ComStock) and release, and select the baseline plus the upgrade scenarios to analyze. Scenario names are fetched from the release's measure documentation.

### Step 2: Geography
Select states, counties, or PUMAs on the map. **ComStock requires a county-level selection**: its usable metadata is published per county (PUMA selections are ignored for ComStock), and every load profile download is scoped to your selection. The selection is applied, and the building metadata downloaded, when you click **Next**. The page's **Apply selections** button does the same and lets you preview the applicability table without leaving the step.

### Step 3: Building Filters
Filter the building stock by characteristics: building type, heating fuel, vintage, floor area, HVAC type, income bin (ResStock), annual electricity consumption, and any additional `in.*` metadata columns you add. The **Matching Buildings** metric shows how many building models survive your filters. A warning appears below 30 models (see [Interpreting results](#interpreting-results)).

### Step 4: Energy Rates
Build the rate library by fetching URDB tariffs by utility or label, or by constructing flat, time-of-use (TOU), tiered, or demand rates in the editor.

To add URDB rates:
1.	Open the **Search URDB** tab and type part of a utility's name.
2.	Pick the utility, adjust the search filters (sector, service type, effective date, default rates only), and click **Search Rates**.
3.	In the results table, check the box in the **Select** column for each rate you want to add. You can select several at once. The **Label** column links to each rate's page on OpenEI.
4.	The selected rates are listed under **Rates to add**. Click **Add selected rates** to fetch them into the project.

URDB often lists several versions of a tariff under the same name. If a selected rate's name is already in the library, the new one is added as `<name> (<URDB label>)` so neither is overwritten.

Then assign one **or more** electricity rates to each scenario with the per-scenario multiselect. The first rate you pick for a scenario is its primary rate. Any additional rates bill that scenario again (a rate sweep) and populate the Rate Comparison matrix; runtime scales with the total number of rate assignments. Pick **All Configured Rates** to bill a scenario under every rate at once. Optionally, assign fuel rates per scenario and fuel.

At least one scenario must have an assigned electricity rate before **Next** unlocks, and the Run step warns when some scenarios are still unassigned.

{: .warning }
In-progress entries in the rate editors are not preserved if you navigate to another step before adding the rate.

### Step 5: Run
Review the configuration summary and upgrade applicability, optionally enable **Fast preview** (a stratified sample; see [Preview sampling](#preview-sampling)), and start the run. The pipeline:

1.	Loads and filters metadata for your selection.
2.	Downloads the needed 15-minute load profiles from OEDI. These are cached on disk, so reruns only fetch what's missing.
3.	Bills every building × scenario × rate through PySAM (electricity) and the fuel rate expressions.
4.	Flags rate eligibility and joins building characteristics onto the results.

Runs can be canceled, and progress, per-phase timing, and an activity log are shown live. When the run finishes, a **View Results →** button takes you straight to the Results step.

### Step 6: Results
Interactive charts, tables, and exports, covered in detail in [Interpreting results](#interpreting-results). Results are also written to `<project>/results/bill_results.csv` automatically.

### The sidebar
The sidebar shows a **Workflow** panel with one line per step, a completion mark, and a summary of your selections; click any line to jump to that step. Below it, **Setup** holds the data directory and OpenEI API key (expanded automatically until a key is stored), and **Project** holds the project name and the controls for loading an existing project.

### Stopping the app
Click **Quit app** at the bottom of the sidebar, then **Quit** to confirm, to stop the app. You can then close the browser tab.

Closing every browser tab also stops the app automatically after **2 minutes**, which is long enough to survive a page reload or a brief disconnect. The app will not stop while a run is in progress; it waits until the run finishes. Pressing `Ctrl+C` in the terminal also works. To keep the app running with no tab open (for example, when hosting it for several users), set the environment variable `BRIC_NO_AUTO_EXIT=1` before starting it.

## Understanding weights

Both datasets are *samples*: each building model represents a slice of the real stock, and the `weight` column is the number of real-world buildings (or dwelling units) that model stands for. The two datasets handle weights differently, and the app's math follows suit. Getting this right is the difference between reading a number as "a typical customer's bill" and "the whole region's bill."

### ResStock: one scalar weight, per-building bills

ResStock samples the residential stock at a fixed rate, so **every row carries the same weight**: a single dwelling-units-per-model value fixed by the release. The app reads this scalar once from the metadata.

- **Bills are computed per building.** Each model's unscaled 8,760-hour load goes through PySAM, so `annual_bill` is what *one household* of that type would pay. Tier boundaries, demand charges, and fixed charges apply at true household scale.
- **Weights enter only at aggregation:**
  - Totals: `total_annual_bill = Σ (weight × annual_bill)`, in dollars across the represented stock.
  - `units = Σ weight`, the dwelling units represented.
  - Means and medians are simple per-model statistics (each model counts once), representing the *typical modeled household*, not a weight-blended average.

**Interpretation:** ResStock means, medians, and distributions answer "What does a household pay?" Totals answer "What does the selected stock pay in aggregate?"

### ComStock: per-row weights, pre-weighted bills

ComStock weights vary by building and by geography, and **the same `bldg_id` can appear on several rows**, one per county instance, each with its own weight. (Internally, every metadata row gets a unique `_row_uid` so these instances never collide.)

- **Loads are multiplied by weight *before* billing.** Each row's load profile is scaled by its weight before PySAM runs, so `annual_bill`, `annual_energy_kwh`, and `peak_demand_kw` on a ComStock results row are **weighted, stock-level quantities**: real-world dollars, kWh, and kW for all the buildings that row represents, not one building's bill.
- **Per-building figures are recovered by dividing the weight back out.** The app does this wherever a per-building statistic is shown:
  - Cost intensity: `$/ft² = annual_bill / (weight × sqft)`
  - Peak intensity: `W/ft² = peak_demand_kw × 1000 / (weight × sqft)`
  - Eligibility screening compares `annual_energy_kwh / weight` and `peak_demand_kw / weight` against a rate's window.
- **Totals are direct sums**, since the weighting is already included: `total_annual_bill = Σ annual_bill`, and floor area is `total_sqft = Σ (weight × sqft)`.
- **Tiny-weight rows are excluded up front.** Rows whose weighted floor area is 333 ft² or less are dropped before billing, because their very small weights otherwise produce unrealistic $/ft² intensities. The run log reports how many were excluded.

**Interpretation:** ComStock raw dollar figures are *stock* quantities. The comparable per-building lens is always the **$/ft² intensity**, which is what the app reports as means and medians. Never read a ComStock `annual_bill` as one building's bill.

{: .warning-title }
> Nonlinear rate features at weighted scale
>
> Because ComStock loads are weight-scaled before billing, tier thresholds, demand charges, and fixed charges in the tariff are evaluated against the *aggregate* load of the represented buildings, not each building individually. For flat and TOU energy charges (linear in load) this is exact; for strongly tiered rates or per-meter fixed charges, per-building $/ft² intensities derived from weighted bills are approximations. Rows with weights near 1.0 are unaffected; be more cautious with high-weight rows under block-heavy tariffs.

### Preview sampling

**Fast preview** bills a stratified sample instead of the full selection (N models per building type stratum; configurable). To keep the numbers unbiased, each sampled row's weight is multiplied by its stratum's scale factor, `original stratum weight ÷ sampled stratum weight`, so:

- **Weighted totals remain unbiased estimates** of the full-run totals.
- **Means and medians are sample estimates.** Expect them to vary relative to a full run, especially in small strata.
- Fully kept strata get a scale of exactly 1.0 (no distortion).

Preview results are marked with a banner on the Results page and flagged in exports. Use preview to iterate on rate designs, then rerun without it for final numbers.

## Interpreting results

All statistics on the Results page respect three global controls at the top:

- **Energy Sources pills**: which bill components (Electricity, Natural Gas, Propane, Fuel Oil) feed the summary, distribution, and savings statistics. Electricity-specific sections (components, monthly profile, peak, rate comparison) always use electricity regardless.
- **Rate eligibility toggle**: when the assigned rate declares a peak-demand (kW) or monthly-consumption (kWh) applicability window and some buildings fall outside it, a warning reports the weighted share, and a toggle lets you exclude them from every statistic. Buildings are *flagged, not silently dropped*; you decide.
- **Guardrail banners**: a preview-run notice and a small-sample warning (below 30 building models, weighted statistics are unreliable).

With a rate sweep configured, **every section except Rate Comparison shows the primary (assigned) rate only**. The multi-rate views are the Mean Annual Electricity Bill chart, the Electricity Bill Distribution, the Monthly Electricity Bill Profile, and the Rate Comparison matrix.

### Bill Summary

Three views:

- **Total bill**: a per-scenario table over the selected energy sources. For ResStock: mean and median per-household bill, weighted total, and dwelling units. For ComStock: mean and median $/ft², weighted total dollars, and total ft².
- **By fuel**: a stacked bar of per-source mean bills, showing how much of the bill each fuel contributes per scenario.
- **By charge component**: a stacked bar decomposing the *electricity* bill into Fixed, Energy, Demand, and Minimum charges (from PySAM). Use this to see *why* a rate design changes bills, such as a TOU rate shifting dollars from energy to demand charges. Fuel bills are not decomposed.

### Rate Comparison (rate sweep only)

The scenario × rate matrix, in two views:

- **Mean bill**: mean electricity bill ($ or $/ft²) for each scenario under each rate. Compare *across a row* to see the rate design's impact on the same building stock and measure.
- **Mean savings**: **same-rate-baseline** savings, meaning the baseline under rate R minus the upgrade under rate R. This answers *"What does this measure save under this rate design?"* It deliberately does **not** mix rates; the cross-rate question ("What happens if customers also switch rates?") is read from the Mean bill view instead.

### Mean Annual Electricity Bill

A bar chart with bars grouped by scenario and colored by rate. Every rate a scenario was billed under appears. Electricity only.

### Electricity Bill Distribution

Box-and-whisker plots per scenario (and per rate, when a sweep is configured), on per-building values: $/ft² for ComStock, $ for ResStock.

- Boxes span Q1–Q3 with a median tick; whiskers extend to the most extreme values within **1.5 × IQR** of the quartiles (Tukey convention).
- **Outliers are hidden by default** for rendering speed. Enable *Show outliers* to draw the individual points beyond the whiskers.
- A wide box or long whiskers mean the rate or measure affects customers very unevenly. Check the savings histogram and segment breakout to find out who sits where.

### Monthly Electricity Bill Profile

Mean monthly electricity bill per scenario (and per scenario–rate combination under a sweep; use the multiselect to declutter). The seasonal shape reveals rate–load interactions, such as a demand-heavy tariff spiking in cooling months or an envelope measure flattening winter bills. Fuel bills are annual only and never appear here.

### Bill Savings

Shown when a baseline and at least one upgrade are present. **Sign convention: positive savings mean the upgrade is cheaper than the baseline.** Savings compare each scenario *under its own assigned rate*. If you assigned different rates to the baseline and the upgrade, the savings include both the measure effect and the rate change. That is the point of the tool; use the Rate Comparison matrix to isolate the measure effect under a fixed rate.

- **Winners/losers line**: weighted shares of buildings that *save* (more than $1/yr), *increase* (savings below −$1/yr), or are *neutral* (within ±$1). A measure with positive mean savings can still leave 30% of customers paying more, so this line is the fastest equity check on the page.
- **Average / Total toggle**: Average shows per-building mean and median savings ($/ft² for ComStock) plus mean and median **percent savings** (relative to each building's own baseline bill). Total shows weighted aggregate dollar savings across the represented stock.
- **Savings histogram**: the full distribution, weighted by buildings represented. Look for mass below zero (bill increases) and long tails.
- **Break out savings by**: groups savings by a categorical building characteristic (vintage, building type, heating fuel, income bin, and so on) to find which segments a program should target. Segments with fewer than 30 models are flagged as unreliable.

### Peak Demand

Mean electricity peak demand per scenario, with the change vs. baseline: **W/ft²** for ComStock (per-building intensity) and **kW per dwelling** for ResStock. This is the maximum of PySAM's monthly peaks, the quantity demand charges are billed on, which makes it useful alongside the "By charge component" view when evaluating demand-based tariffs.

### Energy Burden (ResStock only)

Household energy bills as a share of income, using the selected energy sources and the dataset's binned income characteristic (`in.income_recs_2020`). Each bin maps to a point estimate: the bin midpoint, half the bound for "<20000", and 1.25 × the bound for "150000+". "Not Available" households are excluded. Thresholds follow DOE Low-Income Energy Affordability Data (LEAD) conventions: **high burden is 6% or more** and **severe burden is 10% or more** of income. The table reports mean and median burden and the weighted share of households above each threshold, per scenario, with the change vs. baseline.

Burden is a *ratio of two estimates* (modeled bills ÷ binned-income midpoints), so treat threshold shares as indicative, not precise. The measure of interest is usually the *change* in high and severe shares between scenarios.

### Upgrade vs. Baseline Comparison

Per-upgrade expanders comparing each upgrade against the baseline, with an **applicability** filter. Upgrades don't apply to every building; for example, a heat pump measure doesn't apply to buildings that already have one. Non-applicable buildings are billed using their **baseline load profile under the upgrade scenario's rate**, so for them, any bill change is purely a rate effect. Toggle "Applicable buildings only" to isolate the measure's effect on the buildings it actually affects; the box plots split applicable vs. not applicable.

### Export

- **Download Results CSV**: the full per-building × scenario × rate frame (see the [units cheat sheet](#units-cheat-sheet) for column meanings).
- **Download HTML Report**: a self-contained, shareable report with the run configuration, headline tables, and interactive charts. It needs an internet connection when opened, to load the chart library.
- **Save to Output Directory**: writes results and the rate JSON files into the project folder.

## Units cheat sheet

Electricity result columns carry an `electricity_` prefix to distinguish them from the fuel bill columns (`natural_gas_bill`, `propane_bill`, `fuel_oil_bill`). Results saved by older app versions used unprefixed names (`annual_bill`, `monthly_bills`, and so on) and are renamed automatically when the project is loaded.

| Quantity | ResStock | ComStock |
|---|---|---|
| `weight` column | Same scalar on every row (dwelling units per model) | Per row; one `bldg_id` may repeat across counties |
| `electricity_annual_bill`, `electricity_annual_energy_kwh`, `electricity_peak_demand_kw` (per row) | Per building (one household) | **Weighted** stock totals for that row |
| Per-building bill lens | `electricity_annual_bill` ($/household) | `cost_per_sqft` = bill ÷ (weight × ft²) |
| Mean / median statistics | $ per household (unweighted across models) | $/ft² (per-building intensity) |
| `total_annual_bill` | Σ weight × bill | Σ bill (already weighted) |
| Stock size metric | `units` = Σ weight (dwelling units) | `total_sqft` = Σ weight × ft² |
| Peak demand reporting | kW per dwelling | W/ft² |
| Savings in "Average" view | $ per household | $/ft² |
| Savings in "Total" view | Σ weight × savings | Σ savings (already weighted) |

Other result columns worth knowing:
- `scenario`: the scenario label.
- `rate_name` and `is_primary`: identify the rate sweep variant.
- `applicable`: whether the upgrade applies to this building.
- `rate_eligible`: whether the building is inside the rate's kW/kWh window.
- `bill_savings_pct`: savings as a fraction of the building's own baseline bill.
- `electricity_monthly_bills`: 12 monthly electricity values as a JSON list.
- `electricity_annual_fixed/energy/demand/min_charge`: the four charge components.
- `error`: non-null when PySAM failed for the row. These rows are excluded from statistics.
- Any `in.*` characteristic columns joined for filtering and segmenting.

## Modeling notes and caveats

- **Single-year, load-only analysis.** Bills are computed for one year of simulated load (AMY 2018 weather) with no on-site generation, net metering, escalation, or multi-year effects.
- **Simulated buildings, statistical weights.** Results describe the *modeled stock*. Individual real buildings will differ, and small filtered samples (fewer than 30 models) make even stock-level statistics unreliable. The app warns when this happens. See also [Sample Size Considerations]({{  site.baseurl  }}{% link docs/resources/explanations/sample_size_considerations.md %}).
- **ComStock weighted-load billing.** Nonlinear tariff features are evaluated at aggregate scale for ComStock (see [ComStock: per-row weights, pre-weighted bills](#comstock-per-row-weights-pre-weighted-bills)).
- **Electricity-only decomposition.** Charge components, monthly profiles, peak demand, and the rate sweep are electricity only. Fuel bills are annual and independent of the electricity tariff.
- **Fuel fixed charges require consumption.** A fuel rate's monthly fixed charge is billed only to buildings with non-zero consumption of that fuel; all-electric buildings pay nothing under an assigned gas, propane, or oil rate.
- **Rate eligibility windows** use URDB's declared limits. Peak-kW bounds are compared against the building's maximum monthly peak; monthly-kWh bounds are compared against annual consumption ÷ 12. Buildings with missing data are treated as eligible.
- **Energy burden** relies on binned-income midpoints (described above) and modeled bills, so read shares as indicative.
- **Non-applicable buildings** keep their baseline load under upgrade scenarios. When comparing all filtered buildings, upgrade-scenario statistics blend measure effects with pure rate effects for those buildings.
- **Error rows.** Occasional PySAM failures (malformed profiles, degenerate rates) produce rows with a non-null `error`. They're counted in the Calculation Errors metric and excluded from all statistics.
- **Caching.** Metadata and load profiles are cached under the output directory, and reruns reuse them. Delete the cache folder to force fresh downloads after a dataset release changes.

If you encounter issues or have suggestions, [open an issue](https://github.com/NatLabRockies/buildstock-rate-impact-calculator/issues) on the calculator's repository or contact [ComStock@nlr.gov](mailto:ComStock@nlr.gov).
