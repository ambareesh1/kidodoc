# kidodoc
Vision: A trustworthy, privacy-first pediatric assistant that unifies infant/kid health records (lab reports, DNA/genetic reports, vaccinations, growth), provides daily guidance and reminders, and lays the groundwork for responsible, explainable risk insights.

## Product requirements
### User access, authentication, and registration
- **Authentication**: Gmail OAuth plus phone OTP; phone verification is mandatory for all users.
- **Recommended flow (account linking)**:
  1. User taps **Continue with Google**.
  2. Backend verifies Google `id_token`.
  3. Create user with verified email, full name, and `AuthProvider=Google`.
  4. **Force add phone** → OTP verification.
  5. User can now sign in via Google (silent) or phone OTP without duplicate accounts.
- **Alternate flow (phone-first)**: phone OTP signup, then link Google later.
- **Account data**: name, gender, verified email, verified phone, DOB, location, language, and consent preferences.
- **Extended profile**: height/weight, allergies, known conditions, optional blood group, lifestyle flags (smoking/alcohol), and family history flags.
- **Family access links**: users can send secure, time-bound links to registered family members so they can upload documents and view summaries.

### Roles and permissions
- **Admin**: manages content library, policies, disclaimers, and audit logs; cannot access raw docs without explicit consent.
- **User**: manages family, uploads docs, views insights, and exports/deletes data.
- **Caregiver (optional)**: shared access for partner/grandparent/nanny with view/upload/edit/share scopes.

### Family structure
- **Partner linking**: invite by phone/email → partner approves → co-owner access to child profiles.
- **Children**: up to **4 children** per account; each child has profile, growth charts, immunizations, milestones, allergies, and conditions.
- **Family tree**: maternal/paternal grandparents + optional relatives for hereditary insights.
- **Family member fields**: relationship type, DOB/age range, blood group, allergies, conditions, key reports.

### Records and uploads
- **Supported inputs**: blood reports, scans, prescriptions, genetic reports, growth reports, and vaccination records.
- **Upload workflow**: drag-and-drop plus mobile camera capture.
- **Automated extraction**: models parse and normalize values from PDFs/images into structured fields.
- **Summaries**: each upload triggers an immediate summary with key findings and red flags.
- **Family-wide rollups**: combined analysis across all family members.
- **Document categories**: CBC, LFT, KFT, thyroid, HbA1c, lipid profile, vitamin D/B12, urine, CRP/ESR, genetic screening, imaging summaries, discharge summaries, prescriptions, and vaccination cards.
- **Document metadata**: patient member, document type, lab/hospital, test date, doctor (optional), tags, and extracted values.

### AI guidance and chatbot
- **Chatbot-first UX**: guided, conversational intake and navigation for non-technical users.
- **Explainable summaries**: each risk signal includes the supporting report values and dates.
- **Safety constraints**: no diagnosis, clear disclaimers, and escalation prompts for urgent findings.
- **Agents**:
  - Report summarizer (parent-friendly summaries).
  - Risk insights (family history + trends).
  - Next actions (vaccines, screenings, diet tips).
  - Reminders (vaccinations, supplements, checkups).

### UI/UX requirements
- **Frontend**: React-based web app.
- **Theme**: pink and sky-blue variations, accessible contrast, and friendly pediatric tone.
- **Ease of use**: minimal steps, large touch targets, and progressive disclosure for complex data.
- **Key UX flows**: family wizard, upload auto-detect + confirm, child timeline (growth + labs + vaccines), and one-click export.

### Child profile (pediatric additions)
- **Birth details**: gestational age, birth weight/length/head circumference, delivery type, NICU history.
- **Growth tracking**: monthly weight/height/HC, percentiles, milestones, sleep/feeding patterns.
- **Preventive care**: vaccination schedules, supplements (vitamin D/iron), visit reminders.
- **Risk flags**: prematurity, low birth weight, jaundice history, recurrent infections, congenital issues.

## Technical approach (high level)
### Data layer
- **Relational DB** for user, family, and clinical metadata.
- **Secure object storage** for files (PDFs, images).
- **Audit logs** for access, changes, and sharing events.
- **Core tables**: Users, Roles, UserRoles, FamilyMembers, Consents, Documents, DocumentTypes, LabResults, GrowthRecords, VaccinationRecords, Insights, AuditLogs.

### AI pipeline
1. **Ingestion**: OCR + document classification.
2. **Extraction**: structured lab/scans/prescription parsing.
3. **Normalization**: standard units and reference ranges.
4. **Summarization**: LLM-generated summaries with citations to extracted values.
5. **Family analysis**: aggregate trends and hereditary risk flags.
6. **RAG layer**: retrieval from a verified pediatric knowledge base for safe answers.

### Safety and compliance
- Consent-first data sharing.
- Role-based access controls and fine-grained permissions.
- Encryption at rest and in transit.
- No PHI in logs; short-lived presigned URLs for documents.

## MVP checklist
- Gmail + phone OTP auth
- Family profiles and children (limit 4)
- Upload and parsing for blood reports, scans, prescriptions
- Chatbot-guided onboarding and report intake
- Summaries for each upload plus family-level rollups
- Admin tools for policy, model prompts, and audits
