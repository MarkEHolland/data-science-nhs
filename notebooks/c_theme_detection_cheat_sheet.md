# Theme detection cheat sheet
## For NHS Resolution harm themes • Longformer models • debugging & error analysis

# 1. Overview
Use this cheat sheet when:
- reviewing model outputs
- doing error analysis
- refining training data
- checking borderline cases

Each theme has:
- key lexical cues
- typical sentences/phrases
- common confusions
- multi‑theme triggers

# 2. Diagnostic themes
## 2.1 Diagnostic error
Core idea: Wrong diagnosis, missed finding, misinterpretation.

Lexical cues:
- “missed” (fracture, bleed, lesion)
- “misdiagnosed”
- “incorrect diagnosis”
- “normal report” later found abnormal
- “not seen on X‑ray/CT/MRI”

Typical sentences:
- “Fracture was missed on initial X‑ray.”
- “Stroke was diagnosed as migraine.”
- “CT was reported as normal despite clear bleed.”

Common confusions:
- vs Diagnostic delay
- If diagnosis is wrong → diagnostic error
- If diagnosis is late but correct → diagnostic delay

## 2.2 Diagnostic delay
Core idea: Correct diagnosis, but too late.

Lexical cues:
- “delay in diagnosis”
- “diagnosis made the following day”
- “late recognition”
- “sepsis recognised late”
- “results not reviewed until…”

Typical sentences:
- “Sepsis was only recognised several hours later.”
- “CT results were not reviewed until the next morning.”
- “Cancer diagnosis delayed by several months.”

Common confusions:
- vs Delay in treatment
- If diagnosis is late → diagnostic delay
- If diagnosis is timely but treatment is late → delay in treatment

# 3. Escalation / treatment themes
## 3.1 Failure to escalate
Core idea: Deterioration not escalated to senior/appropriate level.

Lexical cues:
- “not escalated”
- “no senior review”
- “registrar/consultant not informed”
- “NEWS score not acted upon”
- “no rapid response call”

Typical sentences:
- “Despite NEWS 8, there was no escalation to senior review.”
- “Deterioration was not communicated to the registrar.”
- “Critical observations were not escalated.”
- Multi‑theme triggers:

Often co‑occurs with:
- diagnostic_delay
- delay_in_treatment


3.2 Delay in treatment
Core idea: Treatment started too late.

Lexical cues:
- “delay in starting antibiotics”
- “surgery delayed”
- “treatment commenced late”
- “late intervention”
- “waiting for theatre/bed”

Typical sentences:
- “Antibiotics were given four hours after sepsis was recognised.”
- “Surgery was delayed due to theatre availability.”
- “Treatment was postponed until the next day.”

Common confusions:
- vs Diagnostic delay
- Diagnosis late → diagnostic delay
- Diagnosis on time, treatment late → delay in treatment

# 4. Surgical / procedural themes
## 4.1 Surgical error
Core idea: Intra‑operative mistake.

Lexical cues:
- “wrong site surgery”
- “retained swab/instrument”
- “intra‑operative injury”
- “technical error”
- “incorrect procedure”

Typical sentences:
- “Wrong level spinal surgery was performed.”
- “A swab was retained in the abdomen.”
- “The ureter was injured during surgery.”

## 4.2 Post‑op complication
Core idea: Harm after surgery.

Lexical cues:
- “post‑operative bleed”
- “post‑op infection”
- “post‑op deterioration”
- “complication following surgery”
- “returned to theatre”

Typical sentences:
- “The patient developed a post‑operative haemorrhage.”
- “There was a post‑op wound infection.”
- “The patient deteriorated after surgery and required re‑operation.”

Common confusions:
- Intra‑op → surgical_error
- After surgery → post_op_complication

# 5. Medication theme
## 5.1 Medication error
Core idea: Wrong drug, dose, route, timing, or monitoring.

Lexical cues:
- “wrong dose”
- “wrong drug”
- “omitted medication”
- “not prescribed”
- “INR not monitored”
- “toxicity”

Typical sentences:
- “Warfarin dose was incorrectly increased.”
- “Insulin was omitted.”
- “Lithium levels were not monitored.”

# 6. Communication / handover theme
## 6.1 Communication failure
Core idea: Information not passed or documented.

Lexical cues:
- “poor handover”
- “information not communicated”
- “results not conveyed”
- “documentation incomplete”
- “notes missing”

Typical sentences:
- “Abnormal results were not communicated to the on‑call team.”
- “There was inadequate handover between shifts.”
- “Critical information was missing from the notes.”

Common confusions:
- vs Failure to escalate
- If pathway to senior review fails → failure_to_escalate
- If information itself not passed → communication_failure

# 7. System / organisational themes
## 7.1 Administrative delay
Core idea: System/scheduling/referral delays.

Lexical cues:
- “referral delayed”
- “appointment cancelled”
- “lost paperwork”
- “waiting list”
- “clinic backlog”

Typical sentences:
- “The referral was not sent for several weeks.”
- “The appointment was cancelled and not rebooked.”
- “Paperwork was lost, causing delay.”

## 7.2 Risk assessment failure
Core idea: No or inadequate risk assessment.

Lexical cues:
- “no falls assessment”
- “no VTE assessment”
- “risk not assessed”
- “no safeguarding assessment”
- “no deterioration risk assessment”

Typical sentences:
- “No VTE risk assessment was documented.”
- “Falls risk was not assessed despite multiple risk factors.”
- “There was no formal assessment of deterioration risk.”

# 8. Maternity / neonatal themes
## 8.1 Fetal monitoring failure
Core idea: CTG/fetal distress not recognised or misinterpreted.

Lexical cues:
- “CTG not reviewed”
- “CTG misinterpreted”
- “fetal distress not recognised”
- “non‑reassuring CTG ignored”

Typical sentences:
- “CTG abnormalities were not acted upon.”
- “Fetal distress was not recognised in time.”
- “CTG was incorrectly reported as normal.”

8.2 Delivery complication
Core idea: Harm during labour/delivery.

Lexical cues:
- “shoulder dystocia”
- “instrumental delivery”
- “forceps/ventouse complication”
- “maternal haemorrhage”
- “neonatal injury”

Typical sentences:
- “There was difficulty with shoulder delivery.”
- “Instrumental delivery resulted in trauma.”
- “Significant maternal haemorrhage occurred during delivery.”

# 9. Common confusion pairs (quick reference)
# Diagnostic error vs diagnostic delay
- Wrong diagnosis → error
- Late correct diagnosis → delay

## Failure to escalate vs communication failure
- Escalation pathway fails → failure_to_escalate
- Info not passed → communication_failure

## Surgical error vs post‑op complication
- During surgery → surgical_error
- After surgery → post_op_complication

## Diagnostic delay vs delay in treatment
- Late diagnosis → diagnostic_delay
- Diagnosis on time, treatment late → delay_in_treatment

10. Multi‑theme triggers
Mark a claim with multiple themes when you see chains like:
- “missed/late recognition” + “no escalation” + “late treatment”
- “CTG misinterpreted” + “delay in delivery” + “neonatal harm”
- “referral delayed” + “diagnosis delayed” + “deterioration”

In those cases, it’s usually correct to assign:
- diagnostic_delay
- failure_to_escalate
- delay_in_treatment
- plus any maternity/surgical/medication themes present.
