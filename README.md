# Use 2fa and Pub-key on a Linode

**Use this guide after you have secured your server to only allow pub-key authentication.  Password auth and root login should be disabled.**

First install Google Authenticator.

`sudo apt install libpam-google-authenticator`

Generate a key in your users home directory with the following command.

`google-authenticator`

Answer yes to the questions and scan the QR code.

### Make the following changes to `/etc/ssh/sshd_config`

Add this to the end of the file to allow 2fa input and public key authentication.

`AuthenticationMethods publickey,keyboard-interactive`

Chan values to yes for the following:

```
UsePAM yes

ChallengeResponseAuthentication yes
```

Restart SSH

`sudo systemctl restart ssh`	

### Edit `/etc/pam.d/sshd`

Comment out `#` the following line

`@include common-auth`

Add the following two lines to the bottom of the file

```
#One-time password authentication via Google Authenticator
auth required pam_google_authenticator.so
```
### View Key

Use the following command from the users home directory to show the secret key so that you may add another device manaually.

`head -n 1 .google_authenticator`

More info:  https://github.com/google/google-authenticator-libpam
