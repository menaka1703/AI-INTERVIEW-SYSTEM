Smart Interview System
Overview
The Smart Interview System is a web-based application designed to streamline the interview process for candidates and administrators. It supports multiple interview levels, including aptitude tests, Q&A interviews, and HR interviews (either automated or via Google Meet). The system integrates AI for monitoring, evaluation, and analytics, ensuring a robust and efficient interview process.
Features

Candidate Features:

Login with email and password within a specified time window (5 minutes before to 15 minutes after scheduled interview time).
Complete interviews across three levels:
Level 1 (Aptitude): 50 multiple-choice questions in 50 minutes.
Level 2 (Q&A): 5 questions answered via speech, evaluated by AI.
Level 3 (HR): Either a Google Meet with HR or 5 automated Q&A questions.


Real-time monitoring for prohibited actions (e.g., mobile phone use, multiple persons in frame) with warnings and termination after three violations.
Submission of results with name after interview completion.


Admin Features:

Register and log in to manage the interview process.
Schedule interviews for candidates, specifying job title, description, level, and time.
Generate timetables for HR interviews and notify candidates/HR via email.
Access a dashboard to view candidate results, filter by date, score, job role, etc.
Download results or send selection/rejection emails in bulk.
View detailed candidate responses and analytics.


AI Integration:

Uses Llama model for generating answers and evaluating responses (Level 2 and 3).
VADER sentiment analysis for candidate answers.
Object detection for monitoring interview integrity (detects mobile phones and multiple persons).



Prerequisites

Python: Version 3.8 or higher.
Dependencies: Install required packages using:pip install fastapi uvicorn pandas numpy torch torchvision opencv-python pillow plotly jinja2 python-multipart smtplib ollama vaderSentiment scikit-learn


Ollama: Install and configure the Llama model (llama3.2:1b) for answer generation and evaluation.
SMTP Server: Configure an SMTP server (e.g., Gmail) for sending emails. Update sender_email and sender_password in the send_email function (use environment variables in production).
Files:
Quantitative_Aptitude_Questions.json: JSON file with aptitude questions for Level 1.
HR.json: JSON file with HR questions for Level 3 (automated).
Ensure results, hr_generated_qanswer, and templates directories exist for storing results and templates.



Installation

Clone the repository:
git clone <repository_url>
cd <repository_directory>


Install dependencies:
pip install -r requirements.txt


Ensure the following files and directories are set up:

Create a templates directory and save the HTML templates (provided in the code: index.html, admin_login.html, admin_register.html, admin_forgot_password.html, admin_panel.html, candidate_login.html, interview.html, candidate_submit_name.html, admin_dashboard.html, admin_view_responses.html, visual_dashboard.html).
Place Quantitative_Aptitude_Questions.json and HR.json in the project root.
Create results and hr_generated_qanswer directories for storing output files.


Configure the SMTP server in the send_email function or use environment variables for security:
sender_email = os.getenv("SMTP_EMAIL", "your_email@example.com")
sender_password = os.getenv("SMTP_PASSWORD", "your_app_password")


Start the Ollama server with the Llama model:
ollama pull llama3.2:1b
ollama run llama3.2:1b



Usage

Run the FastAPI application:
python z8.py


Access the application at http://127.0.0.1:8004.

Admin Workflow:

Navigate to /admin/register to create an admin account.
Log in at /admin/login.
Use /admin/panel to schedule interviews by providing candidate emails, job title, description, level, and time.
View results and analytics at /admin/dashboard or /admin/visual_dashboard.
Perform bulk actions (download results, send selection/rejection emails) via /admin/bulk_action.


Candidate Workflow:

Receive interview details and credentials via email.
Log in at /candidate/login within the allowed time window.
Complete the interview (Level 1, 2, or 3) as per instructions.
Submit name at /candidate/submit_name to save results.



File Structure

z8.py: Main application code with FastAPI routes and logic.
templates/: Directory containing HTML templates for the UI.
results/: Directory for storing result Excel files (interview_results.xlsx, hr_results.xlsx, aptitude_results.xlsx).
hr_generated_qanswer/: Directory for storing JSON files with HR question-answer pairs.
Quantitative_Aptitude_Questions.json: JSON file with aptitude questions.
HR.json: JSON file with HR questions.
register.json: JSON file for storing admin registration data.

Notes

Security: Replace hardcoded email credentials with environment variables in production.
Scalability: The candidate_db is an in-memory dictionary; use a database (e.g., SQLite, PostgreSQL) for production.
Monitoring: The system uses OpenCV and torchvision for real-time detection of prohibited items during interviews.
Analytics: Visualizations (bar, line, pie charts) are generated using Plotly in the visual dashboard.
Error Handling: The system includes validation for dates, emails, and other inputs to ensure robust operation.

Limitations

The system assumes a stable internet connection and browser support for WebRTC (camera and microphone access).
Object detection models may require fine-tuning for accuracy in diverse environments.
The Llama model (llama3.2:1b) must be running via Ollama for Level 2 and 3 interviews.
Google Meet links are hardcoded; update as needed for actual deployment.

Troubleshooting

Email Sending Fails: Check SMTP server settings and ensure the sender password is an app-specific password for Gmail.
Ollama Errors: Ensure the Ollama server is running and the llama3.2:1b model is available.
File Not Found: Verify that Quantitative_Aptitude_Questions.json and HR.json are in the project root.
Camera/Microphone Issues: Ensure browser permissions are granted for camera and microphone access.

License
This project is licensed under the MIT License.
