---
created: 2025-05-12 05:31:26
author: Cong Le
version: "1.0"
license(s): MIT, CC BY 4.0
copyright: Copyright (c) 2025 Cong Le. All Rights Reserved.
---




# Visual Schematic Collection for "Privacy-Preserving Real-Time Vietnamese-English Translation on iOS using Edge AI"
> **Disclaimer:**
>
> This document contains my personal notes on the topic,
> compiled from publicly available documentation and various cited sources.
> The materials are intended for educational purposes, personal study, and reference.
> The content is dual-licensed:
> 1. **MIT License:** Applies to all code implementations (Swift, Mermaid, and other programming languages).
> 2. **Creative Commons Attribution 4.0 International License (CC BY 4.0):** Applies to all non-code content, including text, explanations, diagrams, and illustrations.
---

The following diagrams and illustrative models are designed to visually convey the complex technical concepts, methodology, architecture, and insights detailed in the reviewed LaTeX paper. They utilize Mermaid syntax for clear and modern presentation, and are intended for inclusion in an article, thesis, or as supporting content for presentations.

---

## 1. High-Level System Architecture: On-Device AI Translation Pipeline


```mermaid
---
title: "High-Level System Architecture: On-Device AI Translation Pipeline"
author: "Cong Le"
version: "1.0"
license(s): "MIT, CC BY 4.0"
copyright: "Copyright (c) 2025 Cong Le. All Rights Reserved."
config:
  layout: dagre
  theme: base
---
%%%%%%%% Mermaid version v11.4.1-b.14
%%%%%%%% Available curve styles include the following keywords:
%% basis, bumpX, bumpY, cardinal, catmullRom, linear, monotoneX, monotoneY, natural, step, stepAfter, stepBefore.
%%{
  init: {
    'flowchart': { 'htmlLabels': false, 'curve': 'linear' },
    'fontFamily': 'Monaco',
    'themeVariables': {
      'primaryColor': '#2FB1',
      'primaryTextColor': '#F8B229',
      'lineColor': '#F8B229',
      'primaryBorderColor': '#27AE60',
      'secondaryColor': '#EBF2',
      'secondaryTextColor': '#6C3483',
      'secondaryBorderColor': '#A569BD',
      'fontSize': '15px'
    }
  }
}%%
flowchart TD
	My_Meme@{ img: "https://raw.githubusercontent.com/CongLeSolutionX/MY_GRAPHIC_ASSETS/refs/heads/Designing_graphic_syntax/MY_MEME/My-meme-icon-design.png", label: "User Speech<br/>(Audio Input)", pos: "b", w: 100, h: 100, constraint: "on" }
	
    My_Meme --> B["Speech-to-Text<br/>(AVFoundation)"]
    B --> C{"Edge AI NMT Model<br/>(TinyLlama 1.1B Q4_K_M <br/>+ Wrapper/Core ML)"}
    C --> D["Text Postprocessing<br/>& UI Display<br/>(SwiftUI)"]
    D --> E["Optional:<br/>Text-to-Speech Output<br/>(AVFoundation)"]
    
    style C fill:#f967,stroke:#222,stroke-width:2px
    style B fill:#c6c9
    style D fill:#b5fc
    style E fill:#fcb3
    
```


**Explanation:**  
All translation and inference occur locally on the iOS device. User voice is processed directly into text, passed into the on-device quantized NMT model, then displayed (and optionally vocalized) without any user data leaving the device, ensuring privacy.

---

## 2. Project Methodology & Phase Timeline

