# ssh_c2c
Convert an ssh command to an ssh config entry.

## Architecture overview
Out of the gate, I'm hoping this will be some plain Ruby classes, tested with a
data-driven testing approach, which just take a string input (the ssh command)
and produce string output (the ssh config entry). On top of that, I'd like to
have a one page jekyll site that exposes this functionality in a web app. In the
long term, I'd like to extract the classes into a CL executable gem.

### Web App
Hoping to use a terminal emulator theme. Sticking with vanilla jekyll, I would
need a page reload to get the response, but it definitely seems preferable to
have an endpoint I could hit with ajax. I think that probably means Heroku.

### Testing
I don't think CI is a priority out of the gate, but I would like to make the
test cases easily human readable on Github. I need to decide on a storage format
but I think redcarpet would make it fairly easy to store things in markdown,
then parse those and run the tests. This would also help me audit the tests.

### Core logic
To start with I should probably just write some gross procedural string parsing
but I think there is potential for a simple class that holds a user@host
component with its optional args, and if that is done right, I could use several
in one process to deal with tunneling, etc. Alternatively, those might all just
be strings in the `ProxyCommand` attribute, I haven't actually figured that out.

It probably also makes sense to have a library of translations for arguments.
This would be for composable arguments that aren't on the `-o` flag, like your
private key file location. Each of these has a fairly simple correspondence,
i.e. `-i ~/.ssh/id_rsa` is `IdentityFile ~/.ssh/id_rsa`. This could probably
just be a string-keyed hash.

## References
### DDT
* https://en.wikipedia.org/wiki/Data-driven_testing

### jekyll
* where did I put that nice tutorial that used ruby classes

### ssh
* handling `-o` options - https://serverfault.com/questions/495283/convert-ssh-command-line-arguments-to-ssh-config-properties
* config attributes only (cl options are in cl man pages) - https://linux.die.net/man/5/ssh_config
