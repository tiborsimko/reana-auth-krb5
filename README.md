# REANA Authentication Kerberos5

[![image](https://github.com/reanahub/reana-auth-krb5/workflows/CI/badge.svg)](https://github.com/reanahub/reana-auth-krb5/actions)
[![image](https://img.shields.io/badge/discourse-forum-blue.svg)](https://forum.reana.io)
[![image](https://img.shields.io/github/license/reanahub/reana-auth-krb5.svg)](https://github.com/reanahub/reana-auth-krb5/blob/master/LICENSE)

## About

`reana-auth-krb5` provides a container image for creating and renewing
Kerberos tokens. The container image includes no additional logic or
libraries, just the bare minimum to support the Kerberos operations.

`reana-auth-krb5` was developed for use in the
[REANA](http://www.reana.io/) reusable research data analysis platform.

## Usage

The `reana-auth-krb5` image is used internally in the REANA platform to
refresh the Kerberos token for long running jobs. The end users can ask
for Kubernetes authentication by means of declaring `kerberos: true`,
see the [Kerberos access control documentation](http://docs.reana.io/advanced-usage/access-control/kerberos/)
for more information.

If you want to try it locally, a Kerberos token can be obtained via:

```console
$ docker run -i -t --rm docker.io/reanahub/reana-auth-krb5:1.0.3 /bin/bash
> kinit -k -t /path/to/keytab_file username@CERN.CH
> klist
```

## Configuration

Running the `reana-auth-krb5` and successfully obtaining a shared token
on a sidecar container requires additional information and inputs:

- [Kerberos cache](https://web.mit.edu/kerberos/krb5-1.12/doc/basic/ccache_def.html)
  location to be shared, configured through the `KRB5CCNAME`
  environment variable
- [Kerberos configuration](https://web.mit.edu/kerberos/krb5-1.12/doc/admin/conf_files/krb5_conf.html)
  at `/etc/krb5.conf` (overridable)

## Changes

Version 1.0.3 (2024-04-18)

- Upgrade container base image to Ubuntu 20.04 LTS for consistency
  with the other REANA cluster components.

Version 1.0.2 (2024-04-17)

- Add new dependency `inotify-tools` that provides the `inotifywait`
  utility that is used by REANA 0.95 to quickly stop the Kerberos
  sidecar containers when the user jobs finish their execution.

Version 1.0.1 (2020-08-12)

- Add CERN Kerberos configuration.

Version 1.0.0 (2020-08-05)

- Initial release

## Development

If you would like to contribute to `reana-auth-krb5` development, you
can take advantage of the provided `Makefile`:

```console
$ make build  # build a new version of the container image
$ make test   # test the built image
$ make push   # push it to Docker Hub
```

## More information

For more information about [REANA](http://www.reana.io/) reusable
research data analysis platform, please see
[its documentation](http://docs.reana.io/).
