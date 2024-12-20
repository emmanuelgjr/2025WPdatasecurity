# Table for Large Language Model (LLM) Red Teaming and Defense

## Table of Contents
- [Overview](#overview)
- [Table](#table)
  - [Column Descriptions](#column-descriptions)
- [Insight into Tools](#insight-into-tools)
- [Summary of Tools for Training and Use-Cases](#summary-of-tools-for-training-and-use-cases)

---

## Overview

This repository provides a comprehensive guide for red teaming and defending Large Language Models (LLMs). It includes:
- A table mapping each attack phase to the **Lockheed Martin Kill Chain** and **OWASP Top 10 for LLM 2025**.
- Detailed insights into tools for adversarial testing and mitigation.
- Practical use cases for both red teamers and defenders.

---

## Table

### Column Descriptions:
- **Kill Chain Phase**: The Lockheed Martin Kill Chain attack stage.
- **Methods**: Techniques used by attackers at this phase.
- **OWASP Top 10 for LLM 2025**: Relevant OWASP vulnerability category for LLMs.
- **Red Team Objectives**: Goals attackers aim to achieve at this stage.
- **Tools**: Tools used to execute the attack or defend against it.
- **Resources Exploited**: Systems, endpoints, or datasets targeted.
- **TTPs**: Techniques, Tactics, and Procedures employed.
- **Known Threat Actors**: Notable attackers using similar methods.
- **MITRE ATLAS Techniques**: Mappings to MITRE’s adversarial tactics.
- **Detection Techniques**: How defenders can identify the attack.
- **Mitigation Strategies**: How defenders can counteract the attack.
- **Real-World Examples**: Illustrative scenarios.
- **Purple Team Opportunities**: Collaboration points for red and blue teams.

| **Kill Chain Phase**         | **Methods**                                  | **OWASP Top 10 for LLM 2025**             | **Red Team Objectives**                   | **Tools**                                  | **Resources Exploited**                   | **TTPs**                                      | **Known Threat Actors**      | **MITRE ATLAS Techniques**                   | **Detection Techniques**                  | **Mitigation Strategies**                   | **Real-World Examples**                     | **Purple Team Opportunities**               |
|------------------------------|----------------------------------------------|------------------------------------------|------------------------------------------|--------------------------------------------|--------------------------------------------|-----------------------------------------------|--------------------------------|---------------------------------------------|--------------------------------------------|--------------------------------------------|---------------------------------------------|---------------------------------------------|
| **Reconnaissance**           | API probing, data scraping, model fingerprinting | **LLM02:2025 Sensitive Information Disclosure** | Identify model version, gather endpoints | Postman, Nmap, Burp Suite, ART           | Public endpoints, API documentation        | Metadata collection, API enumeration         | APT28, independent researchers | TA0001: Discover APIs, T1592: Gather data from APIs | Monitor traffic anomalies, API usage trends | Enforce strict API key access controls, rate limiting | Unauthorized scraping of GPT endpoints for downstream attacks | API monitoring during red team simulations  |
| **Weaponization**            | Prompt engineering, adversarial input generation | **LLM01:2025 Prompt Injection**              | Craft malicious prompts or adversarial payloads | TextAttack, Prompt Injectors, Hugging Face Transformers | Prompt injection frameworks, adversarial datasets | Craft inputs that bypass filters             | FIN7, cyber mercenaries       | TA0002: Generate adversarial inputs         | Analyze patterns of prompt anomalies        | Train models to detect adversarial prompts  | Prompt injection vulnerabilities exploited in real-world apps | Joint validation of model response to adversarial inputs |
| **Delivery**                 | Phishing prompts, API calls, supply chain attacks | **LLM03:2025 Supply Chain Vulnerabilities**  | Deliver payload to model or application  | Burp Suite, OWASP ZAP, Dependency-Check   | API endpoints, third-party plugins         | Exploit plugin vulnerabilities, API chaining  | Lazarus Group                 | TA0003: API exploitation                    | Track unusual API request headers           | Enforce strict input validation, audit integrations | Exploitation of compromised chatbot APIs in finance | Continuous endpoint monitoring for payload injection |
| **Exploitation**             | Code execution, sensitive data extraction    | **LLM02:2025 Sensitive Information Disclosure** | Exfiltrate sensitive data or alter responses | PyCaret, ART, Custom Python Scripts      | Memory, context history, token limits      | Hijack responses, overflow context length    | ShadowPad Operators            | TA0004: Context overflow, prompt manipulation | Observe abrupt changes in token usage or memory limits | Enforce token size limits, monitor abnormal context usage | Context overflow exploited in GPT models for data leaks | Analyze high token usage scenarios             |
| **Installation**             | Embedding backdoors, modifying datasets      | **LLM04:2025 Data and Model Poisoning**       | Establish persistence in model behavior  | Poisoning Tools, LLM Injectors, Hugging Face Transformers | Fine-tuning datasets, model weights        | Modify training data, introduce poisoned data | APT groups targeting AI labs  | TA0005: Insert malicious prompts/data      | Regularly audit training pipelines and datasets | Validate datasets, secure retraining processes | Poisoning of open-source datasets used for fine-tuning | Validate retraining with tampered inputs          |
| **Command and Control (C2)** | Response hijacking, prompt chaining          | **LLM06:2025 Excessive Agency**              | Maintain communication for prolonged exploitation | Dynamic Payload Generators, MITM Tools  | Compromised APIs, manipulated model responses | Establish persistent control via prompt chaining | FIN12, independent actors      | TA0006: Maintain access via prompt loops    | Detect unusual or persistent API calls      | Validate model responses with behavioral filters | Hijacking of chatbots in customer support for misinformation | Validate anomaly detection systems for long-term C2 |
| **Actions on Objectives**    | Data exfiltration, misinformation campaigns  | **LLM09:2025 Misinformation**                | Show operational disruption, spread misinformation | Automation Frameworks, Fake News Detection APIs | Model outputs, manipulated datasets        | Execute misinformation or extract insights    | Nation-state actors, hacktivists | TA0007: Exfiltrate sensitive content        | Audit for tampered or unusual model outputs | Validate output consistency, tamper detection | Campaigns using AI-generated misinformation to influence public opinion | Test misinformation detection algorithms          |

---

## Insight into Tools

### 1. **Postman**
- **Purpose**: API testing and endpoint exploration.
- **Features**: Simplifies crafting API calls and payloads for reconnaissance.
- **Installation**: [Download Postman](https://www.postman.com/downloads/)
- **Example Command**:
  ```bash
  curl -X GET https://api.example.com/endpoint
  ```
- **Use Case**: Explore APIs, craft requests, and identify misconfiguration.
- **Training Resources:**
  **Postman Learning Center:** Postman Tutorials
  **API Security Testing Guide:** Focuses on API fuzzing and endpoint security.
- **Use-Case Example:**
  **Red Team:** Craft various API requests with malformed headers or data to test API validation.
  **Defenders:** Use Postman’s Monitor feature to test endpoint behaviours under various conditions.

---

### 2. **TextAttack**
- **Purpose**: Generate adversarial text samples.
- **Features**: NLP adversarial attacks like synonym replacement or paraphrasing.
- **Installation**: 
  ```bash
  pip install textattack
  ```
- **Use Case**: Test LLM robustness against adversarial text manipulations.
- **Training Resources:**
  **TextAttack Documentation:** [TextAttack GitHub](https://github.com/QData/TextAttack)
  **Adversarial NLP Workshops:** Look for workshops at ACL, NeurIPS, or AI Village at DEFCON.
- **Use-Case Example:**
  **Red Team:** Generate paraphrased adversarial inputs to bypass LLM-based spam filters.
  **Defenders:** Simulate attacks to train LLMs to recognize adversarial inputs.


---

### 3. **Burp Suite**
- **Purpose**: Intercept, modify, and deliver malicious payloads.
- **Features**: Web and API fuzzing, proxying requests, and manipulating traffic.
- **Installation**: [Download Burp Suite](https://portswigger.net/burp/communitydownload)
- **Use Case**: Deliver crafted payloads and manipulate API responses.
- **Training Resources:**
  **Burp Academy:** [Burp Suite Web Security Academy](https://portswigger.net/web-security)
  **API Testing Guides:** Covers exploitation techniques for APIs.
- **Use-Case Example:**
  **Red Team:** Intercept API traffic and test response manipulation.
  **Defenders:** Configure Intruder to stress-test API endpoints for input validation.

---

### 4. **Adversarial Robustness Toolbox (ART)**
- **Purpose**: Evaluate model resilience to adversarial attacks.
- **Features**: Supports adversarial testing for LLMs.
- **Installation**:
  ```bash
  pip install adversarial-robustness-toolbox
  ```
- **Use Case**: Create adversarial inputs and test model robustness.
- **Training Resources:**
  **ART Documentation:** [ART GitHub](https://github.com/Trusted-AI/adversarial-robustness-toolbox)
  **Workshops:** Tutorials at MLSecOps conferences or MITRE ATLAS events.
- **Use-Case Example:**
  **Red Team:** Craft adversarial inputs to confuse LLM predictions.
  **Defenders:** Validate model resilience by injecting adversarial examples into test datasets.


---

### 5. **Hugging Face Transformers**
- **Purpose**: Model deployment, fine-tuning, and testing.
- **Features**: Easy customization for adversarial or defensive training.
- **Installation**:
  ```bash
  pip install transformers
  ```
- **Use Case**: Train and modify LLMs to detect vulnerabilities.
- **Training Resources:**
  **Hugging Face Tutorials:** [Hugging Face Docs](https://huggingface.co/docs)
  **Fine-Tuning Guides:** Learn to fine-tune and secure models.
- **Use-Case Example:**
  **Red Team:** Modify pretrained models to insert backdoors.
  **Defenders:** Train models to detect and reject manipulated datasets.

---

### 6. **OWASP ZAP**
- **Purpose**: Automated security testing for APIs and web apps.
- **Features**: Input fuzzing, vulnerability scanning, and traffic analysis.
- **Installation**: [Download OWASP ZAP](https://www.zaproxy.org/download/)
- **Use Case**: Identify API vulnerabilities and misconfigurations.
- **Training Resources:**
  **OWASP ZAP Tutorials:** [ZAP Documentation](https://www.zaproxy.org/docs/)
  **API Testing Courses:** Look for API penetration testing courses that include ZAP.
- **Use-Case Example:**
  **Red Team:** Perform fuzzing attacks on API endpoints to test their resilience.
  **Defenders:** Set up automated scans to detect misconfigurations.

---

### 7. **Poisoning Tools**
- **Purpose**: Create datasets to embed malicious patterns.
- **Features**: Inject adversarial data or backdoors during fine-tuning.
- **Use Case**: Simulate risks from tainted datasets and audit dataset integrity.
- **Training Resources:**
  **Workshops:** Offered at MLSecOps conferences.
  **Online Courses:** Search for “dataset integrity and ML security” courses on Coursera or Udemy.
- **Use-Case Example:**
  **Red Team:** Demonstrate risks from tainted datasets in open-source projects.
  **Defenders:** Audit datasets to detect and remove malicious patterns.

---

## Summary of Tools for Training and Use-Cases

| **Tool**              | **Primary Function**                  | **Key Use Case**                              |
|-----------------------|---------------------------------------|----------------------------------------------|
| **Postman**           | API testing                          | Test API validation and misconfigurations.   |
| **TextAttack**        | Adversarial NLP                      | Generate adversarial text samples.           |
| **Burp Suite**        | Traffic interception and fuzzing     | Manipulate API responses and test resilience.|
| **ART**               | Adversarial robustness testing       | Create adversarial inputs to simulate attacks.|
| **Hugging Face**      | LLM fine-tuning                      | Train, modify, and secure LLMs.              |
| **OWASP ZAP**         | API fuzzing                          | Identify API vulnerabilities.                |

---
