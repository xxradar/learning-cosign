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

## Keyless example (docker image)
### Create a signature (keyless) - using Google
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
#### Using Google
```
cosign verify \
    --certificate-identity philippe.bogaerts@radarsec.com  \
    --certificate-oidc-issuer https://accounts.google.com \
    xxradar/hackon
```
#### Using Github
```
cosign verify \
    --certificate-identity philippe.bogaerts@radarhack.com  \
    --certificate-oidc-issuer https://github.com/login/oauth \
    xxradar/hackon
```
```
Verification for index.docker.io/xxradar/hackon:latest --
The following checks were performed on each of these signatures:
  - The cosign claims were validated
  - Existence of the claims in the transparency log was verified offline
  - The code-signing certificate was verified using trusted certificate authority certificates

[{"critical":{"identity":{"docker-reference":"index.docker.io/xxradar/hackon"},"image":{"docker-manifest-digest":"sha256:19aaac925c3f3d3a9953d8bf0e179bf2961852f1d4e6872b59a8ef979f25838e"},"type":"cosign container image signature"},"optional":{"1.3.6.1.4.1.57264.1.1":"https://github.com/login/oauth","Bundle":{"SignedEntryTimestamp":"MEUCIQCFdAuz8rb+y9r99Onb9fZvJtOitQC9Bonb/l9qEASfPAIgHYqjbVCKTab1gGBLs0++da2RXu8QEcsC3NoV6zhLUGM=","Payload":{"body":"eyJhcGlWZXJzaW9uIjoiMC4wLjEiLCJraW5kIjoiaGFzaGVkcmVrb3JkIiwic3BlYyI6eyJkYXRhIjp7Imhhc2giOnsiYWxnb3JpdGhtIjoic2hhMjU2IiwidmFsdWUiOiI0YmI1OGJhZmU3Y2EyODE2MWNhNGNiZWVjMDk4NTE2ZTc3MzI3Nzk0YjlkZmFmZDk2Zjg2NGI5NzRlOTUxMDQyIn19LCJzaWduYXR1cmUiOnsiY29udGVudCI6Ik1FWUNJUUQ5VThSNmMxckZoWE9jRWdiYkE0cmZINjlkVDBZRWROWUk1K3U2NHpQVEpnSWhBS3hIaUVxQUY1T3BWaDBVWkZ1K0FsRHZ6ZFVJVU5qQ2tEM2c2Wkh6YmEzcSIsInB1YmxpY0tleSI6eyJjb250ZW50IjoiTFMwdExTMUNSVWRKVGlCRFJWSlVTVVpKUTBGVVJTMHRMUzB0Q2sxSlNVTTBSRU5EUVcxWFowRjNTVUpCWjBsVlFtMTVjbkIwWWxsMVZFdFZjVlpHYVdremVUVk5iemhFUzNVNGQwTm5XVWxMYjFwSmVtb3dSVUYzVFhjS1RucEZWazFDVFVkQk1WVkZRMmhOVFdNeWJHNWpNMUoyWTIxVmRWcEhWakpOVWpSM1NFRlpSRlpSVVVSRmVGWjZZVmRrZW1SSE9YbGFVekZ3WW01U2JBcGpiVEZzV2tkc2FHUkhWWGRJYUdOT1RXcFZkMDE2U1ROTlZHc3dUbnBSZUZkb1kwNU5hbFYzVFhwSk0wMVVhekZPZWxGNFYycEJRVTFHYTNkRmQxbElDa3R2V2tsNmFqQkRRVkZaU1V0dldrbDZhakJFUVZGalJGRm5RVVZWWjFWNFFWZGFUMUJaVGtaWWJFZDBWWEF4TkdGTFZVRkhNRVUwV1dsQ01tbGhNMnNLWm5aVE5HbGxlVFJKTXpONVdGTkVTVWszWkdsaU4wSnVaVXREWTNCTlNWRjBkRGhLWjJwQlNuZDFSMVp5Tm1sWVVqWlBRMEZaVVhkblowZEJUVUUwUndwQk1WVmtSSGRGUWk5M1VVVkJkMGxJWjBSQlZFSm5UbFpJVTFWRlJFUkJTMEpuWjNKQ1owVkdRbEZqUkVGNlFXUkNaMDVXU0ZFMFJVWm5VVlY0ZUhnd0NsaEdXVEZ0Umt0d01XcEpNRWQzVjFKWVZUTkRSMjl6ZDBoM1dVUldVakJxUWtKbmQwWnZRVlV6T1ZCd2VqRlphMFZhWWpWeFRtcHdTMFpYYVhocE5Ga0tXa1E0ZDB4UldVUldVakJTUVZGSUwwSkRUWGRKV1VWbVkwZG9jR0pIYkhkalIxVjFXVzA1YmxsWFZubGtTRTVCWTIxR2ExbFlTbTlaVjA1eVRHMU9kZ3BpVkVGelFtZHZja0puUlVWQldVOHZUVUZGUWtKQ05XOWtTRkozWTNwdmRrd3laSEJrUjJneFdXazFhbUl5TUhaaVJ6bHVZVmMwZG1JeVJqRmtSMmQzQ2t4bldVdExkMWxDUWtGSFJIWjZRVUpEUVZGblJFSTFiMlJJVW5kamVtOTJUREprY0dSSGFERlphVFZxWWpJd2RtSkhPVzVoVnpSMllqSkdNV1JIWjNjS1oxbHpSME5wYzBkQlVWRkNNVzVyUTBKQlNVVm1VVkkzUVVoclFXUjNSR1JRVkVKeGVITmpVazF0VFZwSWFIbGFXbnBqUTI5cmNHVjFUalE0Y21ZclNBcHBia3RCVEhsdWRXcG5RVUZCV2xoYVNsZFdkRUZCUVVWQmQwSkpUVVZaUTBsUlEwdFlRVEpvVTNneFUwbzBPWG96Wm1WbFVrdHdZMWhUY0ZRd1pTdDJDa1phUW1jM1dWSlFhR3RGVkRSQlNXaEJUM2czWlc4eFdXeFVNMngzUjJnMllsWkpkbFpQYkZoSmRFY3lWVVE1YlM5Uk4xbE9TVVJ6VTFKb2FVMUJiMGNLUTBOeFIxTk5ORGxDUVUxRVFUSnJRVTFIV1VOTlVVUndWRVpWZG5GNlFXSnNNMGswTm5selYwTndNMDFyUTIxWE5tNHlSVVZTTTAxa1p6QlVUeXN5Y1Fwc1lqRjRUVUp0TVhWVGMyZzJabUowU2k5SkswaGhkME5OVVVOU1NFdExlamR0THpOUFpHVTNSamRZVFhwaWVXWkxVMVZCV1ZkUFJHWjZaVUZNVURJdkNrMXhXVWg1Y0cxQmF6QjJPRVpoZDB0cWRVTXpNRUoyWnpWblFUMEtMUzB0TFMxRlRrUWdRMFZTVkVsR1NVTkJWRVV0TFMwdExRbz0ifX19fQ==","integratedTime":1743104864,"logIndex":189083675,"logID":"c0d23d6ad406973f9559f3ba2d1ca01f84147d8ffc5b8445c224f98b9591801d"}},"Issuer":"https://github.com/login/oauth","Subject":"philippe.bogaerts@radarhack.com"}},{"critical":{"identity":{"docker-reference":"index.docker.io/xxradar/hackon"},"image":{"docker-manifest-digest":"sha256:19aaac925c3f3d3a9953d8bf0e179bf2961852f1d4e6872b59a8ef979f25838e"},"type":"cosign container image signature"},"optional":{"1.3.6.1.4.1.57264.1.1":"https://github.com/login/oauth","Bundle":{"SignedEntryTimestamp":"MEUCIQCEd31I719xXbW7/p8/+8Zfvkll9FVFEuBcSPX584cM2gIgNbt6LcA61emMtSBHddQLTxoH1QdaHwIdz8JQCxuTCZE=","Payload":{"body":"eyJhcGlWZXJzaW9uIjoiMC4wLjEiLCJraW5kIjoiaGFzaGVkcmVrb3JkIiwic3BlYyI6eyJkYXRhIjp7Imhhc2giOnsiYWxnb3JpdGhtIjoic2hhMjU2IiwidmFsdWUiOiI0YmI1OGJhZmU3Y2EyODE2MWNhNGNiZWVjMDk4NTE2ZTc3MzI3Nzk0YjlkZmFmZDk2Zjg2NGI5NzRlOTUxMDQyIn19LCJzaWduYXR1cmUiOnsiY29udGVudCI6Ik1FVUNJUURTQXdlMFRqa3pLZ0FyL1dyaHd0SEVqWE1NeHIzVFp1a1o1MTNEZEdnQ1pnSWdIU284VUdDNGFaTURZbFlCNHhCbEt2SG4zd2ZxcGgvcUJScWdIZFIyOUk0PSIsInB1YmxpY0tleSI6eyJjb250ZW50IjoiTFMwdExTMUNSVWRKVGlCRFJWSlVTVVpKUTBGVVJTMHRMUzB0Q2sxSlNVTXpWRU5EUVcxVFowRjNTVUpCWjBsVlFXWkNNVzk0V0dGRk5WbEVRME5XWkN0bGNVTXlhbVpIVDBoM2QwTm5XVWxMYjFwSmVtb3dSVUYzVFhjS1RucEZWazFDVFVkQk1WVkZRMmhOVFdNeWJHNWpNMUoyWTIxVmRWcEhWakpOVWpSM1NFRlpSRlpSVVVSRmVGWjZZVmRrZW1SSE9YbGFVekZ3WW01U2JBcGpiVEZzV2tkc2FHUkhWWGRJYUdOT1RXcFZkMDE2U1ROTlZHc3dUMFJWZUZkb1kwNU5hbFYzVFhwSk0wMVVhekZQUkZWNFYycEJRVTFHYTNkRmQxbElDa3R2V2tsNmFqQkRRVkZaU1V0dldrbDZhakJFUVZGalJGRm5RVVZMTkZaRFdXMW5jR2d5VldWWGJIbFhPSEoxYlRaa2JGTkZjVlJTZFVWb2RXeGxaMmdLYm1sVk1VZDBXRWRRWW1WaGJIZGFNWGhqUzJKdmFpdHFValpWYVVVcll6ZHJWMXAyT0VGclpYZzJja00yY1hoTVIwdFBRMEZaVFhkblowWXZUVUUwUndwQk1WVmtSSGRGUWk5M1VVVkJkMGxJWjBSQlZFSm5UbFpJVTFWRlJFUkJTMEpuWjNKQ1owVkdRbEZqUkVGNlFXUkNaMDVXU0ZFMFJVWm5VVlZFVDNsa0NsbFZMelJ2VXpGT1ZURmFRa1JtY1VjcmVHOVNNRTFqZDBoM1dVUldVakJxUWtKbmQwWnZRVlV6T1ZCd2VqRlphMFZhWWpWeFRtcHdTMFpYYVhocE5Ga0tXa1E0ZDB4UldVUldVakJTUVZGSUwwSkRUWGRKV1VWbVkwZG9jR0pIYkhkalIxVjFXVzA1YmxsWFZubGtTRTVCWTIxR2ExbFlTbTlaVjA1eVRHMU9kZ3BpVkVGelFtZHZja0puUlVWQldVOHZUVUZGUWtKQ05XOWtTRkozWTNwdmRrd3laSEJrUjJneFdXazFhbUl5TUhaaVJ6bHVZVmMwZG1JeVJqRmtSMmQzQ2t4bldVdExkMWxDUWtGSFJIWjZRVUpEUVZGblJFSTFiMlJJVW5kamVtOTJUREprY0dSSGFERlphVFZxWWpJd2RtSkhPVzVoVnpSMllqSkdNV1JIWjNjS1oxbHZSME5wYzBkQlVWRkNNVzVyUTBKQlNVVm1RVkkyUVVoblFXUm5SR1JRVkVKeGVITmpVazF0VFZwSWFIbGFXbnBqUTI5cmNHVjFUalE0Y21ZclNBcHBia3RCVEhsdWRXcG5RVUZCV2xoYVNtNWhLMEZCUVVWQmQwSklUVVZWUTBsUlEwcFFSVlozVnpKNWEzaHlkMlZxT0ZGa2IxazVSbkp3VkU1VFVHWnhDa3QzY2s0eU4xaDNja0ZxVmpsQlNXZFpTbWczTHpSVFZEWmpaMDFaV1VsSE5qTnFUVmt2U2tKd1VVZDRXRzEwTHpONlZFOXZZWFZXVEU0d2QwTm5XVWtLUzI5YVNYcHFNRVZCZDAxRVduZEJkMXBCU1hkSVduUnJSR1ZCTjBJeWRDdFJRMnRFZFhoWGJqTldRbU5wVTA1cVVtdFpLMmhtUjJkRE1XSlRkWEJUYVFwRGFTdG9UbGRMVmtkS2JHMVNWbmhyZVhoSlRrRnFRbE00T1c1YVVGVmxVWEpaVGxCMFQxTm5aRWRKZDBKTU9GZG5ha3R6YW1OUk1HOXVSR3RoZFdSMUNubzBibEp0Y25saWJsVkpZbFJwU0VWNGVqUk5lbW93UFFvdExTMHRMVVZPUkNCRFJWSlVTVVpKUTBGVVJTMHRMUzB0Q2c9PSJ9fX19","integratedTime":1743104934,"logIndex":189083879,"logID":"c0d23d6ad406973f9559f3ba2d1ca01f84147d8ffc5b8445c224f98b9591801d"}},"Issuer":"https://github.com/login/oauth","Subject":"philippe.bogaerts@radarhack.com"}},{"critical":{"identity":{"docker-reference":"index.docker.io/xxradar/hackon"},"image":{"docker-manifest-digest":"sha256:19aaac925c3f3d3a9953d8bf0e179bf2961852f1d4e6872b59a8ef979f25838e"},"type":"cosign container image signature"},"optional":{"1.3.6.1.4.1.57264.1.1":"https://github.com/login/oauth","Bundle":{"SignedEntryTimestamp":"MEYCIQD43zQr5E3zEwyrITwpkvN4W+asI9YclFY31yTCyuvvSAIhAO3N9sX5oufenx9v9bR/bwRzjA+6+5iax8EUMYAZlYC7","Payload":{"body":"eyJhcGlWZXJzaW9uIjoiMC4wLjEiLCJraW5kIjoiaGFzaGVkcmVrb3JkIiwic3BlYyI6eyJkYXRhIjp7Imhhc2giOnsiYWxnb3JpdGhtIjoic2hhMjU2IiwidmFsdWUiOiI0YmI1OGJhZmU3Y2EyODE2MWNhNGNiZWVjMDk4NTE2ZTc3MzI3Nzk0YjlkZmFmZDk2Zjg2NGI5NzRlOTUxMDQyIn19LCJzaWduYXR1cmUiOnsiY29udGVudCI6Ik1FWUNJUURYMGl6cU9LNkFHYmd0bzE2c1VEc2RGV2JLa2txdTZ4NG1BYW16dERtTk1nSWhBUElBd0JHM2JrS05jRzJPZmJjQWhCMzdHMVhhSExUMllucG1RaTNxYzBqNSIsInB1YmxpY0tleSI6eyJjb250ZW50IjoiTFMwdExTMUNSVWRKVGlCRFJWSlVTVVpKUTBGVVJTMHRMUzB0Q2sxSlNVTXpha05EUVcxVFowRjNTVUpCWjBsVlQyY3Zja2hNYkVZelJuQkRPVXBFZFVkbVFtVlVRbWh4UVZGQmQwTm5XVWxMYjFwSmVtb3dSVUYzVFhjS1RucEZWazFDVFVkQk1WVkZRMmhOVFdNeWJHNWpNMUoyWTIxVmRWcEhWakpOVWpSM1NFRlpSRlpSVVVSRmVGWjZZVmRrZW1SSE9YbGFVekZ3WW01U2JBcGpiVEZzV2tkc2FHUkhWWGRJYUdOT1RXcFZkMDE2U1ROTlZHc3hUbXBWZWxkb1kwNU5hbFYzVFhwSk0wMXFRWGRPYWxWNlYycEJRVTFHYTNkRmQxbElDa3R2V2tsNmFqQkRRVkZaU1V0dldrbDZhakJFUVZGalJGRm5RVVZtWjBkTVZEbHdZbXBHY21wVGVHSlJhMVJhWkVGdE5ubEhjREp0WkhjMWVXdFFTM2tLUmpnNU5qbEpOMDA1Y0RVNWF5OVVSRFZaYW5rMk9GWTBVR0pWV2xsQ05HODRPV2hOUW5CWlpWQTROVkZ4ZDNRclVFdFBRMEZaVFhkblowWXZUVUUwUndwQk1WVmtSSGRGUWk5M1VVVkJkMGxJWjBSQlZFSm5UbFpJVTFWRlJFUkJTMEpuWjNKQ1owVkdRbEZqUkVGNlFXUkNaMDVXU0ZFMFJVWm5VVlUzZFZkQkNtOXFjVEJQZVVsNVRWUkRRbmhTVURObE5sUm5NaTlWZDBoM1dVUldVakJxUWtKbmQwWnZRVlV6T1ZCd2VqRlphMFZhWWpWeFRtcHdTMFpYYVhocE5Ga0tXa1E0ZDB4UldVUldVakJTUVZGSUwwSkRUWGRKV1VWbVkwZG9jR0pIYkhkalIxVjFXVzA1YmxsWFZubGtTRTVCWTIxR2ExbFlTbTlaVjA1eVRHMU9kZ3BpVkVGelFtZHZja0puUlVWQldVOHZUVUZGUWtKQ05XOWtTRkozWTNwdmRrd3laSEJrUjJneFdXazFhbUl5TUhaaVJ6bHVZVmMwZG1JeVJqRmtSMmQzQ2t4bldVdExkMWxDUWtGSFJIWjZRVUpEUVZGblJFSTFiMlJJVW5kamVtOTJUREprY0dSSGFERlphVFZxWWpJd2RtSkhPVzVoVnpSMllqSkdNV1JIWjNjS1oxbHZSME5wYzBkQlVWRkNNVzVyUTBKQlNVVm1RVkkyUVVoblFXUm5SR1JRVkVKeGVITmpVazF0VFZwSWFIbGFXbnBqUTI5cmNHVjFUalE0Y21ZclNBcHBia3RCVEhsdWRXcG5RVUZCV2xoYVRHUk1la0ZCUVVWQmQwSklUVVZWUTBsRWFVVlRSRXhSYVV4V1lrOXZMMDQzSzBkNlpFWXdlRlpOVVRSSGVqQnRDbUp5VUN0SldrZGtPVGhLTjBGcFJVRnpURXBJVGt0MlVYVkpTMWc1U0ZaUFpUSllURTVqVXl0RlRFRnhhV1pxT1UweVJGWktjbFEzVGxwTmQwTm5XVWtLUzI5YVNYcHFNRVZCZDAxRVlVRkJkMXBSU1hoQlRVWjBkbHByTUc1RGRFaGpWRFZ0YVN0aFFsZHdTak5OUjAwd1dEWXlkakpuU1VveWRIZHNWWFpOUmdwR1ZtWlVReTlXY1N0TGRrNXlWMjVxTURKcFVGRlJTWGREUkdZeFYwMVNXSFk1ZGpWMWRWWm1hSGcwV21Kb1dUQnNiVlJTVnl0T2FHVmplR1ZOVDAwMUNrTlJkbVF3Um5oWFNqRnJNR296ZG5KbFZucE1ORXBTUndvdExTMHRMVVZPUkNCRFJWSlVTVVpKUTBGVVJTMHRMUzB0Q2c9PSJ9fX19","integratedTime":1743105417,"logIndex":189085156,"logID":"c0d23d6ad406973f9559f3ba2d1ca01f84147d8ffc5b8445c224f98b9591801d"}},"Issuer":"https://github.com/login/oauth","Subject":"philippe.bogaerts@radarhack.com"}}]
```
## Keyless example (blob)

