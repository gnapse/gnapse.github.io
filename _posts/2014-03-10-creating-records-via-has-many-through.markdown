---
layout: post
title: "Creating records via a has_many :through association"
date: 2014-03-10 12:37
tags: [active-record, ruby-on-rails]
categories: programming
---

Today I found a really odd situation while working on a Rails 4 project. I
created a couple of data models having a many-to-many association with
`has_many :through` in both directions. A bit of code will help clearing up
things:

```ruby
# app/models/account.rb
class Account < ActiveRecord::Base
  has_many :account_users
  has_many :users, through: :account_users
end

# app/models/user.rb
class User < ActiveRecord::Base
  has_many :account_users
  has_many :accounts, through: :account_users
end
```

And the join model of course:

```ruby
# app/models/account_user.rb
class AccountUser < ActiveRecord::Base
  belongs_to :account
  belongs_to :user
end
```

Up until now everything's ok.  I can access associated records from each
endpoint of the association, and Rails handles the SQL join, like we're all
used to.  Hell, I can even create associated records with a single command, and
ActiveRecord creates the intermediate `AccountUser` record for me.  It's
heaven.

```ruby
> account = Account.create(name: "Main")
=> #<Account id: 1, name: "Main", ...>
> account.users.count
=> 0
> user = account.users.create(name: "johndoe")
=> #<User id: 1, name: "johndoe", ...>
> user.accounts.count
=> 1
```

The problem occurs when trying to build the record first in memory, and saving
it later:

```ruby
> user = account.users.build(name: "uglyjoe")
=> #<User id: nil, name: "uglyjoe", ...>
> user.save
=> true
> user.accounts.count
=> 0
```

The user record was saved, but **it was not associated to the account**.

## The solution

After a while digging, I found [this issue on Rails' github repository][], and
specifically, [this comment][].  It turns out that in these cases, **you need
to specify the `:inverse_of` option to the `belongs_to` association in the
intermediate join model**, in our case, the `AccountUser` class.

[this issue on Rails' github repository]: https://github.com/rails/rails/issues/6161
[this comment]: https://github.com/rails/rails/issues/6161#issuecomment-6330795

```ruby
# app/models/account_user.rb
class AccountUser < ActiveRecord::Base
  belongs_to :account
  belongs_to :user, inverse_of: :account_users
end
```

Note that you only have to specify the inverse on the association that links
with the model being created.  If you ever want to create accounts from the
user model, then you also have to specify the inverse to the `belongs_to
:account` association.  However, my recommendation would be to **always specify
the inverse to both associations** in a join model like this one.

## But wait, there's more

It turns out that the `:inverse_of` option is there for even more reasons.
After discovering the solution above, I decided to dig a bit deeper about it,
and I found out that **it is always desirable to declare the inverse on all
`belongs_to` associations**.  It optimizes the way Rails instantiates
ActiveRecord objects.  I won't go through the details, but you can take a look
[here](http://bit.ly/1fQDDHq), [here](http://bit.ly/1ndTDXQ) and
[here](http://bit.ly/1glyguN) if you're interested.

By the way, I still don't know why Rails doesn't make all these `:inverse_of`
goodness work out of the box when it could infer the inverse association in
cases like the one above.  I'm sure there must be a reason though.
