# Swing Launch Content — Ready to Post

## 1. Reddit r/ClaudeAI

### Title Options (pick one):
- **A)** "I asked Claude if SQLite could handle 1000 users. It said no. It was wrong. So I built a skill that forces it to verify."
- **B)** "Claude said my JWT auth code 'looks good.' It missed a Critical security flaw. Here's the skill that catches what Claude won't."
- **C)** "Made 6 skills that fix the 6 ways Claude thinks poorly — hallucination, anchoring, confirmation bias, and more"

### Body:

I've been using Claude Code daily for 6 months. It's great at writing code, but terrible at *thinking about* code.

**Example 1: Confidently wrong research**

I asked: "Is SQLite viable for an app with 1000 concurrent users?"

Claude said: "No, SQLite is not suitable for high-concurrency applications. Use PostgreSQL or MySQL instead."

This is wrong. 1000 concurrent users ≠ 1000 concurrent writes. Typical web apps at this scale generate ~30 concurrent write transactions. SQLite handles ~120 writes/sec in WAL mode. Expensify runs 10M+ users on SQLite.

Claude didn't check. It just gave the "safe" answer.

**Example 2: Missing a Critical security flaw**

I asked Claude to review a JWT auth module. It said: "The code looks well-structured. Consider adding more comments."

With my skill, it found:
- 🔴 **Critical**: No refresh token rotation — a stolen token gives 7-day persistent access with zero detection
- 🟠 **Major**: Rate limiting uses in-memory Map() — resets on restart, bypassed in multi-instance deploys

**What I built**

I identified 6 distinct cognitive failures in AI coding agents and built a "firewall" skill for each one:

| Failure | Skill | What it forces |
|---------|-------|----------------|
| Premature closure | swing-clarify | Asks clarifying questions before executing |
| Hallucination | swing-research | Cross-verifies claims with source tier grading |
| Confirmation bias | swing-review | Steel-man then 3-vector attack |
| Anchoring bias | swing-options | 5 probability-weighted alternatives |
| Black-box reasoning | swing-trace | Assumption inventory + weakest link |
| Optimism bias | swing-mortem | Assumes failure, works backward |

Install: `npx skills add whynowlab/swing-skills --all`

MIT licensed, works with Claude Code + other agents via Agent Skills standard.

Repo: https://github.com/whynowlab/swing-skills

Happy to answer questions about the cognitive failure framework or how any of the skills work.

---

## 2. Show HN

### Title:
```
Show HN: Swing – Six cognitive firewalls for AI coding agents
```

### Body:
```
Swing installs 6 skills that defend against specific AI reasoning failures.

The problem: AI coding agents are confident but think poorly. Ask Claude about SQLite scaling — it says "use PostgreSQL" without checking. Ask it to review your auth code — it says "looks good" and misses a Critical security flaw.

I mapped 6 distinct cognitive failures (hallucination, anchoring, confirmation bias, premature closure, black-box reasoning, optimism bias) and built a structured skill for each:

- swing-clarify: 5W1H decomposition before execution
- swing-research: 4-stage pipeline with source tier grading (S/A/B/C)
- swing-review: steel-man then 3-vector adversarial attack
- swing-options: probability-weighted alternatives (conventional → wild card)
- swing-trace: assumption inventory with weakest-link analysis
- swing-mortem: prospective failure analysis with circuit breakers

Each skill has explicit quality calibration with BAD and GOOD output examples, so the AI knows exactly what's expected.

Install: npx skills add whynowlab/swing-skills --all

Works with Claude Code, Cursor, Copilot via Agent Skills standard. MIT licensed.

https://github.com/whynowlab/swing-skills
```

---

## 3. X/Twitter Thread (7 tweets)

### Tweet 1 (Hook):
```
Your AI is not swinging at all.

It picks the safe default every time. Agrees when it should push back. Makes up answers it can't verify.

I mapped 6 cognitive failures and built a firewall for each one.

🧵 Thread:
```

