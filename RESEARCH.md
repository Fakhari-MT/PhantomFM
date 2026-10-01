# PhantomFM v1.0 — DeadAir-ODC

> **Research & Development Report**

## 1. Overview

PhantomFM is a research prototype at the intersection of **natural language processing, speech synthesis, audio systems engineering, and human–AI interaction design**. The primary goal of the first version was to build an automated audio broadcasting system capable of creating an experience resembling a live radio program rather than a sequence of independent responses.

The main focus of **DeadAir-ODC** was **perceived continuity, presenter personality, listener interaction, and reducing noticeable interruptions in the audio experience**.

## 2. Research Problem

A significant portion of AI-based audio systems reduces interaction to a simple **User Input → AI Response → Speech** pipeline, in which each response is treated as an independent event.

Radio, however, has a temporal flow: its experience is composed of speech, transitions between topics, presenter behavior, audience interaction, and continuation of the program.

The central research question of PhantomFM was therefore:

**How can an AI-based speech generation system be transformed from an audio responder into a continuous, character-driven experience?**

## 3. Design Hypothesis

The main design hypothesis was that the feeling of a system being “alive” does not depend solely on the quality of its synthesized voice. It can emerge from a combination of **speech continuity, presenter personality, listener responsiveness, natural fillers, transitions between topics, and timing**.

Accordingly, PhantomFM was designed from the beginning around the **overall temporal and interactive experience**, rather than TTS quality alone.

## 4. Initial Development and Experiments

The first experiment generated text incrementally, character by character, so that the text appeared as a continuously forming stream before being passed to the TTS system. Although this approach was interesting from a text-generation perspective, it was not well suited to the audio experience.

To introduce variation into the initial content, a small dataset of sample sentences was created and a **Markov model** was tested. In the first stage, **character-level Markov** generation produced invalid combinations and artificial words that also caused problems for TTS. The approach was therefore changed to **word-level Markov generation**. This improved lexical quality but did not fully resolve semantic coherence; some generated sentences were grammatically acceptable while having weak semantic relationships. This limitation was accepted in the current version because the primary goal of v1.0 was to establish a stable audio stream.

## 5. Content Design and Segment Continuity

The initial experiments showed that the quality of the experience was strongly dependent on audio-content design. A collection of long and varied segments was therefore designed for different programs. For example, the weather program covered topics such as weather conditions, travel, driving, personal care, and everyday activities related to weather.

A key design decision was that segments should not feel like independent pieces. Each segment was written as if the presenter had already been speaking before it began and would continue speaking after it ended.

The goal was to eliminate the feeling of **“playing a new audio file.”** This principle was later extended to other programs; for example, news segments were designed to transition naturally from one story to the next.

## 6. Presenter Character Design

One of the core components of the project was defining presenter-specific behavior. In PhantomFM, personality is not limited to voice characteristics or a presenter name; it includes **tone, reaction style, response length, filler type, interaction style, degree of seriousness, humor style, and audience behavior**.

The **Weather Presenter** is serious, precise, patient, and kind, with responses that are generally short and practical. In unexpected situations, hesitations such as “umm...” or “ah...” are used to introduce subtle changes in the presenter's apparent state.

Future versions also envision additional presenters, including a **Political Presenter** with a strongly opinionated personality who reacts to the listener's views and may engage in discussion, and a **Parody Presenter** with a young, humorous, and uninhibited personality whose reactions to the listener can themselves become part of the program's comedy.

## 7. TTS, Latency, and Pre-buffering

In the first stage, the project was connected to **pyttsx3**. Although it was suitable in terms of speed and simplicity, the emotional quality and naturalness of the generated voice were not satisfactory. **Coqui TTS** was subsequently tested and provided more suitable output quality, but increased computational cost and generation time.

After integrating TTS, it became clear that word-by-word audio generation was unsuitable for the intended experience because it increased latency while also reducing natural continuity and **prosody**. The project architecture was therefore shifted from immediate generation of individual pieces toward **segment-level synthesis**.

To reduce noticeable interruptions, the system generates several segments before playback begins and places them in a queue; after playback starts, subsequent segments continue to be generated in the background. This **pre-buffering** approach enabled the initial version to produce several minutes of relatively continuous audio.

At this stage, an important design decision was made:

**In v1.0, perceived continuity takes priority over strict real-time generation.**

## 8. Odyssey and Listener Interaction Architecture

After establishing a stable audio stream, the next stage focused on listener interaction. **Odyssey** was created to answer the following question:

**If a listener speaks in the middle of a program, can the system respond in a way that makes the listener feel genuinely acknowledged?**

In the current version, listener input is divided into three primary categories: **Question, Comment, and Disturbance**. Questions include general and specialized inquiries; comments include positive, negative, and emotional reactions; and disturbances include noise, insults, and other irrelevant inputs. Subcategories and predefined responses are designed for each group.

To avoid repeating identical responses, responses are divided into **Opener + Core Response + Ender** modules, allowing different combinations to be generated for the same intent.

During an interruption, the current segment is discarded so that the system does not resume from the middle of the previous sentence. A short filler is then played, the main response is generated and passed to TTS, and the program returns to the next segment after the Ender. Fillers and Enders are generated before playback and stored in the queue, allowing the system to provide a short audio reaction immediately after receiving listener input.

## 9. Dead Air

Despite the optimizations described above, the main response still requires TTS generation. Therefore, an unavoidable delay remains between receiving a question and producing the final response.

This limitation gave the first version its name:

**DeadAir**

Rather than hiding this limitation, the current version documents it as a known architectural characteristic.

## 10. Language Model and Debugging

In the MVP, **Phi-3 Mini** was used for grammar experiments and intent detection. It performed adequately in the initial evaluations, but was not sufficient for deeper contextual understanding or fully natural response generation. Future versions will require a stronger language model and a more context-aware architecture.

The initial version also encountered a noise-related issue associated with multi-speaker synthesis. After examining samples and reported experiences with the model, the issue was identified as a known problem under certain configurations. The multi-speaker configuration was subsequently adjusted, resolving the issue in the version used. This was one example of debugging based on observing the system's actual behavior.

## 11. Research Limitations

This version is not yet a complete user-centered study. No claim is made regarding a definitive improvement in user satisfaction or a reduction in loneliness.

For more rigorous evaluation, future versions will require a **user study** and quantitative measures of **latency, naturalness, continuity, and perception of presence**, along with comparison against conventional voice assistants.

## 12. Future Direction

### PhantomFM v2.0 — 4THW411 

The next version will focus on **natural interaction**. Primary goals include:

- Multiple independent presenters
- Switching between programs
- Voice-based interaction
- Faster TTS
- More coherent text generation
- Contextual response generation
- Presenter-specific language policies
- Reduced dead air
- More natural interaction

### Long-term Vision

PhantomFM is ultimately not intended to be merely an artificial radio station. The long-term vision is a framework for **interactive autonomous audio experiences**, in which each program can have its own **personality, behavioral rules, linguistic style, content source, speech model, and interaction policy**.

This architecture could eventually be explored for **radio, podcasts, education, entertainment, broadcasting, branded audio, and other interactive audio systems**.

## 13. Conclusion

PhantomFM v1.0 demonstrated that creating an experience resembling a radio program using language models and speech synthesis is not simply a matter of generating text and converting it to audio. The central challenge lies in **coordinating time, content, voice, personality, and interaction**.

This project represents the first stage of an effort to build such a system. The current version is not complete, but it has transformed a specific research question into an executable prototype:

> **Can a machine-generated broadcast feel like someone is actually there?**
