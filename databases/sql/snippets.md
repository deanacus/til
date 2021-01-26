# Random Notes

- `LIKE` and `IN` are hella bad performance wise
- JOIN syntax `JOIN table on table.colum = alreadySelectedTable.column`
- We use proper SQL DATEs, so we can do date maths, like

  ```sql
  SET @table := '';
  SET @date := '';

  @table.when_created >= @date
  ```

  or

  ```sql
  SET @table := '';
  SET @intervalDays := '';

  @table.start_time BETWEEN date_sub(current_date(), INTERVAL @intervalDays DAY) AND current_date()
  ```

## User Records for Emails in US

```sql
SELECT id,
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
FROM
  user_records
WHERE
  country = 'US'
```

## Select Newsletter Details for users with a specific game by game_key

```sql
SET @gameKey := 'fortnite'

SELECT id,
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
FROM user_records ur
  JOIN
    user_games ug
  ON
    ug.id = ur.id
  JOIN
    game g
  ON
    g.id = ug.game_id
WHERE
  g.game_key = @gameKey;
```

## Select Newsletter Details for users with a specific game by game_id

```sql
SET @gameID := '1234abcd';

SELECT user_records.id,
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
FROM user_records ur
  JOIN
    user_games ug
  ON
    ug.user_id = ur.id
WHERE
  ug.game_id = @gameID;
```

## Get external game id for a specific game for a player

```sql
SET @handle := 'xShootingBlanksx';
SET @gameName := 'fortnite';

SELECT distinct(external_game_identity)
FROM user_game_region ugr
  JOIN
    user_games ug
  ON
    ug.id = ugr.user_game_id
  JOIN
    game g
  ON
    g.id = ug.game_id
  JOIN
    user_records ur
  ON
    ur.id = ug.user_id
WHERE
  ur.handle LIKE @handle
AND
  g.name = @gameName;
```

## Select game_region data for @gameKey plus user.handle for @userHandle that has entered a tournament after @date

```sql
SET @handle := 'xShootingBlanksx';
SET @gameName := 'fortnite';
SET @entryDate := '2020-11-30';

SELECT gr.*,
  u.handle
FROM tournament_entry_score tes
  JOIN
    game_result gr
  ON
    gr.id = tes.game_result_id
  JOIN
    single_player_tournament_entry spte
  ON
    spte.id = tes.entry_id
  JOIN
    single_player_tournament spt
  ON
    spt.id = spte.tournament_id
  JOIN
    game g
  ON
    g.id = spt.game_id
  JOIN
    user_records u
  ON
    u.id = spte.user_id
WHERE
  u.handle = @userHandle
AND
  g.gameKey = @gameKey
AND
  tes.when_created >= @entryDate
```

## Top 3 Closed Tournaments in the last 90 days by entries for @gameKey

```sql
set @gameKey := 'fortnite';

SELECT g.name as Game,
  spt.name as Tournament,
  spt.slots as 'Tournament Slots',
  spte.tournament_id as 'Tournament ID',
  count(*) as entries
FROM
  single_player_tournament_entry spte
JOIN
  single_player_tournament spt
ON
  spt.id = spte.tournament_id
JOIN
  game g
ON
  g.id = spt.game_id
AND
  g.gameKey = @gameKey
AND
  spt.start_time
  BETWEEN
    date_sub(current_date(), INTERVAL 90 DAY)
  AND
    current_date()
AND
  spt.finished = 1
GROUP BY
  spte.tournament_id
ORDER BY
  count(*)
DESC
LIMIT
  3
```

Get
```sql
set @tournamentID := '';

SELECT *
FROM
  user_records ur
JOIN
  single_player_tournament_entry spte
ON
  spte.user_id = ur.id
JOIN
  single_player_tournament spt on spt.id = spte.tournament_id
WHERE
  spt.id = @tournament;
```
