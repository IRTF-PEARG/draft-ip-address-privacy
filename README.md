# Use Cases and Requirements for IP Address Privacy

This is the working area for the individual Internet-Draft, "Use Cases and Requirements for IP Address Privacy".

## Building the Draft

### Setup
See
[the installation instructions](https://github.com/martinthomson/i-d-template/blob/master/doc/SETUP.md). For this I-D, Markdown is the canonical source, so `kramdown-rfc2629` is what I use. This must be installed in addition to the right version of `make` and `xml2rfc` (yay, software) - please check the [SETUP doc](https://github.com/martinthomson/i-d-template/blob/master/doc/SETUP.md) for details. 

Also note that `main` is the default and working branch.

### Build

Once you have the right tooling, formatted text and HTML versions of the draft can be built using `make`.

```sh
$ make
```

## Quick commands

The usual flow is:

```sh
$ make
```

This generates the `.txt` and `.html` outputs. 

To automatically fix lints:
```sh
$ make fix-lint
```

Then do a `git commit`. Note that a git commit will typically fail if the lint check fails.

### Submitting
You have to use git tags to version the doc correctly. If the last tag is `00` then `make submit` will generate `01`. 

When you're ready to cut a new version, do:
```sh
make 
make next # this generates the versioned doc
# Once you're happy with it, upload the XML file: https://datatracker.ietf.org/submit/
# Then, upload the new tag to GitHub
git tag draft-ip-address-privacy-00 # the version you just uploaded to datatracker
git push origin draft-ip-address-privacy-00
```

This is the manual process outlined in the [I-D template docs](https://github.com/martinthomson/i-d-template/blob/main/doc/SUBMITTING.md#manual-process). 


## Contributing

See the
[guidelines for contributions](https://github.com/ShivanKaul/draft-ip-address-privacy/blob/main/CONTRIBUTING.md).
