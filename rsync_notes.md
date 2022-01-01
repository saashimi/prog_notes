## Rsync notes

For backup:
`rsync -avrum --delete <source/folder/> <dest/folder/>`

Notes:
- add -n for dry run
- ensure that there is a trailing slash `/` so folder-level --delete works.