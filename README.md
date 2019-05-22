# ipfs-npm-republish

Republish your node project's dependencies to IPFS as a micro-registry

## How it works:

1. List dependencies for current directory from package-lock.json

2. Calculate list of packages to be republished

3. create an folder to act as ROOT

4. For each package

  1. Fetch packuments for each package and write to ROOT

  2. For each depended upon version:

    1. download the tarball to ROOT

    2. ipfs add tarball

    3. rewrite the dist.tarball url to a local gateway url with tarball hash

5. ipfs add -r ROOT

6. pin ROOT hash

7. set per-project npm config to use new micro-registry

8. output command to update registry to point to ipfs ROOT hash

## TODOS

- respect .npmrc for registry configs
- Publish ROOT to ipns (optional due to speed issues)
- Don't upload private modules
- support git dependencies
- check that IPFS is running locally
- check that you have a package-lock.json

## Making it work offline

- Use locally cached tarballs (`.npm/_cacache/content-v2` integrity hash split on `-` then base64 decoded to hex and sha512/12/34/56789...)
- Use locally cached packuments (`.npm/_cacache/index-v5` which then points to `.npm/_cacache/content-v2`, not sure what the cache key for requests is yet)