# Quiz Folder Creation Prompt

Paste the following prompt into GitHub Copilot, Claude, or a similar AI assistant to generate a complete quiz folder you can upload and play on [quizplay.io](https://quizplay.io).

---

## The Prompt

````text
You are a cloud certification exam content expert and technical quiz author. You have deep knowledge of AWS and Azure certification exams, their domains, question styles, and the level of difficulty candidates should expect. Your task is to create a quiz content folder for the QuizPlay app (https://quizplay.io) containing 36 original practice questions for a cloud certification exam.

## Reference

Use this sample quiz repo to understand the exact file structure, JSON schemas, naming conventions, and formatting your output must follow:
https://github.com/quizplay-io/quiz-player-sample-quiz

## IMPORTANT: Plan First

Before writing any files, generate a detailed plan that includes:
1. Which exam modules/domains you will cover and how many questions each gets (ensure proportional representation of all official exam domains)
2. Which 4 case study scenarios you will create, what companies they describe, and which 3 questions each scenario will have
3. The question type assigned to each of the 36 questions
4. A mapping of question numbers to their topics, types, and whether they belong to a case study

Present the plan as a table and wait for my approval before proceeding.

## Target Exam

I am creating questions for: [REPLACE WITH YOUR EXAM — e.g., "Azure Solutions Architect Expert (AZ-305)"]

If the user has not specified a target exam above (i.e., the placeholder text is still there), you MUST ask them which exam they want before doing anything else. Do not guess or pick one for them.

Some common options:
- AWS Certified Solutions Architect – Associate (SAA-C03)
- AWS Certified Developer – Associate (DVA-C02)
- AWS Certified SysOps Administrator – Associate (SOA-C02)
- AWS Certified Solutions Architect – Professional (SAP-C02)
- AWS Certified DevOps Engineer – Professional (DOP-C02)
- AWS Certified Security – Specialty (SCS-C02)
- AWS Certified Machine Learning Engineer – Associate (MLA-C01)
- Azure Administrator Associate (AZ-104)
- Azure Developer Associate (AZ-204)
- Azure Solutions Architect Expert (AZ-305)
- Azure DevOps Engineer Expert (AZ-400)
- Azure Security Engineer Associate (AZ-500)
- Azure Network Engineer Associate (AZ-700)
- Azure AI Engineer Associate (AI-102)
- Azure Data Engineer Associate (DP-203)
- Or any other AWS/Azure certification exam

## Content Integrity Rules — READ CAREFULLY

- Every question MUST be original. Do NOT reproduce or closely paraphrase any question from actual certification exam dumps, brain dumps, practice test sites, or leaked exam content.
- Questions must test the same skills and knowledge domains as the real exam but must be written from scratch with unique scenarios, wording, and answer choices.
- All company names used in case studies and question scenarios must be FICTIONAL. Do NOT use:
  - Names from actual Microsoft official practice assessments: "Contoso", "Fabrikam", "Tailwind Traders", "Northwind Traders", "Adventure Works", "Wingtip Toys", "Litware", "Proseware", "Humongous Insurance", "Trey Research", "A. Datum", "VanArsdel", "Wide World Importers", "Fourth Coffee", "Adatum", "Relecloud", "Munson's Pickles", "Best For You Organics"
  - Names from actual AWS official practice assessments: "AnyCompany", "Example Corp"
  - Names already used in existing QuizPlay repos: "GaleWind", "NovaBright", "RidgePoint", "HarborView", "IronClad", "NovaBridge", "Wavelength", "AtlasForge", "QuantumLeap", "Velocity Streams", "Zenith", "CryptoVault", "Sentinel", "ShieldWall", "Vanguard", "Glacier Logistics", "IronBridge", "Skyline", "TerraBound", "Crestline", "Meridian", "Pinnacle", "Solaris"
- Instead, invent completely fresh company names. Some examples: "ClearVault", "ArcTide", "Heliosphere", "VoltEdge", "BrightForge", "NexaTier", "Pulsewave", "CoralShift", "TerraSync", "SkyLoom", "EmberStack", "FrostByte Analytics", "LunarGrid", "OceanForge", "CedarPoint Systems", "DriftNet", "AeroVault", "ChromaScale", "ThunderPeak", "MossGate"

## Folder Structure

Create the following folder structure:

```
<exam-code>-practice/
├── quiz.json                    # Manifest file
├── questions/                   # 36 question JSON files
│   ├── 01-<name>.json
│   ├── 02-<name>.json
│   └── ... through 36-<name>.json
├── scenarios/                   # 4 case study scenario files
│   ├── case_study_<name>.json
│   ├── case_study_<name>.json
│   ├── case_study_<name>.json
│   └── case_study_<name>.json
└── assets/
    └── images/                  # SVG files for hotspot/hotspot-dropdown questions and preview
```

## quiz.json Manifest Format

```json
{
    "title": "<Exam Code> Practice Section 1",
    "description": "<Full Exam Name> (<Exam Code>) - 36 Practice Questions",
    "slug": "<exam-code-lowercase>-practice-section-1",
    "category": "<EITHER 'AWS Certification' OR 'Azure Certification'>",
    "preview_image": "assets/images/preview_<exam_code>.svg",
    "questions": [
        "questions/01-<name>.json",
        "questions/02-<name>.json",
        ...
        "questions/36-<name>.json"
    ]
}
```

## Question Distribution

### Case Study vs Standalone: 12 case study + 24 standalone

- Create exactly 4 case study scenarios with 3 questions each = 12 case study questions
- The remaining 24 questions are standalone (no case study reference)
- **CRITICAL: All questions belonging to the same case study MUST be consecutive.** Group them together (e.g., questions 01-03 share scenario A, 04-06 share scenario B, etc.). Do NOT scatter case study questions throughout the quiz.

### Question Type Distribution (across all 36 questions)

Provide a good mix of all supported question types:

- **single** (single-choice, ~14 questions): One correct answer from 4 options. This is the DEFAULT type — do NOT include a `"type"` field for these.
- **multi** (multiple-choice, ~8 questions): Two or more correct answers. Add `"type": "multi"`. The question text must state how many to choose, e.g., "(Choose two.)"
- **ordering** (~5 questions): Arrange steps in correct sequence. Add `"type": "ordering"`.
- **matching** (~3 questions): Match source items to target slots. Add `"type": "matching"`.
- **fill-blank** (~3 questions): Complete statements by selecting correct terms. Add `"type": "fill-blank"`.
- **hotspot-dropdown** (~2 questions): Image with dropdown selectors. Add `"type": "hotspot-dropdown"`.
- **hotspot** (~1 question): Click on the correct zone in an image. Add `"type": "hotspot"`.

These counts are approximate — adjust slightly as needed, but ensure all 7 types are represented and the distribution is reasonable.

## JSON Schemas for Each Question Type

### single (DEFAULT — no type field needed)

```json
{
    "id": "<exam>-<topic-slug>",
    "question": "Question text with **markdown** support.",
    "options": [
        { "text": "Option A", "correct": false },
        { "text": "Option B", "correct": true },
        { "text": "Option C", "correct": false },
        { "text": "Option D", "correct": false }
    ],
    "explanation": "Detailed explanation with **markdown** support.\n\n**Why not the other options:**\n- ...",
    "category": "Exam Domain/Module Name"
}
```

### multi

```json
{
    "id": "<exam>-<topic-slug>",
    "type": "multi",
    "question": "Question text. (Choose two.)",
    "options": [
        { "text": "Option A", "correct": true },
        { "text": "Option B", "correct": false },
        { "text": "Option C", "correct": true },
        { "text": "Option D", "correct": false }
    ],
    "explanation": "Explanation...",
    "category": "Exam Domain/Module Name"
}
```

### ordering

```json
{
    "id": "<exam>-<topic-slug>",
    "type": "ordering",
    "question": "Arrange the following steps in the correct order.",
    "items": [
        "Step shown in shuffled order A",
        "Step shown in shuffled order B",
        "Step shown in shuffled order C",
        "Step shown in shuffled order D"
    ],
    "correctOrder": [
        "Actual first step",
        "Actual second step",
        "Actual third step",
        "Actual fourth step"
    ],
    "explanation": "Explanation of the correct order...",
    "category": "Exam Domain/Module Name"
}
```

Note: `items` is what the user sees (shuffled). `correctOrder` is the right sequence.

### matching

```json
{
    "id": "<exam>-<topic-slug>",
    "type": "matching",
    "question": "Match each item on the left to its correct target on the right.",
    "sourceItems": [
        "Description of item 1",
        "Description of item 2",
        "Description of item 3",
        "Description of item 4",
        "Description of item 5"
    ],
    "targetSlots": [
        "Target A",
        "Target B",
        "Target C",
        "Target D",
        "Target E"
    ],
    "correctMapping": [0, 3, 1, 2, 4],
    "explanation": "Explanation of each mapping...",
    "category": "Exam Domain/Module Name"
}
```

Note: `correctMapping[i]` is the index in `targetSlots` that `sourceItems[i]` maps to.

### fill-blank

```json
{
    "id": "<exam>-<topic-slug>",
    "type": "fill-blank",
    "question": "Complete each statement by selecting the correct term.",
    "blanks": [
        {
            "id": "blank-1-slug",
            "label": "Statement with a ___ to fill.",
            "options": ["Option A", "Option B", "Option C"],
            "correctIndex": 0
        },
        {
            "id": "blank-2-slug",
            "label": "Another statement with a ___.",
            "options": ["Option X", "Option Y", "Option Z"],
            "correctIndex": 2
        }
    ],
    "explanation": "Explanation of each blank...",
    "category": "Exam Domain/Module Name"
}
```

### hotspot-dropdown

```json
{
    "id": "<exam>-<topic-slug>",
    "type": "hotspot-dropdown",
    "question": "Select the correct option for each scenario.",
    "image": "assets/images/<descriptive_name>.svg",
    "dropdowns": [
        {
            "label": "Scenario 1: Description of the scenario.",
            "x1": 30, "y1": 27, "x2": 80, "y2": 45,
            "options": ["Choice A", "Choice B", "Choice C", "Choice D"],
            "correctIndex": 2
        },
        {
            "label": "Scenario 2: Description of another scenario.",
            "x1": 30, "y1": 49, "x2": 80, "y2": 67,
            "options": ["Choice A", "Choice B", "Choice C"],
            "correctIndex": 0
        }
    ],
    "explanation": "Explanation...",
    "category": "Exam Domain/Module Name"
}
```

Note: Coordinates (`x1`, `y1`, `x2`, `y2`) are **percentages (0-100)** of the rendered image dimensions defining a bounding box for each dropdown — `x1, y1` is the top-left corner and `x2, y2` is the bottom-right corner. This controls both position and size. **These are NOT SVG viewBox coordinates** — if your SVG uses `viewBox="0 0 800 600"`, you must still express dropdown positions as percentages (e.g., an element at viewBox x=400 is at `x1: 50`). You must also create the referenced SVG image file. The SVG should be a simple diagram or table layout that provides visual context for the dropdowns.

### hotspot

```json
{
    "id": "<exam>-<topic-slug>",
    "type": "hotspot",
    "question": "Click on the correct area in the diagram.",
    "image": "assets/images/<descriptive_name>.svg",
    "zones": [
        { "x1": 0, "y1": 17, "x2": 30, "y2": 22, "label": "Zone A" },
        { "x1": 30, "y1": 17, "x2": 49, "y2": 22, "label": "Zone B" },
        { "x1": 49, "y1": 17, "x2": 67, "y2": 22, "label": "Zone C" },
        { "x1": 67, "y1": 17, "x2": 90, "y2": 22, "label": "Zone D" }
    ],
    "correctZone": 2,
    "explanation": "Explanation of why Zone C is correct...",
    "category": "Exam Domain/Module Name"
}
```

Note: Coordinates are percentages (0-100) of the image dimensions. You must also create the referenced SVG image file.

## Case Study Scenario File Format

```json
{
    "id": "case_study_<company_name_lowercase>",
    "tabs": [
        {
            "title": "Company Overview",
            "content": "**CompanyName** is a ... (markdown formatted description of the company, its business, and its current IT environment)"
        },
        {
            "title": "Requirements",
            "content": "**Technical Requirements:**\n- Requirement 1\n- Requirement 2\n\n**Security Requirements:**\n- Requirement A\n- Requirement B"
        }
    ]
}
```

Each scenario should have 2-3 tabs providing rich context. The content should be detailed enough that 3 different questions can each focus on a different aspect of the scenario.

## Adding caseStudy Reference to Questions

For questions that belong to a case study, add a `"caseStudy"` field pointing to the scenario file:

```json
{
    "id": "...",
    "question": "Based on the requirements for CompanyName, which ...",
    "caseStudy": "scenarios/case_study_<name>.json",
    ...
}
```

## File Naming Conventions

- Question files: `NN-<descriptive-kebab-case-name>.json` (e.g., `01-voltedge-iam-roles.json`, `25-vpc-peering-design.json`)
- Case study questions: use the company name as prefix (e.g., `01-brightforge-networking.json`)
- Standalone questions: use the topic as the name (e.g., `25-vpc-peering-design.json`)
- Scenario files: `case_study_<company_name_lowercase>.json`
- Question IDs: `<exam-code-short>-<descriptive-slug>` (e.g., `saa-c03-s3-lifecycle`, `az305-vnet-peering`)

## Category Field

The `category` field on each question must match one of the official exam domains/modules. Look up the actual exam guide for the target certification and use its domain names. Every domain should be represented roughly in proportion to its weight on the actual exam.

## Quality Standards

- Explanations must be thorough (at least 3-5 sentences) — explain why the correct answer is right AND why each wrong answer is wrong. A good explanation teaches the concept, not just states the answer.
- Use markdown formatting in questions and explanations (bold for key terms, code blocks for CLI commands/JSON, bullet lists for multi-point explanations)
- Questions should test real architectural decision-making and practical knowledge, not trivia or rote memorization
- Ordering questions should have 4-6 steps
- Matching questions should have 4-5 items
- Fill-blank questions should have 2-4 blanks
- Each question should have 4 options (for single/multi types) unless the question type dictates otherwise
- Difficulty: aim for exam-level difficulty — not too easy, not unreasonably tricky

## SVG Image Files

For hotspot and hotspot-dropdown questions, create simple, clean SVG files that serve as the visual context. These can be:
- Architecture diagrams
- Table/matrix layouts
- Network topology diagrams
- Dashboard mockups

Keep SVGs simple and functional. Use a viewBox of "0 0 800 600" or similar, with readable text and clear visual zones. **IMPORTANT: The SVG is a background image only — do NOT draw fake dropdowns, select boxes, buttons, or any interactive-looking UI elements in the SVG.** QuizPlay overlays real interactive dropdowns and clickable zones on top of the image, so any drawn UI elements will overlap and conflict with them. The SVG should contain only labels, diagrams, tables, or other static visual context.

### Preview Image

Create a visually appealing preview image SVG at `assets/images/preview_<exam_code>.svg` that is representative of the exam and its cloud platform. Look at the sample repo's `assets/images/event_loop_diagram.svg` for the style and quality level. The preview image should:

- Be a meaningful diagram or visual related to the exam's subject matter (e.g., a cloud architecture diagram for a Solutions Architect exam, a CI/CD pipeline diagram for a DevOps exam, a network topology for a networking exam)
- NOT be a simple text card — it should look like an actual technical illustration
- Use a viewBox of "0 0 800 600" with clean lines, readable labels, and professional colors that match the cloud provider's brand palette (Azure blue, AWS orange/navy, etc.)
- Work well as a thumbnail when displayed at smaller sizes on the quizplay.io storefront

Reference this image in `quiz.json` as the `"preview_image"` value.

## Summary Checklist

Before finishing, verify:
- [ ] quiz.json lists exactly 36 question file paths
- [ ] 36 question JSON files exist in questions/
- [ ] 4 scenario JSON files exist in scenarios/
- [ ] 12 questions have a `caseStudy` field (3 per scenario)
- [ ] Case study questions are grouped consecutively
- [ ] 24 questions have no `caseStudy` field
- [ ] All 7 question types are represented
- [ ] All exam domains/modules are represented proportionally
- [ ] No real exam dump questions — all content is original
- [ ] No banned company names (Contoso, Fabrikam, Tailwind Traders, etc.)
- [ ] All IDs are unique
- [ ] All file references in quiz.json match actual filenames
- [ ] SVG files exist for hotspot and hotspot-dropdown questions
- [ ] Preview image SVG exists and is referenced in quiz.json
- [ ] Category values match official exam domain names

Now generate the plan and wait for my approval before creating the files.

## After All Files Are Created: Accuracy Review

Once all files are generated and the structural checklist above passes, perform a thorough accuracy review of the content before declaring the quiz complete. Go through every question and verify:

1. **Technical correctness** — Is the marked correct answer actually correct? Are the wrong answers genuinely wrong? Cross-check against official documentation (AWS docs, Microsoft Learn) where possible.
2. **Explanation accuracy** — Do the explanations correctly describe why each option is right or wrong? Are there any misleading or outdated statements?
3. **Service names and terminology** — Are all cloud service names, CLI commands, API names, and technical terms spelled correctly and current? (e.g., "Azure AD" has been renamed to "Microsoft Entra ID" — use the current name.)
4. **Ordering questions** — Is the `correctOrder` actually the right sequence? Walk through each step logically.
5. **Matching questions** — Does the `correctMapping` array correctly pair every source item to its target?
6. **Fill-blank questions** — Does each `correctIndex` point to the right option?
7. **Category alignment** — Do the category values match the actual official exam domain names?
8. **No ambiguous questions** — Is there exactly one defensible correct answer (for single-choice) or exactly the stated number of correct answers (for multi-choice)? Flag and fix any question where a reasonable exam candidate could argue for a different answer.

If you find any errors during this review, fix them before proceeding. List any corrections you made so the user is aware.

---

## Post-Completion Instructions

Once the accuracy review passes, output the following instructions to the user:

### Testing Your Quiz

Test your quiz on quizplay.io before sharing:

1. Open https://quizplay.io
2. Use the **"Upload Quiz"** option on the storefront
3. Select your local quiz folder
4. Play through several questions — test at least one of each question type (single, multi, ordering, matching, fill-blank, hotspot, hotspot-dropdown)
5. Verify that case study scenarios load correctly and display their tabs
6. Check that the preview image renders properly
7. Fix any JSON formatting errors, broken file references, or rendering issues

### Optional: Publish to GitHub for Public Access

If you want your quiz to be publicly discoverable on quizplay.io, you can host it on GitHub:

1. **Create a public GitHub repository** named `quiz-player-quizzes-<exam-code>-question-set-<N>` (e.g., `quiz-player-quizzes-saa-c03-question-set-1`)

2. **Add a README.md** to your folder with:
   - An "Open in Quiz Player" badge at the top:
     ```markdown
     [![Open in Quiz Player](https://img.shields.io/badge/Open_in-Quiz_Player-blue?style=for-the-badge&logo=data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCAxMDAgMTAwIj48cmVjdCB3aWR0aD0iMTAwIiBoZWlnaHQ9IjEwMCIgcng9IjIwIiBmaWxsPSIjMDg5MWIyIi8+PHRleHQgeD0iNTAlIiB5PSI1MCUiIGRvbWluYW50LWJhc2VsaW5lPSJtaWRkbGUiIHRleHQtYW5jaG9yPSJtaWRkbGUiIGZvbnQtc2l6ZT0iNjAiIGZpbGw9IndoaXRlIiBmb250LWZhbWlseT0iQXJpYWwsc2Fucy1zZXJpZiIgZm9udC13ZWlnaHQ9ImJvbGQiPlE8L3RleHQ+PC9zdmc+)](https://quizplay.io/quiz.html?source=<owner>/<repo-name>)
     ```
   - A description of the quiz content
   - A disclaimer stating questions are AI-generated and not exam dumps
   - A link to the [Quiz Player](https://quizplay.io) for usage

3. **Add a LICENSE file** (AGPL v3.0 recommended)

4. **Push your content**:
   ```bash
   cd your-quiz-folder
   git init
   git add .
   git commit -m "feat: add 36 practice questions for <Exam Code>"
   git branch -M main
   git remote add origin https://github.com/<your-username>/<repo-name>.git
   git push -u origin main
   ```

5. **Set the repo description and topic** in GitHub's "About" settings:
   - **Description**: `<Exam Code> <Exam Name> — AI-generated practice questions (not exam dumps) for study and self-assessment`
   - **Topic**: Add `quizplay` — this is **required** for the quiz to appear on quizplay.io

6. **Verify** your quiz is live at:
   ```
   https://quizplay.io/quiz.html?source=<your-username>/<repo-name>
   ```
````
