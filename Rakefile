require 'rake'
require 'sinatra'

#  Rake task
desc "local webserver backend start => http://localhost:3000/"
task :start => 'sinatra:start'

desc "local webserver stop"
task :stop => 'sinatra:stop'

desc "local webserver start (without guard-livereload)  => http://localhost:3000/"
task :run => 'sinatra:run'

namespace :sinatra do
  STARTSINATRA = "ruby -rsinatra -e 'set :port, 3000; set :public_folder, \"./\"; set :environment, :producntion; get(\"/\"){send_file File.join(settings.public_folder, \"index.html\")}'"
  STOPSINATRA  = 'kill `ps auxww |grep "[r]uby -rsinatra -e set :port, 3000; set :public_folder, \"./\";" |awk \'{print $2}\'`'

  STARTGUARD   = 'guard -i'
  STOPGUARD    = 'kill `ps auxww |grep "[g]uard -i" |awk \'{print $2}\'`'

  task :run do
    sh STARTSINATRA
  end
  task :start do
    sh STARTSINATRA + " >/dev/null 2>&1 &"
    sh STARTGUARD   + " >/dev/null 2>&1 &"
    p ""
    p "http://localhost:3000/"
  end
  task :stop do
    sh STOPSINATRA + " >/dev/null 2>&1 &"
    sh STOPGUARD   + " >/dev/null 2>&1 &"
  end
end
