# ns8-commit-status

This is a template module for [NethServer 8](https://github.com/NethServer/ns8-core).
To start a new module from it:

1. Click on [Use this template](https://github.com/NethServer/ns8-commit-status/generate).
   Name your repo with `ns8-` prefix (e.g. `ns8-mymodule`). 
   Do not end your module name with a number, like ~~`ns8-baaad2`~~!

1. Clone the repository, enter the cloned directory and
   [configure your GIT identity](https://git-scm.com/book/en/v2/Getting-Started-First-Time-Git-Setup#_your_identity)

1. Rename some references inside the repo:
   ```
   modulename=$(basename $(pwd) | sed 's/^ns8-//')
   git mv imageroot/systemd/user/commit-status.service imageroot/systemd/user/${modulename}.service
   git mv tests/commit-status.robot tests/${modulename}.robot
   sed -i "s/commit-status/${modulename}/g" $(find .github/ * -type f)
   git commit -a -m "Repository initialization"
   ```

1. Edit this `README.md` file, by replacing this section with your module
   description

1. Adjust `.github/workflows` to your needs. `clean-registry.yml` might
   need the proper list of image names to work correctly. Unused workflows
   can be disabled from the GitHub Actions interface.

1. Commit and push your local changes

## Install

Instantiate the module with:

    add-module ghcr.io/nethserver/commit-status:latest 1

The output of the command will return the instance name.
Output example:

    {"module_id": "commit-status1", "image_name": "commit-status", "image_url": "ghcr.io/nethserver/commit-status:latest"}

## Configure

Let's assume that the commit-status instance is named `commit-status1`.

Launch `configure-module`, by setting the following parameters:
- `<MODULE_PARAM1_NAME>`: <MODULE_PARAM1_DESCRIPTION>
- `<MODULE_PARAM2_NAME>`: <MODULE_PARAM2_DESCRIPTION>
- ...

Example:

    api-cli run module/commit-status1/configure-module --data '{}'

The above command will:
- start and configure the commit-status instance
- (describe configuration process)
- ...

Send a test HTTP request to the commit-status backend service:

    curl http://127.0.0.1/commit-status/

## Smarthost setting discovery

Some configuration settings, like the smarthost setup, are not part of the
`configure-module` action input: they are discovered by looking at some
Redis keys.  To ensure the module is always up-to-date with the
centralized [smarthost
setup](https://nethserver.github.io/ns8-core/core/smarthost/) every time
commit-status starts, the command `bin/discover-smarthost` runs and refreshes
the `state/smarthost.env` file with fresh values from Redis.

Furthermore if smarthost setup is changed when commit-status is already
running, the event handler `events/smarthost-changed/10reload_services`
restarts the main module service.

See also the `systemd/user/commit-status.service` file.

This setting discovery is just an example to understand how the module is
expected to work: it can be rewritten or discarded completely.

## Uninstall

To uninstall the instance:

    remove-module --no-preserve commit-status1

## Testing

Test the module using the `test-module.sh` script:


    ./test-module.sh <NODE_ADDR> ghcr.io/nethserver/commit-status:latest

The tests are made using [Robot Framework](https://robotframework.org/)

## UI translation

Translated with [Weblate](https://hosted.weblate.org/projects/ns8/).

To setup the translation process:

- add [GitHub Weblate app](https://docs.weblate.org/en/latest/admin/continuous.html#github-setup) to your repository
- add your repository to [hosted.weblate.org]((https://hosted.weblate.org) or ask a NethServer developer to add it to ns8 Weblate project
