---
created: 2025-05-12 05:31:26
author: Cong Le
version: "1.0"
license(s): MIT, CC BY 4.0
copyright: Copyright (c) 2025 Cong Le. All Rights Reserved.
---



# Visual Representation of Project Concepts
> **Disclaimer:**
>
> This document contains my personal notes on the topic,
> compiled from publicly available documentation and various cited sources.
> The materials are intended for educational purposes, personal study, and reference.
> The content is dual-licensed:
> 1. **MIT License:** Applies to all code implementations (Swift, Mermaid, and other programming languages).
> 2. **Creative Commons Attribution 4.0 International License (CC BY 4.0):** Applies to all non-code content, including text, explanations, diagrams, and illustrations.
---

Based on the comprehensive LaTeX document detailing the project "Privacy-Preserving Real-Time Vietnamese-English Translation on iOS using Edge AI," the following diagrams provide visual representations of the core architecture, model rationale, development process, and related aspects.


## 1. On-Device AI Translation Pipeline Architecture

This diagram illustrates the core data flow and components of the on-device translation system as described in the methodology and technical highlights.

```mermaid
---
title: "On-Device AI Translation Pipeline Architecture"
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
      'primaryBorderColor': '#27AE60',
      'secondaryColor': '#EEF0',
      'secondaryTextColor': '#6C3483',
      'secondaryBorderColor': '#A569BD',
      'fontSize': '15px'
    }
  }
}%%
flowchart TD
  My_Meme@{ img: "https://raw.githubusercontent.com/CongLeSolutionX/MY_GRAPHIC_ASSETS/refs/heads/Designing_graphic_syntax/MY_MEME/My-meme-icon-design.png", label: "User Voice Input", pos: "b", w: 100, h: 100, constraint: "on" }

    My_Meme --> B("AVFoundation <br/>- Audio Capture")
    B --> C("Native iOS Speech-to-Text")
    C --> D{"Text Input<br/>(Vietnamese)"}
    D --> E("Core ML /<br/>Local LLM Wrapper")
    E --> F["TinyLlama 1.1B Chat v1.0 GGUF<br/>(Q4_K_M)"]
    E --> G("On-Device Inference")
    G --> H{"Translated Text Output<br/>(English)"}
    H --> I("SwiftUI UI Display")
    H --> J("AVFoundation <br/>- Text-to-Speech")
    J --> K["Translated Speech Output"]

    %% Styling and grouping
    subgraph Edge_AI_Processing["Edge AI Processing<br/>(On Device)"]
    style Edge_AI_Processing fill:#2f11,stroke:#333,stroke-width:2px
        D
        E
        F
        G
        H
    end
    subgraph Core_iOS_Frameworks["Core iOS Frameworks"]
    style Core_iOS_Frameworks fill:#22f1,stroke:#333,stroke-width:2px
       B
       C
       I
       J
    end

   classDef coreML fill:#21ff,stroke:#333,stroke-width:2px
   class F coreML
   class G coreML

   classDef framework fill:#c22,stroke:#333,stroke-width:2px
   class B framework
   class C framework
   class I framework
   class J framework

   classDef inputoutput fill:#f21c2,stroke:#333,stroke-width:2px
   class My_Meme inputoutput
   class D inputoutput
   class H inputoutput
   class K inputoutput
   
```


---

## 2. Model Selection Rationale: Why TinyLlama GGUF?

This graph highlights the decision process and key factors leading to the selection of the TinyLlama 1.1B Chat v1.0 in GGUF format with Q4\_K\_M quantization.

