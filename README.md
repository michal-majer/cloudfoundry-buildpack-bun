# Cloud Foundry Bun Buildpack

A Cloud Foundry buildpack for [Bun](https://bun.sh) - the fast all-in-one JavaScript runtime.

## Usage

### With manifest.yml

```yaml
applications:
  - name: my-app
    buildpacks:
      - https://github.com/michal-majer/cloudfoundry-buildpack-bun.git
    command: bun run start
```

### With cf push

```bash
cf push my-app -b https://github.com/michal-majer/cloudfoundry-buildpack-bun.git
```

## Version Pinning

Create a `.bun-version` file in your project root to pin a specific Bun version:

```
1.1.0
```

Or with the `v` prefix:

```
v1.1.0
```

If no `.bun-version` file exists, the latest version will be used.

## Detection

This buildpack will detect your app if any of the following are true:

1. A `bun.lockb` file exists (primary indicator)
2. A `.bun-version` file exists
3. `package.json` contains `"bun"` in the engines field

## Build Process

1. **Supply**: Downloads and installs the Bun binary
2. **Finalize**:
   - Runs `bun install --frozen-lockfile`
   - Runs `bun run build` if a build script exists
   - Runs `bun run heroku-postbuild` if it exists (Heroku compatibility)
   - Prunes devDependencies for production

## Caching

The buildpack caches:
- Bun binaries (for pinned versions)
- `node_modules` directory (based on `bun.lockb` hash)

## Environment Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `NODE_ENV` | `production` | Set automatically |
| `BUN_PRUNE` | `true` | Set to `false` to keep devDependencies |

## Requirements

- Cloud Foundry with `cflinuxfs4` stack (Ubuntu 22.04)
- For older stacks, the baseline Bun build may be needed

## Compatibility

- SAP BTP Cloud Foundry
- Pivotal Cloud Foundry
- Any Cloud Foundry v7+

## License

MIT License - see [LICENSE](LICENSE)

## Contributing

Contributions welcome! Please open an issue or pull request.

## Credits

Inspired by:
- [Heroku Bun Buildpack](https://github.com/jakeg/heroku-buildpack-bun)
- [Cloud Foundry Node.js Buildpack](https://github.com/cloudfoundry/nodejs-buildpack)
