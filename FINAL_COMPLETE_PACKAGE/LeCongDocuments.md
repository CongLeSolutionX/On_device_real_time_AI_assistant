
---

## 1. **Semantic Review: Key Components and Flow**

### **A. Architectural Overview**

- **`AICardView`** is a SwiftUI View that serves as a chat interface for asking questions *by voice* in Vietnamese, and then relaying the response from a local LLM (Large Language Model).
- **Voice functionality** is two-fold:
    - **Speech-to-Text**: Converts user's spoken Vietnamese question to text via **Speech framework** (SFSpeechRecognizer & AVAudioEngine).
    - **Text-to-Speech**: Reads out the AI's answer aloud using **AVSpeechSynthesizer**.
- The **AI logic** uses a local model (TinyLlama/Arcee-VyLinh, quantized) loaded via a `LLM` abstraction and queried asynchronously.
- Extensive **state management** ensures:
    - User's question and AI's answer are displayed and voiced,
    - Loading, error, and permission states are clearly presented to the user.
- **Permissions** for both speech recognition and microphone usage are requested and handled.
- **Error handling** is structured and user-facing, using custom error types (`AIError`) with localized descriptions.

### **B. Core Functional Flow**

1. **User taps mic button**:
    - If not recording:
        - Microphone and speech permissions checked/requested.
        - Recording starts and transcribes user's question in Vietnamese.
    - If recording:
        - Recording stops, final transcription is produced.
2. **On final transcription**:
    - The transcribed question is sent asynchronously to the local LLM.
    - Spinner/progress shows during processing.
    - AI's response is displayed AND spoken aloud via TTS.
3. **Error management**:
    - At any stage (permission denied, recording issues, LLM errors), clear error messages appear in the card.

---

## 2. **Visual Explanations (Mermaid Diagrams)**

---

### **A. Top-Level App Architecture**

```mermaid
flowchart TD
    AICardView["AICardView<br/>(SwiftUI Card)"]
    subgraph VoiceInteraction
        SR["Speech-to-Text<br/>(SFSpeechRecognizer, AVAudioEngine)"]
        TTS["Text-to-Speech<br/>(AVSpeechSynthesizer)"]
    end
    AI["Local LLM<br/>(TinyLlama / Arcee-VyLinh via LLM API)"]
    User["User<br/>(Speaks Question)"]
    Permissions["Permission Handling<br/>(Speech & Microphone)"]
    UIState["UI State Management<br/>(Loading, Errors, Display)"]

    User -->|"Taps mic, speaks"| AICardView
    AICardView --> Permissions
    AICardView --> SR
    SR -->|"transcription"| AICardView
    AICardView -->|"query"| AI
    AI -->|"AI answer"| AICardView
    AICardView --> TTS
    TTS -->|"audio out"| User
    AICardView --> UIState
    UIState --> AICardView
    
```

- **Legend**: All flows are mediated by the `AICardView`, which orchestrates user input, permissions, AI calls, and output.

---

### **B. UI State Machine: What the User Sees**

```mermaid
stateDiagram-v2
    [*] --> Idle
    Idle : "Nhấn nút micro để hỏi. <br/> ('Press the microphone button to ask.')"
    Idle --> Recording : Mic tapped
    Recording : "Đang nghe... <br/> ('Listening...')"
    Recording --> AIProcessing : Recording stopped or transcription final
    AIProcessing : "Đang xử lý... <br/> ('Processing...')"
    AIProcessing --> Answered : AI response received
    AIProcessing --> Error : AI error or empty result
    Recording --> Error : Speech permission denied OR mic error
    AnyState --> Error : Any error
    Error : Error banner/message shown
    Answered : Shows 'AI trả lời' and reads aloud
    Error --> Idle : User retries/discards error
    Answered --> Idle : User asks new question
    
```

---

### **C. Detailed Sequence: Voice-to-AI-to-Voice Pipeline**

