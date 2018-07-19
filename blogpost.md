# Securing Secrets During Software Development

## Convenience vs. Security in Software Development

When building a site like [BecomeAnEX](https://becomeanex.org) or a mobile app like [This Is Quitting](http://www.thisisquitting.com/), we rely on communication with third party services for hosting, text messaging, code deployment, and more. Along with the basic request, we send authentication information along to prove that we are who we claim to be. This is not unlike the use of usernames and passwords on personal accounts.

This leads to two competing priorities:

1. The ability for developers to run test versions of the code for development and testing.
2. The need to keep the authentication secrets secure.

Priority 1 means that the developers work most easily with these secrets stored in configuration files on their computers. Just secrets in plain, unencrypted files on laptops that can get stolen or lost. (Note: This is not unique to developers on our team.)

This does not mesh well with Priority 2.

## Can We Use LastPass for This?

As a team, we're already using [LastPass](https://www.lastpass.com/) for managing passwords. Its most common use is for those same usernames and passwords we use in web browsing. We also use LastPass's secure notes and the ability to share those for non-web use (_e.g._ database credentials).

With this mindset, I set out to make my app authentication credentials more secure using LastPass. I knew that LastPass provides a command line utility for accessing data stored in LastPass. 

I figured that it would be possible to use this to set the environment variables I need without storing the secrets directly in files on my laptop, so I wrote some helper functions that work with the [`lastpass-cli`](https://github.com/lastpass/lastpass-cli) command line tool to set environment variables in my shell for this purpose.

After making some LastPass configuration changes and loading the file, you can set an environment variable like this:

    set_env_var_from_lastpass AWS_ACCESS_KEY_ID

This is what is now saved in those insecure configuration files. Your security auditors will be much happier about this.

## Using This

For full installation and configuration instructions, see the README on the [project page at github](https://github.com/dougharris/lpassfunctions).

Once you've set up the folder and the note template, enter your secrets. Entry looks like this:

![](api-entry.png)


With that set, you can set your `ACME_API_KEY` in your environment without storing sensitive
data in your .bashrc

    $ set_env_var_from_lastpass ACME_KEY_ID
    $ echo $ACME_KEY_ID
    321decaf000bad
or

    $ connect-to-acme-server --api-key $ACME_KEY_ID

Alternately, put `autoset_all_lastpass_keys` into your shell startup script to set environment variables for all entries in your LastPass `API_KEYS` folder.

## Bugs or Suggestions?

Try this out. Let us know if you have problems or ideas for improvement by adding an issue on the project's [issue tracker](https://github.com/dougharris/lpassfunctions/issues)
