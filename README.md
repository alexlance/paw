paw: symmetric encryption over ssh
==================================

Remote network access via ssh + gpg symmetric encryption
--------------------------------------------------------

Secrets should never be stored on laptops or any local devices. Every device
should be treated as though it could be confiscated at the airport, or stolen
away from you on the train.

And there are less exciting reasons not to store secrets on a local device, say
you have to re-format your laptop and you forgot to backup ~/.gnupg, or the device
just stops working due to a dead disk etc.

Symmetric vs asymetric encryption
---------------------------------

 - **Symmetric** relies on you storing a passphrase (and anyone with that passphrase
   can decrypt your secrets)

 - **Asymetric** relies on you holding onto key files in ~/.gnupg, storing the
   related passphrase somewhere that unlocks those key files, and updating the
   keys whenever they expire. Someone would need to steal both your key files and
   obtain your passphrase to steal your secrets. This means asymetric is obviously
   the tougher mechanism to encrypt credentials/secrets. But...

**The `paw` tool exists because I am more confident in my ability to retain a
simple password (i.e. symmetric encryption) over a time span of 20 or 40 years.**

Whereas using asymetric encryption means storing and backing up the `~/.gnupg`
folder securely, I fear it is going to result in some really poor security
hygiene (encraption etc).
