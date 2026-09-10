# The Economics of Health and Health Care — NHS Resolution–Relevant Summary

## 1. The book’s central idea: health systems behave like economic ecosystems

The book frames health care as a complex market with imperfect information, asymmetric incentives, and high stakes.

For NHS Resolution, this matters because clinical negligence claims are downstream outputs of upstream economic behaviours inside hospitals.

The book’s core themes map directly to your modelling work:
- Incentives → behaviour → harm → claims → severity
- Resource constraints → variation → errors → high-cost incidents
- Information gaps → poor decisions → avoidable harm

This gives you a conceptual backbone for designing severity models that reflect why incidents happen, not just what happened.

## 2. How the book explains claim severity drivers
### A. Asymmetric information

Patients, clinicians, managers, and regulators all hold different information.

The book shows how this creates:
- documentation gaps
- delayed diagnosis
- missed escalation
- poor handovers

These are exactly the patterns behind high-severity maternity, neonatal, and emergency claims.

Modelling implication - Include features that proxy information asymmetry:
- number of handovers
- staffing ratios
- agency usage
- time-of-day
- documentation completeness
- diagnostic pathway complexity

### B. Moral hazard & defensive behaviour

The book explains how insurance systems create moral hazard.

In the NHS, the analogue is defensive medicine and risk-averse behaviour.

This affects claim severity because:
- unnecessary interventions → complications → severe harm
- delayed interventions → catastrophic outcomes
- excessive referrals → system congestion → errors

Modelling implication - Capture behavioural signals:
- variation in intervention rates
- escalation delays
- overuse/underuse patterns
- outlier clinicians or departments

### C. Adverse selection & risk segmentation

Hospitals serve different populations with different risk profiles.

The book shows how population risk drives cost variation.

For NHS Resolution, this explains why:
- some trusts have persistently high maternity severity
- some emergency departments generate more catastrophic claims
- deprivation and comorbidity amplify harm trajectories

Modelling implication - Include population-level features:
- Index of Multiple Deprivation (IMD) decile
- ethnicity mix
- comorbidity (two or more health or mental health conditions) burden
- birth complexity index
- staffing stability

### D. Production functions & hospital efficiency

This is one of the most important parts for your modelling.

The book treats hospitals as production units converting inputs (staff, equipment, time) into outputs (health outcomes).
Inefficiency → variation → errors → severe harm.

Key efficiency drivers the book highlights:
- Labour productivity (skill mix, experience, burnout)
- Capital utilisation (equipment availability, theatre scheduling)
- Process flow efficiency (bottlenecks, queues, delays)
- Scale effects (high-volume centres have fewer severe incidents)

Coordination costs (handover quality, multidisciplinary alignment)

Modelling implication - Build features that capture production inefficiency:
- queue length / waiting time
- staff turnover
- bed occupancy
- theatre utilisation
- agency proportion
- weekend/overnight activity
- number of steps in the care pathway

These are powerful predictors of claim severity because severe harm often emerges from system strain.

## 3. How the book helps you improve NHS Resolution’s data collection

The book repeatedly emphasises that health systems under-collect the data that actually explains variation.

It argues for collecting:

### A. Inputs
- staffing levels
- skill mix
- equipment availability
- bed capacity
- IT system reliability

### B. Processes
- time-to-intervention
- escalation patterns
- diagnostic pathway steps
- handover quality
- queue lengths

### C. Outputs
- complications
- adverse events
- readmissions
- mortality
- long-term outcomes

## D. Context
- deprivation
- ethnicity
- geography
- comorbidity burden

NHS Resolution gap:  
- Claims data is rich on outcomes but thin on inputs and processes.
- The book gives you the economic justification for expanding data capture upstream.

## 4. How the book strengthens your claim severity modelling
### A. Severity is a function of system strain

The book’s production-function logic shows that catastrophic harm often emerges when:
- demand > capacity
- staff are stretched
- queues form
- decision quality drops

This gives you a theoretical basis for including operational strain variables in severity models.

### B. Severity is predictable from variation

The book’s repeated theme: variation is expensive.

Variation in:
- practice
- documentation
- escalation
- staffing
- intervention rates
→ leads to severe harm.

This supports using:
- outlier detection
- variation indices
- clustering of atypical pathways

C. Severity is amplified by inequality
The new chapter on disparities is directly relevant.

It shows how:
- deprivation
- ethnicity
- geography
drive worse outcomes.

This aligns with NHS Resolution’s maternity and neonatal patterns.

## 5. How the book helps you build “themes” in your model

The book’s conceptual structure naturally maps to themes you can use:

### Theme 1 — Information failure
- missed escalation
- poor documentation
- diagnostic delay

### Theme 2 — Production strain
- queues
- staffing gaps
- bed pressure

### Theme 3 — Behavioural economics
- defensive medicine
- risk aversion
- inconsistent practice

### Theme 4 — Inequality & risk exposure
- deprivation
- comorbidity
- ethnicity

### Theme 5 — System design
- coordination failures
- pathway complexity
- siloed departments

These themes help you build a severity model that is explainable, economic, and aligned with NHS Resolution’s strategy.

## 6. A concise NHS Resolution–specific takeaway

The book teaches that severe harm is not random — it is the economic output of strained, variable, information-poor systems.

Your severity model should therefore focus on upstream system behaviours, not just incident-level details.
