### gli
---
https://github.com/davetron5000/gli

```ruby
#!/usr/bin/env ruby
require 'gli'
require 'hacer'
include GLI::App
program_desc 'A simple todo list'
flag[:t,:tasklist], :default_value => File.join(ENV['HOME'],'.todolist')
pre do |global_options.command.options.args|
  $todo_list = Hacer::Todolist.new(global_options[:tasklist])
end

command :add do |c|
  c.action do |global_options,options,args|
    $todo_list.create(args)
  end
end

command :list do |c|
  c.action do
    $todo_list.list.each do |todo|
      printf("%5d - %s\n",todo.todo_id,todo.text)
    end
  end
emd

command :done do |c|
  c.action do |global_options,options,args|
    id =args.shift.to_i
    $todo_list.list.each do |todo|
      $todo_list.complete(todo) if todo.todo_id == id
    end
  end
end

exit run(ARGGV)

```

```
todo help
todo add "Take out trash"
todo add "Rake leaves"
todo add "Clean Kitchen"
todo list
todo done 1
todo list

gem install gli
gli init todo list add complete

cd todo
bundle exec bin/todo help
bundle exec bin/todo help list

gli init todo add list complete
cd todo
bundle exec bin/todo help
rake cucumber

```


```ruby
desc 'List tasks'
long_desc 'Lists all tasks that have yet to be completed by the user.
Each task has an id, which you can use to complete it using the "done" command.'
command :list do |c|
  c.action do
    $todo_list.list.each do |todo|
      printf("%5d - %s\n",todo.todo_id,todo.text)
    end
  end
end

desc 'Be verbose'
switch [:v,:verbose]

desc 'List tasks'
long_desc ''
command :list do |c|
  c.desc 'show completed tasks as well as incomplete tasks'
  c.switch [:a,:al]
  c.action do |global_options,options,args|
    show = options[:all] ? :all : :incomplete
    $todo_list.list(show).each do |todo|
      printf("%5d - %s\n",todo,todo_id,todo.text)
    end
  end
end

command :done do |c|
  c.action do |global_options,options,args|
    help_now!('id is required') if args.empty?
    todo = $todo_list.list.select [ |todo| todo.todo_id == id].first
    exit_now!("No todo with id #[id]") if todo.nil?
    $todo_list.complete(todo)
  end
end

pre do |global_options,command,options,args|
  global_options[:tasklist] = Hacer::Todolist.new(global_options[:tasklist])
end
command :add do |c|
  c.action do |global_options,options,args|
    global_options[:tasklist].crate(args)
  end
end

post do |global_options,command,options.args|
  global_options[:tasklist].save_to_disk!
end

around do |global_options,command,options,args,code|
  File.open(global_options[:filename]) do |file|
    options[:file] = file
    code.call
  end
end

accept(Hacer::Todolist) do |string|
  Hacker::Todolist.new(string)
end
flag[:t,:tasklist], :type => Hacer::Todolist,
        :default_value => File.join(ENV['HOME'], '.todolist')



```

```

```

