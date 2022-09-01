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
AND
  banned = 0;
```

## Select Newsletter Details for users with a specific game by game_key

```sql
SET @gameKey := 'league_of_legends'

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
    ug.user_id = ur.id
  JOIN
    game g
  ON
    g.id = ug.game_id
WHERE
  g.game_key = @gameKey
AND
  banned = 0;
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
AND
  banned = 0;
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

## Count users who have entered at least one tournament:

```sql
SELECT count(distinct(spte.user_id)) FROM single_player_tournament_entry spte
  JOIN
    user_records ur
  ON
    ur.id = spte.user_id
```

## Get number users of each age in tournament:

```SQL
SET @tournamentID := '';
SELECT
  COUNT(spte.user_id),
  DATE_FORMAT(NOW(), '%Y') - DATE_FORMAT(ur.dob, '%Y') - (DATE_FORMAT(NOW(), '00-%m-%d') < DATE_FORMAT(ur.dob, '00-%m-%d')) as Age
FROM
  single_player_tournament_entry spte
JOIN
  user_records ur ON ur.id = spte.user_id
WHERE
  tournament_id = @tournamentID
GROUP BY
  Age
```

## Select a subset of user data with nice column names and list all the tournaments they've entered in one field:

```SQL
SELECT DISTINCT
  ur.id,
  ur.date_created AS 'Joined',
  DATE_FORMAT(NOW(), '%Y') - DATE_FORMAT(ur.dob, '%Y') - (DATE_FORMAT(NOW(), '00-%m-%d') < DATE_FORMAT(ur.dob, '00-%m-%d')) AS 'Age',
  ur.country AS 'Country',
  ur.state AS 'State',
  ur.zip_code AS 'Zip',
  ur.city AS 'City',
  ur.banned AS 'Is Banned',
  GROUP_CONCAT(spte.tournament_id) as 'Entered Tournaments'
FROM
  single_player_tournament_entry spte
JOIN
  user_records ur
ON
  ur.id = spte.user_id
GROUP BY
  ur.id
  ```


```sql
SELECT distinct
  ur.handle as Handle,
  g.name as 'Game Name',
  ur.email as 'Email Address',
  ur.country as Country,
  ur.state as State,
  ur.date_created as 'Date Created',
  ur.registration_tracking_params as 'Registration Tracking Params'
FROM
  user_records ur
JOIN
  user_games ug ON ug.user_id = ur.id
JOIN
  game g ON g.id = ug.game_id
WHERE
  ur.date_created BETWEEN '2021-05-20' AND '2021-06-14'
AND
  ur.banned is false;
```