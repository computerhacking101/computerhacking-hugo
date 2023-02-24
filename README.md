
## Upgrade theme

```shell
$ hugo mod get github.com/razonyang/hugo-theme-bootstrap@master
$ hugo mod npm pack
$ npm update
$ git add go.mod go.sum package.json package-lock.json
$ git commit -m 'Update the theme'
```






The following parameters also need to be tweaked.

- Replace the `utterances.repo` with your own to get notified when someone comments.
- Modify the `repo` to your own, or delete it if it's unused.
- `contact.endpoint`.

There are some hooks under the `layouts/partials/hooks` folder for showing how to use them, please feel free to delete them.

## Documentations

- [English](https://hbs.razonyang.com/v1/en/)

