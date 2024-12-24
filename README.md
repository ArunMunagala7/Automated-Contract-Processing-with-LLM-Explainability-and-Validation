# Zenskar-Assignment

# Automated Contract Processing with LLM Explainability and Validation

---

## **Introduction**

This project involves extracting and validating key fields from contracts, generating reasoning logs for each extracted field, creating visual overlays on PDFs, and integrating with the Zenskar Contract API. The system employs advanced techniques like dynamic prompting, validation with regex/SpaCy, LLM explainability, and API integration to provide a comprehensive solution for automated contract processing.

---

## **Approach and Thought Process**

### **1. Dynamic Prompt Engineering**

The prompt engineering approach in this project is central to ensuring accurate field extraction from contracts. Below are the key techniques and considerations:

#### **Dynamic Prompting**
- **Dynamic Variability**: The prompt was designed dynamically with variations for each field. Fields such as "Contract ID," "Payment Terms," and "Billing Frequency" had unique instructions tailored to their expected structure and context within the document. For example:
  - **Contract ID**: "Look for unique identifiers near terms like 'Agreement Number.'"
  - **Payment Terms**: "Identify phrases like 'Net X' or 'Due in X days.'"
  - **Billing Frequency**: "Detect recurring terms like 'Monthly' or 'Weekly.'"
  
- **Dynamic Construction**: The prompt was dynamically constructed in two stages:
  1. **Extract fields** as they appear in the text.
  2. **Transform fields** into validated formats according to pre-defined rules.

#### **Few-Shot Prompting**
- Examples of valid JSON outputs were included in earlier stages to provide the LLM (Gemini) with context on the desired format. This technique enhanced the accuracy and structure of the extracted data.

#### **Chain of Thought (CoT) Prompting**
- CoT prompting was utilized to guide the LLM through multi-step reasoning for:
  - First extracting fields as raw text.
  - Then transforming them into validated, structured formats.

#### **LLM Explainability**
- Justifications for every field extracted were explicitly requested via the prompt. This led to the generation of detailed reasoning logs (e.g., "Detected near the 'Agreement Number' label in the document"). This added layer of explainability ensures transparency and traceability for each extraction.

---

### **2. Challenges Faced and Resolutions**

#### **a. Designing Accurate Regex/SpaCy Patterns**
- **Challenge**: Creating regex patterns to validate diverse formats like monetary values, dates, and alphanumeric IDs while maintaining flexibility for variations.
- **Resolution**: Iteratively refined patterns based on test cases. For example:
  - Dates: `^\d{4}-\d{2}-\d{2}$`
  - Currency: `^\$\d{1,3}(,\d{3})*(\.\d{2})?$`
  - Alphanumeric fields: `^[A-Za-z0-9\s\-]+$`
- SpaCy was also employed for advanced validations, such as detecting organization names.

#### **b. Implementing Dynamic Prompting**
- **Challenge**: Ensuring the prompt adapted dynamically to different fields and contexts.
- **Resolution**: The prompt was modularized with specific instructions for each field, and the system dynamically assembled the final prompt for each contract.

#### **c. Coding PDF Highlighting with PyMuPDF**
- **Challenge**: Ensuring extracted fields were accurately highlighted in PDFs.
- **Resolution**: Used PyMuPDF's `search_for` and `add_highlight_annot` methods. Iterated over all extracted fields and validated instances before applying highlights.

#### **d. API Integration with Zenskar**
- **Challenge**: Resolving API authorization errors (e.g., 403 errors).
- **Resolution**: Ensured correct inclusion of API keys, organization IDs, and payload formats as per the Zenskar documentation.

#### **e. Validation and Explainability Logs**
- **Challenge**: Logging validation and reasoning details without disrupting the flow of the code.
- **Resolution**: Implemented separate `StringIO` buffers to capture logs for validation and explainability. This allowed logs to be stored in string variables for seamless reporting.

---

## **Instructions for Running the Code**

1. **Vertex AI Setup**:
   - Create a Vertex AI account and ensure you have **Gemini credits**.
   - Authenticate using your Vertex AI credentials.

2. **Setup Environment**:
   - Install dependencies:
     ```bash
     pip install fitz spacy requests
     ```
   - Configure the required API keys and organization ID for the Zenskar API.

3. **Run the Code**:
   - Execute the script step by step to:
     1. Generate LLM outputs for field extraction.
     2. Validate and transform extracted fields.
     3. Highlight fields in the PDF and generate logs.
     4. Post the validated data to the Zenskar API.

4. **Output Details**:
   - **Highlighted PDF**: Saved with visual overlays for extracted fields.
   - **Validation and Explainability Logs**: Captured separately as string variables (`validation_logs_as_string` and `explainability_logs_as_string`) for further analysis.
   - **Zenskar API Response**: JSON response indicating the status of the contract creation.

---

## **Summary**

This project exemplifies the integration of LLMs for dynamic prompting, regex/SpaCy validation, PDF annotation, and API interaction. The approach emphasizes explainability, transparency, and robust error handling to ensure accurate contract processing.
