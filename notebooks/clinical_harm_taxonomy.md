# Clinical Harm Taxonomy Book

Summary
This book provides:
- a unified harm taxonomy
- clinically validated theme definitions
- extended PSIRF/NRLS/WHO categories
- hierarchical harm structure
- multi‑theme interaction modelling
- NLP mapping guidance
- multi‑label modelling foundations

It is designed to support:
- NHS Resolution claim analysis
- clinical NLP pipelines
- multi‑task transformers
- medico‑legal explainability
- long‑sequence modelling


## A Unified Reference for NHS Resolution, PSIRF, NRLS, WHO, and Clinical NLP Pipelines

# 1. Introduction
Clinical harm is rarely caused by a single failure.

Real NHS Resolution claims typically involve multiple interacting harm mechanisms, spanning:
- diagnostic processes
- escalation and deterioration
- treatment delays
- communication failures
- maternity complications
- surgical/procedural issues
- medication errors
- organisational/system factors

This book provides a unified taxonomy for modelling, annotating, and analysing harm in clinical narratives — designed for:
- clinical NLP
- multi‑task transformers
- multi‑label harm classification
- medico‑legal review
- NHS Resolution claim pipelines

# 2. Core Harm Theme Taxonomy (NHS Resolution Aligned)

These are the 12 clinically validated harm themes commonly used in NHS Resolution modelling. The following are 7 harm domains that contain the 12 harm themes.

## 2.1 Diagnostic‑Related Harm

| Theme | Description |
| --- | --- |
| **Diagnostic Error** | Incorrect diagnosis, misinterpretation of tests, missed findings. |
| **Diagnostic Delay** | Correct diagnosis eventually made, but delayed due to system or human factors. |

Examples:
- Missed fracture on X‑ray
- Sepsis not recognised early
- Stroke misdiagnosed as migraine

## 2.2 Treatment / Escalation / Deterioration

| Theme | Description |
| --- | --- |
| **Delay in Treatment** | Treatment initiated too late (antibiotics, surgery, imaging, interventions). |
| **Failure to Escalate** | Failure to escalate to senior review, specialist input, or higher level of care. |

Examples:
- Delayed antibiotics in sepsis
- Failure to escalate deteriorating vital signs

## 2.3 Procedural / Surgical Harm

| Theme | Description |
| --- | --- |
| **Surgical Error** | Intra‑operative mistakes, wrong site, retained foreign object. |
| **Post‑Op Complication** | Complications arising after surgery (bleeding, infection, organ injury). |

## 2.4 Medication‑Related Harm

| Theme | Description |
| --- | --- |
| **Medication Error** | Wrong drug, wrong dose, omission, incorrect monitoring (INR, lithium). |

## 2.5 Communication / Handover / Documentation

| Theme | Description |
| --- | --- |
| **Communication Failure** | Poor handover, missing information, failure to inform patient/family. |

## 2.6 Maternity / Neonatal Harm

| Theme | Description |
| --- | --- |
| **Fetal Monitoring Failure** | CTG misinterpretation, failure to recognise fetal distress. |
| **Delivery Complication** | Shoulder dystocia, instrumental delivery issues, maternal injury. |

## 2.7 Organisational / System Factors

| Theme | Description |
| --- | --- |
| **Administrative Delay** | Delayed referrals, missing appointments, scheduling failures. |
| **Risk Assessment Failure** | Failure to assess risk (falls, VTE, deterioration, safeguarding). |


# 3. Extended Clinical Harm Taxonomy (PSIRF + NRLS + WHO)

These categories expand the core 12 themes into a full clinical harm ontology.

## 3.1 Diagnostic Process Failures
- failure to take adequate history
- failure to perform appropriate examination
- failure to order appropriate tests
- failure to follow up abnormal results
- misinterpretation of imaging
- cognitive biases (anchoring, premature closure)

## 3.2 Escalation & Deterioration Failures
- failure to recognise deterioration
- failure to act on observations
- failure to escalate to senior review
- failure to activate rapid response
- inadequate monitoring

## 3.3 Treatment Delivery Failures
- delayed treatment
- incorrect treatment
- incomplete treatment
- failure to provide treatment
- procedural delays
- equipment unavailability

## 3.4 Communication & Coordination Failures
- poor handover
- missing documentation
- failure to communicate abnormal results
- lack of shared mental model
- inter‑departmental communication gaps

## 3.5 Medication Safety Failures
- prescribing error
- dispensing error
- administration error
- monitoring error
- omission

## 3.6 Surgical & Procedural Failures
- wrong site surgery
- retained foreign object
- intra‑operative injury
- anaesthetic complications
- post‑operative deterioration

## 3.7 Maternity & Neonatal Failures
- CTG misinterpretation
- delay in recognising fetal distress
- delay in delivery
- mismanagement of shoulder dystocia
- maternal haemorrhage
- neonatal resuscitation failures

## 3.8 System & Organisational Failures
- staffing shortages
- inadequate supervision
- poor workflow design
- IT system failures
- administrative delays
- policy non‑compliance

# 4. Multi‑Theme Harm Interactions (The Reality)

Most claims involve multiple interacting themes, not a single cause.

## 4.1 Example Interaction Chains

Diagnostic delay → failure to escalate → delay in treatment → deterioration

Fetal monitoring failure → communication failure → delay in delivery → neonatal harm

Administrative delay → diagnostic delay → deterioration → escalation failure

## 4.2 Why Causality Is Not Linear

Clinical harm is usually:

- multi‑factorial
- non‑linear
- interacting
- system‑level
- not reducible to a single cause

This is why multi‑label classification is clinically correct.


# 5. Hierarchical Harm Taxonomy

A clinically robust hierarchy:

## Level 1 — Domain
- Diagnostic
- Treatment
- Escalation
- Surgical
- Medication
- Communication
- Maternity
- System

## Level 2 — Theme

Your 12‑theme map.

## Level 3 — Sub‑Theme

Examples:
- diagnostic error → misinterpretation of imaging
- failure to escalate → failure to act on observations
- medication error → wrong dose
- fetal monitoring failure → CTG misinterpretation

## Level 4 — Contributory Factors
- staffing
- workload
- environment
- equipment
- cognitive bias
- communication gaps

# 6. Clinical NLP Mapping Guide
## 6.1 Token‑Level Indicators

Examples:
- “missed” → diagnostic error
- “delay” → delay in treatment
- “not escalated” → failure to escalate
- “CTG” → fetal monitoring failure
- “handover” → communication failure

## 6.2 Sentence‑Level Indicators

Use Captum IG or attention rollout to identify:
- deterioration patterns
- escalation failures
- diagnostic reasoning gaps
- treatment delays

# 7. Multi‑Label Modelling Guide
## 7.1 Why Multi‑Label Is Required

Claims often contain:

diagnostic_delay + failure_to_escalate + communication_failure


## 7.2 Recommended Architecture
- Clinical‑Longformer encoder
- Multi‑label sigmoid theme head
- Cross‑entropy severity head
- Weighted loss balancing

# Example Theme Map

{
  "diagnostic_error": 0,
  "diagnostic_delay": 1,
  "failure_to_escalate": 2,
  "delay_in_treatment": 3,
  "surgical_error": 4,
  "post_op_complication": 5,
  "medication_error": 6,
  "communication_failure": 7,
  "administrative_delay": 8,
  "risk_assessment_failure": 9,
  "fetal_monitoring_failure": 10,
  "delivery_complication": 11
}

