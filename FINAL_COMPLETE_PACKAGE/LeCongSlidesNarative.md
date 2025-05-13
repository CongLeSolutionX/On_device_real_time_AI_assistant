

---

Here is a plan for a 10-slide presentation based on the provided document:

**Slide 1: Title Slide**

*   **Title:** Privacy-Preserving Real-Time Vietnamese-English Translation on iOS using Edge AI
*   **Subtitle:** Leveraging Lightweight Edge AI for On-Device Translation
*   **Prepared by:** Cong Le
*   **Advisor:** Dr. Ning Chen
*   **Affiliation:** Department of Computer Science, California State University, Fullerton
*   **Date:** Spring 2025

**Slide 2: The Problem: Cloud Translation Challenges**

*   Growing need for instant, accurate language translation.
*   Traditional cloud-based services:
    *   Compromise user privacy (data transmitted externally, risk of breaches/misuse).
    *   Limited by connectivity (unreliable network access).
*   Issues acute for Vietnamese-English translation:
    *   Many rely on constant internet access and cloud processing.
    *   Significant privacy concerns and limitations in resource-constrained environments.
*   Lack of seamlessness, inefficiency, inconsistent accuracy (Vietnamese nuances), inadequate offline capability in mainstream solutions.

**Slide 3: Our Solution: Edge AI Approach**

*   **Goal:** Deliver a robust, real-time, offline, privacy-preserving NMT system for Vietnamese-English on iOS.
*   **Approach:** Edge AI – conducting all natural language processing directly on the device.
*   **Supported by:** Harnessing computational power and privacy focus of modern Apple devices.
*   **Key Benefits Enabled by Edge AI:**
    *   Real-time translation speeds (immediate communication).
    *   Full offline capability (usability anywhere, anytime).
    *   Stringent privacy (absolutely no user data leaves the device).
    *   Transparent and reassuring user experience.

**Slide 4: System Architecture & Core iOS Frameworks**

*   **Architecture:** All computational workload for NLP models strictly on the end-user device.
*   **Pipeline Outlined:** Unidirectional data flow:
    *   Audio Input $\rightarrow$ Speech-to-Text (STT) $\rightarrow$
    *   Text Input to NMT Model (via local inference) $\rightarrow$
    *   Translated Text Output $\rightarrow$ Display / Optional Text-to-Speech (TTS)
*   **Integrated iOS Frameworks:**
    *   **AVFoundation:** Robust audio handling (STT input, TTS output).
    *   **Core ML:** Efficient, hardware-accelerated model inference on-device.
    *   **SwiftUI:** Building a modern, responsive user interface.
    *   **Combine:** Managing reactive and efficient data pipelines.

**Slide 5: Model Selection Deep Dive: TinyLlama 1.1B Chat v1.0**

*   Needed a compact yet capable NMT model suitable for mobile.
*   Identified **TinyLlama 1.1B Chat v1.0** (1.1 billion parameters) as a promising candidate.
*   **Rationale:**
    *   **Compactness:** Significantly smaller than larger LLMs (e.g., Llama-2-7B), key for mobile resources.
    *   **Chat-Optimized:** Fine-tuned on dialogue datasets, beneficial for conversational translation.
    *   **Open License/Architecture:** Facilitates adoption and integration.
    *   Available in highly efficient formats like GGUF.

**Slide 6: Bringing the Model On-Device: GGUF & Quantization**

*   **The GGUF Format:** Optimized file format for storing LLM weights, developed for efficient CPU inference (\emph{llama.cpp}). Reduces file size, improves loading speed.
*   **Quantization:** Technique to reduce precision of model weights (e.g., 32-bit float to 4-bit integer).
    *   Drastically shrinks model size and reduces memory usage.
    *   Enables faster computations, leverages specialized hardware (Neural Engine).