### Create a sample file `myfile.txt`
```
echo "hello sigstore" > myfile.txt
```
```
cosign sign-blob myfile.txt --bundle cosign.bundle
```
```
Using payload from: myfile.txt
Generating ephemeral keys...
Retrieving signed certificate...

	The sigstore service, hosted by sigstore a Series of LF Projects, LLC, is provided pursuant to the Hosted Project Tools Terms of Use, available at https://lfprojects.org/policies/hosted-project-tools-terms-of-use/.
	Note that if your submission includes personal data associated with this signed artifact, it will be part of an immutable record.
	This may include the email address associated with the account with which you authenticate your contractual Agreement.
	This information will be used for signing this artifact and will be stored in public transparency logs and cannot be removed later, and is subject to the Immutable Record notice at https://lfprojects.org/policies/hosted-project-tools-immutable-records/.

By typing 'y', you attest that (1) you are not submitting the personal data of any other person; and (2) you understand and agree to the statement and the Agreement terms at the URLs listed above.
Are you sure you would like to continue? [y/N] y
Your browser will now be opened to:
https://oauth2.sigstore.dev/auth/auth?access_type=online&client_id=sigstore&code_challenge=Upj4BvPs-T0vZBGEKcTTcEgygLUR9vGfSL0TyphPRGI&code_challenge_method=S256&nonce=2uuk9AECj1ULplSJO7MucMaQxZN&redirect_uri=http%3A%2F%2Flocalhost%3A57623%2Fauth%2Fcallback&response_type=code&scope=openid+email&state=2uuk9Dki00RJ8F5abV1LzqNm3sQ
Successfully verified SCT...
using ephemeral certificate:
-----BEGIN CERTIFICATE-----
MIIC3jCCAmSgAwIBAgIUcgePT6FWUmQCxvUXjEpfModlwUkwCgYIKoZIzj0EAwMw
NzEVMBMGA1UEChMMc2lnc3RvcmUuZGV2MR4wHAYDVQQDExVzaWdzdG9yZS1pbnRl
cm1lZGlhdGUwHhcNMjUwMzI3MjAwMTM3WhcNMjUwMzI3MjAxMTM3WjAAMFkwEwYH
KoZIzj0CAQYIKoZIzj0DAQcDQgAEa/nqn/p2dmQHEYd4Tj8Ek/OSb5yrYiTJ9yhZ
u+ffHMO95Fcb4fZbZheFep+pkizmF4d98nyZA41Sza4TYCbES6OCAYMwggF/MA4G
A1UdDwEB/wQEAwIHgDATBgNVHSUEDDAKBggrBgEFBQcDAzAdBgNVHQ4EFgQU+18R
18Zsc/M/6AjrMkezJK6LJcEwHwYDVR0jBBgwFoAU39Ppz1YkEZb5qNjpKFWixi4Y
ZD8wLQYDVR0RAQH/BCMwIYEfcGhpbGlwcGUuYm9nYWVydHNAcmFkYXJoYWNrLmNv
bTAsBgorBgEEAYO/MAEBBB5odHRwczovL2dpdGh1Yi5jb20vbG9naW4vb2F1dGgw
LgYKKwYBBAGDvzABCAQgDB5odHRwczovL2dpdGh1Yi5jb20vbG9naW4vb2F1dGgw
gYoGCisGAQQB1nkCBAIEfAR6AHgAdgDdPTBqxscRMmMZHhyZZzcCokpeuN48rf+H
inKALynujgAAAZXZMigsAAAEAwBHMEUCIHVW7lWLjviWzG8YYJsU34E7PTYn22Vd
Gz5UXB6DapapAiEAtKplv8dcuwtOMcvL2LtQygKxMi882x84oZyHdnLb9vUwCgYI
KoZIzj0EAwMDaAAwZQIwcJoUBr8qybiQ+O64OVk3YFlgKYVLhB4pdYGEa9ZJWmzY
k8G3UL802Wsym4UFcoRdAjEAi/KCcBzosvIcvFWXlrNZAxsNTUbRcHrLdyZJhTME
KmtvUUp1gHJxCI6PWvKNR7vC
-----END CERTIFICATE-----

tlog entry created with index: 189085851
using ephemeral certificate:
-----BEGIN CERTIFICATE-----
MIIC3jCCAmSgAwIBAgIUcgePT6FWUmQCxvUXjEpfModlwUkwCgYIKoZIzj0EAwMw
NzEVMBMGA1UEChMMc2lnc3RvcmUuZGV2MR4wHAYDVQQDExVzaWdzdG9yZS1pbnRl
cm1lZGlhdGUwHhcNMjUwMzI3MjAwMTM3WhcNMjUwMzI3MjAxMTM3WjAAMFkwEwYH
KoZIzj0CAQYIKoZIzj0DAQcDQgAEa/nqn/p2dmQHEYd4Tj8Ek/OSb5yrYiTJ9yhZ
u+ffHMO95Fcb4fZbZheFep+pkizmF4d98nyZA41Sza4TYCbES6OCAYMwggF/MA4G
A1UdDwEB/wQEAwIHgDATBgNVHSUEDDAKBggrBgEFBQcDAzAdBgNVHQ4EFgQU+18R
18Zsc/M/6AjrMkezJK6LJcEwHwYDVR0jBBgwFoAU39Ppz1YkEZb5qNjpKFWixi4Y
ZD8wLQYDVR0RAQH/BCMwIYEfcGhpbGlwcGUuYm9nYWVydHNAcmFkYXJoYWNrLmNv
bTAsBgorBgEEAYO/MAEBBB5odHRwczovL2dpdGh1Yi5jb20vbG9naW4vb2F1dGgw
LgYKKwYBBAGDvzABCAQgDB5odHRwczovL2dpdGh1Yi5jb20vbG9naW4vb2F1dGgw
gYoGCisGAQQB1nkCBAIEfAR6AHgAdgDdPTBqxscRMmMZHhyZZzcCokpeuN48rf+H
inKALynujgAAAZXZMigsAAAEAwBHMEUCIHVW7lWLjviWzG8YYJsU34E7PTYn22Vd
Gz5UXB6DapapAiEAtKplv8dcuwtOMcvL2LtQygKxMi882x84oZyHdnLb9vUwCgYI
KoZIzj0EAwMDaAAwZQIwcJoUBr8qybiQ+O64OVk3YFlgKYVLhB4pdYGEa9ZJWmzY
k8G3UL802Wsym4UFcoRdAjEAi/KCcBzosvIcvFWXlrNZAxsNTUbRcHrLdyZJhTME
KmtvUUp1gHJxCI6PWvKNR7vC
-----END CERTIFICATE-----

Wrote bundle to file cosign.bundle
MEUCIERiPK/EKBdrdt9DkjWps89lQFsP6hsckvNubEbjI7rlAiEA7af/uA/p3UXndc8qF8gXpnOa3bvyWXPdBsaPz2t8g9s=
```
### Inspect `cosign.bundle`
```
ls
```
```
cosign.bundle	myfile.txt
```
### Verify the file (blob)
```
cosign verify-blob myfile.txt \
    --bundle cosign.bundle \
    --certificate-identity philippe.bogaerts@radarhack.com  \
    --certificate-oidc-issuer https://github.com/login/oauth
```
