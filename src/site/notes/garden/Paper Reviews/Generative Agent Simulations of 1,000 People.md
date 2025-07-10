---
{"dg-publish":true,"permalink":"/garden/paper-reviews/generative-agent-simulations-of-1-000-people/"}
---

## Generative Agent Simulations of 1,000 People

### 1. Objective
- Simulate **over 1,000 real individuals** using **LLMs + qualitative interviews**.
- Enable **human-like behavioral simulations** across surveys, games, and experiments.
- Evaluate agents by comparing their responses to participants’ **actual responses**, normalized by the **participant's own test-retest reliability**.

### 2. Methodology

#### Interview Collection
- Stratified sampling of 1,052 U.S. participants across demographic dimensions.
- Two-hour **voice-to-voice interview** conducted by an **AI interviewer**.
- Interview based on the **American Voices Project** script for broad topical coverage.

#### AI Interviewer Architecture
- Conducts semi-structured interviews.
- Decides whether to ask follow-up questions or move to the next script item.
- Uses a **reflection module** to synthesize participant insights in real time.
- Implements **audio interface** using OpenAI’s TTS and Whisper models for naturalistic interaction.
### 3. Generative Agent Architecture

#### Memory System
- Agents' memory consists of:
  - Full **interview transcripts**
  - **Expert reflections** (domain-specific summaries)

#### Expert Reflection
- Prompted using personas of:
  - **Psychologist**
  - **Behavioral economist**
  - **Political scientist**
  - **Demographer**

Each reflection captures different behavioral, ideological, or demographic aspects of the participant.

#### Response Generation
- When prompted (e.g., survey question), agent performs:
  - **Step-by-step reasoning** over options.
  - Retrieval of **relevant expert reflections**.
  - Final prediction output using **GPT-4o**.
### 4. Evaluation

#### Tasks Evaluated
- **General Social Survey (GSS)**
- **Big Five Personality Inventory (BFI-44)**
- **Behavioral economic games** (e.g., Dictator, Trust, Prisoner’s Dilemma)
- **Five experimental replications**

#### Metric
- **Normalized accuracy or correlation** = Agent performance / Human self-consistency (2-week retest).

#### Results
| Task | Metric | Interview-based Agents | Demographic-based | Persona-based |
|------|--------|------------------------|-------------------|---------------|
| GSS | Accuracy | **0.85** | 0.71 | 0.70 |
| BFI | Corr.    | **0.80** | 0.55 | 0.75 |
| Econ Games | Corr. | **0.66** | 0.31 | lower |

- Agents replicated **4/5 social science experiments**—same as human participants.
- **Effect sizes between humans and agents:** Correlation = **0.98**.

### 5. Ablation and Comparative Studies

#### Interview Ablation
- Even with **80% of the interview removed**, agents outperform baselines.

#### Interview Summary Test
- Summarized versions (bullet point facts) still outperform demographic and persona-based agents.

#### Composite Agent Test
- Agents trained on survey data (not interviews) underperform interview-informed agents.

### 6. Bias and Fairness

- Used **Demographic Parity Difference (DPD)** across subgroups:
  - **Political ideology**
  - **Race**
  - **Gender**

#### Findings
- Interview-based agents significantly **reduce DPD** compared to demographic-based agents.

### 7. Agent Bank Access

#### Two Access Tiers
- **Open access**: Aggregated data for general use.
- **Restricted access**: Individual-level responses, under research agreements.
- **Safeguards**: Consent process, withdrawal option, audit trails.

### 8. Contributions & Significance

- Demonstrates that **LLMs + interviews** can generate agents that:
  - Simulate individuals with **high fidelity**.
  - Reflect diverse human behaviors and ideologies.
- Proposes a **scalable method** to simulate **policy testing, social experiments, and behavioral research** at scale.

### 9. Limitations & Future Work
- Model outputs still reflect some LLM biases.
- Over-reliance on textual inferences (e.g., lack of multimodal cues).
- Future work to explore richer memory systems, more nuanced agent behavior, and longitudinal changes.



# Thoughts: 

The entire interview transcript is injected into the model prompt(average number of words: 6k). This will result in a significant increase in costs for API usage. It would be better if they put the transcipt into a text summariser or note generator. It may wont work as well as the raw interview, but it will be cheaper to do

for experiments requiring more multiple decision making steps, agents were given memory of the previous stimuli in the form of short text descriptors.

The main difference from the earlier implmentation of the simulacra is that this time they are trying not simulate real people. 
The strength of simulation is based on a few factors: 
- How the model responds to the same questions? Does it replicate what the person answered
- The participating people were asked to complete the same interview again after 2 weeks
- The output from the model, the original interview answers and the new interview answers were all clubbed together to get a final normalised score

## Predicting an individuals attitures and behaviours: 
There are two approached that were carried out: 
- Demographic attributes: 
	- Atttributes such as the age, gender, political outlook etc are captured based on teh GSS (General Social Survery) questions.
- Paragraph summarisation of the person (persona based):
	- Done by making the participant write a paragraph about themseles after the interview. This is similar to the how they did this in the their previous paper: [[garden/Paper Reviews/Generative Agents Interactive Simulacra of Human Behavior\|Generative Agents Interactive Simulacra of Human Behavior]].


## How the evaluation is done: 
- GSS: 177 questions that offer a quantifiable outcome. These serve as the benchmark against which the models simulated outcomes are compared against.
- Big 5 personality traits: BFI-44


My main concern with this is, how secure is the evaluation? I feel like there is a high likelihood of data leakage in the form of the thematic similarities between the questions that are asked. The authors did address that the interview and the "testing" questionnaires are completely different, but they do overlap thematically. 

Ok so one of methods that they used was the point that I brought up in the beginning of this section, which was they could use a text summarisation model,  which they did and apparently it performed rather well.

Randomly removing 80% of the interview only affected the GSS core from an average of 0.85 to 0.79, and text summarisation was less slightly lesser, reducing the core to just 0.83. 