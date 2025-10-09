[Skip to content](https://github.blog/changelog/2025-10-03-github-copilot-cli-enhanced-model-selection-image-support-and-streamlined-ui/#start-of-content)

[](https://github.com/)/[Blog](https://github.blog/)

[Back to changelog](https://github.blog/changelog/)

Improvement

October 3, 2025 • 3 minute read

# GitHub Copilot CLI: Enhanced model selection, image support, and streamlined UI

![](https://github.blog/wp-content/themes/github-2021-child/assets/img/featured-v3-improvements.svg)

## Table of Contents

Menu. Currently selected:Anthropic Claude Sonnet 4.5 support

We’ve had an exciting week since our initial [public preview release last week](https://github.blog/changelog/2025-09-25-github-copilot-cli-is-now-in-public-preview/). We’ve shipped [numerous improvements](https://github.com/github/copilot-cli/releases) to GitHub Copilot CLI which wouldn’t have been possible without the great engagement and feedback from our preview users. Thank you, and keep it coming!

Here’s what’s new this week.

## [Anthropic Claude Sonnet 4.5 support](https://github.blog/changelog/2025-10-03-github-copilot-cli-enhanced-model-selection-image-support-and-streamlined-ui/#anthropic-claude-sonnet-4-5-support)

Claude Sonnet 4.5, Anthropic’s most advanced model for coding and real-world agents, is now available in public preview in GitHub Copilot CLI. We’re [gradually rolling it out](https://github.blog/changelog/2025-09-29-anthropic-claude-sonnet-4-5-is-in-public-preview-for-github-copilot/) to Copilot Pro, Pro+, Business, and Enterprise users.

## [Choose your model with ease](https://github.blog/changelog/2025-10-03-github-copilot-cli-enhanced-model-selection-image-support-and-streamlined-ui/#choose-your-model-with-ease)

You now have direct control over which AI model powers your CLI sessions. Use the new `/model` slash command to quickly switch between available models. Your selected model is displayed above the input box, so you always know which model you’re working with.

![GIF of a terminal where the user runs "/model" in the Copilot CLI](https://github.com/user-attachments/assets/2e3e84a2-f56b-4852-bbb6-a5e672e2404c)

## [Image recognition](https://github.blog/changelog/2025-10-03-github-copilot-cli-enhanced-model-selection-image-support-and-streamlined-ui/#image-recognition)

Using `@` and mentioning images makes them available to the model as input.

## [Directly execute shell commands](https://github.blog/changelog/2025-10-03-github-copilot-cli-enhanced-model-selection-image-support-and-streamlined-ui/#directly-execute-shell-commands)

Prepend your input with `!` to directly execute it in your shell, without making a call to the model.

![GIF of a terminal where "!git clone https://github.com/github/copilot-cli" has been typed. Instead of making a call to the model, the Copilot CLI executes the command directly in the shell](https://github.com/user-attachments/assets/ee65ca6c-26cf-4fe9-bc41-ac5896ff9334)

## [Refined interface and navigation](https://github.blog/changelog/2025-10-03-github-copilot-cli-enhanced-model-selection-image-support-and-streamlined-ui/#refined-interface-and-navigation)

We’ve polished the CLI experience with thoughtful improvements:

- **Smarter input handling**: The input box now scrolls and limits to 10 lines, keeping your screen clean even with longer prompts. We’ve also improved word motion and multiline navigation so arrow keys behave exactly as you’d expect. These are stepping stones on the way to supporting [multiline input](https://github.com/github/copilot-cli/issues/14), a heavily requested feature.
- **Better visibility**: Timeline events for file operations are now more dense with information.
- **Scrolling list navigation**: A new scrollbar appears in pickers to help you navigate large lists.
- **Session management improvements**: The `--resume` session picker now displays relative timestamps and message counts, making it simple to jump back into recent conversations. We’ve also shipped a `--continue` flag to continue from your last session.

![A terminal showing the output of the "copilot --resume" command, featuring a list of the user's past conversations. There is a scroll bar to help the user navigate the list.](https://github.com/user-attachments/assets/24f262ef-0525-409e-a5c4-13b71c405cd3)

- **Streamlined layout**: We removed unnecessary borders and improved spacing throughout the interface, making it easier to copy text and stay focused on your session.

![A screenshot of the Copilot user interface.](https://github.com/user-attachments/assets/c3699566-9377-4dc5-a5d1-baf3c12199a8)

- **More legible Markdown**: We’ve excluded `#` prefixes from Markdown rendering for a more readable experience.

## [Fine-grained permission control](https://github.blog/changelog/2025-10-03-github-copilot-cli-enhanced-model-selection-image-support-and-streamlined-ui/#fine-grained-permission-control)

Take precise control over what Copilot CLI can execute with enhanced tool permissions. You can now use glob patterns in `--allow-tool` and `--deny-tool` flags to match command patterns. For example, `shell(npm run test:*)` allows any test script while keeping tighter restrictions on other operations. This gives you the flexibility to integrate Copilot CLI securely into your existing workflows.

We’ve improved shell command path extraction to ensure you’re always prompted for permission when Copilot CLI wishes to access a new directory. In addition, we’ve enhanced our parsing of PowerShell commands.

## [Context and usage statistics](https://github.blog/changelog/2025-10-03-github-copilot-cli-enhanced-model-selection-image-support-and-streamlined-ui/#context-and-usage-statistics)

How many premium requests have you used this session? Are you getting close to filling the model’s context? We’ve made improvements to help you answer these questions and give you insight into your usage.

- **`/usage` command**: The `/usage` slash command informs you of how many premium requests you’ve used this session, how long your session has been, how many lines of code you’ve edited, and gives a break down of token usage for each model. The same information will be printed at the conclusion of your session.
- **Truncation notification**: Copilot CLI truncates the model’s context when its token limit is reached. When you’re getting close to truncation (≤ 20% context remaining), a warning will display above the input box.

## [Enterprise and authentication improvements](https://github.blog/changelog/2025-10-03-github-copilot-cli-enhanced-model-selection-image-support-and-streamlined-ui/#enterprise-and-authentication-improvements)

We’ve strengthened support for enterprise environments:

- Copilot CLI now uses per-subscription API endpoints in accordance with [GitHub’s network access management guidelines](https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-access/manage-network-access).
- Clearer error messages when Copilot CLI is blocked by organization policy or when using a PAT missing the `Copilot Requests` permission.
- Fixed bugs in `/user` commands to correctly show users across all authentication modes.

## [Share your feedback](https://github.blog/changelog/2025-10-03-github-copilot-cli-enhanced-model-selection-image-support-and-streamlined-ui/#share-your-feedback)

Update GitHub Copilot CLI by running `npm install -g @github/copilot@latest` in your terminal. Thank you to everyone who has submitted feedback via the `/feedback` command and by opening issues in [our public repository](https://github.com/github/copilot-cli). Your continued feedback is invaluable as we continue to ship improvements daily.

- [Install GitHub Copilot CLI today](https://github.com/github/copilot-cli?utm_source=changelog-source-cta&utm_campaign=agentic-copilot-cli-launch-2025)
- [Learn more about GitHub Copilot CLI](https://docs.github.com/copilot/concepts/agents/about-copilot-cli)

[client apps](https://github.blog/changelog/2025/?label=client-apps)[copilot](https://github.blog/changelog/2025/?label=copilot)

Share[Back to changelog](https://github.blog/changelog/)

## Related Posts

### Oct.02Release

[Claude Sonnet 4.5 is now available in Visual Studio, JetBrains IDEs, Xcode, and Eclipse](https://github.blog/changelog/2025-10-02-claude-sonnet-4-5-is-now-available-in-visual-studio-jetbrains-ides-xcode-and-eclipse)

[copilot](https://github.blog/changelog/2025/?label=copilot)

### Oct.01Improvement

[Spark: 🚀 Expanded access, enhanced reliability, and faster iteration history](https://github.blog/changelog/2025-10-01-spark-%f0%9f%9a%80-expanded-access-enhanced-reliability-and-faster-iteration-history)

[copilot](https://github.blog/changelog/2025/?label=copilot)

### Oct.01Release

[Auto model selection is now in VS Code for Copilot Business and Enterprise](https://github.blog/changelog/2025-09-30-auto-model-selection-is-now-in-vs-code-for-copilot-business-and-enterprise)

[copilot](https://github.blog/changelog/2025/?label=copilot)

### Sep.30Improvement

[Start your new repository with Copilot coding agent](https://github.blog/changelog/2025-09-30-start-your-new-repository-with-copilot-coding-agent)

[collaboration tools](https://github.blog/changelog/2025/?label=collaboration-tools) ...+1

### Sep.30Release

[Anthropic Claude Sonnet 4.5 is in public preview for Copilot coding agent](https://github.blog/changelog/2025-09-30-anthropic-claude-sonnet-4-5-is-in-public-preview-for-copilot-coding-agent)

[copilot](https://github.blog/changelog/2025/?label=copilot)

### Sep.30Release

[Premium requests analytics page is now generally available](https://github.blog/changelog/2025-09-30-premium-requests-analytics-page-is-now-generally-available)

[copilot](https://github.blog/changelog/2025/?label=copilot)

### Sep.30Release

[GitHub Copilot in Visual Studio — September update](https://github.blog/changelog/2025-09-30-github-copilot-in-visual-studio-september-update)

[copilot](https://github.blog/changelog/2025/?label=copilot)

### Sep.30Improvement

[Copilot coding agent remembers context within the same pull request](https://github.blog/changelog/2025-09-30-copilot-coding-agent-remembers-context-within-the-same-pull-request)

[copilot](https://github.blog/changelog/2025/?label=copilot)

### Sep.30Release

[GitHub Spark in public preview for Copilot Enterprise subscribers](https://github.blog/changelog/2025-09-30-github-spark-in-public-preview-for-copilot-enterprise-subscribers)

[copilot](https://github.blog/changelog/2025/?label=copilot)

## Subscribe to our developer newsletter

Discover tips, technical guides, and best practices in our biweekly newsletter just for devs.

Enter your email*

By submitting, I agree to let GitHub and its affiliates use my information for personalized communications, targeted advertising, and campaign effectiveness. See the [GitHub Privacy Statement](https://github.com/site/privacy) for more details.

[

Back to top

](https://github.blog/changelog/2025-10-03-github-copilot-cli-enhanced-model-selection-image-support-and-streamlined-ui/#start-of-content)

## Site-wide Links

[](https://github.com/)

### Product

- [Features](https://github.com/features)
- [Security](https://github.com/security)
- [Enterprise](https://github.com/enterprise)
- [Customer Stories](https://github.com/customer-stories?type=enterprise)
- [Pricing](https://github.com/pricing)
- [Resources](https://resources.github.com/)

### Platform

- [Developer API](https://developer.github.com/)
- [Partners](https://partner.github.com/)
- [Atom](https://atom.io/)
- [Electron](https://www.electronjs.org/)
- [GitHub Desktop](https://desktop.github.com/)

### Support

- [Docs](https://docs.github.com/)
- [Community Forum](https://github.community/)
- [Training](https://services.github.com/)
- [Status](https://www.githubstatus.com/)
- [Contact](https://support.github.com/)

### Company

- [About](https://github.com/about)
- [Blog](https://github.blog/)
- [Careers](https://github.com/about/careers)
- [Press](https://github.com/about/press)
- [Shop](https://shop.github.com/)

- © 2025 GitHub, Inc.
- [Terms](https://docs.github.com/en/github/site-policy/github-terms-of-service)
- [Privacy](https://docs.github.com/en/github/site-policy/github-privacy-statement)

- [GitHub on LinkedIn](https://www.linkedin.com/company/github)
- [GitHub on Instagram](https://www.instagram.com/github/)
- [GitHub on YouTube](https://www.youtube.com/github)
- [GitHub on X](https://twitter.com/github)
- [GitHub on TikTok](https://www.tiktok.com/@github)
- [GitHub on Twitch](https://www.twitch.tv/github)
- [GitHub’s organization on GitHub](https://github.com/github)