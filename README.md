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
```
echo "hello sigstore" > myfile.txt
```
```
cosign sign-blob myfile.txt
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
https://oauth2.sigstore.dev/auth/auth?access_type=online&client_id=sigstore&code_challenge=BAeDRalg9soXPVj5YIp9asdVJffumNHr1NrOQb46OAXQ&code_challenge_method=S256&nonce=2uugRoLVhvhJAKQFn4TCLU3bCsE&redirect_uri=http%3A%2F%2Flocalhost%3A57130%2Fauth%2Fcallback&response_type=code&scope=openid+email&state=2uugRo2AttJjJuxOIHI8y52qjua
Successfully verified SCT...
using ephemeral certificate:
-----BEGIN CERTIFICATE-----
MIIC3TCCAmOgAwIBAgIUEQEVMSiFPh4eSo3wPKDaNXoYaCYwCgYIKoZIzj0EAwMw
NzEVMBMGA1UEChMMc2lnc3RvcmUuZGV2MR4wHAYDVQQDExVzaWdzdG9yZS1pbnRl
cm1lZGlhdGUwHhcNMjUwMzI3MTkzMTE0WhcNMjUwMzI3MTk0MTE0WjAAMFkwEwYH
KoZIzj0CAQYIKoZIzj0DAQcDQgAEWHo3A7lYMgWsW6o4/0Cxty+Fe6hp6RdUc664
b9yAOrnF71nSR5cbT2j95LPmGgvgS/k5yah5GasdHIJM8LdglaOCAYIwggF+MA4G
A1UdDwEB/wQEAwIHgDATBgNVHSUEDDAKBggrBgEFBQcDAzAdBgNVHQ4EFgQUoOFo
hEROfG2bdqvjAq39lqb/hd0wHwYDVR0jBBgwFoAU39Ppz1YkEZb5qNjpKFWixi4Y
ZD8wLQYDVR0RAQH/BCMwIYEfcGhpbGlwcGUuYm9nYWVydHNAcmFkYXJoYWNrLmNv
bTAsBgorBgEEAYO/MAEBBB5odHRwczovL2dpdGh1Yi5jb20vbG9naW4vb2F1dGgw
LgYKKwYBBAGDvzABCAQgDB5odHRwczovL2dpdGh1Yi5jb20vbG9naW4vb2F1dGgw
gYkGCisGAQQB1nkCBAIEewR5AHcAdQDdPTBqxscRMmMZHhyZZzcCokpeuN48rf+H
inKALynujgAAAZXZFlaFAAAEAwBGMEQCIHIrPTCODqrxloa7WHsCUDgbNIkqVwWM
xuUv08/FBBq7AiAPPi1W6gw/fM5H9QysaXBdO65HJOq2kwNPALk0qjtNujAKBggq
hkjOPQQDAwNoADBlAjEA5JiG4xNBJlEAxodh5e6FZ4zGgenjrT8prINGS8Yz2Ch/
Iwol/krgIAQL4SoWwjYyAjANu2emyLAmPWktjTV2t1He0bJLOu9DLgT7BtiwDH1z
87zJBcVxhevA/vQkaWw2kpA=
-----END CERTIFICATE-----

tlog entry created with index: 189081059
MEYCIQCi/SlkHgEGS4d7qO3Djm4K1H6pvxe3/bmBBB+D6mW5fAIhAM0HTxuWlifm3yzsGklVLTxjNEiuc+bFSoHahSYUu020
```
