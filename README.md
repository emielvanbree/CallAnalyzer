# Service Call Analyzer

**Service Call Analyzer** is a single-page, front-end web application that runs entirely in the browser using **JavaScript, HTML, and CSS**.  
It automates the **quality assurance process for customer service calls** by recording, uploading, or pasting call transcripts, then generating a detailed **Service Call Scorecard** using AI models.

---

## 🎯 Goal of the Application

The application helps users streamline QA for service calls by:
1. Recording a service call directly via microphone.
2. Uploading a pre-recorded WAV file.
3. Manually pasting a call transcript.

The result is a **Service Call Scorecard** with:
- Scored analysis based on predefined metrics.  
- A call summary.  
- Recommendations and learning goals.  

---

## ⚙️ Functional Breakdown

### Audio Recording
- `startBtn.addEventListener`: Captures audio using `MediaRecorder`.
- `stopBtn.addEventListener`: Stops recording and saves audio as a `Blob`.

### File Handling
- `selectFileBtn.addEventListener`: Opens hidden file input for WAV upload.  
- `audioFileInput.addEventListener`: Validates file format and stores as `Blob`.

### Transcription & Analysis
- `uploadBtn.addEventListener`: Runs transcription pipeline:  
  `uploadToAssemblyAI → requestTranscription → pollTranscript`.  
- `analyzeManualBtn.addEventListener`: Processes pasted transcripts via:  
  `anonymizePII → analyzeWithOpenAI`.

### UI Management
- `clearBtn.addEventListener`: Resets fields, outputs, and buttons.  
- `downloadWordBtn.addEventListener`: Exports scorecard + transcript as `.docx` using a dynamic docx library.

### Data Anonymization
- `anonymizePII(text)`: Uses regex to replace PII (emails, phone numbers, credentials) with `[REDACTED: ...]`.  
  Ensures GDPR compliance before sending data to APIs.

---

## 🔗 API Integrations

The application interacts directly with **AssemblyAI** and **OpenAI** APIs.

| API Endpoint | Purpose | Method | Body | Return |
|--------------|---------|--------|------|--------|
| `https://api.assemblyai.com/v2/upload` | Upload audio for transcription | POST | Audio `Blob` | JSON with `upload_url` |
| `https://api.assemblyai.com/v2/transcript` | Start transcription job | POST | `{ audio_url }` | JSON with job `id` |
| `https://api.assemblyai.com/v2/transcript/{id}` | Poll transcription status | GET | None | JSON with status + transcript |
| `https://api.openai.com/v1/chat/completions` | Generate scorecard | POST | `{ model, prompt, transcript }` | JSON with AI-generated HTML scorecard |

---

## 💰 Possible Usage Costs

This app relies on **pay-as-you-go APIs**.

### AssemblyAI (Transcription)
- Universal Tier: **$0.27/hr** of audio.  
- Nano Tier: **$0.12/hr** of audio.  
- Free tier available (up to ~185 hrs of audio).

### OpenAI (Scorecard Generation)
- Using `gpt-4o-mini`.  
- Input Tokens: **$0.15 / 1M tokens**.  
- Output Tokens: **$0.60 / 1M tokens**.  
- ~1 token ≈ 4 characters of text.  

💡 **Actual cost depends on transcript + scorecard length.**

---

## 🛡 GDPR Compliance

The Service Call Analyzer is built with **data privacy** in mind:

1. **Client-Side Anonymization**  
   - `anonymizePII()` strips PII before sending data to APIs.  

2. **Explicit AI Instructions**  
   - OpenAI prompt enforces final PII check and replacement with `[ANONYMIZED DATA]`.  

3. **Data Minimization**  
   - Only transcripts are sent to APIs.  
   - No audio or transcripts stored on any backend server.  

4. **Transparency Statement**  
   - Footer disclaimer:  
     *“All call transcripts and scorecards are automatically anonymized in accordance with GDPR standards and regulations.”*

---

## 🚀 Tech Stack

- **Frontend Only** (no backend server).  
- **Languages:** JavaScript, HTML, CSS.  
- **External APIs:** [AssemblyAI](https://www.assemblyai.com/) + [OpenAI](https://platform.openai.com/).  

---

## 📦 Installation & Usage

1. Clone repository:  
   ```bash
   git clone https://github.com/your-username/service-call-analyzer.git
   cd service-call-analyzer
   ```

2. Open `index.html` in a browser.  
3. Ensure you have API keys for:
   - **AssemblyAI**
   - **OpenAI**

4. Use the app to:
   - Record or upload calls.  
   - Paste transcripts.  
   - Generate scorecards.  

---

## 📜 License

This project is released under the [MIT License](LICENSE).
