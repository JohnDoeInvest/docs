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
To allow easy and up-to date reuse of code between projects the plan is to use [Bit](https://bit.dev). An issues with Bit is that it
has not way of handling SSO, you can login with GitHub so that could work for us but when you do you still need to "create an account".
This creation is I guess mostly just to place you in their system and it require only a username, pre-filled from GitHub.

On to Bit it self. It's a CLI installed via NPM and allows use to export components and manage their dependencies. An example usage for
the frontend code would be that we have a button which should have the same logic in two projects and some standard styles. Project A
created the button and project B want to use it with a different color. Project A would create a component and export it, B would
import the button and then locally change the color. If the button later is updated by A the changes can still be pulled in by B. The
system closely resembles Git with commands like `add` and `status`. 

## GitHub


## Email
Currently the plan is to use Mailjet as the email service. There was a choice between it and multiple other services, one
of which was Litmus. Litmus offers more in terms of previewing on different devices and clients but lacks a visual editor which
makes it hard to have a regular marketing person creating email. Mailjet has an editor but lacks the great previews, they do seem to
have checked that their visual editor makes things compatible with all the clients so I'm less worried than if we write the HTML
ourselves as we would with Litmus.

Bottom line is that Mailjet looks like the correct choice and that we are currently checking if SSO is available.

## Release of applications
### Server
To begin a release we start with commiting a change of version number in the package.json. After this we tag that branch with a version format like `vx.y.z`, for example `v1.0.0`, which is the same version as package.json. This should then be pushed to GitHub.

Currently we will then be logging in to the server and pull the changes and run `pm2 restart Server`. Later this should probably be done automatically when the version tag is pushed to master.

### Frontend
To begin a release we start with commiting a change of version number in the package.json. Then we merge the `develop` branch in to the `master` branch. After this we tag that branch with a version format like `vx.y.z`, for example `v1.0.0`, which is the same version as package.json. This should then be pushed to GitHub.

Currently we will then be logging in to the server and pull the changes and run `npm run build` and then `pm2 restart Frontend`. Later this should probably be done automatically when the version tag is pushed to master.
