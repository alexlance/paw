paw
===

Symmetric encryption over ssh
-----------------------------

Paw exists because I wanted to have a way to store my every day login
credentials on a remote server only accessible by ssh, instead of locally on a
laptop.

I have chosen to support symmetric encryption which only requires a passphrase
to encrypt/decrypt secrets, instead of using full blown PKI/asymetric crypto
which requires key files and a gnupg configuration.

From a convenience perspective, paw is designed to work with the normal Linux
PRIMARY and CLIPBOARD buffers (using xclip). Having two buffers, the username
gets sent to one (paste it with middle-click) and the password into the other
(paste it with ctrl-v) this makes filling in web forms very easy.

For secrets/credentials that need to exist in environment variables, paw
supports a mode where you can dump out the secrets and assign them to variables
in your current shell. This is useful for e.g. setting up AWS credentials via
environment variables - whilst not leaving keys around on various systems.


Remote network access via ssh + gpg symmetric encryption
--------------------------------------------------------

Benefits to storing secrets on remote servers:

 - Laptops can be confiscated or stolen
 - Local harddrives can die, and need to be backed up
 - Local ~/.gnupg directory needs to be backed up and protected (and the
   backups protected too!)
 - Remote server access can be policed better than local devices
 - Restrict access via traditional ssh server access
 - One secrets store on a server somewhere that you specify, instead of
   multiple secrets stored on all your devices


Symmetric vs asymetric encryption
---------------------------------

 - **Symmetric** relies on you storing a passphrase (and anyone with that
   passphrase can decrypt your secrets)

 - **Asymetric** relies on you holding onto key files in ~/.gnupg, storing the
   related passphrase somewhere that unlocks those key files, and updating the
   keys whenever they expire. Asymetric is obviously the tougher mechanism to
   encrypt secrets. But... gnupg is such a pain in the arse to use!

**The `paw` tool exists because I am more confident in my ability to retain a
simple passphrase (i.e. symmetric encryption) over a time span of 50 years.
Also it makes it far more likely that family members could retrieve my
credentials if need be.**


GPG encryption settings
-----------------------

These appear to be the toughest settings I can find for symmetric encryption,
feel free to create an issue if you find weaknesses or better settings.

```
    --cipher-algo AES256
    --digest-algo sha256
    --cert-digest-algo sha256
    --compress-algo none -z 0
    --s2k-mode 3
    --s2k-digest-algo sha256
    --s2k-count 65011712
    --force-mdc
```


How to use
==========

Interactive session
-------------------

usage: paw HOST:DIR

 * Perform searches or add new secrets
 * Prompt for passphrase only once at the start of a session (default 12 hours)
 * Puts the username into the PRIMARY paste buffer (middle click) and the password
   into the CLIPBOARD buffer (ctrl-v), useful for web forms
 * If apg is installed, will offer a suggested password

Environment variables
---------------------

usage: eval $(echo "searchstring" | paw HOST:DIR USERNAME PASSWORD OTHERSECRET)

  * Search for credentials using a search string via stdin
  * Dumps out export variable assignment statements which can be evaluated to set
    environment variables in your local shell
  * Observes the environment variable "passphrase" if you want to avoid typing the
    passphrase out (useful for scripts)
  * E.g. one could setup AWS by using: AWS\_ACCESS\_KEY\_ID AWS\_SECRET\_ACCESS\_KEY

