# Bash Functions for LastPass

Basic configuration and use. (or read the [longer version](blogpost.md))

## Configuration

1. **Install `lpass`**, the LastPass command line interface. See installation instructions in its [README file](https://github.com/lastpass/lastpass-cli/blob/master/README.md) for Mac and Linux. For Windows, read the discussion on [this feature request](https://github.com/lastpass/lastpass-cli/issues/83) for a variety of options.

2. In LastPass:

    * Create an **new folder called API_KEYS**
    * Create a new **secure note template with a field called `Key`**. It's pretty simple to do, but LastPass provides [instructions for creating note templates](https://blog.lastpass.com/2016/07/diy-with-our-custom-secure-note-templates.html/) if you need more help. (I took advantage of the consistency offered by custom note templates and use a template called "API Keys".)

3. Store your secrets in this new LastPass `API_KEYS` folder using this template.

4. Add this to your `.bashrc` or `.bash_profile`:

        # Use the email address associated with your LastPass account
        export LASTPASS_EMAIL="me@example.com"
        
        if type lpass 2>&1 >/dev/null && [[ -f $HOME/.lpassfunctions ]]
        then
            . $HOME/.lpassfunctions
        fi

## Using

Set environment variables from LastPass values like this:

    set_env_var_from_lastpass ACME_KEY_ID
    $ echo $ACME_KEY_ID
    321decaf000bad

If you're feeling bold (or lazy), set all at once by adding this after you load `.lpassfunctions` in your `.bashrc`/`.bash_profile`:

    autoset_all_lastpass_keys

## Cheat Sheet for Data Entry

![API entry example](api-entry.png)
