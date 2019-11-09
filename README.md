paw: because one day I will lose my gpg key files
=================================================

Symmetric vs asymetric encryption
---------------------------------

 - **Symmetric** relies on you storing a passphrase; anyone with that passphrase
   can decrypt your secrets.

 - **Asymetric** relies on you holding onto key files in ~/.gnupg, storing the
   related passphrase somewhere that unlocks those key files, and updating the
   keys whenever they expire. Someone would need to steal both your key files and
   obtain your passphrase to steal your secrets.

The `paw` tool exists because given a time span in the order of a few decades,
I am more confident in my ability to retain a simple password. I.e. symmetric
encryption with a sufficiently complex passphrase is good enough and it seems
more likely that I'll still be capable of using this simpler form of crypto in
20 or 40 years.

Also storing and backing up ~/.gnupg folders securely is going to result in
some really poor security hygiene (encraption etc). Also, having a different
~/.gnupg folder setup on every machine that I work on is going to cause
problems.
