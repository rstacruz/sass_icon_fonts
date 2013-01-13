def icon_data
  require 'json'
  JSON.parse(File.read('support/icons.json'))
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

task 'support/icons.json' do
  write 'support/icons.json' do
    require 'json'

    files = Dir['./_*.sass']
    output = {}

    files.each do |file|
      contents = File.read(file)

      name = (file =~ /_(.*?)\.sass$/) && $1.strip

      pack = output[name] = {
        'prefix' => (contents =~ /= ([^\-]*)-font/ && $1),
        'name' => name,
        'nativeSize' => (contents =~ /-native-size: ([\d]+)px/ && $1.to_i),
        'icons' => []
      }

      list = pack['icons']
      contents.gsub(/^%[^\-]*-(.*?):before/) { list << $1 }
    end

    JSON.pretty_generate(output) + "\n"
  end
end

# ----------------------------------------------------------------------------

task 'support/style.scss' => ['support/icons.json'] do
  edit 'support/style.scss' do |contents|
    includes = []
    icon_data.each do |name, pack|
      size   = pack['nativeSize']
      prefix = pack['prefix']

      includes << "##{name} .icon:before { font-size: #{size}px; }"
      pack['icons'].each do |icon|
        includes << ".#{prefix}-#{icon} { @include #{prefix}-icon(#{icon}); }"
      end
    end
    contents.gsub!(%r[// START //(.*)// END //]m, "// START //\n#{includes.join("\n")}\n// END //")
  end
end

# ----------------------------------------------------------------------------

task 'support/style.css' => ['support/style.scss'] do
  puts "==> Compiling style.css"
  system 'sass -t compact support/style.scss > support/style.css'
end

# ----------------------------------------------------------------------------

task 'index.html' => ['support/style.css'] do
  edit 'index.html' do |contents|
    icons = []
    icon_data.each do |name, pack|
      icons << "<div class='pack' id='#{name}'>"
      icons << "<h3>#{name}</h3>"
      pack['icons'].each do |icon|
        prefix = pack['prefix']
        icons << "<i class='icon #{prefix}-#{icon}'><span>#{prefix}-icon(<b>#{icon}</b>)</span></i>"
      end
      icons << "</div>"
    end

    css_data = "<style type='text/css'>\n%s</style>" % [ File.read('support/style.css') ]
    css_data.gsub! /\n{2,}/, "\n"
    File.unlink 'support/style.css'

    contents.gsub!(%r[<!-- START -->(.*)<!-- END -->]m, "<!-- START -->\n#{icons.join("\n")}\n<!-- END -->")
    contents.gsub!(%r[<!-- START CSS -->(.*)<!-- END CSS -->]m) { "<!-- START CSS -->\n#{css_data}\n<!-- END CSS -->" }
  end
end

# ----------------------------------------------------------------------------

task :all => %w[support/icons.json support/style.css index.html]

task :default => :all


