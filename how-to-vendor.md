# How to Vendor

### Vendoring in a new package

1. Reach out to a [NodeJS module peer](XXXLINKME), and request their help on
  choosing and landing this package.
2. Review available packages with functionality you want, keeping in mind the
  guideliness in the [Package Selection section of the main policy
  doc](./index#package-selection)
3. Work with your reviewer to select a package that meets each requirement in
   the [Policy Section of the main policy doc](./index#policy), or that the
   reviewer agrees should be given an exception to.
  - XXX security steps -- fill in after we get general agreement on the policies
4. Reach out to a NodeJS module reviewer and request approval to vendor this package
5. Use something like this command to install (eventually `mach vendor node` will do these steps):
- npm install --save-exact --save-dev|---save|--save-optional --no-bin-links
  --no-optional package@version --ignore-scripts
- Commit as per license section (XXX pending sheriff approval)

### Updating an existing package

- (XXX security audit steps)
- (XXX license audit steps)
- (XXX own build-system audit steps)
- Commit as per license section (XXX pending sheriff approval)

### Removing a package

- Commit as per license section (XXX pending sheriff approval)
