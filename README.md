paw: symmetric encryption over ssh
==================================

Paw exists because I wanted to have a way to store credentials on a remote
server, and I wanted to use symmetric "passphrase-only" encryption, because
I didn't believe I could store, backup, manage/rotate gpg keys over the
next 50 years - but I could remember or write down a passphrase.

Also I wanted something that worked with the normal Linux PRIMARY and CLIPBOARD
buffers. Having two buffers, the username could go into one and the password
into the other.

Also I needed a way to populate environment variables with secrets, eg AWS
secret ID and access key.


Remote network access via ssh + gpg symmetric encryption
--------------------------------------------------------

Benefits to storing secrets on remote servers:

 - Laptops can be confiscated or stolen
 - Local harddrives can die, and need to be backed up
 - Local ~/.gnupg directory needs to be backed up and protected (and the backups protected too!)
 - Remote server access can be policed better than local devices
 - Restrict access via traditional ssh server access


Symmetric vs asymetric encryption
---------------------------------

 - **Symmetric** relies on you storing a passphrase (and anyone with that passphrase
   can decrypt your secrets)

 - **Asymetric** relies on you holding onto key files in ~/.gnupg, storing the
   related passphrase somewhere that unlocks those key files, and updating the
   keys whenever they expire. Someone would need to steal both your key files and
   obtain your passphrase to steal your secrets. This means asymetric is obviously
   the tougher mechanism to encrypt secrets. But...

**The `paw` tool exists because I am more confident in my ability to retain a
simple passphrase (i.e. symmetric encryption) over a time span of 50 years.

Also it makes it far more likely that loved ones could retrieve my credentials
if need be.**


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

