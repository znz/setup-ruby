# use-ruby

This action downloads a prebuilt ruby and adds it to `$PATH`.

It currently supports the latest stable versions of MRI, JRuby and TruffleRuby.

See https://github.com/eregon/ruby-install-builder/blob/metadata/versions.json
for the available Ruby versions.

The action works for the `ubuntu-16.04`, `ubuntu-18.04` and `macos-latest` GitHub-hosted runners.  
`windows-latest` is not yet supported.

The prebuilt rubies are generated by https://github.com/eregon/ruby-install-builder.

## Usage

### Single Job

```yaml
name: My workflow
on: [push]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2
    - uses: eregon/use-ruby-action@master
      with:
        ruby-version: ruby-2.6.5
    - run: ruby -v
```

### Matrix

```yaml
name: My workflow
on: [push]
jobs:
  test:
    strategy:
      fail-fast: false
      matrix:
        os: [ 'ubuntu-latest', 'macos-latest' ]
        ruby: [ 'ruby-2.6.5', 'ruby-2.7.0', 'truffleruby-19.3.0', 'jruby-9.2.9.0' ]
    runs-on: ${{ matrix.os }}
    steps:
    - uses: actions/checkout@v2
    - uses: eregon/use-ruby-action@master
      with:
        ruby-version: ${{ matrix.ruby }}
    - run: ruby -v
```

If a specific version is not given, it uses the latest stable release of that implementation.
For instance `truffleruby` is currently the same as `truffleruby-19.3.0`.

If it's just a version number (e.g., `2.6.5`), then MRI is assumed (same as `ruby-2.6.5`).

### All Stable Versions

With that, we can test on all stable releases of MRI, JRuby and TruffleRuby with:

```yaml
name: My workflow
on: [push]
jobs:
  test:
    strategy:
      fail-fast: false
      matrix:
        os: [ 'ubuntu-latest', 'macos-latest' ]
        ruby: [ '2.4.9', '2.5.7', '2.6.5', '2.7.0', 'truffleruby', 'jruby' ]
    runs-on: ${{ matrix.os }}
    steps:
    - uses: actions/checkout@v2
    - uses: eregon/use-ruby-action@master
      with:
        ruby-version: ${{ matrix.ruby }}
    - run: ruby -v
```

## Efficiency

It takes about 5 seconds to setup the given Ruby.

## Limitations

* Currently does not work on Windows since the builder doesn't build on Windows.
  https://github.com/MSP-Greg/actions-ruby is an alternative on Windows.
* This action currently only works with GitHub-hosted runners, not private runners.
