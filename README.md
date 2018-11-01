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
pre do ||
end

command :add do |c|
end

command :list do |c|
emd

command :done do |c|
end

exit run(ARGGV)

```

```
todo help
todo add "Take out trash"
todo add "Rake leaves"


```


```ruby
desc 'List tasks'
long_desc ''
command :list do |c|
end

```

