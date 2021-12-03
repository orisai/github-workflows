# Github Workflows

[Github Actions](https://docs.github.com/en/actions) reusable workflows and composed actions

## Content

- [Setup](#setup)
- [Workflows](#workflows)
	- [Lock closed issues and PRs](#lock-closed-issues-and-prs)
- [Actions](#actions)
	- [Inputs](#inputs)
	- [Setup PHP](#setup-php)
	- [Setup Composer](#setup-composer)
	- [PHPUnit](#phpunit)
	- [PHPStan](#phpstan)
	- [PHP_CodeSniffer](#php_codesniffer)
	- [Infection PHP](#infection-php)
	- [Setup NodeJS](#setup-nodejs)
	- [Coveralls PHP upload](#coveralls-php-upload)
	- [Coveralls finish](#coveralls-finish)

## Setup

Setup any workflow with few simple steps:

- Create `.github/workflows` directory
- Create file `workflow-name.yaml` in the directory
- Copy one of following workflows into the file
- Set required inputs (if workflow has any)
- Set `on` run condition (or use our recommended defaults)
- Good to go!

## Workflows

### Lock closed issues and PRs

Lock inactive closed issues and PRs after period of time.

`lock-closed-threads.yaml`:

```yaml
name: "Lock inactive closed issues and PRs"

on:
  schedule:
    - cron: "0 0 * * *"

jobs:
  lock:
    uses: "orisai/github-workflows/.github/workflows/lock-closed-threads.yaml@v1.x"
```

Inputs:

`issue-lock-inactive-days`

- Lock inactive closed issue after X days
- type: `string`
- default: `"30"`

`pr-lock-inactive-days`

- Lock inactive closed PR after X days
- type: `string`
- default: `"60"`

## Actions

Multiple actions composed into one, simpler and easier to read action.

### Inputs

Certain actions can be configured through inputs. Inputs are defined via `with` key. Check individual actions for
specific options.

```yaml
jobs:
  example:
    steps:
      - name: "example"
        uses: "example/action@v1.0"
        with:
          input-name: "Value"
```

### Setup PHP

Setup [PHP](https://www.php.net), with extensions, build caching and reporting of issues via GitHub Actions interface.

```yaml
jobs:
  example:
    strategy:
      matrix:
        php-version: [ "7.4", "8.0" ]

    steps:
      - name: "PHP"
        uses: "orisai/github-workflows/.github/actions/setup-php@v1.x"
        with:
          version: "${{ matrix.php-version }}"
          token: "${{ secrets.GITHUB_TOKEN }}"
```

Inputs:

`version`

- PHP version
- type: `string`
- required

`coverage`

- Code coverage tool
- type: `string`
- default: `"none"`
- values: `"none"`, `"pcov"`, `"xdebug"`

`extensions`

- PHP extensions
- type: `string` (extension names separated by a comma `,`)
- default: `"fileinfo, intl, json, mbstring, sodium"`

`token`

- GitHub token for Composer to prevent being rate limited by GitHub API
- type: `string`
- required

### Setup Composer

Setup [Composer](https://getcomposer.org) (v2), with dependencies caching and installation.

```yaml
jobs:
  example:
    steps:
      - name: "Composer"
        uses: "orisai/github-workflows/.github/actions/setup-composer@v1.x"
```

Inputs:

`command`

- Composer's installation command
- type: `string`
- default: `"composer install --no-interaction --no-progress --prefer-dist"`

### PHPUnit

Run [PHPUnit](https://phpunit.de), store cached data between runs and reporting of issues via GitHub Actions interface.

```yaml
jobs:
  example:
    steps:
      - name: "PHPUnit"
        uses: "orisai/github-workflows/.github/actions/phpunit@v1.x"
        with:
            command: "make coverage-clover"
            cache-path: "var/tools/PHPUnit"
```

Inputs:

`command`

- PHPUnit command
- type: `string`
- required

`cache-path`

- PHPUnit cache directory
- type: `string`
- required

### PHPStan

Run [PHPStan](https://phpstan.org) and store cached data between runs.

```yaml
jobs:
  example:
    steps:
      - name: "PHPStan"
        uses: "orisai/github-workflows/.github/actions/phpstan@v1.x"
        with:
            command: "make phpstan"
            cache-path: "var/tools/PHPStan"
```

Inputs:

`command`

- PHPStan command
- type: `string`
- required

`cache-path`

- PHPStan cache directory
- type: `string`
- required

### PHP_CodeSniffer

Run [PHP_CodeSniffer](https://github.com/squizlabs/PHP_CodeSniffer) and store cached data between runs.

```yaml
jobs:
  example:
    steps:
      - name: "PHP_CodeSniffer"
        uses: "orisai/github-workflows/.github/actions/php-codesniffer@v1.x"
        with:
            command: "make cs ARGS='--report=checkstyle -q | vendor/bin/cs2pr'"
            cache-path: "var/tools/PHP_CodeSniffer"
```

Inputs:

`command`

- PHP_CodeSniffer command
- type: `string`
- required

`cache-path`

- PHP_CodeSniffer cache directory
- type: `string`
- required

### Infection PHP

Run [Infection PHP](https://infection.github.io), store cached data between runs and send coverage report to Stryker dashboard.

```yaml
jobs:
  example:
    steps:
      - name: "Infection PHP"
        uses: "orisai/github-workflows/.github/actions/infection-php@v1.x"
        with:
            command: "make mutations-infection ARGS='--logger-github'"
            cache-path: "var/tools/Infection"
            stryker-token: "${{ secrets.STRYKER_DASHBOARD_API_KEY }}"
```

Inputs:

`command`

- Infection command
- type: `string`
- required

`cache-path`

- Infection cache directory
- type: `string`
- required

`stryker-token`

- Stryker token for submitting code coverage
- type: `string`
- required

### Setup NodeJS

Setup [NodeJS](https://nodejs.dev), with dependencies caching and installation.

```yaml
jobs:
    strategy:
      matrix:
        nodejs-version: [ "14" ]

    example:
        steps:
            - name: "NodeJS"
              uses: "orisai/github-workflows/.github/actions/setup-nodejs@v1.x"
              with:
                  version: "${{ matrix.nodejs-version }}"
```

Inputs:

`version`

- NodeJS version
- type: `string`
- required

`command`

- NodeJS's installation command
- type: `string`
- default: `"npm ci"`

### Coveralls PHP upload

Upload code coverage report to [coveralls.io](https://coveralls.io)
via [php-coveralls](https://github.com/php-coveralls/php-coveralls). Requires finishing report
via [coveralls-finish](#coveralls-finish) action.

```yaml
jobs:
  example:
    steps:
      - name: "Coveralls"
        if: "${{ github.event_name == 'push' }}"
        uses: "orisai/github-workflows/.github/actions/coveralls-php-upload@v1.x"
        with:
            config: "tools/.coveralls.yml"
            token: "${{ secrets.GITHUB_TOKEN }}"
```

Inputs:

`config`

- Configuration file of php-coveralls
- type: `string`
- required

`token`

- GitHub token for Coveralls
- type: `string`
- required

### Coveralls finish

Finish [coveralls.io](https://coveralls.io) report started via [Coveralls PHP upload](#coveralls-php-upload) action.

```yaml
jobs:
  example:
    steps:
      - name: "Coveralls"
        if: "${{ github.event_name == 'push' }}"
        uses: "orisai/github-workflows/.github/actions/coveralls-finish@v1.x"
        with:
            token: "${{ secrets.GITHUB_TOKEN }}"
```

Inputs:

`token`

- GitHub token for Coveralls
- type: `string`
- required
