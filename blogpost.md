# Securing Secrets During Software Development

### Convenience vs. Security in Software Development

When building a site like [BecomeAnEX](https://becomeanex.org) or a mobile app like [This Is Quitting](http://www.thisisquitting.com/), our team communicates with third party services for hosting, text messaging, code deployment, and more. We send authentication information along with each basic request to prove that we are who we claim to be. This is not unlike the use of usernames and passwords on personal accounts.

Two competing priorities thus emerge:

1. The ability for developers to run test versions of the code for development and testing.
2. The need to keep the authentication secrets secure.

The easiest way for developers to run test versions of code is to keep these secrets stored in configuration files on their computers. However, that’s also highly unsecure, as secrets in plain, unencrypted files on laptops can easily be lost or stolen if the machine itself gets lost or stolen.

I was determined to find a secure way to manage authentication secrets without making them overly-burdensome to access.

### Can We Use LastPass for This?

Spoiler alert: Yes!

As a team, we're already using [LastPass](https://www.lastpass.com/) for managing passwords. Its most common use is for those same usernames and passwords we use in web browsing. We also use LastPass's secure notes and the ability to share those for non-web use (_e.g._ database credentials).

With this mindset, I set out to make my app authentication credentials more secure using LastPass. I knew that LastPass provides a command line utility for accessing data stored in LastPass. I figured that it would be possible to use this to set the environment variables I need without storing the secrets directly in files on my laptop, so I wrote some helper functions that work with the [`lastpass-cli`](https://github.com/lastpass/lastpass-cli) command line tool to set environment variables in my shell for this purpose.

After making some LastPass configuration changes and loading the file, you can set an environment variable like this:

    set_env_var_from_lastpass AWS_ACCESS_KEY_ID

This is what is now saved in those insecure configuration files. Your security auditors will be much happier about this.

### Using LastPass for your Secrets

For full installation and configuration instructions, see the README on the [project page at github](https://github.com/dougharris/lpassfunctions).

Once you've set up the folder and the note template, enter your secrets. Entry looks like this:

![Screenshot of LastPass entry]!(api-entry.png)

With that set, you can set your `ACME_API_KEY` in your environment without storing sensitive data in your .bashrc

    $ set_env_var_from_lastpass ACME_KEY_ID
    $ echo $ACME_KEY_ID
    321decaf000bad

    or

    $ connect-to-acme-server --api-key $ACME_KEY_ID

Alternately, put `autoset_all_lastpass_keys` into your shell startup script to set environment variables for all entries in your LastPass `API_KEYS` folder.

### Bugs or Suggestions?

Try this out. Let us know if you have problems or ideas for improvement by adding an issue on the project's [issue tracker](https://github.com/dougharris/lpassfunctions/issues)