```mermaid
---
title: "Project Methodology & Phase Timeline"
author: "Cong Le"
version: "1.0"
license(s): "MIT, CC BY 4.0"
copyright: "Copyright (c) 2025 Cong Le. All Rights Reserved."
config:
  theme: base
---
%%%%%%%% Mermaid version v11.4.1-b.14
%%{
  init: {
    'gantt': { 'htmlLabels': false},
    'fontFamily': 'Monaco',
    'themeVariables': {
      'primaryColor': '#F2E9',
      'primaryTextColor': '#F8B229',
      'lineColor': '#F8B229',
      'primaryBorderColor': '#27AE60',
      'secondaryColor': '#EBDEF0',
      'secondaryTextColor': '#DD99',
      'secondaryBorderColor': '#A569BD',
      'fontSize': '15px'
    }
  }
}%%
gantt
    title Project Timeline: Key Phases and Milestones
    dateFormat  YYYY-MM-DD
    section Phase 1: Model & Architecture
    Model Selection & Quantization         :done,   m1,2025-01-20,2025-02-10
    NMT Architecture Finalization          :done,   m2,2025-02-11,2025-02-28
    CoreML/GGUF Integration                :done,   m3,2025-03-01,2025-03-09
    section Phase 2: App & Pipeline
    iOS UI Development (SwiftUI)           :active, m4,2025-03-10,2025-03-31
    Real-Time Data Pipeline Optimization   :active, m5,2025-03-17,2025-04-14
    Initial App Testing / Profiling        :        m6,2025-04-01,2025-04-20
    section Phase 3: Testing & Finalization
    Systematic Device Testing & Tuning     :        m7,2025-04-21,2025-05-10
    Usability Studies and Feedback         :        m8,2025-05-01,2025-05-15
    Project Documentation & Demo           :        m9,2025-05-11,2025-05-18
```

**Explanation:**  
This Gantt chart summarizes the project execution: model and format selection, pipeline development, and multi-staged validation/refinement, concluding with comprehensive testing and demonstration.

---

## 3. Core Edge AI Model Stack

```mermaid
---
title: "Core Edge AI Model Stack"
author: "Cong Le"
version: "1.0"
license(s): "MIT, CC BY 4.0"
copyright: "Copyright (c) 2025 Cong Le. All Rights Reserved."
config:
  layout: dagre
  theme: base
---
%%%%%%%% Mermaid version v11.4.1-b.14
%%%%%%%% Available curve styles include the following keywords:
%% basis, bumpX, bumpY, cardinal, catmullRom, linear, monotoneX, monotoneY, natural, step, stepAfter, stepBefore.
%%{
  init: {
    'flowchart': { 'htmlLabels': false, 'curve': 'linear' },
    'fontFamily': 'Monaco',
    'themeVariables': {
      'primaryColor': '#2FB1',
      'primaryTextColor': '#F8B229',
      'lineColor': '#F8B229',
      'primaryBorderColor': '#27AE60',
      'secondaryColor': '#EBDEF0',
      'secondaryTextColor': '#6C3483',
      'secondaryBorderColor': '#A569BD',
      'fontSize': '15px'
    }
  }
}%%
flowchart TD
    A["TinyLlama 1.1B Chat v1.0<br/>(Transformer Architecture)"]
    A --> B["Community Quantization Toolkit"]
    B --> C["GGUF Model Format<br/>(2, 3, 4, 5, 6, 8-bit Levels)"]
    C --> D["Q4_K_M<br/>(Project's Selected Compromise)"]
    D --> E["LLM Wrapper/Runtime<br/>(Library/Bridge)"]
    E --> F["iOS Inference Engine<br/>(Core ML/Neural Engine or Local Lib)"]
    
```

**Explanation:**  
This diagram traces the journey from model selection (TinyLlama) through quantization, efficient storage (GGUF), and final deployment as a high-performance, low-memory iOS model.

---

## 4. Data Privacy Focus: Comparison Flow

```mermaid
---
title: "Data Privacy Focus: Comparison Flow"
author: "Cong Le"
version: "1.0"
license(s): "MIT, CC BY 4.0"
copyright: "Copyright (c) 2025 Cong Le. All Rights Reserved."
config:
  layout: dagre
  theme: base
---
%%%%%%%% Mermaid version v11.4.1-b.14
%%%%%%%% Available curve styles include the following keywords:
%% basis, bumpX, bumpY, cardinal, catmullRom, linear, monotoneX, monotoneY, natural, step, stepAfter, stepBefore.
%%{
  init: {
    'flowchart': { 'htmlLabels': false, 'curve': 'linear' },
    'fontFamily': 'Monaco',
    'themeVariables': {
      'primaryColor': '#22BB',
      'primaryTextColor': '#F8B229',
      'lineColor': '#F8B229',
      'textColor': '#22F829',
      'primaryBorderColor': '#27AE60',
      'secondaryColor': '#E222',
      'secondaryTextColor': '#6C3483',
      'secondaryBorderColor': '#A569BD',
      'fontSize': '15px'
    }
  }
}%%
flowchart LR
    A["Traditional Cloud Translation"]
      -->|Audio/Text sent to remote servers| B["Cloud NMT Model"]
      -->|External Processing| C["Result Returned to Device"]
      -->|Potential Privacy Risk| D["Data Breach/Leak"]
    
    style B fill:#fd22,stroke:#f2f,stroke-width:2px
    style D fill:#e373
    
    subgraph Alternative_Option["Alternative Option"]
    style Alternative_Option fill:#2FB2,stroke:#3e3c
        E["Edge AI On-Device"]
        E -.->|No External Data| F["Local Model/Processing"]
        F -.->|Private| G["Secure Result"]
    end
    style F fill:#c6c9,stroke:#3e3c
```

