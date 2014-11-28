task :default do
	sizes = [
    ['small', '320x'],
    ['medium', '900x'],
    ['large', '1600x']
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
      
      numeric_logo_path = File.dirname(vector_logo_path) + "/#{i + 1}.svg"
      `cp #{f.path} #{numeric_logo_path}`

      logos_for_readme[name] += [vector_logo_path]
    end
  end

  logos_for_readme = logos_for_readme.map do |name, files|
    "\n\nColor Logo | Grayscale Logo | Sizes\n| ------ | ------ | -------\n" + 
    %{<img src="http://pigment.github.io/fake-logos/logos/vector/color/#{name}.svg" alt="#{name}" width="160" /> | } +
    %{<img src="http://pigment.github.io/fake-logos/logos/vector/grayscale/#{name}.svg" alt="#{name}" width="160" /> | } +
    %{<a href="http://pigment.github.io/fake-logos/logos/vector/color/#{name}.svg">Color SVG</a>, } +
    %{<a href="http://pigment.github.io/fake-logos/logos/vector/grayscale/#{name}.svg">grayscale SVG</a>}
  end.join("\n")

  readme = File.read('README.md')
  readme = readme.gsub(/The Logos.*##/m, "The Logos\n\n" + logos_for_readme + "\n\n##")
  File.open('README.md', 'w') { |file| file.write(readme) }
end


task :clean do
  `rm ./logos/*/**/*.svg`
end