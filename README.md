<div align="center">
  <div>
    <img src=".github/logo-grad.svg" alt="OpenCommit logo"/>
    <h1 align="center">OpenCommit</h1>
    <h4 align="center">Follow the bird <a href="https://twitter.com/_sukharev_"><img src="https://img.shields.io/twitter/follow/_sukharev_?style=flat&label=_sukharev_&logo=twitter&color=0bf&logoColor=fff" align="center"></a>
  </div>
	<h2>Auto-generate meaningful commits in a second</h2>
	<p>Killing lame commits with AI 🤯🔫</p>
	<a href="https://www.npmjs.com/package/opencommit"><img src="https://img.shields.io/npm/v/opencommit" alt="Current version"></a>
  <h4 align="center">🪩 Winner of <a href="https://twitter.com/_sukharev_/status/1683448136973582336">GitHub 2023 hackathon</a> 🪩</h4>
</div>

---

<div align="center">
    <img src=".github/opencommit-example.png" alt="OpenCommit example"/>
</div>

All the commits in this repo are authored by OpenCommit — look at [the commits](https://github.com/di-sukharev/opencommit/commit/eae7618d575ee8d2e9fff5de56da79d40c4bc5fc) to see how OpenCommit works. Emojis and long commit descriptions are configurable.

## Setup OpenCommit as a CLI tool

You can use OpenCommit by simply running it via the CLI like this `oco`. 2 seconds and your staged changes are committed with a meaningful message.

1. Install OpenCommit globally to use in any repository:

   ```sh
   npm install -g opencommit
   ```

   Alternatively run it via `npx opencommit` or `bunx opencommit`

   MacOS may ask to run the command with `sudo` when installing a package globally.

2. Get your API key from [OpenAI](https://platform.openai.com/account/api-keys). Make sure that you add your payment details, so the API works.

3. Set the key to OpenCommit config:

   ```sh
   oco config set OCO_OPENAI_API_KEY=<your_api_key>
   ```

   Your API key is stored locally in the `~/.opencommit` config file.

## Usage

You can call OpenCommit directly to generate a commit message for your staged changes:

```sh
git add <files...>
opencommit
```

You can also use the `oco` shortcut:

```sh
git add <files...>
oco
```

Link to the GitMoji specification: https://gitmoji.dev/

You can also run it with local model through ollama:

- install and start ollama
- run `ollama run mistral` (do this only once, to pull model)
- run (in your project directory):

```sh
git add <files...>
OCO_AI_PROVIDER='ollama' opencommit
```

if you have ollama that is set up in docker/ on another machine with GPUs (not locally), you can change the default endpoint url.
You can do so by setting the `OCO_OLLAMA_API_URL` environment variable as follows:

```sh
OCO_OLLAMA_API_URL='http://192.168.1.10:11434/api/chat' opencommit
```

where 192.168.1.10 is example of endpoint URL, where you have ollama set up.

### Flags

There are multiple optional flags that can be used with the `oco` command:

#### Use Full GitMoji Specification

This flag can only be used if the `OCO_EMOJI` configuration item is set to `true`. This flag allows users to use all emojis in the GitMoji specification, By default, the GitMoji full specification is set to `false`, which only includes 10 emojis (🐛✨📝🚀✅♻️⬆️🔧🌐💡).
This is due to limit the number of tokens sent in each request. However, if you would like to use the full GitMoji specification, you can use the `--fgm` flag.

```sh
oco --fgm
```

#### Skip Commit Confirmation

This flag allows users to automatically commit the changes without having to manually confirm the commit message. This is useful for users who want to streamline the commit process and avoid additional steps. To use this flag, you can run the following command:

```sh
oco --yes
```

## Configuration

OpenCommit offers various configuration options to customize its behavior. You can set these configurations either globally for all repositories or locally for a specific repository.

### Local per repo configuration

Create a `.env` file and add OpenCommit config variables there like this:

```env
OCO_OPENAI_API_KEY=<your OpenAI API token>
OCO_TOKENS_MAX_INPUT=<max model token limit (default: 4096)>
OCO_TOKENS_MAX_OUTPUT=<max response tokens (default: 500)>
OCO_OPENAI_BASE_PATH=<may be used to set proxy path to OpenAI api>
OCO_DESCRIPTION=<postface a message with ~3 sentences description of the changes>
OCO_EMOJI=<boolean, add GitMoji>
OCO_MODEL=<either 'gpt-4', 'gpt-4-turbo', 'gpt-3.5-turbo' (default), 'gpt-3.5-turbo-0125', 'gpt-4-1106-preview', 'gpt-4-turbo-preview' or 'gpt-4-0125-preview'>
OCO_LANGUAGE=<locale, scroll to the bottom to see options>
OCO_MESSAGE_TEMPLATE_PLACEHOLDER=<message template placeholder, default: '$msg'>
OCO_PROMPT_MODULE=<either conventional-commit or @commitlint, default: conventional-commit>
OCO_ONE_LINE_COMMIT=<one line commit message, default: false>
```

### Global Configuration

To set global configurations that apply to all repositories:

```sh
oco config set <CONFIG_KEY>=<VALUE>
```

### Available Configuration Options

For a more detailed explanation of each configuration option:

| Config Key | Description | Default Value | Possible Values |
|------------|-------------|---------------|-----------------|
| `OCO_OPENAI_API_KEY` | Your OpenAI API key | - | Valid OpenAI API key |
| `OCO_ANTHROPIC_API_KEY` | Your Anthropic API key | - | Valid Anthropic API key |
| `OCO_AZURE_API_KEY` | Your Azure OpenAI API key | - | Valid Azure API key |
| `OCO_TOKENS_MAX_INPUT` | Maximum number of input tokens | 4096 | Positive integer |
| `OCO_TOKENS_MAX_OUTPUT` | Maximum number of output tokens | 500 | Positive integer |
| `OCO_OPENAI_BASE_PATH` | Custom base path for OpenAI API | - | Valid URL string |
| `OCO_DESCRIPTION` | Include a detailed description in commit messages | false | `true` or `false` |
| `OCO_EMOJI` | Include emojis in commit messages | false | `true` or `false` |
| `OCO_MODEL` | AI model to use | 'gpt-3.5-turbo' | See [Supported Models](#supported-models) |
| `OCO_LANGUAGE` | Language for commit messages | 'en' | See [Supported Languages](#supported-languages) |
| `OCO_MESSAGE_TEMPLATE_PLACEHOLDER` | Placeholder for custom message templates | '$msg' | String starting with '$' |
| `OCO_PROMPT_MODULE` | Commit message style | 'conventional-commit' | 'conventional-commit' or '@commitlint' |
| `OCO_AI_PROVIDER` | AI service provider | 'openai' | 'openai', 'anthropic', 'azure', 'ollama', or 'test' |
| `OCO_GITPUSH` | Automatically push commits | true | `true` or `false` |
| `OCO_ONE_LINE_COMMIT` | Generate one-line commit messages | false | `true` or `false` |
| `OCO_AZURE_ENDPOINT` | Azure OpenAI endpoint | - | Valid Azure endpoint URL |
| `OCO_OLLAMA_API_URL` | Custom Ollama API URL | - | Valid URL string |

### Supported Models

#### OpenAI Models

- gpt-3.5-turbo
- gpt-3.5-turbo-0125
- gpt-4
- gpt-4-turbo
- gpt-4-1106-preview
- gpt-4-turbo-preview
- gpt-4-0125-preview
- gpt-4o

#### Anthropic Models

- claude-3-haiku-20240307
- claude-3-sonnet-20240229
- claude-3-opus-20240229

### Supported Languages

OpenCommit supports various languages for generating commit messages. The available languages can be found in the [i18n folder](https://github.com/di-sukharev/opencommit/tree/master/src/i18n) of the repository.

To set a language:

```sh
oco config set OCO_LANGUAGE=<language_code>
```

Example:

```sh
oco config set OCO_LANGUAGE=fr  # Set language to French
```

### Using Different AI Providers

OpenCommit supports multiple AI providers. To switch between them:

#### OpenAI (default)

```sh
oco config set OCO_AI_PROVIDER=openai
oco config set OCO_OPENAI_API_KEY=<your_openai_api_key>
```

#### Anthropic

```sh
oco config set OCO_AI_PROVIDER=anthropic
oco config set OCO_ANTHROPIC_API_KEY=<your_anthropic_api_key>
```

#### Azure OpenAI

```sh
oco config set OCO_AI_PROVIDER=azure
oco config set OCO_AZURE_API_KEY=<your_azure_api_key>
oco config set OCO_AZURE_ENDPOINT=<your_azure_endpoint>
```

#### Ollama (for local AI models)

```sh
oco config set OCO_AI_PROVIDER=ollama
# Optionally, set a custom API URL:
oco config set OCO_OLLAMA_API_URL=http://localhost:11434/api/chat
```

### Advanced Configuration

#### Custom Message Templates

You can customize the commit message template using the `OCO_MESSAGE_TEMPLATE_PLACEHOLDER` config:

```sh
oco config set OCO_MESSAGE_TEMPLATE_PLACEHOLDER='$msg'
```

Then use it in your commit command:

```sh
oco '#205: $msg'
```

This will include the issue number in your commit message.

#### Commit Message Style

Choose between conventional commits and commitlint style:

```sh
# For conventional commits (default)
oco config set OCO_PROMPT_MODULE=conventional-commit

# For commitlint style
oco config set OCO_PROMPT_MODULE=@commitlint
```

#### One-Line Commit Messages

To generate concise, one-line commit messages:

```sh
oco config set OCO_ONE_LINE_COMMIT=true
```

### Viewing Current Configuration

To view the current value of a configuration option:

```sh
oco config get <CONFIG_KEY>
```

Example:

```sh
oco config get OCO_MODEL
```

## Git flags

The `opencommit` or `oco` commands can be used in place of the `git commit -m "${generatedMessage}"` command. This means that any regular flags that are used with the `git commit` command will also be applied when using `opencommit` or `oco`.

```sh
oco --no-verify
```

is translated to :

```sh
git commit -m "${generatedMessage}" --no-verify
```

To include a message in the generated message, you can utilize the template function, for instance:

```sh
oco '#205: $msg'
```

> opencommit examines placeholders in the parameters, allowing you to append additional information before and after the placeholders, such as the relevant Issue or Pull Request. Similarly, you have the option to customize the OCO_MESSAGE_TEMPLATE_PLACEHOLDER configuration item, for example, simplifying it to $m!"

### Message Template Placeholder Config

#### Overview

The `OCO_MESSAGE_TEMPLATE_PLACEHOLDER` feature in the `opencommit` tool allows users to embed a custom message within the generated commit message using a template function. This configuration is designed to enhance the flexibility and customizability of commit messages, making it easier for users to include relevant information directly within their commits.

#### Implementation Details

In our codebase, the implementation of this feature can be found in the following segment:

```javascript
commitMessage = messageTemplate.replace(
  config?.OCO_MESSAGE_TEMPLATE_PLACEHOLDER,
  commitMessage
);
```

This line is responsible for replacing the placeholder in the `messageTemplate` with the actual `commitMessage`.

#### Usage

For instance, using the command `oco '$msg #205'`, users can leverage this feature. The provided code represents the backend mechanics of such commands, ensuring that the placeholder is replaced with the appropriate commit message.

#### Committing with the Message

Once users have generated their desired commit message, they can proceed to commit using the generated message. By understanding the feature's full potential and its implementation details, users can confidently use the generated messages for their commits.

### Ignore files

You can remove files from being sent to OpenAI by creating a `.opencommitignore` file. For example:

```ignorelang
path/to/large-asset.zip
**/*.jpg
```

This helps prevent opencommit from uploading artifacts and large files.

By default, opencommit ignores files matching: `*-lock.*` and `*.lock`

## Git hook (KILLER FEATURE)

You can set OpenCommit as Git [`prepare-commit-msg`](https://git-scm.com/docs/githooks#_prepare_commit_msg) hook. Hook integrates with your IDE Source Control and allows you to edit the message before committing.

To set the hook:

```sh
oco hook set
```

To unset the hook:

```sh
oco hook unset
```

To use the hook:

```sh
git add <files...>
git commit
```

Or follow the process of your IDE Source Control feature, when it calls `git commit` command — OpenCommit will integrate into the flow.

## Setup OpenCommit as a GitHub Action (BETA) 🔥

OpenCommit is now available as a GitHub Action which automatically improves all new commits messages when you push to remote!

This is great if you want to make sure all of the commits in all of your repository branches are meaningful and not lame like `fix1` or `done2`.

Create a file `.github/workflows/opencommit.yml` with the contents below:

```yml
name: 'OpenCommit Action'

on:
  push:
    # this list of branches is often enough,
    # but you may still ignore other public branches
    branches-ignore: [main master dev development release]

jobs:
  opencommit:
    timeout-minutes: 10
    name: OpenCommit
    runs-on: ubuntu-latest
    permissions: write-all
    steps:
      - name: Setup Node.js Environment
        uses: actions/setup-node@v2
        with:
          node-version: '16'
      - uses: actions/checkout@v3
        with:
          fetch-depth: 0
      - uses: di-sukharev/opencommit@github-action-v1.0.4
        with:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}

        env:
          # set openAI api key in repo actions secrets,
          # for openAI keys go to: https://platform.openai.com/account/api-keys
          # for repo secret go to: <your_repo_url>/settings/secrets/actions
          OCO_OPENAI_API_KEY: ${{ secrets.OCO_OPENAI_API_KEY }}

          # customization
          OCO_TOKENS_MAX_INPUT: 4096
          OCO_TOKENS_MAX_OUTPUT: 500
          OCO_OPENAI_BASE_PATH: ''
          OCO_DESCRIPTION: false
          OCO_EMOJI: false
          OCO_MODEL: gpt-3.5-turbo
          OCO_LANGUAGE: en
          OCO_PROMPT_MODULE: conventional-commit
```

That is it. Now when you push to any branch in your repo — all NEW commits are being improved by your never-tired AI.

Make sure you exclude public collaboration branches (`main`, `dev`, `etc`) in `branches-ignore`, so OpenCommit does not rebase commits there while improving the messages.

Interactive rebase (`rebase -i`) changes commits' SHA, so the commit history in remote becomes different from your local branch history. This is okay if you work on the branch alone, but may be inconvenient for other collaborators.

## Payments

You pay for your requests to OpenAI API on your own.

OpenCommit stores your key locally.

OpenCommit by default uses 3.5-turbo model, it should not exceed $0.10 per casual working day.

You may switch to gpt-4, it's better, but more expensive.
