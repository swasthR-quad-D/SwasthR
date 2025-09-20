# SwasthR

**AI-powered medication "spell-checker" — scan pill labels, detect dangerous drug interactions, and get clear, plain-language warnings.**

---

## 🚩 One-line pitch

SwasthR helps patients and caregivers avoid harmful drug interactions by scanning medication labels with your phone camera and instantly showing plain‑language, color‑coded warnings — like a spell‑checker for medicines.

---

## Problem

* 1 in 5 patients experience harmful drug interactions.
* Causes: patients forget to check, clinicians miss complex combinations, and labels use hard-to-understand medical jargon.
* Example: Warfarin (blood thinner) + Aspirin → increased risk of severe/internal bleeding.

---

## Solution

SwasthR is a mobile-first app that:

1. Uses OCR to read medication labels from a photo (or lets users search manually).
2. Checks interactions using trusted drug databases (RxNorm / DrugBank).
3. Displays immediate, color-coded warnings with simple explanations.
4. Offers accessibility features (voice input, large buttons) and clinician alerts for critical risks.

---

## Key Features

* 📷 **Label scanning:** Take a photo of a pill bottle or prescription label; OCR extracts drug name / NDC codes.
* 🔎 **Manual lookup:** Search by drug name if the label is unreadable.
* 🔴🟠🟢 **Color-coded interaction levels:** Red = high risk (contact doctor), Orange = moderate, Green = low/no interaction.
* 🧾 **Plain-language explanations:** "These pills thin your blood too much — talk to your doctor."
* 📣 **Doctor alerts:** Send SMS/email for critical interactions (Twilio / SMTP).
* 🗣️ **Voice UX:** Ask questions like "Can I take Tylenol with Advil?"
* 📈 **Trend tracking (optional):** Track sedating meds, blood thinners, adherence signals.

---

## Tech Stack

**Frontend**

* Mobile: Flutter (cross-platform camera UI)
* Web: Next.js (admin/dashboard)

**Backend**

* API: Python + FastAPI
* Database: Firebase Firestore (or PostgreSQL)
* Auth: Firebase Auth

**AI / OCR / Data**

* OCR: Google Cloud Vision API
* Drug interactions: RxNorm (free) / DrugBank (paid)

**Integrations**

* SMS / Notifications: Twilio
* EHR integration (future): FHIR API

**Security & Compliance**

* HIPAA-aware architecture: end-to-end encryption at rest & in transit, minimal PHI stored, anonymized Firebase IDs.

---

## Architecture (high level)

```
[Mobile App (Flutter)] --image--> [OCR (Vision API)]
         |                                 |
         |---> [Backend (FastAPI)] ---lookup---> [Drug DB: RxNorm / DrugBank]
                             |
                             |---> [Cache / Local DB for frequent lookups]
                             |
                             |---> [Notifications: Twilio / Email]
                             |
                             '---> [Analytics / Firestore]
```

---

## Quickstart — Local Development

> These are example steps for running the backend and a simple frontend mock.

### Prerequisites

* Python 3.10+
* Node.js 18+ (for Next.js demo)
* Flutter SDK (for mobile development)
* Google Cloud account + Vision API enabled
* Firebase project (Auth + Firestore)
* (Optional) DrugBank API key or access to RxNorm dataset
* Twilio account (for SMS alerts)

### Backend (FastAPI)

```bash
# clone repository
git clone https://github.com/<your-org>/swasthr.git
cd swasthr/backend

# create virtual env
python -m venv .venv
source .venv/bin/activate  # Windows: .venv\Scripts\activate

pip install -r requirements.txt

# .env example (create .env in backend/)
# GOOGLE_APPLICATION_CREDENTIALS=/path/to/gcloud-creds.json
# FIREBASE_PROJECT_ID=your-firebase-project
# DRUGBANK_API_KEY=your-drugbank-key
# TWILIO_SID=ACxxxx
# TWILIO_AUTH_TOKEN=yyyy
# TWILIO_FROM=+1234567890

uvicorn main:app --reload --host 0.0.0.0 --port 8000
```

### Frontend (Next.js demo)

```bash
cd ../web
npm install
npm run dev
# Open http://localhost:3000
```

### Flutter (mobile prototype)

```bash
cd ../mobile
flutter pub get
flutter run
```

---

## Example API endpoints

**POST /scan**
Request: multipart/form-data with `image` and `user_id`.
Response: `{ drugs: ["Warfarin"], ndc: ["..."], ocr_confidence: 0.92 }`

**POST /interactions**
Request: `{ "drugs": ["Warfarin", "Aspirin"] }`
Response: `{ "interaction_level": "High", "message": "May increase bleeding", "explain_like_im_5": "These pills thin your blood too much." }`

**POST /notify\_doctor**
Request: `{ "user_id": "anon-xyz", "message": "High risk interaction: ..." }`

---

## UX / Accessibility considerations

* Large tappable buttons, high-contrast UI, and an "Explain Like I’m 5" toggle for plain-language summaries.
* Voice input and text-to-speech for visually impaired or low‑literacy users.
* Local caching and offline mode for slow networks.

---

## Privacy & HIPAA notes

* Minimize PHI: store only anonymized identifiers and non-identifying medication lists when possible.
* Encrypt data at rest and in transit.
* Provide clear consent screens before sharing notifications with clinicians.
* In production deployments, consult a legal/compliance expert and host in a HIPAA-compliant environment.

---

## Challenges & Mitigations

* **OCR errors:** use Google Vision + manual lookup fallback + allow NDC code entry.
* **Latency from external APIs:** cache common interactions and provide graceful degraded UX.
* **False positives / medical liability:** include disclaimers, provide evidence links, and require clinician confirmation for critical actions.

---

## Roadmap / Future Upgrades

* AR overlay: point phone at pill blister → see interaction badges.
* EHR/FHIR integration to auto-import medication lists.
* Community side-effect reporting (crowdsourced).
* ML model to predict individual risk combining vitals, age, and comorbidities (research phase).

---

## Hackathon Pitch (30–60s)

1. Problem: "1 in 5 patients face harmful drug interactions because labels are confusing and combinations are complex."
2. Product: "SwasthR — the medication spell-checker: scan labels, get instant, plain-language warnings."
3. Demo flow: Scan Warfarin bottle → add Aspirin → app shows a red warning: ‘High risk: may cause severe bleeding’ → Option: Notify My Doctor.
4. Impact: Reduces ER visits, increases medication safety for elderly and polypharmacy patients.

---

## Contributing

Contributions welcome! Please open issues or pull requests. Use the following process:

1. Fork repo
2. Create a branch `feature/your-feature`
3. Add tests and documentation
4. Open a PR with a clear description

---

## License

MIT License — see `LICENSE` file.

---

## Contact

Project lead: Your Name — [email@example.com](mailto:email@example.com)
GitHub: `https://github.com/<your-org>/swasthr`

---

*Made with ❤️ for the "AI‑Powered Healthcare Assistant" hackathon theme.*
