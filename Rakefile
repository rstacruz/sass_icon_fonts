task 'icons.json' do
  require 'json'

  files = Dir['./_*.sass']
  output = {}

  files.each do |file|
    contents = File.read(file)

    name = (file =~ /_(.*?)\.sass$/) && $1.strip

    pack = output[name] = {
      'prefix' => (contents =~ /= ([^\-]*)-font/ && $1),
      'name' => name,
      'icons' => []
    }

    list = pack['icons']
    contents.gsub(/^%[^\-]*-(.*?):before/) { list << $1 }
  end

  File.open('icons.json', 'w') { |f| f.write JSON.pretty_generate(output) + "\n" }
end

task 'demo/style.css' do
  system 'sass demo/style.sass > demo/style.css'
end

task :all => %w[icons.json demo/style.css]

task :default => :all


