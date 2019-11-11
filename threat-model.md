# Threat model

This covers the node specific threats that have been considered as a part of
this plan to allow NodeJS usage in the build system. It is not intended to
include the threats that arise from including any third-party code in Firefox.

## Packages unpublished from npm (left-pad)

Previously a popular npm package was unpublished from the repository meaning that attempts to install packages that required it started failing. There are more details at the [npm blog](https://blog.npmjs.org/post/141577284765/kik-left-pad-and-npm).

Our chief protection against this is that we are physcially vendoring required modules into mozilla-central. The npm repository and the packages in it are only needed what adding a new package to the tree or upgrading an existing package.

In addition npm have adopted a [new policy](https://www.npmjs.com/policies/unpublish) that requires working with npm support to unpublish any package that has been published for more than 72 hours.

## Packages that execute scripts on install

Node packages can provide scripts to run at install time. These scripts will be run for all dependencies of the package you are actually installing. A [previous security incident](https://blog.npmjs.org/post/175824896885/incident-report-npm-inc-operations-incident-of) used this in order to attempt to steal the npm credentials of users installing the `eslint` package.

Again, vendoring means that this is only an issue when adding or upgrading packages. In addition, `mach vendor node` will use npm with the `--ignore-scripts` option, which turns off running these scripts. For some packages, this will mean installation will fail. Those cases will be reviewed more thoroughly on a case by case basis.

## Packages with similar names

Developers [have in the past](https://blog.npmjs.org/post/163723642530/crossenv-malware-on-the-npm-registry) registered malicious packages with names very similar to popular npm packages. This can lead to accidentally adding a dependency on a malicious package rather than the expected package.

npm have introduced [new package naming rules](https://blog.npmjs.org/post/168978377570/new-package-moniker-rules) that reduce the abilitiy to register package names that are similar to other packages in certain ways however it is still possible for this to be an issue.

Vendoring means we are only vulnerable when adding or upgrading packages. Reviewing the modification to `package.json` should catch instances of this happening at the top level, however, we will have top rely on security scans such as `npm audit` and `snyk` to catch this for deeper dependencies.
