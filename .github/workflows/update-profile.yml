name: Update My Professional Profile

on:
  workflow_dispatch:
  schedule:
    - cron: "23 */6 * * *"
  push:
    paths:
      - .github/workflows/update-profile.yml

permissions:
  contents: write

concurrency:
  group: professional-profile
  cancel-in-progress: false

jobs:
  update-profile:
    runs-on: ubuntu-latest

    steps:
      - name: Generate and update README
        uses: actions/github-script@v7
        with:
          script: |
            const username = "as4688541-pixel";
            const leetcode = "Abhisheksingh_321";

            const { owner, repo } = context.repo;
            const branch = context.payload.repository.default_branch;

            // Fetch your public GitHub repositories.
            const repositories = await github.paginate(
              github.rest.repos.listForUser,
              {
                username,
                type: "owner",
                sort: "pushed",
                direction: "desc",
                per_page: 100
              }
            );

            const publicRepos = repositories.filter(r => !r.private);

            // Include original, active projects.
            const projects = publicRepos
              .filter(r =>
                !r.fork &&
                !r.archived &&
                r.name.toLowerCase() !== username.toLowerCase()
              )
              .sort((a, b) =>
                new Date(b.pushed_at || b.created_at) -
                new Date(a.pushed_at || a.created_at)
              );

            const { data: profile } =
              await github.rest.users.getByUsername({ username });

            // Escape repository text for Markdown tables.
            const escape = value => String(value ?? "")
              .replace(/&/g, "&amp;")
              .replace(/</g, "&lt;")
              .replace(/>/g, "&gt;")
              .replace(/\|/g, "&#124;")
              .replace(/\[/g, "&#91;")
              .replace(/\]/g, "&#93;")
              .replace(/[\r\n]+/g, " ");

            const originalRepos = publicRepos.filter(r => !r.fork);

            const stars = originalRepos.reduce(
              (sum, r) => sum + r.stargazers_count,
              0
            );

            // Count main languages across original public repositories.
            const languages = {};

            for (const repository of originalRepos) {
              if (repository.language) {
                languages[repository.language] =
                  (languages[repository.language] || 0) + 1;
              }
            }

            const languageRows = Object.entries(languages)
              .sort((a, b) => b[1] - a[1])
              .map(([language, count]) =>
                `| ${escape(language)} | ${count} |`
              );

            const languageTable = languageRows.length
              ? [
                  "| Main language | Repositories |",
                  "| --- | ---: |",
                  ...languageRows
                ].join("\n")
              : "Language information will appear as projects are added.";

            const projectRows = projects.map(project => {
              const date = project.pushed_at
                ? project.pushed_at.slice(0, 10)
                : "—";

              return [
                `| [${escape(project.name)}](${project.html_url})`,
                escape(
                  project.description ||
                  "Explore the repository for details."
                ),
                escape(project.language || "—"),
                project.stargazers_count,
                `${date} |`
              ].join(" | ");
            });

            const projectTable = projectRows.length
              ? [
                  "| Project | Description | Main language | Stars | Last push (UTC) |",
                  "| --- | --- | --- | ---: | --- |",
                  ...projectRows
                ].join("\n")
              : "My public projects will appear here automatically.";

            const readme = [
              '<div align="center">',
              "",
              "# 👋 Hi, I'm Abhishek Singh",
              "",
              "### Aspiring Software Developer | Java & DSA | AI/ML Enthusiast",
              "",
              "Building practical software and improving through consistent problem solving.",
              "",
              `<a href="https://github.com/${username}">`,
              '<img src="https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white" alt="GitHub" />',
              "</a>",
              "",
              `<a href="https://leetcode.com/u/${leetcode}/">`,
              '<img src="https://img.shields.io/badge/LeetCode-FFA116?style=for-the-badge&logo=leetcode&logoColor=white" alt="LeetCode" />',
              "</a>",
              "",
              "</div>",
              "",
              "---",
              "",
              "## 🚀 About Me",
              "",
              "- 🎓 B.Tech student in Computer Science and Engineering (AI & ML).",
              "- ☕ Solving Data Structures and Algorithms problems using Java.",
              "- 🧠 Building projects using Python, machine learning, and generative AI.",
              "- 💻 Interested in software development and practical problem solving.",
              "- 📚 Continuously strengthening programming and database fundamentals.",
              "",
              "## 🛠️ Professional Skills",
              "",
              "| Area | Skills |",
              "| --- | --- |",
              "| Programming | Java, Python, SQL |",
              "| Fundamentals | DSA, OOP, DBMS |",
              "| AI & Data | Machine Learning, Scikit-learn, Pandas |",
              "| Application Development | FastAPI, Streamlit |",
              "| Databases | MySQL, MongoDB |",
              "| Tools | Git, GitHub, VS Code |",
              "",
              '<p align="center">',
              '<img src="https://skillicons.dev/icons?i=java,python,mysql,mongodb,fastapi,git,github,vscode" alt="Technology stack" />',
              "</p>",
              "",
              "---",
              "",
              "## 🧩 LeetCode Progress",
              "",
              '<div align="center">',
              "",
              `<a href="https://leetcode.com/u/${leetcode}/">`,
              `<img src="https://leetcard.jacoblin.cool/${leetcode}?theme=dark&font=Baloo&ext=heatmap" alt="LeetCode statistics and activity" />`,
              "</a>",
              "",
              "</div>",
              "",
              `[View my LeetCode profile](https://leetcode.com/u/${leetcode}/)`,
              "",
              "---",
              "",
              "## 📈 GitHub Progress",
              "",
              "| Metric | Current value |",
              "| --- | ---: |",
              `| Public repositories | ${profile.public_repos} |`,
              `| Active original projects listed below | ${projects.length} |`,
              `| Stars on original public repositories | ${stars} |`,
              `| Followers | ${profile.followers} |`,
              `| Following | ${profile.following} |`,
              "",
              "## 🔥 GitHub Contribution Streak",
              "",
              '<div align="center">',
              "",
              `<a href="https://github.com/${username}">`,
              `<img src="https://streak-stats.demolab.com/?user=${username}&theme=tokyonight&hide_border=true" alt="GitHub contribution streak" />`,
              "</a>",
              "",
              "</div>",
              "",
              "## 💻 Languages Across My Repositories",
              "",
              "Main language of each original public repository; this is not a skill rating.",
              "",
              languageTable,
              "",
              "---",
              "",
              "## 📂 Projects & Repositories",
              "",
              "Automatically refreshed from my public, original, non-archived repositories, ordered by recent code pushes.",
              "",
              projectTable,
              "",
              `[View all repositories](https://github.com/${username}?tab=repositories)`,
              "",
              "---",
              "",
              "## 🎯 Learning Focus",
              "",
              "- Strengthen Java and computer science fundamentals.",
              "- Improve DSA problem-solving skills.",
              "- Build useful software and AI applications.",
              "- Practice clean, readable, maintainable code.",
              "",
              "## 📫 Connect With Me",
              "",
              `- [GitHub](https://github.com/${username})`,
              `- [LeetCode](https://leetcode.com/u/${leetcode}/)`,
              "",
              "---",
              "",
              "> Consistent practice. Practical projects. Continuous improvement.",
              "",
              "<!-- Generated by update-profile.yml. Edit the workflow to change profile text. -->",
              ""
            ].join("\n");

            // Read the existing README, if available.
            let existing;

            try {
              const response = await github.rest.repos.getContent({
                owner,
                repo,
                path: "README.md",
                ref: branch
              });

              existing = response.data;
            } catch (error) {
              if (error.status !== 404) throw error;
            }

            if (existing) {
              const current = Buffer.from(
                existing.content,
                "base64"
              ).toString("utf8");

              if (current === readme) {
                core.info("Profile is already up to date.");
                return;
              }
            }

            // Create or update the generated profile README.
            await github.rest.repos.createOrUpdateFileContents({
              owner,
              repo,
              branch,
              path: "README.md",
              message: "Refresh professional profile",
              content: Buffer.from(readme).toString("base64"),
              ...(existing ? { sha: existing.sha } : {})
            });
