# Publish NuGet Action

This action publishes a NuGet package from a project or solution to a feed.

## Inputs

This action has some inputs.

1.  nuget-feed-url (not required)

    - This is the NuGet Feed to publish to.
    - This parameter defaults to 'https://nuget.pkg.github.com/${{ github.repository_owner }}.index.json'.

2.  nuget-feed-name (not required)

    - This is the name of the NuGet Feed to use for restoration and publishing.
    - This parameter defaults to 'github'.

3.  config (not required)

    - This parameter specifies the build configuration to use.
    - This parameter defaults to 'Release'

4.  version (required)

    - This parameter specifies the version of the NuGet package.

5.  project-path (required)

    - This parameter specifies the path to the .csproj to package.
    - This parameter is used in the dotnet pack step.

6.  solution-path (required)

    - This parameter specifies the path to the .sln file to restore, build, and test.
    - This parameter is used in the dotnet restore, build, and test steps.

7.  nuget-feed-password (required)

    - This parameter specifies the password for NuGet feed authentication & api key for pushing

8.  nuget-feed-user (not required)

    - This parameter specifies the username for NuGet feed authentication.
    - This parameter defaults to ${{ github.repository_owner }}.

9.  output-path (not required)
    - This parameter specifies the path to use for output building.
    - This parameter defaults to ${{ github.workspace }}/out

## Outputs

This action has no outputs.

## Notes

It is necessary to have two steps defined in your workflow before using this workflow.

These steps are below:

1.  Checkout code

```
- name: Checkout Code
  uses: actions/checkout@v2
```

2.  Setup .NET

```

- name: Setup .NET
  uses: actions/setup-dotnet@v1
  with:
    dotnet-version: "5.0"
```

## Example end-to-end workflow

```
# publish-nuget.yml
name: Publish NuGet Package on Push to Main

on:
  push:
    branches:
      - main

jobs:
  Package:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Code
        uses: actions/checkout@v2

      - name: Setup .NET
        uses: actions/setup-dotnet@v1
        with:
          dotnet-version: "5.0"

      - name: Build, Test, and Publish NuGet
        uses: wahinekai/actions-publish-nuget@v3.0.1
        with:
          version: 1.0.0
          project-path: Test/Test.csproj
          solution-path: Test.sln
          nuget-feed-password: ${{ secrets.PACKAGES_TOKEN }}
```