```mermaid
sequenceDiagram
    participant User
    participant UI as AICardView
    participant Perm as Permissions
    participant Rec as SpeechRecog
    participant LLM as Local AI
    participant TTS as TextToSpeech

    User->>UI: Tap mic
    UI->>Perm: Check/request permissions
    Perm-->>UI: Result (authorized/denied)
    alt Permission granted
        UI->>Rec: Start recording
        Rec-->>UI: Live transcription (updates UI)
        User->>UI: Tap stop OR auto-stop
        Rec-->>UI: Final transcription
        UI->>LLM: Query AI with question
        LLM-->>UI: AI answer
        UI->>TTS: Speak answer
        TTS-->>User: Audio
    else Permission denied/error
        Perm-->>UI: Show error message
    end
```

---

### **D. AIModel Loading and Error Handling**

```mermaid
flowchart LR
    subgraph AIInteraction
        A1("runDemoAIModel(question)")
        A2{Model loads successfully?}
        A3["-> Preprocess question"]
        A4["-> Get completion from AI"]
        A5["<- AI returns response"]
        A6["Return response to view"]
    end
    subgraph Errors
        E1["Initialization failure"]
        E2["Processing error"]
        E3["Speech recognition error"]
        E4["Permission denied"]
    end

    A1 --> A2
    A2 -- Yes --> A3
    A3 --> A4 --> A5 --> A6
    A2 -- No --> E1
    A4 -- Error --> E2
    A1 -- Error (speech recog) --> E3
    A1 -- Error (permission) --> E4
```

---

### **E. Speech Recognition Flow**

```mermaid
flowchart TD
    Start["User taps mic"] --> CheckPerm["Check Speech & Mic Permissions"]
    CheckPerm -- authorized --> SetupAudio["Setup AVAudioEngine"]
    SetupAudio --> StartRecogSession["Create recognition request & task"]
    StartRecogSession --> Listening["Listen & transcribe..."]
    Listening -- partial results --> UpdateUI["Update displayed question"]
    Listening -- user stops/auto final --> Finalize["Stop recording"]
    Finalize --> IsTextFinal["Is transcription final & non-empty?"]
    IsTextFinal -- Yes --> FetchAI["Call AI with question"]
    IsTextFinal -- No --> ShowError["Show 'no voice detected' error"]
    CheckPerm -- denied/error --> ShowError
    Listening -- mic/audio error --> ShowError
```

---

### **F. Error Handling Map**

```mermaid
graph TD
    EStart["Error Occurs"]
    EStart --> EPerm["Permission Denied"]
    EStart --> ESpeechRec["Speech Recognition Error"]
    EStart --> EAudio["Microphone/Audio Error"]
    EStart --> EAIInit["AI Initialization Failure"]
    EStart --> EAICall["AI Processing Error"]
    EStart --> EPlayback["TTS/Audio Output Error"]

    EPerm --> UIError["Show permission error (red)"]
    ESpeechRec --> UIError
    EAudio --> UIError
    EAIInit --> UIError
    EAICall --> UIError
    EPlayback --> UIError

    UIError["Display error message on card and reset state"]
```

---

### **G. Core Data Structures & Dependencies**

```mermaid
classDiagram
    class AICardView {
        +aiResponse: String?
        +userQuestion: String?
        +isLoading: Bool
        +errorMessage: String?
        +isRecording: Bool
        +speechRecognizer: SFSpeechRecognizer
        +recognitionRequest: SFSpeechAudioBufferRecognitionRequest?
        +recognitionTask: SFSpeechRecognitionTask?
        -audioEngine: AVAudioEngine
        -speechSynthesizer: AVSpeechSynthesizer
        +runDemoAIModel(question): String
        +fetchAIResponse(question)
        +speak(text)
        +requestSpeechAuthorization()
        +startRecording()
        +stopRecording()
        +toggleRecording()
    }
    class AIError {
        <<enum>>
        initializationFailed
        processingError
        speechRecognitionError
        permissionDenied
    }
    class LLM
    class SFSpeechRecognizer
    class AVAudioEngine
    class AVSpeechSynthesizer

    AICardView --> LLM : uses for AI
    AICardView --> SFSpeechRecognizer : speech-to-text
    AICardView --> AVAudioEngine : audio input
    AICardView --> AVSpeechSynthesizer : text-to-speech
    AICardView --> AIError
```

---

### **H. Permissions Decision Tree**

