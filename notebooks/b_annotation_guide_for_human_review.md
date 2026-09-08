# Annotation Guide for Human Reviewers

## Clinical Harm Themes • NHS Resolution • Multi‑Theme Claims • Severity • Examples • Inclusion/Exclusion Rules

# 1. Purpose of This Guide

This guide ensures that human reviewers label clinical claims consistently, clinically correctly, and in a way that is machine‑learning friendly.

It defines:
- harm themes
- inclusion/exclusion criteria
- borderline cases
- multi‑theme rules
- severity rules
- maternity‑specific rules
- escalation‑failure indicators
- diagnostic‑error indicators
- annotation workflow

This guide is designed for:
- NHS Resolution claim analysis
- clinical NLP pipelines
- multi‑task transformers
- medico‑legal review
- training data creation

2. Harm Theme Definitions (12 Themes)
These are the core harm themes used in modelling.
Each theme includes:
- definition
- inclusion criteria
- exclusion criteria
- examples
- borderline cases

## 2.1 Diagnostic Error

Definition: Incorrect diagnosis, misinterpretation of tests, missed findings, wrong clinical conclusion.

Include when:
- imaging misread
- abnormal results not recognised
- incorrect diagnosis given
- cognitive bias (anchoring, premature closure)

Exclude when:
- diagnosis was correct but delayed → use Diagnostic Delay

Examples:
- “Fracture missed on X‑ray”
- “Stroke misdiagnosed as migraine”

## 2.2 Diagnostic Delay

Definition: Correct diagnosis eventually made, but later than clinically appropriate.

Include when:
- delayed imaging
- delayed review
- delayed test interpretation
- delayed referral

Exclude when:
- diagnosis was wrong → use Diagnostic Error

Examples:
- “Sepsis recognised 6 hours late”
- “CT ordered but not reviewed until next day”

## 2.3 Failure to Escalate

Definition: Failure to escalate deteriorating patient to senior review, specialist input, or higher level of care.

Include when:
- abnormal vitals not escalated
- NEWS score ignored
- failure to call registrar/consultant
- failure to transfer to HDU/ICU

Exclude when:
- escalation occurred but treatment was delayed → use Delay in Treatment

Examples:
- “NEWS 7 but no escalation”
- “Registrar not informed of deterioration”

2.4 Delay in Treatment
Definition: Treatment initiated too late.

Include when:
- delayed antibiotics
- delayed surgery
- delayed imaging
- delayed intervention

Exclude when:
- delay caused by diagnostic uncertainty → use Diagnostic Delay

Examples:
- “Antibiotics given 4 hours after sepsis recognition”
- “Surgery delayed due to theatre availability”

## 2.5 Surgical Error

Definition: Intra‑operative mistakes.

Include when:
- wrong site
- retained foreign object
- organ injury
- incorrect technique

Exclude when:
- post‑operative deterioration → use Post‑Op Complication

## 2.6 Post‑Op Complication
Definition:  Complications arising after surgery.

Include when:
- bleeding
- infection
- organ injury
- failure to monitor post‑op

Exclude when:
- intra‑operative error → use Surgical Error

## 2.7 Medication Error

Definition: Wrong drug, wrong dose, omission, incorrect monitoring.

Include when:
- prescribing error
- dispensing error
- administration error
- monitoring error (INR, lithium)

## 2.8 Communication Failure

Definition: Poor handover, missing information, failure to inform patient/family.

Include when:
- handover omissions
- failure to communicate abnormal results
- documentation gaps

2.9 Administrative Delay

Definition: Delays caused by scheduling, referral, or system processes.

Include when:
- delayed referral
- missed appointment
- lost paperwork
- system backlog

2.10 Risk Assessment Failure

Definition: Failure to assess risk (falls, VTE, deterioration, safeguarding).

Include when:
- no VTE assessment
- no falls assessment
- no deterioration risk assessment

## 2.11 Fetal Monitoring Failure

Definition: CTG misinterpretation or failure to recognise fetal distress.

Include when:
- CTG not reviewed
- CTG misinterpreted
- fetal distress missed

## 2.12 Delivery Complication

Definition: Complications during labour/delivery.

Include when:
- shoulder dystocia
- instrumental delivery issues
- maternal injury
- neonatal injury

# 3. Multi‑Theme Annotation Rules
## 3.1 Most claims have multiple themes

If multiple harm mechanisms contributed, label all applicable themes.

## 3.2 Do NOT force a single theme
Example:
- “CTG misinterpreted → delay in delivery → neonatal harm”

Correct labels:
- fetal_monitoring_failure
- delay_in_treatment
- delivery_complication

## 3.3 Use the “chain of harm” rule

Label each step in the chain:
1. failure to recognise
2. failure to escalate
3. delay in treatment
4. harm outcome

## 3.4 If unsure, include the theme

Better to include than exclude — ML models handle multi‑label well.


# 4. Severity Annotation Rules

## Severity 0 — Low
- no long‑term harm
- minor delay
- temporary symptoms

## Severity 1 — Moderate
- prolonged recovery
- significant deterioration
- avoidable complication

## Severity 2 — High
- permanent harm
- severe deterioration
- maternal/neonatal injury
- surgical catastrophe
- ICU admission

# 5. Borderline Cases (Critical Section)

Diagnostic Delay vs Delay in Treatment
- If diagnosis was late → diagnostic_delay
- If diagnosis was timely but treatment was late → delay_in_treatment

Failure to Escalate vs Communication Failure
- If escalation pathway failed → failure_to_escalate
- If information wasn’t passed → communication_failure

Surgical Error vs Post‑Op Complication
- Intra‑operative → surgical_error
- After surgery → post_op_complication


# 6. Annotation Workflow
- Read full claim narrative
- Identify harm chain
- Assign all relevant themes
- Assign severity
- Add notes for ambiguous cases
- Flag maternity cases for dual review
- Save annotation


# 7. Examples

## Example 1: “CTG misinterpreted. Delay in recognising fetal distress. Delivery delayed.”

Themes:
- fetal_monitoring_failure
- diagnostic_delay
- delay_in_treatment
- delivery_complication

Severity: High

## Example 2: “NEWS 8 but no escalation. Antibiotics delayed.”

Themes:
- failure_to_escalate
- delay_in_treatment

Severity: Moderate

## Example 3: “Fracture missed on X‑ray. Patient discharged.”

Themes:
- diagnostic_error
- communication_failure

Severity: Moderate

8. Final Notes
This guide ensures:
- consistent annotation
- clinically valid labels
- multi‑theme accuracy
- high‑quality training data
- medico‑legal defensibility
- transformer‑friendly structure
