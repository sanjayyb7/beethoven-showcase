# Beethoven

**Drop a painting onto a musical score and a live AI band plays it.**
Every bar, [Jev](https://typesafe.ai) by TypeSafe AI conducts: it decides how the band should play next, and Google's Lyria RealTime turns those decisions into live, generated music.

Built at the CodeRabbit hackathon.

🔊 **Turn the sound on.**

## How it works

<!-- 🎬 YouTube demo goes here -->

1. **Gemini looks at each painting.** It reads the mood, the sounds it suggests and its cultural roots. *The Great Wave* comes back as Edo-period Japanese music (koto, taiko, shakuhachi). A Rajasthani miniature with peacocks comes back as Hindustani classical, with sitar, bansuri and a matching raga.
2. **You compose by placing paintings on the score.** Left to right is *when* a painting plays, height is *how high* it sounds, and size is *how strongly* its mood leads.
3. **Jev conducts every bar.** A playhead sweeps across the score. Each time it crosses a bar line, Jev is told what's playing, what's coming next and what just happened. It answers up to 11 questions in parallel, in about 200 ms, and the music changes exactly on the next bar line.
4. **Lyria RealTime plays it.** Jev's decisions become weighted prompts and settings for Google's live music model, streamed to the browser, with a live-coded synth band as an instant fallback.

```
painting ──► Gemini vision ──► mood · sounds · instruments · culture
                                              │
where you put it (time · height · size) ──► the scene, in words ──► Jev ──► decisions
                                                                             │
                                                           Lyria RealTime ──► live music
```

## What Jev decides

Every bar, Jev chooses:

| Decision | Options |
|---|---|
| How loud each painting plays | off · low · medium · high · keep |
| Mood | dark · neutral · bright · keep |
| Energy | sparse · medium · busy · keep |
| Drums, bass | on · off · keep |
| Lead instrument | flute · violin · cello · piano · guitar · none |
| Two textures | from 8 candidate sounds for the scene |
| Transition | build up · fade down · sudden cut · keep going |
| Whose culture leads | when paintings from different cultures overlap |

Each answer comes back as a typed choice with a confidence and a probability for every option.

## Why Jev, and not fixed rules

Fixed rules match keywords; Jev reads the whole scene.

- **Mixed signals:** *The Starry Night* is described as "turbulent, luminous, dreamy, restless". Rules see "restless" and go dark; Jev weighed it and chose **neutral (53%)**, with bright at 39%.
- **Overlapping cultures:** a large *Shakuntala* over a smaller Van Gogh. Rules take the first painting in the list; Jev chose **Indian (Hindustani) at 89–99%**, weighing size, dominance and context.
- **Words nobody wrote a rule for:** new uploads and audience stickers ("lost at sea") change the music meaningfully.
- **Continuity:** Jev can "keep" a choice instead of flipping on every keyword, so the band sounds conducted rather than switched.

Measured: a full 11-question bar has a median of **204 ms** (the budget is about 2.5 s), with **0 missed bars** in long sessions, at about **$0.00004 per bar**.

## See it's real

A live log inside the app shows every real call as it happens: Jev's HTTP status, server model version (`jev-1.13.0`), latency and full responses; each Gemini call; and the audio streaming back from Lyria.

## Looks

Two paper looks, **black paper** and **woven parchment**, generated in the browser with grain that "breathes" across the sheet. While you drag a painting, a preview shows exactly where it will land.

## Built with

- **[Jev](https://typesafe.ai)** by TypeSafe AI: the conductor
- **Gemini** by Google: seeing each painting's mood and culture
- **Lyria RealTime** by Google: live generated music
- **[Strudel](https://strudel.cc)**: the live-coded fallback band
- Preset paintings are public domain, from Wikimedia Commons: Hokusai, Monet, Van Gogh, Bruegel, Seurat, Rembrandt and Raja Ravi Varma

---

*This repository is a showcase. The source code is private. © All rights reserved.*
