**In this Repository we learned about**
- Create Workflows in .github/workflows/"your_yml_file.yml"
- Run Shell command using run
- Events Types & Filters Events
- Envets Controll for specific branches and specific folder or file path
- Skip workflow while your git commits msg contains [skip ci] or other official keyword.
- cancell workflow manually.
- write multiple jobs and steps in single workflows pipelines.
- Run 2nd Jobs after 1st Jobs completed using needs: "job_name" keywords.

- Bydefault if you dont specify needs: keyword , its working as **Run workflows Paralally**.

**Now, Next Topic will be Create Artifact and use That Artifact in second jobs.**

**Go to** [github action part 2 (aritfact and manuy more)](https://github.com/GitEic-Bhavin/gh-action-P2_repo.git) 



##### Know about Github Actions
1) What is Github Actions ?
- Github Actions is a CI/CD platform that enables automations of software workflows directly form your github repository. It allows you to build, test and deploy your code right from github, using YAML file to define the automation process.

2) What are the key components of a Github Actions Workflow ?
- The key componets of a Github Actions Workflow are as follows:
    - ```Workflow```: A YAML file that defines a series of jobs to be executed. Workflows are triggered by events.
    - ```Job```: A set of steps that run in the same runner, Jobs can run sequentially or paralle.
    - ```Step```: A single task within a job, such as runnig a script or installing dependencies.
    - ```Runner```: The environment / machines where the jobs are executed.
    - ```Event```: Triggers that start the workflows, such as a push to a repository or creation of a pull request.

3) How do you trigger a Github Actions Workflow ?
- Workflows are triggered by events and evnents is defined by key **ON**.

4) What is Self-Hosted Runner in Github Actions ?
- A Self-Hosted Runner is a custom machine that you manage to run jobs from github actions.
- Unlike Github-Hosted runners, which are provided by GitHub, while Self-Hosted Runners give you more control over the environment, including the software and hardware specifications.

5) How do you define environment variables in a workflows ?
- You can define env vars at the workflow level, job level and step level using key **env**.
```yaml
env:
  NODE_ENV: production

jobs:
  build:
    runs-on: ubuntu-latest
      steps:
        - name: Print Node Env
          run: echo $NODE_ENV
```

6) Explain the use of matrix build in Github Actions.
- Matrix builds allow you to run jobs in parallel across multiple configurations, such as diff versions of a language or diff OS.
```yaml
jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        node-version: [10, 12, 14]
    steps:
      - uses: actions/checkout@v3
      - name: Setup NodeJs
        uses: action/setup-noe@v4
        with:
          node-version: ${{ matrix.node-version }}
      - run: npm install
      - run: npm test
# This will install nodejs version 10, 12, 13 parallely.
```

7) How can you cache dependencies in Github Actions ?
- By using actions **actions/cache@v3**.
```yml
steps:
  - uses: actions/cache@v3
    with:
      path: ~/.npm
      key: ${{ runner.os }}-node-${{ hashFiles('package-lock.json') }}
      restore-keys: |
        ${{ runner.os }}-node
  - run: npm install

  # This caches the '~/.npm' folder based on the 'package-lock.json' hash, which will improve build times.
  ```

  8) What are secrets in Github Actions and how do you use them ?
  - Secrets are encrypted env vars stored in Github that you can use in your workflows for sensitive data like API Keys or Passwords. They are referenced in workflow like this:
  ```yml
  steps:
    - name: Checkout repo
      uses: actions/checkout@v3
    - name: Deploy to productions
      run: deploy-script.sh
      env:
        API_KEY: ${{ secrets.API_KEY }}
```

9) How ccan you reuse workflows in Github Actions ?
- By Calling workflow 1 into workflow 2 by using ```workflow_call``` events. and give workflow 1 path in key ```uses```.
```yml
jobs:
  call-workflow:
    uses: ./.github/workflow/deploy.yml
```

10) How do you handle conditional steps in a Github Actions workflow ?
- By using **if*8 key to run steps conditionally.
```yml
steps:
  - name: Run tests on main branch
    if: github.ref == 'refs/heads/main'
    run: npm test
# This wofkflow of npm test will triggers only while its running in **main branch**.
```
11) What is purpose of key ```runs-on``` ?
- To define the runners / machine where our code will be executed.

12) How can you pass data between jobs in a workflows ?
- By using actions of ```upload artifacts``` and ```download artifacts```.
```yml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - run: echo "Build-output.txt" > output.txt
      - uses: actions/upload-artifact@v3
        with:
          name: build-output # Give your artifacts name
          path: output.txt  # Where will be save artifacts.
  test:
    runs-on: ubuntu-latest
    needs: build
    steps:
      - uses: actions/download-artifact@v3
        with:
          name: build-output
      - run: cat output.txt
```

13)  How can you use Github Actions to create a custom Docker Image ?
- you can use Github Actions to build and push a docker image to docker reg.
```yml
name: Build and push Docker image
on:
  push:
    branches:
      - main
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Build Docker Image
        run: docker build -t my-app .
      - name: Log in to Docker Hub
        run: echo "${{ secrets.DOCKER_PASSWORD }}" | docker login -u "${{ secrets.DOCKER_USERNAME }}" --password-stdin
      - name: push docker image
        run: docker push my-app
```

14) How do you create **reusable** actions in Github Actions ?
- To create reusabel actions you have to create **action.yml** file.

15) Explain the concept of ```contexts``` in Github Actions.
- Contexts are set of predefined variables that Github Actions providew, which are use in your workflows.

    - `github`: Informations abot the wokflow run and repository.
    - `secrets`: Access to your repository's secrets.
```yml
steps:
  - run: echo "This job is running on ${ runner.os } in the ${ github.ref } branch"
```

