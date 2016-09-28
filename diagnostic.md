# Rails API Relationships Diagnostic

Place your responses inside the fenced code-blocks where indivated by comments.

1.  Describe a reason why a join tables may be valuable.

```sh
Join tables are valuable to establish many-to-many relationships between two separate tables _through_ the join table. To beat the example to death, a physician can have many patients, and a patient can have many doctors, and they are able to have this relationship through a join table, appointments. We can then view all a doctor\'s patients or a patient\'s doctors directly, without having to go through extra steps to access that information.
```

1.  Provide a database table structure and explain the Entity Relationship that
describes a many-to-many relationship for `Profiles`, `Movies` and `Favorites`
(Think of Netflix). A `Profile` has a `given_name`, `surname` and `email` and a
`Movies` have `title`, `release_date`, and `length` and `Favorites` would be the
join table with references to `Movies` and `Profiles`.

```sh
PROFILE
  - given_name:string
  - surname:string
  - email:string

MOVIES
  - title:string
  - release_date:datetime
  - length:integer

FAVORITES
  - movie:references
  - profile:references

A Movie can have many profiles, through favorites, and a profile can have many movies, also through favorites. Favorites is the join table that creates the link and relatioship between these two entities.
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
  belongs_to :movie, inverse_of :favorites
  belongs_to :profile, inverse_of :favorites
end
```

1.  What is the purpose of a serializer? What would our `Profile` serializer look
like to show all movies favorited by a profile on
`http://localhost:3000/profiles/1`

```sh
A serializer is used to specify what data/information will be passed back to the user / in the JSON response from the server.
```

```rb
class ProfileSerializer < ActiveModel::Serializer
  attributes: :given_name :surname :email :movies
end
```

1.  What would the command be to _scaffold_ out a **join table** for Favorites from
the above `Movies` and `Profiles`.

```sh
bundle exec rails g scaffold Favorites profile:references movie:references
```

1.  What is `Dependent: Destroy` and where/why would we use it?

```sh
Dependent: Destroy is used in the has_many relationship line in the model, and it indicates that, if we delete an instance of that model, we can delete all the associated classes. So using the above example of profiles/movies/favorites, if we delete a movie, we wouldn\'t want to also delete the profile... but if we delete the profile, we would want to delete that profile\'s favorite movies, so the has_many :favorites line in the model would have a dependent: destroy clause.
```

1.  Think of **ANY** example where you would have a one-to-many relationship as well
as a many-to-many relationship in an application. You only need to list the
description about the resources and how they relate to one another.

```sh
A to do list app:

A user would have many lists, but each list would only have one user (one-to-many).

However, a list would have many tasks, and a task could be on more than one list (say, we\'re packing for an upcoming trip - that could appear on a shopping to-do list, as well as a packing to-do list) so we would need a join table, something like 'category' that could link the two.

I\'m basing this idea roughly off of Apple\'s 'reminders' app.
```
