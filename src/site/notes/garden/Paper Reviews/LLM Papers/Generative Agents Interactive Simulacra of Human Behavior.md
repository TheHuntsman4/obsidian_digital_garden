---
{"dg-publish":true,"permalink":"/garden/paper-reviews/llm-papers/generative-agents-interactive-simulacra-of-human-behavior/"}
---

[Link to the paper](https://arxiv.org/abs/2304.03442)


### Initial review on the abstract and the introduction

#### Abstract 
The point of having AI agents that simulate people could help in sociology realted scenrios, such as taking a survey, or for example in video games to make the experience more immersive. 

Another good example that we would be working on this summer is how we coudl come up with a system that would facilitate a more simpler scenario of people interactnig with each other for the first time

The paper presents an architecture for the agents with specific defined characteristics to interact with one another, allowing them to remember and plan the course and actions to be done the next day. They also describe some emergent behaviours being coming up during the course of the simulation.

Belivevable simulations for of human behaviour, via observation, planning and reflection done by the agents. the agents are a combination of LLMs and computations interactive agents

Questions that I have right now:
- How are the memories stored, does it use some sort of RAG or is there some sort of content digestion happening to make this process easier.
- How are they evaluating the entire architecture numerically? They mentioned that they are doing this using ablation, proving that each component of the thee part system: observation, planning, and reflection; are important and are interdependent.

#### Introduction

How this system is supposed to work:  "Success requires an approach that can retrieve relevant events and interactions over a long period, reflect on those memories to generalize and draw higher-level inferences, and apply that reasoning to create plans and reactions that make sense in the moment and in the longer-term arc of the agent’s behavior"

Model Architecture components:
- **Memory stream**: a long term memory module that stores the agents experineces using natural language
- **Reflection**:  Synthesises memories into hiugher level inferences that can be used to better guide the agents response/ behaviour
- **Planning**:  Get the inference made from reflection, combines it with the current environment to come up with a action and reactions that are then fed back into the memory stream for future reference.

Evaluation System
- Evaluation to test whether the agents produce believable individual behaviours in isolation.
- E2E test to check the stability of emergent behaviours in the agents over a 2 day in-game period.
- Technical Evaluation: interview the agents to check how much they could go on with the things that they had learned.

Known Issues:
- Agents have a tendency to go into formal speech patterns that is inherited from the LLM behind it.
- Agents have a tendency fail create false memories, fail to retrieve the associated memories.

![Pasted image 20250708193029.png](/img/user/images/Pasted%20image%2020250708193029.png)

### Methodology

- Have a total of 25 agents in total. Each agent is given a seed memory. in the form of a paragraph, which contains information like; the agents identity, occupation, relationship with other agents.
- At each time step in the sandbox world, each agent performs some action which is shown in hte form of emojis over the sprite. this is done using a language model. 
- Agents are aware of the others around them, and a generative model decides whether they walk past or interact with each other.
- All interactions take place in in natural language.
- The user can also direct the agent to do certain actions by playing out as their "inner voice".
- The user can also interact with the sandbox environment, but it is not the focus of the study. The interesting  feature being that the agents will change/ rework their current or future behavious based on the changes that the user introduced into the environment.
- Emergent social behaviours:
	- **Information diffusion**: How information is spread from one agent to the other. This happens via conversation among the agents
	- **Relationship Memory:** Agents form new relations as time goes on, based on retrieving the correct memories.
	- **Coordination:** Agents can coordinate amongst themselves to get certain tasks done. In the paper, a Valentines day party was planned out and executed by multiple agents interacting and coordinating with each other. The only user inputs given were the idea for planning the party and that the particular agent had an interest in another agent.

#### Memory and Retrieval
##### Memory Stream
- Contains a list of memory object.
- Each object contains: natural language description of the memory, creation time stamp, most recent access time stamp.
- The agent then retrieves the necessary memory using a memory function, that takes the current situation of the agent as the input and returns a subset of the memory stream.
- Three factors are used for the function:
	- **Recency:** Makes sure that more recent memory remain fresh in the agents attentional sphere. decay factor is 0.995.
	- **Importance:** core vs mundane memories, this is the prompt that they used :
		``On the scale of 1 to 10, where 1 is purely mundane (e.g., brushing teeth, making bed) and 10 is extremely poignant (e.g., a break up, college acceptance), rate the likely poignancy of the following piece of memory. Memory: buying groceries at The Willows Market and Pharmacy Rating:``
	- **Relevance:** Assigns a higher value to a memory that is important to the current situation. relevance is conditioned on a query memory. This was implemented by converting the memory into a vector and then taking the cosine similarity of the vector with the current situation.
		``𝑠𝑐𝑜𝑟𝑒 = 𝛼𝑟𝑒𝑐𝑒𝑛𝑐𝑦 · 𝑟𝑒𝑐𝑒𝑛𝑐𝑦 + 𝛼𝑖𝑚𝑝𝑜𝑟𝑡𝑎𝑛𝑐𝑒 ·𝑖𝑚𝑝𝑜𝑟𝑡𝑎𝑛𝑐𝑒 +𝛼𝑟𝑒𝑙𝑒𝑣𝑎𝑛𝑐𝑒 ·𝑟𝑒𝑙𝑒𝑣𝑎𝑛𝑐``

##### Reflection
Reflections are generated periodically, they are a type of memory that is the fom of a hig level abstraction. Reflections are generated when the sum of the importance scores for the latest events perceived by the agents exceeds a threshold (150 in this implementation). 


#### Planning and Reacting

##### Planning
- Plans describe a **future sequence of actions** for an agent and ensure behavior remains coherent over time.
- Each plan includes: **location**, **start time**, and **duration**.
- Plans are stored in the **memory stream** and influence future behavior through retrieval.
- Planning proceeds in a **top-down recursive manner**:
  - **Coarse plan**: A high-level daily schedule is generated based on the agent’s traits, previous day’s summary, and goals.
    - _Example_: "Wake up at 8:00 am", "go to college", "compose music", etc.
  - **Hourly plan**: Breaks down coarse steps into hour-long chunks with intermediate goals (e.g., “brainstorm ideas”).
  - **Fine-grained actions**: Further decomposed into 5–15 minute actions (e.g., “grab a granola bar”, “take a short walk”).

##### Reacting and Updating Plans
- At each time step, agents **perceive their environment**, and observations are stored as memory.
- When something notable is perceived, the agent decides whether to:
  - **Continue** with the current plan, or
  - **React** and modify the plan accordingly.
- If a reaction is warranted, the **plan is regenerated** from that point forward.
  - _Example_: If a stove is “burning,” the agent may abort breakfast and remake it.

##### Dialogue
- Dialogue is triggered by **agent interaction** and grounded in:
  - The agent’s **memory** of the other person,
  - The agent’s **own traits and context**,
  - The **history of the conversation so far**.
- For each utterance:
  - The **initiating agent** generates their message using retrieved memories.
  - The **responding agent** retrieves context and responds appropriately.
- This cycle continues until **one agent ends the conversation**.


#### Sandbox Environment Implementation

##### Architecture
- Built using the **Phaser** game development framework.
- Agents interact with a shared environment via a central **JSON state** and communicate with a sandbox server.
- The server maintains agent positions, actions, and object states in the world (e.g., coffee machine = "brewing").

##### Environment Modeling
- Represented as a **tree structure**: areas → sub-areas → objects (e.g., House → Kitchen → Stove).
- Each agent maintains a **partial tree view** of the world (what they’ve seen).
- The world is translated into **natural language descriptions** for the language model (e.g., "There is a stove in the kitchen.").

##### User Interaction
- Users can act as agents, inject new events or manipulate object states (e.g., turning a stove to “burning”).
- Agents update plans and behaviors based on new inputs in real time.
#### Controlled Evaluation

##### Procedure
- Agents were **interviewed using natural language** to assess five behavioral capabilities:
  - Self-knowledge
  - Memory recall
  - Future planning
  - Reaction to situations
  - Higher-level reflections

##### Conditions Tested
- **Full architecture**
- **Ablated architectures** removing one or more of memory, planning, or reflection
- **Crowdworker-authored baseline** (humans writing answers in the agent's voice)

##### Results
- Full architecture performed best in **believability rankings** (measured using TrueSkill).
- Each removed component led to a measurable performance drop.
- Common failure modes included:
  - Irrelevant memory retrieval
  - Fabricated details
  - Overly formal dialogue due to LLM bias
#### End-to-End Evaluation

- Ran a **two-day simulation** with 25 agents in an open world.
- Observed **emergent phenomena** such as:
  - Information diffusion (e.g., election news spreading)
  - Relationship formation (e.g., new friends or crushes)
  - Social coordination (e.g., agents organizing and attending a party)

#### Ethical and Societal Risks

- **Parasocial relationships**: Users may emotionally bond with agents, mistaking them for real people.
- **Deepfakes and persuasion**: LLM-driven agents could be exploited for manipulation or misinformation.
- **Human-in-the-loop design**: Generative agents should **augment**, not replace, human decision-makers.
- Recommendation: Apply **logging**, **tuning**, and **design constraints** to mitigate harm.

#### Conclusion 

##### Limitations
- Formal and sometimes unrealistic language (due to LLM instruction tuning).
- Memory retrieval still prone to errors.
- Long-term coherence remains challenging.

##### Future Directions
- Improve language realism and persona fidelity.
- Expand complexity of environments and agent roles.
- Integrate agents into broader interactive systems and HCI pipelines.