*   **Selection Rationale (Q4\_K\_M):**
    *   Experiments and community observation show Q4\_K\_M offers the best balance for practical mobile deployment.
    *   Manageable file size (approx. 0.67GB).
    *   Reasonable runtime memory (around 3.17GB peak).
    *   Retains sufficient translation quality.
    *   Enables near real-time speeds on modern iOS devices.

**Slide 7: Development Pipeline Summary & Progress**

*   Project structured in three phases (Spring 2025):
    1.  **Phase 1 (Weeks 1-7):** Model Refinement, Architecture Finalization, Quantization.
    2.  **Phase 2 (Weeks 8-13):** iOS Application Development (UI, Pipeline Optimization).
    3.  **Phase 3 (Weeks 14-16):** System Testing, User Evaluation, Finalization.
*   **Progress (as of Spring 2025):**
    *   **Phase 1 Completed:** Literature review, framework/dataset selection, analysis of architectures. Integrated TinyLlama 1.1B Chat v1.0 GGUF (Q4\_K\_M). Dataset collected/preprocessed. Prototype model pipeline integrated (feasibility shown).
    *   **Phase 2 Progress:** Core iOS application skeleton functioning. Basic integration points for audio capture, Core ML inference calls, rudimentary UI. Data flow using Combine in place.

**Slide 8: Key Achievements & Demonstrated Performance**

*   **Efficient Quantized NMT Model Integrated:** Successfully integrated TinyLlama 1.1B Chat v1.0 (Q4\_K\_M GGUF) into iOS.
    *   Achieved substantial size and speed benefits (e.g., 73% size reduction, 49% CPU speedup vs. preliminary baselines/quantized targets; quantified benefits depend on exact TinyLlama Q4\_K\_M deployment specifics but significant).
    *   Minimal BLEU decrease (-0.6) in preliminary tests suggests usable quality retention.
*   **Functional Real-Time iOS Prototype Built:** Core Audio $\rightarrow$ STT (Native) $\rightarrow$ On-Device Translation (TinyLlama/Core ML) $\rightarrow$ UI Output pipeline verified on recent iOS device (e.g., iPhone 13).
*   **Demonstrated Sub-Second Latency:** Achieved near real-time response crucial for conversational use.

**Slide 9: Challenges, Limitations, and Lessons Learned**

*   **Challenges:**
    *   Implementing a low-latency, reliable audio pipeline (AVFoundation state management).
    *   Balancing model quantization trade-offs (efficiency vs. accuracy/fluency).
    *   Ensuring seamless data pre/post-processing between frameworks (Hugging Face/PyTorch/TF vs. Core ML/GGUF via Swift).
*   **Limitations:**
    *   Translation quality not yet consistently matching high-end cloud solutions (larger models/datasets).
    *   Basic user interface (needs refinement, localization, accessibility).
    *   Benchmarking scope limited (need systematic testing across devices).
    *   Lacks broader multilingual capabilities (currently VN-EN).
*   **Lessons Learned:** Iterative experimentation, careful asynchronous coordination, and precise data interface management are critical.

**Slide 10: Conclusion and Future Work**

*   **Conclusion:** Successfully established a blueprint and delivered a functional prototype for privacy-focused, offline NMT on iOS.
    *   Key: Strategic fusion of efficient, quantized TinyLlama (Q4\_K\_M GGUF) with optimized iOS framework integration.
    *   Achieved absolute independence (no external servers), preserving privacy.
    *   Demonstrated sub-second latency on commodity hardware.
    *   Validates edge AI potential for powerful, privacy-respecting mobile apps.
*   **Future Work:**
    *   Explore advanced model architectures, distillation, domain-specific fine-tuning.
    *   Deepen data augmentation, curate domain-specific datasets.
    *   Optimize memory footprint, reduce battery consumption (hardware tuning, memory mapping).
    *   Expand UI features (saving translations, accessibility, new languages).
    *   Conduct systematic user studies and field testing.

---

