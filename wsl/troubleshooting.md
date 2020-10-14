# Troubleshooting Occasional Issues

## Apache can't access symfony cache folders

An issue I've run into a few times is that the www-data user that apache runs
under doesn't have permissions to read/write the cache directories in the
symfony project. Rather than dealing with any sort of changing groups or
owners or anything, I just changed the Apache user to be my user account, so
that when doctrine creates the directories, it does so with the same user that
will try to access them later.
