# CircleCI Example

## Pre-requisites

In order to rotate a key that's stored in CircleCI env vars, you'll need:

1. A GitHub user (preferably a dedicated machine-user, rather than a human user)
with write access to the GitHub repository that the CircleCI project is linked to.
2. A CircleCI API key for the GitHub user, which can be generated by logging in
to [circleci.com](circleci.com) as the user, then creating a [personal API token](https://circleci.com/account/api).
3. Auth to actually perform the rotation operation with whichever cloud provider
you're using. This will require a service-account or user (with the cloud-provider you're rotating with) that has the required set of permissions. Then, auth will
need to be given to `cloud-key-rotator` (usually in the form of a .json file or
env vars).

## Configuration

```json
  "AccountKeyLocations": [
    {
      "ServiceAccountName": "my_aws_machine_user",
      "CircleCI": [
        {
          "UsernameProject": "my_org/my_repo"
        }
      ]
    }
  ],
  "Credentials": {
    "CircleCIAPIToken": "my_circle_ci_api_token"
  }
```

When rotating AWS keys, there are some optional fields,
`keyIDEnvVar` and `keyEnvVar`, that represent the env var names in CircleCI,
defaulting to values `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`
respectively.

So, if you store your Key ID and Key values in env vars in CircleCI that're
named differently, you could set something like this instead:

```json
    "CircleCI": [{
      "UsernameProject": "my_org/my_repo",
      "KeyIDEnvVar": "AWS_KEY_ID",
      "KeyEnvVar": "AWS_KEY"
    }]
```

When rotating GCP keys, to override the default CircleCI env var name (`GCLOUD_SERVICE_KEY`), 
you only need to override the `KeyEnvVar` value (as only a single value,
the key, is needed for GCP)

```json
    "CircleCI": [{
      "UsernameProject": "my_org/my_repo",
      "KeyEnvVar": "GCP_KEY"
    }]
```