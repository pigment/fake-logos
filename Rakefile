task :default do
  sizes = [
    ['small', '320'],
    ['medium', '900'],
    ['large', '1600']
  ]

  types = ['color', 'grayscale']
  logos_for_readme = Hash.new([])

  types.each do |type|
    files = Dir["./src/logos/#{type}/*.svg"].map {|path| File.new(path) }
    files.sort! {|a, b| a.ctime <=> b.ctime }

    files.each_with_index do |f, i|
      name = File.basename(f.path, '.svg')
      vector_logo_path = f.path.gsub("src/logos/", "logos/vector/")
      `mkdir -p #{File.dirname(vector_logo_path)}`
      `cp #{f.path} #{vector_logo_path}`
      
      logos_for_readme[name] += [vector_logo_path]

      sizes.each do |size, width|
        new_dir = File.dirname(f.path.gsub('src/logos/', "logos/#{size}/"))
        new_path = File.join(new_dir, File.basename(f.path, '.svg'))
        numeric_logo_path = File.dirname(new_path) + "/#{i + 1}.png"

        logos_for_readme[name] += [new_path + '.png']

        `mkdir -p #{new_dir}`
        `svg2png -w #{width} #{f.path} #{new_path}.png`
        `cp #{new_path}.png #{numeric_logo_path}`
      end
    end
  end

  logos_for_readme = logos_for_readme.map do |name, files|
    "\n\nColor Logo | Grayscale Logo | Sizes\n| ------ | ------ | -------\n" + 
    %{<img src="http://pigment.github.io/fake-logos/logos/vector/color/#{name}.svg" alt="#{name}" width="160" /> | } +
    %{<img src="http://pigment.github.io/fake-logos/logos/vector/grayscale/#{name}.svg" alt="#{name}" width="160" /> | } +
    %{<a href="http://pigment.github.io/fake-logos/logos/vector/color/#{name}.svg">svg</a>, } +
    sizes.map {|size, geom|
      %{<a href="http://pigment.github.io/fake-logos/logos/#{size}/color/#{name}.png">#{size}</a>}
    }.join(", ")
  end.join("\n")

  readme = File.read('README.md')
  readme = readme.gsub(/The Logos.*##/m, "The Logos\n\n" + logos_for_readme + "\n\n##")
  File.open('README.md', 'w') { |file| file.write(readme) }
end


task :clean do
  `rm ./logos/*/**/*.png`
  `rm ./logos/*/**/*.svg`
end