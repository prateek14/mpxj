# MPXJ

This gem allows a Ruby developer to work with a read-only view of project plans saved by a number of popular project planning applications.
The work required to read data from these files is actually carried out by a [Java library](http://mpxj.sf.net), hence you will need Java installed
and available on your path in order to work with this gem. Once the project data has been read from a file, a set of Ruby objects provides access to the
structure of the project plan and its attributes.

This gem only came about through the interest and support of Procore, who would love to [hear from you](https://www.procore.com/jobs/) 
if you're excited about working with Ruby and Rails.

## Installation

Add this line to your application's Gemfile:

	gem 'mpxj'

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install mpxj

## Changelog
You'll find details of what has changed in this version [here](http://mpxj.sourceforge.net/changes-report.html).

## Supported File Types

This gem uses the file name extension to determine what kind of project data it is reading. The list below shows the supported file extensions:

* **MPP** - Microsoft Project MPP file
* **MPT** - Microsoft Project template file
* **MPX** - Microsoft Project MPX file
* **XML** - Microsoft Project MSPDI (XML) file
* **MPD** - Microsoft Project database (only when the gem is used on Microsoft Windows)
* **PLANNER** - Gnome Planner
* **XER** - Primavera XER file
* **PMXML** - Primavera PMXML file
* **PP** - Asta Powerproject file
 
## Example Code
 
The following is a trivial example showing some basic task and resource details being queried from a project:


	project = MPXJ::Reader.read("project1.mpp")
	
	puts "There are #{project.all_tasks.size} tasks in this project"
	puts "There are #{project.all_resources.size} resources in this project"
	
	puts "The resources are:"
	project.all_resources.each do |resource|
	  puts resource.name
	end
	
	puts "The tasks are:"
	project.all_tasks.each do |task|
	  puts "#{task.name}: starts on #{task.start}, finishes on #{task.finish}, it's duration is #{task.duration}"
	end 

## Entities

The gem represents the project plan using the following classes, all of which reside in the MPXJ module.

* Project
* Resource
* Task
* Assignment
* Relation

A **Project** contains **Resource**s and **Task**s. Each **Resource** can be **Assigned** to one or more **Task**s. 
**Task**s can have dependencies between them which are represented as **Relation**s.

 
## Methods, Attributes and Data Types

There are very few explicit methods implemented by the classes noted above. Access to the attributes of each class is provided via a `method_missing` handler which checks to see if the requested method name matches a known attribute name. If it does match, the attribute value is returned, otherwise the normal method missing exception is raised.

The methods defined explicitly by these classes are:

	Project#all_resources
	Project#all_tasks
	Project#child_tasks
	Project#all_assignments
	Project#get_resource_by_unique_id(unique_id)
	Project#get_task_by_unique_id(unique_id)
	Project#get_resource_by_id(id)
	Project#get_task_by_id(id)
	
	Resource#parent_project
	Resource#assignments
	
	Task#parent_project
	Task#assignments
	Task#predecessors
	Task#successors
	Task#child_tasks
	Task#parent_task
	
	Assignment#parent_project
	Assignment#task
	Assignment#resource

Each attribute supported by these classes is represented by appropriate data types:

* String
* Duration [https://rubygems.org/gems/duration](https://rubygems.org/gems/duration)
* Time 
* Integer
* Float

The attributes supported by each class are listed here:

* [Properties Attributes](docs/PropertiesAttributes.md)
* [Task Attributes](docs/TaskAttributes.md)
* [Resource Attributes](docs/ResourceAttributes.md)
* [Assignment Attributes](docs/AssignmentAttributes.md)
* [Relation Attributes](docs/RelationAttributes.md)

## Acknowledgements 
This gem includes functionality provided by POI [http://poi.apache.org](http://poi.apache.org/)
 
This gem includes functionality provided by RTF Parser Kit [https://github.com/joniles/rtfparserkit](https://github.com/joniles/rtfparserkit/)

