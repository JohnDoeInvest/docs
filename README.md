# Docs

## Publish to NPM
We have been following [this](https://gist.github.com/coolaj86/1318304) guide for publishing to NPM.

## Updating package on NPM
When we are ready to publish an update to NPM we update the version number according to [semver](https://semver.org/) in 
the `package.json`. After this we run `npm publish ./` in the root of the project and the package should be published.

## Reverting a inccorect publish
The documentation for this is here: https://www.npmjs.com/policies/unpublish. Note that some of these require waiting 
for 24 hours until a new publish can be performed so be careful!
