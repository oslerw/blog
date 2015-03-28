require 'find'
require 'haml'

task default: %w[haml gitignore build]

# Generate haml files
task :haml do
  forall_haml do |path|
    outfile = File.new(path[0..-6], 'w')
    engine = Haml::Engine.new(File.read(path))
    outfile.write(engine.render)
    outfile.close
  end
end

# Update gitignore to ignore html files generated from haml files
task :gitignore do
  orig_gitignore = File.read('./.gitignore')
  new_gitignore = File.new('./.gitignore', 'a')

  forall_haml do |path|
    ignore_path = path[2..-6]
    unless orig_gitignore.include? ignore_path
      new_gitignore.puts ignore_path
    end
  end

  new_gitignore.close
end

# Build the site with jekyll
task :build do
  `bundle exec jekyll build`
end

def forall_haml
  Find.find('./') do |path|
    # Exclude vendor
    if path.start_with? './vendor'
      Find.prune
    end

    # Only run on files
    if File::file? path
      if /\.html.haml$/.match path
        yield path
      end
    end
  end
end
