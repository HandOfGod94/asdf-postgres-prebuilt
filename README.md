# asdf-postgres ![Build](https://github.com/smashedtoatoms/asdf-postgres/workflows/Build/badge.svg?branch=master)

Postgresql plugin for [asdf](https://github.com/asdf-vm/asdf) version manager

## Dependencies

- openssl
- libreadline
- zlib
- curl
- uuid
- icu-devtools (linux) or icu4c (MacOS)

_This assumes macOS, a Debian-flavored linux.  If you
need it to work on something else, you may need to modify the plugin. You'll
need the dependencies referenced in the list above installed via whatever method
you prefer. There are some suggestions below._

Note: This plugin has been tested only on macOS and Debian. Other operating systems have not been tested.

### Mac

```sh
brew install gcc readline zlib curl ossp-uuid icu4c pkg-config
```

If you are on Apple silicon, you may need to set the HOMEBREW_PREFIX environment
variable to `/opt/homebrew`. You can do this by putting `export
HOMEBREW_PREFIX=/opt/homebrew` in your `~/.zshrc` file. It isn't always
required, but I don't know why this is required for some people.  The same goes
for intel machines only the prefix is `/usr/local`, which can also be dropped
into your `~/.zshrc` file `export HOMEBREW_PREFIX=/usr/local`.

If you are using pkgconfig, you may run into issues with linking, particularly
with icu4c.  Often they can be resolved by setting your PKG_CONFIG_PATH
environment variable to reference where your linked libs live.  For example:

```sh
brew install gcc readline zlib curl ossp-uuid icu4c pkg-config
export PKG_CONFIG_PATH="/opt/homebrew/bin/pkg-config:$(brew --prefix icu4c)/lib/pkgconfig:$(brew --prefix curl)/lib/pkgconfig:$(brew --prefix zlib)/lib/pkgconfig"
```

### Ubuntu

```sh
sudo apt-get install linux-headers-$(uname -r) build-essential libssl-dev \
libreadline-dev zlib1g-dev libcurl4-openssl-dev uuid-dev icu-devtools libicu-dev
```

### Ubuntu (WSL)

```sh
sudo apt-get install build-essential libssl-dev libreadline-dev zlib1g-dev \
libcurl4-openssl-dev uuid-dev icu-devtools libicu-dev
```

### Fedora

```sh
sudo dnf install openssl-devel readline-devel zlib-devel libcurl-devel uuid-devel libuuid-devel
```

### (open)SUSE

```sh
sudo zypper install -t pattern devel_basis
sudo zypper in openssl-devel readline-devel zlib-devel libcurl-devel uuid-devel libuuid-devel
```

## Install

1. Add the plugin:

   ```sh
   asdf plugin add https://github.com/HandOfGod94/asdf-postgres-prebuilt.git
   ```

2. In your project's `.tool-versions` file, add the desired Postgres version using this plugin name:

   ```
   postgres-prebuilt 17.4.0
   ```

3. Install via asdf:

   ```sh
   asdf install
   ```

## Post-installation

To start a new database, you must provide a data directory when starting Postgres:

```sh
pg_ctl start -D /path/to/your/data
```

# How to use (easier version)

## Install

1. Add the plugin:

   ```sh
   asdf plugin add https://github.com/HandOfGod94/asdf-postgres-prebuilt.git
   ```

2. Create your `.tool-versions` file in the project that needs Postgres and add:
   `postgres-prebuilt 17.4.0`
3. Run `asdf install`

## Run

1. Once it is done, run `pg_ctl start -D /path/to/your/data`
2. Once that starts, run `createdb default` _you can sub `default` with whatever
   you want your db name to be._
3. Then log in with `psql -d default` _if you didn't use default, make sure you
   use whatever you put in step 2._
4. If you need to move your data file around, move data from your install
   directory, which will look something like this:
   `~/.asdf/installs/postgres/9.4.7/data` to wherever you want it. Once it is
   moved, start your db with `pg_ctl -D wherever/you/moved/it start`. Your
   postgresql.conf file is in that directory if you need to change the port or
   anything.

## Stop

1. Just run `pg_ctl stop` if you moved your data directory, you have to do
   `pg_ctl -D wherever/you/moved/it stop`

## Credits

Thanks to [theseus-rs/postgresql-binaries](https://github.com/theseus-rs/postgresql-binaries) for providing awesome PostgreSQL binaries.
