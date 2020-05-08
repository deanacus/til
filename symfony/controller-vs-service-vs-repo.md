# What Files For What Purpose

I'm dumb, and don't know which files are for which purpose in MVC. So my boss gave me this:

## Controller

The request handler, really routing and templating, shouldn't do any thinking beyond that

## Service

Business logic, all the thinking and combining and data formatting

## Repository

Data access/DAO, where the SQL lives, should return in PHP object
