# Varnish Cache BOSH Release
Varnish Cache to sit at a near edge layer in front of Gorouter. 

## Development
For development purposes, you can deploy this to a Cloud Foundry environment hosted on BOSH Lite. To get one of these up and running, see https://github.com/govau/syslog-to-loggregator-boshrelease and execute up until successfully logging into Cloud Foundry API.

Set up our local workspace and pull down the latest Varnish Cache as it is referenced as a submodule:
```
$ cd ~
~$ git clone https://github.com/govau/varnish-cache-bosh-release.git
~$ cd varnish-cache-bosh-release
varnish-cache-bosh-release$ git submodule update --init --recursive
varnish-cache-bosh-release$ cd src/varnish-cache/
varnish-cache-bosh-release/src/varnish-cache$ git ls-remote
varnish-cache-bosh-release/src/varnish-cache$ git checkout refs/tags/varnish-6.0.0

```

Follow https://bosh.io/docs/create-release/ as a guide, make your modifications and push your changes to BOSH Lite with the example operator file in the manifests directory:

```
varnish-cache-bosh-release$ bosh create-release --force
varnish-cache-bosh-release$ bosh upload-release
cf-deployment$ bosh -d cf deploy -n cf-deployment.yml \
    -o operations/bosh-lite.yml \
    -o operations/use-compiled-releases.yml \
    -o ~/varnish-cache-bosh-release/manifests/add-varnish-cache.yml \
    -v system_domain=bosh-lite.com
```

By default, Varnish Cache runs on port 6081 with an origin port of 80.
