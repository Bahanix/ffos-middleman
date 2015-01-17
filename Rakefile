require 'highline/import'

desc "Build the app"
task :build do
  system "middleman build"
  system "cd build ; rm -f app.zip ; zip -r app.zip *"
end

desc "Deploy the app to gh-pages"
task :deploy do
  puts "Deploying everything up to this commit:"
  system "git log -1 --oneline"
  puts

  system "git fetch"
  unless `git log HEAD..origin/master --oneline`.empty?
    confirm = ask("You need to PULL. Deploy anyway? [y/n] ") { |yn| yn.limit = 1, yn.validate = /[yn]/i }
    exit unless confirm.downcase == 'y'
    puts
  end

  unless `git branch` =~ /\* master/
    confirm = ask("You are NOT on master. Deploy anyway? [y/n] ") { |yn| yn.limit = 1, yn.validate = /[yn]/i }
    exit unless confirm.downcase == 'y'
    puts
  end

  unless `LC_ALL=C git status` =~ /clean/
    confirm = ask("Uncommited files WON'T be deployed. Deploy anyway? [y/n] ") { |yn| yn.limit = 1, yn.validate = /[yn]/i }
    exit unless confirm.downcase == 'y'
    puts
  end

  system "git push origin :gh-pages -f"
  puts

  if system "git subtree push --prefix build origin gh-pages"
    puts
    puts "\e[32mEverything seems OK!\e[39m Take a look at:"
    data = `git remote -v | grep push`.split(':').last.split('.').first.split('/')
    puts "https://#{data.first.downcase}.github.io/#{data.last}/"
  else
    puts
    puts "\e[31mSomething goes wrong :(\e[39m"
  end
end