**Explanation:**  
Contrasts traditional cloud-based translation (with inherent data privacy risks) to the privacy-preserving on-device Edge AI approach in this research.

---

## 5. Quantization Trade-Offs: Size vs. Quality

```mermaid
---
title: "Quantization Trade-Offs: Size vs. Quality"
author: "Cong Le"
version: "1.0"
license(s): "MIT, CC BY 4.0"
copyright: "Copyright (c) 2025 Cong Le. All Rights Reserved."
config:
  theme: base
---
%%%%%%%% Mermaid version v11.4.1-b.14
%%%%%%%% Toggle theme value to `base` to activate the initilization below for the customized theme version.
%%{
  init: {
    'pie': { 'htmlLabels': false },
    'fontFamily': 'Monaco',
    'themeVariables': {
      'primaryColor': '#B2F8'
    }
  }
}%%
pie
    title TinyLlama 1.1B Quantization Impact
    "Model Size Gain (Q4_K_M vs. Q8_0)" : 73
    "CPU Speedup" : 49
    "Quality Loss (BLEU metric, normalized scale)" : 1
```

**Explanation:**  
Relative illustration of efficiency gains with Q4_K_M quantization over larger/baseline models. Substantial reduction in required storage and compute for only a small drop in translation quality.

---

## 6. Technical Stack and Resource Map

```mermaid
---
title: "Technical Stack and Resource Map"
author: "Cong Le"
version: "1.0"
license(s): "MIT, CC BY 4.0"
copyright: "Copyright (c) 2025 Cong Le. All Rights Reserved."
config:
  theme: base
---
%%%%%%%% Mermaid version v11.4.1-b.14
%%%%%%%% Available curve styles include the following keywords:
%% basis, bumpX, bumpY, cardinal, catmullRom, linear, monotoneX, monotoneY, natural, step, stepAfter, stepBefore.
%%{
  init: {
    'fontFamily': 'Monaco',
    'themeVariables': {
      'primaryColor': '#D5E3',
      'primaryTextColor': '#F8B229',
      'primaryBorderColor': '#27AE60',
      'secondaryColor': '#EBF2',
      'secondaryTextColor': '#6C3483',
      'secondaryBorderColor': '#A569BD',
      'fontSize': '20px'
    }
  }
}%%
mindmap
  root((iOS Translation App Technical Stack))
    Model_Development
      Python
      PyTorch
      HuggingFace_Transformers
      Quantization_Toolkits
    Data_Prep
      WMT
      OPUS
      IWSLT
      UIT-ViEn
      BPE_Tokenization
      Cleaning_Scripts
    App_Dev
      Xcode
      Swift
      SwiftUI
      AVFoundation
      CoreML
      Combine
      LLM_Wrapper
    Testing
      iPhone_with_A15_or_newer
      Usability_Testers
      BLEU_Scores
      Real-Device_Latency
    Collaboration
      Faculty_Advisor
      NLP_Research_Groups
```

**Explanation:**  
Mindmap shows the interrelated technological and human resources: from data collection and model training to app development, device testing, and academic collaboration.

---

## 7. Pipeline Data/Control Flow Overview

