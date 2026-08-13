# Limn Design Contract

This document defines the interface, interaction, and visual system of Limn. It is a decision system derived from the Adaptive Product Design Operating System, tailored to agentic workflows and developer tool interfaces.

---

## 1. Core Principles

Our design quality is evaluated by a single formula:

$$
\text{Design Quality}
=
\text{Useful Outcomes} + \text{Clarity} + \text{Reliability} + \text{Trust} + \text{Efficiency} + \text{Character}
-
\text{Avoidable Friction}
$$

1. **Utility First**: If a design element doesn't help the user (or the agent guiding them) achieve their outcome, delete it.
2. **Extreme Clarity**: Interfaces must explain *why* something is happening, not just *what*. In agent-driven systems, logs and machine-readable states are as important as visual indicators.
3. **Low Interaction Cost**: Minimize physical steps, decision burden, and memory strain. 
   - $$\text{Interaction Cost} = \text{Steps} + \text{Decision Burden} + \text{Waiting} + \text{Memory Burden} + \text{Error Recovery}$$
4. **Anti-Fashion Invariance**: No glassmorphism, no heavy gradients, no rounded corners for the sake of aesthetics. Visual styling must serve a clean, technical editorial identity.
5. **Character over Slop**: Lean on strong layout grid alignment, high-contrast monospace highlights, and clean editorial type scales (Fraunces + Inter).

---

## 2. Visual & Semantic Tokens

Limn uses a "deep studio console" visual identity. 

### 2.1 The Palette
- **Background (Apex)**: `#07090f` (deep blue-black)
- **Surface (Primary Panel)**: `#10141f`
- **Surface (Secondary)**: `#0c1018`
- **Borders/Lines**: `rgba(148, 163, 199, 0.10)`
- **Strong Borders**: `rgba(148, 163, 199, 0.18)`
- **Text (Ink)**: `#e7ecf6`
- **Muted Text**: `#93a0ba`
- **Dimmed Details**: `#55617c`
- **Primary Accent (Iris)**: `#6d5ef0` (used for active tabs, selected states, highlights)
- **Accent Glow**: `#8f83ff`
- **Warning/Danger (Coral)**: `#ff6b6b` (used for errors, panic states, record indicators)
- **Success (Teal)**: `#3ee0c8` (used for successful compilations, active states, active meters)

### 2.2 Typography
- **Headings & Brands**: *Fraunces* (Editorial Serif) for character and identity.
- **Body & Controls**: *Inter* (Clean Sans-Serif) for legible density.
- **States & Code**: Monospace (SF Mono / JetBrains Mono) for precise metrics.

---

## 3. The 15 State Model

Every control, row, or interactive card in Limn must design for and handle the following states:

1. **Default**: Sane rest state. Low visual weight.
2. **Hover**: Indication of touchability. Shift background/borders slightly.
3. **Focus**: Explicit keyboard capture indicator (ring or high-contrast outline).
4. **Active**: The moment of click/press. Subtle translation or scale shift.
5. **Selected**: PERSISTENT active state (e.g., active tab). Accent borders or fills.
6. **Disabled**: Clarifies *why* it is disabled, not just lower opacity.
7. **Loading**: Spinning/pulse indicator. Keeps layout stable to avoid shifting.
8. **Success**: Clear green/teal confirmation, auto-expiring where appropriate.
9. **Warning**: Coral border, alerts user of boundary conditions.
10. **Error**: Red/danger border + actionable recovery instructions.
11. **Empty**: Explains what goes here, how to create it, and the smallest first step.
12. **Partial**: Sane defaults for missing values.
13. **Offline**: Indicates sync state and whether local changes will persist.
14. **Stale**: Soft indicators for out-of-date telemetry.
15. **Permission Denied**: Clear instructions on how to authenticate or request scope.

---

## 4. Critical Flow Template

Every feature in Limn must map onto this critical flow template before implementation:

```text
USER INTENT (What are they trying to do?)
  ↓
ENTRY POINT (How do they discover it?)
  ↓
DECISION BURDEN (What fields must they fill? Keep to < 3 by default)
  ↓
FAILURE MODES & RECOVERY (What can go wrong? How do they fix it?)
  ↓
OPTIMISTIC FEEDBACK (Can we update the UI before the roundtrip finishes?)
  ↓
SUCCESS SIGNAL (How do they know it worked?)
```

---

## 5. Anti-Cargo-Cult Design Rules

- **No Icon-Only Controls**: Controls must have clear text labels. An icon alone is a cognitive burden.
- **No Hidden Controls**: Do not hide key functions behind hover states to look "clean". If it's useful, keep it visible.
- **No Fashion Gradients**: Gradients are forbidden unless representing a physical mapping (e.g., audio meter intensity).
- **Undo over Confirmation**: For high-frequency actions, execute immediately and provide an "Undo" toast. Do not block the flow with confirmation alerts.
- **Responsive Priority**: When a screen shrinks, do not just scale down. Hide secondary details and prioritize the core task controls.
