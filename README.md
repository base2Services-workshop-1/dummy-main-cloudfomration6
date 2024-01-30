# Main Cloudformation Sync Repo

## Inital Repo Setup Tasks

### Rename master branch to main

Due to current limitations of the AWS CodeStar Github Cloudformation resource when it creates a repo it creates a master branch to rename running the following command

```bash
git branch -m master main
git push origin main -u
```

Then goto to github and change to main in the repo settings

### Enabling GitHub Actions

To enable github actions for this repo run

```bash
git checkout -b enable-gh-actions
git mv github-actions .github
git add .github/
git add github-actions/
git commit -m 'enables github actions'
git push origin enable-gh-actions -u
```

Then create a Pull-Request from the `enable-gh-actions` to `main` and merge

## Environment Setup

See [Stacks README](stacks/README.md)

## CfHighlander Components

CfHighlander components can be added to the root directory of the repo. Each component will be compiled automatically by the .github/workflows/build-cloudformation.yml workflow and copied checked into the stacks/templates directory as part of a pull-request.

### Compiling the templates locally

Assuming you have `make` installed and `docker` running locally you can run the following:

```bash
$ make test
```
This will compile all the cfhighlander components so you can check they compile correctly before committing them

You might also want to diff the compiled template changes prior to pushing. To do that you can run

```bash
$ make build
```

This will compile the templates and copied the compiled templates to the `stacks/templates` directory. You can then do a `git diff` to check the changes. But remember not to include the compiled templates when you commit the changes

### Current Limitations

1. You can only deploy standalone stacks using git-cfn-sync, nested stacks are not supported. This is due to a limitation of the cloudformation git sync and nested stacks URLs. So best practice is to isoloate related resources into there own components and use the `render: Inline` option when including multiple cfhighlander components in one stack. See [main.cfhighlander.rb](main.cfhighlander.rb) as an example.

## Native Cloudformation support

It is possible to use git-cfn-sync without cfhighlander as it only provides a convinent way to generate cloudformation templates. Do to this you can just checkin raw cloudformation templates into the `stacks/templates` directory and then reference the template in `template-file-path` in the stack deployment file.