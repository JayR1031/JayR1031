![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=flat&logo=javascript&logoColor=black) ![Bash](https://img.shields.io/badge/Bash-4EAA25?style=flat&logo=gnubash&logoColor=white) ![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=flat&logo=typescript&logoColor=white) ![Git](https://img.shields.io/badge/Git-F05032?style=flat&logo=git&logoColor=white) ![Vim](https://img.shields.io/badge/Vim-019733?style=flat&logo=vim&logoColor=white) ![IntelliJ IDEA](https://img.shields.io/badge/IntelliJ_IDEA-000000?style=flat&logo=intellijidea&logoColor=white) ![PyCharm](https://img.shields.io/badge/PyCharm-000000?style=flat&logo=pycharm&logoColor=white) ![VS Code](https://img.shields.io/badge/VS_Code-007ACC?style=flat&logo=visual-studio-code&logoColor=white) ![Xcode](https://img.shields.io/badge/Xcode-147EFB?style=flat&logo=xcode&logoColor=white) ![AWS](https://img.shields.io/badge/AWS-232F3E?style=flat&logo=amazon-aws&logoColor=white) ![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat&logo=docker&logoColor=white) ![GitHub Actions](https://img.shields.io/badge/GitHub_Actions-2088FF?style=flat&logo=github-actions&logoColor=white) ![Flask](https://img.shields.io/badge/Flask-000000?style=flat&logo=flask&logoColor=white) ![Bloc](https://img.shields.io/badge/Bloc-6A1B9A?style=flat&logo=flutter&logoColor=white) ![REST API](https://img.shields.io/badge/REST_API-000000?style=flat&logo=json&logoColor=white) ![Postman](https://img.shields.io/badge/Postman-FF6C37?style=flat&logo=postman&logoColor=white) ![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=flat&logo=mysql&logoColor=white) ![Firebase](https://img.shields.io/badge/Firebase-FFCA28?style=flat&logo=firebase&logoColor=white) ![SQLite](https://img.shields.io/badge/SQLite-003B57?style=flat&logo=sqlite&logoColor=white) ![Redis](https://img.shields.io/badge/Redis-DC382D?style=flat&logo=redis&logoColor=white) ![Linux](https://img.shields.io/badge/Linux-FCC624?style=flat&logo=linux&logoColor=black) ![macOS](https://img.shields.io/badge/macOS-000000?style=flat&logo=apple&logoColor=white) ![Windows](https://img.shields.io/badge/Windows-0078D6?style=flat&logo=windows&logoColor=white) ![Figma](https://img.shields.io/badge/Figma-F24E1E?style=flat&logo=figma&logoColor=white) ![Material UI](https://img.shields.io/badge/Material_UI-0081CB?style=flat&logo=mui&logoColor=white) ![TailwindCSS](https://img.shields.io/badge/Tailwind_CSS-06B6D4?style=flat&logo=tailwind-css&logoColor=white) ![Canva](https://img.shields.io/badge/Canva-00C4CC?style=flat&logo=canva&logoColor=white) ![TensorFlow](https://img.shields.io/badge/TensorFlow-FF6F00?style=flat&logo=tensorflow&logoColor=white) ![PyTorch](https://img.shields.io/badge/PyTorch-EE4C2C?style=flat&logo=pytorch&logoColor=white) ![NumPy](https://img.shields.io/badge/NumPy-013243?style=flat&logo=numpy&logoColor=white) ![pandas](https://img.shields.io/badge/pandas-150458?style=flat&logo=pandas&logoColor=white) ![matplotlib](https://img.shields.io/badge/Matplotlib-11557c?style=flat&logo=plotly&logoColor=white) ![seaborn](https://img.shields.io/badge/Seaborn-9E9E9E?style=flat) ![Jira](https://img.shields.io/badge/Jira-0052CC?style=flat&logo=jira&logoColor=white) ![Notion](https://img.shields.io/badge/Notion-000000?style=flat&logo=notion&logoColor=white) ![Slack](https://img.shields.io/badge/Slack-4A154B?style=flat&logo=slack&logoColor=white) ![Discord](https://img.shields.io/badge/Discord-5865F2?style=flat&logo=discord&logoColor=white)

## 🌐 Languages:
![English](https://img.shields.io/badge/🇺🇸%20English-Native-blue?style=flat&logo=googletranslate&logoColor=white) ![Spanish](https://img.shields.io/badge/🇪🇸%20Spanish-Native-red?style=flat&logo=googletranslate&logoColor=white)

## 📊 GitHub Activity:
![GitHub Activity Map](https://github-profile-summary-cards.vercel.app/api/cards/profile-details?username=JayR1031&theme=github_dark)

## 🐍 Contribution Snake:
![GitHub Snake](https://raw.githubusercontent.com/JayR1031/JayR1031/output/github-contribution-grid-snake.svg)

### 🔧 How to Set Up the GitHub Snake:

The snake animation is auto-generated using a GitHub Action. Follow these steps to set it up:

#### Step 1: Create the GitHub Action Workflow

1. In your repository, create the directory `.github/workflows/` if it doesn't exist
2. Create a new file: `.github/workflows/snake.yml`
3. Add the following content:

```yaml
name: Generate Snake

on:
  schedule:
    - cron: "0 0 * * *" # Runs daily at midnight UTC
  workflow_dispatch: # Allows manual trigger
  push:
    branches:
      - main

jobs:
  generate:
    runs-on: ubuntu-latest
    timeout-minutes: 10
    
    steps:
      - name: Generate github-contribution-grid-snake.svg
        uses: Platane/snk/svg-only@v3
        with:
          github_user_name: ${{ github.repository_owner }}
          outputs: |
            dist/github-contribution-grid-snake.svg
            dist/github-contribution-grid-snake-dark.svg?palette=github-dark
            
      - name: Push github-contribution-grid-snake.svg to output branch
        uses: crazy-max/ghaction-github-pages@v3.1.0
        with:
          target_branch: output
          build_dir: dist
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

#### Step 2: Enable GitHub Actions

1. Go to your repository **Settings**
2. Navigate to **Actions** → **General**
3. Under **Workflow permissions**, select **Read and write permissions**
4. Check **Allow GitHub Actions to create and approve pull requests**
5. Click **Save**

#### Step 3: Run the Action

1. Go to the **Actions** tab in your repository
2. Click on **Generate Snake** workflow
3. Click **Run workflow** → **Run workflow**
4. Wait for the action to complete (creates an `output` branch with the SVG)

#### Step 4: Update Your README

The snake image will be available at:
```markdown
![GitHub Snake](https://raw.githubusercontent.com/YOUR_USERNAME/YOUR_USERNAME/output/github-contribution-grid-snake.svg)
```

**Note:** Replace `YOUR_USERNAME` with your actual GitHub username. The snake will auto-update daily based on your contributions!

## 🔍 [Search My Projects](https://github.com/search?q=user:JayR1031+)
