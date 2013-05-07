[rvm](https://rvm.io/) can be used to easily set up different ruby environments for testing and running sup and gemsets.

## Installation

```bash
curl -L https://get.rvm.io | bash -s stable --autolibs=enabled --ruby=1.9.3
# Follow whatever instructions rvm prints later.
# For a normal user it could probably be this:
source ~/.rvm/scripts/rvm
```

## Creating gemset and alias for easy development

```bash
rvm gemset create sup-development
rvm alias create sup 1.9.3@sup-development
rvm use sup
```

Now you're ready to hack on sup.