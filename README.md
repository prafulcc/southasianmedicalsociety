# SAMS Quiz Platform

Production instance hosted here: [southasianmedicalsociety.pages.dev](https://southasianmedicalsociety.pages.dev/)

## Project Overview

Welcome to the South Asian Medical Society (SAMS) Quiz Platform. This is a simple, file-based web application designed to create, manage, and deliver multiple-choice quizzes for medical students. The platform consists of a quiz builder, a password-protected question bank, and a quiz-taking interface.

The entire system is designed to be self-contained and does not require a backend server or database. It uses local files (like `.html` and `.json`) and the browser's `localStorage` to function.

## Key Features

* **Password Protection**: The question bank (`index.html`) is gated with a simple password (`2025`) to restrict access. Access is granted for 24 hours before requiring the password again.
* **Dynamic Quiz Builder**: Easily create complex multiple-choice questions with unique explanations for each option, hints, and optional image URLs.
* **Client-Side Storage**: Quiz results are stored directly in the user's browser (`localStorage`), meaning no server is needed to track progress.
* **Serverless Architecture**: The entire platform can be hosted on any static web hosting service (like GitHub Pages, Netlify, or Vercel) as it requires no backend processing.
* **Data Portability**: Since each quiz is just a string of text, it's incredibly easy to share, back up, or move quizzes between different `bank.json` files.

## File Structure

Here's a breakdown of the key files in this project:

* `index.html`: The main landing page for the **Question Bank**. It is password-protected and dynamically loads and displays all available quizzes from `bank.json`.
* `builder.html`: An interactive webpage for **creating quizzes**. It provides a form to add questions, options, and explanations, and then generates a Base64-encoded string representing the quiz data.
* `quiz.html`: The **Quiz Viewer**. This page takes the Base64 string (usually from a URL parameter) and renders an interactive quiz for the user to take. It also handles storing results in the browser's local storage.
* `bank.json`: The central "database" for the question bank. It contains a structured list of all quizzes, organized by semester and week. The actual quiz content is stored as a Base64 string.
* `about.html`: A static page providing information about the South Asian Medical Society.

## How it Works: The Data Flow

The application operates in a clear cycle:

1.  **Creation**: A quiz administrator uses `builder.html` to create a new quiz. They fill in the title, questions, options (including the correct one), and explanations for each option.
2.  **Generation**: After filling out the quiz, the admin clicks the "Generate" button. The `builder.html` page validates the form and converts the entire quiz structure into a single, long string of **Base64** text.
3.  **Storage**: The administrator copies this Base64 string and manually pastes it into the `bank.json` file under the appropriate semester and week, inside the `quizData` field.
4.  **Access**: A student navigates to `index.html`. They are prompted for a password (`2025`). If correct, the page fetches the data from `bank.json` and displays a list of all available quizzes. It also checks `localStorage` to show scores from previous attempts.
5.  **Taking the Quiz**: When the student clicks on a quiz link, they are redirected to `quiz.html?quiz=<base64_string>`. The `quiz.html` page reads the Base64 string from the URL, decodes it back into a usable quiz object, and presents the questions to the student.
6.  **Results**: Upon completion, `quiz.html` calculates the score and saves the results (including the score, date, and attempt number) to the browser's `localStorage`, using the quiz's Base64 string as the unique key.


## Getting Started: How to Add a New Quiz

To add a new quiz to the question bank, follow these steps:

1.  **Open the Quiz Builder**: Open the `builder.html` file in your web browser.
2.  **Create the Quiz**:
    * Fill in the "Quiz Title".
    * For each question, add the question text, options, and explanations.
    * **Crucially, select the correct answer** using the radio button for each question.
    * Use the "Add Question" button to add more questions to your quiz.
3.  **Generate the Base64 Code**:
    * Once you're finished, click the "Generate" button at the bottom.
    * If there are no validation errors, a "Base64 Output" text area will appear. Copy the entire string from this box.
4.  **Update the Question Bank**:
    * Open the `bank.json` file in a text editor.
    * Navigate to the semester and week where you want to add the quiz.
    * Find the `quizzes` array and add a new object. Paste your copied Base64 string into the `quizData` property.

    **Example `bank.json` structure:**
    ```json
    {
      "semesters": [
        {
          "title": "Semester 1: Mind and Movement",
          "weeks": [
            {
              "title": "Theme 1a: Early Embryology...",
              "quizzes": [
                {
                  "quizData": "PASTE_YOUR_NEW_BASE64_STRING_HERE"
                },
                {
                  "quizData": "ANOTHER_QUIZ_STRING..."
                }
              ]
            }
          ]
        }
      ]
    }
    ```
5.  **Save and Commit**: Save the `bank.json` file and commit your changes to the Git repository.

## Deployment

This project is configured with a Continuous Integration/Continuous Deployment (CI/CD) pipeline. This means that once your changes are committed and pushed to the `main` branch of the Git repository, the deployment process is handled automatically. You do not need to perform any manual steps to get your changes live; the CI/CD workflow takes care of publishing the updated files to the web server.
