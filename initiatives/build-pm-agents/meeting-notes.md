# Meeting Notes: Build PM Agents

Running log of meetings related to this initiative. Most recent first.

---

## [Date] — [Meeting Title]

**Attendees:**

**Notes:**

**Action items:**
- [ ]
Feb 24, 2026
Claude Epic Project — Live Demo
Invited Erin Runingen Alen Tersakyan Candace Norton David Klappoth Max Pederson Justin Witz Tom Kazer Katie Metter
Attachments Claude Epic Project — Live Demo 
Meeting records Recording 

Summary
Justin Witz described how they utilized Claude and AI tools to establish a reusable system for creating standardized epics aligned with a template provided by Katie, iteratively refining the AI instructions based on feedback and business context. Max Pederson recommended using the Opus 4.6 model, uploading full documents for better context, and adding user role information to the `skills.md` document for refined output. The participants, including Alen Tersakyan, discussed Claude's connectivity to Jira for read-only access and agreed on the importance of continued collaboration, template alignment, and feedback, with Max Pederson committing to sharing a proposed structure for story granularity.

Details
Setting up the Project and Leveraging AI for Epic Creation: Justin Witz outlined how they used Claude and AI tools to streamline the creation of epics, aiming for alignment with a template provided by Katie. The goal was to establish a consistent, reusable system by setting up a project in Claude with specific instructions and the template document, allowing for easy generation of standardized epics and leveraging collective improvements. They initially used a prompt to ask Claude for assistance in setting up a project feature for the template and followed the step-by-step instructions provided by the AI.
Project Configuration and Iterative Instruction Refinement: The template document, which was uploaded as a PDF, along with a set of high-level instructions (similar to an agent markdown document) outlining how the AI should behave, were included in the "epic documentation project" in Claude. These instructions were iteratively built upon and refined over time based on feedback from engineers and experience, including adding requirements for concise formatting and treating the conversation as iterative development. Justin Witz demonstrated the process of feeding high-level business context into a new chat, which then prompts the user for missing information based on the template, leading to an iterative, conversational build-out of the epic document.
Best Practices and Model Recommendations for AI Use: Max Pederson offered several recommendations for optimizing AI usage, strongly advising the use of the highest graded model, Opus 4.6, over Sonnet, as it is the strongest and smartest. They also suggested that instead of copying and pasting text into the prompt box, users should upload full documents (such as Word documents) for contextual additions, noting that giving the agent full documents makes it easier to scan, whereas too much text in the prompt box can lead to confusion. Additionally, Max Pederson suggested adding context about the user's role and job to the `skills.md` instruction document, helping the AI refine the output to the user's actual role.
Connecting Claude to Jira and Discussion on Co-Work: Alen Tersakyan inquired about Claude's connectivity with Jira. Justin Witz confirmed that Jira is listed as an available connector in the settings, demonstrating how it was set up for read-only access to pull down existing epics. Max Pederson noted that while connecting co-work to a project allows for more advanced features like automatically creating PRDs and other agentic tasks, some connectors, like those for writing and releasing data, might require approval or be restricted due to compliance concerns.
Template Alignment and Future Collaboration: The conversation concluded with agreement on the importance of continued collaboration and alignment on best practices and the epic template itself. Alen Tersakyan and Max Pederson agreed that there is room for providing feedback on the template Katie provided, and Max Pederson committed to sharing out a proposed structure for story granularity that they had already developed.

Suggested next steps
Justin Witz will share out the project for the weekly product email and the detailed instructions for the epic template project afterwards.
Justin Witz will delete the recording of this meeting due to the discussion about the Jira connector.
Max Pederson will send out the proposed ideal epic structure to the group.

You should review Gemini's notes to make sure they're accurate. Get tips and learn how Gemini takes notes
David suggested using a github repo and building agents to do this work. He will mock up a prototype

---
