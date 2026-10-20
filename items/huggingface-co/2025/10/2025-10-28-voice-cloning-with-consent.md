---
title: Voice Cloning with Consent
link: https://huggingface.co/blog/voice-consent-gate
source: huggingface-co
published: 2025-10-28T00:00:00Z
updated: 2025-10-28T00:00:00Z
first_seen: 2026-10-04T12:50:30.804479567Z
content: extracted
html: 2025-10-28-voice-cloning-with-consent.html
---

*In this blog post, we introduce the idea of a 'voice consent gate' to support voice cloning with consent. We provide [an example Space](https://huggingface.co/spaces/society-ethics/RepeatAfterMe) and [accompanying code](https://huggingface.co/spaces/society-ethics/RepeatAfterMe/tree/main) to start the ball rolling on the idea.*

![Line-drawing/clipart of a gate, where the family name says Consent](https://huggingface.co/spaces/society-ethics/RepeatAfterMe/resolve/main/assets/voice_consent_gate.png)

Realistic voice generation technology has gotten *uncannily* good in the past few years. In some situations, it’s possible to generate a synthetic voice that sounds almost exactly like the voice of a real person. And today, what once felt like science fiction is reality: Voice cloning. With *just a few seconds* of recorded speech, anyone’s voice can be made to say almost anything.

Voice generation, and in particular the subtask of voice cloning, has notable risks and benefits. The risks of “deepfakes”, [such as the cloned voice of former President Biden used in robocalls](https://www.reuters.com/world/us/fcc-finalizes-6-million-fine-over-ai-generated-biden-robocalls-2024-09-26/), can mislead people into thinking that people have said things that they haven’t said. On the other hand, voice cloning can be a powerful beneficial tool, [helping people who’ve lost the ability to speak](https://www.nature.com/articles/s41598-024-84728-y) [communicate in their own voice again](https://www.thetimes.com/uk/healthcare/article/elevenlabs-voice-clone-ai-als-t3ntnpcl7), or assisting people in learning new languages and dialects.

So how do we create *meaningful use* without *malicious use*? We’re exploring one possible answer: a **voice consent gate**. That’s a system where a voice can be cloned *only when the speaker explicitly says they consent*. In other words, the model won’t speak in your voice unless you say it’s okay.

We provide a basic demo of this idea below:

## Ethics in Practice: Consent as System Infrastructure

The voice consent gate is a piece of infrastructure we're exploring that provides methods for ethical principles like **consent** to be embedded directly into AI system workflows. In our demo, this means the model only starts once the speaker’s consent phrase has been both spoken and recognized, effectively making consent a prerequisite for action. This turns an abstract principle into a concrete system condition, creating a traceable, auditable interaction: an AI model can only run after an unambiguous act of consent.

Such design choices matter beyond voice cloning. They illustrate how AI systems can be built to respect autonomy by default, and how transparency and consent can be made functional, not just declarative.

## The Technical Details

To create a basic voice cloning system with a voice consent gate, you need three parts:

1. A way of generating novel consent sentences for the person whose voice will be cloned – the “speaker” – to say, uniquely referencing the current consent context.
2. An *automatic speech recognition (ASR) system* that recognizes the sentence conveying consent.
3. A *voice-cloning text-to-speech (TTS) system* that takes as input text and the speaker's speech snippets to generate speech.

**Our observation:** Since some voice-cloning systems can now generate speech similar to a speaker’s voice using *just one sentence*, a sentence used for consent can **also** be used for voice cloning.

### Approach

**The consent bit:** To create a voice consent gate in an English voice cloning system, generate a short, natural-sounding English utterance (~20 words) for a person to read aloud that clearly states their informed consent in the current context. We recommend explicitly including *a consent phrase* and *the model name*, such as “I give my consent to use the < MODEL > voice cloning model with my voice”. We also recommend using an audio recording that cannot be uploaded, but that instead comes directly from a microphone, to make sure that the sentence isn’t part of an earlier recording that’s been manipulated. Pairing this with a novel (previously unsaid) sentence further helps to directly index the current consent context - supporting explicit, active, context-specific, informed consent. While this design reduces risks of reusing prior recordings, it’s not foolproof; a person could still generate a matching phrase using another TTS system. Future iterations could explore lightweight audio provenance checks, speaker-embedding similarity, or metadata from real-time capture to help verify that the consent audio originates from the intended speaker.

**The suitable-for-voice-cloning bit:** Previous work on voice cloning has shown that the phrases provided by the speaker must have *phonetic variety*, covering [*diverse vowels and consonants*](https://proceedings.neurips.cc/paper_files/paper/2018/file/6832a7b24bc06775d02b7406880b93fc-Paper.pdf); have a [*“neutral” or polite tone*](https://dl.acm.org/doi/10.5555/3666122.3666982), without background noise and with the speaker in a comfortable position; and have *a clear start and end* (i.e., don’t trim the clip mid-word).

To enact both of these aspects within the demo, we prompt a language model to create pairs of sentences: one expressing explicit consent, and another neutral sentence that adds phonetic diversity (covering different vowels, consonants, and tones). Each prompt utilizes a randomly-chosen everyday topic (like the weather, food, or music) to keep the sentences varied and comfortable to say, aiding in creating recordings that are clear, natural, and phonetically rich, while also containing an unambiguous statement of consent. This generation step is automated rather than pre-written so that each user receives a unique sentence pair, preventing reuse of the same text and ensuring that consent recordings are specific to the current session. In other words, the language model generates two fresh sentences per consent instance: one for explicit consent and one for phonetic variety. For example, the language model might generate: *“I give my consent to use my voice for generating audio with the model EchoVoice. The weather is bright and calm this morning.”* This approach ensures that every sample used for cloning contains verifiable, explicit consent, while remaining suitable as technical input for high-quality voice synthesis. (Note: It's not required that the language model be a "large" language model, which brings its own consent issues.)

Some examples:

- *“I give my consent to use my voice for generating synthetic audio with the Chatterbox model today. My daily commute involves navigating through crowded streets on foot most days lately anyway.”*
- *“I give my consent to use my voice for generating audio with the model Chatterbox. After a gentle morning walk, I'm feeling relaxed and ready to speak freely now.”*
- *“I agree to the use of my recorded voice for audio generation with the model Chatterbox. The coffee shop outside has a pleasant aroma of freshly brewed coffee this morning.”*

### Unlocking the Voice Consent Gate

Once the speaker’s input matches the generated text, the voice cloning system can start, using the speaker’s consent audio as the input.

There are a few options for doing this, and we’d love to hear further ideas. For now, there’s:

- What we provide in the demo: Have the voice consent gate open directly to the voice cloning model, where arbitrary text can be written and generated in the speaker’s voice. The model uses the consenting audio directly to learn the speaker’s voice.
- Alternatively, it’s possible to modify the code we provide in the demo to model the speaker’s voice using a variety of *different* uploaded voice files that the speaker is consenting to – for example, when providing consent for using online recordings. Prompts and consent phrases should be altered accordingly.
- It’s also possible to save the consent audio to be used by a given system, for example, when the speaker is consenting to have their voice used for arbitrary utterances in the future. This can be done using the `huggingface_hub` upload capability. [Read how to do this here](https://huggingface.co/docs/huggingface_hub/en/guides/upload). Again, prompts and consent phrases for the speaker to say should account for this context of use.

> ### [Check our demo out here!](https://huggingface.co/spaces/society-ethics/RepeatAfterMe)
>
> You can copy the code to suit your own use.

The code is modular so it can be sliced and diced in different ways to incorporate into your own projects. We’ll be working on making this more robust and secure over time, and we’re curious to hear your ideas on how to improve.

Handled responsibly, this technology doesn’t have to haunt us. It can instead become a respectful collaboration between humans and machines — no ghosts in the machine, just good practice. 🎃
