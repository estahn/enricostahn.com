---
layout: post
subtitle: null
date: "2011-06-11"
published: true
title: PostgreSQL - Add Primary Key to an Existing Table
redirect_from:
  - /2010/06/11/postgresql-add-primary-key-to-an-existing-table
---

The database we’re working on has some strange design issues and some of them are incompatible with the ORM layer we’re moving to. One of the issues is the lack of a primary key in some tables. In PostgreSQL we can solve this with the following steps:

1. Add a column with type integer to your table
2. Create a sequence
3. Update the column table with sequence values
4. Set the necessary column properties (e.g. default, not null, etc.)

Example for table `foo` and column `id`:
```SQL
ALTER TABLE "public"."foo" ADD COLUMN "id" INTEGER;
CREATE SEQUENCE "public"."foo_id_seq";
UPDATE foo SET id = nextval('"public"."foo_id_seq"');
ALTER TABLE "public"."foo" ALTER COLUMN "id" SET DEFAULT nextval('"public"."foo_id_seq"');
ALTER TABLE "public"."foo" ALTER COLUMN "id" SET NOT NULL;
ALTER TABLE "public"."foo" ADD UNIQUE ("id");
ALTER TABLE "public"."foo" DROP CONSTRAINT "foo_id_key" RESTRICT;
ALTER TABLE "public"."foo" ADD PRIMARY KEY ("id");
```
