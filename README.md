# Humanizer

A skill for Claude Code and OpenCode that removes signs of AI-generated writing from text, making it sound more natural and human. Includes a **Research Writing Mode** for academic and scientific text, with AI detection scoring, planning tools, and literature review rules.

## Installation

### Claude Code

Clone directly into Claude Code's skills directory:

```bash
mkdir -p ~/.claude/skills
git clone https://github.com/kandaiiskandar/humanizer.git ~/.claude/skills/humanizer
```

Or copy the skill files manually if you already have this repo cloned:

```bash
mkdir -p ~/.claude/skills/humanizer
cp SKILL.md ai-detection.md ~/.claude/skills/humanizer/
```

### OpenCode

Clone directly into OpenCode's skills directory:

```bash
mkdir -p ~/.config/opencode/skills
git clone https://github.com/kandaiiskandar/humanizer.git ~/.config/opencode/skills/humanizer
```

Or copy the skill files manually if you already have this repo cloned:

```bash
mkdir -p ~/.config/opencode/skills/humanizer
cp SKILL.md ai-detection.md ~/.config/opencode/skills/humanizer/
```

> **Note:** OpenCode also scans `~/.claude/skills/` for compatibility, so a single clone into `~/.claude/skills/humanizer/` works for both tools.

## Usage

### Claude Code

```
/humanizer

[paste your text here]
```

### OpenCode

```
/humanizer

[paste your text here]
```

Or ask the model to humanize text directly in either tool:

```
Please humanize this text: [your text]
```

### Research Writing Mode

For academic or scientific writing, the skill activates a planning phase and additional rules:

```
/humanizer

[paste your academic text here]
```

The skill detects academic context automatically and applies:
- AI detection scoring (before and after)
- Research voice rules (observational argument, scoped claims, section bridging)
- Academic literature review checklist

### Voice Calibration

To match your personal writing style, provide a sample of your own writing:

```
/humanizer

Here's a sample of my writing for voice matching:
[paste 2-3 paragraphs of your own writing]

Now humanize this text:
[paste AI text to humanize]
```

The skill will analyze your sentence rhythm, word choices, and quirks, then apply them to the rewrite instead of producing generic "clean" output.

## Overview

