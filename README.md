# RunTree

RunTree is an open-source, interactive troubleshooting engine. It converts flat YAML diagnostic flows into web-based decision trees, replacing static wiki runbooks for IT and Service Desk teams.

Built with **Jekyll**, **Alpine.js**, and **Tailwind CSS**, RunTree is designed to be lightweight, easy to author, and simple to deploy.

## Features
- **YAML-Driven:** Author your decision trees using simple, human-readable YAML files.
- **Interactive State Machine:** Navigate diagnostic flows instantly without page reloads using Alpine.js.
- **Deep Linking:** Every step is uniquely targetable via URL parameters, allowing you to share exact states with team members.
- **Dark Mode:** Native dark mode support using Tailwind CSS.
- **Static & Secure:** Compiles to static HTML/JS/CSS. No databases or backend servers required.

## Local Development (Docker)
RunTree uses Docker Compose to provide a consistent development environment.

1. Clone the repository.
2. Start the development server:
   ```bash
   sudo docker compose up -d
   ```
3. Open your browser and navigate to `http://localhost:4000`.

*Note: Changes to YAML files, Markdown files, and HTML layouts will live-reload automatically.*

## Authoring Flows
Troubleshooting flows are stored in the `_data/flows/` directory. Create a new YAML file to define a flow.
For detailed instructions on the schema and configuration options, see the [Documentation](/docs) page in the running app.

## Deployment
RunTree is pre-configured to deploy automatically to GitHub Pages.
Simply push your changes to the `main` branch, and the included GitHub Action will build and deploy your site.

## License
This project is licensed under the [GNU General Public License v3.0 (GPLv3)](LICENSE).
You may use this software for commercial and personal purposes, provided that you disclose the source code of any modified versions. Copyright &copy; 2026 Kyle Spillane.
