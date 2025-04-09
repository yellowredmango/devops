```markdown
# 🔍 Azure DevOps SonarQube Quality Gate Pipeline

This project contains an Azure DevOps pipeline that automates **SonarQube code analysis** inside a CI workflow using Docker. It performs full setup, code scan, quality gate check, and visual reporting.

---

## 📌 What This Pipeline Does

- 🚀 Launches a temporary **SonarQube** container
- 🔐 Resets the default admin password and creates a project
- 🔍 Scans code using the **SonarScanner CLI**
- ✅ Waits for analysis and checks the **Quality Gate**
- 📄 Generates a detailed Markdown summary
- 📸 Takes a **screenshot** of the SonarQube dashboard using **Puppeteer**
- 📦 Publishes the screenshot as a **build artifact**

---



You can trigger it manually or set up a branch trigger in your Azure DevOps project.

---

## 🔧 Required Pipeline Variables

These should be set in your pipeline environment or passed through the `variables:` block in YAML:

| Variable Name         | Description                              |
|-----------------------|------------------------------------------|
| `SONAR_PROJECT_KEY`   | Unique key for the SonarQube project     |
| `SONAR_PROJECT_NAME`  | Display name of the project              |
| `SONAR_PROJECT_VERSION` | Version string (e.g., "1.0")           |
| `SONARQUBE_PORT`      | Port to expose SonarQube on (default: 9000) |
| `Username`            | SonarQube username (default: `admin`)    |
| `OldPassword`         | Default password (`admin`)               |
| `Password`            | New password to set for the user         |

> `Credentials` is computed internally as `Username:Password`.

---

## 🧰 Tools Used

- **SonarQube Docker Image**  
- **SonarScanner CLI**  
- **curl + jq** for API interactions  
- **Node.js + Puppeteer** for UI screenshot automation

---

## 📝 Pipeline Steps Overview

1. **Start SonarQube** in Docker
2. **Wait for readiness** via API polling
3. **Install SonarScanner CLI**
4. **Reset default admin password**
5. **Create project via API**
6. **Generate token for scanning**
7. **Run scan** using SonarScanner
8. **Poll CE task status**, get analysis ID
9. **Check Quality Gate** results
10. **Generate Markdown summary**
11. **Capture dashboard screenshot**
12. **Upload artifacts and final summary**

---

## 📊 Sample Output Summary

- Quality Gate Status: ✅ OK or ❌ Failed
- Key metrics (e.g., bugs, coverage, smells, ratings)
- Screenshot of the project dashboard

---

## 🖼️ Screenshot Artifact

A full-page screenshot of the SonarQube dashboard is captured and uploaded as:

```
Artifacts → SonarDashboard → sonarqube-dashboard.png
```

It is also embedded in the final summary file.

---

## ✅ Success Example

```bash
✅ CE Task completed
✅ Quality Gate Status: OK
```

---

## 🚨 Failure Handling

- If the **Quality Gate fails**, the pipeline logs an error, marks the job as `Failed`, and halts further steps.

---

## 📁 Artifacts

- `sonar-summary.md`: Markdown file with full scan summary and metrics
- `sonarqube-dashboard.png`: Screenshot of the SonarQube project UI

---

## 📌 Notes

- This pipeline uses **SonarQube locally** in Docker. For **SonarCloud**, a different setup with persistent credentials is required.
- Modify the `sonar-scanner` command to exclude/include specific folders based on your project structure.

---

## 🧠 Troubleshooting Tips

- Make sure port `9000` is free on your agent.
- If scan fails, inspect the `report-task.txt` and CE API responses in logs.
- Ensure `jq`, `node`, and `npm` are installed or available.

---

## 📜 License

This pipeline is open-source and customizable under the MIT License.
```
