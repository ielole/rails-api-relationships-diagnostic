# Rails API Relationships Diagnostic

Place your responses inside the fenced code-blocks where indivated by comments.

1.  Describe a reason why a join tables may be valuable.

```sh
  Joined tables are useful when you want to show the relationship between two different tables which contain related objects. If those two tables share a relationship via a joined table, then each time a change is made to one of the objects, those changes will be able to be seen as it relates to the other object.
```

1.  Provide a database table structure and explain the Entity Relationship that
describes a many-to-many relationship for `Profiles`, `Movies` and `Favorites`
(Think of Netflix). A `Profile` has a `given_name`, `surname` and `email` and a
`Movies` have `title`, `release_date`, and `length` and `Favorites` would be the
join table with references to `Movies` and `Profiles`.

```sh
  table: Profile
  columns: id, given_name, surname, email, created_at, updated_at

  table: Movies
  columns: id, title, release_date, length, created_at, updated_at

  table: Favorites
  columns: id, movie_id, profile_id, created_at, updated_at

  A profile can have many Favorites
  A profile can have many Movies
  A movie can have many Favorites
  A movie can belong to many profiles
  A Favorite belongs to a profile and a movie

```

1.  For the above example, what needs to be added to the Model files?

```rb
class Profile < ActiveRecord::Base
  has_many :movies, through: :favorites
  has_many :favorites
end
```

```rb
class Movie < ActiveRecord::Base
  has_many :profiles, through: :favorites
  has_many :favorites
end
```

```rb
class Favorite < ActiveRecord::Base
  belongs_to :profile, inverse_of: :favorites
  belongs_to :movie, inverse_of: favorites
end
```

1.  What is the purpose of a serializer? What would our `Profile` serializer look
like to show all movies favorited by a profile on
`http://localhost:3000/profiles/1`

```sh
  A serializer is a way to filter what columns of a table, in the database, are shown.

```

```rb
class ProfileSerializer < ActiveModel::Serializer
  attributes: :id, :given_name, :surname, :email, :favorites
end
```

1.  What would the command be to _scaffold_ out a **join table** for Favorites from
the above `Movies` and `Profiles`.

```sh
  bundle exec rails g scaffold favorite movie_id:integer profile_id:integer
```

1.  What is `Dependent: Destroy` and where/why would we use it?

```sh
  It is used inside of the models for the two objects that make up the joined table. They say that the joined table record would be deleted if either of them was deleted.
```

1.  Think of **ANY** example where you would have a one-to-many relationship as well
as a many-to-many relationship in an application. You only need to list the
description about the resources and how they relate to one another.

```sh
  # < Your Response Here >
```
