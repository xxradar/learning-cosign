# SBOM
## Creating an SBOM for a container image
```
trivy image --format spdx --output result.spdx xxradar/hackon:latest
```
## Attaching the SBOM
```
cosign attach sbom --sbom result.spdx xxradar/hackon:latest
```
```
WARNING: SBOM attachments are deprecated and support will be removed in a Cosign release soon after 2024-02-22 (see https://github.com/sigstore/cosign/issues/2755). Instead, please use SBOM attestations.
WARNING: Attaching SBOMs this way does not sign them. To sign them, use 'cosign attest --predicate result.spdx --key <key path>'.
Uploading SBOM file for [index.docker.io/xxradar/hackon:latest] to [index.docker.io/xxradar/hackon:sha256-19aaac925c3f3d3a9953d8bf0e179bf2961852f1d4e6872b59a8ef979f25838e.sbom] with mediaType [text/spdx].
```
## Signing the SBOM
```
cosign generate-key-pair
```
```
Enter password for private key:
Enter password for private key again:
Private key written to cosign.key
Public key written to cosign.pub
```
```
cosign sign --key cosign.key xxradar/hackon:sha256-19aaac925c3f3d3a9953d8bf0e179bf2961852f1d4e6872b59a8ef979f25838e.sbom
```
```
cosign verify --key cosign.pub xxradar/hackon:sha256-19aaac925c3f3d3a9953d8bf0e179bf2961852f1d4e6872b59a8ef979f25838e.sbom
```
or 
```
cosign attest -predicate result.spdx -key cosign.key xxradar/hackon:latest
```
## Storing the public key in Docker registry
```
oras push registry-1.docker.io/xxradar/hackon:cosign-public-key \
  --artifact-type application/vnd.cncf.notary.cosign-public-key \
  cosign.pub:application/vnd.cncf.notary.cosign-public-key
```
## Pulling the public key from the Docker registry

```
oras pull registry-1.docker.io/xxradar/hackon:cosign-public-key
```
