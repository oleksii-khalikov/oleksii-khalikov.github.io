---
title: "hugo list"
slug: hugo_list
url: /commands/hugo_list/
---

## hugo list

Listing out various types of content

### Synopsis

Listing out various types of content.

List requires a subcommand, e.g. hugo list drafts

### Options

```
  -h, --help   help for list
```

### Options inherited from parent commands

```
      --clock string               set the clock used by Hugo, e.g. --clock 2021-11-06T22:30:00.00+09:00
      --config string              config file (default is hugo.yaml|json|toml)
      --configDir string           config dir (default "config")
      --debug                      debug output
  -d, --destination string         filesystem path to write files to
  -e, --environment string         build environment
      --ignoreVendorPaths string   ignores any _vendor for module paths matching the given Glob pattern
      --logLevel string            log level (debug|info|warn|error)
      --quiet                      build in quiet mode
      --renderToMemory             render to memory (mostly useful when running the server)
  -s, --source string              filesystem path to read files relative from
      --themesDir string           filesystem path to themes directory
  -v, --verbose                    verbose output
```

### SEE ALSO

* [hugo](../hugo.md)     - hugo builds your site
* [hugo list all](list/all)     - List all posts
* [hugo list drafts](list/drafts)     - List all drafts
* [hugo list expired](list/expired)     - List all posts already expired
* [hugo list future](list/future)     - List all posts dated in the future

