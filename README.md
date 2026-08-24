# \# HAADS

# \*\*Hospital Adaptive Anomaly Detection System\*\*

# 

# \&gt; A sub-$50, fully offline, edge-AI early warning system for sepsis in low-resource hospital wards.

# 

# \---

# 

# \## The Problem

# 

# Sepsis kills \*\*11 million people every year\*\*  85% of them in low-resource settings like Pakistani government hospitals. In these wards, one nurse manages 20-30 patients. Vitals are checked manually every 4-6 hours. Sepsis can spiral from "looks like a fever" to organ failure in 3-4 hours.

# 

# \*\*Patients die in the gaps.\*\* No alarm fires. No one is watching.

# 

# Existing solutions (Epic, Cerner, commercial CDSS) cost $50,000–$200,000+ and require internet, EHR integration, and lab results. Pakistani government hospitals have paper charts, unreliable internet, closed labs at 2 AM, and zero budget.

# 

# \---

# 

# \## The Solution

# 

# HAADS is a bedside device that:

# 

# \- \*\*Monitors\*\* 3 vitals continuously (SpO2, Heart Rate, Temperature)

# \- \*\*Predicts\*\* sepsis risk using a lightweight quantized ML model running entirely on-device

# \- \*\*Alerts\*\* nurses with a simple Green/Yellow/Red system + protocol-aligned reminders

# \- \*\*Costs\*\* \~$30 per bed

# \- \*\*Requires\*\* no internet, no EHR, no lab results

# 

\### How It Works



===

# \---

# 

# \## System Architecture

# 

# | Layer | Component | Spec | Cost |

# |-------|-----------|------|------|

# | \*\*Sensing\*\* | MAX30102 | SpO2 + HR, I2C, finger clip | $3–5 |

# | | MLX90614 | Contactless temperature, I2C | $3–5 |

# | \*\*Compute\*\* | ESP32-S3-WROOM-1 | 512KB SRAM, WiFi, 240MHz dual-core | $8–10 |

# | \*\*Display\*\* | 0.96" OLED | 128×64, I2C, bedside vitals display | $3 |

# | \*\*Alert\*\* | Buzzer + RGB LED | Local audible/visual alarm | $1 |

# | \*\*Power\*\* | 18650 + TP4056 | 4-6 hour backup for load-shedding | $5 |

# | \*\*Enclosure\*\* | 3D printed PETG | IP54, wall-mountable, wipe-down cleanable | $2 |

# | | | \*\*Total BOM\*\* | \*\*\~$30\*\* |

# 

# \*\*Blood Pressure:\*\* Entered manually by nurse during rounds (gold standard). V2 will add automated oscillometric measurement with hard safety caps (max 180 mmHg, emergency deflate).

# 

# \---

# 

# \## The ML Model

# 

# \- \*\*Algorithm:\*\* Gradient Boosted Decision Trees (LightGBM/XGBoost)

# \- \*\*Training Data:\*\* MIMIC-IV ICU + ED records, filtered to 3 vitals only

# \- \*\*Features:\*\* 9 time-series features (mean, slope, variance over 6-hour window)

# \- \*\*Quantization:\*\* 8-bit integer thresholds via Treelite → C code

# \- \*\*On-device:\*\* 35KB model, <10ms inference, 0.89 AUROC target

# \- \*\*Output:\*\* Risk score thresholded to Green/Yellow/Red

# 

# \*\*Why this model:\*\* A 2022 JAMIA Open study achieved \*\*0.94 AUROC using only HR + Temperature\*\* with GBDT. We add SpO2, constrain to edge hardware, and validate in a real LMIC ward  a combination no prior work has attempted.

# 

# \---

# 

# \## Clinical Decision Support

# 

# HAADS does \*\*not\*\* diagnose or prescribe. At 🔴 Red alert, it surfaces protocol-aligned reminders:

# 

# \- Check Airway, Breathing, Circulation (ABCs)

# \- Call attending doctor immediately

# \- Prepare IV access

# \- If no response in 15 minutes: escalate to Medical Superintendent

# 

# This amplifies nurse awareness without replacing clinical judgment.

# 

# \---

# 

# \## Why This Hasn't Been Done

# 

# | Barrier | Why It Blocks Existing Solutions | How HAADS Solves It |

# |---------|----------------------------------|---------------------|

# | No continuous vitals in general wards | All ML models assume digitally available data | We build the hardware that creates the data |

# | No internet / EHR | Cloud AI and Epic integrations impossible | 100% offline edge inference |

