# `psql`

`psql` is a command line tool which allows us to write SQL commands directly on the command line. You can think of it as being similar to the `mongo` shell.

## Installation

Install `psql` with Homebrew:

```sh
brew install postgresql
```

This will also add the `createdb` and `dropdb` commands for us. By default when you launch `psql` it will attempt to connect to a database that has the same name as your username. Once `psql` is installed you should create this database:

```sh
# replace mickyginger with your username that you use with your laptop
createdb mickyginger
```

Check that everything is installed correctly by typing:

```sh
psql
```

You should see the following:

```sh
mickyginger=#
```

## Usage

Outside of writing SQL, we can use the following commands to navigate `psql`:

| command | description | example |
|---------|-------------|---------|
| `\c` | connect to a sepecific database | `\c cardb` |
| `\d` | describe a table | `\d cars` |
| `\l` | list databases | `\l` |
| `\q` | Quit psql | `\q` |

## SQL commands

Once you have navigated to a database you can directly type SQL commands in the terminal. The prompt will change depending on what you are typing:

| prompt | meaning |
|--------|---------|
| `=#` | Default prompt. `psql` is ready for a command |
| `(#` | Inside parenthesis. The parenthesis needs closing before executing the command |
| `'#` | Inside a single quote. The single quote needs closing before executing the command |
| `"#` | Inside a double quote. The double quote needs closing before executing the command |
| `-#` | Command requires a semi-colon to execute |

If at any point you need to cancel the current command, you can do so with <kbd>CTRL</kbd> + <kbd>C</kbd>

## Further reading

* [PSQL Command Line Cheet Sheet - Github](https://gist.github.com/Kartones/dd3ff5ec5ea238d4c546)
* [psql - Postgres Guide](http://postgresguide.com/utilities/psql.html)
