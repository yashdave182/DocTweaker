# 🔄 Document Tweaker - Visual Workflow Guide

## 📋 Complete System Workflow

```
┌─────────────────────────────────────────────────────────────────┐
│                         USER INTERACTION                        │
└────────────────────────┬────────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────────┐
│  Step 1: UPLOAD DOCUMENT                                        │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │  User Actions:                                            │ │
│  │  • Click "Choose File" button                             │ │
│  │  • Select PDF, Word, or TXT file                          │ │
│  │  • OR paste text directly into textarea                   │ │
│  │                                                            │ │
│  │  Files Supported:                                         │ │
│  │  ✓ .pdf  (Adobe PDF documents)                           │ │
│  │  ✓ .doc  (Microsoft Word 97-2003)                        │ │
│  │  ✓ .docx (Microsoft Word 2007+)                          │ │
│  │  ✓ .txt  (Plain text files)                              │ │
│  └───────────────────────────────────────────────────────────┘ │
└────────────────────────┬────────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────────┐
│  Step 2: PROVIDE CONTEXT                                        │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │  User describes enhancement goal:                         │ │
│  │                                                            │ │
│  │  Examples:                                                │ │
│  │  "Make this more professional for a business proposal"    │ │
│  │  "Improve grammar and clarity for an academic paper"      │ │
│  │  "Simplify language for a general audience"               │ │
│  │  "Add persuasive elements for marketing content"          │ │
│  │                                                            │ │
│  │  Tips:                                                    │ │
│  │  • Be specific about the goal                            │ │
│  │  • Mention target audience                               │ │
│  │  • Specify desired tone/style                            │ │
│  └───────────────────────────────────────────────────────────┘ │
└────────────────────────┬────────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────────┐
│  Step 3: CLICK "ENHANCE WITH AI"                                │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │  Button triggers the processing pipeline                 │ │
│  │  Progress bar: 0% ──────────────────────────────► 100%   │ │
│  └───────────────────────────────────────────────────────────┘ │
└────────────────────────┬────────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────────┐
│                    BACKEND PROCESSING                           │
└─────────────────────────────────────────────────────────────────┘

        ┌──────────────────────────────────────────────┐
        │  Stage 1: UPLOAD (0% → 20%)                  │
        │  ┌────────────────────────────────────────┐  │
        │  │  • File received by Flask server       │  │
        │  │  • File validated (type, size)         │  │
        │  │  • Saved to uploads/ directory         │  │
        │  │  • Unique document_id generated        │  │
        │  └────────────────────────────────────────┘  │
        └──────────────────┬───────────────────────────┘
                           │
                           ▼
        ┌──────────────────────────────────────────────┐
        │  Stage 2: OCR EXTRACTION (20% → 40%)         │
        │  ┌────────────────────────────────────────┐  │
        │  │  Unstract LLMWhisperer API Call:      │  │
        │  │                                        │  │
        │  │  1. Submit document                    │  │
        │  │     POST /whisper → whisper_hash       │  │
        │  │                                        │  │
        │  │  2. Poll for status (every 2 seconds)  │  │
        │  │     GET /whisper-status                │  │
        │  │     Status: processing → processed     │  │
        │  │                                        │  │
        │  │  3. Retrieve extracted text            │  │
        │  │     GET /whisper-retrieve              │  │
        │  │                                        │  │
        │  │  Features:                             │  │
        │  │  ✓ Layout-preserving extraction        │  │
        │  │  ✓ Line number tracking                │  │
        │  │  ✓ Table/form detection                │  │
        │  │  ✓ Formatting markers preserved        │  │
        │  └────────────────────────────────────────┘  │
        └──────────────────┬───────────────────────────┘
                           │
                           ▼
        ┌──────────────────────────────────────────────┐
        │  Stage 3: FORMAT EXTRACTION (40% → 50%)      │
        │  ┌────────────────────────────────────────┐  │
        │  │  DocumentProcessor.extract()           │  │
        │  │                                        │  │
        │  │  Analyzes document structure:          │  │
        │  │  • Identify headings (H1-H6)          │  │
        │  │  • Detect paragraphs                   │  │
        │  │  • Find lists (bullet/numbered)        │  │
        │  │  • Locate tables                       │  │
        │  │                                        │  │
        │  │  Extract formatting:                   │  │
        │  │  • Bold, italic, underline             │  │
        │  │  • Font family & size                  │  │
        │  │  • Colors & backgrounds                │  │
        │  │  • Alignment & spacing                 │  │
        │  │  • Margins & indentation               │  │
        │  │                                        │  │
        │  │  Create template:                      │  │
        │  │  {                                     │  │
        │  │    blocks: [                           │  │
        │  │      { text, block_format,             │  │
        │  │        char_formats }                  │  │
        │  │    ],                                  │  │
        │  │    metadata: {...}                     │  │
        │  │  }                                     │  │
        │  └────────────────────────────────────────┘  │
        └──────────────────┬───────────────────────────┘
                           │
                           ▼
        ┌──────────────────────────────────────────────┐
        │  Stage 4: AI ENHANCEMENT (50% → 70%)         │
        │  ┌────────────────────────────────────────┐  │
        │  │  Google Gemini API Call:               │  │
        │  │                                        │  │
        │  │  Model: gemini-1.5-flash              │  │
        │  │  Temperature: 0.3                      │  │
        │  │  Max Tokens: 8192                      │  │
        │  │                                        │  │
        │  │  System Prompt:                        │  │
        │  │  "You are an expert document enhancer. │  │
        │  │   PRESERVE all formatting markers.     │  │
        │  │   MAINTAIN document structure.         │  │
        │  │   ENHANCE text content based on        │  │
        │  │   user's specific context."            │  │
        │  │                                        │  │
        │  │  User Prompt:                          │  │
        │  │  "Context: {user_context}              │  │
        │  │   Original: {document_text}            │  │
        │  │   Return enhanced version only."       │  │
        │  │                                        │  │
        │  │  AI Processing:                        │  │
        │  │  ✓ Understands context                 │  │
        │  │  ✓ Improves grammar/clarity            │  │
        │  │  ✓ Enhances tone/style                 │  │
        │  │  ✓ Preserves format markers            │  │
        │  │  ✓ Maintains structure                 │  │
        │  └────────────────────────────────────────┘  │
        └──────────────────┬───────────────────────────┘
                           │
                           ▼
        ┌──────────────────────────────────────────────┐
        │  Stage 5: FORMAT REAPPLICATION (70% → 90%)   │
        │  ┌────────────────────────────────────────┐  │
        │  │  DocumentProcessor.apply_format()      │  │
        │  │                                        │  │
        │  │  Smart Format Mapping:                 │  │
        │  │                                        │  │
        │  │  1. Split enhanced text into blocks    │  │
        │  │     • Paragraph boundaries             │  │
        │  │     • Section breaks                   │  │
        │  │     • List items                       │  │
        │  │                                        │  │
        │  │  2. Match blocks to template           │  │
        │  │     • Semantic matching                │  │
        │  │     • Structure preservation           │  │
        │  │     • Format inheritance               │  │
        │  │                                        │  │
        │  │  3. Apply character formats            │  │
        │  │     • Proportional mapping             │  │
        │  │     • Format span calculation          │  │
        │  │     • Style application                │  │
        │  │                                        │  │
        │  │  Result:                               │  │
        │  │  Enhanced text with original           │  │
        │  │  formatting perfectly preserved!       │  │
        │  └────────────────────────────────────────┘  │
        └──────────────────┬───────────────────────────┘
                           │
                           ▼
        ┌──────────────────────────────────────────────┐
        │  Stage 6: SAVE & PREPARE (90% → 100%)        │
        │  ┌────────────────────────────────────────┐  │
        │  │  • Save format template as JSON        │  │
        │  │  • Save enhanced result                │  │
        │  │  • Generate document_id for retrieval  │  │
        │  │  • Cleanup temporary files             │  │
        │  │  • Return success response             │  │
        │  └────────────────────────────────────────┘  │
        └──────────────────┬───────────────────────────┘
                           │
                           ▼
┌─────────────────────────────────────────────────────────────────┐
│                    FRONTEND DISPLAY                             │
└─────────────────────────────────────────────────────────────────┘

        ┌──────────────────────────────────────────────┐
        │  Results Display                             │
        │  ┌────────────────────────────────────────┐  │
        │  │  Tab View:                             │  │
        │  │  ┌─────────────┬──────────────────┐   │  │
        │  │  │  ORIGINAL   │    ENHANCED      │   │  │
        │  │  └─────────────┴──────────────────┘   │  │
        │  │                                        │  │
        │  │  Original Tab:                         │  │
        │  │  ┌────────────────────────────────┐   │  │
        │  │  │ [Extracted text from OCR]      │   │  │
        │  │  │                                │   │  │
        │  │  │ Shows what Unstract extracted  │   │  │
        │  │  │ from your document             │   │  │
        │  │  └────────────────────────────────┘   │  │
        │  │                                        │  │
        │  │  Enhanced Tab (Default):               │  │
        │  │  ┌────────────────────────────────┐   │  │
        │  │  │ [AI-enhanced version]          │   │  │
        │  │  │                                │   │  │
        │  │  │ Improved content with          │   │  │
        │  │  │ preserved formatting!          │   │  │
        │  │  └────────────────────────────────┘   │  │
        │  │                                        │  │
        │  │  Action Buttons:                       │  │
        │  │  ┌──────┐ ┌──────────┐               │  │
        │  │  │ Copy │ │ Download │               │  │
        │  │  └──────┘ └──────────┘               │  │
        │  └────────────────────────────────────────┘  │
        └──────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────┐
│                    USER ACTIONS                                 │
└─────────────────────────────────────────────────────────────────┘

        ┌──────────────────────────────────────────────┐
        │  Copy to Clipboard                           │
        │  • Click "Copy" button                       │
        │  • Enhanced text copied                      │
        │  • Paste anywhere (Word, Email, etc.)        │
        └──────────────────────────────────────────────┘

        ┌──────────────────────────────────────────────┐
        │  Download as File                            │
        │  • Click "Download" button                   │
        │  • Saves as .txt file                        │
        │  • Filename: enhanced_document.txt           │
        └──────────────────────────────────────────────┘

        ┌──────────────────────────────────────────────┐
        │  Process Another Document                    │
        │  • Click "Reset" button                      │
        │  • Clear all fields                          │
        │  • Start fresh                               │
        └──────────────────────────────────────────────┘


═══════════════════════════════════════════════════════════════════
                      DATA FLOW DIAGRAM
═══════════════════════════════════════════════════════════════════

User Browser                Backend Server             External APIs
     │                           │                           │
     │  1. POST /api/process     │                           │
     │  (file + context)         │                           │
     ├──────────────────────────>│                           │
     │                           │                           │
     │                           │  2. POST /whisper         │
     │                           │  (document binary)        │
     │                           ├──────────────────────────>│
     │                           │                     Unstract API
     │                           │  3. whisper_hash          │
     │                           │<──────────────────────────┤
     │                           │                           │
     │                           │  4. Poll /whisper-status  │
     │                           │  (every 2 seconds)        │
     │                           ├──────────────────────────>│
     │                           │  Status: processing       │
     │                           │<──────────────────────────┤
     │                           │         ...               │
     │                           │  Status: processed        │
     │                           │<──────────────────────────┤
     │                           │                           │
     │                           │  5. GET /whisper-retrieve │
     │                           ├──────────────────────────>│
     │                           │  extracted_text           │
     │                           │<──────────────────────────┤
     │                           │                           │
     │                           │  6. Format extraction     │
     │                           │  (internal processing)    │
     │                           │                           │
     │                           │  7. POST chat.completions │
     │                           │  (text + context)         │
     │                           ├──────────────────────────>│
     │                           │                     Gemini API
     │                           │  8. enhanced_text         │
     │                           │<──────────────────────────┤
     │                           │                           │
     │                           │  9. Format reapplication  │
     │                           │  (internal processing)    │
     │                           │                           │
     │  10. Response:            │                           │
     │  {                        │                           │
     │    original_text,         │                           │
     │    enhanced_text,         │                           │
     │    document_id            │                           │
     │  }                        │                           │
     │<──────────────────────────┤                           │
     │                           │                           │
     │  11. Display results      │                           │
     │                           │                           │


═══════════════════════════════════════════════════════════════════
                    PROCESSING TIMELINE
═══════════════════════════════════════════════════════════════════

Time    Stage                   Status                  Progress
────────────────────────────────────────────────────────────────────
0s      Upload                  Starting...             0%
1s      Upload                  File validated          10%
2s      Upload                  Saved to server         20%
────────────────────────────────────────────────────────────────────
3s      OCR                     Submitted to Unstract   25%
5s      OCR                     Processing...           30%
10s     OCR                     Still processing...     35%
15s     OCR                     Text extracted!         40%
────────────────────────────────────────────────────────────────────
16s     Format Extract          Analyzing structure     45%
18s     Format Extract          Template created        50%
────────────────────────────────────────────────────────────────────
20s     AI Enhancement          Sent to Gemini          55%
25s     AI Enhancement          Generating...           60%
30s     AI Enhancement          Complete!               70%
────────────────────────────────────────────────────────────────────
31s     Format Apply            Mapping blocks          75%
33s     Format Apply            Applying styles         80%
35s     Format Apply            Finalizing...           85%
────────────────────────────────────────────────────────────────────
36s     Save                    Saving results          90%
37s     Complete                Ready!                  100%
────────────────────────────────────────────────────────────────────

Total Time: ~37 seconds (varies by document size and complexity)


═══════════════════════════════════════════════════════════════════
                    ERROR HANDLING FLOW
═══════════════════════════════════════════════════════════════════

        ┌──────────────────────────────────────────────┐
        │  Error Detection Points                      │
        └──────────────────────────────────────────────┘

[1] File Upload Errors
    ├─ File too large (>15MB)
    │  → "File size must be less than 15MB"
    ├─ Invalid file type
    │  → "Please upload .pdf, .doc, .docx, or .txt"
    └─ Corrupted file
       → "File appears to be corrupted"

[2] OCR Errors
    ├─ Unstract API key invalid
    │  → "Unstract API key not configured"
    ├─ Processing timeout
    │  → "OCR processing timeout - try smaller document"
    ├─ No text extracted
    │  → "No readable text found - may be image-based PDF"
    └─ API quota exceeded
       → "API quota exceeded - check Unstract dashboard"

[3] AI Enhancement Errors
    ├─ Gemini API key invalid
    │  → "Gemini API key not configured"
    ├─ Context too long
    │  → "Text too long - try shorter document"
    ├─ Rate limit hit
    │  → "Rate limit exceeded - try again in a moment"
    └─ Generation failed
       → "Enhancement failed - please try again"

[4] Format Errors
    ├─ Template creation failed
    │  → "Failed to extract format template"
    └─ Format application failed
       → "Format preservation failed - using plain text"

        ┌──────────────────────────────────────────────┐
        │  Error Recovery                              │
        └──────────────────────────────────────────────┘

All errors are:
✓ Logged to console (detailed info)
✓ Shown to user (friendly message)
✓ Recoverable (user can retry)
✓ Non-blocking (partial success possible)


═══════════════════════════════════════════════════════════════════
                    FILE STORAGE STRUCTURE
═══════════════════════════════════════════════════════════════════

doc_tweak-main/
│
├── uploads/                     # Temporary uploaded files
│   ├── abc123.pdf              # Original document
│   ├── def456.docx             # Original document
│   └── ...
│
├── processed/                   # Processing results
│   ├── abc123_template.json    # Format template
│   ├── abc123_result.json      # Enhanced with format
│   ├── def456_template.json
│   ├── def456_result.json
│   └── ...
│
└── ...

File Lifecycle:
1. Upload → saved to uploads/
2. Process → template & result saved to processed/
3. Download → read from processed/
4. Cleanup → files can be deleted after download

Note: Files are kept for potential re-downloads. Implement
automatic cleanup for production (e.g., delete after 24 hours).


═══════════════════════════════════════════════════════════════════
                    CONFIGURATION FLOW
═══════════════════════════════════════════════════════════════════

[.env File] ──────> [Backend Reads on Startup]
    │                         │
    │                         ├─> UNSTRACT_API_KEY
    │                         │   ├─> Configure UnstractOCRService
    │                         │   └─> Enable OCR processing
    │                         │
    │                         └─> GEMINI_API_KEY
    │                             ├─> Configure GeminiEnhancerService  
    │                             └─> Enable AI enhancement
    │
    └─> [Frontend Reads .env.local]
            │
            └─> VITE_API_URL
                └─> Configure API client


Health Check Response:
{
  "status": "healthy",
  "unstract_configured": true,  ← Has valid API key
  "gemini_configured": true,    ← Has valid API key
  "timestamp": "2024-01-15T10:30:00Z"
}


═══════════════════════════════════════════════════════════════════
                    DEPLOYMENT CHECKLIST
═══════════════════════════════════════════════════════════════════

□ Development Setup
  □ Python 3.9+ installed
  □ Node.js 18+ installed
  □ Virtual environment created
  □ Dependencies installed (pip install -r requirements.txt)
  □ Node modules installed (npm install)

□ Configuration
  □ .env file created from .env.example
  □ UNSTRACT_API_KEY configured
  □ GEMINI_API_KEY configured
  □ .env.local created (optional)

□ Testing
  □ Run test_setup.py (python test_setup.py)
  □ All 8 tests pass
  □ Backend health check returns "healthy"
  □ Test document processes successfully

□ Running
  □ Backend server started (python backend_api.py)
  □ Frontend server started (npm run dev)
  □ Application accessible at localhost:5173

□ Production (Additional)
  □ Use production WSGI server (gunicorn)
  □ Set up HTTPS
  □ Configure environment variables (not .env file)
  □ Set up monitoring
  □ Implement rate limiting
  □ Configure backup/logging
  □ Set up automatic file cleanup


═══════════════════════════════════════════════════════════════════
                    QUICK REFERENCE
═══════════════════════════════════════════════════════════════════

Start Application:
  Windows: double-click start.bat
  Manual:  python backend_api.py (terminal 1)
           npm run dev (terminal 2)

URLs:
  Frontend:  http://localhost:5173/enhanced-doc-tweaker
  Backend:   http://localhost:5000
  Health:    https://doctweaker.vercel.app/health

Test Setup:
  python test_setup.py

Common Commands:
  Install Python deps:  pip install -r requirements.txt
  Install Node deps:    npm install
  Build frontend:       npm run build
  Lint code:           npm run lint

API Keys:
  Unstract: https://unstract.com/
  Gemini:   https://makersuite.google.com/app/apikey

Documentation:
  QUICK_START.md       - 5-minute setup
  SETUP_GUIDE.md       - Detailed installation
  README_NEW.md        - Complete documentation
  IMPLEMENTATION_SUMMARY.md - Technical overview


═══════════════════════════════════════════════════════════════════

                    🎉 You're All Set! 🎉

              Your document enhancement system is ready!

═══════════════════════════════════════════════════════════════════