
---

# 🧠 Resume Screener – AI Trainer Fit Evaluator

This workflow processes resumes and determines a candidate’s suitability for an **AI Trainer** role by:

* Extracting text from uploaded PDF resumes
* Sending the content to OpenAI for analysis
* Parsing and structuring the results
* Appending the evaluated data into a connected **Google Sheet**

---

## 🚀 Workflow Steps

1. **Webhook**

   * Endpoint: `POST /webhook-test/resume-screener`
   * Accepts: `multipart/form-data` with `data` as the file key
   * Supports: Multiple file uploads (ensure Postman/Webhook settings support multiple binaries)

2. **Extract from File**

   * Reads and extracts text from each uploaded resume PDF.

3. **OpenAI (GPT-3.5-Turbo)**

   * Prompt instructs the model to extract:

     * Candidate Name
     * Total Years of Experience
     * Skills
     * Suitability for AI Trainer role (`Yes/No + Reason`)
     * Tags (e.g., `"Python"`, `"Cloud"`, `"Trainer"`)

4. **Edit Fields**

   * Transforms OpenAI response into structured fields:

     * `name`
     * `is_fit_for_ai_trainer`
     * `reason`
     * `skills`
     * `tags`
     * `timestamp` (formatted as `Wed, 23 Jul 2025`)

5. **Google Sheets**

   * Appends each evaluation to a sheet:

     * 🔗 [Google Sheet]
     * Sheet Name: `Sheet1`

---

## 📥 How to Use

1. Start the n8n workflow.
2. In Postman:

   * Set method to `POST`
   * Use `form-data` with key `data` (choose `File`) and attach one or more resumes
3. Send the request.
4. The workflow will:

   * Process each resume
   * Extract and format the evaluation
   * Append it to your Google Sheet

---

## ✅ Example Output (per resume)

| Candidate Name | Fit for AI Trainer | Reason                                       | Skills                     | Tags                        | Timestamp        |
|----------------| ------------------ | -------------------------------------------- | -------------------------- | --------------------------- | ---------------- |
| Abdullah Khan  | Yes                | Strong AI background and teaching experience | \["AI", "Python", "Azure"] | \["Trainer", "Cloud", "ML"] | Wed, 23 Jul 2025 |

---

## 🔧 Notes

* To support **multiple file uploads**, ensure:

  * Postman allows multiple `data` keys.
  * n8n webhook accepts all entries.
* Timestamp formatting uses: `{{$now.toFormat("ccc, dd LLL yyyy")}}`

---
