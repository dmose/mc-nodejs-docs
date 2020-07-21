# Step-By-Step Vendoring Guide
### Vendoring in a new package

1. Reach out to a [NodeJS module peer](XXXLINKME), and request their help with
  choosing and landing this package.
2. Review available packages with functionality you want, keeping in mind the
  guideliness in the [Package Selection section of the main policy
  doc](./index#package-selection).
3. Work with your reviewer to select the best package
4. Use `mach vendor node install` to vendor in the package (this will handle
   installing with the correct `npm` options and validating security and
   license invariants).
   <details><summary>Details</summary>

    To be implemented in `mach vendor node install`:
     * ```npm install --save-exact --save-dev --no-bin-links --no-optional package@version --ignore-scripts```
     * run `mach lint node`, which will:
       * run a license linter locally (until implemented, see http://npm.broofa.com/)
       * run `npm audit`
       * run [`lockfile-lint`](https://snyk.io/blog/why-npm-lockfiles-can-be-a-security-blindspot-for-injecting-malicious-modules/)

   </details>
5. Review the generated patch to be sure it looks like you would expect.
6. Commit as per the General Policy section.
7. Upload your patch to Phabricator and request review.

### Updating an existing package

1. Use `mach vendor node install` to vendor in the new version of the
   package and validate the usual invariants.
2. Check whether any APIs have changed, and adjust the minimal amount of code
   necessary to keep the builds running and tests passing.  File follow-up bugs
   for any other changes you want to make.
3. Execute steps 5-7 from the previous section.

### Removing a package

1. Use `npm uninstall --save-dev`
2. Commit and review as usual