# | No lab results at night | Sepsis models need lactate, WBC | Uses only 3 non-invasive vitals |

# | No budget | $100K+ systems impossible | $30 BOM, scalable to 50 beds for $1,500 |

# | No validation in LMICs | Academic models never leave the ICU | Pilot at Nishtar Hospital, Multan (1,800-bed government hospital) |

# 

# \---

# 

# \## Research Timeline

# 

# | Phase | Date | Deliverable |

# |-------|------|-------------|

# | Data \& Baseline | Sept–Dec 2026 | MIMIC-IV pipeline, feature engineering, baseline model |

# | Model Optimization | Jan–Mar 2027 | Quantized model, ESP32 deployment, bench testing |

# | FSc Board Exams | Apr 2027 |  |

# | Hardware Build | May–Jun 2027 | Working prototype, enclosure, power testing |

# | Technical Pilot | Jul 2027 | Device validation at Nishtar (empty beds/staff volunteers) |

# | Ward Pilot | Aug 2027 | Real-world validation (ethics permitting) |

# | ISEF Regionals | Sept 2027–Mar 2028 | Competition circuit |

# | ISEF 2028 | May 2028 | International presentation |

# 

# \---

# 

# \## Validation Plan

# 

# \*\*Phase 1  Model Accuracy\*\*

# \- Train on MIMIC-IV, validate with temporal downsampling (simulating 4-hour ward checks)

# \- Target: AUROC ≥0.88, Sensitivity ≥0.80, False Alarm Rate <2/shift

# 

# \*\*Phase 2  Hardware Reliability\*\*

# \- 72-hour continuous bench test

# \- Motion artifact detection, sensor fault handling, battery life validation

# 

# \*\*Phase 3  Hospital Pilot\*\*

# \- Nishtar Hospital, Multan (our local 1,800-bed government hospital)

# \- Measure: device uptime, nurse response time, alert correlation with clinical deterioration

# 

# \---

# 

# \## Who's Building This

# 

# Built by a high school student in Multan, Pakistan, with direct access to Nishtar Hospital  the exact environment this device is designed for. Prior published work on ML accessibility networks for Pakistan. Co-mentored by an active ML/control systems researcher.

# 

# \---

# 

# \## Funding Ask (Hack Club Highway)

# 

# We are requesting \*\*$250–350\*\* in parts credit for:

# 

# | Item | Cost | Qty | Total |

# |------|------|-----|-------|

# | ESP32-S3-WROOM-1 | $9 | 3 | $27 |

# | MAX30102 modules | $4 | 5 | $20 |

# | MLX90614 sensors | $4 | 5 | $20 |

# | OLED 0.96" displays | $3 | 3 | $9 |

# | Breadboards + jumper wires | $8 | 2 | $16 |

# | 18650 batteries + TP4056 boards | $5 | 4 | $20 |

# | MPS20N0040D pressure sensors (V2 BP) | $3 | 3 | $9 |

# | Mini air pumps + solenoid valves (V2) | $5 | 3 | $15 |

# | BP cuffs | $3 | 3 | $9 |

# | PETG filament for enclosures | $15 | 1 | $15 |

# | Miscellaneous (resistors, LEDs, MOSFETs, etc.) | $20 | 1 | $20 |

# | | | \*\*Total\*\* | \*\*\~$200\*\* |

# 

# Remaining funds for spare parts, shipping, and iteration.

# 

# \---

# 

# \## Why Hack Club

# 

# HAADS is hardware that matters. It's not a gadget  it's a device designed to catch sepsis 4 hours earlier in a government hospital ward where patients currently die silently between nurse rounds. Every hour earlier improves survival by \~7%. From a $30 device. Built by a student who can see the hospital from his house.

# 

# We need the parts to build it. Hack Club gets us there.

# 

# \---

# 

# \## Links

# 

# \- \*\*Research Concept Document:\*\* \[PDF](./docs/HAADS\_Concept\_Document.pdf)

# \- \*\*JOURNAL.md:\*\* \[Build log \& research notes](./JOURNAL.md)

# \- \*\*MIMIC-IV Data Pipeline:\*\* \[Coming soon](./src/)

# \- \*\*Hardware Schematics:\*\* \[Coming soon](./hardware/)

# 

# \---

# 

# \*HAADS is an investigational patient monitoring system for clinical research. It is designed to augment not replace routine nursing assessment and clinical judgment. Deployment on patients requires institutional ethics approval, informed consent, and physician oversight per local regulatory guidelines (DRAP, Pakistan). This repository documents the engineering and research methodology only.\*