```mermaid
---
title: "Pipeline Data/Control Flow Overview"
author: "Cong Le"
version: "0.1"
license(s): "MIT, CC BY 4.0"
copyright: "Copyright (c) 2025 Cong Le. All Rights Reserved."
config:
  layout: elk
  theme: base
---
%%%%%%%% Mermaid version v11.4.1-b.14
%%{
  init: {
    'sequence': { 'mirrorActors': true, 'showSequenceNumbers': true, 'actorMargin': 50 },
    'fontFamily': 'Monaco',
    'themeVariables': {
      'primaryColor': '#2BB8',
      'primaryBorderColor': '#7C0000',
      'lineColor': '#F8B229',
      'secondaryColor': '#6122',
      'tertiaryColor': '#fff',
      'fontSize': '15px',
      'textColor': '#F8B229',
      'actorTextColor': '#E2E',
      'stroke':'#033',
      'stroke-width': '0.2px'
    }
  }
}%%
sequenceDiagram
    actor User as User
    participant UI as SwiftUI
    participant Audio as AVFoundation
    participant STT as Speech-to-Text
    participant NMT as TinyLlama Q4K_M Wrapper
    participant TTS as Text-to-Speech

    User->>UI: Tap Microphone/Start
    UI->>Audio: Start Recording
    Audio-->>STT: Audio Buffer
    STT-->>UI: Recognized Text
    UI->>NMT: Send Text for Translation
    NMT-->>UI: Translated Text
    UI->>TTS: (Optional) Speak Output
    TTS-->>UI: Playback Status
    UI-->>User: Display Translation, Optionally Play Audio
    
```

**Explanation:**  
Sequence diagram captures the user-driven interaction — from voice input, through model inference, to translation display and speech output.

---

## 8. Challenges Overview Diagram

```mermaid
---
title: "Challenges Overview Diagram"
author: "Cong Le"
version: "1.0"
license(s): "MIT, CC BY 4.0"
copyright: "Copyright (c) 2025 Cong Le. All Rights Reserved."
config:
  layout: dagre
  theme: base
---
%%%%%%%% Mermaid version v11.4.1-b.14
%%%%%%%% Available curve styles include the following keywords:
%% basis, bumpX, bumpY, cardinal, catmullRom, linear, monotoneX, monotoneY, natural, step, stepAfter, stepBefore.
%%{
  init: {
    'flowchart': { 'htmlLabels': false, 'curve': 'linear' },
    'fontFamily': 'Monaco',
    'themeVariables': {
      'primaryColor': '#2FB1',
      'primaryTextColor': '#F8B229',
      'lineColor': '#F8B229',
      'primaryBorderColor': '#27AE60',
      'secondaryColor': '#DEF2',
      'secondaryTextColor': '#6C3483',
      'secondaryBorderColor': '#A569BD',
      'fontSize': '15px'
    }
  }
}%%
graph LR
    C1["Data Preparation"] -- Noise, Duplicates, Encoding --> C2["Cleaned Dataset"]
    Q1["Quantization"] -- Trade-off:<br/>Speed vs. Quality --> Q2["Optimal Setting Selection"]
    P1["Audio Pipeline"] -- Real-Time, Asynchronicity --> P2["Low-Latency<br/> Data Flow"]
    I1["Model Integration"] -- Tokenization, IO formats --> I2["Bridged Pipeline"]
    C1 & Q1 & P1 & I1 --> X["Integrated Edge AI App"]
    
```

**Explanation:**  
Emphasizes the main technical hurdles and the necessary coordination to bridge data, quantization, audio, and model aspects in a seamless app.

---

## 9. Limitations and Future Work Map

```mermaid
---
title: "Limitations and Future Work Map"
author: "Cong Le"
version: "1.0"
license(s): "MIT, CC BY 4.0"
copyright: "Copyright (c) 2025 Cong Le. All Rights Reserved."
config:
  layout: dagre
  theme: base
---
%%%%%%%% Mermaid version v11.4.1-b.14
%%%%%%%% Available curve styles include the following keywords:
%% basis, bumpX, bumpY, cardinal, catmullRom, linear, monotoneX, monotoneY, natural, step, stepAfter, stepBefore.
%%{
  init: {
    'flowchart': { 'htmlLabels': false, 'curve': 'linear' },
    'fontFamily': 'Monaco',
    'themeVariables': {
      'primaryColor': '#2FB1',
      'primaryTextColor': '#F8B229',
      'lineColor': '#F8B229',
      'primaryBorderColor': '#27AE60',
      'secondaryColor': '#EBDEF0',
      'secondaryTextColor': '#6C3483',
      'secondaryBorderColor': '#A569BD',
      'fontSize': '15px'
    }
  }
}%%
graph TB
    L[Current Prototype Limitations]
    L --> LA[Occasional Quality Gap vs. Cloud]
    L --> LB[Basic UI/UX]
    L --> LC[Limited Device Coverage for Testing]
    L --> LD[Only Vietnamese-English Supported]
    
    L -.-> FW[Future Work]
    FW --> FWA[Model Architecture Discovery/Distillation]
    FW --> FWB[Data Augmentation for Fluency]
    FW --> FWC[Memory/Battery Optimization]
    FW --> FWD[Advanced UI, Localization, Accessibility]
    FW --> FWE[Usability Studies, Wider Device Benchmarks]
    FW --> FWF[Expansion to Multilingual Support]
```