### Tweet 2 (SQLite flip):
```
FAILURE #1: Hallucination

"Is SQLite viable for 1000 users?"

Default AI: "No. Use PostgreSQL."

With swing-research: "Yes. 1000 users ≠ 1000 writers. SQLite handles ~120 writes/sec in WAL mode."

Completely opposite conclusion. The skill forced source verification.
```

### Tweet 3 (JWT miss):
```
FAILURE #2: Confirmation bias

"Review this JWT auth module"

Default AI: "Looks good. Add comments."

With swing-review:
🔴 Critical: No refresh token rotation = 7-day persistent access for attackers
🟠 Major: Rate limiting in-memory Map() = bypassed on restart

The baseline found nothing. The skill found a Critical vulnerability.
```

### Tweet 4 (Full list):
```
6 failures. 6 firewalls:

swing-clarify → stops premature execution
swing-research → blocks hallucination
swing-review → kills confirmation bias
swing-options → breaks anchoring
swing-trace → exposes hidden assumptions
swing-mortem → counters blind optimism

Each one independent. Chain them for full rigor.
```

### Tweet 5 (How it works):
```
Each skill has:
- Absolute rules (what's banned)
- Structured pipeline (step by step)
- BAD/GOOD calibration examples
- Integration notes for chaining

Not vibes. Structure.
```

### Tweet 6 (Install):
```
Install in 30 seconds:

npx skills add whynowlab/swing-skills --all

Works with Claude Code, Cursor, Copilot.
MIT licensed. 6 skills. Zero config.
```

### Tweet 7 (CTA):
```
"Your AI is not swinging at all."

Make it swing.

github.com/whynowlab/swing-skills
```

---

## 4. Dev.to Blog Post

### Title:
```
I built 6 cognitive firewalls because my AI kept confidently giving wrong answers
```

### Tags:
`claudecode`, `ai`, `productivity`, `opensource`

### Body:

# I built 6 cognitive firewalls because my AI kept confidently giving wrong answers

I use Claude Code every day. It writes code fast. But it *thinks* poorly.

Not always. Not obviously. That's what makes it dangerous.

## The moment I realized something was wrong

I asked Claude: "Is SQLite viable for an app with 1000 concurrent users?"

It said: **"No, SQLite is not suitable for high-concurrency applications. Use PostgreSQL or MySQL instead for production workloads."**

Confident. Clear. Completely wrong.

1000 concurrent users ≠ 1000 concurrent writes. A typical web app at this scale generates about 30 concurrent write transactions. SQLite in WAL mode handles ~120 writes/sec. Expensify serves 10M+ users on SQLite.

Claude didn't check any sources. It just gave the "safe" answer — the one that sounds right because everyone else says it.

## Six failures, not one

I started paying attention. I noticed the same AI wasn't just hallucinating — it was failing in six distinct patterns:

1. **Premature closure**: Rushes to execute ambiguous requests instead of asking questions
2. **Hallucination**: States claims without verification
3. **Anchoring bias**: Locks onto the first "obvious" answer
4. **Confirmation bias**: Agrees with you instead of challenging
5. **Black-box reasoning**: Gives conclusions without showing assumptions
6. **Optimism bias**: Assumes the plan will work

Each one is a different cognitive failure. Each one needs a different structural fix.

## The fix: structured skills, not better prompts

I tried prompt engineering. "Be more careful." "Check your sources." "Consider alternatives."

It doesn't work. The AI nods, then does the same thing.

What works is *structure*. Force the AI through a pipeline that makes the failure impossible:

### swing-research (anti-hallucination)
Every claim must be traced to a specific source or labeled "Unverified." Sources get tier grading: S (academic), A (official), B (community — flagged), C (general). Key claims need 2+ independent sources.

### swing-review (anti-confirmation bias)
Before criticizing, the AI must articulate the *strongest case FOR* the current approach (steel-man). Then attack from 3 independent vectors. "Looks good" is structurally banned.

### swing-clarify (anti-premature closure)
5W1H decomposition of every request. Score ambiguity 0-6. If score > 0, generate up to 3 clarifying questions with multiple choice options and stated defaults. Clear requests get a green light.

### swing-options (anti-anchoring)
Generate exactly 5 options across a probability distribution. At least 1 must be unconventional or wild card. Expose the hidden assumptions behind the "obvious" choice.

