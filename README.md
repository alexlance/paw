paw: Password Manager for Linux
===============================

Symmetric encryption over ssh
-----------------------------

Paw exists because I wanted to have a way to store my every day login
credentials as gpg encrypted files that live on a remote server only accessible
by ssh.

Paw supports two modes: an interactive mode that lets you search and populate
your two Linux paste buffers. And also a non-interactive mode that prints out
the secrets that can then be used to (eg) populate environment variables.

When used interactively, paw sends the username to the PRIMARY buffer (paste it
with middle-click) and the password to the CLIPBOARD buffer (paste it with
ctrl-v) this makes filling in web forms very easy.


Remote network access via ssh + gpg symmetric encryption
--------------------------------------------------------

Why store secrets on a remote server?

 - Laptops can be confiscated or stolen
 - Local harddrives die, servers get redundency
 - Local ~/.gnupg directory would need to be backed up and protected (and the
   backups protected too!)
 - Remote server access can be policed better than local devices
 - Restrict access via normal ssh server access
 - One secrets collection on a single server, instead of multiple secrets stored on
   all your devices


Symmetric vs asymetric encryption
---------------------------------

 - **Symmetric** relies on you storing a password (and anyone with that
   password can decrypt your secrets)

 - **Asymetric** relies on you holding onto key files in ~/.gnupg, storing the
   related passphrase somewhere that unlocks those key files, and updating the
   keys whenever they expire. The keys exist as files that you need to either
   store on various devices, or have to exist on a secure key/card that you
   carry around with you.

Asymetric is obviously the tougher mechanism to encrypt secrets. But ...

**The `paw` tool exists because I am more confident in my ability to retain a
simple password (i.e. symmetric encryption) over a time span of 50 years, than
I am at storing keys and remembering gpg commands to work with those keys.**

**Also it makes it far more likely that family members could retrieve my
password encrypted data if need be.**


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

```
    # usage
    paw HOST:DIR

    # example
    paw alex@example.com:passwords
```

 * Perform searches or add new secrets on the remote host
 * Prompt for password only once at the start of a session (default 12 hours)
 * Puts the username into the PRIMARY paste buffer (middle click) and the password
   into the CLIPBOARD buffer (ctrl-v), useful for web forms
 * If apg is installed, will offer a suggested password

Non-interactive
---------------

```
    # example in ~/.bashrc
    my-aws-shell() {
      local output="$(echo aws-account-123 | paw example.com:/path/to/passwords)"
      export AWS_ACCESS_KEY_ID="$(sed '1q;d' <<< ${output})"
      export AWS_SECRET_ACCESS_KEY="$(sed '2q;d' <<< ${output})"
    }
```

  * Search for credentials using a search string via stdin
  * Dumps out the username, password, and other secret, one on each line
  * Use some sed magic to grab a particular line from the output
  * Observes the environment variable "PAW_PASSWORD" if you want to avoid typing the
    password out (useful for scripts)


Other info
==========

  * Secrets may not contain newlines, so this is only good for one line secrets

  * The data storage format stores this information, eg:

      - Username: alexlance
      - Password: abcd1234
      - Other secret: the security question was about mum's maiden name - I answered "bloop"
      - Keywords: github

  * When you're searching for a secret you can search by keyword (eg "github"
    in the example above) or you can also search by the filename that the info
    is stored in.

  * The searchable keywords are stored in the gpg "comment" field. This field is
    stored inside the encrypted file, but the comment itself is not encrypted.
    This means we can grep through all the secrets very quickly, and only decrypt
    the one file that we care about.

