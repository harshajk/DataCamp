# Github actions

## Github actions workflow

Notice that the job name is declared as a YAML key, unlike steps and workflow names, which have an explicit name key for specifying names as its values.

### Write a GitHub Actions Workflow

```yaml
name: CI

on:
  push:
    # Specify the branch that will trigger the workflow
    branches: [ "myfeature" ]

jobs:
  # Write name of the job as a key
  hello-from-runner:
    # Specify the runner machine
    runs-on: ubuntu-22.04
    steps:
      # Write the step name
      - name: Print Name
        run: |
          echo "Hello from $(whoami)"
```

### Running Python code in GitHub Actions

```yaml
name: CI

on:
  # Specify the event that will trigger the workflow
  pull_request:
    branches: [ "main" ]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      # Use checkout action to check out the repository on the runner
      - name: Checkout
        uses: actions/checkout@v3

      # Setup Python to 3.9 using setup-python action
      - name: Setup Python
        uses: actions/setup-python@v4
        with:
          python-version: 3.9

      # Run the hello_world.py python script
      - name: Run Python script
        run: |
          python3 hello_world.py

```

### Working with environment variables

```yaml
name: Testing Environment variables

on:
  pull_request:
    # Write the target branch
    branches: ["main"]

env:
  # Set the value of global environment variable
  GLOBAL_VARIABLE: global_value

jobs:
  
  # Job block
  print_env_and_secrets:
    runs-on: ubuntu-latest
    
    env:
      # Set the value of local environment variable
      JOB_VARIABLE: job_value

    steps:
      - name: Print Variables
        # Write the environment variable whose value is set
        run: |
          echo "Global Variable: ${{ env.GLOBAL_VARIABLE }}"
          echo "Set job Variable: ${{ env.JOB_VARIABLE }}"
```

### Working with secrets

```yaml
name: Testing Secrets

on:
  # Write the event triggering the workflow
  pull_request:
    branches: ["master"]

jobs:
  print_secrets:
    runs-on: ubuntu-latest
    
    permissions:
      # Grant the write permissions
      pull-requests: write

    steps:
      - name: Comment on Pull Request
        uses: thollander/actions-comment-pull-request@v2
        with:
          # Access the GITHUB_TOKEN token from secrets
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          message: |
            Hello world ! :wave:
```