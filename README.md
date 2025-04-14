
# 🎯 Build a LinkedIn Job Collector with Make.com + ChatGPT + Airtable + Google Docs

This automation collects job listings from LinkedIn RSS feeds, stores them in Airtable, generates a custom resume using GPT, and logs everything for tracking.

---

## 🔧 Prerequisites

- Accounts on:
  - [Make.com](https://make.com)
  - [Airtable](https://airtable.com)
  - [OpenAI](https://platform.openai.com)
  - [Google Docs](https://docs.google.com)
- Airtable base with job tracking structure (columns: Job Title, Company, Link, GPT Summary, Resume Link, etc.)
- LinkedIn job RSS feed (e.g., via [Feedly](https://feedly.com) or any feed service)

---

## 🧠 Scenario Breakdown

### 🔁 Step-by-Step Make Scenario

#### 1. **RSS Feed Module (Trigger)**
- **Module:** RSS > Watch RSS Feed Items
- **Feed URL:** Use a feed from LinkedIn search (you can get it via tools like [RSS.app](https://rss.app/))
- **Polling interval:** e.g., every 15 min or 1 hour

#### 2. **Airtable > Search Records**
- Check if job link already exists to avoid duplicates
- Match based on `Link` field

#### 3. **Filter: If Not Found**
- Only continue if the record is **not** found in Airtable

#### 4. **Airtable > Create Record**
- Add new job entry: Title, Link, Date Added, etc.

#### 5. **HTTP Module**
- Use HTTP module to retrieve the full job description from the job posting link
- This output (e.g., `{{8.data}}`) is used as the input for the GPT prompt

#### 6. **OpenAI (ChatGPT) > Create Custom Resume**
- **Module:** OpenAI > Create a Completion
- Use the following prompt in the ChatGPT module:

```plaintext
---

As a resume expert, your role is to craft resumes tailored to specific job applications.

🔹 **Objective:** Adapt the candidate’s resume to closely match the job description provided in {{8.data}}, ensuring it remains genuine and optimized for ATS compatibility.

🔹 **Input:** The candidate's base resume (including experience, education, and skills) along with the job description.

🔹 **Output:** A polished resume that is both ATS-friendly and highlights the candidate’s most relevant skills and experience, derived from the job description.

**Resume Structure:**

- **Contact Information:** Use the candidate’s personal details.
- **Professional Summary:** Tailor to reflect the job’s key requirements.
- **Skills:** Integrate relevant skills from both the job description and the candidate’s resume.
- **Work Experience:** Revise bullet points to showcase the candidate’s most applicable experience.
- **Education & Certifications:** Highlight degrees, certifications, and training that are most relevant.
- **Additional Sections:** If applicable, include sections such as Projects, Awards, Languages, etc.

📌 **Guidelines:**
- Keep the formatting clear and professional for optimal readability.
- Include key phrases and keywords from the job posting to ensure ATS compatibility.
- Quantify achievements when possible (e.g., “Boosted sales by 30%”).
- Eliminate redundancy by focusing on impactful, concise information.

❌ **Do Not:**
- Use clichéd phrases like:
    - "Seasoned professional with a proven track record of success"
    - "Driven by a passion for innovation"
    - "Detail-oriented with strong problem-solving skills"
- Insert artificial job titles that don’t match the candidate’s role.
- Rely on excessive adjectives like “extremely,” “highly,” or “exceptionally.”
- Write repetitive or mechanical bullet points.
- Include phrases like “Here’s your custom resume for…” or career advice.
- Disrupt the flow with unnecessary headings (e.g., “Your summary is as follows:”)

✅ **Do:**
- Maintain a tone that is slightly informal yet professional.
- Demonstrate impact with specific examples (without using “I”) — for instance, “Increased customer retention by 20%...”
- Use action verbs to convey responsibilities: “Led,” “Developed,” “Transformed,” “Optimized.”
- Break up long paragraphs for easier reading.
- Integrate industry-specific language only when relevant.

The final output should feel like it was written by a real person, specifically for the job in question.

---
```

#### 7. **HTTP Module > Retrieve GPT Output**
- Use the **HTTP module** in Make.com to retrieve the resume text from the GPT-generated output (`{{8.data}}`).
- This output will contain the tailored resume content based on the job description.
- The **HTTP module** fetches this text from the GPT response, which will then be passed into the next step to create the Google Doc.

#### 8. **Google Docs > Create Document from Text**
- Use the **Google Docs module** to create a new document.
- The text received from the HTTP module (which is the GPT-generated resume) is inserted directly into the document, ensuring the resume is in the correct format.
- The text is inserted into the Google Docs document as a formatted resume.

#### 9. **Google Docs > Get Public Link**
- Once the document is created, retrieve the public link to the resume for sharing or further processing.

#### 10. **Airtable > Update Record**
- Store the public link to the Google Doc back into Airtable for tracking and easy access.

---

## 🧪 Testing Tips

```plaintext
- Test with 1–2 job listings before running full
- Log and review each module’s output
- Handle errors with Make’s “Error Handlers” and alerts
```

---

## 📥 Want This Prebuilt?

```plaintext
I can help you export the scenario or provide an Airtable template + resume doc. DM or comment “Interested” and I’ll share the link.
```