**Explanation:**  
Displays current limitations as starting points for proposed enhancements handled in future iterations.

---

## 10. Full Research Workflow Overview

```mermaid
---
title: "Full Research Workflow Overview"
author: "Cong Le"
version: "1.0"
license(s): "MIT, CC BY 4.0"
copyright: "Copyright (c) 2025 Cong Le. All Rights Reserved."
config:
  layout: dagre
  theme: base
---
%%%%%%%% Mermaid version v11.4.1-b.14
%%%%%%%% Available curve styles include the following keywords:
%% basis, bumpX, bumpY, cardinal, catmullRom, linear, monotoneX, monotoneY, natural, step, stepAfter, stepBefore.
%%{
  init: {
    'flowchart': { 'htmlLabels': false, 'curve': 'linear' },
    'fontFamily': 'Monaco',
    'themeVariables': {
      'primaryColor': '#2FB1',
      'primaryTextColor': '#F8B229',
      'lineColor': '#F8B229',
      'primaryBorderColor': '#27AE60',
      'secondaryColor': '#EBDEF0',
      'secondaryTextColor': '#6C3483',
      'secondaryBorderColor': '#A569BD',
      'fontSize': '15px'
    }
  }
}%%
graph LR
    A["Personal Motivation"]
      --> B["Needs Analysis<br/>(Privacy/Offline/Seamless)"]
      --> C["Research:<br/>Literature & App Review"]
      --> D["Efficient Model Design &<br/> Quantization"]
      --> E["Data Acquisition &<br/> Preparation"]
      --> F["Model Training &<br/> Export"]
      --> G["iOS Pipeline Build"]
      --> H["Testing,<br/>Feedback,<br/>Evaluation"]
      --> I["Refinement,<br/>Finalization,<br/>Demo"]
```

**Explanation:**  
Outlines the high-level trajectory of the entire effort from problem conception, through modeling and app development, to evaluation and showcase.


---
<!-- 
```mermaid
%% Current Mermaid version
info
``` 
-->


```mermaid

---
title: "CongLeSolutionX"
author: "Cong Le"
version: "1.0"
license(s): "MIT, CC BY 4.0"
copyright: "Copyright (c) 2025 Cong Le. All Rights Reserved."
config:
  theme: base
---
%%{
  init: {
    'flowchart': { 'htmlLabels': false },
    'fontFamily': 'Brush Script MT',
    'themeVariables': {
      'primaryColor': '#fc82',
      'primaryTextColor': '#F8B229',
      'primaryBorderColor': '#27AE60',
      'secondaryColor': '#81c784',
      'secondaryTextColor': '#6C3483',
      'lineColor': '#F8B229',
      'fontSize': '20px'
    }
  }
}%%
flowchart LR
    My_Meme@{ img: "https://github.com/CongLeSolutionX/MY_GRAPHIC_ASSETS/blob/Designing_graphic_syntax/MY_MEME_ICONS/Orange-Cloud-Search-Icon-Base-Color-Black-1024x1024.png?raw=true", label: "Ăn uống gì chưa ngừi đẹp?", pos: "b", w: 200, h: 150, constraint: "on" }

    Closing_quote@{ shape: braces, label: "Math and code work together to bring interactive art to life!" }

My_Meme ~~~ Closing_quote

```


---
>**Licenses:**
>
>- **MIT License:**  [![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE) - Full text in [LICENSE](LICENSE) file.
>- **Creative Commons Attribution 4.0 International:** [![License: CC BY 4.0](https://licensebuttons.net/l/by/4.0/88x31.png)](LICENSE-CC-BY) - Legal details in [LICENSE-CC-BY](LICENSE-CC-BY) and at [Creative Commons official site](http://creativecommons.org/licenses/by/4.0/).