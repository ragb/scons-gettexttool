# SCons Gettext Tool


 This tool allows generation of gettext .mo compiled files, pot files from source code files
and pot files for merging.

## Requirements


You need a [Gettext](https://www.gnu.org/software/gettext/). Most unix-like (Linux/OSX) come with this or have them in repositories. For windows you can check [here](http://gnuwin32.sourceforge.net/packages/gettext.htm) or better use **cygwin** to be happy.

## Installation

In your SCons based build, put this folder on your
**site_scons/site_tools** directory. If you are using git you can even add this repo as a submodule.

## Usage

Pass ```'gettexttool'``` on SCons environment construction to get access to the new builders.

Three new builders are added into the constructed environment:

- gettextMoFile: generates .mo file from .pot file using msgfmt.
- gettextPotFile: Generates .pot file from source code files.
- gettextMergePotFile: Creates a .pot file appropriate for merging into existing .po files.

To properly configure get text, define the following variables:

- gettext_package_bugs_address
- gettext_package_name
- gettext_package_version

For example, you can put this on your **SConstruct file**
```Python
gettextvars={
		'gettext_package_bugs_address' : 'email@example.com',
		'gettext_package_name' : 'mypackage,
		'gettext_package_version' : '1.0'
	}
env = Environment(ENV=os.environ, tools=['gettexttool'], **gettextvars)

# ... whatever you need to build

# To generate gettext compiled files:
for poFile in env.Glob(os.path.join("po", "*.po")):
	moFile=env.gettextMoFile(poFile)

# To generate a pot file:
pot = env.gettextPotFile("mypackage.pot", ["main.c", "utils.c"])
env.Alias('pot', pot)


```

## Credits

This code is based on code from the [NVDA Projet](http://www.nvda-project.org) and is primarily used on the [NVDA add-on template project](https://bitbucket.org/nvdaaddonteam/addontemplate)

