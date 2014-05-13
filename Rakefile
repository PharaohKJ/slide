require 'rake'
require 'sinatra'
require 'rdiscount'

#  Rake task
desc "local webserver backend start => http://localhost:3000/"
task :start => 'sinatra:start'

desc "local webserver stop"
task :stop => 'sinatra:stop'

desc "local webserver start => http://localhost:3000/"
task :run => 'sinatra:run'

namespace :sinatra do
#  RUNCOMMAND = "rbenv exec bundle exec ruby -rsinatra -e 'set :port, 3000; set :public_folder, \"./\"; set :environment, :producntion; get(\"/\"){send_file File.join(settings.public_folder, \"index.html\")}'"
  RUNCOMMAND = "rbenv exec bundle exec ruby -rsinatra -e 'set :port, 3000; set :public_folder, \"./\"; set :environment, :producntion; get(\"/\"){\"markdown :index, :layout_engine => :erb\"}'"
  STARTCOMMAND = RUNCOMMAND + " >/dev/null 2>&1 &"
  STOPCOMMAND = 'kill `ps auxww |grep "[r]uby -rsinatra -e set :port, 3000; set :public_folder, \"./\";" |awk \'{print $2}\'` >/dev/null 2>&1 &'

  task :run do
    sh RUNCOMMAND
  end
  task :start do
    sh STARTCOMMAND
  end
  task :stop do
    sh STOPCOMMAND
  end
end
