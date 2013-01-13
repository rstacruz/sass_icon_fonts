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
      'nativeSize' => (contents =~ /Native size: ([\d]+)px/ && $1.to_i),
      'icons' => []
    }

    list = pack['icons']
    contents.gsub(/^%[^\-]*-(.*?):before/) { list << $1 }
  end

  File.open('icons.json', 'w') { |f| f.write JSON.pretty_generate(output) + "\n" }
end

task 'demo/style.scss' => ['icons.json'] do
  contents = File.read('demo/style.scss')
  icons = JSON.parse(File.read('icons.json'))

  includes = []
  icons.each do |_, pack|
    pack['icons'].each do |icon|
      includes << ".%s-%s { @include %s-icon(%s, %ipx); }" % [ pack['prefix'], icon, pack['prefix'], icon, pack['nativeSize'] ]
    end
  end
  contents.gsub!(%r[// START //(.*)// END //]m, "// START //\n#{includes.join("\n")}\n// END //")

  File.open('demo/style.scss', 'w') { |f| f.write contents }
end

task 'demo/style.css' => ['demo/style.scss'] do
  system 'sass demo/style.scss > demo/style.css'
end

task :all => %w[icons.json demo/style.css]

task :default => :all