```mermaid
---
title: "Model Selection Rationale: Why TinyLlama GGUF?"
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
      'primaryBorderColor': '#27AE60',
      'secondaryColor': '#EBDEF0',
      'secondaryTextColor': '#6C3483',
      'secondaryBorderColor': '#A569BD',
      'fontSize': '15px'
    }
  }
}%%
flowchart TD
    A["Project Goal:<br/>Privacy-Preserving, Real-Time On-Device Translation"] --> B{"Challenge:<br/> Deploying NMT on Resource-Limited iOS"}
    
    B --> C1["Requires Small Model Size"]
    B --> C2["Requires Fast Inference"]
    B --> C3["Needs Framework Compatibility<br/>(Core ML, Swift)"]
    B --> C4["Privacy:<br/>Must run OFFLINE"]
    B --> C5["Performance:<br/>Leveraging Hardware (Neural Engine)"]

    C1 & C2 & C4 --> D["Research & Experimentation:<br/>Lightweight Transformers"]
    D --> E["Identified Candidate Models<br/> (MobileBERT, TinyBERT, Large LLMs)"]
    E --> F["Large LLMs<br/>(e.g., Llama-2-7B)"]
    E --> G["Compact/<br/>Optimized Models"]
    G --> H["TinyLlama 1.1B Chat v1.0"]

	%% Contrast
    F --> F1["High Quality, Large Size, High Resource Needs"]
    
    G --> G1["Compact Size, Lower Resource Needs"]

    H --> H1["~1.1B Parameters<br/>(Manageable)"]
    H --> H2["Chat-Optimized Training<br/>(Relevant)"]
    H --> H3["Open License &<br/> Llama 2 Architecture"]
    H --> H4["Availability of GGUF Format"]

    H4 --> I{"GGUF Format:<br/>Optimized for CPU Inference"}
    I --> I1["Reduced File Size"]
    I --> I2["Improved Loading Speed"]
    I --> I3["Quantization Support"]

    I3 --> J{"Quantization<br/>(INT8, GGUF Q_K Levels)"}
    J --> J1["Q2_K: Min Size, Max Quality Loss"]
    J --> J2["Q3_K_M: Small Size, High Quality Loss"]
    J --> J3["Q4_K_M:<br/>Good Balance (Size/Quality)"]
    J --> J4["Q5_K_S/M:<br/>Better Quality, Larger Size"]
    J --> J5[Q6_K/Q8_0:<br/>Near Original, Largest Size]

    J3 --> K["Selected:<br/>Q4_K_M<br/>(~0.67GB, ~3.17GB RAM)"]
    K --> L["Proven Community Performance /<br/> Initial Experiments"]

    H & K & C3 & C5 --> M["FINAL SELECTION:<br/> TinyLlama 1.1B Chat v1.0 GGUF<br/>(Q4_K_M)"]

	%% Connects back to fulfilling the project goal
    M --> A
    

   classDef challenge fill:#f2c3,stroke:#333,stroke-width:1px
   class B challenge
   class C1,C2,C3,C4,C5 challenge

   classDef model fill:#c1f3,stroke:#333,stroke-width:1px
   class F,G,H model
    class M model

   classDef format fill:#d1f1,stroke:#333,stroke-width:1px
   class I format
   class J format

   classDef selection fill:#9c22,stroke:#333,stroke-width:2px
   class K selection
    class M selection
```

---

## 3. Key iOS Frameworks and Their Functionality

This diagram illustrates how specific iOS frameworks are integrated and utilized within the project's pipeline.

```mermaid
---
title: "Key iOS Frameworks and Their Functionality"
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
      'primaryBorderColor': '#27AE60',
      'secondaryColor': '#EBDEF0',
      'secondaryTextColor': '#6C3483',
      'secondaryBorderColor': '#A569BD',
      'fontSize': '15px'
    }
  }
}%%
flowchart LR
  A["iOS Application"] --> B("AVFoundation")
  B --> B1["Audio Capture/Input"]

  %% Native iOS or AVFoundation related
  B1 --> C("Speech-to-Text")
    
  B --> B2["Audio Playback/Output"]
  
  %% Native iOS or AVFoundation related
  B2 --> D("Text-to-Speech")
 
    A --> E("Core ML")
    E --> E1["Load & Run ML Model<br/>(TinyLlama)"]
    E --> E2["Leverage Neural Engine<br/>(Hardware Accel.)"]

    A --> F("SwiftUI")
    F --> F1["Build User Interface<br/>(UI)"]
    F1 --> F2["Manage UI State"]
    F1 --> F3["Display Translated Text"]

    A --> G("Combine")
    G --> G1["Manage Asynchronous Data Flow"]
    G1 --> G2["Between Audio, STT, Model, UI"]
    G1 --> G3["Handle Pipeline Coordination"]

  %% STT output is model input
  C --> E1
  
  %% Model output displayed in UI
  E1 --> F1
  
  %% UI could trigger TTS playback
  F1 --> B2
  
  %% TTS output goes through AVFoundation
  D --> B2

  classDef framework fill:#ace2,stroke:#333,stroke-width:2px
  class B,E,F,G framework

  classDef functionality fill:#e22e,stroke:#333,stroke-width:1px
  class B1,B2,C,D,E1,E2,F1,F2,F3,G1,G2,G3 functionality
   
```

---

## 4. Project Development Pipeline and Timeline

