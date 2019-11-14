# NPM/NodeJS-Specific Threat Modelling and Mitigation

This covers the NodeJS specific threats that have been considered as a part of
this plan to allow NodeJS usage in the build system.  It also includes some
npm/NodeJS-specific mitigations to more general vendoring concerns.
It is not, however, currently intended to include the threats that arise from including any third-party code in Firefox.

## Packages unpublished from npm (left-pad)

Previously a popular npm package was unpublished from the repository meaning that attempts to install packages that required it started failing. There are more details at the [npm blog](https://blog.npmjs.org/post/141577284765/kik-left-pad-and-npm).

Our chief protection against this is that we are physcially vendoring required modules into mozilla-central. The npm repository and the packages in it are only needed when adding a new package to the tree or upgrading an existing package.

In addition, npm have adopted a [new policy](https://www.npmjs.com/policies/unpublish) that requires working with npm support to unpublish any package that has been published for more than 72 hours.

## Packages that execute scripts on install

Node packages can provide scripts to run at install time. These scripts will be run for all dependencies of the package you are actually installing. A [previous security incident](https://blog.npmjs.org/post/175824896885/incident-report-npm-inc-operations-incident-of) used this in order to attempt to steal the npm credentials of users installing the `eslint` package.

Again, vendoring means that this is only an issue when adding or upgrading
packages. In addition, `mach vendor node` will use npm with the
`--ignore-scripts` option, which turns off running these scripts. For some
packages, this will mean installation will fail. Those cases will be reviewed
on a case by case basis.

## Packages with similar names

Developers have, in the past, [registered malicious packages with names very similar to popular npm packages](https://blog.npmjs.org/post/163723642530/crossenv-malware-on-the-npm-registry). This can lead to accidentally adding a dependency on a malicious package rather than the expected package.

npm, Inc have introduced [new package naming rules](https://blog.npmjs.org/post/168978377570/new-package-moniker-rules) that
reduce the abilitiy to register package names that are similar to other
packages in certain ways.  However, it is still possible for this to be an
issue.

Vendoring means we are only vulnerable when adding or upgrading packages. Reviewing the modifications to `package.json` should catch instances of this happening at the top level, however, we will have top rely on security scans such as `npm audit` and `snyk` to catch this for deeper dependencies.

# General vendoring threats, with some npm/NodeJS-specific commentary

## Increased attack surface due to large dependency trees

npm packages often have a substantially larger dependency set than packages in
other ecosystems, but this varies greatly.  As an example, there are a variety
of JavaScript bundlers in the ecosystem.  rollup is specifically focussed on ES
modules; webpack is more of Swiss army knife.  As of this writing, rollup has 3
total dependencies, while webpack has 304.  

This will already be mitigated by the fact that we're vendoring packages in,
rather than installing them at build time, as well as with regular automated
`npm audit` (and probably `snyk`) scanning.

Other possible mitigations include:

### Setting limits on the number of dependencies allowed for a given package

Whether this is worth it needs to be discussed further.

[TODO(nodejs-peers & sec folks): need to decide before signoff].

### Encourage or enforce a waiting period after release and before vendoring

The rationale is that we don't necessarily need to be the first to use most
upgrades.  That gives time for at least some subset of flaws or malice to
surface before we start using it.  Obviously, exceptions would need
to be made for (for example), known security fixes.

[TODO(nodejs-peers & sec folks): need to decide before signoff]

### Sandbox the builds

There is nothing npm-specific about this, but it's [been talked about for a little while](https://bugzilla.mozilla.org/show_bug.cgi?id=1510416),
and would definitely help.

It would prevent at least two classes of attacks: writing to other parts
of the developers' system, and exfiltrating developer data (e.g. `ssh` keys or
bitcoins).  It would likely make remediation of many attacks easier as well.

We should consider the possibility of resourcing this as a project, but it's
probably a big enough piece that we don't want it to block improving our
existing NodeJS infrastructure.  Which is to say that NodeJS/npm is already
informally in use in various parts of the tree, and this project should help
make it more auditable and manageable.
