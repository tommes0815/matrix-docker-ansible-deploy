# Setting up Beeper Linkedin bridging (optional)

The playbook can install and configure [beeper-linkedin](https://github.com/beeper/linkedin) for you, for bridging to [LinkedIn](https://www.linkedin.com/) Messaging. This bridge is based on the mautrix-python framework and can be configured in a similar way to the other mautrix bridges

See the project's [documentation](https://github.com/beeper/linkedin/blob/master/README.md) to learn what it does and why it might be useful to you.

## Adjusting the playbook configuration

To enable the bridge, add the following configuration to your `inventory/host_vars/matrix.example.com/vars.yml` file:

```yaml
matrix_beeper_linkedin_enabled: true
```

There are some additional things you may wish to configure about the bridge before you continue.

Encryption support is off by default. If you would like to enable encryption, add the following to your `vars.yml` file:
```yaml
matrix_beeper_linkedin_configuration_extension_yaml: |
  bridge:
    encryption:
      allow: true
      default: true
```

If you would like to be able to administrate the bridge from your account it can be configured like this:
```yaml
matrix_beeper_linkedin_configuration_extension_yaml: |
  bridge:
    permissions:
      '@YOUR_USERNAME:example.com': admin
```

You may wish to look at `roles/custom/matrix-bridge-beeper-linkedin/templates/config.yaml.j2` to find other things you would like to configure.

## Installing

After configuring the playbook, run the [installation](installing.md) command: `just install-all` or `just setup-all`

## Set up Double Puppeting by enabling Appservice Double Puppet or Shared Secret Auth

The bridge will automatically perform Double Puppeting if you enable the [Appservice Double Puppet](configuring-playbook-appservice-double-puppet.md) service or the [Shared Secret Auth](configuring-playbook-shared-secret-auth.md) service for this playbook.

Enabling [Appservice Double Puppet](configuring-playbook-appservice-double-puppet.md) is the recommended way of setting up Double Puppeting, as it's easier to accomplish, works for all your users automatically, and has less of a chance of breaking in the future.

Enabling double puppeting by enabling the [Shared Secret Auth](configuring-playbook-shared-secret-auth.md) service works at the time of writing, but is deprecated and will stop working in the future.


## Usage

You then need to start a chat with `@linkedinbot:example.com` (where `example.com` is your base domain, not the `matrix.` domain).

Send `login YOUR_LINKEDIN_EMAIL_ADDRESS` to the bridge bot to enable bridging for your LinkedIn account.

If you run into trouble, check the [Troubleshooting](#troubleshooting) section below.

After successfully enabling bridging, you may wish to [set up Double Puppeting](#set-up-double-puppeting), if you haven't already done so.


## Troubleshooting

### Bridge asking for 2FA even if you don't have 2FA enabled

If you don't have 2FA enabled and are logging in from a strange IP for the first time, LinkedIn will send an email with a one-time code. You can use this code to authorize the bridge session. In my experience, once the IP is authorized, you will not be asked again.
