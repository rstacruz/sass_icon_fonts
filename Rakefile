task 'icons.json' do
  require 'json'

  files = Dir['./_*.sass']
  output = {}

  files.each do |file|
    name = (file =~ /_(.*?)\.sass$/) && $1.strip
    list = output[name] = []
    File.read(file).gsub(/^%[^\-]*-(.*?):before/) { list << $1 }
  end

  puts JSON.pretty_generate(output)
end
