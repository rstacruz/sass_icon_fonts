def icon_data
  require 'json'
  JSON.parse(File.read('ref/icons.json'))
end

def edit(file, &blk)
  contents = File.read(file)
  contents = yield(contents)
  puts "==> Updating #{file}"
  File.open(file, 'w') { |f| f.write contents }
end

def write(file, &blk)
  contents = yield
  puts "==> Writing #{file}"
  File.open(file, 'w') { |f| f.write contents }
end

# ----------------------------------------------------------------------------

task 'ref/icons.json' do
  write 'ref/icons.json' do
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

    JSON.pretty_generate(output) + "\n"
  end
end

# ----------------------------------------------------------------------------

task 'ref/style.scss' => ['ref/icons.json'] do
  edit 'ref/style.scss' do |contents|
    includes = []
    icon_data.each do |_, pack|
      pack['icons'].each do |icon|
        prefix = pack['prefix']
        size   = pack['nativeSize']
        includes << ".#{prefix}-#{icon} { @include #{prefix}-icon(#{icon}, #{size}px); }"
      end
    end
    contents.gsub!(%r[// START //(.*)// END //]m, "// START //\n#{includes.join("\n")}\n// END //")
  end
end

# ----------------------------------------------------------------------------

task 'ref/style.css' => ['ref/style.scss'] do
  puts "==> Compiling style.css"
  system 'sass -t compact ref/style.scss > ref/style.css'
end

# ----------------------------------------------------------------------------

task 'index.html' => ['ref/style.css'] do
  edit 'index.html' do |contents|
    icons = []
    icon_data.each do |name, pack|
      icons << "<div class='pack'>"
      icons << "<h3>#{name}</h3>"
      pack['icons'].each do |icon|
        prefix = pack['prefix']
        icons << "<i class='icon #{prefix}-#{icon}'><span>#{prefix}-icon(<b>#{icon}</b>)</span></i>"
      end
      icons << "</div>"
    end

    css_data = "<style type='text/css'>\n%s</style>" % [ File.read('ref/style.css') ]
    File.unlink 'ref/style.css'

    contents.gsub!(%r[<!-- START -->(.*)<!-- END -->]m, "<!-- START -->\n#{icons.join("\n")}\n<!-- END -->")
    contents.gsub!(%r[<!-- START CSS -->(.*)<!-- END CSS -->]m) { "<!-- START CSS -->\n#{css_data}\n<!-- END CSS -->" }
  end
end

# ----------------------------------------------------------------------------

task :all => %w[ref/icons.json ref/style.css index.html]

task :default => :all


