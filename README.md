<h1 align="center">
	<img src="https://github.com/orisai/.github/blob/main/images/repo_title.png?raw=true" alt="Orisai"/>
	<br/>
	GitHub Workflows
</h1>

<p align="center">
    <a href="https://docs.github.com/en/actions">GitHub Actions</a> reusable workflows and composed actions
</p>

<p align="center">
	📄 Check out our <a href="docs/README.md">documentation</a>.
</p>

<p align="center">
	💸 If you like Orisai, please <a href="https://orisai.dev/sponsor">make a donation</a>. Thank you!
</p>

<p align="center">
	<a href="https://choosealicense.com/licenses/mpl-2.0/">
		<img src="https://badgen.net/badge/license/MPL-2.0/blue?cache=3600">
	</a>
<p>

##

```yaml
name: "Static analysis"
on: ["push", "pull_request"]

jobs:
  static-analysis:
    name: "Static analysis"
    runs-on: "ubuntu-latest"

    steps:
      - name: "Checkout"
        uses: "actions/checkout@v2"

      - name: "PHP"
        uses: "orisai/github-workflows/.github/actions/setup-php@v1"
        with:
          version: "8.1"
          token: "${{ secrets.GITHUB_TOKEN }}"

      - name: "Composer"
        uses: "orisai/github-workflows/.github/actions/setup-composer@v1"

      - name: "PHPStan"
        uses: "orisai/github-workflows/.github/actions/phpstan@v1"
        with:
          command: "vendor/bin/phpstan"
          cache-path: "var/tools/PHPStan"
```

Example is simplified, check [documentation](docs/README.md) for complex version
