# Minica

Minica is a simple CA intended for use in situations where the CA operator
also operates each host where a certificate will be used. It automatically
generates both a key and a certificate when asked to produce a certificate.
It does not offer OCSP or CRL services. Minica is appropriate, for instance,
for generating certificates for RPC systems or microservices.

On first run, minica will generate a keypair and a root certificate in the
current directory, and will reuse that same keypair and root certificate
unless they are deleted.

On each run, minica will generate a new keypair and sign an end-entity (leaf)
certificate for that keypair. The certificate will contain a list of DNS names
and/or IP addresses from the command line flags. The key and certificate are
placed in a new directory whose name is chosen as the first domain name from
the certificate, or the first IP address if no domain names are present. It
will not overwrite existing keys or certificates.

## Installation

First, install the [Go tools](https://golang.org/dl/) and set up your `$GOPATH`.
Then, run:

`go get github.com/jsha/minica`

## Example usage

```
# Generate a root key and cert in minica-key.pem, and minica.pem, then
# generate and sign an end-entity key and cert, storing them in ./foo.com/
$ minica --domains foo.com
```

## Building

Run `make help` to get info on builds. The general process for building on your own is just to run `make`. 

```
$ make help
AUTHORS                        Generate the AUTHORS file from the git log
all                            Runs a clean, build, fmt, lint, test, staticcheck, vet and install
build                          Builds a dynamic executable or package (default target)
clean                          Cleanup any build binaries or packages
cover                          Runs go test with coverage
fmt                            Verifies all files have been `gofmt`ed
install                        Installs the executable or package
lint                           Verifies `golint` passes
release                        Build cross-compiled binaries for target architectures
static                         Builds a static executable
staticcheck                    Verifies `staticcheck` passes
tag                            Create a new git tag to prepare to build a release
test                           Runs the go tests
version-bump-major             Increment the major version number in VERSION.txt, e.g. v1.2.3 -> v2.2.3
version-bump-minor             Increment the minor version number in VERSION.txt, e.g. v1.2.3 -> v1.3.3
version-bump-patch             Increment the patch version number in VERSION.txt, e.g. v1.2.3 -> v1.2.4
vet                            Verifies `go vet` passes
```

### Releases


If you're maintaining your own releases you'll need to update `.travis.yml` with your `api_key` (see travis-ci docs, you'll need the travis CLI and you'll want to run `travis setup releases`). Then use the following process for a patch-level version update:

```
make version-bump-patch # this increments the appropriate number in VERSION.txt
# git add and push your changes to the branch
# merge the branch to master
git checkout master
make tag
# you will need to unlock your pgp keychain for signing purposes.
# run the "git push" command that is printed by make
```

At this point a tagged version release will be created on gitlab and travis-ci will automatically push the build artifacts to github.