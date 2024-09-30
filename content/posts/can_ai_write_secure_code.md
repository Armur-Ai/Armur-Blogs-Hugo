---
title: "Can AI Write Secure Code?"
date: 2024-09-29T15:20:22+05:30
image: "https://i.ibb.co/k8tfQCH/Bzyvtd-P-famous-paintings-wallpaper.jpg"
tags: ["secure", "ai", "chatgpt"]
---

Generative AI is transforming the way software is built, offering potential benefits on par with the impact of the internet, mobile devices, and cloud computing. According to Gartner, AI has the potential to automate up to 30% of the work developers currently perform, freeing them to focus on higher-value tasks. AI coding assistants like GitHub Copilot are proving to be game-changers, boosting productivity by up to 50% and enhancing code quality through faster testing cycles. But the question remains—can AI write secure code?

In this article, we’ll explore whether AI-generated code can be trusted for secure applications, the limitations it faces, and how developers can mitigate associated security risks.

### The Current Capabilities of AI in Writing Secure Code

AI coding assistants have shown promise in generating clean, syntactically correct code, especially when it comes to identifying and avoiding certain common weaknesses (CWEs). For example, GitHub Copilot has demonstrated proficiency in steering clear of vulnerabilities like:

- CWE 787: Out-of-Bounds Write
- CWE 79: Cross-Site Scripting (XSS)
- CWE 125: Out-of-Bounds Read
- CWE 119: Improper Restriction of Operations

These vulnerabilities are relatively easier to avoid because they are syntax-based issues—errors related to how code is structured. However, the real challenge lies in more complex security flaws, such as improper input validation and the interaction between applications and external data. AI tools often struggle with these more nuanced security problems, leading to potentially dangerous vulnerabilities.

### The Limitations of AI-Generated Code in Security

While AI tools can speed up development and reduce basic errors, they have limitations when it comes to ensuring secure code in more complex scenarios. A few key issues include:
Contextual Misunderstanding: AI tools lack deep contextual awareness, meaning they can miss security issues tied to business logic or the specific environment where the code is deployed.

- **Lack of Input Validation:** AI-generated code often fails to properly validate user inputs, leaving applications open to attacks such as CWE 20: Improper Input Validation and CWE 502: Deserialization of Untrusted Data.
- **Vulnerabilities Introduced by AI:** Multiple studies, including one from 2021 titled “Asleep at the Keyboard? Assessing the Security of GitHub Copilot’s Code Contributions,” found that about 40% of Copilot-generated programs contained vulnerabilities. Another study from 2023 revealed that Copilot-generated code had vulnerabilities about one-third of the time.
- **The evidence points to a clear conclusion:** while AI can help streamline coding tasks, it cannot reliably ensure secure code, particularly in complex or sensitive applications.

### How Armur Addresses AI-Generated Code Security

With the increasing adoption of AI-generated code, securing it has become more complex. This is where Armur come into play. Armur uses Large Language Model (LLM) agents to build security tooling that scans AI-generated code for vulnerabilities. It offers a comprehensive solution to the challenges AI introduces, from detecting vulnerabilities to protecting LLMs from adversarial attacks.

**Armur’s Key Features Include:**

- **LLM Agents for Code Vulnerability Scanning:** Armur’s LLM-driven tools can automatically scan AI-generated code to detect critical vulnerabilities, ensuring that developers don’t have to manually validate each piece of code. This is crucial as the speed at which AI generates code often outpaces the ability of developers to scrutinize it for security issues.
- **SAST (Static Application Security Testing):** Armur specializes in detecting common vulnerabilities and exposures (CVEs) by scanning AI-generated code for OWASP Top 10 security risks, such as injection flaws, insecure configurations, and improper input handling. It supports languages like Go, Rust, Python, JavaScript, and even Solidity smart contracts.
- **DAST (Dynamic Application Security Testing):** In addition to static analysis, Armur’s LLM-based tools can run dynamic security tests, simulating real-world attack scenarios to catch vulnerabilities that only appear during runtime.
- **VAPT (Vulnerability and Penetration Testing):** Armur's LLM agents perform vulnerability and penetration testing, identifying both coding flaws and potential security breaches within containers and infrastructure as code.
  Self-Hostable LLM Protection: Armur has also developed a self-hostable reverse proxy server that uses a tiny LLM to protect large models from adversarial attacks and prompt injections. This makes sure that the AI models generating or securing code aren’t themselves compromised.

### Strategies to Secure AI-Generated Code

While tools like Armur are essential for securing AI-generated code, organizations must adopt a holistic approach to mitigate risks. Here are some strategies that can help:

**1. Establish Clear Guidelines for AI Use**

Organizations should create clear policies outlining when and how AI-generated code should be used. These policies should address:
Permitted use cases for AI coding assistants (e.g., writing boilerplate code but not for sensitive security modules).
Code review processes to ensure AI-generated code adheres to security standards.
Licensing and intellectual property protections for AI-generated code.

**2. Vet AI Tools Thoroughly**

Before adopting any AI coding assistant, organizations should evaluate the tool for compliance with security standards and transparency. Questions to consider include:
What data is the AI model trained on? Ensure it doesn’t inadvertently use insecure or outdated libraries.
How does the tool handle IP? Ensure that AI tools do not introduce risks by using or generating code that infringes on intellectual property.
How are vulnerabilities managed? Vet the AI tool’s ability to identify and address security weaknesses. Tools like Armur make this easier by scanning for vulnerabilities in real-time using advanced LLMs.

**3. Implement Rigorous Verification Processes**

Despite AI’s capabilities, human developers must validate and review AI-generated code. Recommended practices include:
Static Application Security Testing (SAST): Use SAST tools like Armur’s to analyze the structure and syntax of AI-generated code for security vulnerabilities.
Dynamic Application Security Testing (DAST): Test AI-generated code in a real-world environment to uncover any hidden vulnerabilities that may appear at runtime.
Manual Code Reviews: Developers should always conduct manual reviews of AI-generated code, particularly for security-sensitive applications, to catch vulnerabilities AI might miss.

**4. Continuous Monitoring and Learning**

AI tools can improve over time through exposure to more secure code patterns and ongoing feedback. Organizations should implement continuous learning systems that allow AI coding assistants to enhance their security awareness. This includes:
Integrating security feedback into AI-generated code reviews.

Regularly updating AI models, like those used by Armur, to ensure they learn from previous vulnerabilities and apply security best practices.

**5. Focus on Training Developers**

Developers must not rely entirely on AI for writing secure code. Providing continuous training on security best practices ensures that human oversight complements the use of AI tools. Security training should focus on:

- Identifying common vulnerabilities that AI tools may miss, such as improper input validation or deserialization attacks.
- Encouraging collaboration between developers and AI tools to improve overall security.

### Can AI Write Secure Code?

In short, AI can write secure code to an extent, but it still requires human oversight, verification, and continuous improvement. While AI coding assistants like GitHub Copilot can avoid basic syntactical flaws and boost productivity, they often struggle with deeper, more complex vulnerabilities related to input handling, business logic, and security testing. Armur provide a solution by leveraging LLMs to scan AI-generated code for critical vulnerabilities, ensuring that security issues are identified and remediated faster and more comprehensively.

As AI continues to evolve and play a larger role in software development, the onus remains on developers to ensure that AI-generated code is thoroughly tested and secure before deployment. With the right policies, tools, and processes—such as those provided by Armur—AI can help accelerate development without compromising the security of your applications.
