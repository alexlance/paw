paw: access symmetrically encrypted gpg secrets over ssh
========================================================

> *"because one day I will lose my gpg keys"*

Symmetric vs asymetric encryption
---------------------------------

 - **Symmetric** relies on you storing a passphrase (and anyone with that passphrase
   can decrypt your secrets!)

 - **Asymetric** relies on you holding onto key files in ~/.gnupg, storing the
   related passphrase somewhere that unlocks those key files, and updating the
   keys whenever they expire. Someone would need to steal both your key files and
   obtain your passphrase to steal your secrets. Asymetric is obviously the
   "tougher" mechanism to encrypt credentials/secrets.

**The `paw` tool exists because I am more confident in my ability to retain a
simple password (i.e. symmetric encryption!) over a time span of 20 or 40
years.**

Storing and backing up `~/.gnupg` folders securely is going to result in
some really poor security hygiene (encraption etc). Also, having a different
`~/.gnupg` folder set up on every machine that I work on is going to cause
problems long term - even short-term given laptop re-formats and simply
upgrading / breakage etc.
