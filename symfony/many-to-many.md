Some gotchas about ManyToMany and join tables:

You can't control the names of the automatically generated indexes with 
Doctrine, doctrine will manage those automatically. If you set the indexes 
manually, Doctrine will rename them to whatever names it wants to generate, 
leading to an ever expanding bunch of shite in the doctrine:migrations:diff file 
- we're learning from experience here cos the documentation aint great. So for 
    join tables only, don't rename the indexes and foreign keys from the garbage 
    FK_3CE1205833D1A3E7 format in the migration file.

For ManyToMany you only define the jointable on one side, otherwise it tries to 
duplicate the tables and throws an error

You have to do mappedBy and inversedBy so Doctrine knows what the other side is, 
ie.

```
@ORM\ManyToMany(targetEntity="XYGaming\UserBundle\Entity\User", 
inversedBy="featuredPlayerTournaments")
private $featured_players;
...
@ORM\ManyToMany(targetEntity="XYGaming\ChallengeBundle\Entity\SinglePlayerTournament", 
mappedBy="featured_players")
private $featuredPlayerTournaments;
```
