Cloud Foundry UAA Credentials Broker
=====================================
[![Code Climate](https://codeclimate.com/github/cloudfoundry-community/uaa-credentials-broker/badges/gpa.svg)](https://codeclimate.com/github/cloudfoundry-community/uaa-credentials-broker)

This service broker allows Cloud Foundry users to provision and deprovision UAA users and clients. UAA users managed by the broker are scoped to a given organization and space and can be used to push applications when password authentication is needed--for example, when deploying from a continuous integration service. UAA clients can be used to [leverage UAA authentication](https://cloud.gov/docs/apps/leveraging-authentication/) in tenant applications.

## Usage

### UAA users

* Create service instance:

    ```bash
    $ cf create-service cloud-gov-service-account space-deployer my-service-account
    ```

* Get dashboard link from service instance:

    ```bash
    $ cf service my-service-account

    Service instance: my-service-account
    Service: cloud-gov-service-account
    ...
    Dashboard: https://fugacious.18f.gov/m/k3MtzJWVZaNlnjBYJ7FUdpW2ZkDvhmQz
    ```

* Retrieve credentials from dashboard link.

* To delete the acount, delete the service instance:

    ```bash
    $ cf delete-service my-service-account
    ```

### UAA clients

* Create service instance:

    ```bash
    $ cf create-service cloud-gov-identity-provider oauth-client my-uaa-client \
        -c '{"redirect_uri": ["https://my.app.cloud.gov"]}'
    ```

* Retrieve credentials and deprovision instance as above

## Deployment

* Create UAA client:

    ```bash
    $ uaac client add uaa-credentials-broker \
        --name uaa-credentials-broker \
        --authorized_grant_types client_credentials \
        --authorities scim.write,uaa.admin,cloud_controller.admin \
        --scope uaa.none
    ```

* Update Concourse pipeline:

    ```bash
    fly -t ci set-pipeline -p uaa-credentials-broker -c pipeline.yml -l credentials.yml
    ```

## Public domain

This project is in the worldwide [public domain](LICENSE.md). As stated in [CONTRIBUTING](CONTRIBUTING.md):

> This project is in the public domain within the United States, and copyright and related rights in the work worldwide are waived through the [CC0 1.0 Universal public domain dedication](https://creativecommons.org/publicdomain/zero/1.0/).
>
> All contributions to this project will be released under the CC0 dedication. By submitting a pull request, you are agreeing to comply with this waiver of copyright interest.
