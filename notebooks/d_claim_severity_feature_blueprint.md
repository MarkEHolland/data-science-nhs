# Claim Severity Feature Blueprint (NHS Resolution)

Purpose: To predict severity, you need features that capture system strain, information quality, care pathway complexity, and population risk — not just incident-level details.

Five feature families, each with concrete examples.

# 1. System Strain & Production Efficiency Features

These capture how hospital operational pressure increases the probability of severe harm.

Why they matter: Economic evidence shows that severity increases when systems are strained, queues form, and decision quality drops. This aligns with severity weighting logic: severe outcomes reflect large “shortfalls” in expected health .

Feature candidates:
- Bed occupancy % at time of incident
- ED queue length / waiting time
- Theatre utilisation rate
- Staffing ratios (RN:patient, midwife:birth)
- Agency staff proportion
- Overnight / weekend indicator
- Number of concurrent critical incidents
- Delayed escalation flags (time-to-review, time-to-intervention)

Feasibility: Moderate.

Most trusts have operational dashboards; extracting these into structured datasets requires cooperation but is achievable.


# 2. Information Quality & Documentation Features

These proxy the information asymmetry and documentation gaps that drive severe outcomes.

Why they matter: Severity weighting research emphasises that severe cases reflect large gaps between expected and actual health outcomes — often caused by information failures (poor documentation, missed escalation) .

Feature candidates:
- Completeness of clinical notes (missing fields, missing timestamps)
- Number of handovers
- Diagnostic pathway complexity (steps, branching)
- Time between observations
- Missing vital signs
- Inconsistent coding patterns
- Free-text sentiment / uncertainty markers (NLP)

Feasibility: High for structured fields.

Medium for NLP (depends on access to EPR text).


# 3. Care Pathway Complexity & Variation Features

These capture how variation in practice leads to severe harm.

Why they matter: Injury severity modelling literature shows that variation in care pathways is strongly associated with worse outcomes and higher economic cost .

Feature candidates:
- Number of steps in the pathway
- Deviation from standard pathway (protocol adherence score)
- Outlier clinician behaviour (intervention rates, escalation patterns)
- Time-to-diagnosis
- Time-to-treatment
- Unexpected transfers (ED → ward → ED → ICU)

Feasibility: Medium.

Requires pathway reconstruction from timestamps and coded events.


# 4. Population Risk & Inequality Features

These capture underlying risk exposure that amplifies severity.

Why they matter: Severity weighting frameworks (absolute and proportional shortfall) show that severity is higher when patients start from worse baseline health or face greater lifetime health loss.

Feature candidates:
- IMD decile
- Ethnicity mix
- Comorbidity burden (Charlson index)
- Birth complexity index (for maternity)
- Age × comorbidity interaction
- Language/communication barriers

Feasibility: High.

Most of this is already in HES or trust-level datasets.


# 5. Incident-Level Severity Predictors

These are the traditional features but enriched with economic severity logic.

Why they matter: Severity weighting research shows that severe cases represent large QALY shortfalls — meaning the incident type, age, and baseline health strongly influence severity categories .

Feature candidates:
- Age at incident
- Baseline health status
- Type of harm (neurological, obstetric, surgical)
- Complication cascade length

ICU admission indicator
- Mortality risk score
- Long-term disability likelihood

Feasibility: High.

This is the easiest category.





# How realistic is it that you can build upstream features?
Short answer: You can build ~60–70% of these features realistically. The remaining 30–40% require trust-level cooperation or new data pipelines.

Below is a realistic breakdown.

## High feasibility (you can do this now)
- IMD, ethnicity, comorbidity
- Age, baseline health
- Incident type, harm type
- ICU admission, mortality risk
- Time-to-treatment (if timestamps exist)
- Missing data indicators
- NLP on claim documents (you already do this)

Reason: These exist in HES, claims data, or trust-level structured fields.

Medium feasibility (requires trust collaboration or partial data)
- Staffing ratio
- Agency proportion
- Bed occupancy
- Queue length
- Theatre utilisation
- Handover count
- Diagnostic pathway complexity
- Protocol adherence scores

Reason: Trusts have this data, but not centrally.
You need:
- data-sharing agreements
- operational dashboards
- EPR access
- standardisation across trusts

This is achievable but slow.

## Low feasibility (requires new data collection or NHS-wide change)
- Real-time escalation delays
- Documentation quality scoring
- Coordination cost metrics
- Multidisciplinary alignment indicators
- Clinician behavioural variation at scale

Reason:  
These require:
- new data fields
- structured documentation standards
- consistent timestamping
- cross-trust behavioural analytics

This is multi-year work.

# Your realistic path forward (practical strategy)
## Phase 1 — Build what you can now (high feasibility)
You can immediately build:
- population risk features
- incident-level severity features
- NLP-derived information quality features
- missingness and variation indicators
- pathway reconstruction from timestamps

This will already improve severity prediction significantly.

Phase 2 — Partner with 3–5 pilot trusts
Aim to collect:
- staffing ratios
- bed occupancy
- queue lengths
- escalation delays
- documentation completeness

This gives you upstream features for a subset of claims — enough to demonstrate value.

## Phase 3 — Create a national upstream-feature standard
Use pilot results to propose:
- a national “severity modelling dataset”
- upstream operational metrics
- documentation quality standards
- pathway complexity fields

This aligns with NICE’s severity weighting logic (absolute/proportional shortfall) and broader severity modelling frameworks.

