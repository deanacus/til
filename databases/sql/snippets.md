# Random Notes

* `LIKE` and `IN` are hella bad performance wise
* JOIN syntax `JOIN table on table.colum = alreadySelectedTable.column`


# User Records for Emails

To get all users, remove the `where` clause and `limit`

```
select
  id,
  username,
  enabled,
  banned,
  accountSuspendedUntil,
  email,
  handle,
  firstname,
  lastname,
  state,
  country,
  marketing_auth
from
  user_records
where
  country = 'US'
limit
  60000;
```

# Get external game id for a specific game for a player

```
SELECT distinct(external_game_identity) FROM user_game_region ugr
  JOIN user_games ug on ug.id = ugr.user_game_id
  JOIN game g on g.id = ug.game_id
  JOIN user_records ur on ur.id = ug.user_id
  WHERE ur.handle LIKE ''
  AND g.name = '';
```

## Select GR data plus handle for user.handle

```
select
  gr.*,
  u.handle
from
  tournament_entry_score tes
  join game_result gr on gr.id = tes.game_result_id
  join single_player_tournament_entry spte on spte.id = tes.entry_id
  join single_player_tournament spt on spt.id = spte.tournament_id
  join game g on g.id = spt.game_id
  join user_records u on u.id = spte.user_id
where
  u.handle = ''
  and g.gameKey = 'fortnite'
  AND tes.when_created >= '2020-04-13'
```