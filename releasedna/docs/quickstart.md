# ReleaseDNA first-run guide

## Environment verified
- Date: 19 November 2026
- OS: Windows 11
- Python version: 3.14.7
- Machine/session used: Local Windows environment
- Source branch/commit tested: docs/first-run-guide (commit SHA to be added after GitHub commit)
- AI assistance, if any: AI assistance was used for documentation guidance. Application verification and screenshots were performed personally.

## Setup from a fresh download

1. Download the repository ZIP from your GitHub fork.
2. Extract the ZIP file.
3. Open the inner `releasedna` folder containing `requirements-lock.txt`, `backend`, `frontend`, and `README.md`.
4. Open PowerShell in this folder.
5. Check the Python version:

```powershell
python --version
```

6. Create a virtual environment:

```powershell
python -m venv .venv
```

7. Install dependencies:

```powershell
.\.venv\Scripts\python.exe -m pip install -r requirements-lock.txt
```

8. Move into the backend folder:

```powershell
Set-Location backend
```

9. Generate the harmless synthetic archives:

```powershell
..\.venv\Scripts\python.exe -m app.adversarial_corpus
```

10. Start the application:

```powershell
..\.venv\Scripts\python.exe -m uvicorn app.api.main:app --host 127.0.0.1 --port 8000
```

11. Open the application in your browser:

http://127.0.0.1:8000/

Keep the terminal running while using the application.

## Official registry scan

- Package/version: `is-number@7.0.0`
- Actual scan ID: `8dbb1f1e-39f1-44b5-ab14-a05d6eca1fb5`
- Actual verdict: CLEAN
- Actual risk score: 0
- Artifact files: 4
- Source files: 15
- Comparison paths: 15
- Identical files: 4
- Added files: 0
- Removed files: 11
- Changed files: 0
- Artifact integrity: Digest matches the official registry metadata.
- Source confidence: corroborated
- Source tag: 7.0.0
- Publication time: 2018-07-04T15:08:58.238Z
- JSON download result: Successfully downloaded the evidence JSON.

The official registry scan returned a CLEAN verdict with a risk score of 0. No suspicious differences were detected within the available analysis scope.

![Official release result](images/clean-scan.png)

## Harmless local tampering demo

- Artifact: `demo-xz-artifact.tgz`
- Source: `demo-xz-source.tgz`
- Actual scan ID: `a391405e-6515-4b4d-b1c1-5cba2c84ee90`
- Actual verdict: INCONCLUSIVE
- Actual risk score: 30
- Artifact files: 4
- Source files: 4
- Comparison paths: 4
- Identical files: 3
- Added files: 0
- Removed files: 0
- Changed files: 1
- Artifact integrity: Not established.
- Source confidence: user_supplied
- Source tag: none
- Publication time: Unavailable

### Finding

- Rule: `SHELL_PIPELINE`
- File path: `m4/build.m4`
- Line: 2
- Byte offset: 31
- Finding description: New or increased capability indicator. Static heuristic, not proof of malicious intent.

The comparison displayed an **Anomaly spotted** badge and identified a change in `m4/build.m4`.

The overall verdict remained INCONCLUSIVE because the source archive was user-supplied and its identity could not be independently verified. No matched commit was found.

![Local comparison result](images/tampered-scan.png)

## Restart and history

After completing both scans:

1. Stopped the application server using Ctrl+C.
2. Restarted the server using the same startup command.
3. Opened the application history.
4. Reopened the official registry scan result.
5. Reopened the local archive comparison result.

Both results were successfully reopened from history after restarting the server.

## Five-minute judge walkthrough

1. Open ReleaseDNA in the browser.
2. Navigate to the Official registry tab.
3. Select `npm` as the ecosystem.
4. Enter `is-number` as the package name.
5. Enter `7.0.0` as the version.
6. Click Inspect release and wait for the scan to complete.
7. Review the verdict, risk score, counts and source confidence.
8. Click Download evidence JSON.
9. Navigate to the Local archives tab.
10. Select `demo-xz-artifact.tgz` as the artifact.
11. Select `demo-xz-source.tgz` as the source.
12. Click Compare archives.
13. Review the Anomaly spotted badge, finding and `m4/build.m4` diff.
14. Try the file search/filter.
15. Restart the server and reopen both results from history.

## Troubleshooting observed

[Document the actual errors encountered and the fixes that worked. Label untested suggestions separately.]

## Limitations

- CLEAN means no suspicious differences within the available inspection scope.
- It is not a malware-free guarantee or proof of verified build provenance.
- The local source archive is user-supplied, so its identity remains unverified.
- INCONCLUSIVE results must not be presented as CLEAN.
- The SHELL_PIPELINE finding is a static heuristic and is not proof of malicious intent.
- Mention any functionality or environment that could not be tested.