### swing-trace (anti-black-box)
Every assumption gets rated for criticality and verifiability. Every decision fork shows what was rejected and why. Identify the single weakest assumption that, if wrong, changes everything.

### swing-mortem (anti-optimism)
"It's 6 months from now. This failed completely. What went wrong?" Force 5 failure scenarios across Technical / Organizational / External / Temporal / Assumption categories. Every scenario must name specific technologies and quantities.

## What changed

After the SQLite incident, I ran the same question through swing-research.

It found: sqlite.org WAL documentation showing write throughput. Expensify engineering blog confirming production use at scale. The conclusion flipped from "No" to "Yes, with caveats."

After the JWT review, swing-review found a Critical security vulnerability the baseline missed entirely — no refresh token rotation means a stolen token grants persistent 7-day access.

Not better answers. *Structurally different reasoning.*

## Try it

```bash
npx skills add whynowlab/swing-skills --all
```

Six skills. Each targets one cognitive failure. Use one, or chain them.

MIT licensed. Works with Claude Code, Cursor, Copilot.

**GitHub: [whynowlab/swing-skills](https://github.com/whynowlab/swing-skills)**

---

## 5. GeekNews (한국어)

### Title:
```
Swing – AI 코딩 에이전트를 위한 6개의 인지 방화벽
```

### URL:
```
https://github.com/whynowlab/swing-skills
```

### Comment (첫 댓글):
```
Claude Code를 매일 쓰면서 발견한 건, AI가 코드를 '쓰는' 건 잘하지만 '생각하는' 건 못한다는 점이었습니다.

SQLite가 1000명 동시접속을 감당할 수 있냐고 물었더니 "안 됩니다. PostgreSQL 쓰세요"라고 자신있게 답했는데, 이건 틀린 답이었습니다. 1000 동시접속 ≠ 1000 동시 쓰기. WAL 모드에서 SQLite는 초당 ~120 쓰기를 처리합니다.

이런 식의 '인지적 실패'를 6가지로 분류하고, 각각에 대한 구조적 방어 스킬을 만들었습니다:

- swing-clarify: 모호한 요청을 명확화 (조기결론 방지)
- swing-research: 소스 검증 + 교차확인 (할루시네이션 차단)
- swing-review: 반론 3축 공격 (확증편향 격파)
- swing-options: 확률가중 대안 5개 (앵커링 탈출)
- swing-trace: 가정 목록 + 최약점 분석 (블랙박스 해체)
- swing-mortem: 사전 실패 분석 (낙관편향 분쇄)

Claude Code, Cursor, Copilot 호환. MIT 라이선스.

설치: npx skills add whynowlab/swing-skills --all
```

---

## 6. Claude Code Discord

### Channel: #show-your-work

### Message:
```
Been working on something that changed how I use Claude Code.

I noticed Claude has 6 distinct reasoning failures — and "be more careful" prompts don't fix them. So I built structured skills that make each failure structurally impossible.

Quick example: I asked Claude if SQLite handles 1000 concurrent users. It said "No, use PostgreSQL." Wrong — it didn't check any sources.

With swing-research, it found WAL mode handles ~120 writes/sec, and Expensify runs 10M+ users on SQLite. Completely different conclusion because it was *forced* to verify.

6 skills, each targeting one cognitive failure:
- swing-clarify (premature closure)
- swing-research (hallucination)
- swing-review (confirmation bias)
- swing-options (anchoring)
- swing-trace (black-box reasoning)
- swing-mortem (optimism bias)

npx skills add whynowlab/swing-skills --all

Would love feedback from anyone who tries it.
https://github.com/whynowlab/swing-skills
```

---

## Posting Schedule (Recommended)

| Day | Channel | Content |
|-----|---------|---------|
| Mon | Claude Discord | #show-your-work message |
| Tue | Reddit r/ClaudeAI | Full post (Title A recommended) |
| Wed | X/Twitter | 7-tweet thread |
| Thu | Show HN | Morning 8-10am EST |
| Fri | GeekNews | 한국어 제출 |
| Next week | Dev.to | Blog post |
