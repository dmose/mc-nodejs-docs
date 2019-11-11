# How to Vendor

### Vendoring in a new package

- Review available packages with functionality you want, keeping in mind the
  guideliness in the [Package Selection section of the main policy
  doc](./index#package-selection).
  - Check that this adheres to the requirements in the [Policy Section of the main policy
    doc](./index#policy), or talk to a reviewer about any desired exceptions.
  - XXX security steps -- fill in after we get general agreement on the policies
  - XXX license steps -- fill in after we get general agreement on the policies
  - XXX avoid contained build system -- fill in after we get general agreement
    on policy
- Reach out to a NodeJS module reviewer and request approval to vendor this package
-
- Use something like this command to install (eventually `mach vendor node` will do these steps):
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
