require "rubygems"
require "tmpdir"
require "bundler/setup"
require "jekyll"

namespace :site do
  desc "Generate blog files"
  task :generate do
    Jekyll::Site.new(Jekyll.configuration({
      "source"      => ".",
      "destination" => "_site"
    })).process
  end

  desc "Generate and publish blog to master"
  task :publish => [:generate] do
    Dir.mktmpdir do |tmp|
      system "mv _site/* #{tmp}"
      if system "git show-ref --verify --quiet refs/heads/master"
        system "git checkout master"
      else
        system "git checkout --orphan master"   # create new branch with no history
      end
      next if $?.exitstatus != 0      # abort if checkout failed
      system "rm -rf *"
      system "mv #{tmp}/* ."
      message = "Site updated at #{Time.now.utc}"
      system "git add --all ."
      system "git commit -am #{message.shellescape}"
      system "git push origin master --force"
      system "git checkout source"
      puts "Site published successfully."
    end
  end
end
