Active Record allows you to name your associations therefore making it more straight forward when working with it

Charlie, Sara, Laura, Ben team survey app

class User < ActiveRecord::Base
has_many :surveys_taken, through: :user_surveys, source: survey

(source == what is the column we are looking at in that user_surveys table? we are looking at survey (e.g. survey_id))

----

our survey app

class User < ActiveRecord::Base
  include BCrypt

  has_many :completions
  has_many :surveys, through: :completions

  has_many :responses
  has_many :choices, through: :responses

  has_many :surveys_created, :class_name => 'Survey', :foreign_key => 'creator_id'


  validates :email, uniqueness: true, presence: true

  line 22 is another way to name the association, a user can make many surveys via survey known as 'the creator id'
  ---

  steven, danny, brian, sam team

  "bubbling in the dom" -- adding bindings to the dom via javascript how append to it and keep going?

  combination of using the click event "on" and removing the last class

  ____

  jacky, kevin, tara, diana

  team implemented a "modal", a hover pop up to sign in if not signed in

implemented nav bar via javascript


----

ADVANCED ACTIVE RECORD ASSOCIATIONS


github.com/joshcheek/active_record_exercises

active record error 'uninitialized constant' means that thing DNE

class User < ActiveRecord::Base
  has_many :surveys
end

class User < ActiveRecord::Base
  has_many :created_surveys
end

will throw an 'uninitialized constant' for user.all.first.created_surveys

class User < ActiveRecord::Base
  has_many :created_surveys, class_name: 'Survey'
end


this understands now which table to use!

will want to update the survey too as well!

class Survey < ActiveRecord::Base
  belongs_to :creator, class_name: "User"
  ##belongs_to :creator, foreign_key: "user_id" class_name: "User"
end

could do opt 2 but to people outside whom may only analyze the database this is not intuitive as to who is the creator_id ergo use opt 1 and make schema as below

create_table :surveys do |t|
  t.string :name
  t.string :creator_id
end

advanced active record associations allow for more context in your code

(clear console command + k)

-- now add that users can take many surveys!

class User < ActiveRecord::Base
  has_many :created_surveys, class_name: 'Survey'
  has_man :survey_completions
  has_many :completed_surveys, through: :survey_completions, source: :survey

  ## of if no source on survey_complets do ....
end

create_table :survey_completions do |t|
  t.integer :user_id
  t.integer :survey_id
end

class SurveyCompletion
  belongs_to :survey
  belongs_to :user
  ##belongs_to :completed_survey, class_name: "Survey", foreign_key: "survey_id" --> if no source on line 95
end