# Docs

## NPM
### Publish to NPM
We have been following [this](https://gist.github.com/coolaj86/1318304) guide for publishing to NPM.

### Updating package on NPM
When we are ready to publish an update to NPM we update the version number according to [semver](https://semver.org/) in 
the `package.json`. After this we run `npm publish ./` in the root of the project and the package should be published.

### Reverting a inccorect publish
The documentation for this is here: https://www.npmjs.com/policies/unpublish. Note that some of these require waiting 
for 24 hours until a new publish can be performed so be careful!

## Frontend
### Goals
Give that the following "DoubleClick by Google found 53% of mobile site visits were abandoned if a page took longer than 3 seconds to load." our biggest goal with the frontend is that it loads quickly. That has a couple of impacts. Number one is the size of libraries, when looking for a new library we don't just get the first result we find. We need to research alternatives, look at size, feature set and activty. Each of these are important on it's own but the most important one is size. Number two, assets need to be small. No huge images, at least not for all clients. And number three, look at the numbers. Lighthouse is a great tool to tell us how we are doing and there are other tools which might give us other information.

### Browser support
Our goal is to support as many users as possible. This means that we need to support both old and new browsers which hurts the size of our application. Since old browsers don't support all API's we need to add extra code to support those as well. What we have ended up doing is building two separate bundles of code to ship down to the clients depending on their browser. Using Browserslists what we support in production with `> 0.5% and not dead` and then we also defined which browsers are considered modern [as of 2019-11-18](https://caniuse.com/#compare=ie+11,edge+15,firefox+53,chrome+58,safari+11,opera+44,ios_saf+11.0-11.2,android+76,op_mob+46,and_chr+78,and_ff+68,and_uc+12.12,samsung+7.2-7.4&compare_cats=). 

## Email
Our email service is SendInBlue.

## Release of applications
A release is done by creating a release branch with the name `release-vX.Y.Z`. Checkout the branch
and update the `package.json` version to the new version, same as the `X.Y.Z` in the branch name,
and then run `npm install` to update the `package-lock.json`. Commit this with the message 
"Release vX.Y.Z" and the push the branch to GitHub. Create a pull request towards the `master`
branch, try to write a short changelog in the pull request description.

When the pull request is approved by enough people we merge it into `master` and then we create a
tag with the name `vX.Y.Z` (This can be done with Releases in GitHub). After this we create a new
pull request towards the `develop` branch. When this pull request is approved by enough people we
merge it into `develop`.

Currently we will then be logging in to the server to update the applications and we need to do
the following commands.
(Later this should probably be done automatically when the version tag is pushed to master.)

**Frontend**
* `git fetch`
* `git checkout vX.Y.Z`
* `npm install`
* `npm run build`
* `pm2 restart Frontend`

**Server**
* `git fetch`
* `git checkout vX.Y.Z`
* `npm install`
* `pm2 restart Server`
