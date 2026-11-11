# Python CI Docker Lab

## Practical Record (Write this in your exam record)

### 1. Aim
To build a Python application with pytest tests, containerize it using Docker, and automate testing, building and pushing of the image to Docker Hub using a GitHub Actions workflow.

### 2. Procedure
1. Create the project folder `python-ci-docker-lab` and move into it.
2. Create `app.py` containing a function `add(a, b)` and print statements, `tests/test_app.py` with a pytest test for `add`, `requirements.txt` containing `pytest==8.3.2`, a `Dockerfile` based on `python:3.11-slim`, and `.gitignore`.
3. Create the folder `.github/workflows` and save the workflow file `ci-dockerhub.yml` in it.
4. Run the application locally using `python app.py`.
5. Run the tests using `pytest -q`.
6. Build the Docker image using `docker build -t klassy01/python-ci-lab:local .`
7. Run the image using `docker run --rm klassy01/python-ci-lab:local`.
8. Initialize Git with `git init`, stage all files using `git add .` and commit using `git commit -m "init lab"`.
9. Rename the branch to main using `git branch -M main`.
10. Create an empty repository named `python-ci-docker-lab` on GitHub and link it using `git remote add origin https://github.com/<your-username>/python-ci-docker-lab.git`.
11. Push to GitHub using `git push -u origin main`.
12. In the GitHub repository, go to Settings > Secrets and variables > Actions and add `DOCKERHUB_USERNAME` and `DOCKERHUB_TOKEN`.
13. Open the Actions tab and watch the workflow run (checkout, tests, login, build and push).
14. Verify the image `klassy01/python-ci-lab` is published on Docker Hub.
15. Create a release tag using `git tag v1.0.0` and push it using `git push origin v1.0.0`.

### 3. Output
- `python app.py` prints:
  ```
  Hello from Python CI Lab!
  2 + 3 = 5
  ```
- `pytest -q` prints `1 passed`.
- `docker run --rm klassy01/python-ci-lab:local` prints the same two lines inside the container.
- The GitHub Actions workflow run shows a green tick with steps Checkout, Set up Python, Install deps, Run tests, Login to Docker Hub and Build and push completed.
- Docker Hub shows the repository `klassy01/python-ci-lab` with tag `latest` (and `v1.0.0` after tagging).

### 4. Result
A Python application with pytest tests was committed to GitHub, and a GitHub Actions workflow automatically ran the tests, built the Docker image, and pushed it to Docker Hub as `klassy01/python-ci-lab`.

---

## Execution Guide (VS Code)

> Replace `<your-username>` everywhere below with your GitHub username, and `klassy01` in the docker build command with your Docker Hub username if different.

1. All files (`app.py`, `tests/test_app.py`, `requirements.txt`, `Dockerfile`, `.gitignore`, `.github/workflows/ci-dockerhub.yml`) are already created in this folder. Open the `python-ci-docker-lab` folder in VS Code and open the terminal.

2. Run the app locally:

```
python app.py
```

   Expected output:
   ```
   Hello from Python CI Lab!
   2 + 3 = 5
   ```

3. Run the tests:

```
pytest -q
```

   Expected: `1 passed`.

4. Build the Docker image:

```
docker build -t klassy01/python-ci-lab:local .
```

5. Run the image:

```
docker run --rm klassy01/python-ci-lab:local
```

   Expected: the same two lines printed inside the container.

6. Initialize Git and push to GitHub. First create an **empty** repository named `python-ci-docker-lab` on github.com (New repository, do not add README):

```
git init
git add .
git commit -m "init lab"
git branch -M main
git remote add origin https://github.com/<your-username>/python-ci-docker-lab.git
git push -u origin main
```

7. Add GitHub Secrets. In the repository on GitHub go to **Settings > Secrets and variables > Actions > New repository secret** and add:
   - `DOCKERHUB_USERNAME` = your Docker Hub username
   - `DOCKERHUB_TOKEN` = Docker Hub access token (Docker Hub > Account Settings > Security > New Access Token)

8. The push already triggered the workflow. Open the repository > **Actions** tab, select the `ci-dockerhub` run and verify all steps are green. (If you added secrets after the first failed run, make a dummy commit and push again.)

9. Verify the image on Docker Hub at `https://hub.docker.com/r/klassy01/python-ci-lab` — tag `latest` should exist.

10. Optional release with a tag:

```
git tag v1.0.0
git push origin v1.0.0
```

   A new workflow run publishes an image tagged `v1.0.0` to Docker Hub.

## Troubleshooting

- **Workflow fails at "Login to Docker Hub" with unauthorized**: The secrets are missing or wrong. Re-add `DOCKERHUB_USERNAME` and `DOCKERHUB_TOKEN` exactly as spelled, then push again.
- **Tests fail in the workflow but pass locally**: Make sure `tests/test_app.py` was committed (check the repo files on GitHub); `git add .` picks it up only if saved.
- **`docker: error during connect`**: Start Docker Desktop and wait until it shows "running", then repeat the build.
- **Push rejected / authentication failed on git push**: Sign in with your GitHub credentials or Personal Access Token, or use GitHub Desktop / `gh auth login`.
- **pytest not recognized**: Run `pip install -r requirements.txt` first, or use `python -m pytest -q`.
