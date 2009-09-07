task :default => [:clean, :css, :html]

desc 'Wipe existing site directory contents'
task :clean do
  `rm -rf _site/*`
  `cp -r _images _site`
end

desc 'Regenerates css files from compass sass'
task :css => :clean do
  `compass`
end

desc 'Regenerates html files from jekyll files'
task :html => :clean do
  `jekyll`
end

desc "Starts jekyll server on port 4000"
task :server => [:html,:css] do
  `jekyll --server`
end

