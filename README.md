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

## Bit
To allow easy and up-to date reuse of code between projects there was a plan to use
[Bit](https://bit.dev).

On to Bit it self. It's a CLI installed via NPM and allows use to export components and manage
their dependencies. An example usage for the frontend code would be that we have a button which
should have the same logic in two projects and some standard styles. Project A created the button
and project B want to use it with a different color. Project A would create a component and export
it, B would import the button and then locally change the color. If the button later is updated by
A the changes can still be pulled in by B. The system closely resembles Git with commands like
`add` and `status`.

After further research Bit has a couple of issues. While exporting a component is simple and quick
the importing is harder. Since Bit want the components to be consumed by Bit and NPM they get some
extra files when imported to a project, including node_modules and package.json.

If we look at some other specific projects like the agentvegan-server we ran into some other issues
with linting and ms-description-checks. Linting was failing since Bit created stub files and the 
ms-description-checks were hard to implement since some of the code would become NPM modules. And
to the point of configuration values and loggers. This would end up in having to pass options
objects down to each microservice which feel like an annoying thing to do.

In the end we decided against Bit for some of the reasons above.

## GitHub


## Email
Currently the plan is to use Mailjet as the email service. There was a choice between it and multiple other services, one
of which was Litmus. Litmus offers more in terms of previewing on different devices and clients but lacks a visual editor which
makes it hard to have a regular marketing person creating email. Mailjet has an editor but lacks the great previews, they do seem to
have checked that their visual editor makes things compatible with all the clients so I'm less worried than if we write the HTML
ourselves as we would with Litmus.

Bottom line is that Mailjet looks like the correct choice and that we are currently checking if SSO is available.

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