```mermaid
flowchart TD
    StartPerm["App launches or mic tapped"]
    StartPerm --> SRPerm{"Speech recognition permission status?"}
    SRPerm -- Authorized --> MicPerm
    SRPerm -- Denied/Restricted/Unknown --> FailSR
    MicPerm{"Microphone permission status?"}
    MicPerm -- Granted --> Proceed["Proceed to record"]
    MicPerm -- Denied --> FailMic
    FailSR["Show: Cần cấp quyền nhận dạng\ngiọng nói và micro"]
    FailMic["Show: Cần cấp quyền micro"]
```

---

### **I. Data & UI State Synchronization**

```mermaid
flowchart TB
    UserQuestion["userQuestion"]
    AIResponse["aiResponse"]
    IsLoading["isLoading"]
    ErrorMsg["errorMessage"]
    IsRecording["isRecording"]

    UserQuestion -.->|Displayed in UI| UI
    AIResponse -.->|Displayed & spoken| UI
    IsLoading -.->|Show spinner/disable| UI
    ErrorMsg -.->|Show error message| UI
    IsRecording -.->|Update button color & text| UI
```

---

### **J. Quantization Model Impact (AI Loading)**

```mermaid
graph TD
    ModelChoice["LLM Model Quantization"]
    Q8("Q8_0 (8-bit):\nHigh quality, more memory")
    Q5("Q5_K_M (5-bit):\nMiddle ground")
    Q4("Q4_K_M (4-bit):\nLow memory, good quality")
    Q3("Q3_K_M (3-bit):\nSmallest, may degrade quality")

    ModelChoice --> Q8
    ModelChoice --> Q5
    ModelChoice --> Q4
    ModelChoice --> Q3
    Q4 -->|Selected| ModelLoading
    Q8 & Q5 & Q3 -->|Possible alternative| ModelLoading
```
*Shows developer’s comments on memory/quality tradeoffs.*

---

## 3. **Concept Map: End-to-End User Journey**

```mermaid
journey
    title User Journey with AI Vietnamese Chat Card
    section Grant Permissions
      Open app: 5: App
      Grant permissions: 3: User/OS
    section Ask by Voice
      Tap mic & speak: 5: User
      Live transcription: 4: UI
      Stop recording (auto or tap): 5: User/UI
    section AI Processing
      Question sent to LLM: 4: UI
      "Đang xử lý..." spinner: 4: UI
      Receive answer: 5: LLM
    section Output
      Display answer, speak aloud: 5: UI/TTS
      Ready for next question: 5: UI
    section Error Path
      Permission/processing error: 1: UI
      Display clear error msg: 1: UI
```

---

# **Summary Table**

| Aspect                         | Visual Diagram | Concepts Illustrated                                |
|---------------------------------|---------------|-----------------------------------------------------|
| **Overall Data Flow**           | A             | All moving parts: UI, AI, speech, permissions       |
| **UI State Transitions**        | B             | States: idle, recording, processing, error, answer  |
| **Interaction Sequence**        | C             | Timeline and message sequence for a user action     |
| **AI Model Handling**           | D             | Model initialization, error branches                |
| **Speech Recognition Details**  | E             | Stepwise logic from tap to transcription & error    |
| **Error Handling**              | F             | Mapping errors to user-facing UI                    |
| **Class & State Structure**     | G             | Objects, state variables, dependencies              |
| **Permissions Logic**           | H             | Nested permission checks and outcomes               |
| **State Synchronization**       | I             | UI updates and their triggers                       |
| **LLM Quantization Choice**     | J             | Memory/quality tradeoffs in LLM selection           |
| **User Journey**                | Journey Map   | Bird’s-eye view of app usage scenario               |

---

## **How to Read These Diagrams**

- **Flowcharts:** Show logical or data pathways and choices.
- **State diagrams:** Depict how the UI state changes with actions or results.
- **Sequence diagrams:** Clarify interaction timeline and message flow between components and actors.
- **Class diagrams:** Map out how code entities connect.
- **Journey diagrams:** Visualize the high-level user-centered journey.
- **Quantization tradeoff:** Relates a developer concern.

---
