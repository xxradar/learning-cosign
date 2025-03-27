# learning-cosign

## Installing cosign
```
brew install sigstore/tap/cosign
```
```
curl -sSL https://github.com/sigstore/cosign/releases/latest/download/cosign-linux-amd64 -o cosign
chmod +x cosign
sudo mv cosign /usr/local/bin/
```
```
cosign version
```

## Keyless example
### Create a sample file `myfile.txt`
```
echo "hello sigstore" > myfile.txt
```
### Create a signature
```
cosign sign xxradar/hackon
```
```
Generating ephemeral keys...
Retrieving signed certificate...

	The sigstore service, hosted by sigstore a Series of LF Projects, LLC, is provided pursuant to the Hosted Project Tools Terms of Use, available at https://lfprojects.org/policies/hosted-project-tools-terms-of-use/.
	Note that if your submission includes personal data associated with this signed artifact, it will be part of an immutable record.
	This may include the email address associated with the account with which you authenticate your contractual Agreement.
	This information will be used for signing this artifact and will be stored in public transparency logs and cannot be removed later, and is subject to the Immutable Record notice at https://lfprojects.org/policies/hosted-project-tools-immutable-records/.

By typing 'y', you attest that (1) you are not submitting the personal data of any other person; and (2) you understand and agree to the statement and the Agreement terms at the URLs listed above.
Are you sure you would like to continue? [y/N] y
Your browser will now be opened to:
https://oauth2.sigstore.dev/auth/auth?access_type=online&client_id=sigstore&code_challenge=2SP7cl-i8woKuFyWFFNw4AMnxIX5QRJgL4L4G_stSOA&code_challenge_method=S256&nonce=2uuiayFKR4LcoxTIC6oIlzpYDgw&redirect_uri=http%3A%2F%2Flocalhost%3A57351%2Fauth%2Fcallback&response_type=code&scope=openid+email&state=2uuiauktK38jV039adedY1BSl9w
Successfully verified SCT...
WARNING: Image reference xxradar/hackon uses a tag, not a digest, to identify the image to sign.
    This can lead you to sign a different image than the intended one. Please use a
    digest (example.com/ubuntu@sha256:abc123...) rather than tag
    (example.com/ubuntu:latest) for the input to cosign. The ability to refer to
    images by tag will be removed in a future release.

tlog entry created with index: 189083879
Pushing signature to: index.docker.io/xxradar/hackon
```
### Verify the file (keyless)
```
cosign verify \
    --certificate-identity philippe.bogaerts@radarsec.com  \
    --certificate-oidc-issuer https://accounts.google.com \
    xxradar/hackon
```
```
Verification for index.docker.io/xxradar/hackon:latest --
The following checks were performed on each of these signatures:
  - The cosign claims were validated
  - Existence of the claims in the transparency log was verified offline
  - The code-signing certificate was verified using trusted certificate authority certificates

[{"critical":{"identity":{"docker-reference":"index.docker.io/xxradar/hackon"},"image":{"docker-manifest-digest":"sha256:19aaac925c3f3d3a9953d8bf0e179bf2961852f1d4e6872b59a8ef979f25838e"},"type":"cosign container image signature"},"optional":{"1.3.6.1.4.1.57264.1.1":"https://accounts.google.com","Bundle":{"SignedEntryTimestamp":"MEUCIQDyJWxqYV6bHoMrocGRXPslHewcVsO3bd1b4e7XdNS6XQIgV19ERhMsIxsgWEIBoum+Axb1dRmWDdOZ6hsoGuD8B2Q=","Payload":{"body":"eyJhcGlWZXJzaW9uIjoiMC4wLjEiLCJraW5kIjoiaGFzaGVkcmVrb3JkIiwic3BlYyI6eyJkYXRhIjp7Imhhc2giOnsiYWxnb3JpdGhtIjoic2hhMjU2IiwidmFsdWUiOiI0YmI1OGJhZmU3Y2EyODE2MWNhNGNiZWVjMDk4NTE2ZTc3MzI3Nzk0YjlkZmFmZDk2Zjg2NGI5NzRlOTUxMDQyIn19LCJzaWduYXR1cmUiOnsiY29udGVudCI6Ik1FWUNJUUMrTklCWXZNcnRwbmZ1QWVtdUxmNFlQMUpyY1Nzbk81RmwrOFg5QnFuWkNnSWhBSTZKTlVDakNZdHdERkJxbU1Kcng0ZzZMT3hTT2c1VjNOWlE5cGxiM2hrZSIsInB1YmxpY0tleSI6eyJjb250ZW50IjoiTFMwdExTMUNSVWRKVGlCRFJWSlVTVVpKUTBGVVJTMHRMUzB0Q2sxSlNVTXhla05EUVd3eVowRjNTVUpCWjBsVlVXMHJZMlZOVmtKdGVUTkdjMWMyV0ZNeWVFNHZTVTQ1VEZkUmQwTm5XVWxMYjFwSmVtb3dSVUYzVFhjS1RucEZWazFDVFVkQk1WVkZRMmhOVFdNeWJHNWpNMUoyWTIxVmRWcEhWakpOVWpSM1NFRlpSRlpSVVVSRmVGWjZZVmRrZW1SSE9YbGFVekZ3WW01U2JBcGpiVEZzV2tkc2FHUkhWWGRJYUdOT1RXcFZkMDE2U1ROTlZHc3hUVVJKTUZkb1kwNU5hbFYzVFhwSk0wMXFRWGROUkVrd1YycEJRVTFHYTNkRmQxbElDa3R2V2tsNmFqQkRRVkZaU1V0dldrbDZhakJFUVZGalJGRm5RVVZ6UW5nNFEyd3ZjMFZSTDJ0MVVYZEZTSGRZTmpVMFIwbzNhMjVxV21WR2JsTnBjalVLT0U0MVNsbGxhRkJ1ZUdaYWMyZFhVV05yYWs5c01FSjBObU5vWnk5WWNqSlhVekZuZGpJeFN6VmFaVUYxVkRkTlRtRlBRMEZZZDNkblowWTBUVUUwUndwQk1WVmtSSGRGUWk5M1VVVkJkMGxJWjBSQlZFSm5UbFpJVTFWRlJFUkJTMEpuWjNKQ1owVkdRbEZqUkVGNlFXUkNaMDVXU0ZFMFJVWm5VVlZYYmxobUNsVXlTVTh4T1hSeGFVWnBTVUpsV21WRWJIVlZURk5yZDBoM1dVUldVakJxUWtKbmQwWnZRVlV6T1ZCd2VqRlphMFZhWWpWeFRtcHdTMFpYYVhocE5Ga0tXa1E0ZDB4QldVUldVakJTUVZGSUwwSkRTWGRKU1VWbFkwZG9jR0pIYkhkalIxVjFXVzA1YmxsWFZubGtTRTVCWTIxR2ExbFlTbnBhVjAxMVdUSTVkQXBOUTJ0SFEybHpSMEZSVVVKbk56aDNRVkZGUlVjeWFEQmtTRUo2VDJrNGRsbFhUbXBpTTFaMVpFaE5kVm95T1haYU1uaHNURzFPZG1KVVFYSkNaMjl5Q2tKblJVVkJXVTh2VFVGRlNVSkNNRTFITW1nd1pFaENlazlwT0haWlYwNXFZak5XZFdSSVRYVmFNamwyV2pKNGJFeHRUblppVkVOQ2FXZFpTMHQzV1VJS1FrRklWMlZSU1VWQloxSTRRa2h2UVdWQlFqSkJUakE1VFVkeVIzaDRSWGxaZUd0bFNFcHNiazUzUzJsVGJEWTBNMnA1ZEM4MFpVdGpiMEYyUzJVMlR3cEJRVUZDYkdScmJqUkVZMEZCUVZGRVFVVmpkMUpSU1doQlRWVllOVFpuZUhwd1lVcDNhelZwTkVsTmJYUnlTVkJEU1Vsc1ZFUXdjbU0zZFRsS01GUjBDbXBLZEdsQmFVRmhTbGhIZEhCTE1XTjZia3RpV2pSVmJETkZVbGw0UlROTlV6bHBOMHhPYTB4cFVVNUhVRWhTYWxoRVFVdENaMmR4YUd0cVQxQlJVVVFLUVhkT2IwRkVRbXhCYWtGdVdIUXdZWGQ1WlU1UlJqRlJhbUZ6V1U5WVdqUnNhM1pFYzJoemNXZG5iVTlEWjNCWUwwZDNkVE56YzNVNFpVcHdaaTlCWWdwcmFtUjZRbGd6ZURoalkwTk5VVU5FVTNaeldWcG5WRmx2YlRZNE1GZ3ZjVGR1UjFOcFJIUndUMDFPVG1aNWRqRmphMGhPUzAxSGVFUmFkREZwV0ZnckNtWnRWWGxIVjBKR0swRnRTa1IwY3owS0xTMHRMUzFGVGtRZ1EwVlNWRWxHU1VOQlZFVXRMUzB0TFFvPSJ9fX19","integratedTime":1743105027,"logIndex":189084140,"logID":"c0d23d6ad406973f9559f3ba2d1ca01f84147d8ffc5b8445c224f98b9591801d"}},"Issuer":"https://accounts.google.com","Subject":"philippe.bogaerts@radarsec.com"}}]
```
