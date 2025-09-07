# Deterministic Builds

SSP Wallet implements deterministic builds to ensure complete transparency and verifiability of browser extension packages. This process guarantees that anyone can rebuild the exact same packages and verify the integrity of submitted browser extensions.

## Overview

Deterministic builds are crucial for security and trust in browser extensions. They ensure that:
- The same source code always produces identical binaries
- No build environment dependencies affect the output
- No hidden code injection is possible
- Complete reproducibility across different machines
- Browser store reviewers can verify submitted packages

## Quick Build Process

For developers and reviewers who want to verify builds:

```bash
# Clone the repository
git clone https://github.com/RunOnFlux/ssp-wallet.git
cd ssp-wallet

# Checkout the exact version being verified
git checkout v1.26.0

# Build deterministically using Docker
npm run build:deterministic

# Verify hashes match the published submission
sha256sum -c SHA256SUMS
```

## Build Environment

The deterministic build system uses:

- **Docker Image:** `node:22.17.1-alpine` (SHA-pinned for consistency)
- **Build Timestamp:** Fixed at `1735689600` (2025-01-01 00:00:00 UTC)
- **Dependencies:** Locked via `yarn.lock` file
- **Environment:** Isolated container with controlled variables
- **Node.js:** Version 22.17.1 (specified in package.json engines)

## Technical Implementation

### Build Script

The build process is automated through `scripts/deterministic-build.sh`:

1. **Version Detection:** Automatically detects version from git tags or package.json
2. **Docker Build:** Uses Docker for complete environment isolation
3. **Package Creation:** Generates both Chrome and Firefox packages
4. **Hash Generation:** Creates SHA256 hashes and unified SHA256SUMS file
5. **Verification:** Outputs ready-to-verify build artifacts

### Docker Configuration

The `Dockerfile` ensures reproducibility through:

```dockerfile
# Fixed base image with SHA pin
FROM node:22.17.1-alpine@sha256:5539840ce9d013fa13e3b9814c9353024be7ac75aca5db6d039504a56c04ea59

# Deterministic environment variables
ENV SOURCE_DATE_EPOCH=1735689600
ENV VITE_BUILD_TIMESTAMP=1735689600
ENV TZ=UTC
ENV LANG=C
ENV LC_ALL=C
```

### Key Features

1. **Timestamp Normalization:** All files get consistent timestamps using `SOURCE_DATE_EPOCH`
2. **Sorted File Lists:** Files are sorted before ZIP creation for consistent archives
3. **Frozen Dependencies:** Uses `yarn install --frozen-lockfile` to ensure exact dependency versions
4. **Multi-Stage Build:** Separates build process from artifact extraction
5. **Security:** Runs as non-root user in container

## Build Outputs

The deterministic build produces:

- `ssp-wallet-chrome-v{version}.zip` - Chrome Web Store package
- `ssp-wallet-firefox-v{version}.zip` - Firefox Add-ons package
- `SHA256SUMS` - Unified hash file for verification
- Individual `.sha256` files for each package

## Package Differences

While both packages contain identical application code, they differ slightly in manifest files:

- **Chrome:** Uses Manifest V3 with `service_worker` background script
- **Firefox:** Uses Manifest V2 with `scripts` array for background scripts
- **Content:** All TypeScript/React application code is identical

## Verification Process

### For Browser Store Reviewers

1. **Clone Repository:** Get the exact tagged version
2. **Build Locally:** Run `npm run build:deterministic`
3. **Compare Hashes:** Verify SHA256 sums match submission
4. **Inspect Contents:** Unzip and review the generated packages

### For Security Auditors

The deterministic build system prevents:
- **Build Environment Attacks:** Docker isolation eliminates host dependencies
- **Supply Chain Issues:** Frozen lockfiles prevent dependency substitution
- **Timestamp Manipulation:** Fixed timestamps prevent metadata-based attacks
- **Non-Reproducible Elements:** All randomness is eliminated from the build

## Integration with Development Workflow

The deterministic build process integrates with the standard development workflow:

```bash
# Standard development build
npm run build

# Development with specific browser target
npm run build:chrome
npm run build:firefox

# Production deterministic build
npm run build:deterministic
```

## Browser Store Compliance

This implementation specifically addresses browser store requirements for:
- **Source Code Verification:** Reviewers can rebuild identical packages
- **Security Transparency:** No hidden build steps or environment dependencies
- **Audit Trail:** Git commit hashes included in SHA256SUMS for traceability
- **Reproducible Security:** Multiple parties can independently verify builds

## Requirements

To run deterministic builds, you need:
- **Docker:** For isolated build environment
- **Git:** For version management and commit tracking
- **Disk Space:** ~2GB for Docker images and build artifacts
- **Memory:** Minimum 8GB RAM recommended for build process

## Troubleshooting

### Common Issues

- **Docker not found:** Install Docker Desktop or Docker Engine
- **Memory errors:** Increase Docker memory limit to 8GB+
- **Permission errors:** Ensure Docker daemon is running and accessible
- **Hash mismatches:** Verify exact git commit and clean working directory

### Build Verification

If hashes don't match:
1. Ensure you're on the exact same git commit
2. Check for uncommitted changes in working directory
3. Verify Docker is using the correct base image
4. Clear Docker cache and rebuild: `docker system prune -a`

This deterministic build system ensures that SSP Wallet maintains the highest standards of transparency and security for browser extension distribution.