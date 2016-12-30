# How to mass rename with zsh

I recently wanted to rename a bunch of folders names DDMMYYY to YYYYMMDD. This
was easily accomplished with the excellent tool `zmv`.

	$ zmv '([0-9][0-9])([0-9][0-9])([0-9][0-9][0-9][0-9])' '$3$2$1'

Note, you probably have to load the `zmv` tool before you can use it.

	$ autoload zmv
