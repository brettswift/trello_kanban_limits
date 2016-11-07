# Kanban Trello Limit checks

This project is a simple rake task to check the Trello API for card limits.

The intention is to be run on the CI server and keep us honest to the card limits.

## Setup Requirements

You need two environment variables setup with your trello API keys.

Two environment variables are required:

* `TRELLO_DEVELOPER_PUBLIC_KEY` get this at [https://trello.com/app-key](https://trello.com/app-key)
* `TRELLO_MEMBER_TOKEN` generate this.  On the page above there is a link to a token.   Use that link to generate and authorize your token.


### Setting Environment variables

* do this on your CI server in the appropriate spot.
* on development machines:
  1. create a file `./config/env.yml`
  2. use the `env.yml.example` as an example.

## Configuration

All board configuration goes in `app.yaml` and is currently hard coded into the source.

Sample:

```
boards:
  'Configuration Management & Pipeline Automation':
    id: 'alhujPoV'
    lists:
      'Estimate':
        cardlimit: 10
```
### `boards`
* hash list - the exact name of the board.
* to view them run `bundle exec rake list_boards`

### `<board_name>`

Has two params:

`id`: the short id in the URL at the top of the board after trello.com/b
`lists`: hash of trello lists, by <list name>

### `<list name>`
* regex is used, so this can be a partial match of the list name
* contains one param - `cardlimit`

### `cardlimit`: integer.  The max count of cards allowed.
