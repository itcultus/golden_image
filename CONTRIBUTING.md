# Contributing to itcultus.golden_images

Thank you for considering contributing to the `mycompany.golden_images` Ansible 
Collection! We appreciate your interest and efforts to make this collection 
better.

These guidelines will help you through the process.

## How to Contribute

We welcome contributions in various forms, including bug reports, feature 
requests, and code contributions.

### 1. Reporting Bugs

* Before submitting a new bug report, please check the existing 
[issues](https://github.com/itcultus/golden_images/issues) to see if the bug has already been reported.
* If not, open a new issue and provide:
    * A clear and concise description of the bug.
    * Steps to reproduce the behavior.
    * Expected behavior.
    * Actual behavior.
    * Your Ansible version, collection version, and any relevant environment details.
    * Any error messages or logs.

### 2. Suggesting Enhancements (Feature Requests)

* Before submitting a new feature request, please check the existing
[issues](https://github.com/itcultus/golden_images/issues) to see if the feature 
has already been requested or is in progress.
* If not, open a new issue and provide:
    * A clear and concise description of the suggested feature.
    * Why this feature would be useful.
    * Any alternative solutions you've considered.

### 3. Submitting Code (Pull Requests)

We use GitHub Flow for contributions.

1.  **Fork the repository:** Start by forking the `golden_images` repository to 
your GitHub account.
2.  **Clone your fork:**  
```bash
    git clone https://github.com/itcultus/golden_images.git
    cd golden_images
```
3.  **Create a new branch:** For each new feature or bug fix, create a new, 
descriptively named branch.  
```bash
    git checkout -b feature/my-awesome-feature
    # or
    git checkout -b bugfix/fix-for-issue-123
```
4.  **Make your changes:** Implement your feature or fix the bug.
5.  **Run Linting/Formatting:** Ensure your code conforms to Ansible's style 
guidelines and follows Red Hat's Automation Good Practices (GPA). 
These guidelines are crucial for maintaining high-quality, consistent, and 
maintainable automation.
    * **Refer to Red Hat GPA:** For detailed information about this coding style, 
    please consult the [Red Hat Automation Good 
    Practices](https://redhat-cop.github.io/automation-good-practices/).
    * **Run `ansible-lint` (Mandatory Production Profile):** All contributions
    **must** pass `ansible-lint` with the `production` profile, which ensures 
    content meets stringent requirements for enterprise-grade automation.  
    ```bash
        ansible-lint -p production
    ```

6.  **Commit your changes:** Write clear and concise commit messages.  
    ```bash
    git add .
    git commit -m "feat: Add new awesome feature"
    ```
7.  **Push your branch to your fork:**
    ```bash
    git push origin feature/my-awesome-feature
    ```
8.  **Open a Pull Request (PR):**
    * Go to your fork on GitHub and open a new Pull Request.
    * Target the `main` branch of the original `itcultus/golden_images` repository.
    * Provide a detailed description of your changes in the PR description.
    * Reference any related issues (e.g., `Closes #123`).

## Questions?

If you have any questions about contributing, please open an issue.
