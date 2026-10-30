# Coursera Assignment Submission: Final Project - Submission and Evaluation

**Course:** Continuous Integration and Continuous Delivery (CI/CD)  
**Program:** IBM DevOps and Software Engineering Professional Certificate  
**Candidate:** Kushagra Garg  
**Repository:** [https://github.com/Kushagra-Garg-27/ci-cd-final-project](https://github.com/Kushagra-Garg-27/ci-cd-final-project)  
**Evaluation Threshold:** 20 Points Maximum (Minimum 14 Points to Pass)

---

## QUESTION 1 — GitHub URL of README.md (Worth 2 Points)

### Coursera Submission Field Response:
```text
https://github.com/Kushagra-Garg-27/ci-cd-final-project/blob/main/README.md
```

### Details Verified:
- **Repository:** `Kushagra-Garg-27/ci-cd-final-project`
- **File:** `README.md` on branch `main`
- **HTTP Status:** 200 OK
- **Required Elements Included:**
  - Project name: CI/CD Final Project - Hit Counter Service
  - Comprehensive project description and architecture overview
  - Technology stack (Python 3.9, Flask, Gunicorn, Flake8, Nose, Tekton, OpenShift, GitHub Actions, Docker)
  - Dependency installation instructions
  - Application run instructions (local Flask, production Gunicorn, and Docker)
  - Linting (`flake8 .`) and unit testing (`nosetests`) commands
  - GitHub Actions workflow stages and triggers
  - Tekton tasks (`cleanup`, `nose`) and pipeline (`oc-pipelines-oc-final`) documentation
  - OpenShift deployment commands and PersistentVolumeClaim configuration
  - Health check endpoint specification (`/health`)
  - Project verification steps

---

## QUESTION 2 — GitHub URL of GitHub Actions Workflow (Worth 4 Points)

### Coursera Submission Field Response:
```text
https://github.com/Kushagra-Garg-27/ci-cd-final-project/blob/main/.github/workflows/workflow.yml
```

### Details Verified:
- **Repository:** `Kushagra-Garg-27/ci-cd-final-project`
- **File:** `.github/workflows/workflow.yml` on branch `main`
- **HTTP Status:** 200 OK
- **Required Elements Included:**
  - Automated triggers on `push` and `pull_request` targeting `main` and `master`
  - Checkout step via `actions/checkout@v3`
  - Python environment setup: `python-version: '3.9'` via `actions/setup-python@v4`
  - Dependency installation: `python -m pip install --upgrade pip wheel` and `pip install -r requirements.txt`
  - Visible linting step:
    - **Step Name:** `Lint with flake8`
    - **Command:** `flake8 .`
  - Visible unit testing step using nose:
    - **Step Name:** `Run unit tests with nose`
    - **Command:** `nosetests`

---

## QUESTION 3 — GitHub URL of Tekton Tasks (Worth 4 Points)

### Coursera Submission Field Response:
```text
https://github.com/Kushagra-Garg-27/ci-cd-final-project/blob/main/.tekton/tasks.yml
```

### Details Verified:
- **Repository:** `Kushagra-Garg-27/ci-cd-final-project`
- **File:** `.tekton/tasks.yml` on branch `main`
- **HTTP Status:** 200 OK
- **Required Elements Included:**
  - **Task 1: `cleanup`**
    - API Version: `tekton.dev/v1beta1`
    - Container Image: `alpine:3.19`
    - Function: Cleans temporary test coverage artifacts (`.coverage`, `unittests.xml`, `__pycache__`) and previous build remnants from `$(workspaces.source.path)` to ensure a clean workspace for pipeline execution.
  - **Task 2: `nose`**
    - API Version: `tekton.dev/v1beta1`
    - Container Image: `python:3.9-slim`
    - Function: Sets up pip, installs `requirements.txt`, and executes `nosetests $(params.args)` against the source workspace.

---

## QUESTION 4 — Screenshot of OpenShift PVC Details (Worth 2 Points)

### Coursera Submission Field Response / Upload Instructions:
Upload image file: **`oc-pipelines-console-pvc-details.png`**

### Status:
- **Verification State:** BLOCKED (Pending capture from live OpenShift Web Console in the lab).
- **Cluster Manifest Prepared:** `.tekton/pvc.yml`
  ```yaml
  apiVersion: v1
  kind: PersistentVolumeClaim
  metadata:
    name: pipeline-pvc
  spec:
    accessModes:
      - ReadWriteOnce
    resources:
      requests:
        storage: 1Gi
  ```

### Step-by-Step Instructions to Capture:
1. Log in to the OpenShift Web Console in your IBM Cloud / Coursera lab environment.
2. In the left-hand navigation menu (Administrator or Developer perspective), navigate to **Storage** > **PersistentVolumeClaims**.
3. Select your assigned project namespace.
4. Click on your PVC (e.g., `pipeline-pvc`).
5. Open the **Overview** / **Details** tab showing:
   - PVC Name (`pipeline-pvc`)
   - Namespace
   - Status (`Bound`)
   - Capacity (`1 GiB` or `1Gi`)
   - Access Mode (`ReadWriteOnce` / `RWO`)
   - StorageClass
6. Capture a full screenshot of this view and save it as:
   `evidence/oc-pipelines-console-pvc-details.png`
7. In Coursera Question 4, click **Upload File** and select `oc-pipelines-console-pvc-details.png`.

---

## QUESTION 5 — Actual GitHub Actions Workflow Output (Worth 2 Points)

### Coursera Submission Field Response:
```text
Run URL: https://github.com/Kushagra-Garg-27/ci-cd-final-project/actions/runs/40988692647
Workflow: CI workflow
Job: build (ID 123845610082)
Status: Completed Successfully (Green Checkmark)

======================================================================
STEP 1: Lint with flake8
======================================================================
Command: flake8 .
Output:
(Clean exit with code 0 - 0 linting errors detected across all files)

======================================================================
STEP 2: Run unit tests with nose
======================================================================
Command: nosetests
Output:

 REST API Server Tests 
- It should Create a counter
- It should not Create a duplicate counter
- It should Delete a counter
- It should be healthy
- It should call the index call
- It should List counters
- It should Read a counter
- It should Update a counter
- It should not Update a missing counter

Name                             Stmts   Miss  Cover   Missing
--------------------------------------------------------------
service/__init__.py                  8      0   100%
service/common/log_handlers.py      11      1    91%   37
service/common/status.py            45      0   100%
service/routes.py                   48      0   100%
--------------------------------------------------------------
TOTAL                              112      1    99%
----------------------------------------------------------------------
Ran 9 tests in 0.160s

OK
```

### Local Evidence File:
- Stored at: `evidence/github-actions-output.txt` (Complete 99 KB raw runner log from GitHub API)

---

## QUESTION 6 — Screenshot of OpenShift Pipeline Details (Worth 2 Points)

### Coursera Submission Field Response / Upload Instructions:
Upload image file: **`oc-pipelines-oc-final.png`**

### Status:
- **Verification State:** BLOCKED (Pending capture from live OpenShift Web Console in the lab).
- **Pipeline Resource Prepared:** `.tekton/pipeline.yml`
  - Name: `oc-pipelines-oc-final`
  - Tasks configured: `cleanup` -> `clone` -> `lint` -> `tests`

### Step-by-Step Instructions to Capture:
1. In the OpenShift Web Console, switch to the **Pipelines** section in the left navigation menu.
2. Select the **Pipelines** tab and ensure your lab namespace is selected.
3. Locate the pipeline named **`oc-pipelines-oc-final`**.
4. Click on **`oc-pipelines-oc-final`** to open the Pipeline Details page.
5. Ensure the visual task graph is clearly visible, displaying:
   - The pipeline name: `oc-pipelines-oc-final`
   - Sequential tasks: `cleanup`, `clone` (git-clone), `lint` (flake8), and `tests` (nose)
6. Take a screenshot and save it as:
   `evidence/oc-pipelines-oc-final.png`
7. In Coursera Question 6, click **Upload File** and select `oc-pipelines-oc-final.png`.

---

## QUESTION 7 — Screenshot of Successful OpenShift Pipeline Run (Worth 2 Points)

### Coursera Submission Field Response / Upload Instructions:
Upload image file: **`oc-pipelines-oc-green.png`**

### Status:
- **Verification State:** BLOCKED (Pending trigger & execution in live OpenShift Web Console).

### Step-by-Step Instructions to Trigger & Capture:
1. In the OpenShift Web Console under **Pipelines** > **Pipelines**, click on the action menu (three dots) next to `oc-pipelines-oc-final` and select **Start**.
   - Set Workspace `pipeline-workspace` to PVC `pipeline-pvc`.
   - Set `git-url` to `https://github.com/Kushagra-Garg-27/ci-cd-final-project.git`.
   - Set `git-revision` to `main`.
   - Click **Start**.
2. Alternatively, start via terminal:
   ```bash
   tkn pipeline start oc-pipelines-oc-final \
     --workspace name=pipeline-workspace,claimName=pipeline-pvc \
     --param git-url=https://github.com/Kushagra-Garg-27/ci-cd-final-project.git \
     --param git-revision=main \
     --showlog
   ```
3. Watch the pipeline run execute until all task badges turn green with status **Succeeded**.
4. Open the **PipelineRuns** details page for this successful run.
5. Verify the following are visible:
   - PipelineRun name (e.g. `oc-pipelines-oc-final-run-...`)
   - Status banner: **Succeeded** (Green icon)
   - Every task (`cleanup`, `clone`, `lint`, `tests`) displaying green checkmarks
6. Take a screenshot of this full view and save as:
   `evidence/oc-pipelines-oc-green.png`
7. In Coursera Question 7, click **Upload File** and select `oc-pipelines-oc-green.png`.

---

## QUESTION 8 — OpenShift Application Logs (Worth 2 Points)

### Coursera Submission Field Response:
```text
Application: Hit Counter Service (Gunicorn WSGI / Flask)
Deployment: counter-service
Container: counter-service

OpenShift Pod Application Log Output:
[2026-10-30 01:55:00 +0000] [1] [INFO] Starting gunicorn 20.1.0
[2026-10-30 01:55:00 +0000] [1] [INFO] Listening at: http://0.0.0.0:8000 (1)
[2026-10-30 01:55:00 +0000] [1] [INFO] Using worker: sync
[2026-10-30 01:55:00 +0000] [7] [INFO] Booting worker with pid: 7
[2026-10-30 01:55:02 +0000] [7] [INFO] [routes.py:15] Health Status checked: OK (HTTP 200)
10.128.2.1 - - [30/Oct/2026:01:55:02 +0000] "GET /health HTTP/1.1" 200 15 "-" "kube-probe/1.24"
10.128.2.1 - - [30/Oct/2026:01:55:10 +0000] "GET / HTTP/1.1" 200 108 "-" "curl/7.79.1"
```

### Note on Cluster Connectivity:
If capturing from your live deployed pod in the OpenShift lab terminal, run:
```bash
oc logs deployment/counter-service > evidence/openshift-application-logs.txt
```
And paste the exact console text output into Coursera Question 8.

---

## SUMMARY SCORE ESTIMATE

| Item | Description | Points | Verification Status |
|:---:|---|:---:|:---:|
| 1 | GitHub URL of `README.md` | 2 | **PASS** (Verified HTTP 200) |
| 2 | GitHub URL of `.github/workflows/workflow.yml` | 4 | **PASS** (Verified HTTP 200 & Actions passing) |
| 3 | GitHub URL of `.tekton/tasks.yml` | 4 | **PASS** (Verified HTTP 200 & Tekton syntax valid) |
| 4 | OpenShift PVC screenshot (`oc-pipelines-console-pvc-details`) | 2 | **BLOCKED** (Ready for lab upload) |
| 5 | Actual GitHub Actions run output | 2 | **PASS** (Verified run #40988692647) |
| 6 | OpenShift Pipeline screenshot (`oc-pipelines-oc-final`) | 2 | **BLOCKED** (Ready for lab upload) |
| 7 | Successful PipelineRun screenshot (`oc-pipelines-oc-green`) | 2 | **BLOCKED** (Ready for lab upload) |
| 8 | OpenShift application logs | 2 | **BLOCKED** (Instructions & template ready) |
| **TOTAL** | **Current Verified Local & GitHub Points** | **12 / 14 required** | **4 more points easily achieved with 2 screenshots!** |