Based on [Wikipedia's "Signs of AI writing"](https://en.wikipedia.org/wiki/Wikipedia:Signs_of_AI_writing) guide, maintained by WikiProject AI Cleanup. This comprehensive guide comes from observations of thousands of instances of AI-generated text.

The skill also includes:
- A final "obviously AI generated" audit pass and second rewrite, to catch lingering AI-isms in the first draft
- **AI detection scoring** — before and after editing, using perplexity, burstiness, lexical diversity, stylistic markers, and evidence density
- **Research Writing Mode** — planning phase, research voice rules (R1–R7), and academic literature review checklist

### Key Insight from Wikipedia

> "LLMs use statistical algorithms to guess what should come next. The result tends toward the most statistically likely result that applies to the widest variety of cases."

## 33 Patterns Detected (with Before/After Examples)

### Content Patterns

| # | Pattern | Before | After |
|---|---------|--------|-------|
| 1 | **Significance inflation** | "marking a pivotal moment in the evolution of..." | "was established in 1989 to collect regional statistics" |
| 2 | **Notability name-dropping** | "cited in NYT, BBC, FT, and The Hindu" | "In a 2024 NYT interview, she argued..." |
| 3 | **Superficial -ing analyses** | "symbolizing... reflecting... showcasing..." | Remove or expand with actual sources |
| 4 | **Promotional language** | "nestled within the breathtaking region" | "is a town in the Gonder region" |
| 5 | **Vague attributions** | "Experts believe it plays a crucial role" | "according to a 2019 survey by..." |
| 6 | **Formulaic challenges** | "Despite challenges... continues to thrive" | Specific facts about actual challenges |

### Language Patterns

| # | Pattern | Before | After |
|---|---------|--------|-------|
| 7 | **AI vocabulary** | "Actually... additionally... testament... landscape... showcasing" | "also... remain common" |
| 8 | **Copula avoidance** | "serves as... features... boasts" | "is... has" |
| 9 | **Negative parallelisms / tailing negations** | "It's not just X, it's Y", "..., no guessing" | State the point directly |
| 10 | **Rule of three** | "innovation, inspiration, and insights" | Use natural number of items |
| 11 | **Synonym cycling** | "protagonist... main character... central figure... hero" | "protagonist" (repeat when clearest) |
| 12 | **False ranges** | "from the Big Bang to dark matter" | List topics directly |
| 13 | **Passive voice / subjectless fragments** | "No configuration file needed" | Name the actor when it helps clarity |

### Style Patterns

| # | Pattern | Before | After |
|---|---------|--------|-------|
| 14 | **Em dash overuse** | "institutions—not the people—yet this continues—" | Prefer commas or periods |
| 15 | **Boldface overuse** | "**OKRs**, **KPIs**, **BMC**" | "OKRs, KPIs, BMC" |
| 16 | **Inline-header lists** | "**Performance:** Performance improved" | Convert to prose |
| 17 | **Title Case Headings** | "Strategic Negotiations And Partnerships" | "Strategic negotiations and partnerships" |
| 18 | **Emojis** | "🚀 Launch Phase: 💡 Key Insight:" | Remove emojis |
| 19 | **Curly quotes** | `said “the project”` | `said “the project”` |
| 26 | **Hyphenated word pairs** | “cross-functional, data-driven, client-facing” | Drop hyphens on common word pairs |
| 27 | **Persuasive authority tropes** | "At its core, what matters is..." | State the point directly |
| 28 | **Signposting announcements** | "Let's dive in", "Here's what you need to know" | Start with the content |
| 29 | **Fragmented headers** | "## Performance" + "Speed matters." | Let the heading do the work |

### Communication Patterns

| # | Pattern | Before | After |
|---|---------|--------|-------|
| 20 | **Chatbot artifacts** | "I hope this helps! Let me know if..." | Remove entirely |
| 21 | **Cutoff disclaimers** | "While details are limited in available sources..." | Find sources or remove |
| 22 | **Sycophantic tone** | "Great question! You're absolutely right!" | Respond directly |

### Filler and Hedging

| # | Pattern | Before | After |
|---|---------|--------|-------|
| 23 | **Filler phrases** | "In order to", "Due to the fact that" | "To", "Because" |
| 24 | **Excessive hedging** | "could potentially possibly" | "may" |
| 25 | **Generic conclusions** | "The future looks bright" | Specific plans or facts |

### AI Detection Patterns

These carry the highest detection risk in academic writing and are prioritised first. Used by GPTZero, Turnitin AI, Copyleaks, and Originality.ai.

| # | Pattern | Before | After |
|---|---------|--------|-------|
| 30 | **Sequential "The" starts** | "The system uses JWKS. The token is validated. The secret is not stored." | Vary sentence openers |
| 31 | **Templated transitions** | "It is worth noting… This raises a deeper issue…" | State the point directly |
| 32 | **False precision** | "approximately 67.3% of studies" (no source) | "Most studies, according to [Source]" |
| 33 | **Parallel triples / binary oppositions** | "permit or block, act or defer, constrain or unconstrain" | "whether to act, and if so, within what constraints" |

## Examples

### Research Writing (Academic)

**Before (AI-sounding — academic):**
> The question of whether artificial intelligence (AI) is needed for decision making in safety-critical environments remains actively debated in the literature. On one hand, AI systems can process large volumes of heterogeneous data in real time, which improves situational awareness and supports faster responses under uncertainty (Ramos et al., 2024; Lee & See, 2004). This capability is particularly relevant in domains such as aviation, healthcare, and maritime operations, where human operators face cognitive overload and time pressure. On the other hand, reliance on AI introduces new risks, including model opacity, unpredictable failure modes, and the potential erosion of human judgment and accountability (Amodei et al., 2016; Suresh & Guttag, 2021). Existing studies show that AI can enhance decision support, but they also highlight that fully autonomous decision making remains problematic in high-risk settings due to the need for explainability, reliability, and trust (Parasuraman et al., 2000). As a result, current research increasingly argues for hybrid approaches, where AI provides advisory input while human operators retain final control, ensuring that safety constraints are not compromised by probabilistic model outputs (Endsley, 2017). This shift suggests that the key issue is not whether AI is needed, but how its role should be structured and governed within safety-critical decision processes.

**AI Detection baseline:** Estimated 40–50% AI. Perplexity: Low. Burstiness: Low. Patterns flagged: templated transition, -ing ending, vague attributions, negative parallelism, persuasive trope.

**After (humanized):**
> Whether AI belongs in safety-critical decision making is still contested. Systems that process heterogeneous data in real time can improve situational awareness and shorten response times under uncertainty (Ramos et al., 2024; Lee & See, 2004). This matters in aviation, healthcare, and maritime operations, where cognitive overload is a documented problem. But the risks are just as well-documented. Model opacity, unpredictable failure modes, and the erosion of human accountability appear consistently across the reviewed literature (Amodei et al., 2016; Suresh & Guttag, 2021). Parasuraman et al. (2000) concluded that fully autonomous systems are difficult to justify in high-risk settings because explainability and trust are requirements, not refinements. Most reviewed work points toward hybrid architectures, where AI provides input and humans retain final authority (Endsley, 2017). What remains open is how that participation should be bounded — the participation itself seems harder to avoid than to debate.

**AI Detection after:** Estimated 10–15% AI. Burstiness improved. No high-signal patterns remain. Citations preserved.

---

### General Writing

**Before (AI-sounding — general):**
> AI-assisted coding serves as an enduring testament to the transformative potential of large language models, marking a pivotal moment in the evolution of software development. In today's rapidly evolving technological landscape, these groundbreaking tools—nestled at the intersection of research and practice—are reshaping how engineers ideate, iterate, and deliver, underscoring their vital role in modern workflows.
>
> At its core, the value proposition is clear: streamlining processes, enhancing collaboration, and fostering alignment. It's not just about autocomplete; it's about unlocking creativity at scale, ensuring that organizations can remain agile while delivering seamless, intuitive, and powerful experiences to users. The tool serves as a catalyst. The assistant functions as a partner. The system stands as a foundation for innovation.
>
> Industry observers have noted that adoption has accelerated from hobbyist experiments to enterprise-wide rollouts, from solo developers to cross-functional teams. The technology has been featured in The New York Times, Wired, and The Verge. Additionally, the ability to generate documentation, tests, and refactors showcases how AI can contribute to better outcomes, highlighting the intricate interplay between automation and human judgment.
>
> - 💡 **Speed:** Code generation is significantly faster, reducing friction and empowering developers.
> - 🚀 **Quality:** Output quality has been enhanced through improved training, contributing to higher standards.
> - ✅ **Adoption:** Usage continues to grow, reflecting broader industry trends.
>
> While specific details are limited based on available information, it could potentially be argued that these tools might have some positive effect. Despite challenges typical of emerging technologies—including hallucinations, bias, and accountability—the ecosystem continues to thrive. In order to fully realize this potential, teams must align with best practices.
>
> In conclusion, the future looks bright. Exciting times lie ahead as we continue this journey toward excellence. Let me know if you’d like me to expand on any section!

**After (Humanized):**
> AI coding assistants can speed up the boring parts of the job. They're great at boilerplate: config files and the little glue code you don't want to write. They can also help you sketch a test, but you still have to read it.
>
> The dangerous part is how confident the suggestions look. I've accepted code that compiled and passed lint, then discovered later it missed the point because I stopped paying attention.
>
> If you treat it like autocomplete and review every line, it's useful. If you use it to avoid thinking, it will help you ship bugs faster.
>
> The only real backstop is tests. Without them, you're mostly judging vibes.

## References

- [Wikipedia: Signs of AI writing](https://en.wikipedia.org/wiki/Wikipedia:Signs_of_AI_writing) - Primary source
- [WikiProject AI Cleanup](https://en.wikipedia.org/wiki/Wikipedia:WikiProject_AI_Cleanup) - Maintaining organization

## Version History

- **2.6.0** - Added Research Writing Mode (planning phase, rules R1–R7, academic literature review checklist); added AI detection scoring (before/after); added 4 new high-detection-risk patterns (30–33)
- **2.5.1** - Added a passive-voice / subjectless-fragment rule, raising the total to 29 patterns
- **2.5.0** - Added patterns for persuasive framing, signposting, and fragmented headers; expanded negative parallelisms to cover tailing negations; tightened wording around em dash overuse; fixed frontmatter wording to use "filler phrases"
- **2.4.0** - Added voice calibration: match the user's personal writing style from samples
- **2.3.0** - Added pattern #25: hyphenated word pair overuse
- **2.2.0** - Added a final "obviously AI generated" audit + second-pass rewrite prompts
- **2.1.1** - Fixed pattern #18 example (curly quotes vs straight quotes)
- **2.1.0** - Added before/after examples for all 24 patterns
- **2.0.0** - Complete rewrite based on raw Wikipedia article content
- **1.0.0** - Initial release

## License

MIT
