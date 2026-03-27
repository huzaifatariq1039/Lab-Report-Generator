🎓 Automated Lab Report Generator (n8n + Claude AI)
I got tired of spending more time formatting Word documents than actually doing the work for my university labs. I built this n8n workflow to completely automate the entire process of generating lab reports for any programming or computer science subject.

I simply feed it a raw lab manual (PDF), and the agent uses Claude to read the instructions, write the required code or solutions, and generate a perfectly formatted .docx file. It automatically handles the cover page, borders, credentials tables, and even renders the code blocks as syntax-highlighted images so Word doesn't ruin the formatting.

🚀 How It Works
The workflow is triggered via a Webhook and processes the document in four main stages:

1. Data Ingestion
I set up a Webhook node that accepts a POST request containing a multipart form. This takes in the lab manual file alongside standard metadata (Student Name, ID, Lab Number, Instructor, Subject, etc.).

2. The AI Brain (Claude)
The file and metadata are passed to Claude. I engineered the system prompt to force the AI to act as a strict lab assistant. It reads the manual, identifies every single task, and writes the complete, runnable code to solve it based on the manual's instructions. It returns a structured JSON containing the task titles, code, expected outputs, and a brief explanation for each.

3. The Document Builder (JavaScript to Python)
Instead of trying to format a Word document directly inside n8n, I wrote a custom JavaScript node that dynamically generates a massive Python script. This script uses python-docx and Pillow. It takes the JSON data from Claude and writes the instructions to build the document from scratch. It enforces a very strict visual format:

A customized cover page with an institutional logo and a dynamic credentials table.

Double page borders on every page.

It takes the raw code and terminal outputs, converts them into dark-theme, syntax-highlighted images, and embeds them into the .docx to preserve formatting.

4. Local Execution & Delivery
The generated Python script is sent via an HTTP request to a lightweight local Flask server (the Python Executor). The server runs the script, generates the final lab_report_output.docx, converts it to a base64 string, and sends it back through the n8n webhook response.

🛠️ Tech Stack
Automation: n8n

AI Model: Claude (Anthropic API)

Document Generation: Python (python-docx, Pillow)

Local Execution: Flask (Python)
