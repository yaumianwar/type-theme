---
layout: post
title: The Problem You Might Find In Django Migrations
description: 
feature-img:
date: 2017-06-02 22:00:00 +08:00
---

Some time ago, i found problem in Django Migrations. When i run `makemigrations` command, there's nothing wrong with my migration. But when i run `migration` command, i got error message :

```
django.db.utils.OperationalError: (1426, "Too-big precision 100 specified for column 'amount'. Maximum is 65.")
```

By that error message, i realize that my mistake is the value of `max_digits` in column `amount` (DecimalField) out of range. I set with 100 but maximum value is 65. So i changed my model, create new migration, then run `migration` command again.

Yeay I think my problem has been fixed. But i got the same error message :

```
django.db.utils.OperationalError: (1426, "Too-big precision 100 specified for column 'amount'. Maximum is 65.")
```

So, checked my model again and make sure that the value  of `max_digits` in column `amount` is not out of range. I'm sure that the value is correct but when i run `migration` command again, i got the same error message again. It's makes me so confused dan exhauted, cause i'm looking for the information about how to solve problem that similiar with mine but i didn't find it.

I finally solved the problem after 8 days. The problem is Django Migrate still applying all of migrations, including the migration of my incorrect model (value of `max_digits` in column `amount` is out of range) that not apply yet. Cause the file of that migration is still exist and i need to change the value of `max_digits` in that file to solve my problem. FYI, if you run `makemigrations` command, it will create new migrations based on the changes you have made to your models and `migrate` command will applying all of migrations that not apply yet. If `migrate` error, then the migrations is still not applying. 

So you need to check your migrations that not applying after run `migrate` command if you got the same problem even though you have changed your model and make new migrations :)