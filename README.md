# azure-pipeline.yml
# Define the pipeline trigger
trigger:
  branches:
    include:
      - main  # Trigger the pipeline on commits to the 'main' branch

# Define the pool of virtual machines to use
pool:
  vmImage: 'ubuntu-latest'

# Define the steps for the pipeline
steps:

# Step 1: Checkout the source code from the repository
- checkout: self

# Step 2: Set up Node.js environment
- task: UseNode@1
  inputs:
    versionSpec: '16.x'  # Specify the Node.js version you need
    addToPath: true

# Step 3: Install dependencies
- script: |
    npm install
  displayName: 'Install dependencies'

# Step 4: Run tests
- script: |
    npm test
  displayName: 'Run tests'

# Optional: Publish build artifacts (e.g., compiled code or binaries)
- task: PublishBuildArtifacts@1
  inputs:
    pathtoPublish: '$(Build.SourcesDirectory)'  # Path to the directory containing your build artifacts
    artifactName: 'drop'
    publishLocation: 'Container'