This flowchart outlines the three distinct phases of the project's execution timeline (Spring 2025).

```mermaid
---
title: "Project Development Pipeline and Timeline"
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
      'primaryBorderColor': '#27AE60',
      'secondaryColor': '#EBDEF0',
      'secondaryTextColor': '#6C3483',
      'secondaryBorderColor': '#A569BD',
      'fontSize': '15px'
    }
  }
}%%
flowchart TD
    P1("Phase 1:<br/>Model Refinement, Architecture Finalization, Quantization")
    P2("Phase 2:<br/>iOS App Development, UI & Pipeline Optimization")
    P3("Phase 3:<br/>System Testing, User Evaluation, Finalization")

    P1 --> A["Weeks 1-7"]
    A --> A1["Literature Review, Tool ID"]
    A --> A2["Dataset Acquisition & Prep"]
    A --> A3["Initial Model Concepts &<br/> Experiments"]
    A --> A4["Model Selection Finalization:<br/>TinyLlama GGUF Q4_K_M"]
    A --> A5["iOS Core Structure Prep"]

    P2 --> B["Weeks 8-13"]
    B --> B1["Develop Primary SwiftUI UI"]
    B --> B2["Implement &<br/> Optimize Real-Time Pipeline<br/> (Audio, STT, Model, TTS)"]
    B --> B3["Incorporate TinyLlama GGUF via wrapper"]
    B --> B4["Initial Performance Profiling"]

    P3 --> C["Weeks 14-16"]
    C --> C1["Intensive System Testing<br/>(Offline, Latency, Usage)"]
    C --> C2["Collect Usability Feedback"]
    C --> C3["Iterative Refinement<br/>(UI/UX, Pipeline)"]
    C --> C4["Complete Documentation"]
    C --> C5["Prepare Demonstration"]
    C --> C6["Final Evaluation"]

    A --> B
    B --> C

   classDef phase fill:#bdf3,stroke:#333,stroke-width:2px;
   class P1,P2,P3 phase

   classDef weeks fill:#fda3,stroke:#333,stroke-width:1px;
   class A,B,C weeks

   classDef activities fill:#e3e2,stroke:#333,stroke-width:1px
   class A1,A2,A3,A4,A5,B1,B2,B3,B4,C1,C2,C3,C4,C5,C6 activities

```

----

## 5. Challenges, Limitations, and Future Work

This diagram groups related points concerning the hurdles faced, current shortcomings, and planned future directions for the project.

```mermaid
---
title: "Challenges, Limitations, and Future Work"
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
      'primaryBorderColor': '#27AE60',
      'secondaryColor': '#EBDEF0',
      'secondaryTextColor': '#6C3483',
      'secondaryBorderColor': '#A569BD',
      'fontSize': '15px'
    }
  }
}%%
flowchart BT
    Challenges
    Limitations
    FutureWork["Future Work"]
    
    Challenges --> C1["Low-Latency Audio Pipeline<br/> (AVFoundation)"]
    Challenges --> C2["Balancing Quantization vs. Accuracy"]
    Challenges --> C3["Bridging Model Input/Output with Swift/iOS pipeline"]


    Limitations --> L1["Translation Quality<br/> (Complex/Idiomatic)"]
    Limitations --> L2["Basic User Interface<br/>(Needs UI/UX, Localization, Accessibility)"]
    Limitations --> L3["Limited Benchmarking<br/>(Across Devices)"]
    Limitations --> L4["Single Language Pair<br/>(Vietnamese-English Only)"]


    FutureWork --> F1["Explore Advanced Architectures/Distillation"]
    FutureWork --> F2["Deepen Data Augmentation /<br/> Domain-Specific Data"]
    FutureWork --> F3["Optimize Memory/<br/>Battery Usage<br/> (Hardware Tuning, Memory Mapping)"]
    FutureWork --> F4["Expand UI Features &<br/>Accessibility"]
    FutureWork --> F5["Incorporate Additional Language Pairs"]
    FutureWork --> F6["Conduct Systematic User Studies"]

    Challenges --> Limitations

	%% Future work aims to address limitations
    Limitations --> FutureWork
    
    classDef group fill:#dff2,stroke:#333,stroke-width:2px
    class Challenges,Limitations,FutureWork group

    classDef points fill:#e2e2,stroke:#333,stroke-width:1px
    class C1,C2,C3,L1,L2,L3,L4,F1,F2,F3,F4,F5,F6 points


```





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