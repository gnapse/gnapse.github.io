---
layout: post
title: "active_importer"
tagline: "Importing data the Ruby way"
date: 2014-02-17 19:34
tags: [Ruby]
categories: programming
---

We all love using YAML or JSON for serializing and storing data that our
applications will consume.  But the truth is we will often need our apps to
consume data coming in different formats.

Spreadsheets are a common example.  At [Continuum](http://continuumhq.co) we
have faced this scenario a few times.  Our clients often want their apps to be
able to import data from a source format that they can build and provide
themselves.  And what data format do they have available, other than Excel, or
any other similar spreadsheets software?  Let's face it: we need to interface
with them wether we like it or not.

In the Ruby world we have the [Roo][] gem, more than enough for reading the
most common spreadsheet formats out there.  But that's just half the job.  The
data won't get imported into your data models by magic.  And that's where
[active_importer][] comes into play.

[Roo]: https://github.com/Empact/roo
[active_importer]: https://github.com/continuum/active_importer

## The basics

At first we needed a simple and generic interface between spreadsheets and our
ORM.  The simplest approach initially was to give the importer a [mapping
between spreadsheet columns and a model's attributes][].  The importer would
then handle [all the messy work][]: parsing the spreadsheet's column headers,
iterate over the rows, and create new records in the database according to the
mapping provided.

We started off by trying to achieve something like this:

```ruby
class EmployeeImporter < ActiveImporter::Base
  imports Employee
  column 'First name', :first_name
  column 'Last name', :last_name
  column 'Department', :department
end

# That class would later be used in a way like this:
EmployeeImporter.import('/path/to/some/spreadsheet.xlsx')
```

That's a pretty straightforward characterization of the goals described before.
We started with this in mind, and developed the library to make that code above
come to life.

[mapping between spreadsheet columns and a model's attributes]: https://github.com/continuum/active_importer/wiki/Mapping-columns-to-attributes
[all the messy work]: https://github.com/continuum/active_importer/wiki/Understanding-how-spreadsheets-are-parsed

## Associations

But wait, usually in a data model like the one you can infer from the previous
example, the department name won't be stored as plain text in each employee
record.  Instead, one would create a `Department` data model, and associate
each employee with the appropriate department record.

So we cannot directly map the value coming from the spreadsheet (a string with
the name of the employee's department), to the employee's department attribute.
We need to find the department by that name, and associate it with the employee
instead.  So we came up with this:

```ruby
class EmployeeImporter < ActiveImporter::Base
  imports Employee
  column 'First name', :first_name
  column 'Last name', :last_name
  column 'Department', :department do |department_name|
    Department.find_by(name: department_name)
  end
end
```

By adding a block to our column declaration, we can customize the incoming
value before storing it in our data models.

## Update instead of create

After a short time this simple approach proved to have some shortcomings. For
instance, what if you want the importer to not always create new records? The
data in the spreadsheet could refer to already existing records, and the intent
in that case is to update those records. What we needed is a way to instruct
the importer to find the record, if it exists, or create a new one if it does
not.

```ruby
class EmployeeImporter < ActiveImporter::Base
  imports Employee

  fetch_model do
    Employee.where({
      first_name: row['First name'],
      last_name: row['Last name'],
    }).first_or_initialize
  end

  # ...
end
```

Here we're instructing the importer to find an employee already registered with
the given first and last names.  If it exists, then that record is updated.  If
it does not exist, then a new employee is created instead.

## What's next

This was just the tip of the iceberg of what we've done with active_importer.
It now supports more complex logic when simple column mappings are not enough.
It supports event callbacks, custom parameters, error handling, skipping rows,
aborting the import process, and even enabling database transactions.  Check
out the project's [wiki](https://github.com/continuum/active_importer/wiki) to
learn more.

This is still a very young project, with lots of potential to improve in the
next few months, and we really hope it will catch the attention of any
developers out there needing to implement similar features in their apps.  And
of course, contributing is really welcome.  So feel free to report issues,
suggest new features, or even submit pull requests.
